---
title: Privileged Access Management az Active Directory Domain Serviceshez | Microsoft Docs
description: További információk a Privileged Access Managementről, valamint az Active Directory-környezet kezelésében és védelmében elfoglalt szerepéről.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
experimental: true
experiment_id: kgremban_images
ms.openlocfilehash: 351a516ccb6a529ca27b157508b06af46f3d243a
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010727"
---
# <a name="privileged-access-management-for-active-directory-domain-services"></a>Privileged Access Management az Active Directory tartományi szolgáltatásokhoz

A webszolgáltatási Privileged Access Management (PAM) olyan megoldás, amely lehetővé teszi a szervezetek számára, hogy egy meglévő és elkülönített Active Directory környezetben korlátozzák a privilegizált hozzáférést.

A Privileged Access Management két célt ér el:

- Visszaszerzi a felügyeletet a sérült biztonságú Active Directory-környezetek fölött: különálló megerősített környezetet működtet, amelyet biztosan nem érintettek a rosszindulatú támadások.
- Elszigeteli a rendszerjogosultságú fiókok használatát, hogy mérsékelje az ilyen hitelesítő adatok ellopásának kockázatát.

> [!NOTE]
> A fakiszolgáló PAM nem különbözik a [Azure Active Directory Privileged Identity Managementtól](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM). A rendszerbe állítás a helyi Active Directory-környezetek elkülönítésére szolgál. Az Azure AD PIM olyan szolgáltatás az Azure AD-ben, amely lehetővé teszi az Azure AD-ben, az Azure-ban és más Microsoft Online-szolgáltatásokban (például Microsoft 365 vagy Microsoft Intune) lévő erőforrásokhoz való hozzáférés kezelését, vezérlését és figyelését. A helyszíni internetkapcsolattal rendelkező környezetekkel és a hibrid környezetekkel kapcsolatos útmutatásért lásd a [privilegizált hozzáférés biztonságossá tétele](/security/compass/overview) című témakört.

## <a name="what-problems-does-mim-pam-help-solve"></a>Milyen problémák megoldására van a webszolgáltatások PAM-megoldása?

Napjainkban túl egyszerű a támadók számára a Tartománygazdák fiók hitelesítő adatainak beszerzése, és túl nehéz felderíteni ezeket a támadásokat a tény után. A PAM célja, hogy a rosszindulatú felhasználók kisebb eséllyel szerezhessenek hozzáférést, Ön pedig nagyobb mértékben kontrollálhassa és felügyelhesse környezetét.

A PAM megnehezíti a támadók számára, hogy behatoljanak a hálózatokra, és rendszerjogosultságú fiókot használjanak. A PAM további védelemmel óvja a rendszerjogosultságú csoportokat, amelyek szabályozzák a hozzáférést sok, a tartományhoz csatlakoztatott számítógéphez és rajtuk futó alkalmazáshoz. További monitorozást, nagyobb láthatóságot és részletesebb szabályozást is biztosít. Ez lehetővé teszi a szervezetek számára, hogy meglássák, kik a Kiemelt rendszergazdák, és mit csinálnak. A PAM révén a szervezetek jobban tájékozódhatnak arról, hogy milyen műveletekre kerül sor a környezetükben a rendszergazdai fiókok használatával.

A felügyeleti csomag PAM-alapú megközelítése olyan elszigetelt környezetek egyéni architektúrájában használható, ahol az Internet-hozzáférés nem érhető el, ahol ez a konfiguráció szükséges a szabályozáshoz, vagy nagy hatással van az elkülönített környezetekre, például az offline kutatási laboratóriumokra és a leválasztott operatív technológiákra, illetve a felügyeleti és adatgyűjtési környezetekre. Ha a Active Directory egy internetkapcsolattal rendelkező környezet részét képezi, további információért lásd: a [privilegizált hozzáférés biztonságossá tétele](/security/compass/overview) .

## <a name="setting-up-mim-pam"></a>A kiállító FAA PAM beállítása

A PAM a szükséges időben (just-in-time) történő felügyelet elvére épül, amely összefügg az [éppen elég felügyelettel (just enough administration, JEA)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362). A JEA egy Windows PowerShell-eszközkészlet, amely a Kiemelt tevékenységek végrehajtásához használható parancsokat határoz meg. Ez egy olyan végpont, amelyben a rendszergazdák engedélyt kaphatnak a parancsok futtatására. A JEA-ban egy rendszergazda dönti el, hogy milyen jogosultságra van szükségük a felhasználóknak ahhoz, hogy elvégezzenek egy feladatot. Minden alkalommal, amikor egy jogosult felhasználónak el kell végeznie ezt a feladatot, aktiválják ezt az engedélyt. Meghatározott idő elteltével az engedélyek lejárnak, hogy egy rosszindulatú felhasználó el ne lophassa a hozzáférési jogosultságot.

