---
title: '5. lépés: PAM telepítése/konfigurálása'
description: Ez az Microsoft Identity Manager a parancsfájlok használatával történő konfigurálásának 5. lépése, amely a PAM-kiszolgálón végrehajtott telepítési lépéseket ismerteti.
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
ms.openlocfilehash: 01fe9cd7704674f408e0b9b5a27673989d0eaecf
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010590"
---
# <a name="step-5-installingconfiguring-pam"></a>5. lépés: PAM telepítése/konfigurálása

> [!div class="step-by-step"]
> [«4](sp1-step4-configuring-sharepoint.md) 
>  . lépés [6. lépés»](sp1-step6-setup-pam-trust.md)

Tartományhoz csatlakoztatott PAMServer esetén jelentkezzen be MIMAdminként, más esetben helyi rendszergazdaként.
1. A PowerShell futtatása rendszergazdaként
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. Az 5. menüelem kiválasztása (MIM PAM beállítása)

>[!NOTE]
>Ha a gép még nincs PRIV-tartományhoz csatlakoztatva, kérni fogja a hitelesítő adatokat. A tartományhoz csatlakozás után a gép újraindul.

Miután a PAMServer újraindult, jelentkezzen be a gépre a MIMAdmin-fiókkal.

1. A PowerShell futtatása rendszergazdaként
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Az 5. menüelem kiválasztása (MIM PAM beállítása)

   Ha a rendszer kéri, írja be a MIM Monitor-fiókhoz, MIM-összetevőfiókhoz, MIM MA-fiókhoz, MIM-szolgáltatásfiókhoz, MIM-rendszergazdafiókhoz és a SharePoint-fiókhoz tartozó jelszót.
   A telepítés befejezése után a gép újraindul.

> [!div class="step-by-step"]
> [«4](sp1-step4-configuring-sharepoint.md) 
>  . lépés [6. lépés»](sp1-step6-setup-pam-trust.md)
