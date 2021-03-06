NVIDIA provides a simple test tool in order to help with testing and debugging your application. This is located in the `Simulator` directory.

In order to use this tool, you should run the tool **_prior to starting your application_**. You may then launch your application through the tool or separately.

For reference, you can use the `SampleCPPGUIApplication` to test out the tool. You'll need to compile the sample application and open the compiled `.exe` in the tool. Be sure to use the correct `GFN.dll` binary (Win32 or x64) otherwise the application will not connect to GFN.

The tool can be used to test sending the various `IGFNApplication` commands to your application and also displays and emulates responses to `IGFNLink` calls from your application.

![GFN Simulator](https://github.com/camify/GFN-Link/blob/master/docs/GFNSimulator.png)
![Sample GFN App](https://github.com/camify/GFN-Link/blob/master/docs/sampleGFNapp.png)

<dl><h4>GFN Environment</h4></dl>
The “GFN Environment” is used to refer to the case where your application is running on a game seat virtual machine (see Architecture section). This can be emulated using the GFN Test Tool described above.
Application developers should be sure to test their application both in and out of the GFN Environment.
From a technical standpoint, two things must happen in order for the application to be in a GFN Environment:<br/>
1. 	The `GFN.dll` must be present next to the application executable or in a registered dll path.<br/>
2. 	The GFN Service or an emulator such at the GUI Test Tool must be running.

It is recommended that application developers NOT package the `GFN.dll` with their application, as otherwise there is a minor startup cost to loading this dll and attempting to connect to the GFN Service. NVIDIA will package the dll with your application as part of the GFN onboarding process.

