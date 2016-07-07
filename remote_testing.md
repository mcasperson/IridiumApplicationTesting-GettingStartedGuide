# Remote Testing

Iridium has support for running tests in [BrowserStack Automate](https://www.browserstack.com/automate), which provides the abaility to test multiple browsers on multiple operating systems.

Setting the `testDestination` system property to `BROWSERSTACK` instructs Iridium to run tests against BrowserStack.

You will need to supply a configuration file with the following settings:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<profile>
    <browserstack>
        <accessToken>accessTokenGoesHere</accessToken>
        <username>usernameGoesHere</username>
    </browserstack>
    <settings>
        <desiredCapabilities enabled="true" group="Chrome,Windows,Desktop">
            <capability name="browser" value="Chrome"/>
            <capability name="browser_version" value="50.0"/>
            <capability name="os" value="Windows"/>
            <capability name="os_version" value="8.1"/>
            <capability name="browserstack.debug" value="true"/>
        </desiredCapabilities>
    </settings>
</profile>
```

The `<accessToken>` element holds the BrowserStack access Token.

The `<username>` element holds the BrowserStack username.

The `<desiredCapabilities>` element defines the [capabilities](https://www.browserstack.com/automate/capabilities) of the browsers that should be opened by BrowserStack.

See [this example](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/16.browserstack), which demonstrate opening a simple test.