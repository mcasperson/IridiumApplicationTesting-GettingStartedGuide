# Writing Tests
## A Simple Example
 
 Let's look at a simple example of a test script.

 Test scripts are broken down into features, scenarios and steps.
A feature is composed of one or more scenarios, and represents a complete test of an entire application, or a significant feature in a web application. For example, a feature could be an end to end test of a quite and sale application which tests the entire process of obtaining a quote and buying a policy.

 A scenario holds a one or more steps that are logically grouped to represent some related actions within a web application. For example, a scenario might defines the steps required to fill out the first page of a quote, or login to an application.
 
 Steps are actions that represent single interactions with a web page. They could be opening a URL, clicking a button or filling in a text box. The first line in a feature is the name of the feature itself:

```
Feature: Test SAR
```

 Underneath this we have scenarios that represent some related actions. In this example we will define some default options that will be used throughout the feature, open the StandAlone Roadside application, and wait for it to load.
 
 Note the use of comments in the test script. Comments start with the # character, and are ignored by Iridium.
 
```
 Scenario: Launch App
   # This defines the amount of time to wait in seconds 
   # between the execution of the steps.
   When I set the default wait time between steps to "2"
   # This opens the default URL defined for the application. 
   # The process of defining a default URL is discussed later.
   And I open the application
   # This waits for an element on the page to be displayed. 
   # We assume that once this element is displayed that the page has been loaded.
   And I wait "30" seconds for the element with the ID of "SARQIC_button" to be displayed
```

 Let's break down this script.

 **When I set the default wait time between steps to "2"** sets the amount of time to wait between steps to 2 seconds. This delay means the script will not run through each step at inhuman speeds, and ensures that updates made to the page in response to an interaction have some time to complete.

 **And I open the application** opens the default URL. This default URL can be supplied with the appURLOverride system property, or it can come from a configuration file.
 
 **And I wait "30" seconds for the element with the ID of "SARQIC_button" to be displayed** pauses the execution of the test script until a HTML element with the ID attribute of SARQIC_button has been displayed on the page. We use the fact that this element has been displayed as indication that the page has loaded.
 
 The complete script looks like this:
 
```
Feature: Test SAR
 Scenario: Launch App
   # This defines the amount of time to wait in seconds 
   # between the execution of the steps.
   When I set the default wait time between steps to "2"
   # This opens the default URL defined for the application. 
   # The process of defining a default URL is discussed later.
   And I open the application
   # This waits for an element on the page to be displayed. 
   # We assume that once this element is displayed that the page has been loaded.
   And I wait "30" seconds for the element with the ID of "SARQIC_button" to be displayed
```
 
 See [this example](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/1.openapp), which demonstrate opening a simple test.
 
## Using Configurations

 Configuration files provide the ability to run the same script against different environments. Typically a test script will need to be run against multiple environments (Integration, QA, PreProduction etc) and against multiple brands. The actual steps run by the script will be largely identical in all environments and all brands, but the URLs used to access the applications change.

This is an example configuration file.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<profile>
    <urlMappings>
  <featureGroup name="HQQ-QA">
   <urlMapping>
    <url name="default">
      https://qa.hostname.com/hqq/new_quote.jsp?hSty=BUDD
    </url>
   </urlMapping>
  </featureGroup>
  <featureGroup name="HQQ-PreProd">
   <urlMapping>
    <url name="default">
      https://preproduction.hostname.com/hqq/new_quote.jsp?hSty=BUDD
    </url>
   </urlMapping>
  </featureGroup>
    </urlMappings>
</profile>
```

Here we define two feature groups: `HQQ-QA` and `HQQ-PreProd`. Inside each of these feature groups we have defined the default url for QA and PreProduction.

In order to make use of the URLs defined in the configuration file, the directive `# FeatureGroup: HQQ-QA,HQQ-PreProd` needs to be added to the top of the script. This comma separated list defines the feature groups that can be used with the script.

