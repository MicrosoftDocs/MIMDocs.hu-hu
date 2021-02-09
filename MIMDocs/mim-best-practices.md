---
title: Microsoft Identity Manager 2016 – Gyakorlati tanácsok | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 71ac80f10fd01e7c784ec1462a9383295e5f8ddb
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835867"
---
# <a name="microsoft-identity-manager-2016-best-practices"></a>Microsoft Identity Manager 2016 – Gyakorlati tanácsok

A következőkben a Microsoft Identity Manager 2016 (MIM) telepítésével és működtetésével kapcsolatos ajánlott eljárásokat ismertetjük

## <a name="sql-setup"></a>SQL-telepítés
> [!NOTE]
> A következő, egy SQL-t futtató kiszolgáló telepítésére vonatkozó javaslatok dedikált SQL-példányok meglétét feltételezik a FIMService-hez és a FIMSynchronizationService adatbázishoz. Ha a FIMService-t összevont környezetben futtatja, akkor el kell végeznie a konfigurációhoz szükséges módosításokat.

A Structured Query Language- (SQL-) kiszolgáló konfigurálása alapvető fontosságú a rendszer optimális teljesítményéhez. Az optimális MIM teljesítmény elérése nagy léptékű környezetekben az SQL-t futtató kiszolgálókra vonatkozó ajánlott eljárások alkalmazásától függ. További információk az SQL-lel kapcsolatos ajánlott eljárásokról következő témakörökben olvashatók:

