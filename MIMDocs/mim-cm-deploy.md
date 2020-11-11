---
title: A Microsoft Identity Manager tanúsítványkezelő üzembe helyezése | Microsoft Docs
description: A Microsoft Identity Manager 2016 tanúsítványkezelő telepítése
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 1885f01d1010565cef9ce50028ae1c86e8769003
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492532"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>A Microsoft Identity Manager Certificate Manager 2016 (MIM CM) üzembe helyezése

A Microsoft Identity Manager Certificate Manager 2016 (MIM CM) telepítése számos lépést foglal magában. A folyamat leegyszerűsítése érdekében leegyszerűsítjük a dolgokat. A tényleges MIM CM lépések előtt el kell végezni az előzetes lépéseket. Az előzetes munka nélkül a telepítés valószínűleg sikertelen lesz.

Az alábbi ábrán egy példa látható, hogy milyen típusú környezetet lehet használni. A számokat tartalmazó rendszerek szerepelnek a diagram alatti listán, és a cikkben ismertetett lépések sikeres végrehajtásához szükségesek. Végezetül a Windows 2016 Datacenter-kiszolgálókat használják:

![Környezeti diagram](media/mim-cm-deploy/image001.png)

1. CORPDC – tartományvezérlő
2. CORPCM – MIM CM-kiszolgáló
3. CORPCA – hitelesítésszolgáltató
4. CORPCMR – MIM CM REST API web – CM portál a REST API-hoz – később használatos
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – Windows 10 tartományhoz csatlakoztatva

## <a name="deployment-overview"></a>Az üzembe helyezés áttekintése

- Operációs rendszer alapszintű telepítése

    A labor Windows 2016 Datacenter-kiszolgálókat tartalmaz.

    >[!NOTE]
    >A webszolgáltatások által támogatott platformokkal kapcsolatos további részletekért tekintse meg a következő témakört: a 2016-es [támogatott 2016 platformok](microsoft-identity-manager-2016-supported-platforms.md)című cikk.

