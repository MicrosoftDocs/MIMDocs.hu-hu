---
title: A rendszer elavult funkcióit és a jövőbeli tervezést is megtervezte | Microsoft Docs
description: 'Ez a cikk a következő dokumentumok elavult funkcióit dokumentálja: a faazonosító Identity Manager 2016 SP2.'
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 7/28/2020
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: cb0159ec70e161dc47140712bd2ac039786e034d
ms.sourcegitcommit: 50cee02a7146445bd6fa361349099c7b294792d8
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/30/2020
ms.locfileid: "87443546"
---
# <a name="deprecated-features"></a>Elavult funkciók

Ez a cikk a Microsoft Identity Manager 2016 SP2 elavult funkcióit ismerteti. Ha a szolgáltatás továbbra is szerepel a Microsoft Identity Managerban, továbbra is támogatott. A funkciók nem ajánlottak új központi telepítések esetén, mivel azok a jövőbeli gyorsjavításokban vagy szervizcsomagokban is eltávolíthatók.  A fejlesztők számára azt javasoljuk, hogy az elavult funkciókat ne használja bármely új alkalmazásban vagy megoldásban.

> [!NOTE]
>
> További információ a webszolgáltatások új támogatási lehetőségeiről: [prémium szintű Azure ad ügyfelek támogatási lehetőségei](support-update-for-azure-active-directory-premium-customers.md).

## <a name="bhold"></a>BHOLD

A Microsoft nem javasolja, hogy az ügyfelek új központi telepítéseket indítsanak a Microsoft BHOLD Suite-összetevőkből. A BHOLD meglévő telepítései továbbra is támogatottak lesznek. Az Azure AD olyan [hozzáférési felülvizsgálatokat tesz elérhetővé](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), amelyek felváltják a BHOLD-igazolási kampány funkcióit és a jogosultságok kezelését, amely felváltja a hozzáférés-hozzárendelési funkciókat.

## <a name="service-and-portal"></a>Szolgáltatás és portál

| **Kategória**                | **Elavult funkció**              | **Megjegyzés**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| A szinkronizálás programozott konfigurációja | Webszolgáltatás konfigurációs felülete (ma-az adathalmaz és az MV-adatforrások) | Előfordulhat, hogy egy jövőbeli gyorsjavításban vagy szervizcsomagban el lehet távolítani a következő lehetőséggel, hogy konfigurálható legyen a fakiszolgáló-szinkronizálási szolgáltatás.
|

## <a name="synchronization-service"></a>Synchronization Service 

A következő MAs el lett távolítva a 2016-es webszolgáltatásban: </br> 1. MA a FIM-tanúsítványok kezeléséhez </br>2. MA Lotus-megjegyzésekhez</br> 3. MA az SAP R/3 esetében </br> A Lotus Notes és az SAP R/3 MAs új összekötővel lett lecserélve. További információ: a [legújabb Connector verziójának korábbi kiadásainak & letöltése](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history).

Az ECMA1/XMA bővíthetőségi keretrendszert a ECMA 2,0 váltotta fel. A meglévő ECMA1-felügyeleti ügynökök ECMA 2.0-összekötővel történő frissítése kötelező.

| **Kategória**                | **Elavult funkció**              | **Megjegyzés**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Felügyeleti ügynökök           | A folyamaton kívüli összekötők futtatása      | A szinkronizációs szolgáltatás mindig ugyanabban a folyamatban hívja meg az összekötőt. Az összekötő feladata a másik folyamat elindítása és kezelése. |
| Felügyeleti ügynökök           | Partíció megjelenítendő nevének konfigurálása    | Ez a beállítás csak a partíciók alternatív nevének megadására szolgál a WMI-felületeken.                                                                                                                                                                       |
| Futtatási profilok                | Egyesített profilok                   | Az egyesített profilok az importálás/szinkronizálás, a teljes importálási/különbözeti szinkronizálás és a teljes importálás/szinkronizálás eltávolítására is használhatók. Használja helyette a futtatási profilokat két lépéssel.

> [!NOTE]
> A kombinált futtatási profilokat csak olyan környezetekben érdemes megőrizni, ahol a teljesítményt nagy számú meglévő leválasztó érinti.

| **Kategória**                | **Elavult funkció**              | **Megjegyzés**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Attribútumsorrend | Többszörös fölény/egyenlő elsőbbség                       | Az egyenlő elsőbbséget el lehet távolítani. Ehelyett manuális prioritást kell beállítania. Továbbra is használhatja ezt a funkciót, ha a környezetében telepítve van egy FIM Service Management-ügynök. Ez a felügyeleti ügynök nem biztosít manuális elsőbbséget a deklaratív kiépítés nem precedens nélküli exportálásának elkerüléséhez. |
| Csatlakozási szabályok           | Csatlakozás "any" típusú objektumhoz                             | Minden csatlakozási szabálynak explicit módon meg kell határoznia azt a metaverse-objektumtípust, amelyhez csatlakozni próbálnak.       |
| Attribútumok folyamatai      | A "null értékek engedélyezése" jelölőnégyzet kihagyása az exportált értékeknél            | A "nullák engedélyezése" mindig ki lesz választva, ezért győződjön meg arról, hogy a jelenlegi környezetben a "nullák engedélyezése" beállítás van kiválasztva.  |
| Attribútumok folyamatai      | "Attribútumok felidézése"                            | A rendszer mindig visszahívja az attribútumokat, ami az ajánlott eljárás.  |
| Szabályok bővítmény      | A metaverse és a ma Rules bővítmény futtatása a folyamaton kívül | A metaverse és az attribútum flow szabályai ugyanabban a folyamatban futnak, mint a szinkronizálási motor.       |
| Szabályok bővítmény      | Tranzakció tulajdonságai                                | Kerülje a bejövő, a kiépítés és a kimenő szinkronizálás közötti adattovábbítást ezzel a segédprogram-osztállyal.  |
| Szabályok bővítmény      | ExchangeUtils: Create55 \* metódusok                     | Előfordulhat, hogy az Exchange 5,5-kiszolgálókhoz tartozó objektumok létrehozására szolgáló metódusok el lesznek távolítva.        |
| Interfész            | Mms_Metaverse                                        | Előfordulhat, hogy a ClmUtils összes tagja el lesz távolítva egy jövőbeli gyorsjavításban vagy szervizcsomagban.   |

## <a name="next-steps"></a>További lépések
További információk:

- A Microsoft Identity Manager számos közös vonást mutat elődjével, a Forefront Identity Managerrel. Ha még FIM-et használ, vagy ha további dokumentációra van szüksége, tekintse meg a [FIM 2010 R2 dokumentáció - áttekintés](https://technet.microsoft.com/library/jj133885.aspx) című oldalt.
- A webszolgáltatások [üzembe helyezésének topológiái szempontjai](topology-considerations.md) Ez a cikk több üzembe helyezési topológiát mutat be.
- [Kapacitás-tervezési útmutató](capacity-planning-guide.md) Ez az útmutató a tesztelési környezetekkel együtt az üzemelő példány megfelelő hatókörének megismerésére használható.

