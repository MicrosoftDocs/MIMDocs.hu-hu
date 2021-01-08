---
title: Rétegmodell PAM-környezetben | Microsoft Docs
description: Információk a rétegmodellről, amely a biztonsági kockázat alapján különíti el a rendszert.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 25bf28731f14331da386ae9f59b041c8c9ee33a1
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010574"
---
# <a name="tier-model-for-partitioning-administrative-privileges"></a>A rendszergazdai jogosultságok felosztásának rétegmodellje

Ebben a cikkben egy olyan biztonsági modellel ismerkedhet meg, amely a jogok kiterjesztéséből adódó támadásokkal szembeni védelmet a magas szintű jogosultságokat igénylő tevékenységek és a nagy kockázatú zónák elkülönítésével biztosítja.

> [!IMPORTANT]
> A jelen cikkben szereplő modell kizárólag az elkülönített Active Directory környezetekhez készült, a faalkalmazási PAM használatával.  Hibrid környezetek esetén tekintse meg a [vállalati hozzáférési modell](/security/compass/privileged-access-access-model)útmutatását.

## <a name="elevation-of-privilege-in-active-directory-forests"></a>A jogok kiterjesztése az Active Directory-erdőkben

A Windows Server Active Directory- (AD-) erdőkhöz állandó rendszergazdai jogosultságokkal rendelkező felhasználók, szolgáltatások és alkalmazások fiókjai komoly kockázatot jelentenek a szervezet céljaira és üzleti tevékenységeire nézve. Ezeket a fiókokat gyakran a támadók célozzák meg, mert ha vannak ilyenek, a támadó jogosult a tartományban lévő más kiszolgálókhoz vagy alkalmazásokhoz való kapcsolódásra.

A rétegmodell annak alapján választja szét egymástól a rendszergazdákat, hogy milyen erőforrásokat kezelnek. A felhasználói munkaállomások felett irányítással rendelkező rendszergazdák elkülönülnek azoktól, akik alkalmazásokat vagy vállalati identitásokat kezelnek.

## <a name="restricting-credential-exposure-with-logon-restrictions"></a>Hitelesítő adatok veszélyeztetettségének korlátozása bejelentkezési korlátozásokkal

A rendszergazdai fiókokhoz tartozó hitelesítő adatok ellopásának kockázata általában a rendszergazdák munkamódszereinek átalakításával csökkenthető, ezáltal korlátozható a támadóknak való kiszolgáltatottság. Első lépésként az alábbi intézkedéseket javasoljuk a szervezeteknek:

- Korlátozzák azoknak a gépeknek a számát, amelyeken elérhetők a rendszergazdai hitelesítő adatok.
- Korlátozzák a szerepkörök jogosultságait a lehető legkevesebbre.
- Semmiképpen ne végezzenek rendszergazdai feladatokat olyan gazdagépeken, amelyeket hagyományos felhasználói műveletekre (például levelezésre vagy böngészésre) használnak.

A következő lépés a bejelentkezési korlátozások megvalósítása, illetve a rétegmodell követelményeit kielégítő folyamatok és műveletek feltételeinek megteremtése. Ideális esetben a hitelesítő adatokhoz való hozzáférést is le kell csökkenteni az egyes rétegeken belül az adott szerepkörhöz szükséges legalacsonyabb jogosultsági szintre.

Bejelentkezési korlátozásokat kell érvényesíteni annak érdekében, hogy a kiemelt jogosultságokkal rendelkező fiókok ne férjenek hozzá a kevésbé biztonságos erőforrásokhoz. Például:

- A tartományi rendszergazdák (0. szint) nem tudnak bejelentkezni a vállalati kiszolgálókra (1. szint) és a normál felhasználói munkaállomásokra (2. szint).
- A kiszolgálói rendszergazdák (1. szint) nem tudnak bejelentkezni a normál felhasználói munkaállomásokra (2. szint).

>[!NOTE]
> A kiszolgálói rendszergazdák ne tartozzanak a tartományi rendszergazdák csoportjához. A tartományvezérlőkért és a vállalati kiszolgálókért felelő személyzetnek külön fiókokat kell biztosítani.

A bejelentkezési korlátozásokat a következőkkel lehet érvényesíteni:

- Csoportházirendek bejelentkezési jogmegadásának korlátozása, beleértve:
    - A számítógép hálózati elérésének megtagadása
    - Kötegelt feladatként való bejelentkezés megtagadása
    - Szolgáltatásként való bejelentkezés megtagadása
    - Helyi bejelentkezés megtagadása
    - Távoli asztali beállításokkal való bejelentkezés megtagadása  
- Hitelesítési házirendek és silók alkalmazása Windows Server 2012 vagy újabb rendszer használata esetén
- Szelektív hitelesítés használata, ha a fiók dedikált rendszergazdai erdőben található

## <a name="next-steps"></a>További lépések

- A következő, a [Megerősített környezet tervezése](planning-bastion-environment.md) című cikkben a Microsoft Identity Manager rendszergazdai fiókjainak létrehozásához szükséges dedikált felügyeleti erdőinek létrehozásáról olvashat.
- A [biztonságos eszközök](/security/compass/concept-azure-managed-workstation) dedikált operációs rendszert biztosítanak az internetes támadásokkal és a veszélyforrási vektorokkal védett bizalmas feladatokhoz.
