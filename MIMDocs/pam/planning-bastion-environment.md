---
title: "Megerősített környezet tervezése | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 09/16/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9eefdf21d0cab3f7c488a66cbb3984d40498f4ef
ms.openlocfilehash: fc4161f98d4367a2124e6253fe11dd1f2712d614


---

# Megerősített környezet tervezése

A dedikált felügyeleti erdővel rendelkező megerősített környezetnek az Active Directory-hoz való hozzáadása lehetővé teszi a szervezetek számára a rendszergazdai fiókok, a munkaállomások és a csoportok egyszerű kezelését olyan környezetben, amely erősebb biztonsági rendszabályokat alkalmaz, mint a meglévő éles környezetük.

Ez az architektúra lehetővé teszi számos olyan rendszabály használatát, amely nem vagy nem könnyen konfigurálható az egy erdőt tartalmazó architektúrában. Ez magában foglalja olyan fiókok létesítését rendszerjogosultsággal nem rendelkező, normál felhasználóként a felügyeleti erdőben, amelyek magas szintű jogosultságokkal rendelkeznek az éles környezetben, ami lehetővé teszi a cégirányítási követelmények nagyobb mértékű technikai érvényesítését. Ez az architektúra emellett lehetővé teszi a szelektív hitelesítési funkció, a megbízhatóság használatát is a bejelentkezések (és a hitelesítő adatok veszélyeztetettségének) korlátozására, hogy csak az erre jogosult gazdagépek jelentkezhessenek be. Olyan esetekben, ahol nagyobb szintű megbízhatóságra van szükség az éles környezetben működő erdő számára az ezzel járó költségek és teljes átalakítás nélkül, egy felügyeleti erdő olyan környezetet tud előállítani, amely megnöveli az éles környezet megbízhatósági szintjét.

A dedikált felügyeleti erdő mellett további technikák is használhatók. Ezek közé tartozik annak korlátozása, hogy hol jelennek meg a rendszergazdai hitelesítő adatok, az adott erdőben található felhasználók szerepkör-jogosultságainak korlátozása, valamint annak biztosítása, hogy a felügyeleti feladatok végrehajtása ne a szokásos felhasználói tevékenységekhez (például a levelezéshez és a webböngészéshez) használt gazdagépeken történjen.

## Az ajánlott eljárásokkal kapcsolatos szempontok

A dedikált felügyeleti erdő az Active Directory felügyeletéhez használt szabványos egytartományos Active Directory-erdő. A felügyeleti erdők és tartományok használatának az az előnye, hogy a korlátozott felhasználási eseteik következtében több biztonsági intézkedést alkalmazhatnak, mint az éles környezetben működő erdők. További előny, hogy ez egy elkülönített erdő, amely nem tekinti megbízhatónak a szervezet meglévő erdőit, ezért a többi erdőben felmerülő biztonsági sérülés nem terjed ki erre a dedikált erdőre.

A felügyeleti erdőt a következő szempontok alapján kell megtervezni:

### Korlátozott hatókör

A felügyeleti erdő értéke a magas szintű biztonság és a kisebb támadási felület. Az erdő további felügyeleti funkciókat és alkalmazásokat is tartalmazhat, de a hatókör növelése minden alkalommal növeli az erdő és az erőforrásainak támadási felületét. A cél az erdő funkcióinak korlátozása úgy, hogy a támadási felület minimális méretű maradjon.

A rendszergazdai jogosultságok felosztásának [rétegmodellje](tier-model-for-partitioning-administrative-privileges.md) szerint a dedikált felügyeleti erdőben lévő fiókoknak egy rétegben, jellemzően a 0. vagy az 1. rétegben kell lenniük. Az 1. rétegbeli erdőt célszerű egy meghatározott alkalmazási körre (például pénzügyi alkalmazásokra) vagy felhasználói közösségre (például kiszervezett informatikai szállítókra) korlátozni.

### Korlátozott megbízhatóság

Az éles környezet *CORP* erdőjének megbízhatónak kell tekintenie a felügyeleti *PRIV* erdőt, de ez megfordítva már nem érvényes. Ez lehet tartományi megbízhatóság vagy erdőszintű megbízhatóság. A felügyeleti erdő tartományának nem kell megbízhatónak tekintenie a felügyelt tartományokat és erdőket az Active Directory felügyeletéhez, bár előfordulhat, hogy egyes alkalmazások kétirányú megbízhatósági kapcsolatot, biztonsági ellenőrzést és tesztelést igényelnek.