```
# FeatureGroup: HQQ-QA,HQQ-PreProd
Feature: Test Mobile Home Quote (HQQ)
 Scenario: Complete First Page
  # HQQ needs a fairly large default wait time, 
  # otherwise it is possible that clicks will be
  # made on buttons that are no available.
  When I set the default wait time between steps to "3"
  And I open the application
  And I wait "120" seconds for the element with the ID of "Continue" to be displayed
  And I click the element with the ID of "Continue"
```

When IAT is run with the `featureGroupName` system property set to HQQ-QA, the step `And I open the application` will open the URL https://qa.hostname.com/hqq/new_quote.jsp?hSty=BUDD, as this is the default URL assigned to the HQQ-QA feature group in the configuration file.

Alternatively, IAT could be run with the `appURLOverride` system property set to https://qa.hostname.com/hqq/new_quote.jsp?hSty=BUDD, which override the default URL defined in the configuration file. In fact, the `featureGroupName` system property is optional in this situation where a default URL is supplied as a system property.

See [this example](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/4.urlmappings), which demonstrates the use of a configuration file.

## Splitting Up Tests
 Tests can be split up so common Scenarios can be shared between multiple features. 
 
 Any feature file can include a comment like this:
 
```
#IMPORT: zap/enablezap.fragment
```

 This comment will be replaced with the contents of the file at the specified path.
Where a feature file has been referenced with a URL (e.g.` -DtestSource=https://whatever.com/test.feature`), the location of the imported files is found using the `importBaseUrl` system property, which defines the location from which relative imports are located.

 For example, if we ran Iridium with `-DimportBaseUrl=http://whatever/fragments/`, the file imported from the example above would be `http://whatever/fragments/zap/enablezap.fragment`.

 If the feature file is file from the local file system, relative imports are located from the location of the feature file.
 
## Using Tags

 You can make use of Cucumber tags, which are documented at https://github.com/cucumber/cucumber/wiki/Tags.
 
 See [this example](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/7.tags), which demonstrates the use of tags.
 
