# System Property Reference

System properties marked as * are required

## configuration
Specify the location of the configuration XML file.

This option accepts a complete file path, or a URL.

## dataset
Specify the location of the data set collection XML file.

This option accepts a complete file path, or a URL.

## testSource *
Path to E2E tests.

If this is a directory, it will be scanned for .feature files that match the `featureGroupName` setting.
If it is a file or a URL, the script is run disregrading the `featureGroupName` setting.

## testDestination
Name of the browser to run the tests (Chrome, Firefox, IE). Select BrowserStack to run the tests remotely in BrowserStack.

## featureGroupName
This is optional, and is only used when `testSource` references a directory. 
It links to the featureGroupName attribute on a feature element.
The featureGroup attribute is a mapping between a feature script and the urls and data sets that will be applied to it.

## webdriver.chrome.driver, webdriver.ie.driver or webdriver.opera.driver
Path to web drivers for those browsers

## groupName
This defines the group that a BrowserStack capabilities profile needs to belong to in order to be included in the test. 
Groups usually indicate the browser (IE, Chrome etc), OS (Windows, MacOS etc) or device kind (Desktop, Mobile etc).
This is useful when you want to limit your tests to a logical group of device or platform.

## leaveWindowsOpen
Tick this option to leave the browser window open after the local tests have been completed.
This is useful when you want to continue using the web application from where the script finished up.

This has no effect when tests are run in BrowserStack.

If you select the "Leave Browser Open" option, you may find that some resources are not released when the test is finished and the application is closed. In particular, the chromedriver and operadriver processes are left in memory (depending on the browser you are using). If you run a number of tests in succession, these processes can take up quite a bit of memory, and it is up to you to kill them manually.

## appURLOverride
If Override URL is selected, this field can be used to define the single custom URL that the application will open with.
This is useful if want want to test a feature branch with a test script, or if you want to test custom query params.
Because you are specifying a single URL, the test will be run once.

Note that you can only override the URL when the test script does not reference a named url. Fo rexample, this 
line in a test script will fail if `appURLOverride` is defined, because these named url references from the
configuration file are no longer loaded.
```
And I open the application "app"
```

## numberDataSets
This defines the maximum number of entries that will be selected from the data set.
Usually when testing locally you'll set this to one to ensure that you are only opening one instance of an application.

## numberOfThreads
In order to improve the performance of the tests, they can be run in parallel. This setting defines the maximum number of threads that Cucumber will be run in. Keep in mind that when you run tests in BrowserStack, there is a limit (probably 2) of the number of parallel tests. You will get errors if you set this value higher than the maximum number of parallel tests when running in BrowserStack.

*NOTE:* Running cucumber in seperate threads does not mean that local browsers are running in their own "sandbox".

It is quite likely that running two or more threads for the same web application in the same browser will result in those browser instances conflicting with each other as they edit the same server session.

When running locally against a single instance of an application (i.e. running multiple data sets against a single application), it is usually best to use a single thread.

## tagsOverride
See https://mcasperson.gitbooks.io/iridiumapplicationtesting-gettingstartedguide/content/
to get more information about this option.

## enableVideoCapture
When running a local test, enabling this option will save a screen capture to an AVI file.
The location of the AVI file is displayed in the output window once the test is finished.

## enableScenarioScreenshots
When set to true, screenshots will be taken at the end of each scenario. This is useful when
running tests against a headless browser like PhantonJS, as it gives you an opportunity to see
the state of the web page at points during the test.

## openReportFile
When set to true, the HTML report will be opened once the test is finished.

## saveReportsInHomeDir
When set to true, all report files and screenshots will be saved in a folder in the users home
directory. Otherwise the files are saved in the current working directory.

You almost always want to set this to true when running tests locally, and set it to false when
running tests in Bamboo.

## phantomJSLoggingLevel
Sets the PhantomJS logging level. PhantomJS will generate a lot of unnecessary log messages while 
waiting for elements to be displayed, so by default the logging is turned off. But it can be handy
to see these messages sometimes, so setting this value to something like `WARN` will give
you some additional logging.

## startInternalProxy
Set this to the name of the internal proxy that should be started with all tests e.g.
```
-DstartInternalProxy=zap
```
