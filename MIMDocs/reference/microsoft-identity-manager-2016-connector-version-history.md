---
title: Az összekötő verziójának kiadási előzményei | Microsoft Docs
description: Ez a témakör felsorolja a Forefront Identity Manager (FIM) és a Microsoft Identity Manager összekötők összes kiadását
services: active-directory
documentationcenter: ''
author: EugeneSergeev
manager: aashiman
editor: ''
reviewer: markwahl-msft
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/11/2020
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ba69b18f3712384da79095d625eb9008a07b741e
ms.sourcegitcommit: dae61d97c9db5402d35e2757a1ce844d16236032
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/12/2020
ms.locfileid: "94532120"
---
# <a name="connector-version-release-history"></a>Összekötő verziókiadásai

Az összekötők összekapcsolják az adott csatlakoztatott adatforrásokat a Microsoft Identity Manager (webszolgáltatások) és a Azure AD Connect. (A Forefront Identity Managerben az összekötők felügyeleti ügynökökként ismertek.) Számos összekötő, például összekötők, amelyek a felhasználók Active Directoryba való kiépítésére szolgálnak, a fakiszolgálói szinkronizációs szolgáltatás telepítése és a Azure AD Connect telepítési csomagja részeként jelennek meg. Emellett további összekötők, például a harmadik féltől származó címtárszolgáltatások is külön letöltés alatt állnak, így gyakrabban frissíthetők, hogy támogatást adjanak a harmadik féltől származó, a külső gyártók által készített rendszerekhez való csatlakozáshoz.  

> [!NOTE]
> Ez a témakör elsősorban a FIM-és a bekapcsolási összekötők esetében érhető el. Az alábbi összekötők nem támogatottak az Azure AD Connect-on való telepítéskor. A megadott buildre való frissítéskor a rendszer előre telepíti a kiadott összekötőket Azure AD Connect.


Ez a témakör felsorolja az általános összekötők csomag összes olyan verzióját, amely külön lett kiadva a webszolgáltatásból.  A következő helyen található összekötők listáját tekintheti meg: [támogatott összekötők a webszolgáltatások 2016 SP1 verziójában](../supported-management-agents.md).  Néhány partner ilyen módon hozta létre a saját összekötőit, és a wiki FIM 2010 és a Webáruház [2016: felügyeleti ügynökök](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-mim-2016-management-agents-from-partners.aspx)teljes listája elérhető a partnerektől.


Kapcsolódó hivatkozások:

