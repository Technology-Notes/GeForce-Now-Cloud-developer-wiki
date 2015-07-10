<dl><a name="gridinit"></dl>
## GRID Initialization/Shutdown Methods
<dl><a name="m_init"></dl>
### &#10146; InitializeGRIDLinkSDK
C&nbsp;&nbsp;&nbsp;&nbsp;	`GRIDLinkError glInitializeGRIDLinkSDK()`<br/>
C++	`GRIDLinkError GRIDLinkSDK::InitializeGRIDLinkSDK()`

_**Description**_<br/>Should be called at application startup and prior to any GRID Link API methods.
	When running outside of a GRID environment (a game seat virtual machine or development environment) it is expected for this method to return a result other than success. In this case all GRID methods become no-ops and have no performance impact on your application.

_**Usage**_<br/>Call as soon as possible during application startup.

_**Return Value**_
* `GRIDLinkError::gleSuccess` on success when running in a GRID environment<br/>
* `GRIDLinkError::gleGRIDDLLNotPresent` if running outside a GRID environment (no GRID.dll present)<br/>
* `GRIDLinkError::gleGRIDComNotEstablished` if running outside a GRID environment (no GRID host or test application running)<br/>
* `GRIDLinkError::gleIncompatibleVersion` Linked GRID Link SDK library is not compatible with the existing GRID.dll) 

<dl><a name="m_shut"></dl>
### &#10146; ShutdownGRIDLinkSDK
C&nbsp;&nbsp;&nbsp;&nbsp;	`void glShutdownGRIDLinkSDK()`<br/>
C++	`void GRIDLinkSDK::ShutdownGRIDLinkSDK()`

_**Description**_<br/>Should be called at Application shutdown. Frees up memory allocated by GRID and disconnects from GRID backend.

_**Usage**_<br/>Call during application shutdown or when GRID Link API methods are no longer needed.

<dl><a name="linkapi"></dl>
## GRID-Link Methods (IGRIDLink Interface)
GRID Link API methods are used to make request from or to notify the GRID backend.
When your application is operating outside of the GRID environment, these methods are simple stubs that incur almost no cost, so it's safe to add these to your main build.
The calling convention differs by which API you've chosen to use. Examples of usage and the required include/using statement are given below:

C&nbsp;&nbsp;&nbsp;&nbsp;	`#include "GRIDLinkSDK_CAPI.h"`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	**`glRequestKeyboardOverlayClose();`**

C++	`#include "GRIDLinkSDK_CAPI.hpp"`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	**`GRIDLinkSDK::Instance()->RequestKeyboardOverlayClose();`**

In most cases `IGRIDLink` methods return a `GRIDLinkError` result, which can be used by the application to check for errors – however, in practice it is unlikely to be useful for the application to check this value as an ideal implementation would not make logic changes based on the result. For example, should `RequestKeyboardOverlayOpen` fail due to running outside of a GRID environment, the application would still continue to accept keyboard input from the native input handler and/or text control. Additionally, since calling `RequestKeyboardOverlayClose` is safe to do even if `RequestKeyboardOverlayOpen` failed, the application can call it when input is no longer needed in all cases.

<dl><a name="m_enbl"></dl>
### &#10146; IsGRIDEnabled
C&nbsp;&nbsp;&nbsp;&nbsp;	`bool glIsGRIDEnabled()`<br/>
C++	`bool IGRIDLink::IsGRIDEnabled()`

_**Description**_<br/>
Determines if application is running in GRID environment or not.

_**Usage**_<br/>
	Use to enable any GRID specific application logic.

_**Return Value**_<br/>
* `true` Application is running on a game seat virtual machine or GRID test environment
* `false` Application is not running in a GRID Environment

<dl><a name="m_kbopen"></dl>
### &#10146; RequestKeyboardOverlayOpen
C&nbsp;&nbsp;&nbsp;&nbsp;	`GRIDLinkError glRequestKeyboardOverlayOpen(GRIDScreenPosition gspPosition)`<br/>
C++	`GRIDLinkError IGRIDLink::RequestKeyboardOverlayOpen(GRIDScreenPosition gspPosition)`

_**Description**_<br/>
Called from application when it is expecting text input from user. Calling this API would trigger a native keyboard overlay to be shown to the GRID user such that he/she can most easily enter text, based on the particular GRID client platform being used.
There's no special input handling needed from application; input will be injected into application by GRID (as is done in all other times running in GRID). Note GRID is not displaying any text input box or prompt to user, only a keyboard overlay.

_**Usage**_<br/>
	This API should be called as a pair with `RequestKeyboardOverlayClose`. Multiple calls to `RequestKeyboardOverlayOpen` will have no effect after the first call.
	
_**Parameters**_<br/>
`gspPosition`	the desired screen positioning of text input element (i.e. Android keyboard). Should be one of the following values: `gspBottom, gspTop, gspLeft, gspRight, gspCenter, gspTopLeft, gspTopRight, gspBottomLeft, gspBottomRight`


