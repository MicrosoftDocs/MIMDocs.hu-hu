---
title: '3. lépés: Az SQL konfigurálása'
description: Ez a cikk a cikkek sorozatának 3. lépése, amely leírja, hogyan konfigurálhatja a Microsoft Identity Manager parancsfájlokkal, és ismerteti az SQL Server konfigurációs lépéseit.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: e4317363084da53e5683cd1a9b3d7bd0ce3ebcda
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010523"
---
# <a name="step-3-configuring-sql"></a>3. lépés: Az SQL konfigurálása

> [!div class="step-by-step"]
> [«2](sp1-step2-configuring-corp-domain.md) 
>  . lépés [4. lépés»](sp1-step4-configuring-sharepoint.md)

Mielőtt folytatná a következő lépésekkel, győződjön meg róla, hogy az SQL Server 2012 SP1 vagy újabb verziót, illetve az SQL Server 2014 verziót használja. A tartományhoz csatlakoztatott kiszolgálók esetében jelentkezzen be a MIMAdmin fiók használatával, vagy jelentkezzen be helyi rendszergazdaként.
1. A PowerShell futtatása rendszergazdaként
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. A 3. menüelem kiválasztása (SQL Server beállítása)

   Ha a kiszolgáló még nem lett csatlakoztatva egy PRIV-tartományhoz, kérni fogja a hitelesítő adatokat és a kiszolgáló csatlakoztatását a tartományhoz.
   A tartományhoz csatlakozás után a gép újraindul. Sikeres újraindítás után jelentkezzen be a kiszolgálóra a MIMAdmin fiókkal.

5. A PowerShell futtatása rendszergazdaként
6. cd $env:SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. A 3. menüelem kiválasztása (SQL Server beállítása)

Ha szükséges, adja meg a MIMAdmin-szolgáltatásfiókhoz tartozó jelszót, és engedélyezze a telepítés folytatását. Ha elkészült, folytassa a 4. lépéssel.

> [!div class="step-by-step"]
> [«2](sp1-step2-configuring-corp-domain.md) 
>  . lépés [4. lépés»](sp1-step4-configuring-sharepoint.md)