A PAM üzembe helyezése és működtetése négy lépésből áll.

![A PAM lépései: előkészítés, védelem, működtetés, figyelés – ábra](media/MIM_PIM_SetupProcess.png)

1. **Előkészítés**: Azonosítsa azokat a csoportokat, amelyek magas jogosultságszinttel rendelkeznek a meglévő erdőn belül. Hozza létre ezeket a csoportokat tagok nélkül a megerősített erdőben.
2. **Védelem**: az életciklus-és hitelesítési védelem beállítása, ha a felhasználók igény szerinti felügyeletet igényelnek. 
3. **Működtetés**: Ha egy felhasználói fiók megfelel a hitelesítési követelményeknek, és jóváhagyják a kérését, átmenetileg bekerül a megerősített erdő rendszerjogosultságú csoportjába. Ezt követően a rendszergazda az előre megadott időtartamra rendelkezik minden olyan jogosultsággal és engedéllyel, amely hozzá van rendelve ehhez a csoporthoz. Ennek az időnek az elteltével a fiók törlődik a csoportból.
4. **Figyelés**: A PAM segítségével naplózhatók a magas jogosultságszint iránti kérések, riasztások köthetők hozzájuk, valamint jelentések készíthetők róluk. Megtekinthető, hogy mikor fért hozzá valaki emelt szintű jogosultságokkal a rendszerhez, és ki végzett egy adott tevékenységet. Megállapítható, hogy a tevékenység megfelel-e a szabályoknak, és könnyen felismerhetők a jogosulatlan tevékenységek (ha például valaki megpróbál közvetlenül hozzáadni egy felhasználót az eredeti erdő valamelyik rendszerjogosultságú csoportjához). Ez a lépés nemcsak a kártékony szoftverek azonosítást segíti, hanem a „belső” támadók figyelését is.

## <a name="how-does-mim-pam-work"></a>Hogyan működik a webalkalmazás-PAM?

A PAM az AD DS új képességein alapul, különösen a tartományi fiókok hitelesítésének és engedélyezésének új lehetőségein, valamint a Microsoft Identity Manager új funkcióin. A PAM leválasztja a rendszerjogosultságú fiókokat a meglévő Active Directory-környezettől. Ha egy rendszerjogosultságú fiókot kell használni, akkor ezt először kérelmezni kell, majd jóvá kell hagyni. A jóváhagyást követően a rendszerjogosultságú fiók nem a felhasználó vagy az alkalmazás aktuális erdejében kap engedélyt, hanem egy új megerősített erdő külső rendszercsoportjának tagjaként. A megerősített erdő használata szélesebb körű szabályozást tesz lehetővé a szervezet számára, például arra vonatkozóan, hogy mikor lehet egy felhasználó tagja egy rendszerjogosultságú fióknak, és hogy a felhasználónak hogyan kell hitelesítenie magát.

Az Active Directory, a MIM szolgáltatás és ennek a megoldásnak az egyéb részei magas rendelkezésre állású konfigurációban is üzembe helyezhetők.

A következő példa részletesebben bemutatja, hogy miképpen működik a PIM.

![A PIM folyamata és résztvevői – ábra](media/MIM_PIM_howitworks.png)

A megerősített erdő korlátozott időre szóró csoporttagságokat oszt ki, amelyek pedig időben korlátozott jegymegadó jegyeket (TGT) állítanak elő. A Kerberos-alapú alkalmazások és szolgáltatások akkor fogadják el az ilyen TGT-ket, illetve tartatják be az engedélyeiket, ha az alkalmazások és szolgáltatások olyan erdőkben léteznek, amelyek megbíznak a megerősített erdőben.

A napi munka során használt felhasználói fiókoknak nem kell új erdőbe költözniük. Ugyanez igaz a számítógépekre, az alkalmazásokra és csoportjaikra. Ugyanott maradnak, ahol most is vannak, egy meglévő erdőben. Vegyünk például egy olyan szervezetet, amelyet aggasztanak napjaink említett számítógépes biztonsági problémái, de még nem tervezi, hogy rövidesen frissíti a kiszolgálói infrastruktúráját a Windows Server következő verziójára. Ez a szervezet ennek ellenére élhet ennek az egyesített megoldásnak az előnyeivel: ehhez az MIM-et és egy új megerősített erdőt kell használnia, aminek köszönhetően jobban tudja szabályozni a hozzáférést a meglévő erőforrásaihoz.

A PAM a következő előnyöket nyújtja:

