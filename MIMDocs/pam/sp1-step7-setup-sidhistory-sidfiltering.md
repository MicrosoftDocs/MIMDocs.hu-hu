---
title: '7. lépés: SID-előzmények/SID-szűrés beállítása'
description: A Microsoft Identity Manager konfigurálásának 7. lépése parancsfájlok használatával. Ehhez a lépéshez a SID-előzmények/SID-szűrés beállítása tartozik.
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
ms.openlocfilehash: c09649bbecfb4608391fef1cda3d8da87ded888b
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010642"
---
# <a name="step-7-setup-sid-historysid-filtering"></a>7. lépés: SID-előzmények/SID-szűrés beállítása

> [!div class="step-by-step"]
> [«6](sp1-step6-setup-pam-trust.md) 
>  . lépés [8. lépés»](sp1-step8-pam-deployment-verification.md)

**Az alábbi parancsok nem szükségesek csak a priv-környezetekhez** Jelentkezzen be a PAMServer a MIMAdmin-fiókkal.

1. Jelentkezzen be a CORP TARTOMÁNYVEZÉRLŐre rendszergazdaként
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. A 8. menüelem kiválasztása (SID-előzmények/SID-szűrés beállítása)

A sikeres végrehajtást követően a következő üzenetek láthatóak:<br/></br>
SID-szűrés esetén: <br/></br>
„Megbízhatósági kapcsolat beállítása a biztonsági azonosítók szűrésének mellőzésére” vagy „A biztonsági azonosítók szűrése nem engedélyezett ebben a megbízhatósági kapcsolatban”. </br></br>
SID-előzményekhez: </br></br>
„A biztonsági azonosítók előzményeinek engedélyezése ebben a megbízhatósági kapcsolatban” vagy „A biztonsági azonosítók előzményei már engedélyezettek ebben a megbízhatósági kapcsolatban”.

> [!div class="step-by-step"]
> [«6](sp1-step6-setup-pam-trust.md) 
>  . lépés [8. lépés»](sp1-step8-pam-deployment-verification.md)
