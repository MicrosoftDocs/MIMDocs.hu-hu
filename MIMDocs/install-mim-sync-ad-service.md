---
title: A Microsoft Identity Manager Synchronize használata az AD szolgáltatással | Microsoft Docs
description: Az Active Directory és a MIM-adatbázisok szinkronizálása kezelőügynökök és a MIM Sync szolgáltatás segítségével.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 81cf34959ccdea5ad9eb463f85a25d26bc1d8ede
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042423"
---
# <a name="install-mim-2016-synchronize-active-directory-and-mim-service"></a>A MIM 2016 telepítése: Az Active Directory és a MIM szolgáltatás szinkronizálása

> [!div class="step-by-step"]
> [« MIM szolgáltatás és -portál](install-mim-service-portal.md)
> 
> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Például:
> - Tartományvezérlő neve – **mimservername**
> - Tartománynév – **contoso**
> - Jelszó<strong>Pass@word1</strong>

Alapértelmezés szerint a MIM Synchronization Service (MIM Sync) szolgáltatáshoz nincs összekötő konfigurálva.  Az első lépés általában a MIM szolgáltatás adatbázisának feltöltése a meglévő Active Directory-fiókokkal a MIM Sync szolgáltatás használatával. Ehhez a MIM Sync Service alkalmazást kell használni.

## <a name="create-the-mim-management-agent"></a>A MIM-kezelőügynök létrehozása
A MIM-kezelőügynök (MA) összekötőként funkcionál a MIM Sync és a MIM szolgáltatás között. Az összekötő a Create Management Agent (Kezelőügynök létrehozása) varázslóval hozható létre.

A MIM-kezelőügynök konfigurálásához meg kell adnia egy felhasználói fiókot. A dokumentumban erre a fiókra **MIMMA** néven hivatkozunk.

> [!NOTE]
> A MIM-kezelőügynökhöz használt fióknak azonosnak kell lennie a MIM szolgáltatás telepítésekor megadott fiókkal. Ha azt tervezi, hogy engedélyezi a "MIMSync-fiók használata" funkciót, akkor a felügyeleti csomag szinkronizálási szolgáltatását csoportosan felügyelt szolgáltatásfiók használatával kell telepíteni.

### <a name="to-create-the-mim-ma"></a>A MIM-kezelőügynök létrehozása

1.  Indítsa el a Synchronization Service Managert.

2.  A felügyeleti ügynök létrehozása varázsló megnyitásához váltson a **felügyeleti ügynökök** lapra, majd a **műveletek** menüben kattintson a **Létrehozás**parancsra.

3.  A **felügyeleti ügynök létrehozása** lapon adja meg a következő beállításokat, majd kattintson a **tovább**gombra.

    -   Management agent for (Felügyeleti ügynök a következőhöz): FIM Service management agent (A FIM szolgáltatás felügyeleti ügynöke)

    -   Name (Név): MIMMA

4.  A **Connect to Database** (Kapcsolódás az adatbázishoz) lapon adja meg a következő beállításokat, majd kattintson a **Next** (Tovább) gombra.
    -   Server (Kiszolgáló): localhost

    -   Database (Adatbázis): FIMService

    -   Webkiszolgálói szolgáltatás alapcíme:http://localhost:5725

    -   Authentication mode (Hitelesítési mód): Windows integrated authentication (Integrált Windows-hitelesítés)

    -   User name (Felhasználónév): mimma

    -   Jelszó: Pass@word

    -   Domain (Tartomány): contoso

5.  A **Selected Object Types** (Kijelölt objektumtípusok) lapon győződjön meg arról, hogy az alábbiakban feltüntetett objektumtípusok ki vannak jelölve, majd kattintson a **Next** (Tovább) gombra.

    -   DetectedRuleEntry

    -   ExpectedRuleEntry

    -   Csoport

    -   Személy

    -   SynchronizationRule

6.  A **Selected Attributes** (Kijelölt attribútumok) lapon jelölje be a **Show All** (Összes megjelenítése) jelölőnégyzetet, ellenőrizze, hogy a feltüntetett attribútumok mind ki vannak-e jelölve, majd kattintson a **Next** (Tovább) gombra.

7.  A **Configure Connector Filter** (Összekötőszűrő konfigurálása) lapon kattintson a **Next** (Tovább) gombra.

