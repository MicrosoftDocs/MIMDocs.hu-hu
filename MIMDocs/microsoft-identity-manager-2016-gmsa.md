---
title: Microsoft Identity Manager-specifikus szolgáltatások átalakítása gMSA | Microsoft Docs
description: Ez a cikk a csoportosan felügyelt szolgáltatásfiók (gMSA) konfigurálásának előfeltételeit és alapvető lépéseit ismerteti.
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 03/10/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 50e5da9c7e3ed7df8edb8dbc315708df5ac5250a
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492542"
---
# <a name="convert-microsoft-identity-manager-specific-services-to-use-group-managed-service-accounts"></a>Microsoft Identity Manager-specifikus szolgáltatások átalakítása csoportosan felügyelt szolgáltatásfiókok használatára

Ez a cikk a támogatott Microsoft Identity Manager szolgáltatások csoportosan felügyelt szolgáltatásfiókok (gMSA) használatára való konfigurálásának útmutatója. Miután előre konfigurálta a környezetet, a gMSA-re való áttérés folyamata egyszerű.

## <a name="prerequisites"></a>Előfeltételek

- Töltse le és telepítse a következő szükséges gyorsjavítást: [Microsoft Identity Manager 4.5.26.0 vagy újabb](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history).

    Támogatott szolgáltatások:

    -   Microsoft Identity Manager szinkronizációs szolgáltatás (FIMSynchronizationService)
    -   Microsoft Identity Manager szolgáltatás (FIMService)
    -   Microsoft Identity Manager jelszó-regisztráció
    -   Új jelszó kérése Microsoft Identity Manager
    -   Privileged Access Management (PAM) figyelési szolgáltatás (PamMonitoringService)
    -   PAM-összetevő szolgáltatás (PrivilegeManagementComponentService)

    Nem támogatott szolgáltatások:

    -   A Microsoft Identity Manager portál nem támogatott. A szolgáltatás része a SharePoint-környezetnek, és telepítenie kell azt Farm módban, és [konfigurálnia kell az automatikus jelszavas változást a SharePoint-kiszolgálón](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
    -   Az összes felügyeleti ügynök a Microsoft Identity Manager Service Management agent kivételével
    -   Microsoft Tanúsítványkezelő
    -   BHOLD

- A környezet beállításával kapcsolatos háttér-és általános tudnivalókat lásd: 

    -   [Csoportosan felügyelt szolgáltatásfiókok – áttekintés](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)  
    -   [Új – ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)  

- Mielőtt elkezdené, [hozza létre a központi terjesztési szolgáltatások legfelső szintű kulcsát](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx) a Windows-tartományvezérlőn. Vegye figyelembe a következő információkat:  

    - A Key Distribution Services-(KDS-) szolgáltatás a legfelső szintű kulcsokat használja a tartományvezérlőkön található jelszavak és egyéb információk létrehozásához.
    - Csak egyszer hozzon létre egy legfelső szintű kulcsot tartományon, ha szükséges.  
    - Belefoglalás `Add-KDSRootKey –EffectiveImmediately` . "– EffectiveImmediately" azt jelenti, hogy akár 10 órát is igénybe vehet, hogy a legfelső szintű kulcsot replikálja az összes tartományvezérlőre. A két tartományvezérlőre való replikálás körülbelül 1 órát is igénybe vehet. 
    ![A "– EffectiveImmediately" karakterlánc](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="actions-to-run-on-the-active-directory-domain-controller"></a>A Active Directory-tartomány vezérlőn futtatandó műveletek

1.  Hozzon létre egy *MIMSync_Servers* nevű csoportot, és adja hozzá az összes szinkronizációs kiszolgálót.

    ![MIMSync_Servers csoport létrehozása](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

1.  Jelentkezzen be a Windows PowerShellbe *tartományi rendszergazdaként* egy olyan fiókkal, amely már csatlakoztatva van a tartományhoz, majd futtassa a következő parancsot: 

    `New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"`

    ![A parancs a PowerShellben](media/f6deb0664553e01bcc6b746314a11388.png)

    - A szinkronizálási gMSA részleteinek megtekintése:  
     ![A szinkronizálás gMSA részletei](media/c80b0a7ed11588b3fb93e6977b384be4.png)

    - Ha a jelszó-módosítási értesítési szolgáltatást (PCN) futtatja, frissítse a delegálást a következő parancs futtatásával:

        `Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames
        @{Add="PCNSCLNT/mimsync.contoso.com"}`

## <a name="actions-to-run-on-the-microsoft-identity-manager-synchronization-server"></a>A Microsoft Identity Manager szinkronizálási kiszolgálón futtatandó műveletek

1. Synchronization Service Manager a titkosítási kulcs biztonsági mentését. A rendszer a módosítási mód telepítését fogja kérni. Tegye a következőket:

    a. Keresse meg a szinkronizálási szolgáltatás kulcskezelő eszközét azon a kiszolgálón, amelyen a Synchronization Service Manager telepítve van. Az **exportálási kulcs készlete** alapértelmezés szerint már ki van választva.

    b. Válassza a **Tovább** gombot. 
    
    c. A parancssorba írja be és ellenőrizze a Microsoft Identity Manager vagy a Forefront Identity Manager (FIM) szinkronizálási szolgáltatás fiókjának adatait:

    -   **Fiók neve** : a kezdeti telepítés során használt szinkronizációs szolgáltatás fiókjának neve.  
    -   **Password (jelszó** ): a szinkronizálási szolgáltatás fiókjának jelszava.  
    -   **Tartomány** : a szinkronizálási szolgáltatásfiók részét képező tartomány.

    d. Válassza a **Tovább** gombot.

    Ha sikeresen megadta a fiók adatait, lehetősége van módosítani a biztonsági másolat titkosítási kulcsának célhelyét vagy a fájl exportálási helyét. Alapértelmezés szerint az exportfájl helye a *C:\Windows\system32\miiskeys-1.bin*.

1. Telepítse a Microsoft Identity Manager 2016 SP1 vagy újabb gyorsjavítást, amelyet a mennyiségi licencelési szolgáltatási központban vagy az MSDN letöltési webhelyén talál. A telepítés befejezése után mentse a kulcskészlet *miiskeys. bin* fájlját.

   ![A Microsoft Identity Manager szinkronizációs szolgáltatás telepítési folyamatának ablaka](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)

1. Telepítse a [gyorsjavítás 4.5.2.6.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) vagy újabb verzióját.

1. A javítás telepítése után állítsa le a FIM szinkronizációs szolgáltatást a következő módon:

   a. A vezérlőpulton válassza a **programok és szolgáltatások**  >  **Microsoft Identity Manager** elemet.  
   b. A **szinkronizálási szolgáltatás** lapon válassza a **módosítás**  >  **tovább** lehetőséget.  
   c. A **karbantartási beállítások** ablakban válassza a **Konfigurálás** lehetőséget.

   ![A karbantartási beállítások ablak](media/dc98c011bec13a33b229a0e792b78404.png)

   d. A **Microsoft Identity Manager szinkronizálási szolgáltatás konfigurálása** ablakban törölje az alapértelmezett értéket a **szolgáltatásfiók** mezőben, majd írja be a **MIMSyncGMSA $** értéket. Ügyeljen arra, hogy tartalmazza a Dollar Sign ($) szimbólumot, ahogy az az alábbi képen is látható. Hagyja üresen a **jelszó** mezőt.

   ![A Microsoft Identity Manager szinkronizálási szolgáltatás konfigurálása ablak](media/38df9369bf13e1c3066a49ed20e09041.png)

   e. Válassza a **következő**  >  **Next**  >  **telepítés** lehetőséget.  
   f. Állítsa vissza a kulcskészlet elemet a korábban mentett *miiskeys. bin* fájlból.

   ![A kulcskészlet-konfiguráció visszaállításának lehetősége](media/44cd474323584feb6d8b48b80cfceb9b.png)

   ![A felügyeleti ügynökök listája Synchronization Service Manager](media/03e7762f34750365e963f0b90e43717c.png)

## <a name="microsoft-identity-manager-service"></a>Microsoft Identity Manager szolgáltatás

>[!IMPORTANT]
>Ha Microsoft Identity Manager szolgáltatással kapcsolatos fiókokat gMSA-fiókokra konvertál, kövesse az ebben a szakaszban leírt utasításokat.

1. Hozzon létre csoportosan felügyelt fiókokat Microsoft Identity Manager szolgáltatáshoz, a PAM REST API-hoz, PAM-figyelési szolgáltatáshoz, PAM összetevő-szolgáltatáshoz, az önkiszolgáló jelszó-visszaállítási (SSPR) regisztrációs portálhoz és a SSPR-visszaállítási portálhoz

    -   A gMSA-delegálás és az egyszerű szolgáltatásnév (SPN) frissítése:

        - `Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames
            @{Add="\<SPN\>"}`
    -   Delegálás

        - `Set-ADServiceAccount -Identity \<gmsaaccount\>
                -TrustedForDelegation $true`
    -   Korlátozott delegálás:
        -   `$delspns = 'http/mim', 'http/mim.contoso.com'`
        -   `New-ADServiceAccount -Name \<gmsaaccount\> -DNSHostName
                \<gmsaaccount\>.contoso.com
                -PrincipalsAllowedToRetrieveManagedPassword \<group\>
                -ServicePrincipalNames $spns -OtherAttributes
                @{'msDS-AllowedToDelegateTo'=$delspns }`

1. Vegyen fel egy fiókot a Microsoft Identity Manager szolgáltatáshoz a szinkronizálási csoportokban. Ez a lépés szükséges a SSPR.

    ![A felhasználók és számítógépek Active Directory ablak](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

    > [!NOTE]  
    > A Windows Server 2012 R2 egyik ismert hibája, hogy a felügyelt fiókokat használó szolgáltatások nem válaszolnak a kiszolgáló újraindítása után, mert a Microsoft Key Distribution Service nincs elindítva a Windows újraindítása után. A probléma megoldásához futtassa a következő parancsot: 
    >
    > `sc triggerinfo kdssvc start/networkon`
    >
    > A parancs elindítja a Microsoft Key Distribution Service szolgáltatást, amikor a hálózat be van kapcsolva (jellemzően a rendszerindítási ciklus korai szakaszában).
    >
    > Hasonló probléma esetén tekintse meg a következőt [: AD FS Windows 2012 R2: a adfssrv lefagy a kiinduló módban](https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS).

1.  Futtassa a Microsoft Identity Manager-szolgáltatás emelt szintű MSI-fájlját, és kattintson a **módosítás** gombra.

1.  A **levelezési kiszolgáló kapcsolatainak konfigurálása** ablakban jelölje be a **másik felhasználó használata Exchange-hez (felügyelt fiókok esetén)** jelölőnégyzetet. Lehetősége van az aktuális Exchange-fiók vagy a Felhőbeli postaláda használatára.

    >[!NOTE]
    >Ha bejelöli **az Exchange Online használata** lehetőséget, hogy Microsoft Identity Manager szolgáltatás feldolgozza a Microsoft Identity Manager Outlook-bővítmény jóváhagyási válaszait, a telepítés után állítsa a *PollExchangeEnabled* **HKLM\SYSTEM\CurrentControlSet\Services\FIMService** értéket **1-re** .
    
    ![A "levelezési kiszolgáló kapcsolatainak konfigurálása" ablak](media/0cd8ce521ed7945c43bef6100f8eb222.png)

1.  A címtárszolgáltatás-szolgáltatásfiók **konfigurálása** ablak **szolgáltatásfiók neve** mezőjébe írja be a nevet. Ügyeljen arra, hogy tartalmazza a Dollar Sign ($) szimbólumot. Adja meg a jelszót is a **szolgáltatás E-mail fiókjának jelszava** mezőben. A **szolgáltatásfiók jelszava** mezője nem érhető el.

    ![A "webszolgáltatási szolgáltatásfiók konfigurálása" ablak](media/db0d543df6e1b0174a47135617c23fcb.png)

    Mivel a LogonUser függvény nem működik a felügyelt fiókoknál, a következő oldalon a figyelmeztetés jelenik meg: "Ellenőrizze, hogy a szolgáltatásfiók biztonságos-e a jelenlegi konfigurációjában."

    ![Fiók biztonsági figyelmeztetési ablaka](media/d350bc13751b2d0a884620db072ed019.png)

1.  A **Privileged Access Management REST API konfigurálása** ablakban az **alkalmazáskészlet fiókjának neve** mezőbe írja be a fiók nevét. Ügyeljen arra, hogy tartalmazza a Dollar Sign ($) szimbólumot. Hagyja üresen az **alkalmazáskészlet fiókjának jelszava** mezőt.

    ![A Privileged Access Management REST API konfigurálása ablak](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

1.  A **PAM összetevő-szolgáltatás konfigurálása** ablak **szolgáltatásfiók neve** mezőjébe írja be a fiók nevét. Ügyeljen arra, hogy tartalmazza a Dollar Sign ($) szimbólumot. Hagyja üresen a **szolgáltatásfiók jelszava** mezőt.

    ![A PAM összetevő-szolgáltatás konfigurálása ablak](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

    ![A fiók biztonsági figyelmeztetési ablaka](media/9d2b52f6faed10601e7e2166a339fb47.png)

1.  A **Privileged Access Management figyelési szolgáltatás konfigurálása** ablak **szolgáltatásfiók neve** mezőjébe írja be a szolgáltatásfiók nevét. Ügyeljen arra, hogy tartalmazza a Dollar Sign ($) szimbólumot. Hagyja üresen a **szolgáltatásfiók jelszava** mezőt.

    ![A Privileged Access Management figyelési szolgáltatásának konfigurálása ablak](media/d1e824248edf12a77fc9ffb011475164.png)

1.  A fiók neve mezőbe írja be a fiók nevét a **rendszerfiók** **jelszavas regisztrációs portáljának konfigurálása** ablakban. Ügyeljen arra, hogy tartalmazza a Dollar Sign ($) szimbólumot. Hagyja üresen a **jelszó** mezőt.

    ![A rendszerjelszó-regisztrációs portál konfigurálása ablak](media/601e935cdfda298b61ae753a2a152996.png)

1.  A webalkalmazási **jelszó-visszaállítási portál konfigurálása** ablak **fiók neve** mezőjébe írja be a fiók nevét. Ügyeljen arra, hogy tartalmazza a Dollar Sign ($) szimbólumot. Hagyja üresen a **jelszó** mezőt.

    ![A webhelyre vonatkozó jelszó-visszaállítási portál konfigurálása ablak](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

1.  Fejezze be a telepítést.

    > [!NOTE]
    > A telepítés során két új kulcs jön létre a beállításjegyzékbeli elérési úton **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Forefront Identity Manager\2010\Service** a titkosított Exchange-jelszó tárolásához. Az egyik bejegyzés a *ExchangeOnline* , a másik pedig a *ExchangeOnPremise*. Az egyik bejegyzésnél az **adatoszlop** értékének üresnek kell lennie.

    > ![A Rendszerleíróadatbázis-szerkesztő](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

Ha módosítani szeretné a jelszót a tárolt fiókokon a módosítási mód futtatása nélkül, [töltse le ezt a PowerShell-parancsfájlt](microsoft-identity-manager-2016-gmsascript.md).

Az Exchange-jelszó titkosításához a telepítő egy további szolgáltatást hoz létre, és a felügyelt fiókban futtatja azt. A telepítés során az **alkalmazás** eseménynaplójában a következő üzenetek lesznek hozzáadva:

![A Eseménynapló ablak](media/95b315454705cd4d939b55ac5ad910f5.jpg)
