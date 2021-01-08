---
title: A PAM üzembe helyezése, 1. lépés – CORP tartomány | Microsoft Docs
description: A CORP tartomány előkészítése meglévő vagy új identitásokkal a Microsoft Identity Manager által felügyelt
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4c9c5736d0215d0423eb989dc5a194a0a00c3c50
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010427"
---
# <a name="step-1---prepare-the-host-and-the-corp-domain"></a>1. lépés – A gazdagép és a CORP tartomány előkészítése

> [!div class="step-by-step"]
> [2. lépés»](step-2-prepare-priv-domain-controller.md)

Ebben a lépésben előkészíti a gazdagépet a megerősített környezet számára. Ha szükséges, létrehoz egy tartományvezérlőt és egy tag munkaállomást egy új tartományban és erdőben (a *CORP* erdőben) olyan identitásokkal, amelyeket a megerősített környezet kezel. A CORP erdő egy kezelendő erőforrásokat tartalmazó, meglévő erdőt szimulál. Ez a dokumentum egy védendő erőforrást, egy fájlmegosztást használ példaként.

Ha már van olyan meglévő Active Directory- (AD-) tartománya, melynek tartományvezérlőjén a Windows Server 2012 R2 vagy későbbi rendszert fut, illetve ahol Ön a tartományi rendszergazda, azt a tartományt is használhatja.  

## <a name="prepare-the-corp-domain-controller"></a>A CORP-tartományvezérlő előkészítése

Ez a szakasz ismerteti, hogyan állíthat be tartományvezérlőt egy CORP tartományhoz. A CORP tartományon belül a rendszergazda felhasználókat a megerősített környezet felügyeli. Ebben a példában *contoso.local* lesz a CORP tartomány DNS-neve.

### <a name="install-windows-server"></a>A Windows Server telepítése

Telepítse a Windows Server 2012 R2-es vagy újabb verzióját egy virtuális gépre egy *CORPDC* nevű számítógép létrehozásához.

1. Válassza **a Windows server 2012 R2 standard (kiszolgáló grafikus felhasználói felülettel) x64** vagy a **Windows Server 2016 (kiszolgáló asztali** felülettel) lehetőséget.

2. Olvassa el és fogadja el a licencfeltételeket.

3. Mivel a lemez üres, válassza az **Egyéni: csak a Windows telepítése** lehetőséget, és használja a nem inicializált lemezterületet.

4. Rendszergazdaként jelentkezzen be az új számítógépre. Nyissa meg a Vezérlőpultot. Adja a számítógépnek a *CORPDC* nevet, és a virtuális hálózaton rendeljen hozzá egy statikus IP-címet. Indítsa újra a kiszolgálót.

5. A kiszolgáló újraindítása után jelentkezzen be rendszergazdaként. Nyissa meg a Vezérlőpultot. Állítsa be a számítógépet a frissítések keresésére, és telepítse a szükséges frissítéseket. Indítsa újra a kiszolgálót.

### <a name="add-roles-to-establish-a-domain-controller"></a>Szerepkörök beállítása tartományvezérlő létrehozásához

Ebben a szakaszban be fogja állítani az Active Directory tartományi szolgáltatások (AD DS), a DNS-kiszolgáló, illetve a (Fájl- és adattárolási szolgáltatások szerepkör részét képező) Fájlkiszolgáló szerepkört, majd ezt a kiszolgálót egy új, contoso.local nevű erdő tartományvezérlőjévé lépteti elő.

