The GFN Link SDK (formerly known as GRID Link SDK) allows application developers to run their software to run in NVIDIA’s cloud environment known as GFN. Integration is simple to do and doesn't require a separate build process or target executable. The lightweight library adds no performance cost when operating outside of GeForce Now, so it is safe to include in builds that run on desktops or other environments.

<dl><a name="ovrvw" /></dl>
## Overview
The GRID Link SDK developer package consists of the following:<br/>
`Samples\...`			Sample application integrations<br/>
`GRIDLinkSDK\bin\...`		Win32 tools useful for development and debugging<br/>
`GRIDLinkSDK\dll\...`		Prebuilt development dll for running in a test environment<br/>
`GRIDLinkSDK\include\...`		GRID Link public include files<br/>
`GRIDLinkSDK\lib\...`		Prebuilt library files for various configurations<br/>
`GRIDLinkSDK\props\...`		VisualStudio .props files for easy integration<br/>
`GRIDLinkSDK\stubs\...`		Stubbed off implementation files that can be used for integration<br/>


At present, NVIDIA provides two different methods you can use to integrate with GFN:

<dl>
<table>
	<tr><th>Language</th><th>Description</th></tr>
	<tr>
		<td><b>C</b></td>
		<td>
		A traditional C library linked into your application. This option is best for applications written in C or other unmanaged languages.

		The C API is the most straightforward integration method, as it uses a traditional C library statically linked to the application. It provides a simple set of global methods to be called from the apps code, as well as a set of global method stubs that should be implemented by the developer.
		</td>
	</tr>
	<tr>
		<td><b>C++</b></td>
		<td>
		An object oriented API in C++. This may be preferable to the C API for those using C++.
		The C++ API provides an object oriented API as well as the advantage of a unique namespace, but is otherwise similar to the C API. C++ developers can choose to implement the stub class provided by NVIDIA or interface with GFN only through the C API methods using the ‘GRIDLinkSDK’ namespace.
		</td>
	</tr>
</table>
</dl>

<dl><a name="arch" /></dl>
## Architecture
The GFN Link SDK provides a very thin static C/C++ library that is linked to the game/application. This library checks for the presence of a `GRID.dll` at initialization time and if no dll is present essentially does nothing. All API methods are no-ops when operated this way so that an application could safely make these calls in all builds without worrying about performance problems or other errors. This way it is not necessary to make a GRID specific build or `#ifdef` out GRID API methods for other builds.

If the dll is there, the dll will attempt to establish communication with the GRID Service and passes any necessary information through that channel. 

A similar situation is used when testing the application during development, where the GRID Service replaced with a test application provided in the GRID Link SDK package.

![GRID Architecture](https://github.com/camify/GFN-Link/blob/master/docs/GridArch.png)