## GRID Initialization/Shutdown Methods
### InitializeGRIDLinkSDK
C&nbsp;&nbsp;&nbsp;&nbsp;	`GRIDLinkError glInitializeGRIDLinkSDK()`<br/>
C++	`GRIDLinkError GRIDLinkSDK::InitializeGRIDLinkSDK()`

_**Description**_<br/>Should be called at application startup and prior to any GRID Link API methods.
	When running outside of a GRID environment (a game seat virtual machine or development environment) it is expected for this method to return a result other than success. In this case all GRID methods become no-ops and have no performance impact on your application.

_**Usage**_<br/>Call as soon as possible during application startup.

_**Return values**_<br/>`GRIDLinkError::gleSuccess` on success when running in a GRID environment<br/>
`GRIDLinkError::gleGRIDDLLNotPresent` if running outside a GRID environment (no GRID.dll present)<br/>
`GRIDLinkError::gleGRIDComNotEstablished` if running outside a GRID environment (no GRID host or test application running)<br/>
	`GRIDLinkError::gleIncompatibleVersion` Linked GRID Link SDK library is not compatible with the existing GRID.dll) 

### ShutdownGRIDLinkSDK
C&nbsp;&nbsp;&nbsp;&nbsp;	`void glShutdownGRIDLinkSDK()`<br/>
C++	`void GRIDLinkSDK::ShutdownGRIDLinkSDK()`

_**Description**_<br/>Should be called at Application shutdown. Frees up memory allocated by GRID and disconnects from GRID backend.

_**Usage**_<br/>Call during application shutdown or when GRID Link API methods are no longer needed.

## GRID Link API Methods (IGRIDLink Interface)

## GRID Application Methods (IGRIDApplication Interface)
