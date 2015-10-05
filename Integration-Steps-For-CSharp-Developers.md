The C# API provides an object oriented API that is very similar to the C++ API.  C# developers must implement the sub class `GFNApplication` provided by Nvidia.
For an example implementation, you can refer to the `SampleCSharpApplication` project included in the GFN Link SDK package.

These steps will guide you with your integration.

<dl><a name="cs_proj" /></dl>
## 1. Project Setup
Copy the following DLLs from the GFN Link SDK directory to your application’s output directory: `GFNLinkNativeWrapper.dll` and `GFNManagedLink.dll`.  Multiple versions of the DLLs are provided depending upon the configuration and platform.  The DLLs are located in the following location:<br/> `GFNLink\SDK\Managed\...`

Copy the sample `GfnApplication.cs` from the following path and add it to your application’s source code tree.
`GFNLinkSDK\stubs\NET stubs\...`


<dl><a name="cs_setup" /></dl>
## 2. GFN Setup and Shutdown

Add a call to `GFNLinkSDK::InitializeGFNLinkSDK()` to your application’s startup code.

Add a call to `GFNLinkSDK::ShutdownGFNLinkSDK()` to your application’s shutdown code.

You will need to include `GFNLinkSDK_CAPI.h` for these definitions.

An Initialize/Shutdown pair should be added each time a process that needs to communicate with GFN is started.
For example, if you have a launcher that implements the patching methods and a game executable that implements the log-in methods, both should call Initialize at startup and Shutdown during shutdown.

<dl><a name="cs_app" /></dl>
## 3. Implement GFN Application Methods
You should now begin implementing the methods stubbed out in the `GFNApplication.cpp` file that you copied into your project.

<dl><a name="cs_api" /></dl>
## 4. Add GFN Link API Calls 
Lastly, you'll need to place Grid API calls in the appropriate locations in your code. See the GFN Link API Methods section for a detailed explanation of each method. 

You will need to include `GFNLinkSDK_CAPI.h` from any file that calls into the Grid API.

Note that in order to prevent any name collisions, all C/C++ API methods use the `GFNLinkSDK` namespace.
 