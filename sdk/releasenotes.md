# Release Notes

## Version 2.0.0
Note that as there are breaking changes, this version is not backwards compatible with 1.0.6.
### Breaking Changes
#### Model changes 
*ExternalBatchId* has been changed to **ExternalBatchIdentifier**.  
*OrderId* has been changed to **OrderIdentifier**.

The following property, enum, and ... names have been changed  

Context | Old Name | New Name
---|---|---
_PtzPaymentRequest_ | ExternalBatchId | ExernalBatchIdentifier
_PtzPaymentRequest_ | OrderId | OrderIdentifier
public enum | PTtzPosEntryMode | PtzPosEntryMode

#### Miscellaneous
Terminal *HardReset* has been removed.
Terminal *SoftReset* has been changed to **ResetDevice** and is accessed directly from the terminal instance.

There is a new _PtzPosEntryMode_ for receipts.  This will be returned as mode **3** and in code it is _PtzPosEntryMode.PtzPosEntryModeFallback_.  It will be returned in the receipt data when there was a card fallback.

### Configuration File
There is a new config file which can be used to set the PowerTranz Gateway URL and configure log4net logging parameters.  The file must be in the same folder as PowerTranzSDK.dll and must be called *PowerTranzSDK.dll.config

The following is a sample PowerTranzSDK.dll.config:


### Non-Breaking Changes
* Bluetooth connection improvements and fixes.  Note that the terminal *must* be paired with the device before attempting to connect.  The Bluetooth paired name must be used to connect.
* SDK no longer gets "stuck" after certain transaction failures
* 


## Version 1.0.6
First release.
