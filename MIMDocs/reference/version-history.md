---
title: Az Identity Manager korábbi verziói | Microsoft Docs
description: Ez a cikk a 2016-es frissítés részeként végrehajtott különféle módosításokat dokumentálja.
keywords: ''
author: EugeneSergeev
manager: aashiman
editor: ''
reviewer: markwahl-msft
ms.devlang: na
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/11/2020
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.topic: reference
ms.openlocfilehash: 59a853c03cd24d89ffa2e89550de51d372c6391d
ms.sourcegitcommit: 7d7fdb47352282c2d98bf09274707a18fd1a22dc
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/30/2020
ms.locfileid: "96321981"
---
# <a name="identity-manager-version-release-history"></a>Az Identity Manager verziójának korábbi kiadásai

A Microsoft Identity Manager csapat rendszeresen frissítéseket szabadít fel. A cikk célja, hogy segítsen nyomon követni a kiadott verziókat. Ezután eldöntheti, hogy már a legújabb szervizcsomaggal és a gyorsjavítások frissítési verziójával rendelkezik-e, vagy ha frissítenie kell. 

>[!NOTE]
>Ez a cikk csak a fakiszolgálói szinkronizálás, a szolgáltatás és a portál, az ügyfél, a CM és a PAM-összetevők kiadásait ismerteti.  Nem biztos benne, hogy milyen összetevőkre van szüksége?  Lásd: [Microsoft Identity Manager licencelés és letöltések](../microsoft-identity-manager-licensing.md).
>
>A Microsoft BHOLD Suite összetevőinek korábbi verzióit a BHOLD- [modulok verziójának korábbi](version-bhold-history.md)verzióiban találja.
>
>Az általános LDAP, az általános SQL, a Web Services, a PowerShell, a Graph és a Lotus Domino-összekötő korábbi verziói a [Connector verziójának korábbi](microsoft-identity-manager-2016-connector-version-history.md)verzióiban találhatók.  

