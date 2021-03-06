## Introdution
The GFN Link SDK allows application developers to run their software  in NVIDIA’s cloud environment known as GeForce Now. Integration is simple to do and doesn't require a separate build process or target executable. The lightweight library adds no performance cost when operating outside of GeForce Now, so it is safe to include in builds that run on desktops or other environments.

<dl><a name="arch" /></dl>
### Architecture
The GFN Link SDK provides a very thin static C/C++ library that is linked to the game/application. This library checks for the presence of a `GFN.dll` at initialization time and if no dll is present essentially does nothing. All API methods are no-ops when operated this way so that an application could safely make these calls in all builds without worrying about performance problems or other errors. This way it is not necessary to make a GFN specific build or `#ifdef` out GFN API methods for other builds.

If the dll is there, the dll will attempt to establish communication with the GFN Service and passes any necessary information through that channel. 

A similar situation is used when testing the application during development, where the GFN Service replaced with a test application provided in the GFN Link SDK package.

![GFN Architecture](https://github.com/camify/GFN-Link/blob/master/docs/GameSeat.png)

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
		The C++ API provides an object oriented API as well as the advantage of a unique namespace, but is otherwise similar to the C API. C++ developers can choose to implement the stub class provided by NVIDIA or interface with GFN only through the C API methods using the ‘GFNLinkSDK’ namespace.
		</td>
	</tr>
	<tr>
		<td><b>C#</b></td>
		<td>
		An object oriented API in C#.  This API is very similar to the C++ API.  It provides a light weight wrapper around the native implementation making it easy to implement a managed solution.  C# developers must implement a class deriving from BaseGFNApplication either through the stub class provided by NVIDIA or their own custom class.
		</td>
	</tr>
</table>
</dl>