Szelektív hitelesítés használatával kell biztosítani, hogy a felügyeleti erdőben lévő fiókok csak a megfelelő gazdagépeket használják az éles környezetben. A tartományvezérlők karbantartásához és a jogosultságoknak az Active Directoryban történő delegálásához ez általában a tartományvezérlők „Bejelentkezés engedélyezett” jogosultságának megadását igényli a felügyeleti erdőben lévő 0. rétegbeli rendszergazdai fiókok számra. További információt a [Configuring Selective Authentication Settings (A szelektív hitelesítés beállításainak konfigurálása)](http://technet.microsoft.com/library/cc816580.aspx) című cikkben talál.

## Logikai elkülönítés alkalmazása

A következő irányelveket kell követni a megerősített környezet rendszereinek előkészítésekor ahhoz, hogy a szervezeti Active Directory fennálló vagy jövőbeli biztonsági incidensei ne legyenek hatással a megerősített környezetre:

- A Windows-kiszolgálók esetében nem ajánlott a tartományhoz való csatlakoztatás, illetve meglévő környezetekben található szoftverek felhasználása vagy beállítások terjesztése.

- A megerősített környezetnek saját Active Directory tartományi szolgáltatásokat kell tartalmaznia, valamint Kerberost, LDAP-ot, DNS-t, időt és időszolgáltatásokat kell biztosítania.

- A MIM esetében nem ajánlott SQL-adatbázisfarm használata a meglévő környezetben. Az SQL Servert a megerősített környezetben található dedikált kiszolgálókra kell telepíteni.

- A megerősített környezet a Microsoft Identity Manager 2016-os verzióját igényli, pontosabban a MIM szolgáltatást és a PAM-összetevőket kell telepíteni.

- A megerősített környezet biztonsági mentési szoftverét és adathordozóit el kell különíteni a meglévő erdők rendszereinek hasonló eszközeitől, hogy a meglévő erdőkben lévő rendszergazdák ne tudjanak hozzáférni a megerősített környezet biztonsági mentéséhez.

- A megerősített környezet kiszolgálóit kezelő felhasználóknak olyan munkaállomásokról kell bejelentkezniük, amelyek nem érhetők el a meglévő környezetben lévő rendszergazdák számára, hogy ne szivárogjanak ki a megerősített környezet hitelesítő adatai.

## A felügyeleti szolgáltatások rendelkezésre állásának biztosítása

Mivel az alkalmazások felügyelete átkerül a megerősített környezetbe, vegye figyelembe, hogyan lehet biztosítani a rendelkezésre állást ezen alkalmazások követelményeinek megfelelően. Ennek technikái a következők:

- Telepítsen Active Directory tartományi szolgáltatásokat több számítógépen a megerősített környezetben. Legalább két kiszolgáló szükséges a folyamatos hitelesítés biztosításához akkor is, ha az egyik kiszolgáló ütemezett karbantartás miatt átmenetileg indul újra. További számítógépekre lehet szükség a nagyobb terheléshez vagy a több földrajzi régióban elhelyezkedő erőforrások és a rendszergazdák kezeléséhez.

- Készítsen elő vészhelyzeti fiókokat a meglévő erdőben és a dedikált felügyeleti erdőben, amelyeket vészhelyzetben lehet használni.

- Telepítse az SQL Servert és a MIM szolgáltatást több számítógépre a megerősített környezetben.

- Készítse el az AD és az SQL biztonsági másolatát a dedikált felügyeleti erdőben lévő felhasználók vagy szerepkör-definíciók minden módosításakor.

## Megfelelő Active Directory-engedélyek konfigurálása

A felügyeleti erdőt a legkevesebb szükséges jogosultsággal kell konfigurálni az Active Directory felügyeleti követelményeinek megfelelően.

- A felügyeleti erdőnek az éles környezet felügyeletéhez használt fiókjai számára ne adjon rendszergazdai jogosultságokat a felügyeleti erdőhöz, annak tartományaihoz, illetve az abban lévő munkaállomásokhoz.

- A felügyeleti erdőre vonatkozó rendszergazdai jogosultságokat tartsa szigorú ellenőrzés alatt egy offline folyamattal, amivel csökkenthető annak veszélye, hogy egy támadó vagy egy kártevő szándékú belső felhasználó törli a felügyeleti naplókat. Ez hozzájárul annak biztosításához is, hogy az éles környezet rendszergazdai fiókjaival rendelkező munkatársak ne tudják megengedőbbre állítani a fiókjuk korlátozását, és ne növeljék a szervezetre jelentett veszélyeket.

- A felügyeleti erdőnek követnie kell a Microsoft Security Compliance Manager (SCM) konfigurációit a tartományra vonatkozóan, beleértve a hitelesítési protokoll erős konfigurációit.

A megerősített környezet létrehozásakor a Microsoft Identity Manager telepítése előtt határozza meg és hozza létre azokat a fiókokat, amelyeket a felügyelethez fognak használni ebben a környezetben. Ide tartoznak a következők:

- **Vészhelyzeti fiókok**, amelyek csak a tartományvezérlőkre tudnak bejelentkezni a megerősített környezetben.

- **„Piros kártyás” rendszergazdák**, akik más fiókokat létesíthetnek, és tervezett karbantartást hajthatnak végre. Ezeknek a fiókoknak ne adjon hozzáférést a megerősített környezeten kívüli meglévő erdőkhöz vagy rendszerekhez. A hitelesítő adatokat, például az intelligens kártyákat, fizikai védelemmel kell ellátni, és az ilyen fiókok használatát naplózni kell.

- **Szolgáltatásfiókok** a Microsoft Identity Managerhez, az SQL Serverhez és az egyéb szoftverekhez.

## A gazdagépek megerősítése

A felügyeleti erdőhöz csatlakoztatott összes gazdagépre, beleértve a tartományvezérlőket, a kiszolgálókat és a munkaállomásokat, a legújabb verziójú operációs rendszert és szervizcsomagokat kell telepíteni, és naprakészen kell tartani őket.

- A felügyeleti tevékenységek végrehajtásához szükséges alkalmazásokat előzetesen telepíteni kell a munkaállomásokra, hogy az azokat használó fiókokat ne kelljen felvenni a helyi rendszergazdák csoportjába az alkalmazások telepítéséhez. A tartományvezérlő karbantartása általában az RDP és a Távoli kiszolgálófelügyelet eszközei használatával hajtható végre.

- A felügyeleti erdő gazdagépeit automatikusan frissíteni kell a biztonsági frissítésekkel. Bár ez azzal a veszéllyel járhat, hogy megszakadnak a tartományvezérlő karbantartási műveletei, mégis jelentős mértékben csökkenti a nem javított biztonsági rések kockázatait.

### A felügyeleti gazdagépek azonosítása

A rendszerek vagy a munkaállomások kockázatának meghatározásakor az azokon végzett legnagyobb kockázatú tevékenységet kell figyelembe venni, ilyen lehet például az internet böngészése, az e-mailek küldése és fogadása, illetve az olyan alkalmazások használata, amelyek ismeretlen vagy nem megbízható tartalmakat dolgoznak fel.

A felügyeleti gazdagépek közé a következő számítógépek tartoznak:

- Asztali számítógép, amelyen a rendszergazdai hitelesítő adatokat fizikai módon megadják vagy beírják.

- „Jumpbox” típusú felügyeleti kiszolgálók, amelyeken a felügyeleti munkamenetek és eszközök futnak.

- Minden olyan gazdagép, amelyen felügyeleti tevékenységek folynak, beleértve azokat is, amelyek RDP-ügyfelet futtató normál felhasználói asztali környezetet használnak a kiszolgálók és az alkalmazások távoli felügyeletéhez.

- Kiszolgálók, amelyek a felügyelendő alkalmazásokat futtatják, és amelyeket nem kell elérni a korlátozott felügyeleti üzemmódú RDP vagy a Windows PowerShell távoli eljáráshívásának használatával.

### Dedikált felügyeleti munkaállomások telepítése

Bár kényelmetlen lehet, különítse el a felhasználók számára kijelölt megerősített munkaállomásokat, amelyekhez nagy hatású rendszergazdai hitelesítő adatok lehetnek szükségesek. Fontos, hogy olyan biztonsági szintet adjon meg a gazdagépeknek, amely megegyezik a hitelesítő adatokhoz tartozó jogosultságok szintjével, vagy magasabb annál. A további védelem érdekében vegye fontolóra a következő intézkedéseket:

- **Ellenőrizze a buildhez tartozó összes adathordozó tisztaságát**, hogy csökkentse annak veszélyét, hogy kártevő programot telepít a fő rendszerképpel, vagy kártevő program férkőzik be a telepítési fájlokba letöltés vagy tárolás közben.

- **Biztonsági alapterveket** alkalmazzon kiindulási konfigurációként. A Microsoft Security Compliance Manager (SCM) segítségével konfigurálhatja az alapterveket a felügyeleti gazdagépeken.

- **Biztonságos rendszerindítás** használatával csökkenthető annak veszélye, hogy a támadók vagy a kártevő programok aláíratlan kódot töltenek be a rendszerindítási folyamat során.

- **Szoftverkorlátozások** használatával biztosítható, hogy csak az engedélyezett felügyeleti szoftverek legyenek futtathatók a felügyeleti gazdagépeken. Az ügyfelek az AppLockert használhatják erre a célra – az engedélyezett alkalmazások listájának megadásával – a kártevő programok és a nem támogatott alkalmazások futtatásának megakadályozásához.

- **Teljes kötet titkosítása** a veszély csökkentése érdekében a számítógépek, például a felügyeletei tevékenységekhez távolról használt laptopok fizikai elvesztése esetén.

- **Az USB használatára vonatkozó korlátozások** a fizikai fertőzés elleni védelemhez.

- **A hálózat elkülönítése** a hálózati támadások és a nem szándékos felügyeleti műveletek elleni védekezésképpen. A gazdagépeken futó tűzfalnak az explicit módon szükségesnek jelölteken kívül le kell tiltania az összes bejövő kapcsolatot, valamint le kell tiltania minden szükségtelen kimenő internet-hozzáférést.

- **Kártevőirtó** használata az ismert fenyegetések és a kártevő szoftverek elleni védelem érdekében.

- **Biztonsági rések bezárása** az ismeretlen fenyegetések elleni védelem érdekében, beleértve az Enhanced Mitigation Experience Toolkit (EMET) használatát.

- **A támadási felület elemzése** a Windows ellen irányuló új támadásvektorok érvényesítésének megakadályozásához az új szoftverek telepítése során. Az Attack Surface Analyzer (ASA) és hasonló eszközök segítségével felmérheti a gazdagépek konfigurációs beállításait, és azonosíthatja a szoftver vagy a konfiguráció módosításai kapcsán keletkező támadásvektorokat.

- **Rendszergazdai jogosultságokat** nem ajánlott adni a felhasználók számára a helyi számítógépükön.

- **RestrictedAdmin mód** használata a kimenő RDP-munkamenetekhez, kivéve azokat az eseteket, amikor a szerepkörhöz szükség van ezekre. További információ: [A Windows Server 2012 R2 Távoli asztal szolgáltatások funkciójának újdonságai](https://technet.microsoft.com/library/dn283323.aspx).

Egyes intézkedések extrémnek tűnhetnek, de az elmúlt években nyilvánosságra kerülő esetek jól szemléltetik, hogy a képzett támadók milyen komoly fegyverzetet tudnak bevetni a kiszemelt céljaik ellen.

## A meglévő tartományok előkészítése a megerősített környezet általi felügyeletre

A MIM a PowerShell-parancsmagok használatával létesít megbízhatósági kapcsolatot a meglévő AD-tartományok és a dedikált felügyeleti erdők között a megerősített környezetben. A megerősített környezet telepítését követően, de még a felhasználók és a csoportok igény szerintire való átalakítása előtt a `New-PAMTrust` és a `New-PAMDomainConfiguration` parancsmag frissíti a tartományi megbízhatósági kapcsolatokat, és létrehozza az AD és a MIM számára szükséges összetevőket.

Amikor a meglévő Active Directory-topológia megváltozik, a `Test-PAMTrust`, a `Test-PAMDomainConfiguration`, a `Remove-PAMTrust` és a `Remove-PAMDomainConfiguration` parancsmag használható a megbízhatósági kapcsolatok frissítésére.

## Megbízhatósági kapcsolat létrehozása az egyes erdőkhöz

A `New-PAMTrust` parancsmagot le kell futtatni egyszer minden meglévő erdő esetében. A parancsmagot a MIM szolgáltatást tartalmazó számítógépen kell elindítani a felügyeleti tartományban. A parancs paraméterei a következők: a meglévő erdő legfelső szintű tartományának neve, valamint az adott tartomány rendszergazdájának hitelesítő adatai.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

A megbízhatósági kapcsolat létrehozása után konfigurálja az egyes tartományokat úgy, hogy engedélyezzék a megerősített környezet általi felügyeletet (ennek leírását lásd a következő szakaszban).

## A tartományok felügyeletének engedélyezése

A meglévő tartományok felügyeletének engedélyezésére hét követelmény vonatkozik.

### 1. Biztonsági csoport a helyi tartományban

A meglévő tartományban kell lennie egy csoportnak, amelynek neve megegyezik a tartomány NetBIOS-nevével és három dollárjel szerepel a végén, például *CONTOSO$$$*. A csoport hatókörének *tartományi helyi csoportnak*, a típusának pedig *Biztonság* értékűnek kell lennie. Ez az olyan csoportok esetében szükséges, amelyek a dedikált felügyeleti erdőben lesznek létrehozva a tartomány csoportjainak biztonsági azonosítójával megegyező azonosítóval. Ez a csoport a következő PowerShell-paranccsal hozható létre úgy, hogy a parancsot a meglévő tartomány rendszergazdája futtatja a meglévő tartományhoz csatlakoztatott munkaállomáson:

```
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

### 2. Sikeres és sikertelen események naplózása

A tartományvezérlő naplózásra vonatkozó csoportházirend-beállításainak tartalmazniuk kell a fiókkezelés naplózásának és a címtárszolgáltatás-hozzáférés naplózásának sikeres és a sikertelen eseményeit is. Ez a Csoportházirend kezelése konzolon hajtható végre úgy, hogy a műveletet a meglévő tartomány rendszergazdája végzi el a meglévő tartományhoz csatlakoztatott munkaállomáson:

3. Válassza a **Start** > **Felügyeleti eszközök** > **Csoportházirend-kezelés** lehetőséget.

4. Lépjen az **Erdő: contoso.local** > **Tartományok** > **contoso.local** > **Tartományvezérlők** > **Alapértelmezett tartományvezérlői házirend** elemhez. Ekkor megjelenik egy tájékoztató üzenet.

    ![Alapértelmezett tartományvezérlői házirend – képernyőkép](media/pam-group-policy-management.jpg)

5. Kattintson a jobb gombbal az **Alapértelmezett tartományvezérlői házirend** elemre, és válassza a **Szerkesztés** parancsot. Megjelenik egy új ablak.

6. A Csoportházirendkezelés-szerkesztő ablakában, az Alapértelmezett tartományvezérlői házirend konzolfáján keresse meg a **Számítógép konfigurációja** > **Házirendek** > **A Windows beállításai** > **Biztonsági beállítások** > **Helyi házirend** > **Naplórend** elemet.

    ![Csoportházirendkezelés-szerkesztő – képernyőkép](media/pam-group-policy-management-editor.jpg)

5. A részletek ablaktábláján kattintson a jobb gombbal a **Fiókkezelés naplózása** elemre, és válassza a **Tulajdonságok** parancsot. Válassza ki **A következő házirend-beállítások megadása** lehetőséget, jelölje be a **Sikeres** és a **Sikertelen** beállítást, majd kattintson az **Alkalmaz** és az **OK** gombra.

6. A részletek ablaktáblájában kattintson a jobb gombbal a **Címtárszolgáltatás-hozzáférés naplózása** elemre, és válassza a **Tulajdonságok** parancsot. Válassza ki **A következő házirend-beállítások megadása** lehetőséget, jelölje be a **Siker** jelölőnégyzetet, jelölje be a **Hiba** jelölőnégyzetet, és kattintson az **Alkalmaz**, majd az **OK** gombra.

    ![Sikeres és sikertelen házirend-beállítások – képernyőkép](media/pam-group-policy-management-editor2.jpg)

7. Zárja be a Csoportházirendkezelés-szerkesztő ablakát és a Csoportházirend kezelése ablakot. Ezután a naplózási beállítások alkalmazásához nyisson meg egy PowerShell-ablakot, és írja be a következőt:

    ```
    gpupdate /force /target:computere
    ```

Néhány perc elteltével „A számítógép-házirend frissítése sikeresen befejeződött” üzenetnek kell megjelennie.

### 3. A helyi biztonsági szervezethez való csatlakozás engedélyezése

A tartományvezérlőknek engedélyezniük kell a TCP/IP feletti RCP-kapcsolatokat a helyi biztonsági szervezet (LSA) számára a megerősített környezetből. A Windows Server korábbi verziói esetében a beállításjegyzékben kell engedélyezni az LSA TCP/IP-támogatását:

```
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

### 4. A PAM tartománykonfigurációjának létrehozása

A `New-PAMDomainConfiguration` parancsmagot a MIM szolgáltatást tartalmazó számítógépen kell futtatni, a felügyeleti tartományban. A parancs paraméterei a következők: a meglévő tartomány neve, valamint az adott tartomány rendszergazdájának hitelesítő adatai.

```
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

### 5. Olvasási engedélyek hozzárendelése a fiókokhoz

A megerősített erdőben a szerepkörök létrehozására használt fiókoknak (a `New-PAMUser` és a `New-PAMGroup` parancsmagot használó rendszergazdáknak), valamint a MIM figyelő szolgáltatása által használt fióknak olvasási engedélyekkel kell rendelkeznie az adott tartományban.

A következő lépések véghajtásával adhat meg olvasási hozzáférést a *PRIV\Rendszergazda* felhasználónak a *Contoso* tartományhoz a *CORPDC* tartományvezérlőn belül:

1. Győződjön meg arról, a Contoso tartomány rendszergazdájaként (például a Contoso\Rendszergazda fiókkal) jelentkezett be a CORPDC tartományvezérlőre.

2. Nyissa meg az Active Directory - felhasználók és számítógépek beépülő modult.

3. Kattintson a jobb gombbal a **contoso.local** tartományra, és válassza a **Vezérlés delegálása** parancsot.

4. A Kijelölt felhasználók és csoportok lapon kattintson a **Hozzáadás** gombra.

5. A Felhasználók, számítógépek vagy csoportok kiválasztása előugró ablakban kattintson a **Helyek** elemre, és váltson át a *priv.contoso.local* helyre. Az objektum nevéhez írja be a *Tartományi rendszergazdák* értéket, és kattintson a **Névellenőrzés** gombra. Amikor megjelenik egy előugró ablak, írja be a *priv\rendszergazda* felhasználónevet és a jelszót.

6. A Tartományi rendszergazdák név után írja be a *; MIMMonitor* értéket. Amikor megjelenik az aláhúzás a Tartományi rendszergazdák és a MIMMonitor név alatt, kattintson az **OK**, majd a **Tovább** gombra.

7. A gyakori feladatok listáján jelölje ki **Az összes felhasználói információ olvasása** elemet, és kattintson a **Tovább**, majd a **Befejezés** gombra.

18. Zárja be az Active Directory – felhasználók és számítógépek beépülő modult.

### 6. Vészhelyzeti fiók

Ha a rendszerjogosultságú hozzáférések felügyeletére irányuló projektnek az a célja, hogy csökkentse a tartományhoz tartósan hozzárendelt, tartományi rendszergazdai jogosultságokkal rendelkező fiókok számát, akkor a tartományban létre kell hozni egy *vészhelyzeti fiókot* arra az esetre, ha a későbbiekben probléma adódna a megbízhatósági kapcsolattal. Minden tartományban léteznie kell az éles környezetben működő erdőhöz vészhelyzeti hozzáféréssel rendelkező fióknak, és úgy kell beállítani ezeket a fiókokat, hogy csak a tartományvezérlőkre tudjanak bejelentkezni. A több hellyel rendelkező szervezetek esetében további fiókokra lehet szükség a redundancia biztosításához.

### 7. Az engedélyek frissítése a megerősített környezetben

Tekintse át az adott tartomány Rendszer tárolójában található *AdminSDHolder* objektum engedélyeit. Az *AdminSDHolder* objektumhoz egyedi hozzáférés-vezérlési lista (ACL) tartozik. Ennek használatával felügyelhetők azoknak a rendszerbiztonsági tagoknak az engedélyei, akik a magas jogosultsági szintű, beépített Active Directory-csoportok tagjai. Vegye figyelembe, hogy az alapértelmezett engedélyeknek a tartomány rendszergazdai jogosultságokkal rendelkező felhasználóit érintő bármilyen módosítása esetén ezek az engedélyek nem lesznek érvényesek azokra a felhasználókra, akiknek fiókja a megerősített környezetben található.

## A felhasználók és a csoportok kiválasztása

A következő lépése a PAM-szerepkörök konfigurálása, valamint azon felhasználók és csoportok hozzárendelése, amelyekhez a szerepköröknek hozzá kell férniük. Ez általában a megerősített környezetben felügyelt réteg felhasználóinak és csoportjainak részhalmaza lesz. További információ: [Szerepkörök definiálása az emelt szintű hozzáférések felügyeletéhez](defining-roles-for-pam.md).



<!--HONumber=Sep16_HO3-->


