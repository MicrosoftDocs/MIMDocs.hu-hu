---
title: Licencelés és letöltések Microsoft Identity Manager | Microsoft Docs
description: Ez a cikk a licencelési Microsoft Identity Manager (2016), valamint a szoftver letöltésére szolgáló mutatókkal kapcsolatos módszereket ismerteti.
keywords: ''
author: markwahl-msft
ms.author: mwahl
manager: daveba
ms.date: 10/18/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: b7b0dd73e5a87f338dc8cd91e61ee6a19c84068a
ms.sourcegitcommit: d6178a67014d66d37056c13d10328ae03e3cd781
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970362"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Microsoft Identity Manager 2016 licencelés és letöltések

Ez a cikk a licencelési Microsoft Identity Manager (2016), valamint a szoftver letöltésére szolgáló mutatókkal kapcsolatos módszereket ismerteti.

## <a name="licensing-mim-for-your-organization"></a>A szervezet licencelése

A Microsoft Identity Manager 2016 licenc felhasználónként történik.  A licencelés részleteit a termék használati feltételei és a kapcsolódó dokumentumok tartalmazzák, amelyek a [licencelési feltételek](https://www.microsoft.com/licensing/product-licensing/products.aspx) lapról tölthetők le.

### <a name="licensing-for-azure-ad-premium-customers"></a>Licencelés prémium szintű Azure AD ügyfelek számára

A Microsoft Identity Manager 2016 prémium szintű Azure Active Directory (P1 és P2) része, amely Enterprise Mobility + Security részét képezi.

Prémium szintű Azure AD egy [Microsoft-nagyvállalati szerződés](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx), a [nyílt Mennyiségi licencprogram](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx)és a [Cloud Solution Providers](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) program segítségével érhető el. Az Azure és Microsoft 365 előfizetők prémium szintű Azure Active Directory P1 és P2 online is vásárolhatnak.  További információk: [Azure Active Directory díjszabása](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="mim-cals"></a>Web-ügyféllicencek

Ha nem rendelkezik prémium szintű Azure Active Directory-előfizetésekkel a felhasználók számára, és több, a szinkronizálást meghaladó, a Rendszerfelügyeleti webszolgáltatással kapcsolatos funkciót használ, akkor minden olyan felhasználóhoz [ügyféllicencre van szükség](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx) , amelynek identitását a felügyeleti pontban felügyeli. Ha azt szeretné, hogy a külső felhasználók – például az üzleti partnerek, a külső vállalkozók vagy az ügyfelek – számára is hozzáférhessenek a munkaterülethez, ügyféllicenceket vásárolhat a külső felhasználók számára, vagy külső összekötő (EC) licenceket is vásárolhat. Microsoft Identity Manager 2016 ügyféllicencek nem szükségesek azokhoz a felhasználókhoz, akiknek az identitása csak a Microsoft Identity Manager szinkronizációs szolgáltatásban van, és a rendszer nem felügyel semmilyen más felügyeleti szervizcsomag-összetevőben.

### <a name="licenses-for-platform-components"></a>Platform-összetevők licencei

Windows Server-licenc szükséges ahhoz, hogy a Microsoft Identity Manager 2016 kiszolgálói szoftverét Windows Server-bővítményként használhassa. Emellett a webszolgáltatások üzembe helyezéséhez SQL Server telepítés szükséges.  A Windows Server és a SQL Server licencek nem tartoznak a felügyeleti csomaghoz.

## <a name="obtaining-mim-software"></a>A webalkalmazási szoftverek beszerzése

Mielőtt megkezdené a webszolgáltatások új telepítését vagy egy korábbi verzióról történő frissítést, ellenőrizze, hogy rendelkezik-e a legújabb verziókkal.

Ha új telepítést indít, le kell töltenie az adott forgatókönyvhöz kapcsolódó egyes, a rendszerhez tartozó összes egyes rendszerelem-összetevő telepítési fájljait. Ezután töltse le a fájlok frissítéseit, majd töltse le a letöltőközpontból különálló letöltéseket tartalmazó további összetevőket.


| Használati eset | Összetevő | A forgatókönyvhöz szükséges? | DVD ISO-mappa neve | Megjegyzések |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Szinkronizálás| Szinkronizálási szolgáltatás (beleértve az AD-összekötőt) | Igen | `Synchronization Service` | |
| Szinkronizálás | PCNS | Nem | `Password Change Notification Service` |  A tartományvezérlőkön való telepítéshez |
| Szinkronizálás | Összekötők az LDAP, az SQL, a Web Services, a PowerShell, a Lotus Domino, a Graph | Nem | n.a. | Terjesztés a letöltőközpont használatával |
| az emelt szintű hozzáférések felügyeletével | MIM szolgáltatás | Igen | `Service and Portal` | |
| Önkiszolgálás | A fakiszolgálói portál | Igen | `Service and Portal` | |
| Önkiszolgálás | Beépülő modulok és bővítmények | Nem | `Add-ins and extensions` | A végfelhasználói számítógépekre történő telepítéshez |
| Önkiszolgálás | SCSM-jelentéskészítés | Nem | `Data Warehouse Support Scripts` | |
| Önkiszolgálás | Hibrid jelentéskészítő ügynök | Nem | n.a. | Terjesztés a letöltőközpont használatával |
| Önkiszolgálás | Nyelvi csomagok | Nem | `LANGUAGE Packs` | |
| Tanúsítványkezelés | CM | Igen | `Certificate Management` | |
| Tanúsítványkezelés | CM tömeges ügyfél | Nem | `CM Bulk Client` | |
| Tanúsítványkezelés | CM-ügyfél | Nem | `CM Client`  | |
| Tanúsítványkezelés | Windows CM-alkalmazás | Nem | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Windows Installer-csomagok beszerzése

Az új telepítéshez a legtöbb szervezet a [mennyiségi licencelési szolgáltatási központból](https://www.microsoft.com/licensing/servicecenter/default.aspx)tölti le a beépítési csomagokat. 


A DVD ISO-fájl egyetlen mappát tartalmaz az egyes címtárszolgáltatás-összetevőkhöz: szinkronizációs szolgáltatás, szolgáltatás és portál stb. Ha a szoftvert egy másik számítógépre fogja telepíteni, amelyről letöltötte, akkor a teljes ISO-fájlt vagy az összetevő mappáját másolja át: ne csak egy MSI-fájlt helyezzen el egy mappából a többi fájl és almappa nélkül.

Ha nem rendelkezik hozzáféréssel a mennyiségi licencelési szolgáltatás központjához, akkor a megfelelő fejlesztői előfizetéssel rendelkező ügyfelek a [Visual Studio által nyújtott előnyök](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202&pgroup=)letöltésével ISO-fájlként tölthetik le a Microsoft webhelyének 2016 SP2-es verzióit.  Keressen rá a "Microsoft Identity Manager 2016 a Service Pack 2" kifejezésre.  

Ha nem rendelkezik hozzáféréssel a mennyiségi licencelési szolgáltatási központhoz, és csak korlátozott ideig szeretné kipróbálni a fakiszolgáló szoftverét, letöltheti a webalkalmazás [próbaverzióját 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244). Ez a szoftver nem éles használatra készült, és az első telepítés után 180 nappal megszűnik, és nem frissíthető. A próbaverzió telepítéséhez a Windows Server 2008 R2, a Windows Server 2012 vagy a Windows Server 2012 R2 szükséges.  Ha még nem ismeri a webszolgáltatást, és megtanítja a technológiát, vegye figyelembe, hogy az összes felügyeleti ponthoz Active Directory tartomány, Windows Server és SQL Server szükséges. Ha nem rendelkezik Windows Serverrel vagy SQL Server már jelen van, érdemes kipróbálnia [egy virtuális gép üzembe helyezését SQL Server 2016 és a Windows server 2016](https://azure.microsoft.com/blog/azure-images-sql-server-2016-on-windows-server-2016/)használatával.

### <a name="obtaining-updates"></a>Frissítések beszerzése

Miután telepítette a webszolgáltatást az MSI-ről, a következő lépésekkel telepítse a szükséges gyorsjavításokat.

Tekintse meg a legfrissebb frissítési kiadáshoz tartozó [Identity Manager verzió előzményeit](./reference/version-history.md) , amely a telepítési javítási fájlok letöltési helyére mutató hivatkozást tartalmaz.

Annak megállapításához, hogy mely frissítési fájlok szükségesek, ez a táblázat a frissítésben szereplő összetevők és a hozzá tartozó patch (MSP) fájl nevét sorolja fel.

| Használati eset | Összetevő | DVD ISO-mappa neve | A frissítési javítókészlet megfelelő fájljának neve |
|----------|-----------|-   |-------------------|----------|--------------|
|Szinkronizálás| Szinkronizálási szolgáltatás | `Synchronization Service` | `MIMSyncService_x64*.msp` |
| Önkiszolgálás | A fakiszolgálói portál | `Service and Portal` | `MIMService_x64*msp` |
| Önkiszolgálás | Beépülő modulok és bővítmények | `Add-ins and extensions` | `MIMAddinsExtensions*msp` |
| Önkiszolgálás | Nyelvi csomagok | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Hozzáférés-kezelés (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Tanúsítványkezelés | CM |  `Certificate Management` | `MIMCM*.msp` |
| Tanúsítványkezelés | CM tömeges ügyfél |  `CM Bulk Client` |`MIMCMBulkClient*msp` |
| Tanúsítványkezelés | CM-ügyfél | `CM Client` |`MIMCMClient*msp` |

Ügyeljen arra, hogy az MSP-fájl telepítése előtt olvassa el a frissítéshez társított kibocsátási megjegyzéseket.

A [BHOLD](https://www.microsoft.com/download/details.aspx?id=55950) frissítései nem érhetők el MSP-fájlokként, csak MSI-telepítőként.

### <a name="additional-downloads"></a>További letöltések

A következő letöltések is relevánsak lehetnek:

- [Webszolgáltatási hibrid jelentéskészítő ügynök](https://www.microsoft.com/download/details.aspx?id=55112)

- [Általános LDAP-összekötő, általános SQL-összekötő, gráf-összekötő, Lotus Domino-összekötő, PowerShell-összekötő, webszolgáltatás-összekötő](https://go.microsoft.com/fwlink/?LinkId=717495)

- [A SharePoint felhasználói profil tárolójának összekötője](https://www.microsoft.com/download/details.aspx?id=41164)

- Ha még nem rendelkezik Active Directory tartománnyal, és beállít egy PAM-forgatókönyvet a kísérletezéshez, tekintse meg a következőt: a rendszerszintű [2016 SP1 PAM üzembehelyezési szkriptek](sp1-deployment-scripts.md).

## <a name="next-steps"></a>Következő lépések

- További információ a [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md)-es verzióban elérhető forgatókönyvekről.
- Olvassa el a [kapacitás-tervezési útmutatót](capacity-planning-guide.md).
- A beléptetési [forgatókönyv](microsoft-identity-manager-deploy.md) vagy a [privilegizált hozzáférés-kezelési forgatókönyv](./pam/privileged-identity-management-for-active-directory-domain-services.md)üzembe helyezése.