8.  A **Configure Object Type Mappings** (Objektumtípus-hozzárendelések konfigurálása) lapon állítsa be a következő hozzárendelést, majd kattintson a **Next** (Tovább) gombra.

    - A **Data Source Object Type** (Adatforrás-objektum típusa) listában válassza a **Person** (Személy) elemet.
    - Kattintson az **Add Mapping** (Megfeleltetés hozzáadása) elemre a Mapping (Megfeleltetés) párbeszédpanel megnyitásához.
    - A **Metaverse object type** (Metaverzum-objektum típusa) listában válassza a **person** (személy) elemet.
    - A Mapping (Megfeleltetés) párbeszédpanel bezárásához kattintson az **OK** gombra.
    - A **Data Source Object Type** (Adatforrás-objektum típusa) listában válassza a **Group** (Csoport) elemet.
    - Kattintson az **Add Mapping** (Megfeleltetés hozzáadása) elemre a Mapping (Megfeleltetés) párbeszédpanel megnyitásához.
    - A **Metaverse object type** (Metaverzum-objektum típusa) listában válassza a **group** (csoport) elemet.
    - A Mapping (Megfeleltetés) párbeszédpanel bezárásához kattintson az **OK** gombra.

9.  A **Configure Attribute Flow** (Attribútumfolyam konfigurálása) lapon hozza létre az alábbiakban látható attribútumfolyam-megfeleltetéseket, majd kattintson a **Next** (Tovább) gombra.

    -   A Data source object type (Adatforrás objektumtípusa) és a Metaverse object type (Metaverzum objektumtípusa) beállításnál válassza a **Person** (Személy) lehetőséget.

    -   Válassza a **Direct** (Közvetlen) leképezéstípust.

    -   A következő táblázatban szereplő minden sornál végezze el az alábbi lépéseket:

        -   Válassza ki a táblázatban a sorhoz látható **Flow direction** (Folyamat iránya) beállítást.

        -   Válassza ki a táblázatban a sorhoz látható **Data source attribute** (Adatforrás-attribútum) beállítást.

        -   Válassza ki a táblázatban a sorhoz látható **Metaverse attribute** (Metaverzum-attribútum) beállítást.

        -   A folyamat leképezésének alkalmazásához kattintson a **New** (Új) elemre.

    | **Adatforrás-attribútum** | **Folyamat iránya** | **Metaverzum-attribútum** |
    |-|-|-|
    | AccountName | Exportálás | accountName |
    | DisplayName | Exportálás | displayName |
    | Domain | Exportálás | domain |
    | E-mail | Exportálás | Levelezés |
    | EmployeeID | Exportálás | employeeID |
    | EmployeeType | Exportálás | employeeType |
    | FirstName | Exportálás | firstName |
    | LastName | Exportálás | lastName |
    | ObjectSID | Exportálás | objectSid |

    -   Válassza ki **Group** (Csoport) lehetőséget az adatforrás típusaként és a metaverzum-objektum típusaként.

    -   Válassza a **Direct** (Közvetlen) leképezéstípust.

    -   A következő táblázatban szereplő minden sornál végezze el az alábbi lépéseket:

        -   Válassza ki a táblázatban a sorhoz látható **Flow direction** (Folyamat iránya) beállítást.

        -   Válassza ki a táblázatban a sorhoz látható **Data source attribute** (Adatforrás-attribútum) beállítást.

        -   Válassza ki a táblázatban a sorhoz látható **Metaverse attribute** (Metaverzum-attribútum) beállítást.

        -   A folyamat leképezésének alkalmazásához kattintson a **New** (Új) elemre.

    | **Adatforrás-attribútum** | **Folyamat iránya** | **Metaverzum-attribútum** |
    |-|-|-|
    | AccountName | Exportálás | accountName |
    | DisplayName | Exportálás | displayName |
    | Domain | Exportálás | domain |
    | E-mail | Exportálás | Levelezés |
    | MailNickName | Exportálás | mailNickName |
    | Tag | Exportálás | tag |
    | ObjectSID | Exportálás | objectSid |
    | Hatókör | Exportálás | scope |
    | Típus | Exportálás | type |
    | MembershipAddWorkflow | Exportálás | membershipAddWorkflow |
    | MembershipLocked | Exportálás | membershipLocked |
    | AccountName | Importálás | accountName |
    | DisplayedOwner | Importálás | displayedOwner |
    | DisplayName | Importálás | displayName |
    | MailNickName | Importálás | mailNickName |
    | Tag | Importálás | tag |
    | Hatókör | Importálás | scope |
    | Típus | Importálás | type |

