Copy the following file from the "GRIDLinkSDK" directory to your application's source code tree:

GRIDLinkSDK\stubs\C stubs\GRIDApplication.c

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
