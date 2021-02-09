---
title: PowerShell-összekötő | Microsoft Docs
description: Ez a cikk bemutatja, hogyan konfigurálhatja a Microsoft Windows PowerShell-összekötőjét.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6dba8e34-a874-4ff0-90bc-bd2b0a4199b5
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/02/2018
ms.author: billmath
ms.openlocfilehash: 0dcba300f70756dbfa7a29011a37839247e6bf8a
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835930"
---
# <a name="windows-powershell-connector-technical-reference"></a>Műszaki útmutató a Windows PowerShell-összekötőhöz
Ez a cikk a Windows PowerShell-összekötőt ismerteti. A cikk a következő termékekre vonatkozik:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * A gyorsjavítási 4.1.3671.0 vagy újabb [KB3092178](https://support.microsoft.com/kb/3092178)kell használnia.

A MIM2016 és a FIM2010R2 esetében az összekötő letölthető a [Microsoft letöltőközpontból](https://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>A PowerShell-összekötő áttekintése
A PowerShell-összekötő lehetővé teszi a szinkronizációs szolgáltatás integrálását olyan külső rendszerekkel, amelyek Windows PowerShell-alapú API-kat kínálnak. Az összekötő hidat biztosít a híváson alapuló bővíthető connectivity Management agent 2 (ECMA2) keretrendszer és a Windows PowerShell képességei között. További információ a ECMA-keretrendszerről: [bővíthető kapcsolat 2,2 felügyeleti ügynök referenciája](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Előfeltételek
Az összekötő használata előtt győződjön meg arról, hogy rendelkezik az alábbiakkal a szinkronizálási kiszolgálón:

* Microsoft .NET 4.5.2-keretrendszer vagy újabb
* Windows PowerShell 2,0, 3,0 vagy 4,0

A szinkronizálási szolgáltatás kiszolgálóján a végrehajtási házirendet úgy kell konfigurálni, hogy engedélyezze az összekötő számára a Windows PowerShell-parancsfájlok futtatását. Ha az összekötő által futtatott parancsfájlok digitálisan alá vannak írva, a következő parancs futtatásával konfigurálja a végrehajtási házirendet:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Új összekötő létrehozása
Windows PowerShell-összekötőnek a szinkronizációs szolgáltatásban való létrehozásához meg kell adnia egy olyan Windows PowerShell-parancsfájlt, amely végrehajtja a szinkronizálási szolgáltatás által kért lépéseket. Attól függően, hogy melyik adatforráshoz kapcsolódik, és milyen funkciókra van szükség, a szükséges parancsfájlok eltérőek lehetnek. Ez a szakasz az egyes megvalósítható parancsfájlokat és azok szükségességét ismerteti.

A Windows PowerShell-összekötő úgy van kialakítva, hogy az egyes parancsfájlokat a szinkronizációs szolgáltatás adatbázisában tárolja. Habár a fájlrendszerben tárolt parancsfájlok futtathatók, könnyebben beszúrhatja az egyes parancsfájlok törzsét közvetlenül az összekötő konfigurációjához.

PowerShell-összekötő létrehozásához a **szinkronizációs szolgáltatásban** válassza a **felügyeleti ügynök** lehetőséget, és **hozzon létre**. Válassza ki a **PowerShell-(Microsoft-)** összekötőt.

![Összekötő létrehozása](./media/microsoft-identity-manager-2016-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Kapcsolatok
Adja meg a távoli rendszerhez való csatlakozáshoz szükséges konfigurációs paramétereket. Ezeket az értékeket a szinkronizációs szolgáltatás biztonságosan tárolja, és a Windows PowerShell-parancsfájlok számára elérhetővé tette az összekötő futtatásakor.

![Kapcsolatok](./media/microsoft-identity-manager-2016-connector-powershell/connectivity.png)

A következő kapcsolódási paramétereket konfigurálhatja:

**Kapcsolódás**


|              Paraméter               | Alapértelmezett érték |                                                                                                                                                                                                                  Cél                                                                                                                                                                                                                   |
|--------------------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                Kiszolgáló                |    <Blank>    |                                                                                                                                                                                             A kiszolgáló neve, amelyhez az összekötőnek csatlakoznia kell.                                                                                                                                                                                              |
|                Tartomány                |    <Blank>    |                                                                                                                                                                                    Az összekötő futtatásakor használni kívánt hitelesítő adat tartománya.                                                                                                                                                                                    |
|                 User                 |    <Blank>    |                                                                                                                                                                                   Az összekötő futtatásakor használandó hitelesítő adatok felhasználóneve.                                                                                                                                                                                   |
|               Jelszó               |    <Blank>    |                                                                                                                                                                                   Az összekötő futtatásakor használandó hitelesítő adat jelszava.                                                                                                                                                                                   |
|    Összekötő-fiók megszemélyesítése     |     Hamis     | Igaz értéke esetén a szinkronizációs szolgáltatás a megadott hitelesítő adatok kontextusában futtatja a Windows PowerShell-parancsfájlokat. Ha lehetséges, ajánlott, hogy a rendszer a **$credentials** paramétert adja át az egyes parancsfájloknak megszemélyesítés helyett. A beállítás használatához szükséges további engedélyekkel kapcsolatos további információkért tekintse meg a [megszemélyesítés további konfigurálását](#additional-configuration-for-impersonation)ismertető témakört. |
| Felhasználói profil betöltése megszemélyesítéskor |     Hamis     |                          Arra utasítja a Windowsot, hogy a megszemélyesítés során betöltse az összekötő hitelesítő adatainak felhasználói profilját. Ha a megszemélyesített felhasználó barangoló profillal rendelkezik, az összekötő nem tölti be a barangoló profilt. A paraméter használatához szükséges további engedélyekkel kapcsolatos további információkért tekintse meg a [megszemélyesítés további konfigurációját](#additional-configuration-for-impersonation).                           |
|    Bejelentkezési típus megszemélyesítéskor     |     Nincs      |                                                                                                                                                                      Bejelentkezési típus megszemélyesítés közben. További információkért tekintse meg a [dwLogonType][dw] dokumentációját.                                                                                                                                                                       |
|         Csak aláírt parancsfájlok          |     Hamis     |                                                                                                    Ha az értéke igaz, a Windows PowerShell-összekötő ellenőrzi, hogy az egyes parancsfájlok érvényes digitális aláírással rendelkeznek-e. Ha hamis, győződjön meg arról, hogy a szinkronizálási szolgáltatás kiszolgálójának Windows PowerShell-végrehajtási házirendje RemoteSigned vagy nem korlátozott.                                                                                                     |

**Általános modul**  
Az összekötő lehetővé teszi, hogy egy megosztott Windows PowerShell-modult tároljon a konfigurációban. Ha az összekötő parancsfájlt futtat, a rendszer kinyeri a Windows PowerShell-modult a fájlrendszerbe, hogy az egyes parancsfájlok is importálhatók legyenek.

Az importálási, exportálási és jelszó-szinkronizálási parancsfájlok esetében az általános modul az összekötő MAData mappájába lesz kibontva. A séma, az érvényesítés, a hierarchia és a partíciós felderítési parancsfájlok esetében az általános modult a rendszer kinyeri a (z)% TEMP% mappába. Mindkét esetben a kinyert általános modul szkriptje a közös modul-parancsfájl neve beállításnak megfelelően van elnevezve.

A FIMPowerShellConnectorModule. psm1 nevű modulnak a MAData mappából való betöltéséhez használja a következő utasítást: `Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

A FIMPowerShellConnectorModule. psm1 nevű modulnak a% TEMP% mappából való betöltéséhez használja a következő utasítást: `Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Paraméter ellenőrzése**  
Az érvényesítési parancsfájl egy opcionális Windows PowerShell-parancsfájl, amely a rendszergazda által megadott összekötő-konfigurációs paraméterek érvényességének biztosítására használható. A kiszolgáló, a kapcsolati hitelesítő adatok és a kapcsolati paraméterek ellenőrzése az érvényesítési parancsfájl általános használata. Az érvényesítési parancsfájlt a következő lapok és párbeszédpanelek módosítása után hívja meg a rendszer:

* Kapcsolatok
* Globális paraméterek
* Partíció konfigurációja

Az érvényesítési parancsfájl a következő paramétereket fogadja el az összekötőtől:

| Name | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameterPage |[ConfigParameterPage][cpp] |Az érvényesítési kérelmet kiváltó konfiguráció lap vagy párbeszédpanel. |
| ConfigParameters |[KeyedCollection][keyk] [karakterlánc, [ConfigParameter][cp]] |Az összekötő konfigurációs paramétereinek táblázata. |
| Hitelesítő adat |[PSCredential][pscred] |A rendszergazda által a kapcsolat lapon megadott hitelesítő adatokat tartalmazza. |

Az érvényesítési parancsfájlnak egyetlen ParameterValidationResult objektumot kell visszaadnia a folyamatnak.

**Séma felderítése**  
A séma-felderítési parancsfájl kötelező. Ez a szkript visszaadja azokat az objektumtípust, attribútumokat és attribútum-korlátozásokat, amelyeket a szinkronizálási szolgáltatás az attribútumok szabályainak konfigurálásakor használ. A séma-felderítési parancsfájl az összekötő létrehozásakor fut, és feltölti az összekötő sémáját. Ezt a séma frissítése művelet is használja a Synchronization Service Managerban.

A séma-felderítési parancsfájl a következő paramétereket fogadja el az összekötőtől:

| Name | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk] [karakterlánc, [ConfigParameter][cp]] |Az összekötő konfigurációs paramétereinek táblázata. |
| Hitelesítő adat |[PSCredential][pscred] |A rendszergazda által a kapcsolat lapon megadott hitelesítő adatokat tartalmazza. |

A szkriptnek egyetlen [Schema][schema] objektumot kell visszaadnia a folyamatnak. A Schema objektum [SchemaType][schemaT] objektumokból áll (például felhasználók és csoportok). A SchemaType objektum [SchemaAttribute][schemaA] objektumok gyűjteményét tartalmazza, amelyek a típus attribútumait jelölik (például: Utónév, vezetéknév és postai cím).

**További paraméterek**  
A szokásos konfigurációs beállítások mellett további egyéni konfigurációs beállításokat is megadhat, amelyek az összekötő példányára vonatkoznak. Ezek a paraméterek az összekötő, partíció vagy futtatási lépés szintjén adhatók meg, és a megfelelő Windows PowerShell-parancsfájlból érhetők el. Az egyéni konfigurációs beállítások egyszerű szöveges formátumban tárolhatók a szinkronizációs szolgáltatás adatbázisaiban, vagy titkosítva lehetnek. A szinkronizálási szolgáltatás szükség esetén automatikusan titkosítja és visszafejti a biztonságos konfigurációs beállításokat.

Egyéni konfigurációs beállítások megadásához az egyes paraméterek nevét vesszővel (,) válassza el.

Ha egyéni konfigurációs beállításokat szeretne elérni egy parancsfájlból, a nevet aláhúzással ( \_ ) és a paraméter (globális, partíció vagy RunStep) hatókörével kell elvégeznie. A globális FileName paraméter eléréséhez például használja ezt a kódrészletet: `$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Képességek
A felügyeleti ügynök Designer képességek lapja határozza meg az összekötő működését és működését. Az ezen a lapon megadott beállítások nem módosíthatók az összekötő létrehozásakor. Ez a táblázat a képességek beállításait sorolja fel.

![Képességek](./media/microsoft-identity-manager-2016-connector-powershell/capabilities.png)

| Képesség | Leírás |
| --- | --- |
| [Megkülönböztető név stílusa][dnstyle] |Azt jelzi, hogy az összekötő támogatja-e a megkülönböztető neveket, és ha igen, milyen stílust. |
| [Exportálás típusa][exportT] |Meghatározza az exportálási parancsfájlban megjelenített objektumok típusát. <li>AttributeReplace – az attribútum megváltozásakor a többértékű attribútumok teljes készletét tartalmazza.</li><li>AttributeUpdate – csak a többértékű attribútumok változásait tartalmazza, ha az attribútum megváltozik.</li><li>MultivaluedReferenceAttributeUpdate – a nem hivatkozható többértékű attribútumok értékeinek teljes készletét tartalmazza, és csak a többértékű hivatkozási attribútumok különbözeteit.</li><li>ObjectReplace – egy objektum összes attribútumát tartalmazza, ha bármely attribútum megváltozik</li> |
| [Az adatnormalizálás][DataNorm] |Arra utasítja a szinkronizációs szolgáltatást, hogy normalizálja a horgony attribútumokat, mielőtt azok a parancsfájlok számára elérhetővé válnak. |
| [Objektum megerősítése][oconf] |A szinkronizálási szolgáltatás függőben lévő importálási viselkedését konfigurálja. <li>Normál – az alapértelmezett viselkedés, amely az összes exportált módosítást megerősíti az importáláson keresztül</li><li>NoDeleteConfirmation – Ha töröl egy objektumot, a rendszer nem hoz létre függőben lévő importálást.</li><li>NoAddAndDeleteConfirmation – egy objektum létrehozásakor vagy törlésekor nincs függőben lévő importálás.</li> |
| DN használata horgonyként |Ha a megkülönböztető név stílusa LDAP-értékre van beállítva, az összekötő területének Anchor attribútuma szintén a megkülönböztető név. |
| Több összekötő egyidejű műveletei |Ha be van jelölve, több Windows PowerShell-összekötő futhat egyszerre. |
| Partíciók |Ha be van jelölve, az összekötő több partíciót és partíciós felderítést is támogat. |
| Hierarchia |Ha be van jelölve, az összekötő támogatja az LDAP-stílusú hierarchikus struktúra használatát. |
| Importálás engedélyezése |Ha be van jelölve, az összekötő az importálási parancsfájlok használatával importálja az adatmennyiséget. |
| Különbözeti importálás engedélyezése |Ha be van jelölve, az összekötő eltéréseket igényelhet az importálási parancsfájloktól. |
| Exportálás engedélyezése |Ha be van jelölve, az összekötő exportált parancsfájlok használatával exportálja az adatexportálást. |
| Teljes Exportálás engedélyezése |Ha be van jelölve, az exportálási parancsfájlok támogatják a teljes összekötő terület exportálását. Ha ezt a beállítást szeretné használni, akkor az Exportálás engedélyezése jelölőnégyzetet is be kell jelölni. |
| Nincsenek hivatkozási értékek az első exportálási menetben |Ha be van jelölve, a hivatkozási attribútumok egy második exportálási fázisban lesznek exportálva. |
| Objektum átnevezésének engedélyezése |Ha be van jelölve, a megkülönböztető nevek módosíthatók. |
| Delete-Add csereként |Ha be van jelölve, a törlés-hozzáadási műveletek egyetlen csereként lesznek exportálva. |
| Jelszavas műveletek engedélyezése |Ha be van jelölve, a jelszó-szinkronizálási parancsfájlok támogatottak. |
| Jelszó exportálásának engedélyezése az első menetben |Ha be van jelölve, a rendszer a kiépítés során beállított jelszavakat exportálja az objektum létrehozásakor. |

### <a name="global-parameters"></a>Globális paraméterek
A felügyeleti ügynök Designer globális paraméterek lapja lehetővé teszi az összekötő által futtatott Windows PowerShell-parancsfájlok konfigurálását. A kapcsolat lapon definiált egyéni konfigurációs beállítások globális értékeit is megadhatja.

**Partíció felderítése**  
A partíció egy különálló névtér egy megosztott sémán belül. Például Active Directory minden tartomány egy erdőben található partíció. A partíció az importálási és exportálási műveletek logikai csoportosítása. Az Importálás és az Exportálás környezetként, az összes művelet pedig ebben a környezetben történik. A partícióknak az LDAP-hierarchiát kellene képviselnie. Az importálás során a partíció megkülönböztető nevét kell használni annak ellenőrzéséhez, hogy az összes visszaadott objektum egy partíció hatókörén belül van-e. A partíció megkülönböztető nevét a metaverse-ből az összekötő területére való kiépítés során is felhasználjuk annak meghatározásához, hogy egy objektumnak az exportálás során társítania kell-e a partíciót.

A partíció-felderítési parancsfájl a következő paramétereket fogadja el az összekötőtől:

| Name | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][karakterlánc, [ConfigParameter][cp]] |Az összekötő konfigurációs paramétereinek táblázata. |
| Hitelesítő adat |[PSCredential][pscred] |A rendszergazda által a kapcsolat lapon megadott hitelesítő adatokat tartalmazza. |

A szkriptnek egy vagy [több partíciós objektumot,][part] vagy egy [T] listát kell visszaadnia a folyamatnak.

**Hierarchia felderítése**  
A hierarchia-felderítési parancsfájlt csak akkor használja a rendszer, ha a megkülönböztető név stílusának tulajdonsága LDAP. A parancsfájl segítségével megkeresheti és kiválaszthatja azokat a tárolókat, amelyek az importálási és exportálási műveletek hatókörében vagy kívül vannak betekintve. A parancsfájlnak csak azoknak a csomópontoknak a listáját kell megadnia, amelyek a parancsfájlhoz megadott legfelső szintű csomópont közvetlen gyermekei.

A hierarchia-felderítési parancsfájl a következő paramétereket fogadja el az összekötőtől:

| Name | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][karakterlánc, [ConfigParameter][cp]] |Az összekötő konfigurációs paramétereinek táblázata. |
| Hitelesítő adat |[PSCredential][pscred] |A rendszergazda által a kapcsolat lapon megadott hitelesítő adatokat tartalmazza. |
| ParentNode |[HierarchyNode][hn] |A hierarchia legfelső szintű csomópontja, amely alatt a parancsfájlnak közvetlen gyermekeket kell visszaadnia. |

A szkriptnek egy egygyermekes HierarchyNode objektumot, vagy a gyermek HierarchyNode-objektum [T] listáját kell visszaadnia a folyamatnak.

#### <a name="import"></a>Importálás
Az importálási műveletet támogató összekötőknek három parancsfájlt kell alkalmazniuk.

**Importálás megkezdése**  
Az importálás megkezdése parancsfájl az importálás futtatása lépés elején fut. Ebben a lépésben kapcsolatot létesíthet a forrásrendszer-kapcsolattal, és elvégezheti az előkészítési lépéseket az adatok a csatlakoztatott rendszerből való importálása előtt.

Az importálás megkezdése parancsfájl a következő paramétereket fogadja el az összekötőtől:

| Name | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][karakterlánc, [ConfigParameter][cp]] |Az összekötő konfigurációs paramétereinek táblázata. |
| Hitelesítő adat |[PSCredential][pscred] |A rendszergazda által a kapcsolat lapon megadott hitelesítő adatokat tartalmazza. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Az importálási Futtatás (delta vagy teljes), a partíció, a hierarchia, a vízjel és a várt oldalméret típusáról tájékoztatja a parancsfájlt. |
| Típusok |[Séma][schema] |Az importált összekötő-terület sémája. |

A parancsfájlnak egyetlen [OpenImportConnectionResults][oicres] -objektumot kell visszaadnia a folyamathoz, például: `Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Adatok importálása**  
Az Adatimportálási parancsfájlt az összekötő hívja meg, amíg a parancsfájl nem jelzi, hogy nincs több importálandó információ. A Windows PowerShell-összekötőnek van 9 999 objektum mérete. Ha a parancsfájl több mint 9 999 objektumot ad vissza az importáláshoz, támogatnia kell a lapozást. Az összekötő egy egyéni adattulajdonságot tesz elérhetővé, amelyet egy vízjelek tárolására használhat, így az Adatimportálási parancsfájl minden alkalommal meghívja a parancsfájlt, így a szkript folytatja az importálást.

Az adatok importálása parancsfájl a következő paramétereket fogadja az összekötőtől:

| Name | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][karakterlánc, [ConfigParameter][cp]] |Az összekötő konfigurációs paramétereinek táblázata. |
| Hitelesítő adat |[PSCredential][pscred] |A rendszergazda által a kapcsolat lapon megadott hitelesítő adatokat tartalmazza. |
| GetImportEntriesRunStep |[ImportRunStep][irs] |A lapozható importálások és a különbözeti importálások során használható vízjelet (CustomData) tartalmazza. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Az importálási Futtatás (delta vagy teljes), a partíció, a hierarchia, a vízjel és a várt oldalméret típusáról tájékoztatja a parancsfájlt. |
| Típusok |[Séma][schema] |Az importált összekötő-terület sémája. |

Az Adatimportálási parancsfájlnak [[CSEntryChange][csec]] objektumot kell írnia a folyamatba. Ez a gyűjtemény olyan CSEntryChange-attribútumokból áll, amelyek az egyes importált objektumokat jelölik. A teljes importálási művelet során ennek a gyűjteménynek teljes CSEntryChange-objektummal kell rendelkeznie, amely minden objektumhoz rendelkezik minden attribútummal. A különbözeti importálás során a CSEntryChange objektumnak tartalmaznia kell az egyes importálandó objektumok attribútum szintjének különbözetét, vagy a módosult objektumok teljes megjelenítését (csere mód).

**Importálás vége**  
Az importálási Futtatás befejezésekor a rendszer futtatja a befejezési importálási parancsfájlt. A szkriptnek el kell végeznie a szükséges karbantartási feladatokat (például a rendszerekkel való kapcsolat bezárásával és a hibákra való reagálással).

A befejezési importálási parancsfájl a következő paramétereket fogadja el az összekötőből:

| Name | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][karakterlánc, [ConfigParameter][cp]] |Az összekötő konfigurációs paramétereinek táblázata. |
| Hitelesítő adat |[PSCredential][pscred] |A rendszergazda által a kapcsolat lapon megadott hitelesítő adatokat tartalmazza. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Az importálási Futtatás (delta vagy teljes), a partíció, a hierarchia, a vízjel és a várt oldalméret típusáról tájékoztatja a parancsfájlt. |
| CloseImportConnectionRunStep |[CloseImportConnectionRunStep][cecrs] |Az importálás befejezésének okát értesíti a parancsfájlról. |

A parancsfájlnak egyetlen [CloseImportConnectionResults][cicres] -objektumot kell visszaadnia a folyamathoz, például: `Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Exportálás
Az összekötő importálási architektúrájának megegyeznek az exportálást támogató összekötőknek három parancsfájlt kell alkalmazniuk.

**Exportálás megkezdése**  
Az exportálás megkezdése parancsfájl az Exportálás futtatása lépés elején fut. Ebben a lépésben kapcsolatot létesíthet a forrás-rendszerrel, és elvégezheti az előkészítő lépéseket, mielőtt az adatexportálást a csatlakoztatott rendszerbe exportálja.

Az exportálás megkezdése parancsfájl a következő paramétereket fogadja el az összekötőtől:

| Name | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][karakterlánc, [ConfigParameter][cp]] |Az összekötő konfigurációs paramétereinek táblázata. |
| Hitelesítő adat |[PSCredential][pscred] |A rendszergazda által a kapcsolat lapon megadott hitelesítő adatokat tartalmazza. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Az exportálási Futtatás (delta vagy teljes), a partíció, a hierarchia és a várt oldalméret típusáról tájékoztatja a parancsfájlt. |
| Típusok |[Séma][schema] |Az exportált összekötő területének sémája. |

A parancsfájl nem adhat vissza kimenetet a folyamatnak.

**Adatexportálás**  
A szinkronizálási szolgáltatás az adatexportálási parancsfájlt az összes függőben lévő exportálás feldolgozásához szükséges többször hívja meg. Ha az összekötő területének több függőben lévő exportálása van, mint az összekötő oldalméret, az adatexportálási parancsfájl többször is hívható, és valószínűleg többször is ugyanarra az objektumra.

Az adatexportálási parancsfájl a következő paramétereket fogadja el az összekötőből:

| Name | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][karakterlánc, [ConfigParameter][cp]] |Az összekötő konfigurációs paramétereinek táblázata. |
| Hitelesítő adat |[PSCredential][pscred] |A rendszergazda által a kapcsolat lapon megadott hitelesítő adatokat tartalmazza. |
| CSEntries |IList[CSEntryChange][csec] |Az összes, függőben lévő exportálással rendelkező összekötő-terület objektumainak listája a továbbítás során. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Az exportálási Futtatás (delta vagy teljes), a partíció, a hierarchia és a várt oldalméret típusáról tájékoztatja a parancsfájlt. |
| Típusok |[Séma][schema] |Az exportált összekötő területének sémája. |

Az adatexportálási parancsfájlnak egy [PutExportEntriesResults][peeres] objektumot kell visszaadnia a folyamatnak. Az objektumnak nem kell tartalmaznia az egyes exportált összekötők eredmény-információit, kivéve ha a hiba vagy a horgony attribútum módosítása történik. Ha például egy PutExportEntriesResults objektumot szeretne visszaadni a folyamatnak: `Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Exportálás befejezése**  
Az Exportálás futtatásának befejezésekor az exportálási parancsfájl futtatásának vége. A szkriptnek el kell végeznie a szükséges karbantartási feladatokat (például a rendszerekkel való kapcsolat bezárásával és a hibákra való reagálással).

A záró exportálási parancsfájl a következő paramétereket fogadja el az összekötőtől:

| Name | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][karakterlánc, [ConfigParameter][cp]] |Az összekötő konfigurációs paramétereinek táblázata. |
| Hitelesítő adat |[PSCredential][pscred] |A rendszergazda által a kapcsolat lapon megadott hitelesítő adatokat tartalmazza. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Az exportálási Futtatás (delta vagy teljes), a partíció, a hierarchia és a várt oldalméret típusáról tájékoztatja a parancsfájlt. |
| CloseExportConnectionRunStep |[CloseExportConnectionRunStep][cecrs] |Az Exportálás befejezésének okát értesíti a parancsfájlról. |

A parancsfájl nem adhat vissza kimenetet a folyamatnak.

#### <a name="password-synchronization"></a>Jelszó-szinkronizálás
A Windows PowerShell-összekötők a jelszó módosításainak és alaphelyzetbe állításának céljaként használhatók.

A jelszó-parancsfájl a következő paramétereket fogadja el az összekötőtől:

| Name | Adattípus | Leírás |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][karakterlánc, [ConfigParameter][cp]] |Az összekötő konfigurációs paramétereinek táblázata. |
| Hitelesítő adat |[PSCredential][pscred] |A rendszergazda által a kapcsolat lapon megadott hitelesítő adatokat tartalmazza. |
| Partíció |[Partíció][part] |A CSEntry tartalmazó címtárpartíció. |
| CSEntry |[CSEntry][cse] |A jelszó módosítását vagy alaphelyzetbe állítását kérő objektumhoz tartozó összekötő-terület bejegyzése. |
| OperationType |Sztring |Azt jelzi, hogy a művelet egy alaphelyzetbe állítás (**SetPassword**) vagy egy módosítás (**ChangePassword**). |
| PasswordOptions |[PasswordOptions][pwdopt] |A jelszó-visszaállítás kívánt viselkedését megadó jelzők. Ez a paraméter csak akkor érhető el, ha a OperationType **SetPassword**. |
| OldPassword |Sztring |Az objektum régi jelszavával lett feltöltve a jelszó módosításához. Ez a paraméter csak akkor érhető el, ha a OperationType **ChangePassword**. |
| ÚjJelszó |Sztring |Az objektum új jelszavával van feltöltve, amelyet a parancsfájlnak meg kell határoznia. |

A jelszó-parancsfájl nem várt eredményt ad vissza a Windows PowerShell-folyamatnak. Ha hiba történik a jelszó-parancsfájlban, a parancsfájlnak az alábbi kivételek egyikét kell kidobnia, hogy tájékoztassa a szinkronizálási szolgáltatást a problémáról:

* [PasswordPolicyViolationException][pwdex1] – akkor dobták, ha a jelszó nem felel meg a csatlakoztatott rendszer jelszóházirend-szabályzatának.
* [PasswordIllFormedException][pwdex2] – akkor dobták, ha a jelszó nem fogadható el a csatlakoztatott rendszer számára.
* [PasswordExtension][pwdex3] – a jelszó-parancsfájlban található összes egyéb hiba miatt eldobott.

## <a name="sample-connectors"></a>Minta összekötők
Az elérhető minta-összekötők teljes áttekintését lásd: [Windows PowerShell-összekötő minta-összekötő gyűjteménye][samp].

## <a name="other-notes"></a>Egyéb megjegyzések
### <a name="additional-configuration-for-impersonation"></a>További konfiguráció a megszemélyesítéshez
Adja meg azt a felhasználót, aki a következő engedélyeket megszemélyesíti a szinkronizációs szolgáltatás kiszolgálóján:

Olvasási hozzáférés a következő beállításkulcsok számára:

* HKEY_USERS \\ [SynchronizationServiceServiceAccountSID] \Software\Microsoft\PowerShell
* HKEY_USERS \\ [SynchronizationServiceServiceAccountSID] \Environment

A szinkronizálási szolgáltatás szolgáltatásfiók biztonsági azonosítójának (SID) meghatározásához futtassa a következő PowerShell-parancsokat:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Olvasási hozzáférés a következő fájlrendszer-mappákhoz:

* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\Extensions
* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\ExtensionsCache
* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\MaData \\ {ConnectorName}

Helyettesítse be a Windows PowerShell-összekötő nevét a (z) {ConnectorName} helyőrzőhöz.

## <a name="troubleshooting"></a>Hibaelhárítás
* További információ az összekötők hibakeresésének engedélyezéséről: [ETW-nyomkövetés engedélyezése összekötők számára](https://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: https://go.microsoft.com/fwlink/?LinkId=394291
