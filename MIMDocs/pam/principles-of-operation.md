---
title: A PAM-összetevők ismertetése | Microsoft Docs
description: A Privileged Access Management rendelkezik saját összetevőkkel is, de bizonyos összetevői megegyeznek a MIM összetevőivel. Ismerje meg, ezek hogyan működnek együtt.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e3e248092f674a031e255eb368681ab1b6dc21df
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010693"
---
# <a name="understand-the-components-of-mim-pam"></a>A betekintő gyűjtemény összetevőinek megismerése

Privileged Access Management a rendszergazdai hozzáférést külön erdő használatával a napi felhasználói fiókoktól elkülönítve tartja.

> [!NOTE]
> A felügyeleti csomag PAM-alapú megközelítése olyan elszigetelt környezetek egyéni architektúrájában használható, ahol az Internet-hozzáférés nem érhető el, ahol ez a konfiguráció szükséges a szabályozáshoz, vagy nagy hatással van az elkülönített környezetekre, például az offline kutatási laboratóriumokra és a leválasztott operatív technológiákra, illetve a felügyeleti és adatgyűjtési környezetekre. Ha a Active Directory egy internetkapcsolattal rendelkező környezet részét képezi, további információért lásd: a [privilegizált hozzáférés biztonságossá tétele](/security/compass/overview) .

 Ez a megoldás párhuzamos erdőkre támaszkodik:

- *CORP*: Ez az általános célú vállalati erdő, amely egy vagy több tartományt tartalmaz. Előfordulhat, hogy vállalata több erdővel rendelkezik, de a cikkben szereplő példákban az egyszerűség kedvéért egyetlen erdőt és ezen belül egyetlen tartományt feltételeztünk.  
- *PRIV*: Dedikált erdő, kifejezetten ehhez a PAM-forgatókönyvhöz létrehozva. Ez az erdő egyetlen tartományt tartalmaz, amely az egy vagy több CORP tartományból származó rendszerjogosultságú csoportokat és fiókokat kezeli.

A PAM-hez konfigurált MIM megoldás a következő összetevőket tartalmazza:  

- **MIM szolgáltatás**: biztosítja az identitás- és hozzáférés-kezelési feladatok végrehajtásához szükséges üzleti logikát, beleértve a rendszerjogosultságú fiókok felügyeletét és az emelt szintű jogosultsági kérelmek kezelését.
- Webalkalmazás- **portál**: SharePoint 2013 vagy újabb rendszer által üzemeltetett SharePoint-alapú portál, amely rendszergazdai felügyeleti és konfigurációs felhasználói felületet biztosít.
- A **Rendszerkiszolgálói szolgáltatás adatbázisa**: SQL Server 2012-es vagy újabb verzióban tárolva, és a rendszerazonosító adatok és a szükséges meta-adatok tárolására szolgál.
- **PAM figyelőszolgáltatás** és **PAM összetevő-szolgáltatás**: két olyan szolgáltatás, amely kezeli a rendszerjogosultságú fiókok életciklusát, és segíti a PRIV AD működését a csoporttagság életciklusa alatt.
- **PowerShell-parancsmagok**: a MIM szolgáltatás és a PRIV AD CORP erdőbeli PAM-rendszergazdákhoz és rendszergazdai fiókokhoz a szükséges időben történő (JIT) hozzáférési jogosultságot kérelmező végfelhasználókhoz tartozó felhasználókkal és csoportokkal való feltöltésére szolgálnak.
- **PAM REST API és mintaportál**: a MIM megoldást a PAM-forgatókönyvbe integráló, jogosultságszint-emelést kérelmező egyéni ügyfelekkel rendelkező fejlesztők részére készült összetevők, melyekhez a PowerShell vagy a SOAP használata nincs szükség. A REST API használatát egy minta webalkalmazás mutatja be.

A telepítés és a konfigurálás után a PRIV erdőben található áttelepítési eljárás által létrehozott minden csoport egy idegen elsődleges csoport, amely a csoportot az eredeti CORP erdőben tükrözi. A Foreign Principal csoport azokat a felhasználókat biztosítja, akik az adott csoport tagjai a Kerberos-tokenben lévő csoportnak, a CORP erdőben lévő csoport SID-je. Mindemellett amikor a MIM szolgáltatás tagokat vesz fel ezekbe a csoportokba a PRIV erdőben, az adott tagságok időkorlátosak lesznek.

Emiatt, amikor egy felhasználó a PowerShell-parancsmagok használatával jogosultságszint-emelést kér, és a kérelmet jóváhagyja a rendszer, a MIM szolgáltatás a felhasználó fiókját hozzáadja a PRIV erdő egyik csoportjához. Amikor a felhasználó bejelentkezik az emelt jogosultságú fiókkal, a felhasználó Kerberos-jogkivonata tartalmazni fog egy biztonsági azonosítót (SID), mely megegyezik a CORP erdőbeli csoport biztonsági azonosítójával. Mivel a CORP és a PRIV erdő között megbízhatósági kapcsolat van beállítva, a CORP erdőben lévő erőforrások eléréséhez használt rendszerjogosultságú fiók – a Kerberos-csoporttagságot ellenőrző erőforrás szempontjából – az adott erőforrás biztonsági csoportjának tagjává válik. Ezt a Kerberos erdők közötti hitelesítése biztosítja.

Mindemellett ezek a tagságok időkorlátosak, így az előre beállított idő elteltével a felhasználó rendszergazdai fiókja már nem lesz tagja a továbbiakban a PRIV erdőbeli csoportnak. Ennek eredményeképpen a fiók nem használható többé a további erőforrások eléréséhez.
