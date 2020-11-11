---
title: BHOLD SP1 – telepítés | Microsoft Docs
description: A BHOLD SP1 telepítési dokumentációja
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 848bdbb793de97eb9512de93febd939bb45a52d3
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492293"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Microsoft BHOLD Suite SP1 (6,0) – telepítési útmutató

A Microsoft® BHOLD Suite Service Pack 1 (SP1) olyan alkalmazások gyűjteménye, amelyek a Microsoft Identity Manager 2016 SP1 () használata esetén hatékony szerepkör-felügyeletet, elemzést és igazolást biztosítanak a felügyeleti csomaghoz. A Microsoft BHOLD Suite SP1 a következő modulokból áll:

- BHOLD mag
- Hozzáférés-kezelési összekötő
- BHOLD FIM/-integráció
- BHOLD Model Generator
- BHOLD-elemzés
- BHOLD-jelentéskészítés
- BHOLD igazolása


> [!NOTE]
> **A** következőkre vonatkozik: Microsoft Identity Manager 2016 SP1 vagy újabb

## <a name="what-this-document-covers"></a>A dokumentum tartalma

Ez a dokumentum azt ismerteti, hogyan tervezze meg az BHOLD üzemelő példányát az üzleti igények kielégítéséhez és az egyes BHOLD-modulok telepítéséhez. Minden modulhoz a kapcsolódó hardver-, infrastruktúra-és szoftver-követelmények, előtelepítési hálózati konfiguráció, a telepítés során szükséges információk és a telepítés utáni teendőkkel lépések (ha vannak) részletesek.

## <a name="pre-requisite-knowledge"></a>Előfeltételek ismerete