10.  A **Configure Deprovisioning** (Megszüntetés konfigurálása) lapon kattintson a **Next** (Tovább) gombra.

11.  A kezelőügynök létrehozásához a **Configure Extensions** (Bővítmények konfigurálása) lapon kattintson a **Finish** (Befejezés) gombra.

## <a name="create-the-ad-management-agent"></a>Az AD-kezelőügynök létrehozása
Az Active Directory-kezelőügynök összekötőként szolgál az AD tartományi szolgáltatásokhoz. Az összekötő a Create Management Agent (Kezelőügynök létrehozása) varázslóval hozható létre.

1. A Create Management Agent (Kezelőügynök létrehozása) varázsló megnyitásához válassza az **Actions** (Műveletek) menü **Create** (Létrehozás) parancsát.

2. A **Create Management Agent** (Kezelőügynök létrehozása) oldalon adja meg a következő beállításokat, majd kattintson a **Next** (Tovább) gombra.

    - Management agent for (Felügyeleti ügynök a következőhöz): Active Directory Domain Services (Active Directory tartományi szolgáltatások)
    - Name (Név): ADMA

3. A **Connect to Active Directory Forest** (Kapcsolódás Active Directory-erdőhöz) lapon adja meg a következő beállításokat, majd kattintson a **Next** (Tovább) gombra:

    - Forest name (Erdőnév): contoso.local
    - User name (Felhasználónév): administrator
    - Password (Jelszó): &lt;a fiók jelszava&gt;
    - Domain (Tartomány): contoso

4. A **Configure Directory Partitions** (Címtárpartíciók konfigurálása) lapon adja meg a következő beállításokat, majd kattintson a **Next** (Tovább) gombra:

    - A **Select directory partitions** (Címtárpartíciók választása) listában válassza a **DC=CONTOSO, DC=local** elemet.

    - Kattintson a **Containers** (Tárolók) elemre a Select Containers (Tárolók kiválasztása) párbeszédpanel megnyitásához.

    - Ha úgy szeretné módosítani a tárolót, hogy csak a MIM kezelje az adott tároló objektumait, kattintson a **DC=CONTOSO,DC=local** csomópontra, majd kattintson a kívánt tároló csomópontjára.

    - Kattintson az **OK** gombra a Select Containers (Tárolók kiválasztása) párbeszédpanel bezárásához.

5. A **Configure Provisioning Hierarchy** (Kiépítési hierarchia konfigurálása) lapon kattintson a **Next** (Tovább) gombra.

6. A **Select Object Types** (Objektumtípusok választása) lapon adja meg a következő beállításokat, majd kattintson a **Next** (Tovább) gombra:

    - Az **Object types** (Objektumtípusok) listában jelölje ki a **user** (felhasználó) és a **group** (csoport) típust.

7. A **Select Attributes** (Attribútumok kiválasztása) lapon kattintson a **Show ALL** (Az ÖSSZES megjelenítése) elemre, válassza ki a következő attribútumokat, majd kattintson a **Next** (Tovább) gombra:

    -   cég
    -   displayName
    -   employeeID
    -   employeeType
    -   givenName
    -   groupType
    -   managedBy
    -   manager
    -   tag
    -   objectSid
    -   sAMAccountName
    -   sAMAccountType
    -   sn
    -   unicodePwd
    -   userAccountControl

8. A **Configure Connector Filter** (Összekötőszűrő konfigurálása) lapon kattintson a **Next** (Tovább) gombra.

9. A**Configure Join and Projection Rules** (Csatlakozási és leképezési szabályok konfigurálása) lapon kattintson a **Next** (Tovább) gombra.

10. A **Configure Attribute Flow** (Attribútumfolyam konfigurálása) lapon kattintson a **Next** (Tovább) gombra.

11. A megszüntetés **konfigurálása** lapon kattintson a **tovább**gombra.