> [!NOTE]  
> Ha már van olyan tartománya, amelyet CORP tartományként szeretne használni, és ez a tartomány a Windows Server 2012 R2 vagy későbbi verziót használja tartományvezérlőként, ugorjon a [További felhasználók és csoportok létrehozása bemutatóhoz](#create-additional-users-and-groups-for-demonstration-purposes) című részhez.

1. Rendszergazdaként bejelentkezve indítsa el a PowerShellt.

2. Írja be a következő parancsokat:

   ```PowoerShell
   import-module ServerManager

   Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

   Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
   ```

   Ekkor megjelenik egy figyelmeztetés a csökkentett mód rendszergazdai jelszavának használatára vonatkozóan. A DNS-delegálással és a titkosítási beállításokkal kapcsolatosan figyelmeztető üzenetek fognak megjelenni. Ez nem rendellenes.

3. Az erdő létrehozásának befejezése után jelentkezzen ki. A kiszolgáló automatikusan újraindul.

4. A kiszolgáló újraindítása után jelentkezzen be a CORPDC számítógépre tartományi rendszergazdaként. Ez általában a CONTOSO\\Rendszergazda nevű felhasználó, aki ismeri a Windowsnak a CORPDC számítógépre történő telepítésekor megadott jelszót.

### <a name="create-a-group"></a>Csoport létrehozása

Hozzon létre egy csoportot az Active Directory naplózási céljaira, feltéve, hogy még nincs ilyen csoport. A csoport nevének a NetBIOS-tartománynévnek kell lennie, melyet három dollárjel követ, például: *CONTOSO$$$*.

Minden tartományban jelentkezzen be egy tartományvezérlőre tartományi rendszergazdaként, és hajtsa végre az alábbi lépéseket:

1. Indítsa el a PowerShellt.

2. Írja be a következő parancsokat, de a „CONTOSO” szót cserélje ki a saját tartományának NetBIOS-nevével.

   ```PowerShell
   import-module activedirectory

   New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
   ```

Bizonyos esetekben előfordulhat, hogy a csoport már létezik – ami normális, ha a tartományt az AD áttelepítési forgatókönyveihez is használják.

### <a name="create-additional-users-and-groups-for-demonstration-purposes"></a>További felhasználók és csoportok létrehozása bemutatóhoz

Ha létrehozott egy új CORP tartományt, további felhasználókat és csoportokat kell létrehoznia a PAM-forgatókönyv bemutatásához. A bemutatási célokra használt felhasználók és csoportok nem lehetnek tartományi rendszergazdák, és nem szabályozhatják őket az AD adminSDHolder beállításai.

> [!NOTE]
> Ha már van olyan tartománya, amelyet CORP tartományként kíván használni, és abban található olyan felhasználó vagy csoport, amelyet bemutatási célokra használhat, ugorjon a [Naplózás konfigurálása](#configure-auditing) című részhez.

Létre fogjuk hozni a *CorpAdmins* nevű biztonsági csoportot és a *Ilona* nevű felhasználót. Tetszés szerint más neveket is választhat.

1. Indítsa el a PowerShellt.

2. Írja be a következő parancsokat: Helyettesítse a „Pass@word1” jelszót egy másik jelszósztringgel.

   ```PowerShell
   import-module activedirectory

   New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

   New-ADUser –SamAccountName Jen –name Jen

   Add-ADGroupMember –identity CorpAdmins –Members Jen

   $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

   Set-ADAccountPassword –identity Jen –NewPassword $jp

   Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
   ```

### <a name="configure-auditing"></a>Naplózás konfigurálása

A meglévő erdőkben be kell állítania a naplózást ahhoz, hogy létre lehessen hozni a PAM konfigurációját ezekre az erdőkre vonatkozóan.  

Minden tartományban jelentkezzen be egy tartományvezérlőre tartományi rendszergazdaként, és hajtsa végre az alábbi lépéseket:

1. Nyissa **meg a**  >  **felügyeleti eszközök** (vagy a Windows Server 2016, **Windows felügyeleti eszközök**) eszközt, és indítsa el **csoportházirend felügyeletet**.

2. Keresse meg az ehhez a tartományhoz tartozó tartományvezérlői szabályzatot.  Ha új tartományt hozott létre a contoso. local számára, navigáljon az **erdő: contoso. local**  >  **tartományok**  >  **contoso. local**  >  **tartományvezérlők**  >  **Alapértelmezett tartományvezérlői házirend** elemre. Ekkor megjelenik egy tájékoztató üzenet.

3. Kattintson a jobb gombbal az **Alapértelmezett tartományvezérlői házirend** elemre, majd válassza a **Szerkesztés** lehetőséget. Ekkor megjelenik egy új ablak.

4. A csoportházirend-felügyeleti szerkesztő ablak Alapértelmezett tartományvezérlői házirend fájában navigáljon a **számítógép-konfigurációs**  >  **házirendek**  >  **Windows-beállítások**  >  **biztonsági beállítások**  >  **helyi házirendek**  >  **naplózási házirend** elemre.

5. A részletek ablaktábláján kattintson a jobb gombbal a **Fiókkezelés naplózása** elemre, és válassza a **Tulajdonságok** parancsot. Válassza ki **A következő házirend-beállítások megadása** lehetőséget, jelölje be a **Sikeres** és a **Sikertelen** beállítást, majd kattintson az **Alkalmaz** és az **OK** gombra.

6. A részletek ablaktáblájában kattintson a jobb gombbal a **Címtárszolgáltatás-hozzáférés naplózása** elemre, és válassza a **Tulajdonságok** parancsot. Válassza ki **A következő házirend-beállítások megadása** lehetőséget, jelölje be a **Sikeres** és a **Sikertelen** beállítást, majd kattintson az **Alkalmaz** és az **OK** gombra.

7. Zárja be a Csoportházirendkezelés-szerkesztő ablakát és a Csoportházirend kezelése ablakot.

8. A naplózási beállítások alkalmazásához nyisson meg egy PowerShell-ablakot, és írja be a következőt:

   ```cmd
   gpupdate /force /target:computer
   ```

Néhány perc elteltével **A számítógép-házirend frissítése sikeresen befejeződött** üzenetnek kell megjelennie.

### <a name="configure-registry-settings"></a>Beállításjegyzék-beállítások konfigurálása

Ebben a szakaszban konfigurálni fogja az SID-előzmények áttelepítéséhez szükséges beállításjegyzék beállításait. Ezekre a Privileged Access Management-csoportok létrehozásához lesz szükség.

1. Indítsa el a PowerShellt.

2. Írja be a következő parancsokat, amelyekkel beállíthatja, hogy a forrástartomány engedélyezze a távoli eljáráshívás (RPC) hozzáférését a biztonsági fiókkezelő (SAM) adatbázisához.

   ```PowerShell
   New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

   Restart-Computer
   ```

Ez újraindítja a tartományvezérlőt, a CORPDC gépet. Ezzel a beállításjegyzék-beállítással kapcsolatban bővebben lásd: [How to troubleshoot inter-forest sIDHistory migration with ADMTv2 ](https://support.microsoft.com/kb/322970) (A SID-előzmények erdők között, ADMTv2-vel végzett áttelepítésekor jelentkező hibák elhárítása).

## <a name="prepare-a-corp-workstation-and-resource"></a>CORP-munkaállomás és -erőforrás előkészítése

Ha még nincs csatlakoztatva munkaállomás a tartományhoz, készítse elő a számítógépet az alábbi lépésekkel.  

> [!NOTE]
> Ha a tartományhoz már csatlakoztatott egy munkaállomást, ugorjon az [Erőforrás létrehozása bemutató céljára](#create-a-resource-for-demonstration-purposes) című részhez.

### <a name="install-windows-81-or-windows-10-enterprise-as-a-vm"></a>A Windows 8.1 vagy a Windows 10 Enterprise telepítése virtuális gépként

Egy új virtuális gépre, amelyre még nincs telepítve szoftver, telepítse a Windows 8.1 Enterprise vagy a Windows 10 Enterprise verziót. Ez lesz a *CORPWKSTN* számítógép.

1. A telepítéshez használja a gyorsbeállításokat.

2. Vegye figyelembe, hogy előfordulhat, hogy a telepítés nem fog tudni csatlakozni az internethez. Válassza a **Helyi fiók létrehozása** lehetőséget. Adjon meg más felhasználónevet, ne használja a „Rendszergazda” vagy az „Ilona” nevet.

3. A Vezérlőpulton adjon statikus IP-címet a számítógépnek a virtuális hálózatban, és a hálózati kapcsolat elsődleges DNS-kiszolgálójának állítsa be a CORPDC kiszolgáló DNS-kiszolgálóját.

4. A Vezérlőpulton csatlakoztassa a CORPWKSTN számítógépet a contoso.local tartományhoz. Meg kell adnia a Contoso tartomány rendszergazdai hitelesítő adatait. Amikor ezzel elkészült, indítsa újra a CORPWKSTN számítógépet.

### <a name="create-a-resource-for-demonstration-purposes"></a>Erőforrás létrehozása bemutató céljára

A PAM és a biztonságicsoport-alapú hozzáférés-vezérlés bemutatásához szüksége lesz egy erőforrásra.  Ha még nincs ilyen erőforrása, bemutató céljából használhat fájlmappát is.  Ez a contoso.local tartományban létrehozott „Ilona” és „CorpAdmins” nevű AD-objektumot fogja használni.

1. Csatlakozzon a CORPWKSTN munkaállomáshoz. Kattintson a **Felhasználóváltás** ikonra, majd a **Más felhasználó** lehetőségre. Győződjön meg róla, hogy a CONTOSO\\Ilona felhasználó be tud jelentkezni a CORPWKSTN munkaállomásra.

2. Hozzon létre egy új mappát a *CorpFS* névvel, és ossza meg a *CorpAdmins* csoporttal.

3. Nyissa meg a PowerShellt rendszergazdaként.

4. Írja be a következő parancsokat:

   ```PowerShell
   mkdir c:\corpfs

   New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

   $acl = Get-Acl c:\corpfs

   $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

   $acl.SetAccessRule($car)

   Set-Acl c:\corpfs $acl
   ```

A következő lépésben a PRIV tartományvezérlő előkészítésével foglalkozunk.

> [!div class="step-by-step"]
> [2. lépés»](step-2-prepare-priv-domain-controller.md)