_**Return Value**_<br/>
* `gleSuccess` On success
* Otherwise, appropriate error code
	
<dl><a name="m_kbclose"></dl>
### &#10146; RequestKeyboardOverlayClose
C&nbsp;&nbsp;&nbsp;&nbsp;	`GRIDLinkError glRequestKeyboardOverlayClose()`<br/>
C++	`GRIDLinkError IGRIDLink::RequestKeyboardOverlayClose()`

_**Description**_<br/>
Called from application when necessary text input has been processed and user can continue. This would cause a previously requested keyboard overlay on the GRID user's client display to be dismissed 

_**Usage**_<br/>
	`RequestKeyboardOverlayOpen` should be called before this method. If not, `RequestKeyboardOverlayClose` will have no effect.


_**Return Value**_<br/>
* `gleSuccess` On success
* Otherwise, appropriate error code

<dl><a name="m_token"></dl>
### &#10146; RequestGRIDAccessToken
C&nbsp;&nbsp;&nbsp;&nbsp;	`GRIDLinkError glRequestGRIDAccessToken(const char** ppchToken)`<br/>
C++	`GRIDLinkError IGRIDLink::RequestGRIDAccessToken(const char** ppchToken)`

_**Description**_<br/>
Request to obtain a user specific access token to allow access to the GRID backend service (IDM endpoint). 

_**Usage**_<br/>
The access token provided can be used by the application’s backend servers to validate the user and obtain user data from the GRID backend service. The GRID backend service provides an OAuth2 interface for validating users and retrieving data. See Account Federation section for more information.

_**Parameters**_<br/>
`ppchToken`	Populated with a user specific GRID access token.


_**Return Value**_<br/>
* `gleSuccess` On success
* Otherwise, appropriate error code

<dl><a name="m_stor"></dl>
### &#10146; GetStorageLocation
C&nbsp;&nbsp;&nbsp;&nbsp;	`GRIDLinkError glGetStorageLocation(const char** ppchStoragePath)`<br/>
C++	`GRIDLinkError IGRIDLink::GetStorageLocation(const char** ppchStoragePath)`

_**Description**_<br/>
Provides a path to a GRID managed storage location for the current application and user. All files and folders under this location will be persisted in GRID cloud storage. 

_**Usage**_<br/>
Application developers should use the provided location for storing user-specific application state files and options files. `NotifyStorageChange()` should be called once all files have been in order to trigger an immediate backup of these files.

_**Parameters**_<br/>
`ppchStoragePath`	Populated with a local directory path for the application to store files to.

_**Return Value**_<br/>
* `gleSuccess` On success
* Otherwise, appropriate error code

<dl><a name="m_notif"></dl>
### &#10146; NotifyStorageChange
C&nbsp;&nbsp;&nbsp;&nbsp;	`GRIDLinkError glNotifyStorageChange()`<br/>
C++	`GRIDLinkError IGRIDLink::NotifyStorageChange()`

_**Description**_<br/>
Notifies GRID that file saves have completed and that it should immediately backup the local files to cloud storage. Note that all files are automatically saved at the end of a GRID gaming session, so this is only necessary in order to increase robustness.

_**Usage**_<br/>
Called from application when a set of file operations, as defined by the application, are completed at the GRID Storage location. GRID systems may use this notification to provide additional Cloud backup functionality. Application does not need to call this on every single file change.

_**Return Value**_<br/>
* `gleSuccess` On success
* Otherwise, appropriate error code

<dl><a name="gridapp"></dl>
## GRID-Application Methods (IGRIDApplication Interface)
In order for GRID to make requests of your application, you will need to implement a set of methods stubbed off in the GRIDApplication files provided by NVIDIA. You only need to implement those method that are applicable for your application and your business model, and should leave the default implementation for the remainder. The default implementation for these methods return `arNotImplemented`, which indicates to Grid that you’ve not implement that method.
Implementation in most cases will involve calling into your code in order to perform the requested operation and returning `arSuccess` or `arFailure` instead of the default `arNotImplemented`. In cases where the requested operation is asynchronous but no response is required, your application should not block until the operation is complete, but rather return success if the operation was successfully initiated and failure otherwise.

<dl><a name="m_pause"></dl>
### &#10146; RequestApplicationPause
C&nbsp;&nbsp;&nbsp;&nbsp;	`ApplicationResult glRequestApplicationPause()`<br/>
C++	`ApplicationResult IGRIDApplication::RequestApplicationPause()`

_**Description**_<br/>
Should pause the application when called, if possible.
For Multiplayer games, it is recommended that this is implemented similar to a client disconnect.

_**Usage**_<br/>
GRID will call this method in cases where the user loses network connection with the game seat virtual machine or otherwise disconnects from his/her GRID session.

_**Return Value**_<br/>
* `arNotImplemented` Method not implemented for this application
* `arSuccess` Command accepted successfully
* `arFailure` Command not possible at this time


