* List of all methods:
<dl>
	<ul>
		<li>[Initialization/Shutdown Methods](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#gridinit)
			<ul>
				<li>[`InitializeGFNLinkSDK`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_init)</li>
				<li>[`ShutdownGFNLinkSDK`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_shut)</li>
			</ul>
		</li>
		<li>[GFN Link API Methods](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#linkapi)
			<ul>
				<li>[`IsGFNEnabled`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_enbl)</li>
				<li>[`RequestKeyboardOverlayOpen`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_kbopen)</li>
				<li>[`RequestKeyboardOverlayClose`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_kbclose)</li>
				<li>[`RequestGFNAccessToken`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_token)</li>
	                        <li>[`Request3rdPartyToken`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_3token)</li>
	                        <li>[`Set3rdPartyToken`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_set3token)</li>
				<li>[`GetStorageLocation`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_stor)</li>
				<li>[`NotifyStorageChange`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_notif)</li>
			</ul>
		</li>
		<li>[GFN Application Methods](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#gridapp)
			<ul>
				<li>[`RequestApplicationPause`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_pause)</li>
				<li>[`RequestApplicationSave`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_save)</li>
				<li>[`RequestApplicationExit`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_exit)</li>
				<li>[`LockUserOptions`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_lock)</li>
				<li>[`SetLocale`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_locale)</li>
				<li>[`IsUpdateRequired`](/NVIDIA/GFN-Link/wiki/GFN-Link-SDK-Methods#m_update)</li>
			</ul>
		</li>
	</ul>
</dl>

<dl><a name="overview"></dl>
## Overview
GFN Link methods fall into three categories:

1. **Initialization/Shutdown Methods:** Static methods called at initialization and shutdown.
1. **GFN Link API Methods:** Methods called by you application to query or notify GFN.
1. **GFN Application Methods:** Method implemented by you to expose functionality to GFN.

In most cases, GFN Link API methods will return a `GFNLinkError` type, defined in `GFNLinkSDK_CAPI.h`. The application can check the return value in order to verify a given method succeeded.

All GFN Application methods return an `ApplicationResult` type that is used by GFN to determine if a given method has been implemented or not, and if so if a call to it has succeeded. See the GFN Application Methods section for more details.

### Memory
Any memory allocated by GFN Link API methods will be managed by the library and cleaned up during shutdown. The application is not responsible for freeing memory. However, this also means that any pointers or strings returned by GFN Link API methods are only valid until the shutdown call is made.

### Versioning
The GFN Link SDK provides a version number defined in `GFNLinkSDK_CAPI.h` that can be used by the application to check which version is currently linked, if necessary.

<dl><a name="gridinit"></dl>
## GFN Initialization/Shutdown Methods
<dl><a name="m_init"></dl>
### &#10146; InitializeGFNLinkSDK
C&nbsp;&nbsp;&nbsp;&nbsp;	`GFNLinkError glInitializeGFNLinkSDK()`<br/>
C++	`GFNLinkError GFNLinkSDK::InitializeGFNLinkSDK()`

_**Description**_<br/>Should be called at application startup and prior to any GFN Link API methods.
	When running outside of a GFN environment (a game seat virtual machine or development environment) it is expected for this method to return a result other than success. In this case all GFN methods become no-ops and have no performance impact on your application.

_**Usage**_<br/>Call as soon as possible during application startup.

_**Return Value**_
* `GFNLinkError::gleSuccess` on success when running in a GFN environment<br/>
* `GFNLinkError::gleGFNDLLNotPresent` if running outside a GFN environment (no GFN.dll present)<br/>
* `GFNLinkError::gleGFNComNotEstablished` if running outside a GFN environment (no GFN host or test application running)<br/>
* `GFNLinkError::gleIncompatibleVersion` Linked GFN Link SDK library is not compatible with the existing GFN.dll) 

<dl><a name="m_shut"></dl>
### &#10146; ShutdownGFNLinkSDK
C&nbsp;&nbsp;&nbsp;&nbsp;	`void glShutdownGFNLinkSDK()`<br/>
C++	`void GFNLinkSDK::ShutdownGFNLinkSDK()`

_**Description**_<br/>Should be called at Application shutdown. Frees up memory allocated by GFN and disconnects from GFN backend.

_**Usage**_<br/>Call during application shutdown or when GFN Link API methods are no longer needed.

<dl><a name="linkapi"></dl>
## GFN-Link Methods (IGFNLink Interface)
GFN Link API methods are used to make request from or to notify the GFN backend.
When your application is operating outside of the GFN environment, these methods are simple stubs that incur almost no cost, so it's safe to add these to your main build.
The calling convention differs by which API you've chosen to use. Examples of usage and the required include/using statement are given below:

C&nbsp;&nbsp;&nbsp;&nbsp;	`#include "GFNLinkSDK_CAPI.h"`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	**`glRequestKeyboardOverlayClose();`**

C++	`#include "GFNLinkSDK_CAPI.hpp"`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	**`GFNLinkSDK::Instance()->RequestKeyboardOverlayClose();`**

In most cases `IGFNLink` methods return a `GFNLinkError` result, which can be used by the application to check for errors – however, in practice it is unlikely to be useful for the application to check this value as an ideal implementation would not make logic changes based on the result. For example, should `RequestKeyboardOverlayOpen` fail due to running outside of a GFN environment, the application would still continue to accept keyboard input from the native input handler and/or text control. Additionally, since calling `RequestKeyboardOverlayClose` is safe to do even if `RequestKeyboardOverlayOpen` failed, the application can call it when input is no longer needed in all cases.

<dl><a name="m_enbl"></dl>
### &#10146; IsGFNEnabled
C&nbsp;&nbsp;&nbsp;&nbsp;	`bool glIsGFNEnabled()`<br/>
C++	`bool IGFNLink::IsGFNEnabled()`

_**Description**_<br/>
Determines if application is running in GFN environment or not.

_**Usage**_<br/>
	Use to enable any GFN specific application logic.

_**Return Value**_<br/>
* `true` Application is running on a game seat virtual machine or GFN test environment
* `false` Application is not running in a GFN Environment

<dl><a name="m_kbopen"></dl>
### &#10146; RequestKeyboardOverlayOpen
C&nbsp;&nbsp;&nbsp;&nbsp;	`GFNLinkError glRequestKeyboardOverlayOpen(GFNScreenPosition gspPosition)`<br/>
C++	`GFNLinkError IGFNLink::RequestKeyboardOverlayOpen(GFNScreenPosition gspPosition)`

_**Description**_<br/>
Called from application when it is expecting text input from user. Calling this API would trigger a native keyboard overlay to be shown to the GFN user such that he/she can most easily enter text, based on the particular GFN client platform being used.
There's no special input handling needed from application; input will be injected into application by GFN (as is done in all other times running in GFN). Note GFN is not displaying any text input box or prompt to user, only a keyboard overlay.

_**Usage**_<br/>
	This API should be called as a pair with `RequestKeyboardOverlayClose`. Multiple calls to `RequestKeyboardOverlayOpen` will have no effect after the first call.
	
_**Parameters**_<br/>
`gspPosition`	the desired screen positioning of text input element (i.e. Android keyboard). Should be one of the following values: `gspBottom, gspTop, gspLeft, gspRight, gspCenter, gspTopLeft, gspTopRight, gspBottomLeft, gspBottomRight`


_**Return Value**_<br/>
* `gleSuccess` On success
* Otherwise, appropriate error code
	
<dl><a name="m_kbclose"></dl>
### &#10146; RequestKeyboardOverlayClose
C&nbsp;&nbsp;&nbsp;&nbsp;	`GFNLinkError glRequestKeyboardOverlayClose()`<br/>
C++	`GFNLinkError IGFNLink::RequestKeyboardOverlayClose()`

_**Description**_<br/>
Called from application when necessary text input has been processed and user can continue. This would cause a previously requested keyboard overlay on the GFN user's client display to be dismissed 

_**Usage**_<br/>
	`RequestKeyboardOverlayOpen` should be called before this method. If not, `RequestKeyboardOverlayClose` will have no effect.


_**Return Value**_<br/>
* `gleSuccess` On success
* Otherwise, appropriate error code

<dl><a name="m_token"></dl>
### &#10146; RequestGFNAccessToken
C&nbsp;&nbsp;&nbsp;&nbsp;	`GFNLinkError glRequestGFNAccessToken(const char** ppchToken)`<br/>
C++	`GFNLinkError IGFNLink::RequestGFNAccessToken(const char** ppchToken)`

_**Description**_<br/>
Request to obtain a user specific access token to allow access to the GFN backend service (IDM endpoint). 

_**Usage**_<br/>
The access token provided can be used by the application’s backend servers to validate the user and obtain user data from the GFN backend service. The GFN backend service provides an OAuth2 interface for validating users and retrieving data. See Account Federation section for more information.

_**Parameters**_<br/>
`ppchToken`	Populated with a user specific GFN access token.


_**Return Value**_<br/>
* `gleSuccess` On success
* Otherwise, appropriate error code

<dl><a name="m_stor"></dl>
### &#10146; GetStorageLocation
C&nbsp;&nbsp;&nbsp;&nbsp;	`GFNLinkError glGetStorageLocation(const char** ppchStoragePath)`<br/>
C++	`GFNLinkError IGFNLink::GetStorageLocation(const char** ppchStoragePath)`

_**Description**_<br/>
Provides a path to a GFN managed storage location for the current application and user. All files and folders under this location will be persisted in GFN cloud storage. 

_**Usage**_<br/>
Application developers should use the provided location for storing user-specific application state files and options files. `NotifyStorageChange()` should be called once all files have been in order to trigger an immediate backup of these files.

_**Parameters**_<br/>
`ppchStoragePath`	Populated with a local directory path for the application to store files to.

_**Return Value**_<br/>
* `gleSuccess` On success
* Otherwise, appropriate error code

<dl><a name="m_notif"></dl>
### &#10146; NotifyStorageChange
C&nbsp;&nbsp;&nbsp;&nbsp;	`GFNLinkError glNotifyStorageChange()`<br/>
C++	`GFNLinkError IGFNLink::NotifyStorageChange()`

_**Description**_<br/>
Notifies GFN that file saves have completed and that it should immediately backup the local files to cloud storage. Note that all files are automatically saved at the end of a GFN gaming session, so this is only necessary in order to increase robustness.

_**Usage**_<br/>
Called from application when a set of file operations, as defined by the application, are completed at the GFN Storage location. GFN systems may use this notification to provide additional Cloud backup functionality. Application does not need to call this on every single file change.

_**Return Value**_<br/>
* `gleSuccess` On success
* Otherwise, appropriate error code

<dl><a name="gridapp"></dl>
## GFN-Application Methods (IGFNApplication Interface)
In order for GFN to make requests of your application, you will need to implement a set of methods stubbed off in the GFNApplication files provided by NVIDIA. You only need to implement those method that are applicable for your application and your business model, and should leave the default implementation for the remainder. The default implementation for these methods return `arNotImplemented`, which indicates to Grid that you’ve not implement that method.
Implementation in most cases will involve calling into your code in order to perform the requested operation and returning `arSuccess` or `arFailure` instead of the default `arNotImplemented`. In cases where the requested operation is asynchronous but no response is required, your application should not block until the operation is complete, but rather return success if the operation was successfully initiated and failure otherwise.

<dl><a name="m_pause"></dl>
### &#10146; RequestApplicationPause
C&nbsp;&nbsp;&nbsp;&nbsp;	`ApplicationResult glRequestApplicationPause()`<br/>
C++	`ApplicationResult IGFNApplication::RequestApplicationPause()`

_**Description**_<br/>
Should pause the application when called, if possible.
For Multiplayer games, it is recommended that this is implemented similar to a client disconnect.

_**Usage**_<br/>
GFN will call this method in cases where the user loses network connection with the game seat virtual machine or otherwise disconnects from his/her GFN session.

_**Return Value**_<br/>
* `arNotImplemented` Method not implemented for this application
* `arSuccess` Command accepted successfully
* `arFailure` Command not possible at this time

<dl><a name="m_save"></dl>
### &#10146; RequestApplicationSave
C&nbsp;&nbsp;&nbsp;&nbsp;	`ApplicationResult glRequestApplicationSave()`<br/>
C++	`ApplicationResult IGFNApplication::RequestApplicationSave()`

_**Description**_<br/>
Should save all user game and option data. 
It is recommended this be implemented as an autosave if such a feature is supported by your application.

_**Usage**_<br/>
GFN will call this method in order to save user progress in cases where the user has gone idle or otherwise disconnected from the game seat virtual machine.

_**Return Value**_<br/>
* `arNotImplemented` Method not implemented for this application
* `arSuccess` Command accepted successfully
* `arFailure` Command not possible at this time

<dl><a name="m_exit"></dl>
### &#10146; RequestApplicationExit
C&nbsp;&nbsp;&nbsp;&nbsp;	`ApplicationResult glRequestApplicationExit()`<br/>
C++	`ApplicationResult IGFNApplication::RequestApplicationExit()`

_**Description**_<br/>
Should perform a graceful exit of the application when called. 

_**Usage**_<br/>
GFN will call this method in cases where a GFN gaming session has completed prior to the user closing the application themselves; For example, after a prolonged network connection loss.

_**Return Value**_<br/>
* `arNotImplemented` Method not implemented for this application
* `arSuccess` Command accepted successfully
* `arFailure` Command not possible at this time

<dl><a name="m_lock"></dl>
### &#10146; LockUserOptions
C&nbsp;&nbsp;&nbsp;&nbsp;	`ApplicationResult glLockUserOptions(UserOptions uoOptions)`<br/>
C++	`ApplicationResult IGFNApplication::LockUserOptions(UserOptions uoOptions)`

_**Description**_<br/>
Should disable certain user option menus in the application, such as graphics options, screen resolution changes and windowed/fullscreen mode. 

_**Usage**_<br/>
GFN will call this method shortly after being initialized in order to inform the application which user options should be disabled. Alternatively, application developers can use calls to `IsGFNEnabled` at the appropriate locations in order to disable these options themselves.

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
C++	`ApplicationResult IGFNApplication::SetLocale(const char* pchlanguageCode)`

_**Description**_<br/>
Should set application’s localization settings to those provided by GFN.
Since GFN does not perform language specific installs for every language an application supports or have separate game seat virtual machine for each language, it is necessary to set the application locale to the user’s locale dynamically.
In order to properly implement this feature applications must be able to switch locales at runtime without restarting the application.

_**Usage**_<br/>
GFN will call this method shortly after being initialized in order to set the application’s locale to that requested by the user. If the requested locale is not supported, the application should return `arFailure` and use “en-US” as a default.

_**Parameters**_<br/>
`pchLanguageCode`	Code following ISO 639-1 and ISO 3166-1 standards (i.e. 'en-US')

_**Return Value**_<br/>
* `arNotImplemented` Method not implemented for this application
* `arSuccess` Command accepted successfully
* `arFailure` Command not possible at this time


<dl><a name="m_update"></dl>
### &#10146; IsUpdateRequired
C&nbsp;&nbsp;&nbsp;&nbsp;	`ApplicationResult glIsUpdateRequired(bool* pbUpdate)`<br/>
C++	`ApplicationResult IGFNApplication::IsUpdateRequired(bool* pbUpdate)`

_**Description**_<br/>
Determines if the application requires an update or patch in order to continue execution.
In most cases GFN will keep applications updated to the most recent version, but in cases where an application developer releases an update without proving this to NVIDIA, it’s possible that the application is temporarily out of date. Applications should not self patch in the GFN environment.
Note that this is querying if it is possible to run with the current executable, not whether the application is fully up to date. For example, if a game’s backend is backward compatible with slightly out of date clients, then this should return true rather than false.

_**Usage**_<br/>
GFN will call this method shortly after being initialized in order to determine if the application is able to run with the current version.

_**Parameters**_<br/>
`pbUpdate`	Should be set to true by the application if the current version of the application is not able to run, false otherwise.

_**Return Value**_<br/>
* `arNotImplemented` Method not implemented for this application
* `arSuccess` Command accepted successfully
* `arFailure` Command not possible at this time