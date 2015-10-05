The C++ API provides an object oriented API as well as the advantage of a unique namespace, but is otherwise similar to the C API. C++ developers can choose to implement the stub class provided by NVidia or interface with GFN only through the C API methods using the `GFNLinkSDK` namespace.

For an example implementation, you can refer to the `SampleCPPApplication` project included in the GFN Link SDK package.

These steps will guide you with your integration.

<dl><a name="cpp_proj" /></dl>
## 1. Project Setup
Copy one the following files from the GFN Link SDK directory to your application’s source code tree. Use the `.c` file if you wish to use C API methods or use the `.cpp` file if you wish to use the C++ API:

`GFNLinkSDK\stubs\C stubs\GFNApplication.c`<br/>
`GFNLinkSDK\stubs\C++ stubs\GFNApplication.cpp`

Add the following include path to your compiler commands:

`GFNLinkSDK\include`

Link to the appropriate .lib file in the following location:

`GFNLinkSDK\lib\...`

Precompiled libraries are included for Win32 (x86) and x64 architectures, Debug and Release configurations and MS v100, v110 and v120 platform toolsets.

**Note for VS 2010 – 2013 users:**
You can easily integrate the correct library into your project by adding the appropriate .props file using the PropertyManager. There are two props files located here:

**GFNLinkSDK\props**

`GFNLinkSDK.props` should be used if you're using Multithreaded Dll CRT linkage (`/MD` option).

`GFNLinkSDKNT.props` should be used if you're using Multithreaded CRT linkage (`/MT` option).

No further action should be required to correctly compile and link in this case.


<dl><a name="cpp_setup" /></dl>
## 2. GFN Setup and Shutdown

Add a call to `GFNLinkSDK::InitializeGFNLinkSDK()` to your application’s startup code.

Add a call to `GFNLinkSDK::ShutdownGFNLinkSDK()` to your application’s shutdown code.

You will need to include `GFNLinkSDK_CAPI.h` for these definitions.

An Initialize/Shutdown pair should be added each time a process that needs to communicate with GFN is started.
For example, if you have a launcher that implements the patching methods and a game executable that implements the log-in methods, both should call Initialize at startup and Shutdown during shutdown.

<dl><a name="cpp_app" /></dl>
## 3. Implement GFN Application Methods
You should now begin implementing the methods stubbed out in the `GFNApplication.cpp` file that you copied into your project.

<dl><a name="cpp_api" /></dl>
## 4. Add GFN Link API Calls 
Lastly, you'll need to place GFN API calls in the appropriate locations in your code. See the GFN Link API Methods section for a detailed explanation of each method. 

You will need to include `GFNLinkSDK_CAPI.h` from any file that calls into the GFN API.

Note that in order to prevent any name collisions, all C/C++ API methods use the `GFNLinkSDK` namespace.
 