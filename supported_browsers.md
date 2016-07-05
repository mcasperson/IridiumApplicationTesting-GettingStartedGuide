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

The Opera driver be downloaded from https://github.com/operasoftware/operachromiumdriver/releases.

### Configuring Chrome

The `webdriver.chrome.driver` system property needs to be set to the fill path pf the `chromedriver` or `chromedriver.exe` executable.

The Chrome driver be downloaded from http://chromedriver.storage.googleapis.com/index.html?path=2.22/.

### Configuring PhantomJS

The `phantomjs.binary.path` system property needs to be set to the full path of the `phantomjs` or `phantomjs.exe` executable. 

PhantomJS can be downloaded from http://phantomjs.org/download.html.

### Configuring Safari

Safari has an extension called `SafariDriver.safariextz` that needs to be installed before IAT can interact with the browser. 

This can be downloaded from http://selenium-release.storage.googleapis.com/2.48/SafariDriver.safariextz.

### Configuring Firefox

Firefox works without any additional configuration.

