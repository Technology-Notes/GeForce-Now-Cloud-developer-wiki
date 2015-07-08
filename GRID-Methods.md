<script>
alert(123);
</script>
## GRID Initialization/Shutdown Methods

### InitializeGRIDLinkSDK
C	`GRIDLinkError glInitializeGRIDLinkSDK()`<br/>
C++	`GRIDLinkError GRIDLinkSDK::InitializeGRIDLinkSDK()`

#### Description
	Should be called at application startup and prior to any GRID Link API methods.
	When running outside of a GRID environment (a game seat virtual machine or development environment) it is expected for this method to return a result other than success. In this case all GRID methods become no-ops and have no performance impact on your application.

````### *Usage*

	Call as soon as possible during application startup.

#### Return values

	`GRIDLinkError::gleSuccess` on success when running in a GRID environment.
	`GRIDLinkError::gleGRIDDLLNotPresent` if running outside a GRID environment (no GRID.dll present)
	`GRIDLinkError::gleGRIDComNotEstablished` if running outside a GRID environment (no GRID host or test application running)
	`GRIDLinkError::gleIncompatibleVersion` Linked GRID Link SDK library is not compatible with the existing GRID.dll) 

### ShutdownGRIDLinkSDK
C	`void glShutdownGRIDLinkSDK()`
C++	`void GRIDLinkSDK::ShutdownGRIDLinkSDK()`

Description
	Should be called at Application shutdown. Frees up memory allocated by GRID and disconnects from GRID backend.
Usage
	Call during application shutdown or when GRID Link API methods are no longer needed.


## GRID Link API Methods (IGRIDLink Interface)

## GRID Application Methods (IGRIDApplication Interface)


