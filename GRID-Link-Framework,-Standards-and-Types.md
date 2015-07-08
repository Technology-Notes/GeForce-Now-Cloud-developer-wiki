GRID Link methods fall into three categories:

1. Initialization/Shutdown Methods: Static methods called at initialization and shutdown.
1. GRID Link API Methods: Methods called by you application to query or notify GRID.
1. GRID Application Methods: Method implemented by you to expose functionality to GRID.

In most cases, GRID Link API methods will return a GRIDLinkError type, defined in GRIDLinkSDK_CAPI.h. The application can check the return value in order to verify a given method succeeded.

All GRID Application methods return an ApplicationResult type that is used by GRID to determine if a given method has been implemented or not, and if so if a call to it has succeeded. See the GRID Application Methods section for more details.

## Memory
Any memory allocated by GRID Link API methods will be managed by the library and cleaned up during shutdown. The application is not responsible for freeing memory. However, this also means that any pointers or strings returned by GRID Link API methods are only valid until the shutdown call is made.

## Versioning
The GRID Link SDK provides a version number defined in “GRIDLinkSDK_CAPI.h” that can be used by the application to check which version is currently linked, if necessary.