## <a name="mim-version-463590"></a>4.6.359.0-verzió
- Állapot: november 29., 2020
- [Gyorsjavítás letöltése](https://www.microsoft.com/download/details.aspx?id=102301)
- [TUDÁSBÁZISCIKK 4585922](https://support.microsoft.com/help/4585922)

Ez a gyorsjavítás a fakiszolgálói szinkronizációs kezelő, a címtárszolgáltatás és a webszolgáltatási portál összetevőinek frissítéseit tartalmazza, valamint az előző gyorsjavításokból származó, a webhelyhez 2016 SP2-hez készült kumulatív frissítéseket is tartalmaz.
A kicseréli a 4.6.355.0-t a rendszerkiszolgálói szolgáltatások teljesítményével kapcsolatos problémák kezelésére

## <a name="mim-version-463550"></a>4.6.355.0-verzió
- Állapot: november 6., 2020

Felváltotta a 4.6.359.0


## <a name="mim-version-462630"></a>4.6.263.0-verzió
- Állapot: augusztus 7., 2020
- [Gyorsjavítás letöltése](https://www.microsoft.com/download/details.aspx?id=101612)
- [TUDÁSBÁZISCIKK 4576473](https://support.microsoft.com/help/4576473)

Ez a gyorsjavítás a MIM CM, a bevezető szinkronizálási kezelő és a PAM-összetevők frissítéseit tartalmazza.


## <a name="mim-version-462580"></a>4.6.258.0-verzió
- Állapot: 2020. május 22.
- [Gyorsjavítás letöltése](https://www.microsoft.com/download/details.aspx?id=101305)
- [TUDÁSBÁZISCIKK 4560335](https://support.microsoft.com/help/4560335)

Ez a gyorsjavítás a fakiszolgálói szolgáltatás, a webhelyes portál és a PAM-összetevők frissítéseit tartalmazza.


## <a name="mim-version-46340"></a>4.6.34.0-verzió
* Állapot: webszolgáltatási 2016 (SP2) október, 2019
* A megfelelő BHOLD verziószáma: 6.0.62.0
- [Gyorsjavítás letöltése](https://www.microsoft.com/download/details.aspx?id=100412)
- [TUDÁSBÁZISCIKK 4512924](https://support.microsoft.com/help/4512924)
- Az ISO-fájl letölthető [a VLSC webhelyről vagy a](https://www.microsoft.com/Licensing/servicecenter/default.aspx) [Visual Studio-letöltések](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202) használatával

> [!IMPORTANT]
>- A .NET-keretrendszer 4,6-es megadása kötelező<br>
>- [Visual C++ 2013 Újraterjeszthető csomag (vcredist_x64.exe vagy vcredist_x86.exe)](https://www.microsoft.com/download/details.aspx?id=40784) szükséges<br>

>[!NOTE]
> Győződjön meg arról, hogy a felügyeleti csomag [telepítési útmutatója](../microsoft-identity-manager-deploy.md) tartalmazza a csak a TLS 1,2-környezetekkel kapcsolatos előfeltételeket és korlátozásokat, a csoportosan felügyelt szolgáltatásfiókok támogatását, valamint a [korábbi és a rendszerállapot-és FIM-verziók frissítési útvonalát](../microsoft-identity-manager-2016-service-pack-2-upgrade-path.md).

A Service Pack 2 (SP2) összegző csomagja (Build 4.6.34.0) elérhető a következőhöz: Microsoft Identity Manager (2016). Ez egy összesítő frissítés, amely lecseréli a korábbi, a 2016-es, a Build 4.5.412.0-en keresztül frissített 4.4.1302.0.
### <a name="updates-in-mim-2016-service-pack-2"></a>Frissítések a webszolgáltatáshoz 2016 Service Pack 2

#### <a name="mim-client-addons"></a>Web-ügyfél-bővítmények
- A Microsoft Office Outlook-bővítmény támogatása az Outlookba való betöltéshez Microsoft 365 kattintásra futtatott verzióra.

#### <a name="service-and-portal"></a>Szolgáltatás és portál
- A Microsoft Windows Server 2019 rendszerre telepítendő és a (z) SQL Server 2017, az Exchange Server 2019, a SharePoint 2019, a System Center Service Manager az adattárház 2019
- Engedélyezve van a fakiszolgálói szolgáltatás és-portál telepítése a TLS 1,2-környezetekben.
- Engedélyezve van a felügyeleti szolgáltatások telepítése, a jelszó-visszaállítási és a jelszó-regisztrálási webhelyek, a PAM-figyelési szolgáltatás, a PAM-összetevő szolgáltatás a csoportosan felügyelt szolgáltatásfiókok használatához.
- A "keepSQLjobs" telepítő paraméter hozzáadva.
- A SQL Server Agent időbeli feladatait már nem indítja el a másodlagos SQL Always-On rendelkezésre állási csoport replikái.
- A "ExplicitMember. add" és a "ExplicitMember. remove" virtuális attribútumok engedélyezve vannak az egyéni objektumtípusok számára a RCDC űrlapokon, hogy a változásokkal működjenek.

#### <a name="synchronization-service"></a>Synchronization Service
- A Windows Server 2019 rendszerre való telepítéshez és a SQL Server 2017, az Exchange Server 2019
- A rendszer csak a TLS 1,2-környezetekben engedélyezte a rendszerállapot-szinkronizálási szolgáltatás telepítését.
- Engedélyezve van a telepítés a felügyeleti webszolgáltatások szinkronizációs szolgáltatásához csoportosan felügyelt szolgáltatásfiók használatára.
- A "MIMSync-fiók használata" beállítás hozzáadva a Rendszerfelügyeleti webszolgáltatások kezelési ügynökéhez.
 
#### <a name="privilege-access-management"></a>Jogosultság-hozzáférés kezelése 
- A "Get-PAMRequest" PowerShell-parancsmag egy további "FIMRequestID" tulajdonságot ad vissza.


## <a name="mim-version-454120"></a>4.5.412.0-verzió
> [!IMPORTANT]
>- A RCDC űrlapja nem jeleníthető meg, ha a "displayedOwner" attribútum értéke nincs feltöltve. Ilyen hiba esetén nem szerkesztheti és nem tekintheti meg a csoportokat.<br>

- Rendelkezésre állás dátuma: május 10., 2019
- [Gyorsjavítás letöltése](https://www.microsoft.com/en-us/download/details.aspx?id=58213)
- [TUDÁSBÁZISCIKK 4489646](https://support.microsoft.com/en-us/help/4489646/hotfix-rollup-4-5-412-0-available-for-mim-2016-sp1)

Ez a gyorsjavítás a következő frissítéseket tartalmazza: a rendszer-szinkronizálási szolgáltatás, a fakiszolgálói portál, a MIM CM és a PAM-összetevők frissítése.  Ez egy összesítő frissítés, amely lecseréli a korábbi, a 2016-es, a Build 4.5.286.0-en keresztül frissített 4.4.1302.0.

> [!IMPORTANT]
>- A telepítőhöz a .NET-keretrendszer 4,6-es verzióra is szükség van <br>
>- [Visual C++ 2013 x64 újraterjeszthető csomagokat (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) kell megadni<br>

## <a name="mim-hybrid-reporting-agent-version-31410"></a>A 3.1.41.0 hibrid jelentéskészítő ügynökének verziója
- Elérhetőség dátuma: április 22., 2019
- [Hibrid jelentéskészítő ügynök letöltése](https://www.microsoft.com/en-us/download/details.aspx?id=55112)

A 3.1.41.0 hibrid jelentéskészítő ügynök verziója a TLS 1,2-es és a hibajavítási frissítéseket tartalmazza.  A hibrid jelentéskészítő ügynök a 2016 SP1 4.4.1302.0-es vagy újabb verziójával és egy prémium szintű Azure AD-előfizetéssel is használható.


## <a name="mim-version-452860"></a>4.5.286.0-verzió
- Állapot: november 19., 2018
- [Gyorsjavítás letöltése](https://www.microsoft.com/en-us/download/details.aspx?id=57596)
- [TUDÁSBÁZISCIKK 4469694](https://support.microsoft.com/en-us/help/4469694/hotfixrolluppackagebuild452860isavailableformicrosoftidentitymanager20)


Ez a gyorsjavítás a fakiszolgálói szolgáltatás, a webhelyes portál és a PAM-összetevők frissítéseit tartalmazza.  A telepítőhöz a .NET-keretrendszer 4,6-es verzió szükséges.


## <a name="version-452020"></a>4.5.202.0 verziója
- Állapot: augusztus 30., 2018
- [KB-os kiadás KB](https://support.microsoft.com/en-us/help/4346632)

> [!IMPORTANT]
>- A telepítőhöz a .NET-keretrendszer 4,6-es verzióra is szükség van <br>
>- [Visual C++ 2013 x64 újraterjeszthető csomagokat (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) kell megadni<br>
>- Támogatott területi beállítások frissítve az új ISO-szabványokkal ([itt](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-language-support))<br>
>- * Új fejlesztést jelöl 

#### <a name="mim-service"></a>MIM szolgáltatás
- Azure MFA-kiszolgáló integrációja – alternatív Multi-Factor Authentication szolgáltató

#### <a name="privilege-access-management"></a>Jogosultság-hozzáférés kezelése 
- A PAM-REST API nem indítható el, mert nem tölthető be a fájl vagy szerelvény

#### <a name="microsoft-identity-portal"></a>Microsoft Identity Portal
- A portál helytelen táblázatos hosszsal jelenik meg
- A portál speciális keresési párbeszédpanele, a görgetősávok nem jelennek meg megfelelően
- A nyelvi csomag nyelvi csomagja erős név-aláírásának ellenőrzése nem sikerült

#### <a name="certificate-management"></a>Tanúsítványkezelés
- REST API kötési átirányítási utasítása

## <a name="version-45260"></a>4.5.26.0 verziója
- Állapot: június 30., 2018
- [TUDÁSBÁZIS kiadási KB4073679](https://support.microsoft.com/en-us/help/4073679)

> [!IMPORTANT]
>- A telepítőhöz a .NET-keretrendszer 4,6-es verzióra is szükség van <br>
>- [Visual C++ 2013 x64 újraterjeszthető csomagokat (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) kell megadni<br>
>- Támogatott területi beállítások frissítve az új ISO-szabványokkal ([itt](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-language-support))<br>
>- * Új fejlesztést jelöl 

#### <a name="synchronization-service"></a>Szinkronizációs szolgáltatás
- * Csoportosan felügyelt szolgáltatásfiókok támogatása
- * Visual Studio-támogatás (Visual Studio 2013, Visual Studio 2015, Visual Studio 2017)
- MIISACTIVATE.EXE frissítései, gMSA-támogatás hozzáadva 
    - nem gMSA: Miisactivate.exe c:\configBU\ miiserver_01. bin "contoso\mimSyncService" *
    - gMSA: Miisactivate.exe c:\configBU\ miiserver_01. bin "contoso\mimSyncService"
- MIISKMU.exe frissítései, gMSA-támogatás hozzáadva 
    - non-gMSA:MIISKMU.exe/e c:\configBU\ miiserver_02. bin "/u:" contoso\mimSyncService "
    - gMSA:MIISKMU.exe/e c:\configBU\ miiserver_02. bin "/u:" contoso\mimSyncService "*
- A frissített partíciós adatok a várt módon lesznek mentve, amikor a frissítés, majd az OK gombokra kattint
- Ha egy indexelhető karakterlánc-attribútum indexelése túl hosszú, nem várt hiba történt, a rendszer további leíró hibaüzenetet ad vissza.
- Szövegfájl-felügyeleti ügynök létrehozása, ha a felügyeleti pont szinkronizációs szolgáltatása a Windows Server 2016-re van telepítve, bizonyos szöveges kódolási beállítások, beleértve a Unicode-ot, nem voltak elérhetők
- Ha az exportálási hibaüzenet érvénytelen karaktert tartalmaz, akkor ez a rendszer a futtatási előzmények bejegyzéseiben sérülést okoz. Ez a Build el lett távolítva a hibaüzenetből, mielőtt mentjük az összekötő terület objektumra és a futtatási előzményekre.

#### <a name="mim-service"></a>MIM szolgáltatás
- * Csoportosan felügyelt szolgáltatásfiókok támogatása
- * Továbbfejlesztett nyelvi támogatás új meghatározott szabványhoz
- * FIMAutomation Export-FIMConfig PowerShell-parancsmag a "-PamConfig" argumentum elérhető a PAM-konfigurációs objektumok exportálásának kényszerítéséhez
- * FIMAutomation Export-FIMConfig PowerShell-parancsmag a "-Request" paraméter hozzá lett adva.
- * A logikai attribútumok mindig NULL értékre vannak állítva a kötés létrehozásakor, a korábbi logikai értéket a gyorsjavítás frissítése előtt.
> [!IMPORTANT]
>Ez a konfiguráció áttelepítésének megváltoztatását is okozhatja. A konfigurációt ki kell értékelni és frissíteni kell az új szolgáltatásként, mivel a konfiguráció áttelepítése új 
    - Új falemezes logikai attribútumok implementálása hamis értékre az új objektum létrehozásakor
    - Új rendszerállapot-logikai attribútumok implementálása hamis értékre, ha új logikai attribútum kötést ad hozzá az erőforráshoz
- Felhasználói élmény fokozása program a beállítás a False (hamis) értékre van állítva 
- A rendszerállapot-szolgáltatás telepítése az adatbázis-frissítési hiba miatt nem sikerült: NULL érték nem szúrható be a (z) "Name" oszlopba, ha nem használja az alapértelmezett adatbázisnevet.
- Gyorsjavítási esetekben a rendszer törli a Microsoft 365 beállítást, mert a rendszer nem módosítja a webkiszolgáló Exchange Online-postaládájának titkosított jelszavát.
- * Nem volt korlát a rendszerállapot-kezelő szolgáltatás naplófájljának létrehozásához, a frissített naplózás alapértelmezett beállítása és a körkörös naplózási funkció implementálása

#### <a name="privileged-access-management"></a>az emelt szintű hozzáférések felügyeletével 
- * Csoportosan felügyelt szolgáltatásfiókok támogatása
- * Továbbfejlesztett nyelvi támogatás új meghatározott szabványhoz
- A nem felügyelt erőforrásokat használó objektumokat nem törli időben a rendszer.  ezeket az objektumokat a rendszer megfelelően kitakarítja
- * New-PAMRole PowerShell-parancsmag a "-disableAutoApproveIfOwner" megtagadja a szerepkör önálló jóváhagyását
- * Get-PamRequest PowerShell-parancsmag a "-CreatedFrom" lehetővé teszi, hogy a szűrési od PAM-specifikus kérelme
- * PAM-modul kiegészítései
    - Get-PAMSet
    - Add-PAMSetMember
    - Remove-PAMSetMember
- A figyelmeztetés (kivétel: System. ObjectDisposedException: nem érhető el egy eldobott objektum) többé nem fog megjelenni a PAM-eseménynaplóban.
- Set-PAMUser parancsmag a törlés nélkül képes módosítani a PrivAccountName
- New-PamRole most ellenőrzi, hogy az "elérhető" dátum nagyobb-e, mint a "rendelkezésre álló" dátum
- A Get-PAMRole PowerShell-parancsmag visszaadja az "elérhető innen" és "elérhető" értéket.
- A Get-PamRequest parancsmag szűrője már megfelelően működik
- A set-PamGroup parancsmag mostantól képes frissíteni az Active Directory Shadow Principal Group objektumot
- Remove-PamUser PowerShell-parancsmag egy nem egyértelmű hibaüzenettel meghiúsul, ha a felhasználó egy szerepkörhöz van társítva jelöltként. Most az ügyféloldali érvényesítés hozzá lett adva a parancsmaghoz, és a visszaadott kivétel tisztázva lett
- A Change Mode PAM-fiókok nem érhetők el a konfigurációhoz
    - PAM REST API-fiók
    - PAM-összetevő szolgáltatás fiókja
    - PAM-figyelő szolgáltatás fiókja

#### <a name="microsoft-identity-portal"></a>Microsoft Identity Portal
- * Csoportosan felügyelt szolgáltatásfiókok támogatása
- * Továbbfejlesztett nyelvi támogatás új meghatározott szabványhoz
- Identity Picker Control, a vezérlő úgy tűnik, hogy a szöveg körbefuttatása helyett dinamikusan nő a szélessége
- A portálon az előugró párbeszédpanelek nem jelennek meg megfelelően az Internet Explorerben való megtekintéskor (IE) 10
- A címsor szövegében szereplő cirill szimbólumok helyesen jelennek meg
- A felugró ablakok már nem jelennek meg az Internet Explorerben megjelenő további görgetősávon.
- A "munkafolyamat-definíció importálása" művelet végrehajtása nem sikerült, és a rendszer egy kivételt és helyreállítást végez, amely lehetővé teszi a szinkronizálási szabály tevékenységének felvételét a munkafolyamat-definícióba
- <httpRuntime enableVersionHeader="false" /> hozzáadva az alapértelmezett web.config
- A distinguishedName szereplő speciális karakterek többé nem akadályozzák meg, hogy a jelszó alaphelyzetbe állítása Self-Service a felhasználó jelszavát a Active Directory
- A mondatok fejlesztése megfelelően honosítva van a kijelzőn
- Az Outlook rendszerhez készült modul-bővítmény tartalmazza a hiányzó Outlook együttműködési bináris fájlok másolatát

#### <a name="certificate-management"></a>Tanúsítványkezelés
- Virtuális intelligens kártya megújítása a MIM CM modern alkalmazással, a felhasználó tiltott kivételt kap
- * Továbbfejlesztett nyelvi támogatás új meghatározott szabványhoz
- A "CLM" PIN-kód segédprogram hibát észlelt, miközben megpróbálta módosítani az intelligens kártya PIN-kódját.  Helytelen számú argumentum vagy érvénytelen tulajdonság-hozzárendelés. "
- Frissítsen a 4.4.1302.0-ből a felügyeleti webhelyről a 4.4.1459-be később, a telepítés meghiúsul
- Modern alkalmazás a megújítási, regisztrálási és lecserélési műveletekhez a kérelmek előzményei nem tartalmazzák a rögzített állapotú kérelmeket.
- Az online frissítés nem fejeződött be, és a következő kivételt adja vissza: "a rekordot egy másik felhasználó frissítette vagy törölte."  
- A tanúsítvány felügyeleti portál a tanúsítvány letöltése hivatkozásra kattintva a tanúsítvány letöltése (. cer fájl) túl nagy volt.
- A felügyeleti webszolgáltatások tanúsítvány-kezelési tömeges ügyfele a TLS 1,1 és a TLS 1,2 használatával is működik.  

 
## <a name="version-4417490"></a>4.4.1749.0 verziója

- Állapot: november 30., 2017
- [Letöltés](https://www.microsoft.com/download/details.aspx?id=56271)

### <a name="fixed-issues"></a>Megoldott problémák
A 4.4.1749.0-ben a következő problémák lettek kijavítva.

#### <a name="synchronization-service"></a>Szinkronizációs szolgáltatás

- A jelszó-visszaállítási rutin meghiúsul, ha a szinkronizációs kiszolgáló tartománya nem rendelkezik megbízhatósági kapcsolattal a céltartományban.

#### <a name="mim-service"></a>MIM szolgáltatás

- Az adatbázis-frissítés sikertelen. A rendszer az adatbázis-frissítési naplóban rögzíti a külső kulcs megkötésének megsértése alóli kivételt.
- Ha önkiszolgáló jelszó-visszaállítási kérelmeket hajt végre, a rendszer véletlenszerűen leáll.
- A beállított számítás nem felel meg a megfelelő tagságnak. Egy attribútum kötése nem törölhető, ha egy dinamikus készletben vagy egy csoport szűrőben hivatkozik rá.
- A rendszerállapot-nyilvántartási szolgáltatás nem működött a kérelem-jóváhagyási forgatókönyvben az Exchange Online-ban, ahol a jóváhagyásokat a rendszer az Outlookhoz tartozó rendszerállapot-bővítményen keresztül válaszolja meg.
- Az országkód nélküli msidmPhoneGatePhoneNumber attribútum nem a DefaultCountryCode értéket használja MFASettings.xmlban.
- A tagságok beállítása dinamikusan frissíthető anélkül, hogy a FIM_TemporalEventsJob kellene támaszkodnia.
- A szinkronizálási szabályok nem támogatják az attribútum-flow szabályok létrehozását olyan attribútumok esetén, amelyek neve tartalmazza a kivonatot vagy a font szimbólumot (#).
  
#### <a name="privilege-access-management"></a>Jogosultság-hozzáférés kezelése 

- New-PAMDomainConfiguration PowerShell-parancsmag helytelen értéket állít be a tartományi megbízhatósági konfigurációhoz, ami hibát okoz (ez az ismeretlen kérési paraméter nem dolgozható fel).

#### <a name="microsoft-identity-portal"></a>Microsoft Identity Portal

- Kivétel jelenik meg az Identity felügyeleti portál fő képernyőjén, és megjelenik egy Bezárás gomb is.
- Az elemek törlése ablakban helytelenül jelenik meg a gombok.  Ez a probléma az Internet Explorerben, a Firefoxban és a Chrome-ban fordult elő. 
- A Keresés gomb átfedésben van az erőforrás-választó gombbal az engedélyezési munkafolyamat jóváhagyási tevékenység ablakában. Ez a probléma az Internet Explorerben, a Firefoxban és a Chrome-ban fordult elő. 
- A csoport tulajdonságai felugró ablakban a gomb területen átfedésben van a listanézet navigációs vezérlői a tagok törlése vezérlőben.  Ez a probléma az Internet Explorerben, a Firefoxban és a Chrome-ban fordult elő.
- Több FELHASZNÁLÓIFELÜLET-elem nem jelenik meg megfelelően. A következő elemek vannak kijavítva:

    - Fel és le nyilak egyes tulajdonságlapokon.
    - Üres terület a lapok és párbeszédpanelek alján.
    - Hiányzó előugró ablakok.

- Ha a Filter Builder-t használja a termék különböző területein (például a speciális kereséshez), a Filter Builder "beragadt" értékűre kerül, ha a Select Value (érték kiválasztása) párbeszédpanelen az OK gombra kattint, az Add utasítás területen kijelölt objektum nélkül.
- A szinkronizációs szabály szerkesztési párbeszédpanelének új attribútumának folyamata nem a várt módon működik a Chrome-ban.
- Egy objektum-felügyeleti képernyőn (például terjesztési csoportok), ha több objektum van kiválasztva a jelölőnégyzet bejelölésével, és az objektumok hosszú megjelenítendő névvel rendelkeznek. A párbeszédpanelek mérete függőlegesen, így a vezérlő nem terjeszti ki a böngésző képernyőjének végét.
- Egy objektum-kezelés vagy lista képernyőjén (például terjesztési csoportok) a kijelölt elemek vezérlő feljebb helyezheti a képernyőt, hogy közvetlenül a táblázatos listákban szereplő utolsó objektum alá kerüljön.
- A Safari böngészőben a Filter Builder (például a speciális keresés) nem működik.
- a portál az attribútumérték megjelenítésére szolgáló párbeszédpaneleken a rövidebb szavakat a cellába helyezi el, nem pedig balra igazítva. 
- Egyes böngésző-verziók esetében a kijelölt elemek nem frissülnek, amikor módosul az elem kijelölése.
- A párbeszédpanelek és a vágólapra való másolás a TAB billentyűvel való kiemeléskor.  
- Az Internet Explorer 10 esetében, amikor egy objektum rácsának megjelenítését (például terjesztési csoportokat) jeleníti meg, a "a fenti kereséshez használni kívánt terjesztési csoportok keresése" szalagcím befedi a gomb menüszalagjának egyik részét, és nem jelenik meg a párbeszédpanel közepén.  
- Miután telepített egy frissítést a webszolgáltatási portálra, a portál nem fog megjelenni az Internet Explorerben.
- Ha a Firefox böngészőben a speciális keresés funkciót használja, akkor az ENTER billentyű lenyomásakor az attribútum értéke mező hibát jelez.  

#### <a name="certificate-management"></a>Tanúsítványkezelés

- Egy kérelem kezdeményezője (tanúsítványkezelő) nem tud lemondani egy, a végrehajtási engedéllyel rendelkező felhasználó által duplikált vagy elfelejtett kérelmet.
- Ha a modern alkalmazásból próbálja meg megújítani a TPM virtuális intelligens kártyát, a rendszer tiltott kivételt ad vissza.
- Egyes intelligenskártya-tevékenységek során a CertificateManagement-adatbázishoz meglévő kapcsolatok váratlanul nyitva maradnak.  
- Ha a rendszer a MIM CM konfigurációs varázsló futtatása előtt próbálkozik a felügyeleti pont tanúsítványkezelő (CM) frissítésének telepítésével, a frissítés sikertelen lesz, és a probléma nem kapcsolódik a problémához.
- A MIM CM konfigurációs varázsló nem jeleníti meg a termék verziószámával kapcsolatos információkat, és az embléma helytelenül jelenik meg.  
- A Rendszerfelügyeleti webszolgáltatások tanúsítványkezelő jelentésének exportált adatainak eltérése eltér a jelentés adatainak.  Az oszlop oszlopai nem mindig egyeznek az oszlopfejlécek értékével.

## <a name="version-4416420"></a>4.4.1642.0 verziója

- Állapot: augusztus 29., 2017
- [Letöltés](https://www.microsoft.com/download/details.aspx?id=55794)

### <a name="fixed-issues"></a>Megoldott problémák
A 4.4.1642.0-ben a következő problémák lettek kijavítva.

#### <a name="synchronization-service"></a>Synchronization Service

- A jelszó-visszaállítási rutin meghiúsul, ha a szinkronizációs kiszolgáló tartománya nem rendelkezik megbízhatósági kapcsolattal a céltartományban.
- A deklarált importálási szűrő (megkülönböztető név ellenőrzése) nem működik megfelelően, ha egy objektumot egy szervezeti egységből helyeznek át, ahol azt egy másik helyre kell szűrni, ahol nem lehet egy fantom objektum régi nevének használata.
- A felügyeleti ügynök tervezője lefagy a "partíciók és hierarchiák konfigurálása" oldalon.
- A sorrend nem továbbítja a következő objektumra, ha az előző és a prioritás kapcsolata le van választva.
- A Sun egy felügyeleti ügynök, amely az LDAP-kiszolgálón lévő gyermek tárolókat keres, a lapozás hibát okoz, ha a kiszolgáló nem támogatja a lapozást.
- A metaverse objektumtípus dinamikusan módosult, ami összeomlást okoz.

#### <a name="mim-service"></a>MIM szolgáltatás

- A Word függvény nem ad vissza üres karakterláncot, ha a sztring kevesebb, mint a szavak számát tartalmazza.
- A tagok megtekintése felhasználói felület a helytelen tagságot jeleníti meg, ha a feltétel visszakeresésének tartalmaz, és ha a visszakeresésének egy másik feltétel alá esik. 
- A AuthZ munkafolyamat megtagadja a (z) "a (z)" munkafolyamat nem található az állapot-megőrzési tárolóban.
- A munkafolyamat egy enumerálási erőforrás-tevékenységet futtat a famodul lekérdezéséhez, és a folyamat időszakosan leáll.

#### <a name="privilege-access-management"></a>Jogosultság-hozzáférés kezelése

- Get-PAMRequestToApprove a parancsmagot nem ad vissza pályázót, ha a jóváhagyó nem a szerepkört hagyja jóvá.
- A kapcsolódó MPR és PAM navigációs sáv csomópontja akkor is engedélyezve van, ha a PAM nincs telepítve.
- A rendszerállapot-szolgáltatás a PAM-hoz tartozó tartományi konfiguráció szinkronizálójának kivételét és naplóit veti fel.

#### <a name="microsoft-identity-portal"></a>Microsoft Identity Portal

- Ha a webkiszolgálón található Filter Builder-portálon szeretné elérni a Firefoxot, a belső vezérlők (legördülő listák, szövegmezők stb.) nem használhatók. Nem választhatók ki, amíg be nem jelöli a jobb gombbal (azaz a helyi menü megjelenítéséhez kattintson a gombra).
- A portál keresési funkciói nem megfelelően jelennek meg bizonyos képernyőfelbontások esetén. 
- A speciális keresésben szereplő Calendar Control csonkítva van.
- A Filter Builder felhasználói felülete megszakadt – szerkesztési módban (hibás elemek).
- A felugró ablak rögzített mérettel rendelkezik, és a szerkesztési vezérlők nem megfelelő méretűek.
- A főmenüben található Norvégia és más nyelvek esetében a sztringek száma.
- Nem működik a másolt URL-cím az előugró ablakban.

#### <a name="certificate-management"></a>Tanúsítványkezelés

- Webszolgáltatási önkiszolgáló jelszó-portálok.

#### <a name="reporting"></a>Jelentéskészítés

- A jelentéskészítés Import-FIMReportingSchemaDefinition parancsmag hibával meghiúsul.

#### <a name="client-add-in"></a>Ügyféloldali bővítmény

- A felhasználói felület nyelve nem vizsgálja meg az országkód egyenlőségét.

### <a name="new-features-and-improvements"></a>Új funkciók és Újdonságok
A következő funkciók és tökéletesítések lettek hozzáadva a 4.4.1642.0-ben.

#### <a name="mim-service"></a>MIM szolgáltatás

- Újra hozzáadva a leghosszabb kérelmek feldolgozási műveleteihez (ellenőrzési fázis). Nem garantálja, hogy a kérelmek feldolgozása befejeződött, de a kérések stabilabbak. A javítás alapértelmezés szerint le van tiltva. A alwaysOnRetryRequestProcessingTransaction = "true" érték hozzáadásával engedélyezheti a FIMService konfigurációs fájl resourceManagementService szakaszát.

#### <a name="certificate-management"></a>Tanúsítványkezelés

- A CM-kiszolgáló újabb verziói képesek együttműködni a régebbi BulkClient (de nem régebbi, mint 4.4. xxxx. 0). 

#### <a name="mim-self-service-password-portals"></a>Webszolgáltatási önkiszolgáló jelszó-portálok 

- Figyelmeztetés megjelenítése a QAGate, ha a használatban dupla bájtos karaktert tartalmazó választ ad meg
- Konfigurálható tulajdonság hozzáadásával engedélyezheti vagy letilthatja az ÍRÁSJEGYBEVIVŐ használatának engedélyezését és letiltását a SSPR regisztrációs űrlapján 
- A SSPR felfedi a jelszó-regisztrálási kérdéseket a portál ügyfelén

## <a name="version-4415490"></a>4.4.1549.0 verziója
 
* Állapot: korábbi 2016 SP1 gyorsjavítás frissítése, 2017. március 27.
* A megfelelő BHOLD verziószáma: 6.0.36.0
* További információk: [https://support.microsoft.com/en-us/help/4012498](https://support.microsoft.com/en-us/help/4012498)


## <a name="version-4413020"></a>4.4.1302.0 verziója

* Állapot: webszolgáltatási 2016 Service Pack 1 (SP1), 2016. november 9.
* A megfelelő BHOLD verziószáma: 5.0.3355.0

### <a name="updates-in-this-service-pack"></a>A szervizcsomag frissítései


#### <a name="mim"></a>MIM

- **A MIM-portál böngészőkompatibilitása végfelhasználók önkiszolgáló munkafolyamataihoz:** Ebben a szervizcsomagban támogatást biztosítunk a legtöbb ismert böngészőhöz. A felhasználók mostantól elérhetik és használhatják a MIM-portált önkiszolgáló csoportok és profilok kezelésére a Microsoft Edge, a Chrome és a Safari böngészőből.

- **MIM szolgáltatás támogatása Exchange Online-hoz:** A MIM szolgáltatás régóta támogatja e-mailek küldését és fogadását jóváhagyáskor és értesítéskor. Az SP1 előtt a MIM csak az Exchange Server vagy az SMTP szolgáltatást támogatta. A Service Pack 1 csomaggal a beMicrosoft 365 Exchange Online-fiók használatával küldhet és fogadhat kérelmeket, valamint e-mail-értesítéseket.

- **Kép fájlformátumának érvényesítése feltöltéskor:** A MIM-mel már lehetséges a képek fájlformátumának érvényesítése a portálra történő feltöltés közben.

#### <a name="privileged-access-managementpam"></a>Privileged Access Management (PAM)

- **PAM „PRIV” (bástya) erdőtámogatás Windows Server 2016 működési szinthez:** A MIM PAM szolgáltatás konfigurálható olyan tartományvezérlőkkel rendelkező környezetben, amely a Windows Server 2016 Active Directory Domain Services-erdő működési szintjén fut. A konfigurálás után a felhasználók Kerberos-jegye időkorlátossá válik a szerepkör-aktiválásban megmaradt időre.

    >[!Note]
    Ha karban kívánja tartani a Windows Server 2012 R2-erdő működési szintjét a CORP-tartományban, a [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) és a [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) telepítése javasolt a CORP-tartományvezérlőn.

- **Rendszerjogosultságú fiók kiterjesztése a „PRIV” (bástya) erdő kizárólagos csoportjaiba:** Mostantól a rendszergazdák értesíthetik a „PRIV”-erdő kizárólagos MIM-szolgáltatáscsoportjait és -felhasználóit. Ez lehetővé teszi csoportok és felhasználók PAM-szerepkörbe helyezését.  Ezután aktiválhatók egy szerepkörhöz, és csoporttagság rendelhető hozzájuk a „PRIV” erdőben.

- **PAM-üzembehelyezési szkript:** A PAM-üzembehelyezési szkriptek lehetővé teszik a rendszergazdák számára a PAM-környezet telepítésének leegyszerűsítését.

- **PAM-parancsmagok hitelesítési házirendi siló konfigurálásához:** Az 1. szervizcsomagban új parancsmagok találhatók, amelyek megerősítik a bástyaerdő biztonságát. Ezek a parancsmagok automatikusan létrehoznak egy hitelesítési házirendi sablonhoz kötött hitelesítési házirendi silót.

    >[!Note]
    Ezek a parancsmagok automatikusan lefutnak az üzembehelyezési szkriptek részeként.



### <a name="platform-support"></a>Platformtámogatás 

A frissített platformtámogatási információk megtalálhatók a [MIM 2016 által támogatott platformok](../microsoft-identity-manager-2016-supported-platforms.md) című dokumentumban.  A szervizcsomagban támogatott új platformok közé tartozik a SQL Server 2016, a SharePoint 2016.


### <a name="issues-fixed-in-this-release"></a>Ebben a kiadásban rögzített problémák


#### <a name="pam"></a>PAM
- Új – A PAMGroup nem hoz létre MIM-objektumokat tartományi helyi csoportokhoz a PRIV-erdőben.
- Új – A PAMDomainConfiguration sikertelen eredményt adna vissza „netdom” hibaüzenettel.
- A PAM figyelőszolgáltatása által naplózott figyelmeztetések csoportoknak a PRIV-erdőben.


### <a name="how-to-upgrade"></a>A frissítés módja

A Microsoft Identity Manager 2016 Service Pack 1-re áttérő ügyfeleknek ajánlott követni az alábbi útmutatót a környezetben használt összes szolgáltatásnál.

>[!Note]
>A Forefront Identity Manager 2010 R2 SP1 vagy korábbi verziót használó felhasználónak először frissíteniük kell a környezetüket a 2015 augusztusában megjelent Microsoft Identity Manager 2016-ra, majd követni az alábbi lépéseket.

Előkészületek

A MIM-szinkronizálási motor frissítése szükséges a MIM szolgáltatás és a portál frissítése előtt.
Biztonsági másolat készítése szükséges a MIMService és MIM Sync adatbázisról.

1. Távolítsa el a frissíteni kívánt Microsoft Identity Manager-összetevőt.
2. Az eltávolítás után nyissa meg a telepítési adathordozón található „FIMSplash.htm” kezdőlapot.
3. Válassza ki a frissíteni kívánt MIM-összetevőt.
4. Az utasításokat követve folytassa a telepítést.
   * A MIM szolgáltatás és portál telepítése: Az Exchange Online levelezési fiókként történő kiválasztásakor írja be az Exchange Online-fiókhoz tartozó e-mail-címet és hitelesítő adatokat a következő képernyőn.

## <a name="version-4322660"></a>4.3.2266.0 verziója


* Állapot: 2016. július 15., 2016; az ügyfeleknek frissíteniük kell a legújabb verzióra, hogy a támogatottak maradjanak
* További információk: [https://support.microsoft.com/en-us/kb/3171342](https://support.microsoft.com/en-us/kb/3171342)

## <a name="version-4321950"></a>4.3.2195.0 verziója

* Állapot: 2016. március 20., 2016-as gyorsjavítás, az ügyfeleknek újabb verzióra kell frissíteniük, hogy továbbra is támogatottak maradjanak
* A megfelelő BHOLD verziószáma: 5.0.3355.0
* További információk: [https://support.microsoft.com/kb/3134725](https://support.microsoft.com/kb/3134725)
    
## <a name="version-4320640"></a>4.3.2064.0 verziója

* 2016-es állapotú, 2015-es, december 11-én kiadott gyorsjavítás; az ügyfeleknek frissíteniük kell a legújabb verzióra, hogy a támogatottak maradjanak
* A megfelelő BHOLD verziószáma: 5.0.3176.0
* További információk: [https://support.microsoft.com/kb/3092179](https://support.microsoft.com/kb/3092179)

## <a name="version-4319350"></a>4.3.1935.0 verziója

* 2016-as állapotú, 2015. augusztus 6. az ügyfeleknek frissíteniük kell a legújabb verzióra, hogy a támogatottak maradjanak
* A megfelelő BHOLD verziószáma: 5.0.3079.0

