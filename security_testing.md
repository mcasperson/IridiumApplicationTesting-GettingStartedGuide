# Security Testing

## Enabling ZAP

To enable the Zed Attack Proxy, you need to set the system property `startInternalProxy` to zap e.g. `-DstartInternalProxy=zap`.

## Testing With ZAP

Once enabled, you can initialise ZAP with the step

```
@ZAP
Scenario: Initialise ZAP
  Given a scanner with all policies enabled
```

This step needs to be executed before any other steps that interact with the application, as it is this interaction that ZAP uses to passively scan for vulnerabilities and to build up a list of URLs for active attacks.

Once all the steps that that interact with the web application has been completed, you can run an active ZAP scan with the step

```
@ZAP
Scenario: Save the results
  And the application is spidered
  And the attack strength is set to "HIGH"
  And the active scanner is run
  And the ZAP XML report is written to the file "zapreport.xml"
  Then no "Medium" or higher risk vulnerabilities should be present for the base url
"^https://([0-9a-zA-Z-]+\.)?(secure\.budgetdirect|ecommerce\.disconline|\.disconline)\
.com\.au"
```

These steps will instruct ZAP to:
* use its integrated spider to find additional URLs that can be attacked
* set the active attack string to HIGH, which means each URL is subject to more attacks
* run the active scanner, during which ZAP will probe the URLs it knows about (via those passed through it during the passive scan and through the spidering process) for various known vulnerabilities
* save the report file
* fail the Cucumber test if any medium level or higher vulnerabilities were found

## Browser Support

 Not all browser drivers support the use of a proxy. Firefox and PhantomJS work, but others like Chrome and Opera don't. ZAP will not work as expected with browsers that do not support a proxy.
