The C API is the most straightforward integration method, as it uses a traditional C library statically linked to your application. It provides a simple set of global methods to be called from your code, as well as a set of global method stubs that should be implemented by you.
 
For an example implementation, you can refer to the “SampleCApplication” project included in the GFN Link SDK package.

<dl><a name="c_proj" /></dl>
## 1. Project Setup
Copy the following file from the "GFNLinkSDK" directory to your application's source code tree:

`GFNLinkSDK\stubs\C stubs\GFNApplication.c`

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

<dl><a name="c_setup" /></dl>
## 2. GFN Setup and Shutdown

Add a call to `glInitializeGFNLinkSDK()` to your application’s startup code. 

Add a call to `glShutdownGFNLinkSDK()` to your application’s shutdown code.

You will need to include `GFNLinkSDK_CAPI.h` for these definitions.

An Initialize/Shutdown pair should be added each time a process that needs to communicate with GFN is started.
For example, if you have a launcher that implements the patching methods and a game executable that implements the log-in methods, both should call Initialize at startup and Shutdown during shutdown.

<dl><a name="c_app" /></dl>
## 3. Implement GFN Application Methods
You should now begin implementing the methods stubbed out in the `GFNApplication.c`  file that you copied into your project.

<dl><a name="c_api" /></dl>
## 4. Add GFN Link API Calls 
Lastly, you'll need to place Grid API calls in the appropriate locations in your code. See the GFN Link API Methods section for a detailed explanation of each method. 

You will need to include `GFNLinkSDK_CAPI.h` from any file that calls into the Grid API.

Note that in order to prevent any name collisions, all GFN Link C API methods are prefixed by “gl”.
 
