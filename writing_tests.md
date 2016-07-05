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
 
 Note the use of comments in the test script. Comments start with the # character, and are ignored by IAT.
 
```
Scenario: Launch App
   # This defines the amount of time to wait in seconds between the execution of the
steps.
   When I set the default wait time between steps to "2"
   # This opens the default URL defined for the application. The process of defining a
default URL is discussed later.
   And I open the application
   # This waits for an element on the page to be displayed. We assume that once this
element is displayed that the page has been loaded.
   And I wait "30" seconds for the element with the ID of "SARQIC_button" to be
displayed
```

 Let's break down this script.

 **When I set the default wait time between steps to "2"** sets the amount of time to wait between steps to 2 seconds. This delay means the script will not run through each step at inhuman speeds, and ensures that updates made to the page in response to an interaction have some time to complete.

 **And I open the application** opens the default URL. This default URL can be supplied with the appURLOverride system property, or it can come from a configuration file.
 
 **And I wait "30" seconds for the element with the ID of "SARQIC_button" to be displayed** pauses the execution of the test script until a HTML element with the ID attribute of SARQIC_button has been displayed on the page. We use the fact that this element has been displayed as indication that the page has loaded.
 
## Using Configurations

 Configuration files provide the ability to run the same script against different environments. Typically a test script will need to be run against multiple environments (Integration, QA, PreProduction etc) and against multiple brands. The actual steps run by the script will be largely identical in all environments and all brands, but the URLs used to access the applications change.

This is an example configuration file.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<profile>
    <urlMappings>
  <featureGroup name="HQQ-QA">
   <urlMapping>
    <url
name="default">https://qa.hostname.com/hqq/new_quote.jsp?hSty=BUDD</url
>
   </urlMapping>
  </featureGroup>
  <featureGroup name="HQQ-PreProd">
   <urlMapping>
    <url
name="default">https://preproduction.hostname.com/hqq/new_quote.jsp?hSty=BUDD<
/url>
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
        # HQQ needs a fairly large default wait time, otherwise it is possible that
clicks will be
        # made on buttons that are no available.
  When I set the default wait time between steps to "3"
  And I open the application
  And I wait "120" seconds for the element with the ID of "Continue" to be displayed
  And I click the element with the ID of "Continue"
```

When IAT is run with the `featureGroupName` system property set to HQQ-QA, the step `And I open the application` will open the URL https://qa.hostname.com/hqq/new_quote.jsp?hSty=BUDD, as this is the default URL assigned to the HQQ-QA feature group in the configuration file.

Alternatively, IAT could be run with the `appURLOverride` system property set to https://qa.hostname.com/hqq/new_quote.jsp?hSty=BUDD, which override the default URL defined in the configuration file. In fact, the `featureGroupName` system property is optional in this situation where a default URL is supplied as a system property.

## Splitting Up Tests
 Tests can be split up so common Scenarios can be shared between multiple features. 
 
 Any feature file can include a comment like this:
 
```
#IMPORT: zap/enablezap.fragment
```

 This comment will be replaced with the contents of the file at the specified path.
Where a feature file has been referenced with a URL (e.g.` -DtestSource=https://whatever.com/test.feature`), the location of the imported files is found using the `importBaseUrl` system property, which defines the location from which relative imports are located.

 For example, if we ran WAT with `-DimportBaseUrl=http://whatever/fragments/`, the file imported from the example above would be `http://whatever/fragments/zap/enablezap.fragment`.

 If the feature file is file from the local file system, relative imports are located from the location of the feature file.
 
## Using Tags

 You can make use of Cucumber tags, which are documented at https://github.com/cucumber/cucumber/wiki/Tags.
 
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
            <url
name="default">https://qa.hostname.com.au/car/new_quote
.jsp?hSty=BUDD</url>
        </urlMapping>
        <urlMapping tags="~@OPM">
            <url
name="default">https://qa.hostname/car/new_quote
.jsp?hSty=1FOW</url>
        </urlMapping>
        <urlMapping tags="~@OPM">
            <url
name="default">https://qa.hostname/car/new_quote
.jsp?hSty=BBUY</url>
        </urlMapping>
     </urlMappings>
   </featureGroup>
</profile>
```