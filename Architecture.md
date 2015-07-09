The GRID Link SDK provides a very thin static C/C++ library that is linked to the game/application. This library checks for the presence of a `GRID.dll` at initialization time and if no dll is present essentially does nothing. All API methods are no-ops when operated this way so that an application could safely make these calls in all builds without worrying about performance problems or other errors. This way it is not necessary to make a GRID specific build or `#ifdef` out GRID API methods for other builds.

If the dll is there, the dll will attempt to establish communication with the GRID Service and passes any necessary information through that channel. 

A similar situation is used when testing the application during development, where the GRID Service replaced with a test application provided in the GRID Link SDK package.

![GRID Architecture](https://github.com/camify/GFN-Link/blob/master/docs/GridArch.png)