# THIS IS NOT THE LATEST VERSION. 
See the PowerTranzSDK repo for the latest.

# Release Notes

## Version 2.0.0
Please see the PowerTranz SDK Reference Documentation for more details. Note that as there are breaking changes, this version is not backwards compatible with 1.0.6.
### Breaking Changes
#### Model changes 


The following object property names have been changed or added:

Context | Old Name | New Name
---|---|---
_PtzPaymentRequest_ | ExternalBatchId | ExernalBatchIdentifier
_PtzPaymentRequest_ | OrderId | OrderIdentifier
_PtzPaymentRequest_ | * new *  | TerminalSerialNumber
_PtzPaymentRequest_ | moved from Source.CardholderAddress | BillingAddress
_PtzPaymentResponse_ | * new *  | TransactionType
_PtzPaymentResponse_ | * new *  | CardBrand
_PtzTransactionResponse_ | * extensive changes * | see reference docs
_PtzOrderResponse_ | * new * | see reference docs
public enum | PTtzPosEntryMode (typo) | PtzPosEntryMode



#### Miscellaneous
Terminal *HardReset* has been removed.
Terminal *SoftReset* has been changed to **ResetDevice** and is accessed directly from the terminal instance.
The *PTZMiuraTerminal.Driver* property has been removed.  Any required functionality is available directly from *PTZMiuraTerminal*.

There is a new _PtzPosEntryMode_ for receipts.  This will be returned as mode **3** and in code it is _PtzPosEntryMode.PtzPosEntryModeFallback_.  It will be returned in the receipt data when there was a card fallback.

TransactionSearch response model has been extensively modified and a new OrderSearch method has been added which returns an order (*PtzOrderResponse*) and all its related transactions.  See reference documentation for details.

### Configuration File
There is a new config file which can be used to set the PowerTranz Gateway URL and configure log4net logging parameters.  The file must be in the same folder as PowerTranzSDK.dll and must be called *PowerTranzSDK.dll.config.  Note that app.config can no longer be used.  This means that the configuration file for the SDK is completely separated from the POS application.

The following is a sample PowerTranzSDK.dll.config:
```html
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <section name="log4net" type="log4net.Config.Log4NetConfigurationSectionHandler, log4net"/>
  </configSections>

  <appSettings>
    <add key="PowerTranzUrl" value="https://staging.ptranz.com/api"/>
  </appSettings>

  <log4net>
    <logger name="PowerTranzSDKLogger">
      <level value="DEBUG"/>
      <appender-ref ref="RollingFileAppender"/>
      <appender-ref ref="Console"/>
    </logger>
    
    <appender name="RollingFileAppender" type="log4net.Appender.RollingFileAppender">
      <file value="ptzsdk.win.samples.console.log"/>
      <appendToFile value="true"/>
      <rollingStyle value="Size"/>
      <maxSizeRollBackups value="5"/>
      <maximumFileSize value="5MB"/>
      <staticLogFileName value="true"/>
      <layout type="log4net.Layout.PatternLayout">
        <conversionPattern value="%date %level  - %message%newline"/>
      </layout>
    </appender>
    
    <appender name="Console" type="log4net.Appender.ConsoleAppender">
      <layout type="log4net.Layout.PatternLayout">
        <conversionPattern value="%date %-5level: %message%newline"/>
      </layout>
    </appender>
    
  </log4net>
</configuration>
```

The PowerTranz URL can be set in the configuration file or by passing it into the PtzApi constructor.  Note that if it is provided via the constructor, the configuration file url if present will be ignored.

### Non-Breaking Changes
* Bluetooth connection improvements and fixes.  Note that the terminal *must* be paired with the device before attempting to connect.  The Bluetooth paired name must be used to connect.
* SDK no longer gets "stuck" after certain transaction failures.
* More logging has been added, duplicate logging lines were removed.
* Fixed receipt ApplicationLabel formatting.
* Receipt ApplicationLabel now comes from the Gateway for MSR or fallback. 
* Terminal Serial Number is now retrieved from the terminal and sent to the Gateway in every transaction (this is not TerminalId).  TerminalId no longer defaults to the terminal serial number if not supplied.
* *PTZMiuraTerminal.GetSerialNumber* fixed.
* New IsProduction property of *PTZMiuraTerminal* defaults to true.  Setting it to false raises additional events (*DidReceiveResponse* and *WillSendRequest*) for diagnostic purposes.
* *DidFailWithError* event will be be raised if a transaction other than a Credit or an Auth is sent to the terminal.
* *TerminalResult* was not always being set properly in the DidReceiveResponse response object. This has been fixed.
* Async versions of *PtzApi* methods are now available and should be used for all *PtzApi* requests.
* Exception handline improvements
* Various stability improvements


## Version 1.0.6
First release.
