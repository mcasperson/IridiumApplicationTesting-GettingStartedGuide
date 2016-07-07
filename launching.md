# Launching
Iridium can be run multiple different ways. Which method you choose depends on your use case.

## Java Web Start (JNLP)
 Web Start provides an easy way to launch Iridium without requiring any special software beyond a standard Java 8 installation.
 
 Iridium comes with example test scripts which are launched via Web Start. To run these examples, right click on the links below and save the jnlp files to the local disk. Then run the files with the `javaws` application.

|Important|  |
|-|-|
|![](Exclamation_mark-red.png) | You must trust the S3 URL that hosts the example JAR files for these Web Start examples to work properly. See the [Installation chapter](installation.md) for more details. |

|Important|  |
|-|-|
|![](Exclamation_mark-red.png) |You must have Firefox installed, as the examples are configured to use it. |

|Note|  |
|-|-|
|![](light-bulbs-Light-Bulb.jpg) | The Web Start download bar will appear to freeze, restart and then jump to 100%. This is expected.|
 
 
 The following demos run against dummy or public websites that anyone can use.
 
 1. [Security Testing](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/14.securitytest/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/14.securitytest))
 

The following demos run against Auto & General's quote and sale websites. If you run these more than once or twice you'll be blocked by a firewall, so they are more useful as code examples than as actual working demos.

 
 1. [Open Application](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/1.openapp/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/1.openapp))
 2. [Using Aliases](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/2.aliases/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/2.aliases))
 3. [Address Capture](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/3.addresscapture/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/3.addresscapture))
 4. [URL Mappings](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/4.urlmappings/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/4.urlmappings))
 5. [Data Set Collections](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/5.dataset/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/5.dataset))
 6. [Parallel Tests](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/6.paralleltest/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/6.paralleltest))
 7. [Tags](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/7.tags/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/7.tags))
 8. [Configuration Tags](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/8.configurationtags/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/8.configurationtags))
 9. Multiple Environments: [First](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/9.multipleenvironments/test-ecom.jnlp) and [Second](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/9.multipleenvironments/test-secure.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/9.multipleenvironments))
 10. [Multiple Brands](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/10.multiplebrands/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/10.multiplebrands))
 11. [Multiple Apps](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/11.multipleapps/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/11.multipleapps))
 12. [Loops](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/12.loops/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/12.loops))
 13. [Modifying Requests with BrowserMob](https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/13.blockrequests/test.jnlp) ([code](https://github.com/AutoGeneral/IridiumApplicationTesting/tree/master/examples/13.blockrequests))
 
## Gradle
 Tests can be run using the Gradle build script. To launch a test, run the following command:
 
 ```
 ./gradlew clean run -DappURLOverride=https://bodgeit.herokuapp.com -DtestSource=https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/14.securitytest/test.feature -DtestDestination=FIREFOX
 ```

## JAR File
 Tests can be run from a local copy of the Iridium UberJar. To launch a test, run the followig command:
 
 ```
 java -DappURLOverride=https://bodgeit.herokuapp.com -DtestSource=https://raw.githubusercontent.com/AutoGeneral/IridiumApplicationTesting/master/examples/14.securitytest/test.feature -DtestDestination=FIREFOX jar WebappTesting.jar
 ```