-   [A tárolással kapcsolatos 10 legfontosabb gyakorlati tanács](https://go.microsoft.com/fwlink/?LinkID=183663) (angol nyelven)

-   [Optimizing tempdb Performance](https://go.microsoft.com/fwlink/?LinkID=188267) (A tempdb teljesítményének optimalizálása)

-   [SQL Server – Gyakorlati tanácsok cikk](https://go.microsoft.com/fwlink/?LinkID=188268) (angol nyelven)

-   [Reorganizing and Rebuilding Indexes](https://go.microsoft.com/fwlink/?LinkID=188269) (Indexek átszervezése és újraépítése)

### <a name="presize-data-and-log-files"></a>Adat- és naplófájlok előzetes méretezése

Ne támaszkodjon az automatikus növekedés használatára. Ehelyett kezelje manuálisan ezen fájlok méretnövekedését. Biztonsági okokból bekapcsolva hagyhatja az automatikus növekedési funkciót, de proaktívan felügyelje az adatfájlok méretének növekedését. A MIM-adatbázis példaméreteit a [FIM kapacitástervezési útmutatóban](https://go.microsoft.com/fwlink/?LinkID=185246) találja (angol nyelven).

### <a name="to-presize-sql-data-and-log-files"></a>Az adat- és naplófájlok előzetes méretezéséhez:

1.  Indítsa el az SQL Server Management Studiót.

2.  Nyissa meg az adatbázishoz tartozó FIMService-t, kattintson jobb gombbal a FIMService-re, majd kattintson a Properties (Tulajdonságok) parancsra.

3.  A Files (Fájlok) lapon bővítse a szükséges méretre az adatbázisfájlokat.

### <a name="isolate-log-from-data-files"></a>A napló elkülönítése az adatfájloktól

Az adatbázisok tranzakciós és az adatokra vonatkozó naplófájlok külön fizikai lemezekre való elkülönítéséhez kövesse az SQL Server ajánlott eljárásait.

További tempdb-fájlok létrehozása

Az optimális teljesítmény érdekében processzormagonként egy adatfájlt ajánlott létrehozni a tempdb-fájlban.

### <a name="to-create-additional-tempdb-files"></a>További tempdb-fájlok létrehozásához:

1.  Indítsa el az SQL Server Management Studiót.

2.  Keresse meg az adatbázishoz tartozó tempdb-fájlt a rendszeradatbázisok között, kattintson jobb gombbal a tempdb-fájlra, és válassza a Properties (Tulajdonságok) parancsot.

3.  A Files (Fájlok) lapon hozzon létre minden egyes processzormaghoz egy-egy adatfájlt. Ügyeljen arra, hogy a tempdb adat- és naplófájljai különböző meghajtókon és lemezeken legyenek elkülönítve.

### <a name="ensure-adequate-space-for-log-files"></a>Elegendő szabad hely biztosítása a naplófájlok számára

Fontos a helyreállítási modell lemezkövetelményeinek ismerete. Az egyszerű helyreállítási mód a kezdeti rendszerbetöltés során megfelelő lehet a használt lemezterület korlátozására, de a legutóbbi biztonsági mentés után létrehozott adatok adatvesztésnek vannak kitéve. Teljes helyreállítási mód használatakor szükséges a lemezterület biztonsági másolatokra való használatának felügyelete, többek között a tranzakciónapló gyakori biztonsági mentései magas lemezterület-használatának megakadályozására. További információt a [Recovery Model Overview](https://go.microsoft.com/fwlink/?LinkID=185370) (A helyreállítási modell áttekintése) című részben talál.

### <a name="limit-sql-server-memory"></a>Az SQL Server memóriájának korlátozása

Attól függően, hogy mennyi memóriát használ az SQL Server, és hogy megosztja-e az SQL Servert más szolgáltatásokkal (azaz a MIM 2016 szolgáltatással és a MIM 2016 Synchronization Service-szel), előfordulhat, hogy korlátozni kell az SQL memóriahasználatát. Ez az alábbi lépések segítségével tehető meg.

1. Indítsa el az SQL Server Enterprise Managert.

2. Válassza a New Query (Új lekérdezés) lehetőséget.

3. Futtassa az alábbi lekérdezést:

   ```SQL
   USE master

   EXEC sp_configure 'show advanced options', 1

   RECONFIGURE WITH OVERRIDE

   USE master

   EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
   WITH OVERRIDE
   ```

   Ez a példa újrakonfigurálja az SQL Servert úgy, hogy ne legyen több, mint 12 gigabájt (GB) memóriával.

4. A beállítás ellenőrzésére használja a következő lekérdezést:

   ```SQL
   USE master

   EXEC sp_configure 'max server memory (MB)'--- verify the setting

   USE master

   EXEC sp_configure 'show advanced options', 0

   RECONFIGURE WITH OVERRIDE
   ```

### <a name="backup-and-recovery-configuration"></a>Biztonsági mentés és helyreállítás konfigurálása

Általánosságban elmondható, hogy a biztonsági mentési és helyreállítási stratégia kialakításához az adatbázis rendszergazdájával kell dolgoznia. Néhány javaslat a következőkre terjed ki:
- Az adatbázis biztonsági másolatait a szervezet biztonsági mentési szabályzatának megfelelően hajthatja végre. 
- Ha a növekményes naplófájlok biztonsági mentései nem tervezettek, az egyszerű helyreállítási módot kell beállítani az adatbázishoz. 
- Győződjön meg arról, hogy tisztában van a különböző helyreállítási modellek következményeivel a biztonsági mentési stratégia megvalósítása előtt. Ismerje meg a modellek lemezterület-követelményeit. A teljes helyreállítási modell a naplók gyakori biztonsági mentéseit igényli a magas lemezterület-használat elkerülése érdekében. 

További információt a [Recovery Model Overview](https://go.microsoft.com/fwlink/?LinkID=185370) (A helyreállítási modell áttekintése) és a [FIM 2010 Backup and Restore Guide](https://go.microsoft.com/fwlink/?LinkID=165864) (FIM 2010 biztonsági mentési és visszaállítási útmutató) című részben talál.

## <a name="create-a-backup-administrator-account-for-the-fim-service-after-installation"></a>Biztonsági mentési rendszergazdai fiók létrehozása a FIM szolgáltatáshoz a telepítés után

A FIMService-rendszergazdák csoport tagjai egyedi engedélyekkel rendelkeznek, amelyek kritikus fontosságúak a biztonsági azonosítók üzembe helyezésének működéséhez. Ha nem tud bejelentkezni a rendszergazdák készletbe, az egyetlen megoldás a rendszer előző biztonsági másolatának visszaállítása. Ezen helyzet elkerülése érdekében javasolt, hogy a telepítés utáni konfiguráció részeként adjon hozzá más felhasználókat a FIM rendszergazdai csoporthoz.

## <a name="fim-service"></a>FIM szolgáltatás


### <a name="configuring-fim-service-service-exchange-mailbox"></a>A FIM szolgáltatás Exchange-szolgáltatásfiókjának konfigurálása

Az alábbi javasolt megoldásokkal konfigurálhatja a Microsoft Exchange Servert a MIM 2016 szolgáltatás szolgáltatásfiókjához.

- A szolgáltatásfiókot úgy konfigurálja, hogy az csak belső e-mail-címekről fogadhasson leveleket. Konkrétan arról van szó, hogy a szolgáltatásfiók postafiókja sohasem fogadhat leveleket külső SMTP-kiszolgálókról.

#### <a name="to-configure-the-service-account"></a>A szolgáltatásfiók konfigurálása

1.  Az Exchange felügyeleti konzolon válassza **A FIM szolgáltatás szolgáltatásfiókja** lehetőséget.

2.  Válassza a tulajdonságokat, az e-mail-forgalom beállításait, majd az **Üzenetkézbesítési korlátozások** lehetőséget.

3.  Jelölje be **Az összes feladó hitelesítése szükséges** jelölőnégyzetet.

További információkért lásd: [Configure Message Delivery Restrictions](https://go.microsoft.com/fwlink/?LinkID=183625) (Üzenetkézbesítési korlátozások konfigurálása).

-   Konfigurálja a szolgáltatásfiókot az 1 MB-nál nagyobb méretű levelek elutasítására. Kövesse az [üzenetek méretkorlátjának konfigurálása](https://go.microsoft.com/fwlink/?LinkID=183626) ajánlott eljárást (angol nyelvű cikk) a postafiókhoz vagy olyan nyilvános mappához, amelynél engedélyezve van a levelezés.

-   Konfigurálja a szolgáltatásfiókot 5 GB-os tárolási kvótára a postafiókhoz. Az optimális eredmények elérése érdekében kövesse a [Configure Storage Quotas for a Mailbox](https://go.microsoft.com/fwlink/?LinkID=156929) (Postafiókok tárolási kvótáinak konfigurálása) című cikkben felsorolt ajánlott eljárásokat.

## <a name="mim-portal"></a>MIM-portál


### <a name="disable-sharepoint-indexing"></a>SharePoint-indexelés letiltása

Ajánlott letiltani a Microsoft Office SharePoint® indexelését. Nincsenek indexelhető dokumentumok. Az indexelés számos hibanapló-bejegyzést és potenciális teljesítménnyel kapcsolatos problémát okoz. A SharePoint-indexelés letiltásához hajtsa végre az alábbi lépéseket:

1.  A MIM 2016 portált futtató kiszolgálón kattintson a Start gombra.

2.  Mutasson a Minden program pontra.

3.  A programok listájában kattintson a Felügyeleti eszközök pontra.

4.  A Felügyeleti eszközök alatt kattintson a SharePoint központi felügyelet elemre.

5.  A központi felügyelet lapján kattintson a Tevékenység elemre.

6.  A Tevékenység oldalon a Globális beállítások csoportban kattintson az Időzítőfeladat-definíciók elemre.

7.  Az időzítőfeladat-definíciók lapján kattintson a SharePoint Services – keresés frissítése elemre.

8.  Az Időzítőfeladat módosítása lapon kattintson a Letiltás elemre.

## <a name="mim-2016-initial-data-load"></a>A MIM 2016 kezdeti adatbetöltése

Ez a szakasz egy sor lépésből áll, amelyekkel növelheti a kezdeti adatterhelések teljesítményét a külső rendszerről a webhelyre. Fontos tisztában lenni azzal, hogy ezek a lépések csak a rendszer kezdeti populációjában hajthatók végre. A betöltés befejezését követően alaphelyzetbe kell állítani őket. Ez egy egyszeri művelet, és nem folyamatos szinkronizálás.

> [!NOTE]
> További információ a felhasználók és a Active Directory Domain Services közötti szinkronizálásról (AD DS): Hogyan a [felhasználók szinkronizálása Active Directoryról a FIM](https://go.microsoft.com/fwlink/?LinkID=188277) -be a FIM-dokumentációban.
> 
> [!IMPORTANT]
> Győződjön meg arról, hogy alkalmazta a jelen útmutató SQL-telepítéssel foglalkozó szakaszában tárgyalt ajánlott eljárásokat. 

### <a name="step-1-configure-the-sql-server-for-initial-data-load"></a>1. lépés: Az SQL Server konfigurálása a kezdeti adatbetöltéshez
Az adatmennyiség kezdeti betöltése hosszadalmas folyamat lehet. Ha úgy tervezi, hogy kezdetben nagy mennyiségű adatot tölt be, lerövidítheti az adatbázis feltöltéséhez szükséges időt azáltal, hogy ideiglenesen kikapcsolja a teljes szöveges keresést, és újra bekapcsolja azt a felügyeleti csomag 2016 felügyeleti ügynökének (FIM MA) exportálása után.

A teljes szöveges keresés átmeneti kikapcsolásához:

1.  Indítsa el az SQL Server Management Studiót.

2.  Válassza a New Query (Új lekérdezés) lehetőséget.

3.  Futtassa a következő SQL-utasításokat:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

> [!IMPORTANT]
> Ezen eljárások elmulasztása magas lemezterület-használatot eredményezhet, és előfordulhat, hogy elfogy a szabad lemezterület. További információt ebben a témakörben a [Recovery Model Overview](https://go.microsoft.com/fwlink/?LinkID=185370) (A helyreállítási modell áttekintése) című részben találhat. A [FIM biztonsági mentési és visszaállítási útmutatója](https://go.microsoft.com/fwlink/?LinkID=165864) további információkat tartalmaz (angol nyelven).

### <a name="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process"></a>2. lépés: A minimálisan szükséges MIM-konfiguráció alkalmazása a betöltési folyamat során

A kezdeti betöltése során csak a FIM-konfigurációhoz a felügyeletiházirend-szabályokhoz (MPR-ek) és a készletdefiníciókhoz minimálisan szükséges konfigurációt kell alkalmazni. Az adatok betöltése után hozza létre az adott környezethez szükséges további készleteket. A szabályzatoknak a betöltött adatokra való visszamenőleges alkalmazásához használja a munkafolyamatok Run On Policy Update (Futtatás szabályzatfrissítéskor) beállítását.

### <a name="step-3-configure-and-populate-the-fim-service-with-external-identity-data"></a>3. lépés: A FIM szolgáltatás konfigurálása és feltöltése külső azonosító adatokkal

Ekkor kövesse a következő témakörben ismertetett eljárásokat: Hogyan lehet szinkronizálni a felhasználókat a Active Directory Domain Servicesról a FIM-útmutatóba a rendszer a Active Directory felhasználókkal való konfigurálásához és szinkronizálásához. Ha szinkronizálni szeretné a csoport adatait, az adott folyamat eljárásai a [Hogyan szinkronizálhatók a csoportok Active Directory Domain Servicesról a FIM](https://technet.microsoft.com/library/ff686936(v=ws.10).aspx) -útmutatóra.

#### <a name="synchronization-and-export-sequences"></a>A szinkronizálás és az exportálás sorrendje

A teljesítmény optimalizálása érdekében az exportálást az összekötőtérben nagy számú függőben lévő exportálási műveletet eredményező szinkronizálás után kell futtatni. Ezután futtasson megerősítő importálást az érintett összekötőtérrel társított kezelőügynökön. Például, ha a kezdeti adatbetöltés részeként több kezelőügynökön kell szinkronizálási futtatási profilokat futtatni, minden egyes szinkronizálásfuttatás után futtatnia kell egy exportálást, majd egy különbözeti importálást.
Minden az inicializálási ciklus részét képező forrás-kezelőügynök esetén hajtsa végre az alábbi lépéseket:

1.  Teljes importálás a forrás-kezelőügynökön.

2.  Teljes szinkronizálás a forrás-kezelőügynökön.

3.  Exportálás az összes érintett cél-kezelőügynökön szakaszos exportálási műveletekkel.

4.  Különbözeti importálás az összes érintett cél-kezelőügynökön szakaszos exportálási műveletekkel.

### <a name="step-4-apply-your-full-mim-configuration"></a>4. lépés: A teljes MIM-konfiguráció alkalmazása

A kezdeti adatbetöltés befejezése után alkalmaznia kell a teljes MIM-konfigurációt az adott környezethez.

Az alkalmazási helyzettől függően ebbe beletartozik a további készletek, MPR-ek és munkafolyamatok létrehozása. Az esetleges minden meglévő rendszerbeli objektumra visszamenőlegesen alkalmazandó szabályzatok esetén használja a Run On Policy Update (Futtatás szabályzatfrissítéskor) beállítást a munkafolyamatok esetén ezen szabályzatoknak a betöltött adatokra való visszamenőleges alkalmazásához.

### <a name="step-5-reconfigure-sql-to-previous-settings"></a>5. lépés: Az SQL újrakonfigurálása az előző beállításokra

Ne felejtse el módosítani az SQL-beállításokat a normál beállításokra. Ide tartoznak az alábbiak:

-   A teljes szöveges keresés bekapcsolása

-   A biztonsági mentési szabályzat frissítése a szervezeti házirend szerint

Miután befejezte a kezdeti adatbetöltést, kapcsolja be újra a teljes szöveges keresést. Futtassa a következő SQL-utasításokat a teljes szöveges keresés újbóli bekapcsolásához:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

Ha egyszerű helyreállítási módra kell váltani, győződjön meg arról, hogy újrakonfigurálta a biztonsági mentési ütemezését a szervezet biztonsági mentési szabályzatának megfelelően. A FIM biztonsági mentési ütemezésről további részleteket a [FIM biztonsági mentési és visszaállítás útmutatójában](https://go.microsoft.com/fwlink/?LinkID=165864) talál (angol nyelven).

## <a name="configuration-migration"></a>Konfiguráció migrálása


### <a name="avoid-changing-display-names"></a>Ne módosítsa a megjelenített neveket

Számos objektumtípus, például MPR-ek esetén a syncproduction.ps1 szkript a megjelenítési nevet használja egyetlen horgonyattribútumként a két rendszer között. Ennél fogva egy meglévő MPR megjelenített nevének módosítása az új MPR létrehozása után a meglévő MPR törlését eredményezi. Ennek oka az, hogy a migrálási folyamat nem tudja sikeresen összekötni azon MPR-eket, amelyek illesztési feltételei megváltoztak. A probléma elkerülése érdekében minden konfigurációs objektumtípus esetén hozzáköthet egy egyéni attribútumot, és ezt az attribútumot használhatja illesztési feltételként. Ez lehetővé teszi a megjelenített nevek módosítását a migrálási folyamat befolyásolása nélkül.

### <a name="avoid-changing-the-content-of-intermediate-files"></a>Ne változtassa meg a köztes fájlok tartalmát

Bár az alacsony szintű objektumok fájlformátuma és API-ja nyilvános, és a fejlesztők támogatják a módosításukat, nem ajánlott megváltoztatni a köztes formátumok tartalmát a migrálás során. Azonban szükség lehet teljes ImportObject-bejegyzések eltávolítására a changes.xml fájlból vagy keresési és csereműveletek végrehajtása a pilot.xml fájlon a verziószámok vagy a próbaverziós tartománynévrendszer (DNS) adatai a DNS éles verziójának adataira való lecseréléséhez.

### <a name="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions"></a>A verziók közötti migráláskor győződjön meg arról, hogy a verziószám megfelelő a pilot.xml fájlban

Bár a verziószámok közötti migrálás nem javasolt és nem támogatott, gyakran elvégezheti ezt a próbaverziószámot az éles verziószámra cserélve a pilot.xml fájlban. Pontosabban a WorkflowDefinition és

ActivityInformationConfiguration objektumok igényelnek pontosan az éles környezetbeli munkafolyamat-tevékenységekre vonatkozó verziószámot. Ha a verziószámot nem cseréli le, a Compare-FIMConfig parancsmag azonosítja a WorkflowDefinitions XOML-attribútumai közötti eltéréseket, és migrálja a próbaverzió számát. Az éles környezetbeli FIM szolgáltatás indítása a munkafolyamat-tevékenységek a helytelen verziószámmal sikertelen lehet.

### <a name="avoid-cyclic-references"></a>Kerülje a körkörös hivatkozásokat

Általában körkörös hivatkozások nem ajánlottak MIM-konfigurációkban. Azonban néha előfordulhat körkörös hivatkozás, ha az A készlet a B készletre hivatkozik, és a B is hivatkozik az A-ra. A körkörös hivatkozások okozta problémák elkerülése érdekében módosítania kell az A vagy a B készlet definícióját, hogy mindkettő ne hivatkozzon egymásra. Ezután indítsa újra a migrálási folyamatot. Ha körkörös hivatkozások vannak, és a Compare-FIMConfig parancsmag hibát eredményez, manuálisan kell a körkörös hivatkozást megszüntetni. A Compare-FIMConfig parancsmag kimenete elsőbbségi sorrendben jeleníti meg a szükséges módosítások listáját, ezért a konfigurációs objektumok hivatkozásai között nem szerepelhet körkörös hivatkozás.

## <a name="security"></a>Biztonság

### <a name="mim-ma-account"></a>MIM MA-fiók

A MIM MA-fiók nem számít szolgáltatásfióknak, és egy szokásos felhasználói fióknak kell lennie. A fiókoknak be kell tudniuk jelentkezni helyileg ahhoz, hogy a FIM szinkronizálási szolgáltatás szolgáltatásfiókja megszemélyesíthesse.

A MIM MA-fiók helyi bejelentkezésének engedélyezéséhez:

1.  Kattintson a Start gombra, mutasson a Felügyeleti eszközök pontra, és kattintson a Helyi biztonsági házirend elemre.

2.  Nyissa meg a Helyi házirendek csomópontot, majd kattintson a Felhasználói jogok kiosztása elemre.

3.  A Helyi bejelentkezés engedélyezése házirendben győződjön meg arról, hogy a FIM MA-fiók explicit módon van-e megadva, vagy adja hozzá egy olyan csoporthoz, amely már rendelkezik hozzáféréssel.

### <a name="fim-synchronization-service-and-fim-services-accounts"></a>A FIM szinkronizálási szolgáltatás és a FIM Services-fiókok

A MIM-kiszolgáló összetevőt futtató kiszolgálók biztonságos módon való konfigurálásához a szolgáltatásfiókokat korlátozni kell. Az előző eljárással engedélyezve a MIM MA-fiókot állítsa be a FIM szinkronizálási szolgáltatás és a FIM Services-fiókokat a következő korlátozásokkal:

-   Kötegelt feladatként való bejelentkezés megtagadása

-   Helyi bejelentkezés megtagadása

-   A számítógép hálózati elérésének megtagadása

A szolgáltatásfiókok nem lehetnek a helyi Rendszergazdák csoport tagjai.

A FIM szinkronizálási szolgáltatás szolgáltatásfiók nem lehet a FIM szinkronizálási szolgáltatáshoz való hozzáférés szabályozása használt biztonsági csoport (például a FIMSync kezdetű csoportok, mint a FIMSyncAdmins, és így tovább) tagja.

> [!IMPORTANT]
>  Ha a készletekben ugyanazon fiók használatát választja mindkét szolgáltatási fiókhoz, és szétválasztja a FIM szolgáltatást és a FIM szinkronizációs szolgáltatást, akkor az mms Szinkronizációs szolgáltatás kiszolgálóján nem tilthatja le a hozzáférést ehhez a számítógéphez a hálózatról. Ha a hozzáférés meg lenne tagadva, ez megtiltaná a FIM szolgáltatás számára, hogy kapcsolatba lépjen a FIM szinkronizációs szolgáltatással a konfiguráció megváltoztatásához és a jelszavak kezeléséhez.

### <a name="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile"></a>A kioszkmódhoz hasonló módon üzemelő számítógépekre telepített jelszó-visszaállításnak helyi biztonsági beállítással kell rendelkeznie a virtuális memória lapozófájljának törléséhez

A FIM jelszó-visszaállítás kioszkként használandó munkaállomáson történő telepítéskor javasoljuk, hogy kapcsolja be a helyi biztonsági házirend Leállítás: Virtuális memória lapozófájljának törlése beállítását annak biztosítására, hogy a folyamat memóriájából származó érzékeny információkhoz jogosulatlan felhasználók ne férhessenek hozzá.

### <a name="implementing-ssl-for-the-fim-portal"></a>SSL implementálása a FIM-portálon

Határozottan javasoljuk, hogy használjon SSL-t a FIM-portál kiszolgálóján az ügyfelek és a kiszolgáló közötti adatforgalom biztonságossá tétele érdekében.

Az SSL implementálásához:

1.  A MIM-portál kiszolgálóján nyissa meg az IIS Manager alkalmazást.

2.  Kattintson a helyi számítógép nevére.

3.  Kattintson a Kiszolgálói tanúsítványok elemre.

4.  Kattintson a Tanúsítványkérelem létrehozása elemre.

5.  A Köznapi név mezőben adja meg a kiszolgáló nevét.

6.  Kattintson a Tovább, majd ismét a Tovább gombra.

7.  Mentse tetszőleges helyre a fájlt. A későbbi lépésekben szüksége lesz a hely elérésére.

8.  Nyissa meg a következő címet: https://servername/certsrv. A „kiszolgálónév” részt cserélje le a tanúsítványokat kiállító kiszolgáló nevére.

9.  Kattintson az Új tanúsítvány kérése lehetőségre.

10. Kattintson a Speciális kérelem küldése lehetőségre.

11. Kattintson a Tanúsítványkérelem továbbítása base-64 kódolású fájl használatával lehetőségre.

12. Illessze be a fájlt, amelynek tartalmát az előző lépésben mentette.

13. A Tanúsítványsablon csoportban válassza a Webkiszolgáló elemet.

14. Kattintson a Küldés gombra.

15. Mentse a tanúsítványt asztali számítógépére.

16. Az IIS-kezelőben kattintson a Tanúsítványkérelem kitöltése lehetőségre.

17. Irányítsa az IIS-kezelőt az asztali számítógépére nemrég mentett tanúsítványra.

18. A Rövid név mezőbe írja a kiszolgáló nevét.

19. A Helyek gombra kattintva válassza a SharePoint – 80-as port lehetőséget.

20. Kattintson a Kötések, majd a Hozzáadás lehetőségre.

21. Jelölje be a https elemet.

22. A tanúsítványként jelölje ki azt, amelynek neve megegyezik a kiszolgálóéval (azaz az imént importált tanúsítványt).

23. Kattintson az OK gombra.

24. Távolítsa el a HTTP-kötést.

25. Kattintson az SSL-beállításokra, majd jelölje be az SSL megkövetelése négyzetet.

26. Mentse a beállításokat.

27. Kattintson a Start menü Felügyeleti eszközök pontjára, majd kattintson a Központi SharePoint-felügyelet 3.0-s verzió elemre.

28. Kattintson a Tevékenység, majd a Másodlagos címek leképezése elemre.

29. Kattintson a https://servername gombra.

30. Váltson https://servername a értékre https://servername , majd kattintson az OK gombra.

31. Kattintson a Start gombra, majd a Futtatás parancsra, írja be az iisreset parancsot, majd kattintson az OK gombra.

## <a name="performance"></a>Teljesítmény

Az optimális teljesítménykonfigurációhoz:

-   Alkalmazza a jelen a dokumentum SQL-telepítő szakaszában leírt ajánlott eljárásokat.

-   Kapcsolja ki a SharePoint-indexelést a webszolgáltatási portál webhelyén. További tudnivalókat a jelen dokumentum SharePoint-indexelés letiltása című szakaszában találhat.

## <a name="feature-specific-best-practices"></a>Szolgáltatásokra vonatkozó ajánlott eljárások 


### <a name="request-management"></a>Kérelmek kezelése

Alapértelmezés szerint a MIM 2016 kiüríti a lejárt rendszerobjektumokat, amelyek 30 napos intervallumon belül tartalmazzák a befejezett kéréseket a hozzá kapcsolódó jóváhagyásokkal, a jóváhagyási válaszokkal és a munkafolyamat-példányokkal együtt. Ha a szervezetnek hosszabb kérelemelőzményekre van szüksége, exportálnia kell a MIM-től érkező kéréseket és tárolnia kell őket egy kiegészítő adatbázisban, hogy megőrizze őket a 30 napos időszakon túl. Míg a 30 napos kérelemtörlési időszak konfigurálható, az időszak kiterjesztése negatívan befolyásolhatja a teljesítményt a rendszer további objektumai miatt.

### <a name="management-policy-rules"></a>Felügyeletiházirend-szabályok

#### <a name="use-the-appropriate-mpr-type"></a>Használja a megfelelő MPR-típust

A MIM kétféle MPR-t használ, a kérelmet és a készletátmenetet:

- Kérelem-MPR (RMPR)

  - Az erőforrásokon végzett műveletek létrehozási, olvasási, frissítési vagy törlési (CRUD) hozzáférés-vezérlési házirendjének (hitelesítési, engedélyezési és műveleti) meghatározásához használatos.
  - Akkor alkalmazható, ha a rendszer egy szifilisz-műveletet állít ki a többoldalas tárolóban lévő cél erőforráson.
  - Hatóköre a szabályban megadott megfelelési feltételek, azaz hogy mely CRUD-kérelmekre vonatkozik a szabály.

- Készletátmenet-MPR (TMPR)
  - A szabályzat meghatározására használatos függetlenül attól, hogy az objektum milyen módon lépett a készletátmenet által jelzett aktuális állapotba. Használja a TMPR-t jogosultsági szabályzatok modellezésére.
  - Akkor alkalmazza a rendszer, amikor egy erőforrás belép egy kapcsolódó készletbe, vagy elhagyja azt.
  - Hatóköre a készlet tagjaira terjed ki.

> [!NOTE]
> További részletekért tekintse meg az [üzleti házirend szabályainak megtervezése](https://go.microsoft.com/fwlink/?LinkID=183691)című témakört.

#### <a name="only-enable-mprs-as-necessary"></a>Csak szükség esetén engedélyezze az MPR-eket

A konfiguráció alkalmazásakor használja a legalacsonyabb jogosultsági szint elvét. A MPR szabályozza a hozzáférési házirendet a webhelyen üzemelő példányához. Csak a felhasználók többsége által használt szolgáltatásokat engedélyezze. Például nem minden felhasználó használja a felügyeleti webszolgáltatásokat a csoport kezeléséhez, ezért a társított csoport-felügyeleti MPR le kell tiltani. Alapértelmezés szerint a felügyeleti webkiszolgálók a legtöbb nem rendszergazdai jogosultsággal rendelkező hajót letiltják.

#### <a name="duplicate-built-in-mprs-instead-of-directly-modifying"></a>A beépített MPR-ek közvetlen módosítása helyett másolja azokat
Ha módosítani szeretné a beépített MPR-eket, létre kell hoznia egy új MPR-t a szükséges konfigurációval és ki kell kapcsolnia a beépített MPR-t. Ez biztosítja, hogy a beépített MPR-ek a frissítési folyamaton keresztül bevezetett jövőbeli módosításai nem lesznek negatív hatással a rendszerkonfigurációra.

#### <a name="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs"></a>A végfelhasználói engedélyeknek a felhasználók üzleti igényeihez kötött kifejezett attribútumlistákat kell használnia
A kifejezett attribútumlisták használatával megakadályozható az engedélyek véletlen megadása a rendszerjogosultsággal nem rendelkező felhasználók számára, amikor attribútumokat adnak hozzá objektumokhoz. A rendszergazdáknak explicit módon kell hozzáférést adniuk az új attribútumokhoz a hozzáférés eltávolítása helyett.

Az adatok elérését a felhasználók üzleti igényeihez kell kötni. Például a csoportok tagjai nem férhetnek hozzá azon csoport szűrőattribútumához, amelyhez tartoznak. A szűrő véletlenül olyan szervezeti adatokat fedhet fel, amelyekhez a felhasználó normál esetben nem férhetne hozzá.

#### <a name="mprs-should-reflect-effective-permissions-in-the-system"></a>Az MPR-eknek a rendszerbeli vonatkozó engedélyeket kell tükrözniük
Kerülje engedélyek megadását olyan attribútumokhoz, amelyeket a felhasználó soha nem használhat. Például nem adhat engedélyt az alapvető erőforrás-attribútumok, például az objectType módosítására. Az MPR ellenére a rendszer megtagadja az erőforrástípus létrehozását követő bármilyen módosítási kísérletet.

#### <a name="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs"></a>Az olvasási engedélyeknek el kell különülniük a módosítási és létrehozási engedélyektől, ha explicit attribútumokat használ az MPR-ekben

Az MPR-kben szereplő attribútumok explicit listázásakor a létrehozáshoz és a módosításhoz szükséges attribútumok általában eltérnek az olvasáshoz elérhető attribútumoktól. Például az olvasási engedély megadható a rendszerattribútumok, például a létrehozó vagy az objektumazonosító felett, míg a rendszerattribútumoknál nem lehet megadni a Létrehozás vagy a Módosítás lehetőséget.

#### <a name="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules"></a>A létrehozási engedélyeknek el kell különülniük a módosítási engedélyektől, ha explicit attribútumokat használ a szabályokban

A létrehozási művelet megköveteli, hogy a felhasználó kiválassza az objectType elemet a művelet során. Ez az alapvető rendszerattribútum nem módosítható a létrehozási művelet után.

#### <a name="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements"></a>Egy kérelem-MPR-t használjon az összes attribútumhoz ugyanazon hozzáférési követelményekkel

Olyan azonos hozzáférési követelményekkel rendelkező attribútumok esetén, amelyek várhatóan nem változnak, a hatékonyság érdekében egyetlen kérelem- MPR-ben kombinálhatja azokat.

#### <a name="avoid-giving-unrestricted-access-even-to-selected-principal-groups"></a>Kerülje a korlátlan hozzáférés megadását még a kiválasztott egyszerű rendszercsoportokhoz is

A webszolgáltatásban az engedélyek pozitív állításként vannak meghatározva. Mivel a rendszer nem támogatja a megtagadási engedélyeket, így az erőforrásokhoz való korlátlan hozzáférés megnehezíti az engedélyek kizárásának biztosítását. Ajánlott eljárásként azt javasoljuk, hogy csak a szükséges engedélyeket adja meg.

#### <a name="use-tmprs-to-define-custom-entitlements"></a>Használjon TMPR-eket az egyéni jogosultságok megadásához

Egyéni jogosultságok megadása helyett készletátmenet-MPR-eket (TMPR-eket) használjon. A TMPR-ek állapotalapú modellt biztosítanak a jogosultságok hozzárendeléséhez vagy eltávolításához a definiált átmeneti készletek tagsága, illetve a szerepkörök és hozzájuk tartozó munkafolyamat-tevékenységek alapján. A Tmpr-ket mindig párokban kell meghatározni, egyet a-ba áthelyezett erőforrásokra, egyet pedig az átalakulóban lévő erőforrásokra. Emellett minden egyes áttérési MPR külön munkafolyamatokat kell tartalmaznia a tevékenységek kiépítéséhez és megszüntetéséhez.

> [!NOTE]
> Minden megszüntetési munkafolyamatnak biztosítania kell, hogy a Futtatás szabályzatfrissítéskor attribútum igaz értékű legyen.

#### <a name="enable-the-set-transition-in-mpr-last"></a>Készletátmenet bejövő MPR-jének engedélyezése utolsóként

A TMPR-pár létrehozásakor a készletátmenet bejövő MPR-jének bekapcsolása utolsóként. Ez a sorrend biztosítja, hogy nem marad erőforrás a jogosultsággal, ha hozzáadják, illetve eltávolítják a készletből, miközben a bejövő MPR be van kapcsolva, de mielőtt a kimenő MPR ki van kapcsolva.

#### <a name="workflows-in-tmpr-should-check-target-resource-state-first"></a>A TMPR-beli munkafolyamatok először a célerőforrás állapotát kell ellenőrizzék

A kiépítési munkafolyamatnak először azt ellenőrizniük, hogy a célként megadott erőforrás már ki lett-e építve a jogosultságnak megfelelően. Ha igen, nincs teendő.

A megszüntetési munkafolyamatoknak először azt ellenőrizniük, hogy a célként megadott erőforrás már ki lett-e építve. Ha igen, meg kell szüntetnie azt. Ellenkező esetben nincs teendő.

#### <a name="select-run-on-policy-update-for-tmprs"></a>Válassza a Run On Policy Update (Futtatás szabályzatfrissítéskor) lehetőséget a TMPR-ekhez

Ez biztosítja a megfelelő kiépítési működést a szabályzatfrissítések bevezetésekor és a Run On Policy Update (Futtatás szabályzatfrissítéskor) jelző használatakor a TMPR-ekhez társított munkafolyamatokon. Ez biztosítja, hogy a szabályzatdefiníciók módosításai az átmeneti készlet új tagjai számára is alkalmazzák a munkafolyamatokat.

#### <a name="avoid-associating-the-same-entitlement-with-two-different-transition-sets"></a>Kerülje ugyanazon jogosultság társítását két különböző átmeneti készlethez

Ugyanazon jogosultság társítása két különböző átmeneti készlethez jogosultságok szükségtelen visszavonását és újbóli megadását okozhatja, ha az erőforrás egyik készletből a másikba helyeződik át. Győződjön meg róla, hogy egy készlet az összes olyan erőforrást tartalmazza, amelyhez a társított jogosultság szükséges. Ez biztosítja, hogy egy átmeneti készlethez csak egy, a jogosultságot megadó munkafolyamat tartozhat, és viszont.

#### <a name="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system"></a>A jogosultságok a rendszerből való eltávolításakor használja a megfelelő műveleti sorrendet

A jogosultságok a rendszerből való eltávolításakor végrehajtott lépések sorrendje két különböző működési eredményt eredményezhet. Tisztában kell lennie azzal, hogy melyik sorrend eredményezi az elérni kívánt hatást.

A jogosultság eltávolításához a rendszerből (és visszavonásához a jelenleg azzal rendelkező összes tagtól):

1.  Tiltsa le a készletátmenet bejövő MPR-jét. Ezzel elkerülhetők az új jogosultság-megadások.

2.  Törölje az átmeneti készlet szűrőjét, vagy módosítsa oly módon, hogy a készlet üres legyen. Ez az összes meglévő tag kifelé történő áthelyezését váltja ki, és alkalmazza a kifelé történő áthelyezési szabályzatot, beleértve a jogosultsághoz társított beállított megszüntetési munkafolyamatokat is.

3.  Tiltsa le a készletátmenet kimenő MPR-jét.

Ha el kívánja távolítani a jogosultságokat, de csak az aktuális tagokat hagyja el (például ne használja a felügyeleti webszolgáltatást a jogosultság kezeléséhez):

1.  Tiltsa le a készletátmenet bejövő MPR-jét. Ezzel elkerülhetők az új jogosultság-megadások.

2.  Tiltsa le a készletátmenet kimenő MPR-jét.

3.  Törölje az átmeneti készlet szűrőjét, vagy módosítsa oly módon, hogy a készlet üres legyen. Mivel a készlet már nem kötődik TMPR-hez, nem kerül sor megszüntetési munkafolyamat alkalmazására.

### <a name="sets"></a>Halmazok

A készletekre vonatkozó gyakorlati tanácsok alkalmazásakor vegye figyelembe a kezelhetőség optimalizálása és a jövőbeli egyszerű felügyelet szempontjait. Az itt szereplő ajánlások alkalmazása előtt megfelelő tesztelést kell végezni a várható éles üzemelési körülmények között, hogy meg lehessen határozni a megfelelő egyensúlyt a teljesítmény és a kezelhetőség között.

>[!NOTE]
> Az alábbi irányelvek a dinamikus készletekre és a dinamikus csoportokra vonatkoznak.


#### <a name="minimize-the-use-of-dynamic-nesting"></a>Minimalizálja a dinamikus beágyazás használatát

Ez egy olyan készlet szűrőjére utal, amely egy másik készlet ComputedMember attribútumára hivatkozik. A készletek beágyazásának gyakori indoka, hogy elkerülhető legyen a tagsági feltételek többszörözése a készletekben. Bár ez a módszer a készlet jobb kezelhetőségét eredményezheti, a teljesítményre vonatkozóan kompromisszumot jelent. Úgy optimalizálhatja a teljesítményt, ha ahelyett, hogy magát a készletet ágyazná be, többszörözi a beágyazott készlet tagsági feltételeit.

Előfordulhat, hogy nem kerülheti el a készletek beágyazását a működési követelmények kielégítése érdekében. Az alábbiak a leggyakoribb olyan helyzetek, amelyekben készleteket kell beágyaznia. Például a teljes idejű alkalmazott tulajdonosok nélküli csoportok készletének definiálásához a készletek beágyazását kell használni az alábbiak szerint: `/Group[not(Owner = /Set[ObjectID = ‘X’]/ComputedMember]`, ahol az ’X’ az összes teljes idejű alkalmazott készletének objektumazonosítója.

#### <a name="minimize-the-use-of-negative-conditions"></a>Minimalizálja a negatív feltételek használatát

A negatív feltételek azon tagsági feltételek, amelyek a következő operátorokat vagy függvényeket használják: `!=`, `not()`, `\<` , `\<=`. Ha optimalizálni szeretné a teljesítményt, ahol lehetséges, fejezze ki a feltételeket több pozitív feltétellel, mintsem negatív feltételként.

#### <a name="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes"></a>Minimalizálja a többértékű referenciaattribútumokon alapuló tagsági feltételek használatát

A többértékű referenciaattribútumokon alapuló feltételek használata kerülendő, mert ezen készletek nagy száma befolyásolhatja a tagsági feltételben használt attribútumon végzett műveletek teljesítményét.

### <a name="password-reset"></a>Új jelszó kérése

#### <a name="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile"></a>A jelszó-visszaállításhoz használt kioszkmódban üzemelő számítógépeknél a helyi biztonságot a virtuális memória lapozófájljának törlésére kell beállítani

Ha a rendszerállapot-kezelő jelszavas alaphelyzetbe állítást egy olyan munkaállomáson helyezi üzembe, amely a rendszerindításra alkalmas, javasoljuk, hogy kapcsolja be a virtuális memória lapozófájljának kiürítése helyi biztonsági házirend beállítását, és győződjön meg arról, hogy a folyamat memóriájából származó bizalmas adatok nem érhetők el jogosulatlan felhasználók

#### <a name="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to"></a>A felhasználóknak mindig azon a számítógépen kell regisztrálniuk a jelszó-visszaállításra, amelyen bejelentkeztek

Amikor egy felhasználó megpróbál regisztrálni a jelszó-visszaállításra egy webes portálon keresztül, a rendszer mindig a bejelentkezett felhasználó nevében kezdeményezi a regisztrációt, függetlenül attól, hogy ki jelentkezett be a webhelyre. A felhasználóknak mindig azon a számítógépen kell regisztrálniuk a jelszó-visszaállításra, amelyen bejelentkeztek.

#### <a name="do-not-set-the-avoidpdconwan-registry-key-to-true"></a>Ne állítsa az AvoidPdcOnWan beállításkulcsot igaz értékre

A MIM 2016 jelszó-visszaállításra való használata esetén ne állítsa igaz értékűre az AvoidPdcOnWan beállításkulcsot.

Ha a beállításkulcs értéke igaz, a felhasználó nagy valószínűséggel végighalad a jelszókapukon, az elsődleges tartományvezérlőn (PDC) megtörténik a jelszó-visszaállítás, és megpróbál bejelentkezni. A beállításkulcs miatt a helyi tartományvezérlő nem végzi el a másodlagos érvényesítést az elsődleges tartományvezérlővel, ezért elutasítja a belépési kérelmet. Ha a rendszer elegendő alkalommal tagadta meg a hozzáférést a felhasználótól, sor kerülhet a tartományból való kizárására, és hívnia kell az ügyfélszolgálatot.

#### <a name="do-not-turn-on-logging-of-clear-text-passwords"></a>Ne kapcsolja be a tiszta szöveges jelszavak naplózását

Lehetőség van a tiszta szöveges jelszavak naplózására a diagnosztikai szolgáltatásiszint-nyomkövetés bekapcsolásával a Windows

Communication Foundation (WCF) szolgáltatásban. Ez a beállítás nincs bekapcsolva alapértelmezés szerint, és az éles környezetben bekapcsolása nem ajánlott. Ezek a jelszavak tiszta szöveges elemekként jelennek meg a titkosított SOAP-üzenetekben, amikor a felhasználók a jelszó-visszállításra regisztrálnak. További információ: [Configuring Message Logging](https://go.microsoft.com/fwlink/?LinkID=168572) (Az üzenetnaplózás konfigurálása).

#### <a name="do-not-map-an-authorization-workflow-to-the-password-reset-process"></a>Az engedélyezési munkafolyamat nem feleltethető meg a jelszó-visszállítási folyamatnak

Az engedélyezési munkafolyamatokat nem kapcsolhatja össze egy jelszó-visszaállítási művelettel. A jelszó-visszaállításhoz szinkron válaszra van szükség, és jóváhagyási munkafolyamatok pedig aszinkron tevékenységeket tartalmaznak, például a jóváhagyást.

#### <a name="do-not-map-multiple-action-activities-to-password-reset"></a>Több tevékenység nem feleltethető meg a jelszó-visszaállításnak

Nem lehet összekapcsolni egynél több művelet tevékenységet tartalmazó munkafolyamatot a jelszó-visszaállítási művelettel. Ilyen eset lenne például egy második AD DS jelszó-visszaállítási tevékenység hozzákapcsolása egy jelszó-visszaállítási MPR-hez. Ez a forgatókönyv nem támogatott.

#### <a name="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow"></a>A meglévő munkafolyamatok hozzáadása, eltávolítása vagy módosítása esetén követelje meg az újraregisztrálást

A meglévő munkafolyamatban a hitelesítési tevékenységek hozzáadásakor, eltávolításakor vagy sorrendjének módosításakor mindig jelölje be az újraregisztrálás megkövetelését. Azok a felhasználók, akik megpróbálnak hitelesíteni a jelszó-visszaállításhoz, miután egy tevékenységet hozzáadtak vagy eltávolítottak egy munkafolyamatból, de mielőtt regisztrálták őket, nemkívánatos hatásokkal találkozhatnak.

### <a name="portal-configuration-and-resource-control-display-configuration"></a>Portálkonfiguráció és erőforrásvezérlés-megjelenítési konfiguráció

#### <a name="consider-adding-a-privacy-disclaimer-to-the-user-profile-page"></a>Érdemes megfontolni egy adatvédelmi nyilatkozat hozzáadását a felhasználói profil oldalához

A MIM-ben alapértelmezés szerint egyes felhasználói profilok adatai megjelenhetnek más felhasználók számára. A felhasználók segítése érdekében a rendszergazdáknak fontolóra kell venniük a cég szabályzataival összhangban lévő egyéni szöveg hozzáadását a Felhasználói profil oldalhoz. Egyéni szöveg MIM-portál oldalaihoz történő hozzáadásával kapcsolatos további információkért lásd: Introduction to [Configuring and Customizing the FIM Portal](https://go.microsoft.com/fwlink/?LinkID=165848) (Bevezetés a FIM-portál konfigurálásába és testreszabásába).

### <a name="schema"></a>Séma

#### <a name="do-not-delete-person-or-group-resource-types"></a>Ne törölje a személy vagy csoport típusú erőforrásokat

Bár a Személy és Csoport erőforrástípusok nem alapvető erőforrástípusok, magukat az erőforrásokat és a hozzájuk rendelt attribútumokat ne törölje. A MIM-portál felhasználói felülete megköveteli a Személy és Csoport erőforrástípusok és attribútumaik jelenlétét.

#### <a name="do-not-modify-the-core-attributes"></a>Ne módosítsa az alapvető attribútumokat

13 alapvető, az összes erőforrástípushoz hozzárendelt attribútum van. Semmilyen módon ne módosítsa fennálló kapcsolataikat bármely erőforrástípussal. A 13 alapvető attribútum:

-   CreatedTime

-   Létrehozó

-   DeletedTime

-   Description

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   Területi beállítás

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

Ne töröljön a naplózási követelményektől függő sémaerőforrást

Ne töröljön sémaerőforrásokat, amíg fennálló naplózási követelmények találhatók ezekhez az erőforrásokhoz.

#### <a name="making-regular-expressions-case-insensitive"></a>Kis-és nagybetűk megkülönböztetésének kikapcsolása a reguláris kifejezésekben

A webalkalmazásban hasznos lehet néhány reguláris kifejezés esetén a kis-és nagybetűk megkülönböztetése. A ?!: operátor használatával figyelmen kívül hagyhatja a kis- és nagybetűket. Például az alkalmazott típusa esetén használja az alábbi kifejezést:

`\^(?!:contractor\|full time employee)%.`

#### <a name="calculation-of-the-member-attribute"></a>A tag attribútum kiszámítása

A szinkronizáló vezérlő számára felfedett Tag attribútum ténylegesen a ComputedMembers attribútumnak van megfeleltetve. Ez a feltételek alapján és a manuálisan kijelölt tagok kombinációja. Még ha a mindhárom attribútumot (Filter, ExplicitMembers és ComputedMembers) is hozzáadja, a tag attribútum dinamikus számítása csak a csoport és a készlet erőforrástípusok esetében történik meg.

#### <a name="leading-and-trailing-spaces-in-strings-are-ignored"></a>Kezdő és záró szóközök figyelmen kívül hagyása sztringekben

A webalkalmazásban megadhatja a kezdő és záró szóközöket tartalmazó karakterláncokat, de a falemezrendszer figyelmen kívül hagyja ezeket a szóközöket. Egy kezdő és záró szóközzel rendelkező sztring elküldése esetén a szinkronizáló vezérlő és a webszolgáltatások figyelmen kívül hagyják ezeket a szóközöket.

#### <a name="empty-strings-do-not-equal-null"></a>Az üres sztring nem egyenlő a null értékkel

Az üres karakterláncok nem egyenlőek a webszolgáltatási csomag ezen kiadásában NULL értékkel. Az üres sztringet tartalmazó bemeneti érték érvényes értéknek minősül. Ha nem található bemeneti érték, az null értéknek minősül.

### <a name="workflow-and-request-processing"></a>Munkafolyamat- és a kérelemfeldolgozás

#### <a name="do-not-delete-default-workflows-that-are-shipped-with-mim-2016"></a>Ne törölje a MIM 2016-tal szállított alapértelmezett munkafolyamatokat

A következő munkafolyamatok nem törölhetők, és nem törölhető:

-   Lejárati munkafolyamat

-   Szűrőérvényesítési munkafolyamat a rendszergazdák számára

-   Szűrőérvényesítési munkafolyamat a nem rendszergazdák számára

-   Csoport lejárati értesítése munkafolyamat

-   Csoport érvényesítése munkafolyamat

-   Tulajdonos jóváhagyási munkafolyamata

-   Jelszó-visszaállítási művelet munkafolyamat

-   Jelszó-változtatási hitelesítési munkafolyamat

-   Kérelmező érvényesítése tulajdonoshitelesítéssel

-   Kérelmező érvényesítése tulajdonoshitelesítés nélkül

-   Regisztrációhoz szükséges rendszer-munkafolyamat

#### <a name="do-not-run-two-or-more-approvalactivities-in-parallel"></a>Ne futtasson párhuzamosan két vagy több ApprovalActivity-t

Ne futtasson párhuzamosan két vagy több ApprovalActivity-t. Ha így tesz, előfordulhat, hogy a kérelem az engedélyezési fázisban elakad. A többszörös jóváhagyásokhoz vagy csatoljon nagyobb jóváhagyói listát, vagy adja meg a két tevékenység egymás utáni sorrendjét.

#### <a name="authorization-activities-should-not-modify-mim-resources-data"></a>Az engedélyezési tevékenységek nem módosíthatják MIM-erőforrások adatait

Kerülje a MIM-erőforrásokat, például a függvénykiértékelői tevékenységet a munkafolyamatok részeként módosító tevékenységeket az engedélyezési munkafolyamatokban. Mivel a kérelem véglegesítését a feldolgozás engedélyezési pontjában még nem végezték el, a személyazonosító adatokon végrehajtott módosítások a kérelem esetleges visszautasítása ellenére alkalmazhatók.

### <a name="understanding-fim-service-partitions"></a>A FIM szolgáltatás partícióinak ismertetése

A webszolgáltatások célja, hogy feldolgozza azokat a kérelmeket, amelyeket a konfigurált üzleti szabályzatoknak megfelelően különböző, például a FIM synchronization Service és az önkiszolgáló összetevők kezdeményeznek. A kialakításból fakadóan mindegyik FIM-szolgáltatáspéldány egy logikai csoporthoz tartozik, amely egy vagy több FIM-szolgáltatáspéldányból, más néven FIM-szolgáltatáspartícióból áll. Ha csak egy FIM-szolgáltatáspéldány van telepítve az összes kérelem kezelésére, akkor előfordulhat, hogy feldolgozási késéseket tapasztal. Egyes műveletek akár az önkiszolgáló műveletekhez megfelelő alapértelmezett időtúllépési értékeket is meghaladhatják. A FIM-szolgáltatáspartíciók segíthetnek e probléma megoldásában.

További információ: a [FIM Service Partitions ismertetése](https://social.technet.microsoft.com/wiki/contents/articles/2363.understanding-fim-service-partitions.aspx).

## <a name="next-steps"></a>Következő lépések
- [FIM biztonsági mentési és visszaállítási útmutató](https://go.microsoft.com/fwlink/?LinkID=165864)
- [Hogyan felhasználók szinkronizálása Active Directoryról a FIM-be](https://go.microsoft.com/fwlink/?LinkID=188277) 
- A [helyreállítási modell áttekintése](https://go.microsoft.com/fwlink/?LinkID=185370).
