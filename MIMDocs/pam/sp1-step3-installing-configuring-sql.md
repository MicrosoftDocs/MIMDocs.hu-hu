---
title: "3. lépés: Az SQL konfigurálása"
description: "A CORP-tartomány előkészítése a Privileged Identity Manager által szkriptek útján kezelt meglévő vagy új identitásokkal"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: a7d456b1c2baf31ef2d7ca801a567cf42eaef52e


---
# 3. lépés: Az SQL konfigurálása

Mielőtt folytatná a következő lépésekkel, győződjön meg róla, hogy az SQL Server 2012 SP1 vagy újabb verziót, illetve az SQL Server 2014 verziót használja. Tartományhoz csatlakoztatott kiszolgálók esetében jelentkezzen be a MIMAdmin-fiók használatával, más esetben jelentkezzen be helyi rendszergazdaként.
1. A PowerShell futtatása rendszergazdaként
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. A 3. menüelem kiválasztása (SQL Server beállítása)

  Ha a kiszolgáló még nem lett csatlakoztatva egy PRIV-tartományhoz, kérni fogja a hitelesítő adatokat és a kiszolgáló csatlakoztatását a tartományhoz.
  A tartományhoz csatlakozás után a gép újraindul. Sikeres újraindítás után jelentkezzen be a kiszolgálón a MIMAdmin-fiókkal.

1. A PowerShell futtatása rendszergazdaként
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. A 3. menüelem kiválasztása (SQL Server beállítása)

Ha szükséges, adja meg a MIMAdmin-szolgáltatásfiókhoz tartozó jelszót, és engedélyezze a telepítés folytatását. Ha elkészült, folytassa a 4. lépéssel.



<!--HONumber=Sep16_HO4-->