1. Üzembe helyezés előtti lépések

    - [A séma kiterjesztése](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Szolgáltatásfiókok létrehozása

    - [Tanúsítványsablonok létrehozása](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - A Kerberos konfigurálása

    - Adatbázissal kapcsolatos lépések

        - SQL-konfigurációs követelmények

        - Adatbázis-engedélyek

2. Üzembe helyezés

## <a name="pre-deployment-steps"></a>Üzembe helyezés előtti lépések

A MIM CM konfigurációs varázslónak meg kell adni a szükséges információkat, hogy a folyamat sikeresen befejeződjön.

![diagram](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>A séma kiterjesztése

A séma kibővítésének folyamata egyszerű, de körültekintően kell megközelíteni a visszafordíthatatlan jellege miatt.

>[!NOTE]
>Ehhez a lépéshez szükséges, hogy a használt fiókhoz legyen séma-rendszergazdai jogosultság.

1. Tallózással keresse meg a könyvtárbeli adathordozó helyét, és keresse meg a \\ tanúsítványkezelő \\ x64 mappát.

2. Másolja a CORPDC a séma mappájába, majd navigáljon hozzá.

    ![diagram](media/mim-cm-deploy/image005.png)

3. Futtassa a parancsfájlt resourceForestModifySchema.vbs egy erdős forgatókönyvet. Az erőforrás-erdő forgatókönyve esetében futtassa a parancsfájlokat:
   - Domaina – a felhasználók helye (userForestModifySchema.vbs)
   - ResourceForestB – a CM telepítésének helye (resourceForestModifySchema.vbs).

     >[!NOTE]
     >A séma módosítása egyirányú művelet, és egy erdő helyreállítását igényli a visszaállításhoz, ezért győződjön meg arról, hogy rendelkezik a szükséges biztonsági másolatokkal. A séma ezen művelet végrehajtásával végzett módosításaival kapcsolatos részletekért tekintse meg a [Forefront Identity Manager 2010 tanúsítványkezelő sémájának változásait](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx) ismertető cikket.

     ![diagram](media/mim-cm-deploy/image007.png)

4. Futtassa a szkriptet, és a szkript befejeződése után kapjon egy sikerességi üzenetet.

    ![Sikert jelző üzenet](media/mim-cm-deploy/image009.png)

Az Active Directory sémája mostantól támogatja a MIM CM támogatását.

### <a name="creating-service-accounts-and-groups"></a>Szolgáltatásfiókok és csoportok létrehozása

A következő táblázat összefoglalja a MIM CM által igényelt fiókokat és engedélyeket. Engedélyezheti, hogy a MIM CM a következő fiókokat automatikusan hozza létre, vagy a telepítés előtt létre tudja hozni őket. A tényleges fiókok nevei módosíthatók. Ha saját maga hozza létre a fiókokat, érdemes elneveznie a felhasználói fiókokat, hogy könnyen illeszkedjen a felhasználói fiók nevéhez a függvényéhez.

Felhasználók

![Ábra](media/mim-cm-deploy/image010.png)

![Ábra](media/mim-cm-deploy/image012.png)

| **Szerepkör**                   | **Felhasználói bejelentkezés neve** |
|----------------------------|---------------------|
| MIM CM ügynök               | MIMCMAgent          |
| MIM CM kulcs-helyreállítási ügynök  | MIMCMKRAgent        |
| MIM CM engedélyezési ügynök | MIMCMAuthAgent      |
| MIM CM CA Manager-ügynök    | MIMCMManagerAgent   |
| MIM CM web Pool Agent      | MIMCMWebAgent       |
| MIM CM regisztrációs ügynök    | MIMCMEnrollAgent    |
| MIM CM frissítési szolgáltatás      | MIMCMService        |
| Felhasználói fiók telepítése        | MIMINSTALL          |
| Ügyfélszolgálati ügynök            | CMHelpdesk1 – 2       |
| CM-kezelő                 | CMManager1 – 2        |
| Előfizető felhasználója            | CMUser1 – 2           |

Csoportok

| **Szerepkör**               | **Csoport**         |
|------------------------|-------------------|
| CM ügyfélszolgálati tagok    | MIMCM-Helpdesk    |
| CM-kezelő tagjai     | MIMCM-Managers    |
| CM előfizetők tagjai | MIMCM-Subscribers |

PowerShell: ügynök fiókjai:

```powershell
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>**CORPCM** -kiszolgáló helyi házirendjének frissítése az ügynök fiókjaihoz 

| **Felhasználói bejelentkezés neve** | **Leírás és engedélyek**   |
|------|---------------------|
| MIMCMAgent          | A a következő szolgáltatásokat biztosítja: </br>– Titkosított titkos kulcsok beolvasása a HITELESÍTÉSSZOLGÁLTATÓTÓL. </br>– Védi az intelligens kártyás PIN-adatokat a FIM CM-adatbázisban. </br>– Védi a kommunikációt a FIM CM és a HITELESÍTÉSSZOLGÁLTATÓ között. </br></br> Ehhez a felhasználói fiókhoz a következő hozzáférés-vezérlési beállítások szükségesek:</br>-   **Helyi bejelentkezés engedélyezése** a felhasználó számára.</br>-   **A tanúsítványok felhasználói jogosultságának kiállítása és kezelése** . </br>-Olvasási és írási engedély a rendszertemp mappában a következő helyen:% WINDIR% \\ temp.</br>– A felhasználói tárolóban kiadott és telepített digitális aláírási és titkosítási tanúsítvány.
|MIMCMKRAgent        | Helyreállítja az archivált titkos kulcsokat a HITELESÍTÉSSZOLGÁLTATÓTÓL. Ehhez a felhasználói fiókhoz a következő hozzáférés-vezérlési beállítások szükségesek:</br> -   **Helyi bejelentkezés engedélyezése** a felhasználó számára.</br>-Tagság a helyi **rendszergazdák** csoportban. </br>– Regisztráljon engedély a **KeyRecoveryAgent** -tanúsítványsablon számára. </br>– A kulcs-helyreállítási ügynök tanúsítványa ki van állítva, és telepítve van a felhasználói tárolóban. A tanúsítványt hozzá kell adni a HITELESÍTÉSSZOLGÁLTATÓ kulcs-helyreállítási ügynökének listájához. </br>-Olvasási engedély és írási engedély a rendszertemp mappában a következő helyen: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Meghatározza a felhasználók és csoportok felhasználói jogosultságait és engedélyeit. Ehhez a felhasználói fiókhoz a következő hozzáférés-vezérlési beállítások szükségesek: </br>-Tagság az előzetes Windows 2000-kompatibilis hozzáférés tartományi csoportban. </br> – Engedélyezte a **biztonsági naplózások** felhasználói jogosultságának létrehozását.             |
| MIMCMManagerAgent   | HITELESÍTÉSSZOLGÁLTATÓI felügyeleti tevékenységeket végez. </br> A felhasználónak hozzá kell rendelnie a HITELESÍTÉSSZOLGÁLTATÓ kezelése engedélyt.        |
| MIMCMWebAgent       | Megadja az IIS-alkalmazáskészlet identitását. Az FIM CM a Microsoft Win32® alkalmazásprogramozási felületének folyamatán belül fut, amely a felhasználó hitelesítő adatait használja. </br> Ehhez a felhasználói fiókhoz a következő hozzáférés-vezérlési beállítások szükségesek:</br> -Tagság a helyi **IIS_WPGban, windows 2016 = IIS_IUSRS** csoport. </br>-Tagság a helyi **rendszergazdák** csoportban.</br>– Engedélyezte a **biztonsági naplózások** felhasználói jogosultságának létrehozását. </br>– Az operációs rendszer felhasználói jogosultságának részeként biztosította a **működést** . </br>– A **folyamat szintű token** felhasználói jogosultságának lecserélése.</br>-Az IIS-alkalmazáskészlet identitásának **CLMAppPool** van hozzárendelve. </br>-Olvasási engedéllyel rendelkezik a    **HKEY_LOCAL_MACHINE \\ szoftver \\ Microsoft \\ CLM \\ v 1.0 \\ Server \\ webfelhasználó** beállításkulcs kulcsához. </br>– Ennek a fióknak a delegáláshoz is megbízhatónak kell lennie.|
| MIMCMEnrollAgent    | Beléptetést végez egy felhasználó nevében. Ehhez a felhasználói fiókhoz a következő hozzáférés-vezérlési beállítások szükségesek:</br>– A felhasználói tárolóban kiadott és telepített regisztrációs ügynök tanúsítványa.</br>-   **Helyi bejelentkezés engedélyezése** a felhasználó számára. </br>– Regisztráljon engedély a **beléptetési ügynök** tanúsítványsablon (vagy az egyéni sablon, ha van ilyen).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Tanúsítványsablonok létrehozása MIM CM-szolgáltatásfiókok számára

A MIM CM által használt szolgáltatásfiókok közül háromnak szüksége van egy tanúsítványra, és a konfigurációs varázsló megköveteli, hogy adja meg annak a tanúsítvány-sablonnak a nevét, amelyet a tanúsítványok igényléséhez használnia kell.

A tanúsítványokat igénylő szolgáltatásfiókok a következők:

- MIMCMAgent: ennek a fióknak felhasználói tanúsítványra van szüksége

- MIMCMEnrollAgent: a fióknak szüksége van egy regisztrációs ügynök tanúsítványára

- MIMCMKRAgent: ennek a fióknak szüksége van egy **kulcs-helyreállítási ügynök** tanúsítványára

Vannak olyan sablonok, amelyek már szerepelnek az AD-ben, de létre kell hozniuk a saját verzióit, hogy működjenek a MIM CM. Ahogy az eredeti alapsablonokkal kell módosítania.

A fenti fiókok mindhárom fiókja magasabb szintű jogokkal fog rendelkezni a szervezeten belül, és körültekintően kell kezelni őket.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Az MIM CM aláíró tanúsítványsablon létrehozása

1. A **felügyeleti eszközök** területen nyissa meg a **hitelesítésszolgáltatót**.

2. A **hitelesítésszolgáltató** konzol konzolfáján bontsa ki a **contoso-CorpCA** csomópontot, majd kattintson a **Tanúsítványsablonok** elemre.

3. Kattintson a jobb gombbal a **Tanúsítványsablonok** elemre, majd kattintson a **kezelés** parancsra.

4. A **Tanúsítványsablonok konzol** **részleteket** tartalmazó ablaktábláján válassza ki, majd kattintson a jobb gombbal a **felhasználó** elemre, majd kattintson a **sablon megkettőzése** elemre.

5. A **Sablon duplikálása** párbeszédpanelen válassza a **Windows Server 2003 Enterprise** lehetőséget, majd kattintson **az OK** gombra.

    ![Eredményül kapott módosítások megjelenítése](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >MIM CM a 3. verziójú tanúsítványsablonok alapján nem működik a tanúsítványokkal. Létre kell hoznia egy Windows Server® 2003 Enterprise (2. verzió) tanúsítványsablont. További információért lásd a [v3 részleteit](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) .

6. Az **új sablon tulajdonságai** párbeszédpanel **általános** lapjának **sablon megjelenítendő neve** mezőjébe írja be a következőt: **MIM cm aláírás**. Módosítsa az **érvényességi időszakot** **2 évre** , majd törölje a **tanúsítvány közzététele Active Directoryban** jelölőnégyzet jelölését.

7. A **kérelmek kezelésére** szolgáló lapon jelölje be a **titkos kulcs exportálásának engedélyezése** jelölőnégyzetet, majd kattintson a **titkosítás fülre**.

8. A **kriptográfiai kijelölés** párbeszédpanelen tiltsa le a Microsoft Enhanced **kriptográfiai szolgáltató 1.0-s verzióját** , engedélyezze a **Microsoft bővített RSA-és AES titkosítási szolgáltatót** , majd kattintson **az OK** gombra.

9. A **tulajdonos neve** lapon törölje a jelet az **E-mail név belefoglalása a tulajdonos neve** és az **e-mail neve** mezőbe jelölőnégyzetből.

10. Győződjön meg arról, hogy az **alkalmazás-házirendek** lehetőség van kiválasztva a **bővítmények** **lap bővítmények** lapján, majd kattintson a **Szerkesztés** elemre.

11. Az **alkalmazás-házirendek bővítmény szerkesztése** párbeszédpanelen válassza a **titkosított fájlrendszer** és a **biztonságos e-mail alkalmazás-** szabályzatok lehetőséget. Kattintson az **Eltávolítás** elemre, majd **az OK** gombra.

12. A **Biztonság** lapon hajtsa végre a következő lépéseket:

    - Távolítsa el a **rendszergazdát**.

    - **Tartományi rendszergazdák** eltávolítása.

    - **Tartományi felhasználók** eltávolítása.

    - Csak **olvasási** és **írási** engedélyeket rendeljen a **vállalati rendszergazdákhoz**.

    - **MIMCMAgent hozzáadása.**

    - **Olvasási** **és** beléptetési engedélyek kiosztása a **MIMCMAgent**.

13. Az **új sablon tulajdonságai** párbeszédpanelen kattintson **az OK** gombra.

14. Hagyja megnyitva a **Tanúsítványsablonok konzolt** .

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>A MIM CM regisztrációs ügynök tanúsítványsablon létrehozása

1. A **Tanúsítványsablonok konzol** **részletek** ablaktábláján válassza ki, majd kattintson a jobb gombbal a **beléptetési ügynök** elemre, majd kattintson a **sablon megkettőzése** elemre.

2. A **Sablon duplikálása** párbeszédpanelen válassza a **Windows Server 2003 Enterprise** lehetőséget, majd kattintson **az OK** gombra.

3. Az **új sablon tulajdonságai** párbeszédpanel **általános** lapjának **sablon megjelenítendő neve** mezőjébe írja be **MIM cm beléptetési ügynök** nevét. Győződjön meg arról, hogy az **érvényességi időtartam** **2 év**.

4. A **kérelmek kezelésére** szolgáló lapon engedélyezze a **titkos kulcs exportálásának engedélyezése lehetőséget** , majd kattintson a **kriptográfiai vagy titkosítási lap** elemre.

5. A **CSP kiválasztása** párbeszédpanelen tiltsa le a Microsoft **Base kriptográfiai szolgáltató 1.0-s verzióját** , tiltsa le a **Microsoft Enhanced kriptográfiai szolgáltató 1.0-s verzióját** , engedélyezze a **Microsoft Enhanced RSA és AES titkosítási szolgáltatót** , majd kattintson **az OK** gombra.

6. A **Biztonság** lapon hajtsa végre a következő műveleteket:

    - Távolítsa el a **rendszergazdát**.

    - **Tartományi rendszergazdák** eltávolítása.

    - Csak **olvasási** és **írási** engedélyeket rendeljen a **vállalati rendszergazdákhoz**.

    - **MIMCMEnrollAgent** hozzáadása.

    - **Olvasási** **és** beléptetési engedélyek kiosztása a **MIMCMEnrollAgent**.

7. Az **új sablon tulajdonságai** párbeszédpanelen kattintson **az OK** gombra.

8. Hagyja megnyitva a **Tanúsítványsablonok konzolt** .

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>A MIM CM kulcs-helyreállítási ügynök tanúsítványsablon létrehozása

1. A **Tanúsítványsablonok** konzol **részletek** ablaktábláján válassza ki és kattintson a jobb gombbal a kulcs- **helyreállítási ügynök** elemre, majd kattintson a **sablon megkettőzése** elemre.

2. A **Sablon duplikálása** párbeszédpanelen válassza a **Windows Server 2003 Enterprise** lehetőséget, majd kattintson **az OK** gombra.

3. Az **új sablon tulajdonságai** párbeszédpanel **általános** lapjának **sablon megjelenítendő neve** mezőjébe írja be **MIM cm kulcs-helyreállítási ügynök** nevét. Győződjön meg arról, hogy az **érvényességi időtartam** **2 év** a **titkosítás lapon.**

4. A **szolgáltatók kiválasztása** párbeszédpanelen tiltsa le a **Microsoft Enhanced kriptográfiai szolgáltató 1.0-s verzióját** , engedélyezze a **Microsoft bővített RSA-és AES titkosítási szolgáltatót** , majd kattintson **az OK** gombra.

5. A **kiállítási követelmények** lapon győződjön meg arról, hogy a **hitelesítésszolgáltatói tanúsítványkezelő jóváhagyása** **le van tiltva**.

6. A **Biztonság** lapon hajtsa végre a következő műveleteket:

    - Távolítsa el a **rendszergazdát**.

    - **Tartományi rendszergazdák** eltávolítása.

    - Csak **olvasási** és **írási** engedélyeket rendeljen a **vállalati rendszergazdákhoz**.

    - **MIMCMKRAgent** hozzáadása.

    - **Olvasási** **és** beléptetési engedélyek kiosztása a **KRAgent**.

7. Az **új sablon tulajdonságai** párbeszédpanelen kattintson **az OK** gombra.

8. Zárjuk be a **Tanúsítványsablonok konzolt**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>A szükséges tanúsítványsablonok közzététele a hitelesítésszolgáltatónál

1. Állítsa vissza a **hitelesítésszolgáltató** konzolját.

2. A **hitelesítésszolgáltató** konzol konzolfáján kattintson a jobb gombbal a **Tanúsítványsablonok** elemre, mutasson az **új** elemre, majd kattintson a **Kiállítandó tanúsítványsablon** elemre.

3. A **Tanúsítványsablonok engedélyezése** párbeszédpanelen válassza a **MIM cm beléptetési ügynök** , **MIM cm kulcs-helyreállítási ügynök** és **MIM cm aláírás** lehetőséget. Kattintson az **OK** gombra.

4. A konzolfán kattintson a **Tanúsítványsablonok** elemre.

5. Ellenőrizze, hogy a három új sablon megjelenik-e a **részletek** ablaktáblán, majd a **hitelesítésszolgáltató** bezárásával.

    ![Aláírás MIM CM](media/mim-cm-deploy/image016.png)

6. Zárjunk be minden nyitott ablakot, és jelentkezzen ki.

### <a name="iis-configuration"></a>IIS-konfiguráció

A-webhely CM-re való üzemeltetéséhez telepítse és konfigurálja az IIS-t.

#### <a name="install-and-configure-iis"></a>AZ IIS telepítése és konfigurálása

1. Bejelentkezés a **CORLog-** ba **MIMINSTALL** -fiókkal

    >[!IMPORTANT]
    >A felügyeleti webszolgáltatások telepítési fiókjának helyi rendszergazdának kell lennie

2. Nyissa meg a PowerShellt, és futtassa a következő parancsot

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Alapértelmezés szerint az IIS 7 telepíti az alapértelmezett webhely nevű helyet. Ha a hely át lett nevezve vagy el lett távolítva, akkor az alapértelmezett webhely nevű helynek elérhetőnek kell lennie MIM CM telepítése előtt.

#### <a name="configuring-kerberos"></a>A Kerberos konfigurálása

A MIMCMWebAgent-fiók a MIM CM portált fogja futtatni. Alapértelmezés szerint az IIS-ben és a kernel módú hitelesítésben alapértelmezés szerint az IIS-ben használják. Ehelyett letiltja a Kerberos kernel módú hitelesítést, és konfigurálja az SPN-ket a MIMCMWebAgent-fiókban. Egyes parancsokhoz emelt szintű engedély szükséges az Active Directoryban és a CORPCM-kiszolgálón.

![Ábra](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**AZ IIS frissítése a CORPCM-on**

![diagram](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Hozzá kell adnia egy DNS-rekordot a "cm.contoso.com" elemhez, és a CORPCM IP-re kell mutatnia.

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>SSL megkövetelése a MIM CM portálon

Erősen ajánlott az SSL megkövetelése a MIM CM portálon. Ha nem a varázsló is figyelmezteti Önt.

1. Webes tanúsítvány beléptetése a **cm.contoso.com** alapértelmezett helyhez való hozzárendeléséhez

2. Nyissa meg **az IIS-kezelőt** , és navigáljon a **tanúsítványkezelőhöz**

3. A szolgáltatások nézetben kattintson duplán az SSL-beállítások elemre.

4. Az SSL-beállítások lapon jelölje be az **SSL megkövetelése** jelölőnégyzetet.

5. A Műveletek ablaktáblán kattintson az **alkalmaz** elemre.

### <a name="database-configuration-corpsql-for-mim-cm"></a>MIM CM adatbázis-konfigurációs **CORPSQL**

1. Győződjön meg arról, hogy csatlakozik a CORPSQL01-kiszolgálóhoz.

2. Győződjön meg arról, hogy SQL DBA-ként van bejelentkezve.

3. A következő T-SQL-szkript futtatásával engedélyezze a CONTOSO \\ MIMINSTALL-fiók számára, hogy létrehozza az adatbázist, amikor a konfigurációs lépésre megyünk

    >[!NOTE]
    >Vissza kell térnie az SQL-re, amikor készen állnak a kilépési & házirend modulra

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![MIM CM konfigurációs varázsló hibaüzenete](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Microsoft Identity Manager 2016 tanúsítványkezelő üzembe helyezése

1. Győződjön meg arról, hogy csatlakozik a CORPCM-kiszolgálóhoz, valamint arról, hogy a **MIMINSTALL** -fiók tagja a **helyi rendszergazdák** csoportnak.

2. Győződjön meg arról, hogy a contoso MIMINSTALL van bejelentkezve \\ .

3. Csatlakoztassa a Microsoft Identity Manager 2016 SP1 vagy újabb szervizcsomaggal ellátott ISO-t.

4. **Nyissa meg** a **tanúsítványkezelő \\ x64** könyvtárat.

5. Az **x64** ablakban kattintson a jobb gombbal a **telepítés** elemre, majd kattintson a **Futtatás rendszergazdaként** parancsra.

6. Az üdvözli a Microsoft Identity Manager Tanúsítványkezelő telepítővarázsló lapon kattintson a **Tovább gombra.**

7. A End-User licencszerződés lapon olvassa el a szerződést, engedélyezze az Elfogadom a licencszerződés feltételeit **jelölőnégyzetet** , majd kattintson a Tovább gombra.

8. Az egyéni telepítés lapon győződjön meg arról, hogy a **MIM cm-portál** és a **MIM cm frissítési szolgáltatás-összetevői** telepítve vannak, majd **kattintson a Tovább gombra**.

9. A virtuális webes mappa lapon győződjön meg arról, hogy a virtuális mappa neve **CertificateManagement** , majd **kattintson a Tovább gombra**.

10. A Microsoft Identity Manager Tanúsítványkezelő telepítése lapon kattintson a **Telepítés gombra**.

11. A Microsoft Identity Manager Tanúsítványkezelő telepítővarázsló **befejezése** lapon **kattintson a Befejezés gombra**.

![MIM CM varázsló befejezve](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>A Microsoft Identity Manager 2016 tanúsítványkezelő varázsló konfigurálása

Mielőtt bejelentkezne a CORPCM, adja hozzá a MIMINSTALL-t a **Tartománygazdák, a Sémagazdák és a helyi rendszergazdák** csoport számára a konfigurációs varázslóhoz. Ezt később is eltávolíthatja a konfigurálás befejeződése után.

![Hibaüzenet](media/mim-cm-deploy/image028.png)

1. A **Start** menüben kattintson a **tanúsítványkezelő konfiguráció varázsló** elemre. És Futtatás **rendszergazdaként**

2. Az **üdvözli a konfigurációs varázsló** lapon kattintson a **tovább** gombra.

3. A **hitelesítésszolgáltató konfigurációja** lapon győződjön meg arról, hogy a kiválasztott hitelesítésszolgáltató a **contoso-CORPCA-CA** , győződjön meg arról, hogy a kiválasztott kiszolgáló     **CORPCA. CONTOSO.COM** , majd kattintson a **tovább** gombra.

4. A **Microsoft® SQL Server® adatbázis beállítása** lap SQL Server mezőjében írja be a **CORPSQL1** **nevet** , engedélyezze a **hitelesítő adatok használata az adatbázis létrehozásához** jelölőnégyzetet, majd kattintson a **tovább** gombra.

5. Az **adatbázis-beállítások** lapon fogadja el az **FIMCertificateManagement** alapértelmezett nevét, győződjön meg arról, hogy az **SQL integrált hitelesítés** van kiválasztva, majd kattintson a **tovább** gombra.

6. A **Active Directory beállítása** lapon fogadja el a szolgáltatási kapcsolódási ponthoz megadott alapértelmezett nevet, majd kattintson a **tovább** gombra.

7. A **hitelesítési módszer** lapon jelölje be az **integrált Windows-hitelesítés** jelölőnégyzetet, majd kattintson a **tovább** gombra.

8. Az **ügynökök – FIM cm** lapon törölje a jelet az **FIM cm alapértelmezett beállításainak használata** jelölőnégyzetből, majd kattintson az **Egyéni fiókok** elemre.

9. Az **ügynökök – FIM cm** multi-füles párbeszédpanelen az egyes lapokon írja be a következő adatokat:

   - Felhasználónév: **frissítés**

   - Jelszó: **pass \@ word1**

   - Jelszó megerősítése: **pass \@ word1**

   - Meglévő felhasználó használata: **engedélyezve**

     >[!NOTE]
     >Ezeket a fiókokat korábban hoztuk létre. Győződjön meg arról, hogy a 8. lépésben szereplő eljárások megismétlődnek mind a hat ügynök-fiók lapjain.

     ![Fiókok MIM CM](media/mim-cm-deploy/image030.png)

10. Ha az összes ügynök fiókadatok befejeződik, kattintson **az OK** gombra.

11. Az **ügynökök – MIM cm** lapon kattintson a **tovább** gombra.

12. A **kiszolgálói tanúsítványok beállítása** lapon engedélyezze a következő tanúsítványsablont:

    - A helyreállítási ügynök kulcs-helyreállítási megbízottjának tanúsítványához használandó tanúsítványsablon: **MIMCMKeyRecoveryAgent**.

    - A FIM CM-ügynök tanúsítványához használni kívánt tanúsítványsablon: **MIMCMSigning**.

    - A beléptetési ügynök tanúsítványához használni kívánt tanúsítványsablon: **FIMCMEnrollmentAgent**.

13. A **kiszolgálói tanúsítványok beállítása** lapon kattintson a **tovább** gombra.

14. Az **E-mail kiszolgáló beállítása, dokumentum nyomtatása** lap **adja meg az e-mailek regisztrációs értesítéseihez használni kívánt SMTP-kiszolgáló nevét** , majd kattintson a **Tovább gombra.**

15. A **Konfigurálásra kész** oldalon kattintson a **Konfigurálás** gombra.

16. A **konfigurációs varázsló – Microsoft Forefront Identity Manager 2010 R2** figyelmeztetés párbeszédpanelen kattintson **az OK** gombra, hogy a rendszer visszaigazolja, hogy az SSL nincs engedélyezve az IIS virtuális könyvtárában.

    ![média/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Ne kattintson a Befejezés gombra, amíg a konfigurációs varázsló végrehajtása be nem fejeződik. A varázsló naplózása itt található: **% ProgramFiles% \\ Microsoft Forefront Identity Management \\ 2010 \\ Certificate Management \\ config. log**

17. Kattintson a **Finish** (Befejezés) gombra.

    ![MIM CM varázsló befejezve](media/mim-cm-deploy/image033.png)

18. Az összes megnyitott ablak bezárásához.

19. Adja hozzá a `https://cm.contoso.com/certificatemanagement` -t a böngészőben a helyi intranet zónához.

20. Hely meglátogatása a kiszolgáló CORPCM `https://cm.contoso.com/certificatemanagement`  

    ![diagram](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>A CNG-kulcs elkülönítési szolgáltatásának ellenőrzése

1. A **felügyeleti eszközök** területen nyissa meg a **szolgáltatások** elemet.

2. A **részleteket** tartalmazó ablaktáblán kattintson duplán a **CNG-kulcs elkülönítése** elemre.

3. Az **általános** lapon módosítsa az **indítási típust** **automatikus** értékre.

4. Az **általános** lapon indítsa el a szolgáltatást, ha az nincs elindítva állapotban.

5. Az **általános** lapon kattintson az **OK** gombra.

### <a name="installing-and-configuring-the-ca-modules-"></a>A HITELESÍTÉSSZOLGÁLTATÓI modulok telepítése és konfigurálása:

Ebben a lépésben telepíti és konfigurálja a FIM CM HITELESÍTÉSSZOLGÁLTATÓI modulokat a hitelesítésszolgáltatón.

1. A FIM CM konfigurálása, hogy csak a kezelési műveletek felhasználói engedélyei legyenek megvizsgálva

2. A **C: \\ Program Files \\ Microsoft Forefront Identity Manager \\ 2010 \\ tanúsítványkezelő \\ webes** ablakában készítsen másolatot **web.config** a másolási **web.1.config** elnevezéséről.

3. A **webes** ablakban kattintson a jobb gombbal a **Web.config** elemre, majd kattintson a **Megnyitás** parancsra.

    >[!Note]
    >A Web.config fájl megnyitva a Jegyzettömbben

4. A fájl megnyitásakor nyomja le a CTRL + F billentyűkombinációt.

5. A **Keresés és csere** párbeszédpanelen a **keresett** szöveg mezőbe írja be a **UseUser** , majd kattintson háromszor a **tovább** gombra.

6. Zárja be a **Keresés és csere** párbeszédpanelt.

7. A sorban kell lennie **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser,UseGroups” /\>** . Módosítsa a sort olvasásra **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser” /\>** .

8. A fájl bezárásával mentse az összes módosítást.

9. Fiók létrehozása a HITELESÍTÉSSZOLGÁLTATÓI számítógéphez az SQL Serveren \<no script\>

10. Győződjön meg arról, hogy csatlakozik a **CORPSQL01** -kiszolgálóhoz.

11. Győződjön meg róla, hogy **DBA** -ként van bejelentkezve

12. A **Start** menüben indítsa el **SQL Server Management Studio**.

13. A **Kapcsolódás a kiszolgálóhoz** párbeszédpanel kiszolgálónév mezőjébe írja be a CORPSQL01 **nevet** **,** majd kattintson a **Kapcsolódás** elemre.

14. A konzolfán bontsa ki a **Biztonság** csomópontot, majd kattintson a **bejelentkezések** elemre.

15. Kattintson a jobb gombbal a **bejelentkezések** elemre, majd kattintson az **új bejelentkezés** lehetőségre.

16. Az **általános** lap **bejelentkezési név** mezőjébe írja be a **contoso \\ CORPCA \$** nevet. Válassza a **Windows-hitelesítés** lehetőséget. Az alapértelmezett adatbázis a **FIMCertificateManagement**.

17. A bal oldali ablaktáblán válassza a **felhasználó leképezése** elemet. A jobb oldali ablaktáblán kattintson a **FIMCertificateManagement** melletti **Térkép** oszlopban található jelölőnégyzetre. Az **adatbázis-szerepkör tagsága: FIMCertificateManagement** listán engedélyezze a **clmApp** szerepkört.

18. Kattintson az **OK** gombra.

19. **Microsoft SQL Server Management Studio** bezárásához.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Az FIM CM HITELESÍTÉSSZOLGÁLTATÓI moduljainak telepítése a hitelesítésszolgáltatóra

1. Győződjön meg arról, hogy csatlakozik a **CORPCA** -kiszolgálóhoz.

2. Az **x64** -es Windowsban kattintson a jobb gombbal a **Setup.exe** elemre, majd kattintson a **Futtatás rendszergazdaként** parancsra.

3. Az **üdvözli a Microsoft Identity Manager tanúsítványkezelő telepítővarázsló** lapon kattintson a **tovább** gombra.

4. A **végfelhasználói licencszerződés** oldalon olvassa el a szerződést. Jelölje be az **Elfogadom a licencszerződés feltételeit** jelölőnégyzetet, majd kattintson a **tovább** gombra.

5. Az **Egyéni telepítés** lapon válassza a **MIM cm portál** lehetőséget, majd kattintson **a funkció nem lesz elérhető**.

6. Az **Egyéni telepítés** lapon válassza a **MIM cm frissítési szolgáltatás** lehetőséget, majd kattintson **a funkció nem lesz elérhető**.

    >[!Note]
    >Ezzel a beállítással a MIM CM HITELESÍTÉSSZOLGÁLTATÓI fájlok csak a telepítéshez engedélyezett szolgáltatásként jelennek meg.

7. Az **Egyéni telepítés** lapon kattintson a **tovább** gombra.

8. A **Microsoft Identity Manager tanúsítványkezelő telepítése** lapon kattintson a **telepítés** gombra.

9. A **Microsoft Identity Manager tanúsítványkezelő telepítővarázsló befejezése** lapon kattintson a **Befejezés** gombra.

10. Az összes megnyitott ablak bezárásához.

### <a name="configure-the-mim-cm-exit-module"></a>A MIM CM kilépési modul konfigurálása

1. A **felügyeleti eszközök** területen nyissa meg a **hitelesítésszolgáltatót**.

2. A konzolfán kattintson a jobb gombbal a **contoso-CORPCA-CA** elemre, majd kattintson a  **Tulajdonságok** elemre.

3. A **kilépési modul** lapon válassza ki a **FIM cm kilépési modul** elemet, majd kattintson a  **Tulajdonságok** elemre.

4. A **cm-adatbázis kapcsolati karakterláncának megadása** mezőbe írja be a következőt: **csatlakozási időkorlát = 15; Biztonsági adatok megőrzése = True; Integrált biztonság = SSPI; kezdeti katalógus = FIMCertificateManagement; adatforrás = CORPSQL01**. Hagyja engedélyezve a  **kapcsolatok karakterláncának titkosítása** jelölőnégyzetet, majd kattintson **az OK** gombra.
5. A **Microsoft FIM Certificate Management** üzenet mezőben kattintson az **OK** gombra.

6. A **contoso-CORPCA-CA tulajdonságai** párbeszédpanelen kattintson az **OK** gombra.

7. Kattintson a jobb gombbal a **contoso-CORPCA-CA** elemre *,* mutasson a **minden feladat** pontra, majd kattintson a **szolgáltatás leállítása** parancsra. Várjon, amíg Active Directory tanúsítványszolgáltatás leáll.

8. Kattintson a jobb gombbal a **contoso-CORPCA-CA** elemre *,* mutasson a **minden feladat** pontra, majd kattintson a **szolgáltatás indítása** parancsra.

9. Csökkentse a **hitelesítésszolgáltató** konzolját.

10. A **felügyeleti eszközök** területen nyissa meg **Eseménynapló**.

11. A konzolfán bontsa ki az **alkalmazás és szolgáltatások naplók** csomópontot, majd kattintson a **FIM-tanúsítvány kezelése** elemre.

12. Az események listájában ellenőrizze, hogy a tanúsítványszolgáltatások legutóbbi újraindítása óta a legújabb események *nem* tartalmaznak-e **Figyelmeztetési** vagy **hibaüzeneti** eseményeket.

    >[!NOTE] 
    >Az utolsó eseménynek azt kell megjelennie, hogy a kilépési modul a következő beállítások alapján töltődött be: `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Csökkentse a **Eseménynapló**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>A MIMCMAgent-tanúsítvány ujjlenyomatának másolása a Windows® vágólapra

1. Állítsa vissza a **hitelesítésszolgáltató** konzolját.

2. A konzolfán bontsa ki a **contoso-CORPCA-CA** csomópontot, majd kattintson a **kiállított tanúsítványok** elemre.

3. A **részleteket** tartalmazó ablaktáblán kattintson duplán a **contoso \\ MIMCMAgent** tanúsítványra a **kérelmező neve** oszlopban, majd a **Tanúsítványsablon** oszlopban a **FIM cm-aláírás** lehetőségre.

4. A **Részletek** lapon válassza ki az **Ujjlenyomat** mezőt.

5. Jelölje ki az ujjlenyomatot, majd nyomja le a CTRL + C billentyűkombinációt.

    >[!NOTE]
    >Ne **adja meg** a kezdő helyet az ujjlenyomat-karakterek listájában.

6. A **tanúsítvány** párbeszédpanelen kattintson az **OK** gombra.

7. A **Start** menü **Keresés programok és fájlok** mezőjébe írja be a **Jegyzettömb** kifejezést, majd nyomja le az ENTER billentyűt.

8. A **Jegyzettömb** **Szerkesztés** menüjében kattintson a **Beillesztés** elemre.

9. A **Szerkesztés** menüben kattintson a **replace (csere** ) lehetőségre.

10. A **keresett** szöveg mezőbe írja be a szóköz karaktert, majd kattintson az **összes cseréje** gombra.

    >[!Note]
    >Ezzel eltávolítja az összes szóközt az ujjlenyomatban szereplő karakterek között.

11. A **replace (csere** ) párbeszédpanelen kattintson a **Mégse** gombra.

12. Válassza ki a konvertált *thumbprintstring* , majd nyomja le a CTRL + C billentyűkombinációt.

13. A módosítások mentése nélkül zárjuk be a **jegyzettömböt** .

### <a name="configure-the-fim-cm-policy-module"></a>A FIM CM házirend moduljának konfigurálása

1. Állítsa vissza a **hitelesítésszolgáltató** konzolját.

2. Kattintson a jobb gombbal a **contoso-CORPCA-CA** elemre, majd kattintson a **Tulajdonságok** elemre.

3. A **contoso-CORPCA-CA tulajdonságai** párbeszédpanel **házirend modul** lapján kattintson a **Tulajdonságok** elemre.

    - Az **általános** lapon győződjön meg arról, hogy be van jelölve a **nem FIM cm-kérelmek továbbítása az alapértelmezett házirend-modulba** beállítás.

    - Az **aláíró tanúsítványok** lapon kattintson a **Hozzáadás** gombra.

    - A tanúsítvány párbeszédpanelen kattintson a jobb gombbal az **adja meg a hexadecimális kódolású tanúsítvány kivonata** mezőt, majd kattintson a **Beillesztés** elemre.

    - A **tanúsítvány** párbeszédpanelen kattintson az **OK** gombra.

        >[!Note]
        >Ha az **OK** gomb nincs engedélyezve, akkor véletlenül egy rejtett karaktert is tartalmazott az ujjlenyomat-karakterláncban, amikor az ujjlenyomatot a clmAgent-tanúsítványból másolta. Ismételje meg az összes lépést a **4. feladattól kezdődően: másolja a MIMCMAgent-tanúsítvány ujjlenyomatát a Windows vágólapra** ebben a gyakorlatban.

4. A **konfiguráció tulajdonságai** párbeszédpanelen ellenőrizze, hogy az ujjlenyomat megjelenik-e az **érvényes aláíró tanúsítványok** listában, majd kattintson az **OK** gombra.

5. A **FIM Certificate Management** üzenet mezőben kattintson az **OK** gombra.

6. A **contoso-CORPCA-CA tulajdonságai** párbeszédpanelen kattintson az **OK** gombra.

7. Kattintson a jobb gombbal a **contoso-CORPCA-CA** elemre *,* mutasson a **minden feladat** pontra, majd kattintson a **szolgáltatás leállítása** parancsra.

8. Várjon, amíg Active Directory tanúsítványszolgáltatás leáll.

9. Kattintson a jobb gombbal a **contoso-CORPCA-CA** elemre *,* mutasson a **minden feladat** pontra, majd kattintson a **szolgáltatás indítása** parancsra.

10. A **hitelesítésszolgáltató** konzoljának bezárásához.

11. Zárjunk be minden megnyitott ablakot, majd jelentkezzen ki.

**Az üzembe helyezés utolsó lépéseként** meg szeretnénk győződni arról, hogy \\ a contoso MIMCM-kezelői telepíthetnek és létrehozhatnak sablonokat, és a rendszer konfigurálását a séma és a Tartománygazdák nélkül is konfigurálhatja. A következő szkript a dsacls használatával fogja ACL-ként használni a Tanúsítványsablonok engedélyeit. Olyan fiókkal futtassa, amely teljes körű jogosultsággal rendelkezik a biztonsági olvasási és írási engedélyek módosításához az erdőben lévő összes meglévő tanúsítványsablon számára.

Első lépések: **a szolgáltatási kapcsolódási pont és a célcsoport engedélyeinek konfigurálása & a profil-sablonok felügyeletének delegálása**

1. Konfigurálja az engedélyeket a szolgáltatáskapcsolódási pont (SCP) számára.

2. Delegált profil-sablon felügyeletének konfigurálása.

3. Konfigurálja az engedélyeket a szolgáltatáskapcsolódási pont (SCP) számára. **\<no script\>**

4.   Győződjön meg arról, hogy a **CORPDC** virtuális kiszolgálóhoz csatlakozik.

5. Bejelentkezés **contoso \\ corpadmin**

6. A **felügyeleti eszközök** területen nyissa meg **Active Directory felhasználókat és számítógépeket**.

7. **Active Directory felhasználók és számítógépek** **nézet** menüjében ellenőrizze, hogy a **Speciális funkciók** engedélyezve vannak-e.

8. A konzolfán bontsa ki a **contoso.com** \| **System** \| **Microsoft** \| **Certificate Lifecycle Manager elemet** , majd kattintson a **CORPCM** elemre.

9. Kattintson a jobb gombbal a **CORPCM** elemre, majd kattintson a **Tulajdonságok** elemre.

10. A **CORPCM tulajdonságai** párbeszédpanel **Biztonság** lapján adja hozzá a következő csoportokat a megfelelő engedélyekkel:

    | Csoport          | Engedélyek      |
    |----------------|------------------|
    | mimcm – vezetők | Olvasás </br> FIM CM audit</br> FIM CM beléptetési ügynök</br> FIM CM-kérelem regisztrálása</br> FIM CM-kérelem helyreállítása</br> FIM CM-kérelem megújítása</br> FIM CM-kérelem visszavonása </br> FIM CM kérelem feloldása intelligens kártya |
    | mimcm – segélyszolgálat | Olvasás</br> FIM CM beléptetési ügynök</br> FIM CM-kérelem visszavonása</br> FIM CM kérelem feloldása intelligens kártya |

11. A **CORPDC tulajdonságai** párbeszédpanelen kattintson az **OK** gombra.

12. Hagyja nyitva **Active Directory felhasználókat és számítógépeket** .

**Engedélyek konfigurálása a leszármazott felhasználói objektumokhoz**

1. Győződjön meg arról, hogy továbbra is a **Active Directory felhasználók és számítógépek** konzolon van.

2. A konzolfán kattintson a jobb gombbal a **contoso.com** elemre, majd kattintson a **Tulajdonságok** elemre.

3. A **Biztonság** fülön kattintson a **Speciális** lehetőségre.

4. A **contoso speciális biztonsági beállításai** párbeszédpanelen kattintson a **Hozzáadás** gombra.

5. A **felhasználó, számítógép, szolgáltatásfiók vagy csoport kiválasztása** párbeszédpanelen az **írja be a kijelölendő objektum nevét** mezőbe írja be a **mimcm-Managers** nevet, majd kattintson az **OK** gombra.

6. A **contoso engedélyezési bejegyzése** párbeszédpanel **alkalmazás** listájában válassza ki a **leszármazott felhasználói objektumok** elemet, majd engedélyezze a következő **engedélyek** **engedélyezésének** engedélyezése jelölőnégyzetet:

    - **Az összes tulajdonság olvasása**

    - **Olvasási engedélyek**

    - **FIM CM audit**

    - **FIM CM beléptetési ügynök**

    - **FIM CM-kérelem regisztrálása**

    - **FIM CM-kérelem helyreállítása**

    - **FIM CM-kérelem megújítása**

    - **FIM CM-kérelem visszavonása**

    - **FIM CM kérelem feloldása intelligens kártya**

7. A **contoso engedélyezési bejegyzése** párbeszédpanelen kattintson az **OK** gombra.

8. A **contoso speciális biztonsági beállításai** párbeszédpanelen kattintson a **Hozzáadás** gombra.

9. A **felhasználó, számítógép, szolgáltatásfiók vagy csoport kiválasztása** párbeszédpanelen az **írja be a kijelölendő objektum nevét** mezőbe írja be a **mimcm-segélyszolgálat** kifejezést, majd kattintson az **OK** gombra.

10. A **contoso engedélyezési bejegyzése** párbeszédpanel **alkalmazás** listájában válassza ki a **leszármazott felhasználói objektumok** elemet, majd jelölje be az **Engedélyezés** jelölőnégyzetet a következő **engedélyekhez** :

    - **Az összes tulajdonság olvasása**

    - **Olvasási engedélyek**

    - **FIM CM beléptetési ügynök**

    - **FIM CM-kérelem visszavonása**

    - **FIM CM kérelem feloldása intelligens kártya**

11. A **contoso engedélyezési bejegyzése** párbeszédpanelen kattintson az **OK** gombra.

12. A **contoso speciális biztonsági beállításai** párbeszédpanelen kattintson **az OK** gombra.

13. A **contoso.com tulajdonságai** párbeszédpanelen kattintson az **OK** gombra.

14. Hagyja nyitva **Active Directory felhasználókat és számítógépeket** .

**Engedélyek konfigurálása a leszármazott felhasználói objektumokhoz \<no script\>**

1. Győződjön meg arról, hogy továbbra is a **Active Directory felhasználók és számítógépek** konzolon van.

2. A konzolfán kattintson a jobb gombbal a **contoso.com** elemre, majd kattintson a **Tulajdonságok** elemre.

3. A **Biztonság** fülön kattintson a **Speciális** lehetőségre.

4. A **contoso speciális biztonsági beállításai** párbeszédpanelen kattintson a **Hozzáadás** gombra.

5. A **felhasználó, számítógép, szolgáltatásfiók vagy csoport kiválasztása** párbeszédpanelen az **írja be a kijelölendő objektum nevét** mezőbe írja be a **mimcm-Managers** nevet, majd kattintson az **OK** gombra.

6. A **contoso engedélyezési bejegyzése** párbeszédpanel **alkalmazás** listájában válassza ki a **leszármazott felhasználói objektumok** elemet, majd engedélyezze a következő **engedélyek** **engedélyezésének** engedélyezése jelölőnégyzetet:

    - **Az összes tulajdonság olvasása**

    - **Olvasási engedélyek**

    - **FIM CM audit**

    - **FIM CM beléptetési ügynök**

    - **FIM CM-kérelem regisztrálása**

    - **FIM CM-kérelem helyreállítása**

    - **FIM CM-kérelem megújítása**

    - **FIM CM-kérelem visszavonása**

    - **FIM CM kérelem feloldása intelligens kártya**

7. A **contoso engedélyezési bejegyzése** párbeszédpanelen kattintson az **OK** gombra.

8. A **contoso speciális biztonsági beállításai** párbeszédpanelen kattintson a **Hozzáadás** gombra.

9. A **felhasználó, számítógép, szolgáltatásfiók vagy csoport kiválasztása** párbeszédpanelen az **írja be a kijelölendő objektum nevét** mezőbe írja be a **mimcm-segélyszolgálat** kifejezést, majd kattintson az **OK** gombra.

10. A **contoso engedélyezési bejegyzése** párbeszédpanel **alkalmazás** listájában válassza ki a **leszármazott felhasználói objektumok** elemet, majd jelölje be az **Engedélyezés** jelölőnégyzetet a következő **engedélyekhez** :

    - **Az összes tulajdonság olvasása**

    - **Olvasási engedélyek**

    - **FIM CM beléptetési ügynök**

    - **FIM CM-kérelem visszavonása**

    - **FIM CM kérelem feloldása intelligens kártya**

11. A **contoso engedélyezési bejegyzése** párbeszédpanelen kattintson az **OK** gombra.

12. A **contoso speciális biztonsági beállításai** párbeszédpanelen kattintson **az OK** gombra.

13. A **contoso.com tulajdonságai** párbeszédpanelen kattintson az **OK** gombra.

14. Hagyja nyitva **Active Directory felhasználókat és számítógépeket** .

Második lépések: **Tanúsítványsablon-kezelési engedélyek \<script\> delegálása**

- Engedélyek delegálása a Tanúsítványsablonok tárolón.

- Engedélyek delegálása az OID-tárolón.

- Engedélyek delegálása a meglévő tanúsítványsablonok számára.

Engedélyek definiálása a Tanúsítványsablonok tárolón:

1. Állítsa vissza a **Active Directory helyek és szolgáltatások** konzolt.

2. A konzolfán bontsa ki a **szolgáltatások** , majd a **nyilvánoskulcs-szolgáltatások** csomópontot, és kattintson a **Tanúsítványsablonok** elemre.

3. A konzolfán kattintson a jobb gombbal a **Tanúsítványsablonok** elemre, majd kattintson a **vezérlő delegálása** parancsra.

4. A **vezérlés delegálása** varázslóban kattintson a **tovább** gombra.

5. A **felhasználók vagy csoportok** lapon kattintson a **Hozzáadás** gombra.

6. A **felhasználók, számítógépek vagy csoportok kiválasztása** párbeszédpanelen az **adja meg a kijelölendő objektumok nevét** mezőbe írja be a **mimcm-Managers** nevet, majd kattintson az **OK** gombra.

7. A **felhasználók vagy csoportok** lapon kattintson a **tovább** gombra.

8. A **delegálni kívánt feladatok** lapon kattintson az **Egyéni feladat létrehozása delegálásra** elemre, majd kattintson a **tovább** gombra.

9.  A **Active Directory objektumtípus** lapon győződjön meg arról, hogy **Ez a mappa, a mappa meglévő objektumai és új objektumok létrehozása** ebben a mappában lehetőség van kiválasztva, majd kattintson a **tovább** gombra.

10. Az **engedélyek** lap **engedélyek** listájában jelölje be a **teljes hozzáférés** jelölőnégyzetet, majd kattintson a **tovább** gombra.

11. A **varázsló delegálásának befejezése** lapon kattintson a **Befejezés** gombra.

Engedélyek definiálása az OID-tárolón:

1. A konzolfán kattintson a jobb gombbal az **OID** elemre, majd kattintson a **Tulajdonságok** elemre.

2. Az **OID tulajdonságai** párbeszédpanel **Biztonság** lapján kattintson a **speciális** elemre.

3. Az **OID speciális biztonsági beállításai** párbeszédpanelen kattintson a **Hozzáadás** gombra.

4. A **felhasználó, számítógép, szolgáltatásfiók vagy csoport kiválasztása** párbeszédpanelen az **írja be a kijelölendő objektum nevét** mezőbe írja be a **mimcm-Managers** nevet, majd kattintson az **OK** gombra.

5. Győződjön meg arról, hogy az **engedélyek az OID-hez** párbeszédpanelen az **objektumra és az összes leszármazott objektumra** érvényesek, majd kattintson a **teljes hozzáférés** elemre, végül pedig **az OK** gombra.

6. Az **OID speciális biztonsági beállításai** párbeszédpanelen kattintson **az OK** gombra.

7. Az **OID tulajdonságai** párbeszédpanelen kattintson az **OK** gombra.

8. Zárjunk be **Active Directory helyeket és szolgáltatásokat**.

**Parancsfájlok: engedélyek az OID-hez, profil sablon & tanúsítványsablonok tárolója**

![diagram](media/mim-cm-deploy/image021.png)

```powershell
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

**Parancsfájlok: engedélyek delegálása a meglévő tanúsítványsablonok számára.**  

![diagram](media/mim-cm-deploy/image039.png)

```shell
dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
```