## Override Tags

 Tags are supplied with the tagsOverride system property e.g. `-DtagsOverride=~@OPM,@SOMETAG;~@CTM`
 
 Tags are first split on a semicolon into groups that are "and'ed" together. So the string ~@OPM,@SOMETAG;~@CTM is broken down into the logic ~@OPM,@SOMETAG AND ~@CTM.
 
 Tags are then split on a comma into groups that are "or'ed" together. So the strings ~@OPM,@SOMETAG and ~@CTM are broken down into the logic (~@OPM OR @SOMETAG) AND ~@CTM.
 
 This means that `~@OPM,@SOMETAG;~@CTM` will run all scenarios that (don't have the @OPM tag OR do have the @SOMETAG tag) AND don't have the @CTM tag.
 
## Tags in configuration

Tags can also be assigned to the URL mappings defined in the configuration file. These tags are used to include or exclude certain aspects of a common test script depending on the feature group that was used to run the application.

In this example, we have three default URLs assigned to the feature group CQS-QA. Because only the budget branded car quote and salve application will autoregister with OPM, the two partner brands both exclude any step tagged with @OPM.

This means the same script can be run across multiple brands, with tags allowing us to selectively include or exclude those scenarios as appropriate.

```xml
<profile>
   <urlMappings>
      <featureGroup name="CQS-NXQ">
        <urlMapping>
            <url name="default">
              https://qa.hostname.com.au/car/new_quote.jsp?hSty=BUDD
            </url>
        </urlMapping>
        <urlMapping tags="~@OPM">
            <url name="default">
              https://qa.hostname/car/new_quote.jsp?hSty=1FOW
            </url>
        </urlMapping>
        <urlMapping tags="~@OPM">
            <url name="default">
              https://qa.hostname/car/new_quote.jsp?hSty=BBUY
            </url>
        </urlMapping>
     </urlMappings>
   </featureGroup>
</profile>
```

See [this example](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/8.configurationtags), which demonstrates the use of tags in a configuration file.

## Using Aliases

 Often the xpath, id, css selector or other value used to identify an element on the page has an obscure name which makes a step harder to understand.

 Aliases provide a solution for this by mapping a readable name to an unreadable value. This is demonstrated in the example below.
 
```
# Description: Tests of the SAR application
# FeatureGroup: SAR-INT, SAR-QA
Feature: Test SAR
   @PAGEOBJECT
   Scenario: Generate Page Object
    # This is a kind of page object, where we map ids, xpaths and 
    # other attributes to more readable names.
    Given the alias mappings
     | ContinueButtonPage1 | SARQIC_button |
@LAUNCH
Scenario: Launch App
When I set the default wait time between steps to "2"
And I open the application
And I wait "30" seconds for the element with the ID alias of "ContinueButtonPage1" to be displayed
```

The step `Given the alias mappings` accepts a data table which is a key/value mapping between the alias name and what is typically a string used to identify an element on the page.

The step `And I wait "30" seconds for the element with the ID alias of "ContinueButtonPage1" to be displayed` then references the alias to identify the ID of the element.

See [this example](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/2.aliases), which demonstrates the use of aliases in a feture script.

## Using DataSet Collections

Data set collections are XML files that contain multiple sets of alias mappings. These are used to run a test script multiple times with different inputs.

Data set collections include one common data set, which defines alias mappings shared between all other data sets. As the name suggests, a common data set provides the ability to define common data once and share it with each data set.

Data set collections then include individual data sets. Each data set is combined with the common data set to provide a set of aliased values that are used with a single test run.

```xml
<profile>
   <dataSets>
      <commonDataSet>
  <setting name="CarRegoValue">ASD123</setting>
      </commonDataSet>
      <dataSet>
         <!-- Car Details -->
         <setting name="CarDetailsYearIndex">2</setting>
         <setting name="CarDetailsMakeIndex">1</setting>
         <setting name="CarDetailsTransmissionIndex">2</setting>
         <setting name="CarDetailsFuelIndex">1</setting>
         <setting name="CarDetailsModelIndex">2</setting>
         <setting name="CarDetailsRedbookCodeIndex">2</setting>
         <setting name="CarDetailsUseIndex">1</setting>
         <setting name="CarDetailsOdometerIndex">1</setting>
         <setting name="CarRegoValue">ASD123</setting>
      </dataSet>
  <dataSet>
         <!-- Car Details -->
         <setting name="CarDetailsYearIndex">3</setting>
         <setting name="CarDetailsMakeIndex">2</setting>
         <setting name="CarDetailsTransmissionIndex">3</setting>
         <setting name="CarDetailsFuelIndex">2</setting>
         <setting name="CarDetailsModelIndex">3</setting>
         <setting name="CarDetailsRedbookCodeIndex">3</setting>
         <setting name="CarDetailsUseIndex">2</setting>
         <setting name="CarDetailsOdometerIndex">2</setting>
      </dataSet>
   </dataSets>
</profile>
```

Data set collections are referenced with the `dataset` system property e.g.` -Ddataset=http://whatever/data/cqs-dataset.xml`

See [this example](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/5.dataset), which demonstrate the use of data set collections.

## Running Multiple Tests

As you have seen, the configuration file can hold multiple URL mappings for a feature group, and a data set collection can contain multiple data sets.

By default, each URL in a URL mapping and each data set results in a new test being run. For example, if you had 2 URL mappings and a data set collection with 3 data sets, the test would be run 6 times (2 urls * 3 data sets).

The `numberURLs` system property can be used to limit the number of URLs that are selected when running a test e.g. `-DnumberURLs=1`. The `numberDataSets` property can be used to limit the number of data sets that tests are run against e.g. `-DnumberDataSets=1`. Typically you would set the `numberURLs` and `numberDataSets` system properties when you want to quickly run a single test.

Tests can be run in parallel. The number of parallel tests being run is determined by the `numberOfThreads` system property.

See [this example](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/6.paralleltest), which demonstrate running multiple tests in parallel.

## Modifying Requests

 Iridium has included [BrowserMob](https://github.com/lightbody/browsermob-proxy), which provides the ability to modify HTTP requests made by browsers whose drivers support the use of a proxy (not all browser drivers support the use of a proxy, see the [Security Testing](security_testing.md) for more details).
 
 BrowserMob supports the steps like `And I block access to the URL regex "http://google.com" with response "500"`, which will intercept any requests to http://google.com and return a HTTP 500 response code.