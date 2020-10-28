---
title: Általános SQL-összekötő | Microsoft Docs
description: Ez a cikk bemutatja, hogyan konfigurálhatja a Microsoft általános SQL-összekötőjét.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: fd8ccef3-6605-47ba-9219-e0c74ffc0ec9
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/02/2018
ms.author: billmath
ms.openlocfilehash: 862ea1ed910905daa0f2e0be97838f3da2423661
ms.sourcegitcommit: 2950ef38055bf78e3b98e4cd84771b04c2aa5584
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/08/2020
ms.locfileid: "92761050"
---
# <a name="generic-sql-connector-technical-reference"></a>Általános SQL-összekötő – technikai útmutató
Ez a cikk az általános SQL-összekötőt ismerteti. A cikk a következő termékekre vonatkozik:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * A gyorsjavítási 4.1.3671.0 vagy újabb [KB3092178](https://support.microsoft.com/kb/3092178)kell használnia.

A MIM2016 és a FIM2010R2 esetében az összekötő letölthető a [Microsoft letöltőközpontból](https://go.microsoft.com/fwlink/?LinkId=717495).

Az összekötő működés közbeni megtekintéséhez tekintse meg az [általános SQL-összekötő lépésről lépésre](microsoft-identity-manager-2016-connector-genericsql-step-by-step.md) szóló cikket.

## <a name="overview-of-the-generic-sql-connector"></a>Az általános SQL-összekötő áttekintése
Az általános SQL-összekötő lehetővé teszi a szinkronizációs szolgáltatás integrálását egy olyan adatbázis-rendszerrel, amely ODBC-kapcsolatot biztosít.  

Magas szintű perspektívából a következő funkciókat támogatja az összekötő jelenlegi kiadása:

| Szolgáltatás | Támogatás |
| --- | --- |
| Csatlakoztatott adatforrás |Az összekötő az összes 64 bites ODBC-illesztővel támogatott. A következővel lett tesztelve: <li>Microsoft SQL Server & SQL Azure</li><li>IBM DB2 10. x</li><li>IBM DB2 9. x</li><li>Oracle 10 & 11g</li><li>Oracle 12c és 18c</li><li>MySQL 5. x</li> |
| Forgatókönyvek |<li>Objektumok életciklusának kezelése</li><li>Jelszókezelés</li> |
| Műveletek |<li>Teljes Importálás és különbözeti importálás, exportálás</li><li>Exportáláshoz: Hozzáadás, törlés, frissítés és csere</li><li>Jelszó beállítása, jelszó módosítása</li> |
| Séma |<li>Objektumok és attribútumok dinamikus felderítése</li> |

> [!NOTE]
> * A fentiekben nem szereplő adatforrásokhoz való kapcsolódás jelenleg csak olvasási forgatókönyvekre korlátozódik.

### <a name="prerequisites"></a>Előfeltételek
Az összekötő használata előtt győződjön meg arról, hogy rendelkezik az alábbiakkal a szinkronizálási kiszolgálón:

* Microsoft .NET 4.5.2-keretrendszer vagy újabb
* 64 bites ODBC-ügyfél-illesztőprogramok
* Ha az összekötőt használja az Oracle 12c való kommunikációhoz, az ODBC-csomaggal az Oracle Instant Client 12.2.0.1 vagy újabb verzióra van szükség.
* Ha az összekötőt használja az Oracle 18c való kommunikációhoz, ehhez az Oracle Instant Client 18.3.0.0 vagy újabb verzióra van szükség ODBC-csomaggal, és a NLS_LANG rendszerváltozót az UTF8-karakterek támogatására kell beállítani.

### <a name="permissions-in-connected-data-source"></a>A csatlakoztatott adatforrásban lévő engedélyek
Az általános SQL-összekötő által támogatott feladatok bármelyikének létrehozásához vagy végrehajtásához a következőket kell tennie:

* db_datareader
* db_datawriter

### <a name="ports-and-protocols"></a>Portok és protokollok
Az ODBC-illesztő működéséhez szükséges portokért olvassa el az adatbázis gyártójának dokumentációját.

## <a name="create-a-new-connector"></a>Új összekötő létrehozása
Általános SQL-összekötő létrehozásához a **szinkronizációs szolgáltatásban** válassza a **felügyeleti ügynök** lehetőséget, és **hozzon létre** . Válassza ki az **általános SQL-(Microsoft-)** összekötőt.

![CreateConnector](./media/microsoft-identity-manager-2016-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Kapcsolatok
Az összekötő egy ODBC DSN-fájlt használ a kapcsolódáshoz. Hozza létre a DSN-fájlt a **felügyeleti eszközök** menü Start menüjében található **ODBC-adatforrások** használatával. A felügyeleti eszközben hozzon létre egy **Fájl-DSN** -t, hogy meg lehessen adni az összekötőnek.

![CreateConnector](./media/microsoft-identity-manager-2016-connector-genericsql/connectivity.png)

Az új általános SQL-összekötő létrehozásakor a kapcsolati képernyő az első. Először meg kell adnia a következő információkat:

* DSN-fájl elérési útja
* Hitelesítés
  * Felhasználónév
  * Jelszó

Az adatbázisnak támogatnia kell az alábbi hitelesítési módszerek egyikét:

* **Windows-hitelesítés** : a hitelesítő adatbázis a Windows hitelesítő adatait használja a felhasználó ellenőrzéséhez. A megadott felhasználónév/jelszó az adatbázissal való hitelesítésre szolgál. Ennek a fióknak engedélyre van szüksége az adatbázishoz.
* **SQL-hitelesítés** : a hitelesítő adatbázis azt a felhasználónevet/jelszót használja, amelyet a kapcsolati képernyő az adatbázishoz való csatlakozáshoz megadott. Ha a Felhasználónév/pasword fájlt az DSN-fájlban tárolja, a kapcsolati képernyőn megadott hitelesítő adatok elsőbbséget élveznek.
* **Azure SQL Database hitelesítés** : további tudnivalókért tekintse meg a [kapcsolódás SQL Databasehoz Azure Active Directory hitelesítés használatával](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)című témakört.

A **DN a horgony** : Ha ezt a beállítást választja, a rendszer a megkülönböztető nevet is használja az Anchor attribútumként. Egyszerű megvalósításhoz is használható, de a következő korlátozásokkal is rendelkezhet:

* Az összekötő csak egy objektumtípust támogat. Ezért a hivatkozási attribútumok csak ugyanarra az objektumra hivatkozhatnak.

**Exportálás típusa: objektum cseréje** : az exportálás során csak néhány attribútum módosult, az összes attribútummal rendelkező teljes objektum exportálva lesz, és lecseréli a meglévő objektumot.

### <a name="schema-1-detect-object-types"></a>1. séma (objektumtípusok észlelése)
Ezen a lapon konfigurálhatja, hogy az összekötő hogyan fogja megtalálni a különböző objektumtípusok az adatbázisban.

Minden objektumtípus partícióként jelenik meg, és a **partíciók és hierarchiák konfigurálásakor** tovább van konfigurálva.

![schema1a](./media/microsoft-identity-manager-2016-connector-genericsql/schema1a.png)

**Objektumtípus-észlelési módszer** : az összekötő támogatja az Objektumtípus észlelési módszereit.

* **Rögzített érték** : megadhatja az Objektumtípusok listáját vesszővel tagolt listával. Például: `User,Group,Department`.  
  ![schema1b](./media/microsoft-identity-manager-2016-connector-genericsql/schema1b.png)
* **Tábla/nézet/tárolt eljárás** : adja meg a tábla/nézet/tárolt eljárás nevét, majd az oszlop nevét, amely megadja az Objektumtípusok listáját. Ha tárolt eljárást használ, a paramétert a **[név]: [Direction]: [Value]** formátumban is megadja. Minden paramétert külön sorban adjon meg (a CTRL + ENTER billentyűkombinációval új sort kap).  
  ![schema1c](./media/microsoft-identity-manager-2016-connector-genericsql/schema1c.png)
* **SQL-lekérdezés** : Ez a beállítás lehetővé teszi, hogy olyan SQL-lekérdezést adjon meg, amely egy adott típusú oszlopot ad vissza, például: `SELECT [Column Name] FROM TABLENAME` . A visszaadott oszlopnak string (varchar) típusúnak kell lennie.

### <a name="schema-2-detect-attribute-types"></a>2. séma (attribútumok típusának észlelése)
Ezen a lapon megadhatja, hogyan történjen az attribútumok neveinek és típusának észlelése. A konfigurációs beállítások az előző oldalon észlelt összes objektumtípus esetében megjelennek.

![schema2a](./media/microsoft-identity-manager-2016-connector-genericsql/schema2a.png)

**Attribútum típusú észlelési módszer** : az összekötő támogatja ezeket az attribútum típusú észlelési metódusokat minden észlelt objektumtípus esetében az 1. séma képernyőn.

* **Tábla/nézet/tárolt eljárás** : adja meg az attribútumok nevének megkereséséhez használandó tábla/nézet/tárolt eljárás nevét. Ha tárolt eljárást használ, a paramétert a **[név]: [Direction]: [Value]** formátumban is megadja. Minden paramétert külön sorban adjon meg (a CTRL + ENTER billentyűkombinációval új sort kap). Egy többértékű attribútumban található attribútumok nevének észleléséhez adja meg a táblák vagy nézetek vesszővel tagolt listáját. A többértékű forgatókönyvek nem támogatottak, ha a szülő-és gyermektábla azonos oszlopnevek rendelkeznek.
* **SQL-lekérdezés** : Ez a beállítás lehetővé teszi olyan SQL-lekérdezés megadását, amely egy attribútum-névvel rendelkező egyetlen oszlopot ad vissza, például `SELECT [Column Name] FROM TABLENAME` . A visszaadott oszlopnak string (varchar) típusúnak kell lennie.

### <a name="schema-3-define-anchor-and-dn"></a>3. séma (horgony és DN meghatározása)
Ezen a lapon minden észlelt objektumtípus esetében konfigurálhatja a horgonyt és a DN attribútumot. Több attribútumot is kiválaszthat, hogy a horgony egyedi legyen.

![schema3a](./media/microsoft-identity-manager-2016-connector-genericsql/schema3a.png)

* A többértékű és a logikai attribútumok nem szerepelnek a felsorolásban.
* Ugyanez az attribútum nem használható a DN és a Anchor függvényhez, kivéve, ha a **DN nem horgony** van kiválasztva a kapcsolat lapon.
* Ha a kapcsolat lapon a **DN a horgony** van kiválasztva, akkor a lap csak a DN attribútumot igényli. Ez az attribútum a Anchor attribútumként is használható.

  ![schema3b](./media/microsoft-identity-manager-2016-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>4. séma (attribútum típusának, hivatkozásának és irányának meghatározása)
Ezen a lapon konfigurálhatja az attribútum típusát, például Integer, Binary vagy boolean, valamint az egyes attribútumok irányát. A 2. oldal **sémájának** összes attribútuma szerepel a többértékű attribútumok között.

![schema4a](./media/microsoft-identity-manager-2016-connector-genericsql/schema4a.png)

* **Adattípus** : az attribútum típusának a szinkronizálási motor által ismert típusokra való leképezésére szolgál. Alapértelmezés szerint az SQL-sémában észlelt típust kell használnia, de a DateTime és a Reference nem könnyen észlelhető. Ilyen esetben a **datetime** vagy a **Reference** értéket kell megadnia.
* **Irány** : az attribútum irányát beállíthatja importálás, exportálás vagy ImportExport értékre. A ImportExport alapértelmezett értéke.

![schema4b](./media/microsoft-identity-manager-2016-connector-genericsql/schema4b.png)

Megjegyzések:

* Ha az összekötő nem ismeri fel az attribútum típusát, a karakterlánc adattípust használja.
* A **beágyazott táblák** egyoszlopos adatbázis-táblák lehetnek. Az Oracle adott sorrendben tárolja egy beágyazott tábla sorait. Ha azonban a beágyazott táblázatot egy PL/SQL-változóba kéri le, a sorok egymást követő alszkripteket kapnak, 1-től kezdődően. Ez lehetővé teszi, hogy a tömbhöz hasonlóan hozzáférjen az egyes sorokhoz.
* A **VARRYS** nem támogatottak az összekötőben.

### <a name="schema-5-define-partition-for-reference-attributes"></a>5. séma (a partíció meghatározása a hivatkozási attribútumok esetében)
Ezen az oldalon az összes olyan hivatkozási attribútumot be kell állítani, amely számára a partíció (objektumtípus) attribútum hivatkozik.

![schema5](./media/microsoft-identity-manager-2016-connector-genericsql/schema5.png)

Ha a **DN-t használja horgonyként** , akkor ugyanazt az objektumtípust kell használnia, mint amelyre hivatkozik. Más objektumtípust nem lehet hivatkozni.

> [!NOTE]
> A március 2017-es frissítéstől kezdődően a "*" lehetőség is rendelkezésre áll, ha ezt a beállítást választja, a rendszer az összes lehetséges tagot importálja.

![globalparameters3](./media/microsoft-identity-manager-2016-connector-genericsql/any-option.png)

> [!IMPORTANT]
>  A (z \* ) "" más néven a (z) "" más néven **az** importálási és az exportálási folyamat támogatásához a "" más néven 2017 Ha ezt a beállítást szeretné használni, a többértékű táblának/nézetnek rendelkeznie kell egy olyan attribútummal, amely tartalmazza az objektum típusát.

![](./media/microsoft-identity-manager-2016-connector-genericsql/any-02.png)

 </br> Ha a "*" van kiválasztva, akkor az Objektumtípus nevű oszlop nevét is meg kell adni.</br> ![](./media/microsoft-identity-manager-2016-connector-genericsql/any-03.png)

Az importálás után az alábbi képen láthatóhoz hasonló lesz:

  ![globalparameters3](./media/microsoft-identity-manager-2016-connector-genericsql/after-import.png)



### <a name="global-parameters"></a>Globális paraméterek
A globális paraméterek lap a különbözeti importálás, a dátum/idő formátum és a jelszó módszerének konfigurálására szolgál.

![globalparameters1](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters1.png)



Az általános SQL-összekötő a következő módszereket támogatja a különbözeti importáláshoz:

* **Trigger** : [a különbözeti nézetek létrehozása eseményindítókkal](https://technet.microsoft.com/library/cc708665.aspx).
* **Vízjel** : általános megközelítés, amely bármely adatbázishoz használható. A vízjel-lekérdezés előre fel van töltve az adatbázis gyártója alapján. A vízjel oszlopnak jelen kell lennie minden felhasznált táblában/nézetben. Ennek az oszlopnak a táblákat és az azoktól függő (többértékű vagy gyermek) táblákat kell követnie. A szinkronizálási szolgáltatás és az adatbázis-kiszolgáló közötti órákat szinkronizálni kell. Ha nem, akkor előfordulhat, hogy a különbözeti importálás egyes bejegyzései kimaradnak.  
  Korlátozás
  * A vízjel-stratégia nem támogatja a törölt objektumokat.
* **Pillanatkép** : (csak Microsoft SQL Server esetén működik), amely a [Pillanatképek használatával hoz létre különbözeti nézeteket](https://technet.microsoft.com/library/cc720640.aspx)
* **Change Tracking** : (csak Microsoft SQL Server esetén működik) [a Change Tracking](https://msdn.microsoft.com/library/bb933875.aspx)  
  Korlátozások:
  * A horgony & DN attribútumnak a tábla kiválasztott objektumához tartozó elsődleges kulcs részét kell képeznie.
  * Az SQL-lekérdezés nem támogatott az Importálás és az exportálás során a Change Tracking.

**További paraméterek** : adja meg az adatbázis-kiszolgáló időzónáját, amely azt jelzi, hogy hol található az adatbázis-kiszolgáló. Ez az érték a dátum-és idő&ek különböző formátumának támogatására szolgál.

Az összekötő mindig az UTC formátumban tárolja a dátumot és az időpontot. A dátum és az idő helyes átalakításához meg kell adni az adatbázis-kiszolgáló időzónáját és a használt formátumot. A formátumot .net formátumban kell megadni.

Az exportálás során az UTC-időformátumban meg kell adni az összekötőnek minden dátum idő attribútumot.

![globalparameters2](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters2.png)

**Jelszó-konfiguráció** : az összekötő jelszó-szinkronizálási funkciókat biztosít, és támogatja a jelszó beállítását és módosítását.

Az összekötő két módszert biztosít a jelszó-szinkronizálás támogatására:

* **Tárolt eljárás** : ehhez a módszerhez két tárolt eljárás szükséges a set & jelszó megadása beállítás támogatásához. Adja meg az összes paramétert a hozzáadáshoz, és módosítsa a jelszó beállítása a jelszó **megadásához** , és módosítsa a **jelszó SP** paramétereket az alábbi példa szerint.
  ![globalparameters3](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters3.png)
* **Jelszó-kiterjesztés** : ehhez a metódushoz jelszó-kiterjesztési dll szükséges (meg kell adnia a kiterjesztés dll-nevét, amely a [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) felületet implementálja). A jelszó-kiterjesztés szerelvényt a kiterjesztési mappába kell helyezni, hogy az összekötő a DLL-t futásidőben tudja betölteni.
  ![globalparameters4](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters4.png)

Emellett engedélyeznie kell a jelszó-kezelést a **bővítmény konfigurálása** lapon.
![globalparameters5](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Partíciók és hierarchiák konfigurálása
A partíciók és hierarchiák lapon válassza a minden objektumtípus elemet. Minden objektumtípus a saját partíciója.

![partitions1](./media/microsoft-identity-manager-2016-connector-genericsql/partitions1.png)

A **kapcsolat** vagy a **globális paraméterek** lapon definiált értékeket is felül lehet bírálni.

![partitions2](./media/microsoft-identity-manager-2016-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Horgonyok konfigurálása
Ez a lap csak olvasható, mert a horgony már definiálva van. A kiválasztott Anchor attribútumot a rendszer mindig az Objektumtípus alapján fűzi hozzá, hogy az objektum típusa egyedi maradjon.

![kapcsolatok alapjainak](./media/microsoft-identity-manager-2016-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>A futtatási lépés paraméterének konfigurálása
Ezek a lépések az összekötő futtatási profiljain konfigurálhatók. Ezek a konfigurációk az adatok importálásával és exportálásával kapcsolatos tényleges munkát végzik.

### <a name="full-and-delta-import"></a>Teljes és különbözeti importálás
Az általános SQL-összekötő a következő módszerekkel támogatja a teljes és a különbözeti importálást:

* Tábla
* Nézet
* Tárolt eljárás
* SQL-lekérdezés

![runstep1](./media/microsoft-identity-manager-2016-connector-genericsql/runstep1.png)

**Tábla/nézet**  
Ha többértékű attribútumokat szeretne importálni egy objektumhoz, meg kell adnia a tábla/nézet nevét a **többértékű táblák/nézetek** nevében, valamint a megfelelő illesztési feltételeket a fölérendelt tábla **illesztési feltételében** . Ha az adatforrásban több többértékű tábla található, akkor egyetlen nézethez is használhatja a uniont.

> [!IMPORTANT]
> Az általános SQL felügyeleti ügynök csak egyetlen többértékű táblával működhet. Ne helyezze a többértékű tábla nevét, vagy a több mint egy tábla nevét. Ez az általános SQL-korlátozás.


Példa: importálni kívánja az alkalmazott objektumot és az összes többértékű attribútumot. Két táblázat létezik: Employee (fő tábla) és részleg (többértékű).
Tegye a következőket:

* Írja be a következőt: **Employee** in **Table/View/SP** .
* Írja be a részleg **nevet a többértékű táblák/nézetek neve** mezőbe.
* Adja meg az összekapcsolási feltételt az alkalmazott & részlege között a **JOIN feltételben** , például: `Employee.DEPTID=Department.DepartmentID` .
  ![runstep2](./media/microsoft-identity-manager-2016-connector-genericsql/runstep2.png)

**Tárolt eljárások**  
![runstep3](./media/microsoft-identity-manager-2016-connector-genericsql/runstep3.png)

* Ha sok adattal rendelkezik, javasolt a tördelést megvalósítani a tárolt eljárásokkal.
* A tördelést támogató tárolt eljáráshoz meg kell adnia a kezdő indexet és a záró indexet. Lásd: [hatékony lapozás nagy mennyiségű adattal](https://msdn.microsoft.com/library/bb445504.aspx).
* @StartIndex a és a @EndIndex végrehajtási időpontra vált, és a megfelelő oldalméret-érték van konfigurálva a **lépés konfigurálása** lapon. Ha például az összekötő beolvassa az első oldalt, és az oldalméret értéke 500, akkor az ilyen helyzetekben az @StartIndex 1 és a @EndIndex 500. Ezek az értékek növekednek, ha az összekötő a következő lapokat kéri le, és megváltoztatja a @StartIndex & @EndIndex értéket.
* A paraméteres tárolt eljárás végrehajtásához adja meg a paramétereket a következő `[Name]:[Direction]:[Value]` formátumban:. Adja meg az egyes paramétereket külön sorban (a CTRL + ENTER billentyűkombinációval új sort kap).
* Az általános SQL-összekötő támogatja az importálási műveletet is a Microsoft SQL Server csatolt kiszolgálóiról. Ha az adatokat a csatolt kiszolgáló egyik táblájából kell lekérni, a táblázatot a következő formátumban kell megadni: `[ServerName].[Database].[Schema].[TableName]`
* Az általános SQL-összekötő csak azokat az objektumokat támogatja, amelyek hasonló struktúrával rendelkeznek (alias neve és adattípusa) a futtatási lépések információ és a séma észlelése között. Ha a kiválasztott objektum a sémából, és a futtatási lépésben megadott információk eltérnek, akkor az SQL Connector nem tudja támogatni az ilyen típusú forgatókönyveket.

**SQL-lekérdezés**  
![runstep4](./media/microsoft-identity-manager-2016-connector-genericsql/runstep4.png)

![runstep5](./media/microsoft-identity-manager-2016-connector-genericsql/runstep5.png)

* Több eredményhalmaz-lekérdezés nem támogatott.
* Az SQL-lekérdezés támogatja a tördelést, és megadhatja a kezdő indexet és a befejezési indexet változóként a tördelés támogatásához.

### <a name="delta-import"></a>Különbözeti importálás
![runstep6](./media/microsoft-identity-manager-2016-connector-genericsql/runstep6.png)

A különbözeti importálás konfigurációjának további konfigurálására van szükség a teljes importáláshoz képest.

* Ha az trigger vagy a pillanatkép módszert választja a változási változások nyomon követéséhez, akkor adja meg az előzmények táblát vagy a pillanatfelvétel-adatbázist az **előzmények tábla vagy a pillanatkép-adatbázis neve** mezőben.
* Az előzmények tábla és a szülő tábla közötti csatlakozási feltételt is meg kell adnia, például: `Employee.ID=History.EmployeeID`
* Ha nyomon szeretné követni a fölérendelt táblában lévő tranzakciót az előzmények táblából, meg kell adnia a művelet adatait (Hozzáadás/frissítés/törlés) tartalmazó oszlopnevet.
* Ha a Delta változások követéséhez a vízjel lehetőséget választja, adja meg azt az oszlopnevet, amely a művelet adatait tartalmazza a **vízjelek oszlopainak neve** mezőben.
* A változás típusához a **change Type attribútum** oszlop szükséges. Ez az oszlop az elsődleges táblában vagy a többértékű táblában lévő módosítást képezi le a változási nézetben. Ez az oszlop tartalmazhatja az attribútum szintű módosítás Modify_Attribute módosításának típusát, illetve az objektum szintű módosítás típusának hozzáadási, módosítási vagy törlési típusának módosítási típusát. Ha más, mint az alapértelmezett érték hozzáadása, módosítása vagy törlése, akkor ezt a lehetőséget választva megadhatja ezeket az értékeket.

### <a name="export"></a>Exportálás
![runstep7](./media/microsoft-identity-manager-2016-connector-genericsql/runstep7.png)

Az általános SQL Connector támogatja az exportálást négy támogatott módszerrel, például a következőkkel:

* Tábla
* Nézet
* Tárolt eljárás
* SQL-lekérdezés

**Tábla/nézet**  
Ha a tábla/nézet lehetőséget választja, akkor az összekötő létrehozza a megfelelő lekérdezéseket az Exportálás elvégzéséhez.

**Tárolt eljárások**  
![runstep8](./media/microsoft-identity-manager-2016-connector-genericsql/runstep8.png)

Ha a tárolt eljárás beállítást választja, az exportáláshoz három különböző tárolt eljárás szükséges az INSERT/Update/DELETE művelet végrehajtásához.

* **SP-név hozzáadása** : ez az SP akkor fut le, ha bármelyik objektum az adott táblába való beszúráshoz csatlakozik.
* **SP-név frissítése** : ez az SP akkor fut le, ha bármelyik objektum csatlakozik a frissítéshez az adott táblában.
* **SP-név törlése** : ez az SP akkor fut le, ha bármely objektum az összekötőhöz csatlakozik a megfelelő táblában való törléshez.
* A (z) paraméterként használt sémából kiválasztott attribútum a tárolt eljáráshoz. Például `@EmployeeName: INPUT: EmployeeName` : (EmployeeName van kiválasztva az összekötő sémában, és az összekötő helyettesíti a megfelelő értéket az exportáláskor)
* A paraméteres tárolt eljárás futtatásához adja meg a paramétereket `[Name]:[Direction]:[Value]` formátumban. Adja meg az egyes paramétereket külön sorban (a CTRL + ENTER billentyűkombinációval új sort kap).

**SQL-lekérdezés**  
![runstep9](./media/microsoft-identity-manager-2016-connector-genericsql/runstep9.png)

Ha az SQL-lekérdezés lehetőséget választja, az exportáláshoz három különböző lekérdezés szükséges az INSERT/Update/DELETE művelet végrehajtásához.

* **Lekérdezés beszúrása** : Ez a lekérdezés akkor fut le, ha bármelyik objektum az adott táblába való beszúráshoz csatlakozik.
* **Lekérdezés frissítése** : Ez a lekérdezés akkor fut le, ha bármelyik objektum csatlakozik a frissítéshez az adott táblában.
* **Lekérdezés törlése** : ezt a lekérdezést akkor futtatja a rendszer, ha bármelyik objektum csatlakozik a törléshez a megfelelő táblában.
* Az attribútum paraméterként használt sémája a lekérdezéshez van kiválasztva, például: `Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Hibaelhárítás
* További információ az összekötők hibakeresésének engedélyezéséről: [ETW-nyomkövetés engedélyezése összekötők számára](https://go.microsoft.com/fwlink/?LinkId=335731).