<dl><a name="m_save"></dl>
### &#10146; RequestApplicationSave
C&nbsp;&nbsp;&nbsp;&nbsp;	`ApplicationResult glRequestApplicationSave()`<br/>
C++	`ApplicationResult IGRIDApplication::RequestApplicationSave()`

_**Description**_<br/>
Should save all user game and option data. 
It is recommended this be implemented as an autosave if such a feature is supported by your application.

_**Usage**_<br/>
GRID will call this method in order to save user progress in cases where the user has gone idle or otherwise disconnected from the game seat virtual machine.

_**Return Value**_<br/>
* `arNotImplemented` Method not implemented for this application
* `arSuccess` Command accepted successfully
* `arFailure` Command not possible at this time

<dl><a name="m_exit"></dl>
### &#10146; RequestApplicationExit
C&nbsp;&nbsp;&nbsp;&nbsp;	`ApplicationResult glRequestApplicationExit()`<br/>
C++	`ApplicationResult IGRIDApplication::RequestApplicationExit()`

_**Description**_<br/>
Should perform a graceful exit of the application when called. 

_**Usage**_<br/>
GRID will call this method in cases where a GRID gaming session has completed prior to the user closing the application themselves; For example, after a prolonged network connection loss.

_**Return Value**_<br/>
* `arNotImplemented` Method not implemented for this application
* `arSuccess` Command accepted successfully
* `arFailure` Command not possible at this time

<dl><a name="m_lock"></dl>
### &#10146; LockUserOptions
C&nbsp;&nbsp;&nbsp;&nbsp;	`ApplicationResult glLockUserOptions(UserOptions uoOptions)`<br/>
C++	`ApplicationResult IGRIDApplication::LockUserOptions(UserOptions uoOptions)`

_**Description**_<br/>
Should disable certain user option menus in the application, such as graphics options, screen resolution changes and windowed/fullscreen mode. 

_**Usage**_<br/>
GRID will call this method shortly after being initialized in order to inform the application which user options should be disabled. Alternatively, application developers can use calls to `IsGRIDEnabled` at the appropriate locations in order to disable these options themselves.

_**Parameters**_<br/>
`uoOptions`	Set of flags representing the types of options to disable<br/>
At present only a single option flag has been specified:
`uoGraphicsSettings` Disable all optional graphics settings, including screen resolution, windowed mode and graphics quality options.

_**Return Value**_<br/>
* `arNotImplemented` Method not implemented for this application
* `arSuccess` Command accepted successfully
* `arFailure` Command not possible at this time 

<dl><a name="m_locale"></dl>
### &#10146; SetLocale
C&nbsp;&nbsp;&nbsp;&nbsp;	`ApplicationResult glSetLocale(const char* pchlanguageCode)`<br/>
C++	`ApplicationResult IGRIDApplication::SetLocale(const char* pchlanguageCode)`

_**Description**_<br/>
Should set application’s localization settings to those provided by GRID.
Since GRID does not perform language specific installs for every language an application supports or have separate game seat virtual machine for each language, it is necessary to set the application locale to the user’s locale dynamically.
In order to properly implement this feature applications must be able to switch locales at runtime without restarting the application.

_**Usage**_<br/>
GRID will call this method shortly after being initialized in order to set the application’s locale to that requested by the user. If the requested locale is not supported, the application should return `arFailure` and use “en-US” as a default.

_**Parameters**_<br/>
`pchLanguageCode`	Code following ISO 639-1 and ISO 3166-1 standards (i.e. 'en-US')

_**Return Value**_<br/>
* `arNotImplemented` Method not implemented for this application
* `arSuccess` Command accepted successfully
* `arFailure` Command not possible at this time


<dl><a name="m_update"></dl>
### &#10146; IsUpdateRequired
C&nbsp;&nbsp;&nbsp;&nbsp;	`ApplicationResult glIsUpdateRequired(bool* pbUpdate)`<br/>
C++	`ApplicationResult IGRIDApplication::IsUpdateRequired(bool* pbUpdate)`

_**Description**_<br/>
Determines if the application requires an update or patch in order to continue execution.
In most cases GRID will keep applications updated to the most recent version, but in cases where an application developer releases an update without proving this to NVIDIA, it’s possible that the application is temporarily out of date. Applications should not self patch in the GRID environment.
Note that this is querying if it is possible to run with the current executable, not whether the application is fully up to date. For example, if a game’s backend is backward compatible with slightly out of date clients, then this should return true rather than false.

_**Usage**_<br/>
GRID will call this method shortly after being initialized in order to determine if the application is able to run with the current version.

_**Parameters**_<br/>
`pbUpdate`	Should be set to true by the application if the current version of the application is not able to run, false otherwise.

_**Return Value**_<br/>
* `arNotImplemented` Method not implemented for this application
* `arSuccess` Command accepted successfully
* `arFailure` Command not possible at this time