## Project Setup
Copy one the following files from the GRID Link SDK directory to your application’s source code tree. Use the .c file if you wish to use C API methods or use the .cpp file if you wish to use the C++ API:

GRIDLinkSDK\stubs\C stubs\GRIDApplication.c
GRIDLinkSDK\stubs\C++ stubs\GRIDApplication.cpp

Add the following include path to your compiler commands:

GRIDLinkSDK\include

Link to the appropriate .lib file in the following location:

GRIDLinkSDK\lib\...

Precompiled libraries are included for Win32 (x86) and x64 architectures, Debug and Release configurations and MS v100, v110 and v120 platform toolsets.

**Note for VS 2010 – 2013 users:**
You can easily integrate the correct library into your project by adding the appropriate .props file using the PropertyManager. There are two props files located here:

**GRIDLinkSDK\props**

GRIDLinkSDK.props should be used if you're using Multithreaded Dll CRT linkage (/MD option).

GRIDLinkSDKNT.props should be used if you're using Multithreaded CRT linkage (/MT option).

No further action should be required to correctly compile and link in this case.


## GRID Setup and Shutdown

Add a call to `GRIDLinkSDK::InitializeGRIDLinkSDK()` to your application’s startup code.

Add a call to `GRIDLinkSDK::ShutdownGRIDLinkSDK()` to your application’s shutdown code.

You will need to include "GRIDLinkSDK_CAPI.h" for these definitions.

An Initialize/Shutdown pair should be added each time a process that needs to communicate with GRID is started.
For example, if you have a launcher that implements the patching methods and a game executable that implements the log-in methods, both should call Initialize at startup and Shutdown during shutdown.

## Implement GRID Application Methods
You should now begin implementing the methods stubbed out in the GRIDApplication.cpp file that you copied into your project

## Add GRID Link API Calls 
Lastly, you'll need to place Grid API calls in the appropriate locations in your code. See the GRID Link API Methods section for a detailed explanation of each method. 

You will need to include "GRIDLinkSDK_CAPI.h" from any file that calls into the Grid API.

Note that in order to prevent any name collisions, all C/C++ API methods use the GRIDLinkSDK namespace.
 