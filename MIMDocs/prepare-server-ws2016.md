---
title: A Windows Server 2016 vagy 2019 konfigurálása 2016 a felügyeleti CSOMAGhoz Microsoft Docs
description: A Windows Server 2016-es vagy 2019 2016-es VERZIÓjának előkészítéséhez szükséges lépések és minimális követelmények
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cf8261c4e6f6529fd82760206b62b689a75d0acb
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492412"
---
# <a name="set-up-an-identity-management-server-windows-server-2016-or-2019"></a>Identitáskezelés-felügyeleti kiszolgáló beállítása: Windows Server 2016 vagy 2019

> [!div class="step-by-step"]
> [«Tartomány előkészítése](preparing-domain.md) 
>  [SQL Server»](prepare-server-sql2016.md)
> 

> [!NOTE]
> A Windows Server 2019 telepítési eljárása nem különbözik a Windows Server 2016 telepítési eljárásának.


> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Például:
> - Tartományvezérlő neve – **corpdc**
> - Tartománynév – **contoso**
> - **Corpservice** -kiszolgáló neve
> - **Corpsync** -szinkronizálási kiszolgáló neve
> - SQL Server neve – **corpsql**
> - Jelszó <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>A Windows Server 2016 csatlakoztatása a tartományhoz

Indítsa el a Windows Server 2016 rendszerű gépet, amely legalább 8 12GB RAM-mal rendelkezik. A telepítésekor a "Windows Server 2016 standard/Datacenter (kiszolgáló grafikus felhasználói felülettel) x64" kiadást kell megadni.

1. Rendszergazdaként jelentkezzen be az új számítógépre.

2. A Vezérlőpulton osszon ki a számítógépnek egy statikus IP-címet a hálózaton. Konfigurálja úgy a hálózati adaptert, hogy DNS-lekérdezéseket küldjön az előző lépésben megadott tartományvezérlő IP-címére, és állítsa a számítógép nevét a **CORPSERVICE** értékre.  A művelethez szükség lesz a kiszolgáló újraindítására.

3. Nyissa meg a Vezérlőpultot, és csatlakoztassa a számítógépet az utolsó lépésben konfigurált tartományhoz ( *contoso.com* ).  Ez a művelet magában foglalja a tartományi rendszergazda felhasználónevet és hitelesítő adatait (például *Contoso\Rendszergazda* ).  Miután az üdvözlő üzenet megjelenik, zárja be a párbeszédpanelt, és még egyszer indítsa újra a kiszolgálót.

4. Jelentkezzen be a számítógépre *CORPSERVICE* helyi számítógép-rendszergazdaként, például *Contoso\MIMINSTALL*.


5. Rendszergazdaként nyisson meg egy PowerShell-ablakot, és írja be a következő parancsot a számítógép frissítéséhez a csoportházirend-beállításokkal.

    ```
    gpupdate /force /target:computer
    ```

    A folyamat egy percen belül befejeződik, és a következő üzenet jelenik meg: „A számítógép-házirend frissítése sikeresen befejeződött.”

6. Vegye fel a **Webkiszolgáló (IIS)** és az **Alkalmazáskiszolgáló** szerepköröket, a **.NET-keretrendszer** 3.5, 4.0 és 4.5 funkcióit és az **Active Directory-modult a Windows PowerShell környezethez**.

    ![Kép: PowerShell-funkciók](media/MIM-DeployWS2.png)

7. A PowerShell-ablakba írja be következő parancsokat: Elképzelhető, hogy a **.NET-keretrendszer** 3.5 funkcióhoz tartozó forrásfájlokhoz más helyet kell megadni. Ezek a funkciók általában nem érhetők el a Windows Server telepítésekor, csak a következő mappában az operációs rendszer telepítésére szolgáló lemezen, pl.: „*d:\Sources\SxS\*”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>A kiszolgálói biztonsági házirend konfigurálása

A kiszolgálói biztonsági házirendben engedélyezze az újonnan létrehozott fiókok szolgáltatásként történő futtatását.
> [!NOTE] 
> A konfigurációtól függően egyetlen kiszolgálóról (az összesről) vagy az elosztott kiszolgálókról csak a tag számítógépe, például a szinkronizációs kiszolgáló szerepköre alapján kell felvennie. 

1. A Helyi biztonsági házirend program elindítása

2. Keresse meg a **Helyi házirend > Felhasználói jogok kiosztása** csomópontot.

3. A részleteket tartalmazó ablaktáblán kattintson a jobb gombbal a **Bejelentkezés szolgáltatásként** lehetőségre, majd válassza a **Tulajdonságok** lehetőséget.

    ![Kép: Helyi biztonsági házirend](media/MIM-DeployWS3.png)

4. Kattintson a **felhasználó vagy csoport hozzáadása** elemre, majd a szövegmezőbe írja be a következőt a szerepkör alapján: kattintson a Névellenőrzés elemre `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR` , majd az **OK** gombra. **Check Names**

5. Kattintson az **OK** gombra a **Bejelentkezés szolgáltatásként – tulajdonságok** ablak bezárásához.

6.  A részleteket tartalmazó ablaktáblán kattintson a jobb gombbal a **számítógép hálózati elérésének megtagadása** elemre, majd válassza a **tulajdonságok** lehetőséget. >

7. Kattintson a **Felhasználó vagy csoport hozzáadása** gombra, a szövegmezőbe írja be a következőt: `contoso\MIMSync; contoso\MIMService`, majd kattintson az **OK** gombra.

8. Az **OK** gombbal zárja be **A számítógép hálózati elérésének megtagadása – tulajdonságok** ablakot.

9. A részleteket tartalmazó ablaktáblán kattintson a jobb gombbal a **helyi bejelentkezés megtagadása** elemre, majd válassza a **Tulajdonságok** lehetőséget.

10. Kattintson a **Felhasználó vagy csoport hozzáadása** gombra, a szövegmezőbe írja be a következőt: `contoso\MIMSync; contoso\MIMService`, majd kattintson az **OK** gombra.

11. Kattintson az **OK** gombra a **Helyi bejelentkezés megtagadása – tulajdonságok** ablak bezárásához.

12. Zárja be a Helyi biztonsági házirend ablakot.

## <a name="software-prerequisites"></a>Szoftver előfeltételei

A 2016 SP2 összetevőinek telepítése előtt győződjön meg arról, hogy az összes szoftverre vonatkozó előfeltételt telepíti:

13. Telepítse a [Visual C++ 2013 újraterjeszthető csomagjait](https://www.microsoft.com/download/details.aspx?id=40784).

14. Telepítse a .NET-keretrendszer 4,6-es telepítését.

15. Azon a kiszolgálón, amely a fakiszolgálói szinkronizációs szolgáltatást fogja üzemeltetni, [SQL Server Native Client](https://www.microsoft.com/download/details.aspx?id=50402)szükséges.

16. Azon a kiszolgálón, amely a faszerkezetű szolgáltatást fogja üzemeltetni, a rendszer a .NET-keretrendszer 3,5-es verzióját igényli

17. Ha TLS 1,2 vagy FIPS üzemmódot használ, tekintse meg a következő témakört 2016: a (z) ["tls 1,2 only" vagy a FIPS módú környezetekben a "a" rendszerhez](preparing-tls.md)tartozó

## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Az IIS Windows-hitelesítési módjának módosítása szükség esetén

1.  Indítson el egy PowerShell-ablakot.

2.  Az *iisreset /STOP* paranccsal állítsa le az IIS szolgáltatást.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [«Tartomány előkészítése](preparing-domain.md) 
>  [SQL Server»](prepare-server-sql2016.md)