Ez a dokumentum azt feltételezi, hogy a szoftverek kiszolgáló számítógépekre történő telepítésének alapvető ismerete. Azt is feltételezi, hogy a Active Directory® tartományi szolgáltatások, a Forefront vagy a Microsoft Identity Manager (FIM) és a Microsoft SQL Server 2012 Database szoftver alapvető ismeretekkel rendelkezik. A jelen dokumentáció hatókörén kívül a függő technológiák (például a AD DS és a FIM) beállításának és konfigurálásának leírása. További információ a Microsoft BHOLD-modulok által végrehajtott függvényekről: [a Microsoft BHOLD Suite fogalmi útmutatója](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Célközönség

Ez a dokumentum az informatikai tervezők, rendszerfejlesztők, technológiai döntéshozók, tanácsadók, infrastruktúra-tervezők és informatikai szakemberek számára készült, akik a Microsoft BHOLD Suite üzembe helyezését tervezik.

## <a name="bhold-infrastructure-considerations"></a>A BHOLD-infrastruktúra szempontjai

Leggyakrabban a BHOLD és a FIM-t használják nagy infrastrukturális környezetben. Az adott üzleti igényeknek megfelelően testre szabhatja a BHOLD és a FIM-architektúrát. A következő szakaszokban néhány lehetséges építészeti megoldás található. Ez az Áttekintés nem tartalmazza az összes lehetséges beállítást, de azt is sugallja, hogyan telepíthet BHOLD a hálózatban.
 
Ez a szakasz a következő témaköröket tartalmazza:

- Egykiszolgálós architektúra
- Kettős kiszolgáló architektúrája
- Kétrétegű architektúra
- Az SQL Serverre vonatkozó ajánlások

### <a name="single-server-architecture"></a>Egykiszolgálós architektúra

A kis szervezeteknél vagy fejlesztési célokra történő üzembe helyezéshez a BHOLD és a FIM-t telepítheti ugyanarra a kiszolgálóra, mint SQL Server és AD DS, ahogy az a következő ábrán látható.
 
![Egyetlen kiszolgáló architektúrája](media/bhold-installation-guide/single.png)

Ha a BHOLD Suite SP1 és a FIM Portal egyetlen kiszolgálón van telepítve, akkor a BHOLD és a FIM esetében eltérő gazdagép-aliasokat (CNAME vagy rekordokat) kell létrehoznia a DNS-ben. Ez lehetővé teszi, hogy a BHOLD és a FIM-szolgáltatásokhoz külön egyszerű szolgáltatásnév (SPN) legyen létrehozva. További információ: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
A FIM egykiszolgálós konfigurációban való telepítésével kapcsolatos útmutatásért lásd: [első lépések útmutatók általános konfigurációja](https://technet.microsoft.com/library/ff575965.aspx) a Microsoft TechNet könyvtárban.

### <a name="dual-server-architecture"></a>Kettős kiszolgáló architektúrája

A BHOLD Core és a FIM külön kiszolgálókon való telepítése nagyobb teljesítményt és rugalmasságot biztosít a közepes méretű szervezetek számára, amelyek nem igényelnek összetettebb üzembe helyezést, például a többrétegű architektúrák által biztosított szolgáltatásokat. Az alábbi ábrán a saját kiszolgálókon telepített BHOLD és FIM látható. a FIM-kiszolgáló a SQL Server is futtatja, hogy az adatbázis-szolgáltatásokat BHOLD és FIM-ként szolgáltassa. A FIM-kiszolgálón futó FIM synchronization Service szinkronizálja a FIM és a BHOLD adatbázisok változásait. Vegye figyelembe, hogy ha a végfelhasználói önkiszolgáló szolgáltatásra van szükség, a BHOLD FIM-integrációs modult ugyanarra a kiszolgálóra kell telepíteni, mint a FIM szolgáltatás és a FIM Portal. A BHOLD FIM integrációs modulja megköveteli, hogy a FIM szolgáltatás és a BHOLD FIM integrációs modul ugyanarra a kiszolgálóra legyen telepítve.

![Kettős kiszolgáló architektúrája](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> A BHOLD FIM integrációs modul jelentéskészítési funkciója megköveteli, hogy a BHOLD és FIM-adatbázisok ugyanarra a SQL Server példányra legyenek telepítve, és a BHOLD-szolgáltatásfiók hozzáférési jogokkal kell rendelkeznie a FIM szolgáltatás adatbázisához.

### <a name="two-tier-architecture"></a>Kétrétegű architektúra

A legtöbb környezetben, különösen azoknál, ahol a teljesítmény fontos, a BHOLD Suite SP1, a FIM és a SQL Servert külön kiszolgálókon kell futtatnia (kétrétegű architektúra). A kétrétegű architektúrában a memória-és a CPU-erőforrások minden réteghez külön vannak kijelölve. Az alábbi ábrán a kétrétegű architektúra konfigurálásának egyik lehetséges módja látható. A FIM-kiszolgálón futó FIM synchronization Service szinkronizálja a FIM és a BHOLD adatbázisok változásait. Vegye figyelembe, hogy ha a végfelhasználói önkiszolgáló szolgáltatásra van szükség, a BHOLD FIM-integrációs modult ugyanarra a kiszolgálóra kell telepíteni, amelyen a FIM szolgáltatás és a portál található.

![kétrétegű architektúra](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Az SQL Serverre vonatkozó ajánlások

Ha nagyméretű szervezetekben helyez üzembe BHOLD, erősen ajánlott a Microsoft SQL Server-adatbázis beállításához kövesse az alábbi irányelveket:

- A SQL Server üzembe helyezése egy olyan kiszolgálón, amely a FIM-vagy BHOLD-szolgáltatásoktól eltér.
- A naplófájl elkülönítése az adatfájlból a fizikai lemez szintjén.
- Ha RAID-t használ a tárolási redundancia biztosításához, használja a 10 (1 + 0) RAID-szintet. Ne használja az 5. RAID-szintet.
- Győződjön meg arról, hogy a megfelelő beállításokat konfigurálja, ha a SQL Servert futtató kiszolgálóhoz több mint 2 GB-nyi fizikai memóriát használ.

Az SQL Server ajánlott eljárásaival kapcsolatos további információkért lásd: a Microsoft TechNet Library [10 legjobb](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) megoldás a Storage-ban.

### <a name="trusted-certificates-list-update"></a>Megbízható tanúsítványok listájának frissítése

A Windows konfigurálható úgy, hogy a szolgáltatás elindítása előtt érvényesítse a tanúsítványláncot. Ilyen rendszerek esetén a szolgáltatás nem indítható el, ha a szolgáltatás végrehajtható kódját olyan tanúsítvánnyal írták alá, amely nem szerepel a kiszolgáló megbízható tanúsítványok listájában (TCL). A Microsoft BHOLD Suite SP1 szoftver kódja a Microsoft Root Certificate Authority 2010 tanúsítvánnyal rendelkező kód-aláíró tanúsítványlánc használatával van aláírva.
A Windows konfigurálható úgy, hogy a Microsoft legfelső szintű tanúsítványait internetes kapcsolaton keresztül kérje le. A leválasztott rendszereken azonban a Windows Server csak azokat a tanúsítványokat tartalmazza, amelyek a rendszerindító programban voltak elérhetők a Windows megjelenése előtt. A Windows Server 2010 előtti kiadásaiban ezek a tanúsítványok nem tartalmazzák a BHOLD Suite SP1-kód aláírási láncának ellenőrzéséhez szükséges főtanúsítványt. Ha egy vagy több Microsoft BHOLD Suite SP1-modult szeretne telepíteni egy olyan rendszeren, amely nem rendelkezik naprakész TCL, le kell töltenie és telepítenie kell a legfelső szintű frissítési csomagot, vagy az Csoportházirend használatával telepítenie kell a legfelső szintű frissítési csomagot, mielőtt telepítené a BHOLD Suite SP1-modult. További információ: a [Windows Root Certificate program tagjai](https://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>A BHOLD Suite SP1 telepítése a Windows Server 2012/2016 szükséges lépésével 

![IIS telepítése BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Ha a BHOLD Suite SP1 szervizcsomagot a Windows Server 2012-es vagy 2016-es verziójára telepíti, a BHOLD-weblapok addig nem lesznek elérhetők, amíg nem módosítja a applicationHost.config fájlt ```C:\Windows\System32\inetsrv\config``` . A (z) ```<globalModules>``` szakaszban adja hozzá ```preCondition="bitness64``` a szöveget a következő megkezdéséhez ```<add name="SPNativeRequestModule"``` :

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

A fájl szerkesztése és mentése után futtassa az IISRESET parancsot az IIS-kiszolgáló alaphelyzetbe állításához.


## <a name="upgrading-bhold-suite"></a>BHOLD Suite frissítése

Meglévő BHOLD Suite-telepítés nem frissíthető. Ehelyett el kell távolítania egy meglévő BHOLD Suite-telepítést, mielőtt frissíteni tudja a BHOLD modulokat. Ha rendelkezik meglévő BHOLD-modellel, a BHOLD-adatbázist frissítheti, és a frissített BHOLD Core modul telepítésekor használhatja azt. További információ: [a BHOLD Suite cseréje a BHOLD Suite SP1 verzióra](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>További lépések

- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)