12. A **Configure Extensions** (Bővítmények konfigurálása) lapon kattintson a **Finish** (Befejezés) gombra.


## <a name="create-run-profiles"></a>Futtatási profilok létrehozása

Hozzon létre futtatási profilokat az ADMA és a MIMMA összekötők számára.

### <a name="create-run-profiles-for-the-adma-connector"></a>Futtatási profilok létrehozása az ADMA összekötőhöz

Az alábbi táblázatban az ADMA összekötőhöz létrehozandó öt futtatási profil szerepel:

| Name (Név) | Típus |
| ---- | ---- |
| 1. profil | Teljes importálás (csak előkészítés) |
| 2. profil | Teljes szinkronizálás |
| 3. profil | Különbözeti importálás (csak előkészítés) |
| 4. profil | Különbözeti szinkronizálás |
| 5. profil | Exportálás |

Futtatási profilok létrehozása az ADMA összekötőhöz:

1. Nyissa meg a Synchronization Service Manager és az **eszközök** menüben kattintson a **felügyeleti ügynökök**elemre.

2. A **Management Agents** (Kezelőügynökök) listában válassza az **ADMA** lehetőséget.

3. A Configure Run Profiles for (Futtatási profilok konfigurálása) párbeszédpanel megnyitásához válassza az**Actions** (Műveletek) menü **Configure Run Profiles** (Futtatási profilok konfigurálása) parancsát.

4. A táblázatban szereplő minden futtatási profilnál végezze el a következő lépéseket:

    - A Configure Run Profile (Futtatási profil konfigurálása) varázsló megnyitásához kattintson a **New Profile** (Új profil) lehetőségre.

    - A **Name** (Név) mezőbe írja be a profil nevét a táblázatból, majd kattintson a **Next** (Tovább) gombra.

    - A **Type** (Típus) listában válassza ki a lépés típusát a táblázatból, majd kattintson a **Next** (Tovább) gombra.

    - A futtatási profil létrehozásához kattintson a **Finish** (Befejezés) gombra.

5. Kattintson az **OK** gombra a Configure Run Profiles (Futtatási profilok konfigurálása) párbeszédpanel bezárásához.

### <a name="create-run-profiles-for-the-mimma-connector"></a>Futtatási profilok létrehozása a MIMMA összekötőhöz

Az alábbi táblázatban a MIMMA összekötőhöz tartozó öt kapcsolódó futtatási profil szerepel:

| Name (Név) | Típus |
| -------- | -------- |
| 1. profil | Teljes importálás (csak előkészítés) |
| 2. profil | Teljes szinkronizálás |
| 3. profil | Különbözeti importálás (csak előkészítés) |
| 4. profil | Különbözeti szinkronizálás |
| 5. profil | Exportálás |

Futtatási profilok létrehozása a MIMMA összekötőhöz:

1. Nyissa meg a Synchronization Service Manager és az **eszközök** menüben kattintson a **felügyeleti ügynökök**elemre.

2. A **Management Agents** (Kezelőügynökök) listában válassza a **MIMMA** lehetőséget.

3. A Configure Run Profiles for (Futtatási profilok konfigurálása) párbeszédpanel megnyitásához válassza az**Actions** (Műveletek) menü **Configure Run Profiles** (Futtatási profilok konfigurálása) parancsát.

4. A táblázatban szereplő minden futtatási profilnál végezze el a következő lépéseket:

    - A Configure Run Profile (Futtatási profil konfigurálása) varázsló megnyitásához kattintson a **New Profile** (Új profil) lehetőségre.

    - A **Name** (Név) mezőbe írja be a profil nevét a táblázatból, majd kattintson a **Next** (Tovább) gombra.

    - A **Type** (Típus) listában válassza ki a lépés típusát a táblázatból, majd kattintson a **Next** (Tovább) gombra.

    - A futtatási profil létrehozásához kattintson a **Finish** (Befejezés) gombra.

5. Kattintson az **OK** gombra a Configure Run Profiles (Futtatási profilok konfigurálása) párbeszédpanel bezárásához.

## <a name="configure-the-mim-service"></a>A MIM szolgáltatás konfigurálása

Az AD által a MIM szolgáltatás esetében a felhasználók bejövő szinkronizálásához használt szabály a MIM-portál segítségével hozható létre.