- **A jogosultságok izolálása/hatókörének kezelése**: A felhasználók nem rendelkeznek magas szintű jogosultságokkal olyan fiókokban, amelyeket hétköznapi feladatokra (például az e-mailjeik elolvasására vagy internetböngészésre) használnak. A felhasználóknak kérniük kell a magas szintű jogosultságokat. A kérések jóváhagyása és elutasítása a PAM-rendszergazda által definiált MIM-szabályzatok alapján történik. Mindaddig nem vehető igénybe rendszerjogosultságú hozzáférés, amíg nem engedélyezik a kérést.

- **Jogosultságemelés (step-up) és hitelesítésiszint-emelés (proof-up)**: Ezek új hitelesítési és engedélyezési kérések, amelyek a külön rendszergazdai fiókok életciklusának kezelését segítik. A felhasználó kérheti egy rendszergazdai fiók jogosultságainak megemelését. A kérés a MIM munkafolyamatain halad végig.

- **További naplózás**: A MIN beépített munkafolyamatain kívül a PAM további naplózási műveleteket végez. Ezek azonosítják a kérést, az engedélyezését, illetve a jóváhagyás után esetleg bekövetkező eseményeket.

- **Testre szabható munkafolyamat**: A MIM munkafolyamatai különböző helyzetek kezelésére konfigurálhatók, és többféle munkafolyamat is használható, a kérelmező felhasználó vagy a kért szerepkörök alapján.

## <a name="how-do-users-request-privileged-access"></a>Hogyan kérik a felhasználók a rendszerjogosultságú hozzáférést?

A felhasználók a következő felületeken küldhetik be a kéréseket:

- A MIM szolgáltatások webszolgáltatási API-ja
- REST-végpont
- Windows PowerShell (`New-PAMRequest`)

Megismerheti az [Emelt szintű hozzáférések felügyeletének parancsmagjai](https://docs.microsoft.com/powershell/identitymanager/mimpam/vlatest/mimpam) című témakör részleteit.

## <a name="what-workflows-and-monitoring-options-are-available"></a>Milyen munkafolyamatok és figyelési lehetőségek érhetők el?

Tegyük fel például, hogy egy felhasználó egy felügyeleti csoport tagja volt a PAM beállítása előtt. A PAM-telepítés részeként a rendszer eltávolítja a felhasználót a felügyeleti csoportból, és létrehoz egy házirendet a webszolgáltatásban. A házirend meghatározza, hogy ha a felhasználó rendszergazdai jogosultságokat kér, a rendszer jóváhagyja a kérést, és a felhasználó számára külön fiókot ad hozzá a megerősített erdőben található privilegizált csoporthoz.

Ha a kérés jóváhagyást nyer, a munkafolyamat közvetlen kommunikációra lép a megerősített erdő Active Directoryjával, hogy az vegye fel a felhasználót egy csoportba. Ha például Ilona engedélyt kér a személyzeti adatbázis felügyeletére, Ilona rendszergazdai fiókja másodperceken belül bekerül a megerősített erdő rendszerjogosultságú csoportjába. Az adott csoportba tartozó rendszergazdai fiók tagsága egy adott időkorlát után lejár. A Windows Server 2016-es vagy újabb verzióiban ez a tagság Active Directory időkorláttal van társítva.

> [!NOTE]
> Ha új tagot vesz fel egy csoportba, ennek a változásnak replikálódnia kell a megerősített erdő többi tartományvezérlőjére. A replikáció késése megakadályozhatja a felhasználókat az erőforrások elérésében. A replikáció késéséről a [How Active Directory Replication Topology Works](https://technet.microsoft.com/library/cc755994.aspx) (Az Active Directory replikációs topológiájának működése) című cikkből tájékozódhat.
>
> Ezzel szemben a lejárt hivatkozást valós időben értékeli ki a biztonsági fiókkezelő (SAM). Még ha a csoporttag felvételét replikálnia is kell annak a tartományvezérlőnek, amely megkapja a kérést, a csoporttag eltávolítását azonnal kiértékeli minden tartományvezérlő.

Ez a munkafolyamat kifejezetten az ilyen rendszergazdai fiókok számára készült. A rendszerjogosultságú fiókokhoz csak alkalmi hozzáférést igénylő rendszergazdák (sőt akár szkriptek is) pontosan ilyen hozzáférést kérelmezhetnek. Az MIM naplózza a kérést és az Active Directoryban bekövetkező változásokat, Ön pedig megtekinthető őket az Eseménynaplóban, vagy elküldheti az adatokat vállalati figyelési megoldásoknak (például a System Center 2012 – Operations Manager naplózási szolgáltatásának (ACS) vagy más külső gyártású eszközöknek).

## <a name="next-steps"></a>További lépések

- [Emelt szintű hozzáférési stratégia](https://docs.microsoft.com/security/compass/privileged-access-strategy)
- [Emelt szintű hozzáférések felügyeletének parancsmagjai](https://docs.microsoft.com/powershell/identitymanager/mimpam/vlatest/mimpam)
