---
title: A PAM-környezet áttekintése | Microsoft Docs
description: A virtuális gépek száma és konfigurációs adatai, amelyek a Privileged Access Management sikeres üzembe helyezéséhez szükségesek
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ca306e7521bbdc2200663889becd91b1ae952d5a
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010659"
---
# <a name="mim-pam-test-lab-environment-overview"></a>A betekintési szolgáltatás PAM-tesztkörnyezet környezetének áttekintése

A következővel állíthatja be a szoftvert a virtuális gépekre.
A Privileged Access Management olyan virtuális gépekkel (VM) működik, amelyek megosztott hálózaton keresztül kapcsolódnak egymáshoz, és külön meghajtókkal rendelkeznek. Ezek a virtuális gépek Windows 8.1, Windows Server 2012 R2 vagy más operációsrendszer-platformokon is működhetnek.

![PAM-kiszolgálók: kapcsolatok és támogatott platformok – diagram](media/pam-test-lab-architecture.png)

Legalább három virtuális gép szükséges.  Ha még nem rendelkezik a PAM által felügyelt AD-tartománnyal, egy további virtuális gépre van szüksége, amely CORP-tartományvezérlőként működik.  Ha magas rendelkezésre álláshoz szeretné konfigurálni a PRIV-szoftvert, két további virtuális gépre van szüksége.

A virtuális gépek lemezképeit tároló meghajtókon legalább 120 GB szabad lemezterület szükséges.  Ha magas rendelkezésre állású üzemelő példányt tervez létrehozni, győződjön meg róla, hogy a lemezalrendszer megfelel-e az SQL megosztott tárhelyre vonatkozó követelményeknek.  A megosztott tárolás történhet Windows Server feladatátvételi fürtszolgáltatási fürtlemezeken, tárolóhálózaton (SAN) lévő lemezeken vagy SMB-kiszolgálón található fájlmegosztások formájában.

> [!IMPORTANT]
> A tárterületet a megerősített környezetnek kell dedikáltnak lennie. A tárterület a megerősített környezeten kívüli egyéb munkaterhelésekkel való megosztása nem ajánlott, mivel ez veszélyeztetheti a megerősített környezet integritását.

## <a name="next-steps"></a>További lépések

- A [Active Directory tartományi szolgáltatások Privileged Access Management](privileged-identity-management-for-active-directory-domain-services.md) a PAM áttekintése és működése.
- [A PAM összetevőinek megismerése](principles-of-operation.md) a PAM különböző összetevőinek áttekintése.
