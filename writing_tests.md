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
name="default">https://qa-hostname.com/hqq/new_quote.jsp?hSty=BUDD</url
>
   </urlMapping>
  </featureGroup>
  <featureGroup name="HQQ-Preproduction">
   <urlMapping>
    <url
name="default">https://preproduction.hostname.com/hqq/new_quote.jsp?hSty=BUDD<
/url>
   </urlMapping>
  </featureGroup>
    </urlMappings>
</profile>
```