* [Legújabb összekötők letöltése](https://go.microsoft.com/fwlink/?LinkId=717495)
* [Általános LDAP-összekötő](microsoft-identity-manager-2016-connector-genericldap.md) referenciájának dokumentációja
* [Általános SQL-összekötő](microsoft-identity-manager-2016-connector-genericsql.md) dokumentációja
* A [Web Services-összekötő](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) referenciájának dokumentációja
* A [PowerShell-összekötő](microsoft-identity-manager-2016-connector-powershell.md) referenciájának dokumentációja
* A [Lotus Domino-összekötő](microsoft-identity-manager-2016-connector-domino.md) referenciájának dokumentációja
* A [SharePoint felhasználói profil tároló-összekötő](https://go.microsoft.com/fwlink/?LinkID=331344) referenciájának dokumentációja

## <a name="1113460-november-2020"></a>1.1.1346.0 (2020 november)
### <a name="fixed-issues"></a>Megoldott problémák
- Gráf-összekötő
  - Kijavítva egy probléma a helyi összekötő gyorsítótárának sérülése miatt, ami a különbözeti importálási hibákat okozta
  - Az összekötő által jelentett, a teljes importálás során felderítési hibákat okozó duplikált bejegyzések hibája javítva
  - Kijavítva az összetett adattípusok helytelen importálásával kapcsolatos hibát, például *employeeOrgData*
- Általános SQL-összekötő
  - Hiba történt az SQL natív hitelesítési hibája miatt, mert a DSN kapcsolódási karakterlánc tulajdonsága *TrustedConnection* *hamis* értékre van állítva. 
- Általános LDAP-összekötő
  - Kijavítva a *OpenLDAP* *accessLog* bejegyzéseinek a különbözeti importáláson való feldolgozásával kapcsolatos hibát, ami helytelen csoporttagság-változásokat és egyéb hibákat okoz

## <a name="1113020-september-2020"></a>1.1.1302.0 (szeptember 2020)
### <a name="fixed-issues"></a>Megoldott problémák
- Gráf-összekötő
  - A */Beta* -végpont sémájának frissítésével kapcsolatos probléma javítva

## <a name="1113010-august-2020"></a>1.1.1301.0 (2020. augusztus)
### <a name="fixed-issues"></a>Megoldott problémák
- Gráf-összekötő
  - Kijavított egy hibát az importálási hibák miatt, ami az üres *UserType* attribútum értéke okozta.
  - Hiba történt a különbözeti importálási hibák esetében, mert a felderítési hibák miatt nem olvashatók be az összekötő gyorsítótára
  - A szolgáltatás és a proxy által jelentett időtúllépések automatikusan újrapróbálkoznak
  - A */Beta* -végponthoz tartozó séma-felderítés frissítve 
### <a name="enhancements"></a>Fejlesztések
- Gráf-összekötő
  - Az egyéni Directory-bővítmény attribútumaihoz tartozó értékek olvasására és írására vonatkozó támogatás hozzáadva
  - A */Beta* -végpontban a csoportok *egyszerű* tagjainak olvasására vonatkozó támogatás hozzáadva 
  - A séma-felderítéshez kapcsolódó különbözeti importálási futtatások teljesítményének fejlesztése
  - A Graph-összekötő most meghívhatja a külső tagok felhasználóit

  > [!NOTE]
  > Ha már használja a vendég meghívást az összekötő 1.1.1170.0, frissítse a szinkronizálási szabályokat a következő logikával:

  - Kimenő folyamatok
    - A rendszer meghívja a felhasználót, amikor exportálja a felhasználó létrehozását, és az Exportálás tartalmaz egy *mail* attribútumot, de nem *userPrincipalName attribútumot*.  Ha a *userPrincipalName* meg van adva, a rendszer a meghívása helyett felhasználót hoz létre.
    - A *UserType* attribútum csak azt határozza meg, hogy egy felhasználó *tagja* vagy *vendég* lesz-e (alapértelmezés szerint *tag* , ha nincs beállítva)
  - Bejövő folyamatok
    - A külső felhasználók *userPrincipalName* a "as-is" értéket jeleníti meg

## <a name="1111700-april-2020"></a>1.1.1170.0 (2020. április)
### <a name="fixed-issues"></a>Megoldott problémák
- Általános SQL-összekötő
   - Hiba javítva a lekérdezési alapú exportálási stratégiával és a többértékű attribútumok frissítéseivel
- Lotus Notes-összekötő
   - A másodlagos megjegyzések címeiből származó csoportokat a *AdminP* folyamat már nem törli. A közvetlen törlési művelet most már használatban van
- Általános LDAP-összekötő
   - Kijavított egy hibát az LDAP-címtár műveleti attribútumaival, például a *pwdUpdateTime* , nem látható a sémában
### <a name="enhancements"></a>Fejlesztések
- Gráf-összekötő   
   - A külső vendég felhasználók UPN-je már nem jelenik meg, hanem az összekötő térben látható az e-mailek megjelenítéséhez
   - A B2B vendég felhasználók üzembe helyezésének támogatása

   A B2B vendég felhasználóinak meghívásához a következőket kell tennie:
   - Engedélyek megadására a Graph connectorhoz társított Azure AD-alkalmazáshoz
   - Összekötő-konfiguráció befejezése szakasz a külső felhasználók meghívásához: a meghívó átirányítási URL-címének (kötelező) beállítása, valamint a meghívó e-mailek küldésének kiválasztása
   - Kötelező attribútumok beállítása a kimenő szinkronizálási szabályban:
     - "Vendég" => *userType* (csak kezdeti folyamat)
     - külső e-mail-cím => *userPrincipalName*
     - CustomExpression ("CN =" + csObjectID + ", OBJECT = user") => *DN* (csak kezdeti folyamat)
     - csObjectID => *azonosítója* (csak kezdeti folyamat)

## <a name="1111300-february-2020"></a>1.1.1130.0 (február 2020)
### <a name="fixed-issues"></a>Megoldott problémák
- Gráf-összekötő
   - Az Exportálás már nem sikerül, amikor egy tagot egy már hozzáadott csoportba próbál hozzáadni
   - a "export_password" virtuális attribútum támogatása javítva lett, nem kell beállítani a "password" attribútumot többé
   - Többértékű karakterlánc-attribútumok rögzített exportálási folyamata
   - A különbözeti importálást érintő számos hiba javítva
   - Különböző lehetséges datetime formátumú hibák kijavítva
- Általános SQL-összekötő
   - Különböző felhasználói felületi hibák javítva
   - Rögzített helytelen hivatkozások a különbözeti importálás során
   - Az SQL Change Tracking különbözeti importálási stratégiával és a többértékű táblázatokkal kapcsolatos hiba javítva a csoporttagság-változások helyes importálását
   - Kijavítva egy olyan hibát, amelynek attribútum-értékei nem törlődnek az exportáláskor
   - Javítva lett egy hiba, amely a többértékű Reference attribútum utolsó elemét nem törli az exportáláskor
   - Kijavítva egy hiba a séma frissítésével, ami a hivatkozási attribútumokat karakterláncokra állítja
   - Kijavítva egy hiba a tárolt eljárás paramétereinek értékeit 397 bájtra csonkítva
   - Kijavított egy hibát az Oracle Tables és a views séma észlelése kis-és nagybetűk megkülönböztetése
- Lotus Notes-összekötő
   - Teljesítmény javult a csoporttagok importálásakor
   - A teljes importálás már nem sikerül "NULL hivatkozási hibákkal".
   - Hiba javítva a megjegyzések postaládájának törlésével, ha az ACL be van állítva
   - Az üres csoportok nevei már nem okozzák a különbözeti importálási hibákat
   - A karakterlánc-értékek törlése után nem nyomtatható karaktereket tartalmazó hiba javítva
- Általános LDAP-összekötő
   - A különbözeti importálás már nem jelenik meg a "Replace" literális érték, ha nincs megadva érték a forrás changelog
### <a name="enhancements"></a>Fejlesztések
- Gráf-összekötő
   - A szuverén felhők támogatása és a bejelentkezés és a Graph erőforrás URL-címeinek konfigurálása
   - A rendszer kiszűri és elrejti a nem támogatott attribútumokat az összekötő tulajdonságaiban a séma felderítése után

## <a name="4418001-july-2019"></a>4.4.1800.1 (2019. július)
### <a name="enhancements"></a>Fejlesztések
- SharePoint felhasználói profil tároló-összekötő
   - A SharePoint Server 2019 támogatása. Az összekötő letölthető a [Microsoft letöltőközpontból](https://go.microsoft.com/fwlink/?linkid=279713) 

## <a name="119530-june-2019"></a>1.1.953.0 (2019. június)

### <a name="fixed-issues"></a>Kijavított problémák:
- Általános LDAP-összekötő
   - A különbözeti importálás már nem sikerül, ha a változások mező üres az Oracle Internet Directory szolgáltatásban
- Általános SQL-összekötő
   - A StartIndex és a EndIndex paraméterekkel rendelkező egyéni SQL-lekérdezési importálási stratégiával kapcsolatos probléma kijavítva, ami csak az első oldalt importálja.
   - Hiba történt a tábla/nézet importálási stratégiájában az MS SQL-ből való olvasáskor, ami csak az első oldal importálását eredményezi.
- Gráf összekötő:
   - A szervezeti névjegyek mostantól megfelelően kezelhetők, az e-mailek már nem hiányoznak
   - Kijavított egy problémát a Manager-attribútum importálási és exportálási műveleteivel kapcsolatban. Az összekötő már nem sikerül, ha a kezelő üres. A kezelő értéke megfelelően frissül az Azure AD-ben
   - Az üres értékek különbözeti importálásával kapcsolatos probléma javítva
   - Az összekötő összeomlásával kapcsolatos hiba javítva a különbözeti importálás során, ha a helyi gyorsítótár fájlja sérült
   - Az üres értékek helytelen exportálásával vagy csak az attribútum értékének megváltozásával kapcsolatos hibák javítva
   - Az összekötő mostantól kezeli a törlés/hozzáadás műveletet ugyanahhoz az attribútumhoz az Exportálás futtatása során.
   - Számos, a hosszan futó importálással és exportálással kapcsolatos probléma javítva, ha a hozzáférési jogkivonatok az összekötő futtatása során lejárnak. Az összekötő most megújítja a hozzáférési jogkivonatokat, ha a hiba nem szükséges
   - A csoport utolsó tagjával kapcsolatos probléma kijavítva
### <a name="enhancements"></a>Fejlesztések
- Általános SQL-összekötő
   - az Adatolvasó commandTimeout paraméterének beállítása az összekötő időtúllépésének megfeleltetésére van beállítva. Ha hosszú ideig futó lekérdezésekkel rendelkezik, és 30 másodpercnél hosszabb időt vesz igénybe, megnövelheti a "parancs időtúllépése" paraméter értékét a "kapcsolat" szakaszban.
- Gráf összekötő: 
   - A többszálas csoporttagság teljes importálási stratégiája hozzáadva az importálási teljesítmény javításához. A különbözeti importálás egyszálas művelet marad.
   - Az összetett sémák által támogatott olyan attribútumok támogatása, mint például a OnPremisesExtentionAttributes. * jelenleg elérhető
   - A export_password attribútum támogatása az exportálás – nem újraimportálható hibák elkerülése érdekében, és az összekötő terület kezdeti jelszavának mellőzése. A viselkedés hasonló a többi ECMA2 összekötőhöz
   - Felvettek egy kezelőt a HTTP-kérelmek szabályozásának támogatásához. Ha az Azure AD-replika túl sok kérést kap egy ügyféltől, akkor a Retry-After utasítással is reagálhat. Az összekötő szünetelteti és újrapróbálkozik a meghibásodása helyett
   - A különbözeti importálási profil már nem indul el, ha a lekérdezési szűrők meg vannak határozva. Ha csak bizonyos objektumokat szeretne importálni az Azure AD-ből, például ha A vezetékneve * karakterrel kezdődik, akkor a különbözeti importálási funkció le lesz tiltva.


## <a name="119130-january-2019"></a>1.1.913.0 \( január 2019\)

### <a name="fixed-issues"></a>Kijavított problémák:

* Általános SQL:
    * Kijavított egy regressziós hibát az általános SQL-összekötőn, ahol csak az első 5000 objektumot olvasta.
* Gráf összekötő:
    * Kijavítva egy probléma, amelyben az Graph API-összekötő nem tudott beolvasni/írni egy bérlőből, vagy új összekötőt létrehozni, amikor a bétaverzió lehetőséget választotta a kapcsolódáskor.

### <a name="enhancements"></a>Fejlesztések

* Gráf összekötő:
    * Állítsa be a TLS 1,2 protokollt a Graph-összekötő alapértelmezett értékére.
* Általános SQL-összekötő:
    * Az általános SQL-összekötőt az Oracle 12c és az Oracle 18c tesztelte.

## <a name="119110"></a>1.1.911.0

> [!NOTE]  
> Ez az összekötő-összeállítási verzió csak Azure AD Connect üzemelő példányokon belüli használatra használható, és nem biztosítható a kiszolgált webszolgáltatásokhoz.
> 
> Az előző összekötő kiadásával összehasonlítva a rendszer nem tartalmaz fejlesztési vagy frissítési lehetőséget a felhasználók számára.

-  Frissíti a nem szabványos összekötőket (például az általános LDAP-összekötőt és az általános SQL-összekötőt) Azure AD Connect.  

## <a name="118610"></a>1.1.861.0

### <a name="fixed-issues"></a>Kijavított problémák:
* Az összes összekötő beállítás címeinek módosítása a "Forefront to Microsoft" értékre

* Lotus-megjegyzések: 
    * A COM-ban található hibák a Lotus szolgáltatásban időnként nem adnak vissza hibát
* Általános LDAP:
    * A Gldap extra szimbóluma a PING Directory Serverrel
* Általános webszolgáltatások:
    * Hiba történt a JSON-elemzés exportálásakor
* Általános SQL:
    * Bináris attribútum exportálása
    * Az Objektumtípusok nem lehetnek alsztringek
    * A többértékű tábla módosításait a "különbözeti Importálás" művelet nem követi nyomon, ha a "Delta-stratégia" a "Change Tracking"
* Graph-összekötő (nyilvános előzetes verzió)
    * Hiba a csoport törlésekor
    * User-Agent frissítése a http-fejlécre
    * Az összekötő nem ellenőrzi az ügyfél-azonosítót és az ügyfél titkos kulcsát
    * A bérlő nevét be kell vágni

### <a name="enhancements"></a>Fejlesztések
* Lotus Notes: * az időtúllépés növelésének lehetősége a felhasználói felületen
* A Graph Connector (nyilvános előzetes verzió) * Password attribútum az importálás során szűrve az "exportálás nem újraimportálása" lehetőséggel.
    * A (z) $filter lekérdezési paraméter támogatása – a különbözeti lekérdezésben működő összes szűrővel végzett műveletekhez csak a (z) [here](https://developer.microsoft.com/en-us/graph/docs/concepts/paging)

## <a name="118300"></a>1.1.830.0

### <a name="fixed-issues"></a>Kijavított problémák:
* Megoldott ConnectorsLog System. Diagnostics. EventLogInternal. InternalWriteEvent (üzenet: a rendszerhez csatlakoztatott egyik eszköz nem működik)
* Az összekötők ezen kiadásában frissítenie kell a 3.3.0.0-4.1.3.0 és a 4.1.4.0 közötti kötési átirányítást miiserver.exe.config
* Általános webszolgáltatások:
    * A rendszer nem tudja menteni a konfigurációs eszközben a megoldott érvényes JSON-választ
* Általános SQL:
    * Az Exportálás mindig csak a törlési műveletre vonatkozó frissítési lekérdezést hozza létre. Hozzáadás a törlő lekérdezés létrehozásához
    * Az SQL-lekérdezés, amely a különbözeti importálás működéséhez szükséges objektumokat kéri le, ha a "különbözeti stratégia" a "Change Tracking" lett javítva. Ebben a megvalósításban ismert korlátozásban: a különbözeti Importálás "Change Tracking" módban nem követi a többértékű attribútumok változásait
    * Lehetőség van arra, hogy törlési lekérdezést állítson elő az esethez, amikor törölni kell a többértékű attribútum utolsó értékét, és ez a sor nem tartalmaz más adatmennyiséget, kivéve az értéket, amelyet törölni kell.
    * System. ArgumentException kezelése, ha az SP által megvalósított kimeneti paramétereket
    * Helytelen lekérdezés a varbinary (max) típussal rendelkező mezőbe való exportálás műveletének végrehajtásához.
    * A parameterList változóval kapcsolatos probléma kétszer lett inicializálva (a functions ExportAttributes és a GetQueryForMultiValue)


## <a name="116490-aadconnect-116490"></a>1.1.649.0 (AADConnect 1.1.649.0)

### <a name="fixed-issues"></a>Kijavított problémák:

* Lotus-megjegyzések:
  * Egyéni certifiers-beállítás szűrése
  * A ImportOperations osztály importálása rögzítette a "views" (nézetek) módban futtatható műveletek definícióját, és a "keresés" módban.
* Általános LDAP:
  * A OpenLDAP könyvtár DN-t használ a entryUUI helyett. A GLDAP-összekötő új beállítása, amely lehetővé teszi a horgony módosítását
* Általános SQL:
  * Varbinary (max) típusú, rögzített exportálás a mezőbe.
  * Amikor bináris adatforrást ad hozzá egy adatforrásból a CSEntry objektumhoz, a DataTypeConversion függvény nulla bájton meghiúsult. A CSEntryOperationBase osztály rögzített DataTypeConversion funkciója.




### <a name="enhancements"></a>Fejlesztések

* Általános SQL:
  * A "globális paraméterek" lapon az általános SQL Management-ügynök konfigurációs ablakában konfigurálhatja a módot, hogy az elnevezett paraméterekkel rendelkező tárolt eljárást végrehajtsa. A "globális paraméterek" lapon a "nevesített paraméterek használata egy tárolt eljárás végrehajtásához" címkével rendelkezik, amely felelős az elnevezett paraméterekkel rendelkező tárolt eljárás végrehajtásának módjáért.
    * Jelenleg az elnevezett paraméterekkel rendelkező tárolt eljárás végrehajtása csak az IBM DB2 és MSSQL adatbázisok esetében működik. Az Oracle és a MySQL adatbázisai esetében ez a megközelítés nem működik: 
      * A MySQL SQL-szintaxisa nem támogatja a tárolt eljárások elnevezett paramétereit.
      * Az Oracle-hez készült ODBC-illesztő nem támogatja a nevesített paraméterek elnevezett paramétereit a tárolt eljárásokban.

## <a name="116040-aadconnect-116140"></a>1.1.604.0 (AADConnect 1.1.614.0)


### <a name="fixed-issues"></a>Kijavított problémák:

* Általános webszolgáltatások:
  * Kijavított egy problémát, amely megakadályozza egy SOAP-projekt létrehozását, ha két vagy több végpont is volt.
* Általános SQL:
  * Az importálási művelet során a GSQL nem lett megfelelően átalakítva, ha az összekötő területére ment. A GSQL összekötő területének alapértelmezett dátum-és időformátuma az "éééé-hh-nn óó: PP: ssZ" értékről "éééé-hh-nn óó: PP: ssZ" értékre módosult.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Kijavított problémák:

* Általános webszolgáltatások:
  * A WSconfig eszköz nem megfelelően konvertálta a JSON-tömböt a REST szolgáltatás metódusához tartozó "Sample Request" utasításból. Ez problémát okozott a JSON-tömbnek a REST-kérelemhez való szerializálásával kapcsolatban.
  * A webszolgáltatás-összekötő konfigurációs eszköze nem támogatja a szóköz szimbólumok használatát a JSON-attribútumok neveiben. 
    * A WSConfigTool.exe.config fájlba manuálisan is hozzáadhatók helyettesítő mintázatok, például:  ```<appSettings> <add key="JSONSpaceNamePattern" value="__" /> </appSettings>```
      > [!NOTE]
      > A JSONSpaceNamePattern kulcs szükséges az exportáláshoz, a következő hibaüzenet jelenik meg: üzenet: az üres név nem jogi. 

* Lotus-megjegyzések:
  * Ha az **Egyéni Certifiers engedélyezése a szervezethez/szervezeti egységekhez** lehetőség le van tiltva, akkor az Exportálás (frissítés) során az összekötő az exportálási folyamat után az összes attribútumot dominóba exportálja, de a KeyNotFoundException exportálásakor a rendszer visszaküldi a szinkronizálást. 
    * Ez azért fordulhat elő, mert az átnevezési művelet meghiúsul, ha az alábbi attribútumok egyikének módosításával megpróbálja módosítani a DN (UserName) attribútumot:  
      - LastName
      - FirstName
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - ou
      - altcommonname

  * Ha engedélyezve van az **Egyéni Certifiers engedélyezése a szervezet/szervezeti egységek számára** lehetőség, de a kötelező certifiers még mindig üresek, akkor KeyNotFoundException történik.

### <a name="enhancements"></a>Fejlesztések

* Általános SQL:
  * **Forgatókönyv: az újratervezett megvalósítás:** "*" funkció
  * **Megoldás leírása:** Módosult a [többértékű hivatkozási attribútumok kezelési](microsoft-identity-manager-2016-connector-genericsql.md)módszere.


### <a name="fixed-issues"></a>Kijavított problémák:

* Általános webszolgáltatások:
  * A kiszolgáló konfigurációja nem importálható, ha a webszolgáltatási összekötő jelen van
  * A webszolgáltatás-összekötő nem működik több webszolgáltatással

* Általános SQL:
  * Egyetlen értékre hivatkozó attribútum sem szerepel a felsorolásban.
  * A különbözeti importálás Change Tracking stratégiában törli az objektumot, ha a rendszer eltávolítja az értéket a többértékű táblából.
  * OverflowException a GSQL-összekötőben DB2 on AS/400

Lotus
  * A GlobalParameters-lap megnyitása előtt a enable\disable kereséséhez hozzáadott lehetőség

## <a name="114430"></a>1.1.443.0

Kiadás dátuma: 2017 március

### <a name="enhancements"></a>Fejlesztések

* Általános SQL:</br>
  **Forgatókönyv tünetei:**  Ez az SQL-összekötő jól ismert korlátozása, ahol csak egy adott objektumtípus hivatkozását engedélyezzük, és a tagokkal való kereszthivatkozásra van szükség. </br>
  **Megoldás leírása:** A hivatkozások feldolgozási lépésében a "*" beállítás van kiválasztva, az Objektumtípusok összes kombinációja vissza lesz visszaadva a Szinkronizáló motornak.

> [!Important]
> - Ez számos helyőrzőt fog létrehozni
> - Meg kell győződni arról, hogy az elnevezés egyedi típusú-e.


* Általános LDAP:</br>
  **Forgatókönyv:** Ha a rendszer csak néhány tárolót választ ki egy adott partíción, akkor a keresés a teljes partícióban továbbra is megtörténik. A rendszer a szinkronizálási szolgáltatás alapján szűri a konkrétat, de nem a MA, ami a teljesítmény romlását okozhatja. </br>

  **Megoldás leírása:** Módosította a GLDAP-összekötő kódját, hogy a teljes partíción való keresés helyett minden tárolón és keresési objektumon át lehessen lépni.


* Lotus Domino:

  **Forgatókönyv:** A Domino e-mail törlésének támogatása az exportálás során egy személy eltávolításához. </br>
  **Megoldás:** Konfigurálható levelezési törlési támogatás egy személy eltávolításához az exportálás során.

### <a name="fixed-issues"></a>Kijavított problémák:
* Általános webszolgáltatások:
  * Ha a szolgáltatás URL-címét az alapértelmezett SAP WSconfig-projektekben a webszolgáltatási konfigurációs eszközön keresztül változtatja meg, akkor a következő hiba történik: nem található az elérési út egy része

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* Általános LDAP:
  * Az GLDAP-összekötő nem látja az összes attribútumot a AD LDS
  * A varázsló megtöri, ha az LDAP-címtár sémája nem észleli az UPN-attribútumokat
  * A különbözeti importálások sikertelenek, ha a teljes importálás során nem jelennek meg felderítési hibák, ha a "objectClass" attribútum nincs kiválasztva.
  * A "partíciók és hierarchiák konfigurálása" konfigurációs lap nem jeleníti meg azokat az objektumokat, amelyek típusa megegyezik az új kiszolgálók partíciójának általános  
  LDAP MA. Csak a RootDSE partícióból származó objektumokat mutatták be.


* Általános SQL:
  * Javítás az általános SQL vízjel-különbözet importálásának többértékű attribútuma nem importált hiba
  * A többértékű attribútumok deleted\added exportálásakor a rendszer nem deleted\added az adatforrásban.  


* Lotus-megjegyzések:
  * A "teljes név" mező a metaverse-ben helyesen jelenik meg, de a megjegyzésekbe való exportáláskor az attribútum értéke null vagy üres.
  * Javítás duplikált tanúsító hibához
  * Ha az objektum a Lotus Domino-összekötőn semmilyen adat nélkül van kiválasztva más objektumokkal, akkor a rendszer a teljes importálás során megkeresi a felderítési hibát.
  * Ha a különbözeti Importálás a Lotus Domino-összekötőn fut, a Futtatás végén a Microsoft.IdentityManagement.MA.LotusDomino.Service.exe szolgáltatás időnként alkalmazáshiba-hibát ad vissza.
  * A csoporttagság összességében jól működik és karbantartva van, kivéve, ha az Exportálás futtatásával próbálkozik a felhasználó eltávolításával a tagságról, amely egy frissítéssel sikeresként jelenik meg, de a felhasználó nem távolítja el a tagságot a Lotus Notes szolgáltatásban.
  * Lehetőség van az Exportálás módjának kiválasztására, ha a "Hozzáfűzés elem alul" elemet adtak hozzá a Lotus MA konfigurációs grafikus felhasználói felületéhez, hogy az exportálás során a többértékű attribútumok esetében az új elemeket is fel lehessen fűzni.
  * Az összekötő hozzáadja a szükséges logikát a fájl törléséhez a levelezési mappából és az azonosító-tárolóból.
  * A törlési tagság nem működik több, mint NAB-tag számára.
  * Az értékeket sikeresen törölni kell a többértékű attribútumból

## <a name="111170"></a>1.1.117.0
Kiadás dátuma: 2016 március

**Új összekötő**  
Az [általános SQL-összekötő](microsoft-identity-manager-2016-connector-genericsql.md)kezdeti kiadása.

**Új funkciók:**

* Általános LDAP-összekötő:
  * A különbözeti importálás támogatása a Isode-mel.
* Webszolgáltatások összekötője:
  * Frissült a csEntryChangeResult tevékenység és a setImportErrorCode tevékenység, amely lehetővé teszi az objektumorientált hibák visszaadását a Szinkronizáló motornak.
  * A SAP6 és a SAP6User sablonok frissítése az új objektum szintű hiba funkció használatára.
* Lotus Domino-összekötő:
  * Exportálás esetén a címjegyzékhez egy tanúsító szükséges. Mostantól ugyanazzal a jelszóval használhatja az összes certifiers, hogy könnyebb legyen a felügyelet.

**Kijavított problémák:**

* Általános LDAP-összekötő:
  * Az IBM Tivoli DS esetében bizonyos hivatkozási attribútumok nem észlelhetők megfelelően.
  * A különbözeti importálás során az Open LDAP esetében a karakterláncok elején és végén lévő szóközök csonkítva jelennek meg.
  * A Novell és a NetIQ esetében olyan exportálási művelet, amely a szervezeti egységek/tárolók között áthelyezett egy objektumot, és az objektum nem lett átnevezve.
* Webszolgáltatások összekötője:
  * Ha a webszolgáltatás több végpontot is tartalmazott ugyanahhoz a kötéshez, akkor az összekötő nem tudta megfelelően felderíteni ezeket a végpontokat.
* Lotus Domino-összekötő:
  * Nem működött a fullName attribútum exportálása egy levelezési adatbázisba.
  * Egy csoportból hozzáadott és eltávolított tag csak a hozzáadott tagokat exportálta.
  * Ha egy Megjegyzés dokumentum érvénytelen (a isValid attribútum értéke false), akkor az összekötő meghibásodik.

## <a name="older-releases"></a>Régebbi verziók
Március 2016 előtt az összekötők támogatási témakörökként jelentek meg.

**Általános LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, szeptember 2015
* [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, március 2015
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, 2015 január
* [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, szeptember 2014
* [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, március 2014

**WebServices**

* [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, szeptember 2014

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, szeptember 2014

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, szeptember 2015
* [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, március 2015
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, augusztus 2014
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003, február 2014  
* [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, október 2013
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, augusztus 2013

## <a name="troubleshooting"></a>Hibaelhárítás 

> [!NOTE]
> Microsoft Identity Manager vagy AADConnect frissítése bármely ECMA2-összekötő használatával. 

Az összekötő definícióját frissíteni kell az egyeztetéshez, vagy a következő hibaüzenetet fogja kapni az alkalmazás eseménynaplójában: a 6947-es figyelmeztetési azonosító jelentése: "szerelvény verziója a HRE-összekötő konfigurációjában (" X.X.XXX. X ") korábbi, mint a tényleges verzió (" X.X.XXX. X ") a" C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll ".

A definíció frissítése:
* Az összekötő-példány tulajdonságainak megnyitása
* Kattintson a kapcsolat/kapcsolódás a laphoz elemre.
  * Adja meg az összekötő fiókhoz tartozó jelszót
* Kattintson a tulajdonságok lapfülekre, és kapcsolja be
  * Ha az összekötő típusa partíciók lap, a frissítés gomb mellett kattintson a frissítés gombra a lapon.
* Miután az összes tulajdonságlap hozzáfért, kattintson az OK gombra a módosítások mentéséhez.

## <a name="other-connectors"></a>Egyéb összekötők

A fent felsorolt összekötők mellett a SharePointhoz tartozó összekötőket és a Windows Azure Active Directory örökölt összekötőit is külön terjesztettük a Rendszerfelügyeleti webszolgáltatásból.

### <a name="sharepoint-user-profile"></a>SharePoint felhasználói profil

A Forefront Identity Manager-összekötő a SharePoint felhasználói profilok tárolójához a SharePoint 2013 és a SharePoint 2016 felhasználói profil tárolójába való szinkronizálást segíti.   Az összekötő verziójának 4.3.2430.0 közzétett 12/19/2016-es verziója letölthető a [Microsoft letöltőközpontból](https://www.microsoft.com/en-us/download/details.aspx?id=41164).

Az összekötőről további információk találhatók a [gyorsjavítások összegzése](https://support.microsoft.com/en-us/help/3156030/hotfix-rollup-build-4-3-2201-0-is-available-for-forefront-identity-man) című részben, és megtudhatja, hogyan [használhat egy minta-alapú megoldást a SharePoint Server 2016-ben](https://docs.microsoft.com/SharePoint/administration/use-a-sample-mim-solution-in-sharepoint-server-2016).

### <a name="forefront-identity-manager-connector-for-windows-azure-active-directory-legacy-connector"></a>Forefront Identity Manager-összekötő Windows Azure Active Directoryhoz (örökölt összekötő)

A FIM-hez készült Azure AD-összekötő egy korai technológia volt az azonosító adatok Azure Active Directory való szinkronizálásához. A FIM-hez készült Azure AD-összekötő, amely a 2014-es február 19-1.0.6635.0069, a szolgáltatás lefagy. Nem kap [frissítést, de](https://www.microsoft.com/en-us/download/details.aspx?id=41166) továbbra is támogatott. A FIM és az Azure AD Connector használatának megoldását [Azure ad Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect)felülírta. A Microsoft azt javasolja, hogy ne kezdjen el új központi telepítést ezen összekötő használatával.

## <a name="next-steps"></a>Következő lépések

További információ az [általános LDAP](microsoft-identity-manager-2016-connector-genericldap.md) -összekötő dokumentációjában bővebben az[általános SQL-összekötő](microsoft-identity-manager-2016-connector-genericsql.md) dokumentációjában tájékozódhat bővebben a [webszolgáltatások összekötő](microsoft-identity-manager-2016-ma-ws.md) dokumentációjában további információt a [PowerShell](microsoft-identity-manager-2016-connector-powershell.md) -összekötő dokumentációjában talál. További információ a [Lotus Domino-összekötő](microsoft-identity-manager-2016-connector-domino.md) dokumentációjában olvashatók.