Az AD felhasználókra vonatkozó bejövő szinkronizálási szabályának létrehozása:

1. A MIM-portál kezdőlapján a navigációs sávon kattintson az **Administration** (Adminisztráció) elemre.

2. A Synchronization Rules (Szinkronizálási szabályok) lap megnyitásához kattintson a **Synchronization Rules** (Szinkronizálási szabályok) lehetőségre.

3. A Create Synchronization Rule (Szinkronizálási szabály létrehozása) varázsló megnyitásához kattintson a **New** (Új) gombra az eszköztáron.

4. A **General** (Általános) lapon adja meg a következő információkat, majd kattintson a **Next** (Tovább) gombra:

    -   Display Name (Megjelenítendő név): AD felhasználói bejövő szinkronizálási szabály
    -   Data Flow Direction (Adatforgalom iránya): Inbound (Bejövő)

5. A **Scope** (Hatókör) lapon adja meg a következő információkat, majd kattintson a **Next** (Tovább) gombra:

    -   Metaverse Resource Type (Metaverzum erőforrástípusa): person (személy)
    -   External System (Külső rendszer): ADMA
    -   External System Resource Type (Külső rendszer erőforrástípusa): user (felhasználó)

6. A **Relationship** (Kapcsolat) lapon adja meg a következő információkat, majd kattintson a **Next** (Tovább) gombra:

    -   A Relationship Criteria (Kapcsolati feltételek) konfigurálásához válassza az **ObjectSID** elemet a MetaverseObject:person(Attribute) és a ConnectedSystemObject:person(Attribute) listából.

    -   Válassza a **Create Resource in FIM** (Erőforrás létrehozása a FIM-ben) lehetőséget.

7. A **Inbound Attribute Flow** (Bejövő attribútumfolyam) lapon adja meg a következő információkat, majd kattintson a **Next** (Tovább) gombra:

    | Folyamatszabály | Forrás | Cél |
    |-|-|-|
    |1. szabály|samAccountName|accountName|
    |2. szabály|displayName|displayName|
    |3. szabály|EmployeeType|employeeType|
    |4. szabály|givenName|firstName|
    |5. szabály|sn|lastName|
    |6. szabály|Manager|manager|
    |7. szabály|objectSID|ObjectSID|
    |8. szabály|"Contoso"|domain|

    A táblázatban minden egyes sorhoz kapcsolódóan végezze el a következő lépéseket:

    - Az Flow Definition (Adatfolyam definiálása) párbeszédpanelen kattintson a **New Attribute Flow** (Új attribútumfolyam) lehetőségre.

    - A **Source** (Forrás) lapon válassza a táblázatban az adott sorhoz feltüntetett attribútumot.

    - A **Destination** (Cél) lapon válassza a táblázatban az adott sorhoz feltüntetett attribútumot.

    - Az attribútumfolyam konfigurációjának alkalmazásához kattintson az **OK** gombra.

8. A **Summary** (Összegzés) lapon kattintson a **Submit** (Küldés) gombra.

## <a name="initialize-the-testing-environment"></a>A tesztkörnyezet inicializálása
A MIM-konfiguráció AD-ből származó adatokkal való teszteléséhez a következő négy lépést kell elvégeznie:

### <a name="enable-provisioning"></a>Kiépítés engedélyezése

1. Indítsa el a Synchronization Service Managert.

2. Válassza a **Tools** (Eszközök) menü **Options** (Beállítások) elemét az Options (Beállítások) párbeszédpanel megnyitásához.

3. Válassza az **Enable Synchronization Rule Provisioning** (Szinkronizálási szabályok létrehozásának engedélyezése) lehetőséget.

4. Kattintson az **OK** gombra az Options (Beállítások) párbeszédpanel bezárásához.

### <a name="initialize-the-mimma"></a>A MIMMA inicializálása

Futtasson egy teljes szinkronizálási ciklust ezen az összekötőn. A teljes ciklus a következő futtatási profilokból tevődik össze:

- Teljes importálás
- Teljes szinkronizálás
- Exportálás
- Különbözeti importálás

A négy futtatási profil végrehajtásához kövesse az alábbi lépéseket.

