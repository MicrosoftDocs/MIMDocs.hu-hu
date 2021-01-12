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
ms.openlocfilehash: fb38ee26d829acd5c54bf690f7a80f0659baaa85
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104071"
---
# <a name="mim-pam-test-lab-environment-overview"></a>A betekintési szolgáltatás PAM-tesztkörnyezet környezetének áttekintése

> [!NOTE]
> A felügyeleti csomag PAM-alapú megközelítése olyan elszigetelt környezetek egyéni architektúrájában használható, ahol az Internet-hozzáférés nem érhető el, ahol ez a konfiguráció szükséges a szabályozáshoz, vagy nagy hatással van az elkülönített környezetekre, például az offline kutatási laboratóriumokra és a leválasztott operatív technológiákra, illetve a felügyeleti és adatgyűjtési környezetekre. Ha a Active Directory egy internetkapcsolattal rendelkező környezet részét képezi, további információért lásd a [privilegizált hozzáférés biztonságossá](/security/compass/overview) tételét ismertető témakört.

A következővel állíthatja be a szoftvert a virtuális gépekre.
A Privileged Access Management olyan virtuális gépekkel (VM) működik, amelyek megosztott hálózaton keresztül kapcsolódnak egymáshoz, és külön meghajtókkal rendelkeznek. Ezek a virtuális gépek Windows 8.1, Windows Server 2012 R2 vagy más operációsrendszer-platformokon is működhetnek.

![PAM-kiszolgálók: kapcsolatok és támogatott platformok – diagram](media/pam-test-lab-architecture.png)

Legalább három virtuális gép szükséges.  Ha még nem rendelkezik a PAM által felügyelt AD-tartománnyal, egy további virtuális gépre van szüksége, amely CORP-tartományvezérlőként működik.  Ha magas rendelkezésre álláshoz szeretné konfigurálni a PRIV-szoftvert, két további virtuális gépre van szüksége.

A virtuális gépek lemezképeit tároló meghajtókon legalább 120 GB szabad lemezterület szükséges.  Ha magas rendelkezésre állású üzemelő példányt tervez létrehozni, győződjön meg róla, hogy a lemezalrendszer megfelel-e az SQL megosztott tárhelyre vonatkozó követelményeknek.  A megosztott tárolás történhet Windows Server feladatátvételi fürtszolgáltatási fürtlemezeken, tárolóhálózaton (SAN) lévő lemezeken vagy SMB-kiszolgálón található fájlmegosztások formájában.

> [!IMPORTANT]
> A tárterületet a megerősített környezetnek kell dedikáltnak lennie. A tárterület a megerősített környezeten kívüli egyéb munkaterhelésekkel való megosztása nem ajánlott, mivel ez veszélyeztetheti a megerősített környezet integritását.

## <a name="next-steps"></a>Következő lépések

- A [Active Directory tartományi szolgáltatások Privileged Access Management](privileged-identity-management-for-active-directory-domain-services.md) a PAM áttekintése és működése.
- [A PAM összetevőinek megismerése](principles-of-operation.md) a PAM különböző összetevőinek áttekintése.
