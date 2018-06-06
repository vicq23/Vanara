![Vanara](/docs/icons/VanaraHeading.png)

In a number of projects I have needed to use native API calls. Over the years I have collected quite a few of these interop code chunks and I decided to pull them all together into a set of reusable libraries. I have tried to carve up the libraries into small enough chunks that they are easy to identify and consume. The only problem with this is that you can literally end up with over 20 dependencies on some of the higher function libraries (oh well).

I have tried to follow the concepts below in laying out the libraries.
* All functions that are imported from a single DLL should be placed into a single assembly that is named after the DLL
  * (e.g. The assembly `Vanara.PInvoke.Gdi32.dll` hosts all functions and supporting enumerations, constants and structures that are exported from `gdi32.dll` in the system directory.)
* Any structure or macro or enumeration (no function) that is used by many libraries is put into either `Vanara.Core` or `Vanara.PInvoke.Shared`
  * (e.g. The macro `HIWORD` and the structure `SIZE` are both in `Vanara.PInvoke.Shared` and classes to simplfy interop calls and native memory management are in `Vanara.Core`.)
* Inside a project, all constructs are contained in a file named after the header file (*.h) in which they are defined in the Windows API
  * (e.g. In the Vanara.PInvoke.Kernel32 project directory, you'll find a FileApi.cs, a WinBase.cs and a WinNT.cs file representing fileapi.h, winbase.h and winnt.h respectively.)
* Where the direct interpretation of a structure leads to memory leaks or misuse, I have tried to simplify their use
* Where structures are always passed by reference and where that structure needs to clean up memory allocations, I have changed the structure to a class implementing `IDisposable`.
* Wherever possible, all handles have been turned into `SafeHandle` derivatives.
* Wherever possible, all functions that allocate memory that is to be freed by the caller use a safe memory handle.
* All PInvoke calls are in assemblies prefixed by `Vanara.PInvoke`
* If there are classes or extensions that make use of the PInvoke calls, they are in wrapper assemblies prefixed by `Vanara` and then followed by a logical name for the functionality. Today, those are Core, Security, SystemServices, Windows.Forms and Windows.Shell.

## Quick Links
* [Documentation](https://github.com/dahall/Vanara/wiki)
* [Issues](https://github.com/dahall/Vanara/issues)

## Installation
This project's assemblies are available via NuGet.

NuGet Link | Assembly | Description | Dependencies
---- | -------- | ----------- | ------------
[![NuGet](https://img.shields.io/nuget/v/Vanara.Core.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.Core)| [Vanara.Core](https://github.com/dahall/Vanara/blob/master/Core/AssemblyReport.md) | Shared methods, structures and constants for use throughout the Vanara assemblies. Think of it as windows.h with some useful extensions. | None
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.Shared.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.Shared)| [Vanara.PInvoke.Shared](https://github.com/dahall/Vanara/blob/master/PInvoke/Shared/AssemblyReport.md) | Shared methods, structures and constants for use throughout the Vanara.PInvoke assemblies | Vanara.Core
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.AclUI.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.AclUI)| [Vanara.PInvoke.AclUI](https://github.com/dahall/Vanara/blob/master/PInvoke/AclUI/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-100%25-green.svg) | Methods, structures and constants imported from AclUI.dll | Vanara.PInvoke.Security
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.BITS.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.BITS)| [Vanara.PInvoke.BITS](https://github.com/dahall/Vanara/blob/master/PInvoke/BITS/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-100%25-green.svg) | Methods, structures and constants imported from qmgr.dll for Background Intelligent Transfer Service (BITS) | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.ComCtl32.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.ComCtl32)| [Vanara.PInvoke.ComCtl32](https://github.com/dahall/Vanara/blob/master/PInvoke/ComCtl32/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-100%25-green.svg) | Methods, structures and constants imported from ComCtl32.dll | Vanara.PInvoke.User32
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.CredUI.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.CredUI)| [Vanara.PInvoke.CredUI](https://github.com/dahall/Vanara/blob/master/PInvoke/CredUI/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-28%25-yellow.svg) | Methods, structures and constants imported from CredUI.dll | Vanara.PInvoke.Security
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.Crypt32.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.Crypt32)| [Vanara.PInvoke.Crypt32](https://github.com/dahall/Vanara/blob/master/PInvoke/Crypt32/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-0%25-red.svg) | Methods, structures and constants imported from Crypt32.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.DwmApi.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.DwmApi)| [Vanara.PInvoke.DwmApi](https://github.com/dahall/Vanara/blob/master/PInvoke/DwmApi/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-100%25-green.svg) | Methods, structures and constants imported from DwmApi.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.Gdi32.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.Gdi32)| [Vanara.PInvoke.Gdi32](https://github.com/dahall/Vanara/blob/master/PInvoke/Gdi32/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-1%25-red.svg) | Methods, structures and constants imported from Gdi32.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.IpHlpApi.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.IpHlpApi)| [Vanara.PInvoke.IpHlpApi](https://github.com/dahall/Vanara/blob/master/PInvoke/IpHlpApi/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-25%25-yellow.svg) | Methods, structures and constants imported from IpHlpApi.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.Kernel32.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.Kernel32)| [Vanara.PInvoke.Kernel32](https://github.com/dahall/Vanara/blob/master/PInvoke/Kernel32/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-38%25-yellow.svg) | Methods, structures and constants imported from Kernel32.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.Mpr.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.Mpr)| [Vanara.PInvoke.Mpr](https://github.com/dahall/Vanara/blob/master/PInvoke/Mpr/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-0%25-red.svg) | Methods, structures and constants imported from Mpr.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.NetApi32.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.NetApi32)| [Vanara.PInvoke.NetApi32](https://github.com/dahall/Vanara/blob/master/PInvoke/NetApi32/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-1%25-red.svg) | Methods, structures and constants imported from NetApi32.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.NetListMgr.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.NetListMgr)| [Vanara.PInvoke.NetListMgr](https://github.com/dahall/Vanara/blob/master/PInvoke/NetListMgr/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-0%25-red.svg) | Methods, structures and constants imported from NetListMgr.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.NTDSApi.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.NTDSApi)| [Vanara.PInvoke.NTDSApi](https://github.com/dahall/Vanara/blob/master/PInvoke/NTDSApi/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-1%25-red.svg) | Methods, structures and constants imported from NTDSApi.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.Ole.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.Ole)| [Vanara.PInvoke.Ole](https://github.com/dahall/Vanara/blob/master/PInvoke/Ole/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-7%25-red.svg) | Methods, structures and constants imported from Ole32.dll, OleAut32 and PropSys.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.Security.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.Security)| [Vanara.PInvoke.Security](https://github.com/dahall/Vanara/blob/master/PInvoke/Security/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-7%25-red.svg) | Methods, structures and constants imported from AdvApi32.dll, Authz.dll and Secur32.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.Shell32.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.Shell32)| [Vanara.PInvoke.Shell32](https://github.com/dahall/Vanara/blob/master/PInvoke/Shell32/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-16%25-red.svg) | Methods, structures and constants imported from Shell32.dll | Vanara.PInvoke.ComCtl32, Vanara.PInvoke.Ole, Vanara.PInvoke.Security
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.ShlwApi.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.ShlwApi)| [Vanara.PInvoke.ShlwApi](https://github.com/dahall/Vanara/blob/master/PInvoke/ShlwApi/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-0%25-red.svg) | Methods, structures and constants imported from ShlwApi.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.TaskSchd.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.TaskSchd)| [Vanara.PInvoke.TaskSchd](https://github.com/dahall/Vanara/blob/master/PInvoke/TaskSchd/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-100%25-green.svg) | Methods, structures and constants imported from TaskSchd.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.User32.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.User32)| [Vanara.PInvoke.User32](https://github.com/dahall/Vanara/blob/master/PInvoke/User32/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-2%25-red.svg) | Methods, structures and constants imported from User32.dll | Vanara.PInvoke.Kernel32
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.User32.Gdi.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.User32.Gdi)| [Vanara.PInvoke.User32.Gdi](https://github.com/dahall/Vanara/blob/master/PInvoke/User32.Gdi/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-2%25-red.svg) | Methods, structures and constants imported from User32.dll that have GDI references | Vanara.PInvoke.Gdi32, Vanara.PInvoke.Kernel32
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.UxTheme.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.UxTheme)| [Vanara.PInvoke.UxTheme](https://github.com/dahall/Vanara/blob/master/PInvoke/UxTheme/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-100%25-green.svg) | Methods, structures and constants imported from UxTheme.dll | Vanara.PInvoke.Gdi32
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.VirtDisk.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.VirtDisk)| [Vanara.PInvoke.VirtDisk](https://github.com/dahall/Vanara/blob/master/PInvoke/VirtDisk/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-100%25-green.svg) | Methods, structures and constants imported from VirtDisk.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.PInvoke.WinINet.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.PInvoke.WinINet)| [Vanara.PInvoke.WinINet](https://github.com/dahall/Vanara/blob/master/PInvoke/WinINet/CorrelationReport.md)<br>![Coverage](https://img.shields.io/badge/coverage-1%25-red.svg) | Methods, structures and constants imported from WinINet.dll | Vanara.PInvoke.Shared
[![NuGet](https://img.shields.io/nuget/v/Vanara.Security.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.Security)| [Vanara.Security](https://github.com/dahall/Vanara/blob/master/Security/AssemblyReport.md) | Wrapper classes for security related items in the PInvoke libraries | Vanara.PInvoke.NTDSApi, Vanara.PInvoke.Security
[![NuGet](https://img.shields.io/nuget/v/Vanara.SystemServices.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.SystemServices)| [Vanara.SystemServices](https://github.com/dahall/Vanara/blob/master/System/AssemblyReport.md) | Wrapper classes for system related items in the PInvoke libraries | Vanara.PInvoke.Kernel32, Vanara.PInvoke.NetListMgr, Vanara.PInvoke.VirtDisk
[![NuGet](https://img.shields.io/nuget/v/Vanara.Windows.Forms.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.Windows.Forms)| [Vanara.Windows.Forms](https://github.com/dahall/Vanara/blob/master/WIndows.Forms/AssemblyReport.md) | Classes for user interface related items derived from the Vanara PInvoke libraries. Includes extensions for almost all common controls to give post Vista capabilities, WinForms controls (panel, commandlink, enhanced combo boxes, IPAddress, split button, trackbar and themed controls), shutdown/restart/lock control, buffered painting, resource files, access control editor, simplifed designer framework for Windows.Forms. | Vanara.PInvoke.AclUI, Vanara.PInvoke.CredUI, Vanara.PInvoke.ComCtl32, Vanara.PInvoke.DwmApi, Vanara.PInvoke.UxTheme
[![NuGet](https://img.shields.io/nuget/v/Vanara.Windows.Shell.svg?style=flat-square)](https://www.nuget.org/packages/Vanara.Windows.Shell)| [Vanara.Windows.Shell](https://github.com/dahall/Vanara/blob/master/Windows.Shell/AssemblyReport.md) | Classes for Windows Shell items derived from the Vanara PInvoke libraries. Includes shell items, files, icons, links, and taskbar lists. | Vanara.PInvoke.Shell32

## Sample Code
There are numerous examples in the [UnitTest](https://github.com/dahall/Vanara/tree/master/UnitTests) folder.
