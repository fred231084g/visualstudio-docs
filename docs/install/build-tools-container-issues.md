---
title: Known issues for containers
description: Learn more about the known issues that might occur when you install Visual Studio Build Tools into a Windows container.
ms.date: 02/18/2020
ms.topic: conceptual
ms.assetid: 140083f1-05bc-4014-949e-fb5802397c7a
author: anandmeg
ms.author: meghaanand
manager: jmartens
ms.workload:
- multiple
ms.prod: visual-studio-windows
ms.technology: vs-installation
---
# Known issues for containers

 [!INCLUDE [Visual Studio](~/includes/applies-to-version/vs-windows-only.md)]

There are a few issues when installing Visual Studio into a Docker container.

## Windows container

The following known issues occur when you install Visual Studio Build Tools into a Windows container.

* Pass `-m 2GB` (or more) when building the image. Some workloads require more memory than the default 1 GB when installed.
* Configure Docker to use disks larger than the default 20 GB.
* Pass `--norestart` on the command line. As of this writing, attempting to restart a Windows container from within the container returns `ERROR_TOO_MANY_OPEN_FILES` to the host.
* If you base your image directly on mcr.microsoft.com/windows/servercore, the .NET Framework might not install properly and no install error is indicated. Managed code might not run after the install is complete. Instead, base your image on [microsoft/dotnet-framework:4.7.1](https://hub.docker.com/r/microsoft/dotnet-framework) or later. As an example, you might see an error when building with MSBuild that's similar to the following:

  > C:\BuildTools\MSBuild\15.0\bin\Roslyn\Microsoft.CSharp.Core.targets(84,5): error MSB6003: The specified task executable "csc.exe" could not be run. Could not load file or assembly 'System.IO.FileSystem, Version=4.0.1.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The system cannot find the file specified.

## Build Tools container

The following known issues might occur when you use a Build Tools container. To see whether issues have been fixed or if there are other known issues, visit [Developer Community](https://aka.ms/feedback/suggest?space=8).

* IntelliTrace might not work in [some scenarios](https://github.com/Microsoft/vstest/issues/940) within a container.
* On older versions of Docker for Windows, the default container image size is only 20 GB and will not fit Build Tools. Follow [instructions to change image size](/virtualization/windowscontainers/manage-containers/container-storage#storage-limits) to 127 GB or more.
To confirm a disk space issue, check the log files for more information. Your `vslogs\dd_setup_<timestamp>_errors.log` file will include the following if you run out of disk space: 
```
Pre-check verification: Visual Studio needs at least 91.99 GB of disk space. Try to free up space on C:\ or change your target drive.
Pre-check verification failed with error(s) :  SizePreCheckEvaluator.
```
[!INCLUDE[install_get_support_md](includes/install_get_support_md.md)]

## See also

* [Install Build Tools into a Container](build-tools-container.md)
* [Advanced Example for Containers](advanced-build-tools-container.md)
* [Visual Studio Build Tools workload and component IDs](workload-component-id-vs-build-tools.md)
