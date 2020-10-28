---
title: Web Services-összekötő importálása | Microsoft Docs
description: A webszolgáltatás-összekötő importálása Microsoft Identity Manager több webszolgáltatási konfigurációval.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/19/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: ac4ee4f7b6d9f1f70198b79e628737b64b4cb3a0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760541"
---
# <a name="import-web-services-connector"></a>Web Services-összekötő importálása

Ha a webszolgáltatások felügyeleti ügynökét (MA) tartalmazó importálási kiszolgáló konfigurációs művelete a "nem sikerült lekérdezni a bővítmény konfigurációs paramétereit" hibaüzenettel zárul be, akkor az exportált konfigurációs fájlokat be kell jelölni.

A hiba oka a szinkronizálási szolgáltatás korlátozása. A szolgáltatás nem tudja importálni a kiszolgálói konfigurációt az MA webszolgáltatásokkal, és a "nem sikerült beolvasni a konfigurációs paramétereket a bővítményből" hibaüzenetet adja vissza. Bizonyos esetekben a webszolgáltatások MA nem tartalmazhatnak olyan érvénytelen beállításokat, amelyeket meg kell oldani. 

## <a name="how-to-fix-the-issue"></a>Útmutató a probléma megoldásához

1. Másolja a kiszolgáló konfigurációs fájljait a célkiszolgálóra. 

2. A célkiszolgálón másolja a <a href="#web-service-powershell-script">WebServiceMA_apply_before_import_server_configuration.ps1</a> PowerShell-parancsfájlt abba a mappába, amely a kiszolgálói konfiguráció exportált fájljait tartalmazza.

3. Futtassa a WebServiceMA_apply_before_import_server_configuration.ps1 PowerShell-parancsfájlt.

<h3 id="web-service-powershell-script">PowerShell-parancsprogram</h3>

```
# --------------------------------------------------------------------- 
# <copyright file="WebServiceMA_apply_before_import_server_configuration.ps1" company="Microsoft"> 
# Copyright (c). All rights reserved. 
# </copyright> 

# --------------------------------------------------------------------- 
# If the script doesn't work, check the ExecutionPolicy
# by using the following commands: 
# 
#    - Get-ExecutionPolicy 
#    - Set-ExecutionPolicy RemoteSigned 
#

function Get-MIMInstallationPath { 

<# 
  .SYNOPSIS 

  Get the installation path of MIM from the Windows registry.

  .DESCRIPTION 

  Get the installation path of MIM from the Windows registry: 

       HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\
       Forefront Identity Manager\2010\Synchronization Service\ 

  and the paramter Location. 

  .EXAMPLE 

  $path = Get-MIMInstallationPath 
#>   

  try 
  { 
    $MIMpath = Get-ItemProperty -Path "Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Forefront Identity Manager\2010\Synchronization Service\" 
  } 

  catch 
  { 
    $ErrorMessage = $_.Exception.Message 
    throw "Get-MIMInstallationPath exception: $ErrorMessage" 
  } 

  Write-Output "$($MIMpath.Location)Synchronization Service\Extensions\" 

} 

function Get-WsconfigFilesNamesFromExtensionsFolder([string] $path) { 

<# 
  .SYNOPSIS 

  Get the comma-separated list of *.wsconfig files from Extensions folder. 

  .DESCRIPTION 

  Get the comma-separated list of *.wsconfig files from Extensions folder. 

  .PARAMETER path 

  The path to the Extensions folder that contains the *.wsconfig files. 

  .EXAMPLE 

  $wsconfigFiles = Get-WsconfigFilesNamesFromExtensionsFolder("./") 
#>  

 if( [string]::IsNullOrEmpty($path) ) 

  { 
    throw "Get-WsconfigFilesNamesFromExtensionsFolder error: the input variable Path cannot be empty." 
  } 

  try 
  { 
    $files = Get-ChildItem $path -filter "*.wsconfig" -name | ForEach-Object {$_ -replace ".wsconfig", ""} 

    $wsconfigFiles=$files -join "," 
  } 

  catch 
  { 
    $ErrorMessage = $_.Exception.Message 

    Write-Host "Get-WsconfigFilesNamesFromExtensionsFolder exception: $ErrorMessage" 
    Write-Output "" 
  } 

  Write-Output $wsconfigFiles 

} 

function Set-AvailableWebServiceProjects([string] $path) { 

<# 

  .SYNOPSIS 

  Patch exported wsconfig files for Web Service project management agent. 


  .DESCRIPTION 

  The script tries to find the config files of Web Service management agents and tries to fix Web Service project configuration parameter. 

  .PARAMETER path 

  The path to the folder that contains the exported config files of management agents.

  .EXAMPLE 

  $count = Set-AvailableWebServiceProjects("./") 

#>   

  if( [string]::IsNullOrEmpty($path) ) 

  { 
    throw "Set-AvailableWebServiceProjects: path variable is empty" 
  } 

  try 
  { 
    $countPatchedFiles=0 

    $files = Get-ChildItem $path -filter "MA-{*}.xml" -name 

    foreach ($file in $files)  
    { 
      [xml]$xmlConfig = Get-Content $file 

      $connectorName = $xmlConfig."saved-ma-configuration"."ma-data"."ma-listname" 

      if($connectorName -ne "Web Service (Microsoft)" ) 
      { 
        continue 
      } 

      $parameters = $xmlConfig."saved-ma-configuration"."ma-data"."private-configuration"."MAConfig"."parameter-definitions" 
     $target = ($parameters."parameter"|where {$_.name -eq "Web Service project" -and $_.use -eq "connectivity" -and $_.type -eq "drop-down"})          

      if($target -eq $null) 
      { 
        continue 

      } 

     $extensionsFolderPath = Get-MIMInstallationPath 
     $wsconfigFiles = Get-WsconfigFilesNamesFromExtensionsFolder($extensionsFolderPath)

     if($wsconfigFiles -eq '') 
     { 
       continue 
     } 

     $target.validation = [string]$wsconfigFiles   
     $defaultConfig = $wsconfigFiles.Split("{,}") 
     $target."default-value" = $defaultConfig[0] 
     $fileFullPath = "$path$file" 
     Set-ItemProperty $fileFullPath -name IsReadOnly -value $false 
     $xmlConfig.Save($fileFullPath) 
     Set-ItemProperty $fileFullPath -name IsReadOnly -value $true 
     $countPatchedFiles++ 
   } 
  } 

  catch 
  { 
   $ErrorMessage = $_.Exception.Message 

   throw "Set-AvailableWebServiceProjects exception: $ErrorMessage" 
  } 
  Write-Output $countPatchedFiles 
} 

########################################## 
####              Main                #### 
########################################## 

cls 

try 
{ 
  $currentFolder = (Get-Location).path + "\" 

  $countPatchedFiles = Set-AvailableWebServiceProjects($currentFolder) 

  Write-Host "Count of patched Web Service config files:" $countPatchedFiles 
} 
catch 
{ 
  $ErrorMessage = $_.Exception.Message 
  Write-Host "The script was run with excetion: $ErrorMessage" 
} 

[void](Read-Host 'Press Enter to exit…') 
```

## <a name="next-steps"></a>Következő lépések

- [A webszolgáltatás-konfigurációs eszköz telepítése](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP telepítési útmutató](microsoft-identity-manager-2016-ma-ws-soap.md)
- [REST telepítési útmutató](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Webszolgáltatás MA – konfiguráció](microsoft-identity-manager-2016-ma-ws-maconfig.md)
