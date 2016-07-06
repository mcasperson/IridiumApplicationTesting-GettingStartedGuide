# Introduction

The Iridium Application Testing tool has been designed to provide a way for automated scripts to interact with and validate the operation of our web based applications.

The aim of this project is to:
* Provide the ability to write comprehensive test suites for our web applications
* Provide a way for business users to describe the behaviour of new features to be added into our web applications

Iridium has been built on top of [Cucumber](https://cucumber.io/) and [Selenium WebDriver](http://www.seleniumhq.org/projects/webdriver/) to allow plain English scripts to drive the interaction with a web browser. It can optionally run these scripts again [PhantomJS](http://phantomjs.org/) to allow tests to be run in a headless environment such as Bamboo. Tests can optionally be run in [BrowserStack](http://www.browserstack.com/) to test multiple browser versions.

The source code for Iridium can be found on [GitHub](https://github.com/AutoGeneral/IridiumApplicationTesting).
