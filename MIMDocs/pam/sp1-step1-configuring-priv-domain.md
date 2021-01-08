---
title: 1. lépés a PRIV-tartomány konfigurálása
description: A PRIV-tartomány előkészítése meglévő vagy új identitásokkal a Microsoft Identity Manager által felügyelt parancsfájlok használatával
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
ms.openlocfilehash: a4d7c8ed4f5d7a9d6b4653caf03de69f1d18e509
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010625"
---
# <a name="step-1-configuring-the-priv-domain"></a>1. lépés a PRIV-tartomány konfigurálása

> [!div class="step-by-step"]
> [2. lépés»](sp1-step2-configuring-corp-domain.md)

1. Jelentkezzen be a PRIVDC rendszergazdaként
   * Ha ez a PAM-telepítés PRIV-Only-környezet, jelentkezzen be a CORPDC
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Az 1. menüelem kiválasztása (PRIV-erdő konfigurálása)


Az SQL, a SharePoint és a felügyeleti webszolgáltatások kezeléséhez szükséges szolgáltatásfiókok automatikusan létrejönnek, ha még nincsenek jelen a tartományban. A szkript futása során a szolgáltatásfiókok létrehozásához meg kell adnia a szükséges jelszavakat.

Ha a PRIV tartomány Windows Server 2016, a Windows Server 2016-re beállított működési szinttel, a parancsfájl kérni fogja, hogy engedélyezze a PAM által igényelt választható Active Directory "Privileged Access Management funkciót". A folytatáshoz hagyja jóvá az „Igen” lehetőséget. A Windows Server 2016 alatti működési szintek esetén zárja be a figyelmeztetést, amely arról tájékoztatja, hogy a további konfigurációk nem lesznek végrehajtva. Újra kell futtatnia a PAMDeployment.ps1 és a PAM erdő konfigurációját, ha a rendszergazda a Windows Server 2016 rendszerre emeli a funkcionális szintet.

>[!NOTE]
>A következő lépések nem szükségesek a PRIVOnly-konfigurációkhoz

7. Másolja az $env:SYSTEMDRIVE\PAM helyen létrehozott SIDs.txt fájlt az ugyanilyen mappába a CORPDC tartományvezérlőn.
   * Szüksége lesz erre a biztonsági azonosítók listájára a CORPDC, hogy az engedélyek beállításához egy későbbi lépésben a PRIV-felhasználók olvasni tudják a CORP-felhasználó tulajdonságait.
8. A szkript a futását befejezően arra kéri, hogy a módosítások hatályba léptetéséhez indítsa újra a gépet.

> [!div class="step-by-step"]
> [2. lépés»](sp1-step2-configuring-corp-domain.md)
