# Supported Browsers

IAT supports running tests on multiple browsers. The browser is configured using the `testDestination` system property. The following values are valid for this system property:

* CHROME
* SAFARI
* OPERA
* IE
* FIREFOX
* PHANTOMJS

## Configuring Browsers

Some browsers require additional software or extensions to be downloaded before that can be used with IAT. These downloads are listed in http://www.seleniumhq.org/download/.

### Configuring Opera

The `webdriver.opera.driver` system property needs to be set to the full path of the `operadriver` or `operadriver.exe` executable.

### Configuring Chrome

The `webdriver.chrome.driver` system property needs to be set to the fill path pf the `chromedriver` or `chromedriver.exe` executable.

### Configuring PhantomJS

The `phantomjs.binary.path` system property needs to be set to the full path of the executable. This can be downloaded from http://phantomjs.org/download.html.