1. Indítsa el a Synchronization Service Managert, majd a **Tools** (Eszközök) menüben kattintson a **Management Agents** (Kezelőügynökök) elemre.

2. A **Management Agents** (Kezelőügynökök) listában válassza a **MIMMA** lehetőséget.

3. A Run Management Agent (Kezelőügynök futtatása) párbeszédpanel megnyitásához válassza az **Actions** (Műveletek) menü **Run** (Futtatás) parancsát.

4. A fent felsorolt minden egyes futtatási profilnál végezze el a következő lépéseket:

    - A Run Management Agent (Kezelőügynök futtatása) párbeszédpanel megnyitásához válassza az **Actions** (Műveletek) menü **Run** (Futtatás) parancsát.

    - A **Run profiles** (Futtatási profilok) listában jelölje ki a futtatni kívánt futtatási profilokat.

    - A futtatási profil elindításához kattintson az **OK** gombra.

#### <a name="configure-attribute-flow-precedence"></a>Az attribútumfolyam precedenciájának konfigurálása

A MIM-összekötő inicializálása során a rendszer a beállított szinkronizálási szabályokat beemelte a metaverzumba.

Úgy állítsa be az összekötő által szolgáltatott attribútumok attribútumfolyam-precedenciáját, hogy az AD már meglévő attribútumai bekerülhessenek a metaverzumba, később pedig a MIM szolgáltatás adatbázisába is.

### <a name="initialize-the-adma"></a>Az ADMA inicializálása

Az Active Directory-összekötő inicializálásához teljes importálást kell futtatnia, és teljes szinkronizálást kell végrehajtania az importált adatokon. A teljes importálás során az AD-ből a meglévő objektumokat a kapcsolódási térbe kerülnek. A teljes szinkronizálás során a szinkronizálási szabályok a MIM-összekötő szabályainak megfelelően módosulnak.

1. Indítsa el a Synchronization Service Managert, majd a **Tools** (Eszközök) menüben kattintson a **Management Agents** (Kezelőügynökök) elemre.

2. A **Management Agents** (Kezelőügynökök) listában válassza az **ADMA** lehetőséget.

3. A Run Management Agent (Kezelőügynök futtatása) párbeszédpanel megnyitásához válassza az **Actions** (Műveletek) menü **Run** (Futtatás) parancsát.

4. A fent felsorolt minden egyes futtatási profilnál végezze el a következő lépéseket:

    - A Run Management Agent (Kezelőügynök futtatása) párbeszédpanel megnyitásához válassza az **Actions** (Műveletek) menü **Run** (Futtatás) parancsát.

    - A **Run profiles** (Futtatási profilok) listában jelölje ki a futtatni kívánt futtatási profilokat.

    - A futtatási profil elindításához kattintson az **OK** gombra.

### <a name="populate-the-mim-service-database"></a>A MIM szolgáltatás adatbázisának feltöltése

A MIM szolgáltatás adatbázisának objektumokkal való feltöltéséhez szinkronizálási ciklust kell futtatnia a MIMMA összekötőn. Ez a ciklus a következő szakaszokból áll:

- Exportálás
- Teljes importálás
- Teljes szinkronizálás

A három futtatási profil végrehajtásához kövesse az alábbi lépéseket.

1. Indítsa el a Synchronization Service Managert, majd a **Tools** (Eszközök) menüben kattintson a **Management Agents** (Kezelőügynökök) elemre.

2. A **Management Agents** (Kezelőügynökök) listában válassza a **MIMMA** lehetőséget.

3. Válassza az **Actions** (Műveletek) menü **Run** (Futtatás) parancsát a Run Management Agent (Kezelőügynök futtatása) párbeszédpanel megnyitásához.

4. A fent felsorolt minden egyes futtatási profilnál végezze el a következő lépéseket:

    - Válassza az **Actions** (Műveletek) menü **Run** (Futtatás) parancsát a Run Management Agent (Kezelőügynök futtatása) párbeszédpanel megnyitásához.
    - A **Run profiles** (Futtatási profilok) listában jelölje ki a futtatni kívánt futtatási profilokat.
    - A futtatási profil elindításához kattintson az **OK** gombra.

> [!div class="step-by-step"]
> [« MIM szolgáltatás és -portál](install-mim-service-portal.md)
