---
title: Lotus Domino-összekötő | Microsoft Docs
description: Ez a cikk bemutatja, hogyan konfigurálhatja a Microsoft Lotus Domino-összekötőjét.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: e07fd469-d862-470f-a3c6-3ed2a8d745bf
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/05/2018
ms.author: billmath
ms.openlocfilehash: 46e80bce942b32bce7b7c3fb148973c249136b3f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760588"
---
# <a name="lotus-domino-connector-technical-reference"></a>Lotus Domino összekötő műszaki útmutatója
Ez a cikk a Lotus Domino-összekötőt ismerteti. A cikk a következő termékekre vonatkozik:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * A gyorsjavítási 4.1.3671.0 vagy újabb [KB3092178](https://support.microsoft.com/kb/3092178)kell használnia.

A MIM2016 és a FIM2010R2 esetében az összekötő letölthető a [Microsoft letöltőközpontból](https://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>A Lotus Domino-összekötő áttekintése
A Lotus Domino-összekötő lehetővé teszi a szinkronizációs szolgáltatás integrálását az IBM Lotus Domino-kiszolgálójával.

Magas szintű perspektívából a következő funkciókat támogatja az összekötő jelenlegi kiadása:

| Szolgáltatás | Támogatás |
| --- | --- |
| Csatlakoztatott adatforrás |Kiszolgáló: <li>Lotus Domino 8.5. x</li><li>Lotus Domino 9. x</li>Ügyfél:<li>Lotus Domino 8.5. x</li><li>Lotus Notes 9. x</li> |
| Forgatókönyvek |<li>Objektumok életciklusának kezelése</li><li>Csoportkezelés</li><li>Jelszókezelés</li> |
| Műveletek |<li>Teljes és különbözeti importálás</li><li>Exportálás</li><li>Jelszó beállítása és módosítása HTTP-jelszó esetén</li> |
| Séma |<li>Személy (barangoló felhasználó, kapcsolattartó (tanúsítvány nélküli személy))</li><li>Group</li><li>Erőforrás (erőforrás, hely, online értekezlet)</li><li>Levelezési adatbázis</li><li>A támogatott objektumok attribútumainak dinamikus felderítése</li><li>Akár 250 egyéni certifiers támogatása szervezet & szervezeti egységekkel (OU)</li> |

A Lotus Domino-összekötő a Lotus Notes-ügyfelet használja a Lotus Domino-kiszolgálóval való kommunikációhoz. Ennek a függőségnek a következményeként a szinkronizálási kiszolgálón telepíteni kell egy támogatott Lotus Notes-ügyfelet. Az ügyfél és a kiszolgáló közötti kommunikáció a Lotus Notes .NET együttműködési (Interop.domino.dll) felületen keresztül valósul meg. Ez az interfész elősegíti a Microsoft.NET platform és a Lotus Notes-ügyfél közötti kommunikációt, és támogatja a Lotus Domino-dokumentumokhoz és-nézetekhez való hozzáférést. A különbözeti importáláshoz az is lehetséges, hogy a C++ natív felülete is használatban van (a kiválasztott különbözeti importálási módszertől függően).

### <a name="prerequisites"></a>Előfeltételek
Az összekötő használata előtt győződjön meg arról, hogy rendelkezik a következő előfeltételekkel a szinkronizálási kiszolgálón:

* Microsoft .NET 4.5.2-keretrendszer vagy újabb
* A Lotus Notes-ügyfélnek telepítve kell lennie a szinkronizálási kiszolgálón.
* A Lotus Domino-összekötőhöz szükséges, hogy az alapértelmezett Lotus Domino LDAP-séma adatbázisa (Schema. nsf) legyen jelen a Domino-címtár kiszolgálóján. Ha nincs jelen, az LDAP szolgáltatás futtatásával vagy újraindításával telepítheti azt a Domino-kiszolgálón.

### <a name="connected-data-source-permissions"></a>Csatlakoztatott adatforrás engedélyei
A Lotus Domino-összekötő által támogatott feladatok végrehajtásához a következő csoportok tagjainak kell lennie:

* Teljes hozzáférésű rendszergazdák
* Rendszergazdák
* Adatbázis-rendszergazdák

A következő táblázat felsorolja az egyes műveletekhez szükséges engedélyeket:

| Művelet | Hozzáférési jogosultságok |
| --- | --- |
| Importálás |<li>Nyilvános dokumentumok beolvasása</li><li> Teljes hozzáférésű rendszergazda (ha a teljes hozzáférésű rendszergazdák csoport tagja, automatikusan hozzáférhet az ACL-hez.)</li> |
| Jelszó exportálása és beállítása |Érvényes hozzáférés: <li>Dokumentumok létrehozása</li><li>Dokumentumok törlése</li><li>Nyilvános dokumentumok beolvasása</li><li>Nyilvános dokumentumok írása</li><li>Dokumentumok replikálása vagy másolása</li>Az exportálási műveletekhez a következő szerepkörökre is szüksége lesz: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li> |

### <a name="direct-operations-and-adminp"></a>Közvetlen műveletek és AdminP
A műveletek közvetlenül a Domino könyvtárba vagy a AdminP folyamaton keresztül is megadhatók. Az alábbi táblázatok felsorolják az összes támogatott objektumot, műveletet és, ha alkalmazható, a kapcsolódó implementációs módszert:

**Elsődleges címjegyzék**

| Objektum | Létrehozás | Frissítés | Törlés |
| --- | --- | --- | --- |
| Személy |AdminP |Direct |AdminP |
| Group |AdminP |Direct |AdminP |
| MailInDB |Direct |Direct |Direct |
| Erőforrás |AdminP |Direct |AdminP |

**Másodlagos címjegyzék**

| Objektum | Létrehozás | Frissítés | Törlés |
| --- | --- | --- | --- |
| Személy |N/A |Direct |Direct |
| Group |Direct |Direct |Direct |
| MailInDB |Direct |Direct |Direct |
| Erőforrás |N.A. |N.A. |N.A. |

Egy erőforrás létrehozásakor létrejön egy Notes dokumentum. Hasonlóképpen, ha egy erőforrást törölnek, a rendszer törli a jegyzetek dokumentumát.

### <a name="ports-and-protocols"></a>Portok és protokollok
Az IBM Lotus Notes-ügyfél és a Domino-kiszolgálók a Notes távoli eljáráshívás (NRPC) használatával kommunikálnak, ahol a NRPC a TCP/IP protokollt kell használniuk. Az alapértelmezett portszám 1352, de a Domino rendszergazdája módosíthatja.

### <a name="not-supported"></a>Nem támogatott
A Lotus Domino-összekötő jelenlegi kiadása nem támogatja a következő műveleteket:

* Helyezze át a postaládákat a kiszolgálók között.

## <a name="create-a-new-connector"></a>Új összekötő létrehozása
### <a name="client-software-installation-and-configuration"></a>Ügyfélszoftverek telepítése és konfigurálása
A Lotus Notes szolgáltatást a kiszolgálóra kell telepíteni az összekötő telepítése **előtt** .

Ha telepíti, győződjön meg arról, hogy **egyetlen felhasználó telepítését** végzi. Az alapértelmezett **többfelhasználós telepítés** nem működik.  
![Notes1](./media/microsoft-identity-manager-2016-connector-domino/notes1.png)

A szolgáltatások lapon csak a szükséges Lotus Notes szolgáltatásokat és az **ügyfél-egyszeri bejelentkezést** telepítse. Egyszeri bejelentkezés szükséges ahhoz, hogy az összekötő be tudja jelentkezni a Domino-kiszolgálóra.  
![Notes2](./media/microsoft-identity-manager-2016-connector-domino/notes2.png)

**Megjegyzés:** Indítsa el a Lotus Notes szolgáltatást egyszer egy olyan felhasználóval, aki az összekötő szolgáltatásfiókként használt fiókkal azonos kiszolgálón található. Győződjön meg arról is, hogy a kiszolgálón a Lotus Notes-ügyfelet is be kell állítania. Nem futhat egyszerre, amíg az összekötő megpróbál csatlakozni a Domino-kiszolgálóhoz.

### <a name="create-connector"></a>Összekötő létrehozása
Lotus Domino-összekötő létrehozásához a **szinkronizációs szolgáltatásban** válassza a **felügyeleti ügynök** lehetőséget, és **hozzon létre** . Válassza ki a **Lotus Domino (Microsoft)** összekötőt.  
![CreateConnector](./media/microsoft-identity-manager-2016-connector-domino/createconnector.png)

Ha a szinkronizációs szolgáltatás verziója lehetővé teszi az **architektúra** konfigurálását, győződjön meg arról, hogy az összekötő az alapértelmezett értékre van beállítva a **folyamatban** való futtatáshoz.

### <a name="connectivity"></a>Kapcsolatok
A kapcsolat lapon meg kell adnia a Lotus Domino-kiszolgáló nevét, és meg kell adnia a bejelentkezési hitelesítő adatokat.  
![Kapcsolatok](./media/microsoft-identity-manager-2016-connector-domino/connectivity.png)

A Domino-kiszolgáló tulajdonság két formátumot támogat a kiszolgálónévhez:

* ServerName
* Kiszolgálónév/könyvtárnév

A **kiszolgálónév/könyvtárnév** formátum az attribútum előnyben részesített formátuma, mert gyorsabb választ nyújt, amikor az összekötő kapcsolatba lép a Domino-kiszolgálóval.

A megadott felhasználóazonosító-fájlt a szinkronizációs szolgáltatás konfigurációs adatbázisa tárolja.

A **különbözeti importáláshoz** a következő lehetőségek közül választhat:

* **Nincs** . Az összekötő nem végez különbözeti importálást.
* **Hozzáadás/frissítés** . Az összekötő módosítja a különbözeti importálási és frissítési műveleteket. Törlés esetén **teljes importálási** műveletre van szükség. Ez a művelet a .net-együttműködés használatát használja.
* **Hozzáadás/frissítés/törlés** . Az összekötő a különbözeti Importálás Hozzáadási, frissítési és törlési műveletét végzi. Ez a művelet a natív C++ felületeket használja.

A **séma beállításainál** a következő lehetőségek közül választhat:

* **Alapértelmezett séma** . Az összekötő észleli a sémát a Domino-kiszolgálóról. Ez az alapértelmezett beállítás.
* **DSML – séma** . Csak akkor használható, ha a Domino-kiszolgáló nem teszi elérhetővé a sémát. Ezután létrehozhat egy DSML-fájlt a sémával, és importálhatja helyette. További információ a DSML: [Oasis](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Ha a Tovább gombra kattint, a rendszer ellenőrzi a felhasználóazonosító és a jelszó konfigurációjának paramétereit.

### <a name="global-parameters"></a>Globális paraméterek
A globális paraméterek lapon konfigurálhatja az időzónát és az importálási és exportálási műveletet.  
![Globális paraméterek](./media/microsoft-identity-manager-2016-connector-domino/globalparameters.png)

A **Domino-kiszolgáló időzóna** -paramétere határozza meg a Domino-kiszolgáló helyét.

Ez a konfigurációs beállítás szükséges a **különbözeti importálási** műveletek támogatásához, mert lehetővé teszi, hogy a szinkronizálási szolgáltatás meghatározza az utolsó két importálás közötti változásokat.

> [!Note]
> A 2017. márciusi frissítéssel kezdődően a globális paraméterek képernyő tartalmazza azt a lehetőséget, amellyel törölheti a felhasználó levelezési adatbázisát a felhasználó törlése során.

![Felhasználó postaládájának törlése](./media/microsoft-identity-manager-2016-connector-domino/AdminP.png)

#### <a name="import-settings-method"></a>Beállítások importálása, metódus
A **teljes importálás** a következő beállításokkal végezhető el:

* Keresés
* Nézet (ajánlott)

A **Keresés** a Domino indexelését használja, de gyakori, hogy az indexek nem frissülnek valós időben, és a kiszolgáló által visszaadott adatok nem mindig helyesek. A sok változást tartalmazó rendszer esetén ez a lehetőség általában nem működik megfelelően, és bizonyos helyzetekben hamis törléseket biztosít. A **Keresés** azonban gyorsabb, mint a **Megtekintés** .

Az ajánlott lehetőség a **nézet** , mivel az adatok helyes állapotát biztosítja. A keresés valamivel lassabb.

#### <a name="creation-of-virtual-contact-objects"></a>Virtuális kapcsolattartási objektumok létrehozása
A **\_ Contact objektum létrehozásának engedélyezése** a következő beállításokkal rendelkezik:

* Nincsenek
* Nem hivatkozási értékek
* Hivatkozási és nem hivatkozási értékek

A Dominóban a hivatkozási attribútumok számos különböző formátumot tartalmazhatnak más objektumokra való hivatkozáshoz. Ahhoz, hogy a különböző változatokat lehessen képviselni, az összekötő a \_ kapcsolattartási objektumokat, más néven **virtuális névjegyeket** (VC) valósítja meg. Ezek az objektumok úgy jönnek létre, hogy csatlakozzanak a meglévő MV-objektumokhoz, vagy új objektumként legyenek kitervezve. Így az attribútumok hivatkozásai megmaradnak.

Ha engedélyezi ezt a beállítást, és ha egy hivatkozási attribútum tartalma nem DN formátumú, a rendszer egy \_ Contact objektumot hoz létre. Egy csoport tag attribútuma például tartalmazhat SMTP-címeket. Az is lehetséges, hogy Gazdagépbejegyzés és egyéb attribútumok találhatók a hivatkozás attribútumaiban. Ebben a forgatókönyvben válassza a **nem hivatkozási értékek** elemet. Ez a konfiguráció a dominó-implementációk leggyakoribb beállítása.

Ha a Lotus Domino úgy van konfigurálva, hogy a különböző megkülönböztető nevekkel rendelkező címeket külön címjegyzékekkel lássák el, akkor lehetséges, hogy a \_ címjegyzékben található összes viszonyítási értékhez kapcsolattartó objektumokat is létre kell hozni. Ehhez a forgatókönyvhöz válassza a **hivatkozási és a nem hivatkozási értékek** lehetőséget.

Ha a **FullName** attribútumban több érték szerepel, akkor a virtuális névjegyek létrehozását is engedélyezni szeretné, hogy a hivatkozások feloldhatók legyenek. Ennek az attribútumnak például több értéke lehet a házasság vagy a házasság felbontása után. Jelölje be az **Engedélyezés jelölőnégyzetet... A FullName több értékkel rendelkezik** ehhez a forgatókönyvhöz.

A megfelelő attribútumokhoz való csatlakozással a \_ kapcsolattartási objektumok az MV-objektumhoz lesznek csatlakoztatva.

Ezekhez az objektumokhoz VC = \_ Contact lett hozzáadva a DN-hez.

#### <a name="import-settings-conflict-object"></a>Importálási beállítások, ütközési objektum
**Ütközési objektum kizárása**

A nagy dominó-implementációban lehetséges, hogy több objektum ugyanazzal a megkülönböztető névvel rendelkezik a replikálási problémák miatt. Ezekben az esetekben az összekötő két olyan objektumot lát, amelyek eltérő UniversalIDs, de ugyanaz a DN. Ez az ütközés azt eredményezi, hogy az összekötő térben létrejön egy átmeneti objektum. Az összekötő figyelmen kívül hagyhatja azokat az objektumokat, amelyek replikációs áldozataiként vannak kijelölve a Dominóban. Ezt a jelölőnégyzetet érdemes megtartani.

#### <a name="export-settings"></a>Beállítások exportálása
Ha a **AdminP használata a hivatkozások frissítéséhez** lehetőség nincs kiválasztva, akkor a hivatkozási attribútumok (például a tag) exportálása közvetlen hívás, és nem használja a AdminP folyamatot. Csak akkor használja ezt a beállítást, ha a AdminP nincs konfigurálva a hivatkozási integritás fenntartásához.

#### <a name="routing-information"></a>Útválasztási információ
A Dominóban lehetséges, hogy egy Reference attribútum útválasztási információval rendelkezik, amely utótagként van beágyazva a DN-be. Egy csoport tag attribútuma például tartalmazhat <strong>CN = example/organization@ABC </strong>karaktert. Az utótag az @ABC útválasztási információ. Az útválasztási adatokat a Domino arra használja, hogy az e-maileket a megfelelő Domino-rendszerbe küldje, amely egy másik szervezet rendszere lehet. Az útválasztási információ mezőben megadhatja az összekötő hatókörében a szervezeten belül használt útválasztási utótagokat. Ha ezen értékek egyike utótagként található egy hivatkozási attribútumban, a rendszer eltávolítja az útválasztási adatokat a hivatkozásból. Ha a hivatkozási érték útválasztási utótagja nem egyezik meg a megadott értékek egyikével, akkor \_ létrejön egy Contact objektum. Ezeket a \_ kapcsolattartási objektumokat a rendszer a következővel hozza létre a DN-be: **ro = @ <RoutingSuffix>** . Ezekhez \_ a kapcsolattartási objektumokhoz a következő attribútumok is bekerülnek a valódi objektumba való csatlakozás engedélyezéséhez, ha szükséges: \_ routingName, \_ contactName, \_ DisplayName és UniversalID.

#### <a name="additional-address-books"></a>További címjegyzékek
Ha nincs telepítve a **címtár-segítségnyújtás** , amely a másodlagos címjegyzék nevét adja meg, akkor manuálisan is megadhatja ezeket a címjegyzékeket.

#### <a name="multivalued-transformation"></a>Többértékű átalakítás
A Lotus Domino számos attribútuma többértékű. A megfelelő metaverse-attribútumok általában egyetlen értékkel rendelkeznek. Az importálási és exportálási művelet konfigurálásával engedélyezheti, hogy az összekötő segítséget nyújtson az érintett attribútumok szükséges fordításához.

**Exportálás**  
Az exportálási művelet lehetőség két módot támogat:

* Elemek hozzáfűzése
* Elemek cseréje

**Elem cseréje** – ha ezt a beállítást választja, az összekötő mindig eltávolítja az attribútum aktuális értékeit a Dominóban, és lecseréli azokat a megadott értékekre. A megadott érték lehet egy vagy több értékű.

Példa: egy személy objektum Assistant attribútuma a következő értékekkel rendelkezik:

* CN = Greg Winston/OU = contoso/O = Americas, NAB = nevek. nsf
* CN = John Smith/OU = contoso/O = Americas, NAB = nevek. nsf

Ha egy **David Alexander** nevű új asszisztens van hozzárendelve ehhez a személy objektumhoz, az eredmény a következő:

* CN = David Alexander/OU = contoso/O = Americas, NAB = nevek. nsf

**Elem hozzáfűzése** – ha ezt a beállítást választja, az összekötő megőrzi a meglévő értékeket a Domino attribútumban, és beszúrja az új értékeket az Adatlista elejére.

Példa: egy személy objektum Assistant attribútuma a következő értékekkel rendelkezik:

* CN = Greg Winston/OU = contoso/O = Americas, NAB = nevek. nsf
* CN = John Smith/OU = contoso/O = Americas, NAB = nevek. nsf

Ha egy **David Alexander** nevű új asszisztens van hozzárendelve ehhez a személy objektumhoz, az eredmény a következő:

* CN = David Alexander/OU = contoso/O = Americas, NAB = nevek. nsf
* CN = Greg Winston/OU = contoso/O = Americas, NAB = nevek. nsf
* CN = John Smith/OU = contoso/O = Americas, NAB = nevek. nsf

**Importálás**  
Az importálási művelet lehetőség két módot támogat:

* Alapértelmezett
* Többértékű, egyetlen értékre

**Default (alapértelmezett** ) – az alapértelmezett beállítás kiválasztásakor a rendszer az összes attribútum összes értékét importálja.

**Többértékű – egyetlen érték** – ha ezt a beállítást választja, a rendszer egy többértékű attribútumot konvertál egyetlen értékű attribútumba. Ha egynél több érték létezik, a rendszer a felső (ez az érték általában a legújabb) értéket használja.

Példa: egy személy objektum Assistant attribútuma a következő értékekkel rendelkezik:

* CN = David Alexander/OU = contoso/O = Americas, NAB = nevek. nsf
* CN = Greg Winston/OU = contoso/O = Americas, NAB = nevek. nsf
* CN = John Smith/OU = contoso/O = Americas, NAB = nevek. nsf

Az attribútum legújabb frissítése **David Alexander** . Mivel az importálási művelet beállítás értéke többértékű az egyetlen értékre van állítva, az összekötő csak **David Alexander** -t importál az összekötő területére.

A többértékű attribútumok egyértékű attribútumokba való átalakításának logikája nem vonatkozik a csoporttag attribútumra és a személy FullName attribútumára.

Emellett konfigurálható a többértékű attribútumok importálási és exportálási szabályainak konfigurálása is a globális szabály alól kivételként. A beállítás konfigurálásához írja be a következőt: [objektumtípus]. [attributename] a **kizárási attribútumok importálása** és a **kizárási attribútumok exportálása** szövegmezőben. Ha például a person. Assistant értéket adja meg, és a globális jelző az összes érték importálása lehetőségre van beállítva, akkor a Segédnek csak az első érték lesz importálva.

#### <a name="certifiers"></a>Certifiers
Az összekötő felsorolja az összes szervezet/szervezeti egységet. Ahhoz, hogy a személy objektumokat exportálni lehessen az elsődleges címjegyzékbe, meg kell adni a jelszóval rendelkező tanúsító.

Ha minden certifiers ugyanazzal a jelszóval rendelkezik, akkor az **összes Certifers jelszava** használható. Ezután Itt adhatja meg a jelszót, és csak a tanúsító fájlt adja meg.

Ha csak az importálást végzi, nem kell megadnia semmilyen certifiers.

### <a name="configure-provisioning-hierarchy"></a>Kiépítési hierarchia konfigurálása
A Lotus Domino-összekötő konfigurálásakor ugorja át ezt a párbeszédpanelt. A Lotus Domino-összekötő nem támogatja a hierarchiák üzembe helyezését.  
![Kiépítési hierarchia](./media/microsoft-identity-manager-2016-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Partíciók és hierarchiák konfigurálása
A partíciók és a hierarchiák konfigurálásakor ki kell választania az "NAB = Names. nsf" nevű elsődleges címjegyzéket. Az elsődleges címjegyzék mellett kijelölhet másodlagos címjegyzékeket is, ha vannak ilyenek.  
![Partíciók](./media/microsoft-identity-manager-2016-connector-domino/partitions.png)

### <a name="select-attributes"></a>Attribútumok kiválasztása
Az attribútumok konfigurálásakor ki kell választania az összes olyan attribútumot, amely az **\_ MMS \_** előtaggal van ellátva. Ezek az attribútumok akkor szükségesek, ha új objektumokat hoz létre a Lotus Domino számára

![Jellemzők](./media/microsoft-identity-manager-2016-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Objektumok életciklusának kezelése
Ez a szakasz a dominó különböző objektumainak áttekintését tartalmazza.

### <a name="person-objects"></a>Személy objektumok
A személy objektum a szervezeten belüli és a szervezeti egységben lévő felhasználókat jelöli. Az alapértelmezett attribútumok mellett a Domino-rendszergazda egyéni attribútumokat adhat hozzá egy személy objektumhoz. Legalább egy személy objektumnak tartalmaznia kell az összes kötelező attribútumot. A kötelező attribútumok teljes listáját a [Lotus Notes tulajdonságai](#lotus-notes-properties)részben tekintheti meg. Egy személy objektum regisztrálásához a következő előfeltételek teljesülése szükséges:

* Meg kell határozni a címjegyzéket (Names. nsf), és az elsődleges címjegyzéknek kell lennie.
* Az adott felhasználó a szervezet/szervezet egységben való regisztrálásához rendelkeznie kell az O/OU tanúsító azonosítóval és a jelszóval.
* Meg kell adnia egy adott személy objektumhoz tartozó Lotus Notes-tulajdonságok adott készletét. Ezek a tulajdonságok a személy objektum kiépítési céljára szolgálnak. További információért lásd a jelen dokumentum a [Lotus Notes Properties](#lotus-notes-properties) című szakaszát.
* Egy személy kezdeti HTTP-jelszava egy attribútum, és a kiépítés során van beállítva.
* A person objektumnak a következő három támogatott típus egyikének kell lennie:
  1. Egy levelezési fájllal és egy felhasználói azonosító fájllal rendelkező normál felhasználó
  2. Barangoló felhasználó (egy normál felhasználó, amely tartalmazza az összes barangoló adatbázisfájlt)
  3. Névjegyek (azonosító fájl nélküli felhasználó)

Az \_ MMS IDRegType tulajdonság értéke által meghatározott személyek (kivéve a kapcsolattartókat) az USA-beli felhasználók és a nemzetközi felhasználók számára is csoportosíthatók \_ . Ezek a felhasználók a Notes-ügyfelet használják a Lotus Domino-kiszolgálókhoz való hozzáféréshez, Megjegyzés-azonosítóval és személy dokumentummal rendelkeznek. Ha a Notes-e-maileket használják, akkor egy levelezési fájllal is rendelkeznek. A felhasználónak regisztrálnia kell, hogy legyen aktív. További információkért lásd:

* [Megjegyzések felhasználóinak beállítása](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
* [Felhasználói regisztráció](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
* [Felhasználók kezelése](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
* [Felhasználók átnevezése](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_RENAMING_A_LOTUS_INOTES_USER_STEPS.html)

Ezek a műveletek a Lotus Domino-ben történnek, majd importálhatók a szinkronizációs szolgáltatásba.

### <a name="resources-and-rooms"></a>Erőforrások és szobák
Az erőforrás egy másik típusú adatbázis a Lotus Domino-ben. Az erőforrások különböző típusú berendezésekkel (például kivetítők) rendelkező konferenciatermek. Az erőforrástípus attribútum által definiált Lotus Domino-összekötő által támogatott erőforrások altípusai:

| Erőforrás típusa | Erőforrástípus-attribútum |
| --- | --- |
| Szoba |1 |
| Erőforrás (egyéb) |2 |
| Online értekezlet |3 |

Az erőforrás-objektum típusának működéséhez a következők szükségesek:

* Az erőforrás-foglalási adatbázisnak már léteznie kell a csatlakoztatott Domino-kiszolgálón.
* A hely már definiálva van az erőforráshoz.

Az erőforrás-foglalási adatbázis háromféle típusú dokumentumot tartalmaz:

* Hely profilja
* Erőforrás
* Foglalás

Az erőforrás-foglalási adatbázis beállításával kapcsolatos további információkért lásd: az erőforrás-foglalási [adatbázis beállítása](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Erőforrások létrehozása, frissítése és törlése**  
A létrehozási, frissítési és törlési műveleteket az erőforrás-foglalási adatbázisban a Lotus Domino-összekötő hajtja végre. Az erőforrások a nevek. nsf (azaz az elsődleges címjegyzék) dokumentumaiként jönnek létre. További információ az erőforrások szerkesztéséről és törléséről: [erőforrás-dokumentumok szerkesztése és törlése](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Erőforrások importálási és exportálási művelete**  
Az erőforrások importálhatók és exportálhatók a szinkronizálási szolgáltatásból, más típusú objektumokhoz hasonlóan. Válassza ki az objektum típusát erőforrásként a konfiguráció során. A sikeres exportálási művelethez meg kell egyeznie az erőforrás típusával, a konferencia-adatbázissal és a hely nevével.

### <a name="mail-in-databases"></a>Adatbázisok Mail-In
A Mail-In-adatbázis egy olyan adatbázis, amely e-mailek fogadására lett kialakítva. Ez egy olyan Lotus Domino-postaláda, amely nincs hozzárendelve egy adott Lotus Domino-felhasználói fiókhoz (azaz nem rendelkezik a saját azonosító fájljával és jelszavával). A levelezési adatbázisban egyedi felhasználóazonosító ("rövid név") van társítva, és a saját e-mail-címe.

Ha külön postaládára van szükség a saját e-mail-címükkel, amely a különböző felhasználók között megosztható (például group@contoso.com :), akkor a rendszer egy levelezési adatbázist hoz létre. A postaláda hozzáférése a Access Control listáján (ACL) keresztül történik, amely tartalmazza azoknak a megjegyzéseknek a nevét, akik számára engedélyezett a postaláda megnyitása.

A szükséges attribútumok listáját a cikk későbbi, [kötelező attribútumok](#mandatory-attributes) című szakaszában találja.

Ha egy adatbázis e-mail fogadására lett kialakítva, a rendszer egy Mail-In adatbázis-dokumentumot hoz létre a Lotus Domino-ben. Ennek a dokumentumnak léteznie kell minden olyan kiszolgáló Domino-könyvtárában, amely az adatbázis egy példányát tárolja. A levelezési adatbázis-dokumentum létrehozásával kapcsolatos részletes leírást a következő témakörben talál: [Mail-In adatbázis](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html)-dokumentum létrehozása.

Mail-In-adatbázis létrehozása előtt az adatbázisnak már léteznie kell (a Lotus admin-nek kell létrehoznia) a Domino-kiszolgálón.

### <a name="group-management"></a>Csoportkezelés
A Lotus Domino Group Management részletes áttekintését a következő forrásokból érheti el:

* [Csoportok használata](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
* [Csoport létrehozása](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
* [Csoportok létrehozása és módosítása](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
* [Csoportok kezelése](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
* [Csoport átnevezése](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Jelszókezelés
A regisztrált Lotus Domino-felhasználók esetében kétféle jelszó létezik:

1. Felhasználói jelszó (User.id-fájlban tárolt)
2. Internet/HTTP-jelszó

A Lotus Domino-összekötő csak a HTTP-jelszóval rendelkező műveleteket támogatja.

A jelszó-kezelés végrehajtásához engedélyeznie kell az összekötő jelszavas kezelését a felügyeleti ügynök Tervezőjében. A jelszó-kezelés engedélyezéséhez válassza a **jelszó-kezelés engedélyezése** lehetőséget a **bővítmények konfigurálása** párbeszédpanelen.  
![Bővítmények konfigurálása](./media/microsoft-identity-manager-2016-connector-domino/configureextensions.png)

A Lotus Domino-összekötő a következő műveleteket támogatja az internetes jelszóban:

* Jelszó beállítása: a jelszó beállítása új HTTP/Internet jelszót állít be a felhasználóhoz a Domino-ben. Alapértelmezés szerint a fiók is fel van oldva. A feloldási jelző megjelenik a Szinkronizáló motor WMI-felületén.
* Jelszó módosítása: ebben az esetben előfordulhat, hogy a felhasználó módosítani szeretné a jelszót, vagy a rendszer a megadott idő elteltével kéri a jelszó módosítására. Ahhoz, hogy a művelet elvégzése megtörténjen, mindkettő (a régi és az új jelszó) megadása kötelező. A módosítás után az új jelszó frissül a Lotus Domino-ben.

További információkért lásd:

* [Az Internet zárolási funkciójának használata](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
* [Internetes jelszavak kezelése](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Hivatkozási információk
Ez a szakasz felsorolja a Lotus Domino-összekötő attribútum-leírásait és attribútum-követelményeit.

### <a name="lotus-notes-properties"></a>Lotus-megjegyzések tulajdonságai
Amikor kiépíti a személy objektumait a Lotus Domino-címtárba, az objektumoknak a tulajdonságok meghatározott készletével kell rendelkezniük, amelyek meghatározott értékekkel vannak feltöltve. Ezek az értékek csak a létrehozási műveletekhez szükségesek.

A következő táblázat felsorolja ezeket a tulajdonságokat, és ismerteti azok leírását.

| Tulajdonság | Leírás |
| --- | --- |
| \_MMS_AltFullName |A felhasználó alternatív teljes neve. |
| \_MMS_AltFullNameLanguage |A felhasználó másodlagos teljes nevének megadásához használandó nyelv. |
| \_MMS_CertDaysToExpire |Az aktuális dátum és a tanúsítvány lejárata előtti napok száma. Ha nincs megadva, az alapértelmezett dátum az aktuális dátumtól számított két év. |
| \_MMS_Certifier |A tanúsító szervezeti hierarchiájának nevét tartalmazó tulajdonság. Például: OU = OrganizationUnit, O = org, C = ország. |
| \_MMS_IDPath |Ha a tulajdonság üres, akkor a rendszer nem hoz létre helyileg egy felhasználói azonosító fájlt a szinkronizálási kiszolgálón. Ha a tulajdonság fájlnevet tartalmaz, a rendszer létrehoz egy felhasználói azonosítót a madata mappában. A tulajdonság teljes elérési utat is tartalmazhat. |
| \_MMS_IDRegType |A személyeket kapcsolattartóként, USA-beli felhasználókként és nemzetközi felhasználóként lehet besorolni. A következő táblázat felsorolja a lehetséges értékeket: <li>0 – kapcsolat</li><li>1 – USA-beli felhasználó</li><li>2 – nemzetközi felhasználó</li> |
| \_MMS_IDStoreType |Az Amerikai Egyesült Államok és a nemzetközi felhasználók kötelező tulajdonsága. A tulajdonság egy egész értéket tartalmaz, amely megadja, hogy a rendszer a felhasználó azonosítóját mellékletként tárolja-e a jegyzetek címjegyzékében vagy a személy levelezési fájljában. Ha a felhasználói azonosító fájl mellékletként szerepel a címjegyzékben, akkor az MMS_IDPath használatával is létrehozható fájlként \_ . <li>Üres tároló-azonosító fájl az azonosító tárolóban, nincs azonosító fájl (a kapcsolatokhoz használatos).</li><li> 1 – melléklet a jegyzetek címjegyzékében. A \_ mellékleteket tartalmazó felhasználói azonosító fájlok esetében a MMS_Password tulajdonságot be kell állítani</li><li>2 – az áruházbeli azonosító a személy levelezési fájljában. A \_ MMS_UseAdminP false (hamis) értékre kell állítani, hogy a levelezési fájl a személy regisztrációja során legyen létrehozva. A \_ felhasználói azonosító fájlok esetében a MMS_Password tulajdonságot be kell állítani.</li> |
| \_MMS_MailQuotaSizeLimit |Az e-mail-fájl adatbázisához engedélyezett megabájtok száma. |
| \_MMS_MailQuotaWarningThreshold |A figyelmeztetés kiállítása előtt az e-mail fájl adatbázisa számára engedélyezett megabájtok száma. |
| \_MMS_MailTemplateName |A felhasználó e-mail fájljának létrehozásához használt e-mail sablon fájlja. Ha meg van adva egy sablon, a rendszer a megadott sablonnal hozza létre a levelezési fájlt. Ha nincs megadva sablon, a rendszer az alapértelmezett sablonfájl használatával hozza létre a fájlt. |
| \_MMS_OU |Opcionális tulajdonság, amely a szervezeti egység neve a tanúsító alatt. A tulajdonságnak üresnek kell lennie a névjegyeknél. |
| \_MMS_Password |A felhasználók kötelező tulajdonsága. A tulajdonság tartalmazza az objektum azonosító fájljának jelszavát. |
| \_MMS_UseAdminP |A tulajdonságot igaz értékre kell állítani, ha a levelezési fájlt a AdminP folyamat hozza létre a Domino-kiszolgálón (aszinkron módon az exportálási folyamatnak). Ha a tulajdonság értéke false (hamis), akkor a rendszer a Domino-felhasználóval hozza létre a levelezési fájlt (az Exportálás folyamatában szinkronban). |

Egy társított azonosító fájllal rendelkező felhasználó esetén a \_ MMS_Password tulajdonságnak tartalmaznia kell egy értéket. A Lotus Notes-ügyfélen keresztüli e-mail-hozzáféréshez a felhasználó MailServer és MailFile tulajdonságának tartalmaznia kell egy értéket.

Az e-mailek webböngészőn keresztüli eléréséhez a következő tulajdonságoknak értékeket kell tartalmazniuk:

* MailFile – kötelező tulajdonság, amely a levelezési fájlt tároló Lotus Domino-kiszolgáló elérési útját tartalmazza.
* MailServer – kötelező tulajdonság, amely a Lotus Domino-kiszolgáló nevét tartalmazza. Ez az érték a Lotus levelezési fájl Domino-kiszolgálón való létrehozásakor használandó név.
* HTTPPassword – opcionális tulajdonság, amely az objektum webes elérési jelszavát tartalmazza.

Ha a Domino-kiszolgálót levelezési képesség nélkül szeretné elérni, a HTTPPassword tulajdonságnak tartalmaznia kell egy értéket. A MailFile tulajdonság és a MailServer tulajdonság üres is lehet.

\_MMS_ IDStoreType = 2 (áruházbeli azonosító a levelezési fájlban) a NotesRegistrationclass MailSystem tulajdonsága REG_MAILSYSTEM_INOTES (3) értékre van állítva.

### <a name="mandatory-attributes"></a>Kötelező attribútumok
A Lotus Domino-összekötő elsősorban az alábbi típusú objektumokat támogatja (dokumentumtípusok):

* Group
* Adatbázis Mail-In
* Személy
* Kapcsolatfelvétel (tanúsító nélküli személy)
* Erőforrás

Ez a szakasz azokat az attribútumokat sorolja fel, amelyek az egyes támogatott objektumokhoz szükségesek, amelyeket egy Domino-kiszolgálóra kell exportálni.

| Objektumtípus | Kötelező attribútumok |
| --- | --- |
| Group |<li>ListName</li> |
| Adatbázis Main-In |<li>FullName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |
| Személy |<li>LastName</li><li>MailFile</li><li>Gazdagépbejegyzés</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li> |
| Kapcsolatfelvétel (tanúsító nélküli személy) |<li>\_MMS_IDRegType</li> |
| Erőforrás |<li>FullName</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Hely</li><li>DisplayName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |

## <a name="common-issues-and-questions"></a>Gyakori problémák és kérdések
### <a name="schema-detection-does-not-work"></a>A séma észlelése nem működik
Ahhoz, hogy a rendszer képes legyen a séma észlelésére, szükséges, hogy a Schema. nsf fájl megtalálható legyen a Domino-kiszolgálón. Ez a fájl csak akkor jelenik meg, ha az LDAP telepítve van a kiszolgálón. Ha a séma nem észlelhető, ellenőrizze a következőket:

* A (z) Schema. nsf nevű fájl megtalálható a Domino-kiszolgáló gyökérkönyvtárában
* A felhasználónak jogosultsága van a Schema. nsf fájl megtekintésére.
* Az LDAP-kiszolgáló újraindításának kényszerítése. Nyissa meg a **Lotus Domino-konzolt** , és az **LDAP ReloadSchema** utasítás használatával töltse be újra a sémát.

### <a name="not-all-secondary-address-books-are-visible"></a>Nem minden másodlagos címjegyzék látható
A Domino-összekötő a szolgáltatás **könyvtárának segítségére** támaszkodik a másodlagos címjegyzékek megtalálásához. Ha a másodlagos címjegyzékek hiányoznak, ellenőrizze, hogy engedélyezve van-e a [címtár-segítségnyújtás](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ABOUT_DIRECTORY_ASSISTANCE.html) , és hogy be van-e állítva a Domino-kiszolgálón.

### <a name="custom-attributes-in-domino"></a>Egyéni attribútumok a Domino-ben
A Domino számos módon bővíthető a sémában, így az összekötő egyéni attribútumként jelenik meg.

**1. módszer: Lotus Domino-séma kiterjesztése**

1. Hozzon létre egy másolatot a Domino címtár-sablonról ({PUBNAMES). NTF} ezeket a [lépéseket](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) követve (ne szabja testre az alapértelmezett IBM Lotus Domino-címtár sablonját):
2. Nyissa meg a Domino Directory-sablon ({CONTOSO) másolatát. A Domino Designerben létrehozott NTF}-sablon, és kövesse az alábbi lépéseket:
   * Kattintson a megosztott elemek elemre, és bontsa ki a segédűrlapok elemet
   * Kattintson duplán a $ {ObjectName} InheritableSchema segédűrlap (ahol {ObjectName} az alapértelmezett szerkezeti objektum osztály neve, például: person).
   * Nevezze el azt az attribútumot, amelyet hozzá szeretne adni a (z) {MyPersonAtrribute} sémához, és ennek megfelelő attribútumot kell megadni. Hozzon létre egy mezőt a **Létrehozás** menüben, majd válassza ki a **mező** lehetőséget a menüből.
   * A hozzáadott mezőben adja meg a tulajdonságait úgy, hogy a típust, a stílust, a méretet, a betűtípust és az egyéb kapcsolódó paramétereket kiválasztja a Tulajdonságok ablak mezőben.
   * Tartsa meg az attribútum alapértelmezett értékét az attribútumhoz megadott névvel (például ha az attribútum neve MyPersonAttribute, tartsa meg az alapértelmezett értéket ugyanazzal a névvel).
   * Mentse a $ {ObjectName} InheritableSchema segédűrlapot a frissített értékekkel.
3. Cserélje le a Domino Directory-sablont ({PUBNAMES). NTF} az új, testreszabott sablonnal {CONTOSO. NTF} ezeket a [lépéseket](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html)követve.
4. A dominó-rendszergazda bezárásához és a Domino-konzol megnyitásához indítsa újra az LDAP szolgáltatást, és töltse be újra az LDAP-sémát:
   * A Domino-konzolon szúrja be a parancsot a **Domino-parancs** szövege mezőbe, és indítsa újra az LDAP szolgáltatás- [Újraindítási feladat LDAP](https://www.ibm.com/support/knowledgecenter/SSKTMJ_10.0.1/admin/conf_startingandstoppingtheldapservice_c.html)-t.
   * Az LDAP-séma újratöltéséhez használja az LDAP-ReloadSchema
5. Nyissa meg a dominó-rendszergazdát, és válassza a személyek & csoportok fület, hogy megjelenjen a hozzáadott attribútum a Domino hozzáadása személyben.
6. Nyissa meg a Schema. nsf **fájlt a fájlok** lapról, és tekintse meg a hozzáadott attribútumot a dominoPerson LDAP Object osztályban.

**2. megközelítés: hozzon létre egy auxClass egyéni attribútummal, és társítsa az Object osztályhoz**

1. Hozzon létre egy másolatot a Domino címtár-sablonról ({PUBNAMES). NTF} ezeket a [lépéseket](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) követve (soha ne szabja testre az alapértelmezett IBM Lotus Domino Directory-sablont):
2. Nyissa meg a Domino Directory-sablon ({CONTOSO) másolatát. NTF} sablon, amely a Domino Designerben lett létrehozva.
3. A bal oldali ablaktáblán válassza a megosztott kód, majd a segédűrlapok lehetőséget.
4. Kattintson az új segédűrlap elemre.
5. Az új segédűrlap tulajdonságainak megadásához tegye a következőket:
   * Nyissa meg az új segédűrlapot, és válassza a kialakítás-segédűrlap tulajdonságai elemet.
   * A Name (név) tulajdonság mellett adja meg a kiegészítő objektum osztályának nevét (például TestSubform).
   * Tartsa meg a beállítások tulajdonságot "Belefoglalás segédűrlap beszúrása... párbeszédpanel kijelölve
   * Törölje a "leképezés továbbítása HTML-ben megjegyzésekben" lehetőséget.
   * Hagyja meg a többi tulajdonságot, és zárjuk be a segédűrlap tulajdonságai párbeszédpanelt.
   * Mentse és zárda be az új segédűrlapot.
6. Adja hozzá a következőt egy mező hozzáadásához a kiegészítő objektum osztályának definiálásához:
   * Nyissa meg a létrehozott segédűrlapot.
   * Válassza a létrehozás-mező lehetőséget.
   * A mező párbeszédpanel alapok lapján a név elem mellett adja meg a tetszőleges nevet (például: {MyPersonTestAttribute}).
   * A hozzáadott mezőben adja meg a tulajdonságait a típus, a stílus, a méret, a betűkészlet és a kapcsolódó tulajdonságok kiválasztásával.
   * Tartsa meg az attribútum alapértelmezett értékét az attribútumhoz megadott névvel (például ha az attribútum neve MyPersonTestAttribute, tartsa meg az alapértelmezett értéket ugyanazzal a névvel).
   * Mentse a segédűrlapot frissített értékekkel, és tegye a következőket:
     * A bal oldali ablaktáblán válassza a megosztott kód, majd a segédűrlapok lehetőséget.
     * Válassza ki az új segédűrlapot, és válassza a tervezés – kialakítás tulajdonságok elemet.
     * Kattintson a bal oldali harmadik lapra, és válassza a **tervezési módosítás tiltásának propagálása** lehetőséget.
7. Nyissa meg a $ {ObjectName} ExtensibleSchema segédűrlapot (ahol a (z) {ObjectName} az alapértelmezett szerkezeti objektum osztály neve, például – person).
8. Szúrja be az erőforrást, és válassza ki a segédűrlapot (például: TestSubform), és mentse a $ {ObjectName} ExtensibleSchema segédűrlapot.
9. Cserélje le a Domino Directory-sablont ({PUBNAMES). NTF} az új, testreszabott sablonnal {CONTOSO. NTF} ezeket a [lépéseket](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html)követve.
10. A dominó-rendszergazda bezárásához és a Domino-konzol megnyitásához indítsa újra az LDAP szolgáltatást, és töltse be újra az LDAP-sémát:
    * A Domino-konzolon szúrja be a parancsot a **Domino-parancs** szövege mezőbe, és indítsa újra az LDAP szolgáltatás- [Újraindítási feladat LDAP](https://www.ibm.com/support/knowledgecenter/SSKTMJ_10.0.1/admin/conf_startingandstoppingtheldapservice_c.html)-t.
    * Az LDAP-séma újratöltéséhez használja a Tell LDAP-parancsot az **LDAP ReloadSchema** .
11. Nyissa meg a dominó-rendszergazdát, és válassza a személyek & csoportok fület a hozzáadott attribútum megjelenítéséhez a Domino hozzáadása személy (egyebek) lapon.
12. Nyissa meg a Schema. nsf **fájlt a fájlok** lapról, és tekintse meg a hozzáadott attribútumot a TestSubform LDAP kiegészítő objektum osztály alatt.

**3. módszer: az egyéni attribútum hozzáadása a ExtensibleObject osztályhoz**

1. A gyökérkönyvtárba helyezett {Schema. nsf} fájl megnyitása
2. Válassza ki az LDAP-objektumosztály elemet a bal oldali menüben a **séma összes dokumentuma** területen, majd kattintson az **objektum osztály hozzáadása** gombra:
3. Adja meg az LDAP-nevet {zzzExtensibleSchema} formátumban (ahol a zzz az alapértelmezett szerkezeti objektum osztály neve, például személy). A (z) {PersonExtensibleSchema} LDAP-név megadásával például kiterjesztheti a séma a person objektum osztályához.
4. Adja meg a felettes objektumosztály nevét, amelyhez ki szeretné terjeszteni a sémát. Ha például ki szeretné terjeszteni a sémát a személy objektum osztályhoz, adja meg a (z) {dominoPerson} felettesi osztály nevét:
5. Adjon meg egy érvényes OID-t az Object osztálynak megfelelően.
6. A kötelező vagy opcionális attribútum típusok mezőben válassza ki a kiterjesztett/egyéni attribútumok elemet a követelmények szerint:
7. Miután hozzáadta a szükséges attribútumokat a ExtensibleObjectClass, kattintson a **mentés & Bezárás** gombra.
8. A rendszer létrehoz egy ExtensibleObjectClass a megfelelő alapértelmezett objektumosztály számára a kiterjesztett attribútumokkal.

## <a name="troubleshooting"></a>Hibaelhárítás
* További információ az összekötők hibakeresésének engedélyezéséről: [ETW-nyomkövetés engedélyezése összekötők számára](https://go.microsoft.com/fwlink/?LinkId=335731).
