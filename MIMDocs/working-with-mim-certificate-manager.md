---
title: A MIM Tanúsítványkezelő Windows-alkalmazásának telepítése | Microsoft Docs
description: Tájékozódjon arról, hogyan helyezheti üzembe a Tanúsítványkezelő alkalmazást, amely lehetővé teszi a felhasználók számára saját hozzáférési jogosultságaik kezelését.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/16/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2adf1152aaf874d0ff0d93079fb4bfbfcf731b60
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044293"
---
# <a name="mim-certificate-manager-windows-store-application-deployment"></a>A Rendszerfelügyeleti webszolgáltatások Tanúsítványkezelője Windows áruházbeli alkalmazás telepítése

Miután megtörtént a felügyeleti webszolgáltatások 2016 és a Tanúsítványkezelő üzembe helyezése, telepítheti a Rendszerfelügyeleti webszolgáltatások tanúsítványkezelő Windows áruházbeli alkalmazását. A Windows áruházbeli alkalmazással a felhasználók kezelhetik a fizikai intelligens kártyákat, a virtuális intelligens kártyákat és a szoftveres tanúsítványokat. A MIM Tanúsítványkezelő alkalmazás telepítési lépései:

1. Tanúsítványsablon létrehozása.

2. Profilsablon létrehozása.

3. Az alkalmazás előkészítése.

4. Az alkalmazás üzembe helyezése az SCCM-en vagy az Intune-on keresztül.

## <a name="create-a-certificate-template"></a>Tanúsítványsablon létrehozása

A Tanúsítványkezelő alkalmazáshoz a szokásos módon hozhat létre tanúsítványsablont, azzal a különbséggel, hogy meg kell győződnie arról, hogy a tanúsítványsablon 3-as vagy magasabb verziójú.

1. Jelentkezzen be az AD CS-t futtató kiszolgálóra (a tanúsítvány-kiszolgálóra).

2. Indítsa el az MMC-t.

3. Kattintson **a &gt; fájl beépülő modul hozzáadása/eltávolítása**elemre.

4. Az Elérhető beépülő modulok listában kattintson a **Tanúsítványsablonok** lehetőségre, majd a **Hozzáadás** gombra.

5. Ekkor az MMC-ben a **Konzolgyökér** területen megjelenik a **Tanúsítványsablonok** lehetőség. A rendelkezésre álló tanúsítványsablonok megtekintéséhez kattintson rá duplán.

6. Kattintson a jobb gombbal a **Bejelentkezés intelligens kártyával** sablonra, majd kattintson a **Sablon duplikálása** parancsra.

7. A kompatibilitás lapon a hitelesítésszolgáltató területen válassza a Windows Server 2008 lehetőséget. A tanúsítvány címzettje területen válassza a Windows 8.1/Windows Server 2012 R2 lehetőséget. A sablon verziószáma a tanúsítványsablon első létrehozásakor és mentésekor van beállítva. Ha így nem hozta létre a tanúsítványsablont, a megfelelő verzióra nem lehet módosítani.

   > [!NOTE]
   >  Ez a lépés kritikus fontosságú, mivel gondoskodik arról, hogy 3 (vagy magasabb) verziójú tanúsítványsablont biztosítson. Csak a 3. verziójú sablonok működnek a Tanúsítványkezelő alkalmazással.

8. Az **Általános** lap **Megjelenített név** mezőjébe írja be az alkalmazás kezelőfelületén megjeleníteni kívánt nevet; például: **Bejelentkezés virtuális intelligens kártyával**.

9. A **Kérelmek kezelése** lapon állítsa a **Felhasználási cél** beállítást **Aláírás és titkosítás** értékre, majd **A következő történjen...** beállításnál válassza a **Felhasználó értesítése az igénylés alatt** lehetőséget.

10. A **Szolgáltató besorolása** területen, a **Titkosítás** lapon válassza **A kérelmekhez a tulajdonos számítógépén rendelkezésre álló bármely szolgáltató használható** beállítást.

    > [!NOTE]
    > A Kulcstároló-szolgáltató beállítás csak akkor jelenik meg, ha a sablon 3-as verziójú. Ha nem látható, akkor valószínűleg nem megfelelő verziójú tanúsítványsablont hozott létre. Kezdje újra a fenti 5. lépéssel.

11. A **Biztonság** lapon vegye fel azt a biztonsági csoportot, amelyhez **Igénylés** típusú hozzáférést kíván beállítani. Ha például az összes felhasználónak szeretne hozzáférést biztosítani, válassza a **Hitelesített felhasználók** csoportot, majd válasszon **igénylési engedélyt** számukra.

12. A módosítások véglegesítéséhez és az új sablon létrehozásához kattintson az **OK** gombra. Az új sablonnak ekkor meg kell jelennie a tanúsítványsablonok listájában.

13. A Hitelesítésszolgáltató beépülő modul MMC-konzolra való felvételéhez válassza a **Fájl** menü **Beépülő modul hozzáadása/eltávolítása** elemét. Ha a rendszer megkérdezi, hogy melyik számítógépet szeretné felügyelni, válassza a **helyi számítógép**lehetőséget.

14. Az MMC bal oldali ablaktábláján bontsa ki a **hitelesítésszolgáltató (helyi)** csomópontot, majd a hitelesítésszolgáltatók listájában bontsa ki a hitelesítésszolgáltatót.

15. Kattintson jobb gombbal a **Tanúsítványsablonok** elemre, majd az **Új &gt; Kiállítandó tanúsítványsablon** lehetőségre.

16. A listáról válassza ki az újonnan létrehozott sablont, majd kattintson az **OK** gombra.

## <a name="create-a-profile-template"></a>Profilsablon létrehozása

Amikor létrehozza a profilsablont, állítsa be a virtuális intelligens kártya létrehozását/megsemmisítését és az adatgyűjtemény eltávolítását. A Tanúsítványkezelő alkalmazás nem képes az összegyűjtött adatok kezelésére, ezért ezt a funkciót a következő módon le kell tiltani.

1.  Rendszergazdai jogosultsággal jelentkezzen be a Tanúsítványkezelő portálra.

2.  Nyissa meg &gt; az adminisztráció Manage Profile sablonokat. Győződjön meg arról, hogy a jelölőnégyzet be van jelölve a **MIM cm minta intelligens kártya bejelentkezési profil sablonja** mellett, majd kattintson a kiválasztott profil másolása sablonra.

3.  Írja be a profilsablon nevét, majd kattintson az **OK** gombra.

4.  A következő képernyőn kattintson az **Add new certificate template** (Új tanúsítványsablon hozzáadása) elemre, és jelölje be a hitelesítésszolgáltató neve melletti négyzetet.

5.  Jelölje be a **Logon** (Bejelentkezés) profilsablon neve melletti négyzetet, majd kattintson az **Add** (Hozzáadás) gombra.

6.  A SmartCardLogon sablon eltávolításához jelölje be a mellette található négyzetet, kattintson a **Delete selected certificate templates** (Kijelölt tanúsítványsablonok törlése) parancsra, majd az **OK** gombra.

7.  Görgessen le a képernyő aljára, és kattintson a **Change settings** (Beállítások módosítása) elemre.

8.  Jelölje be a **Create/Destroy virtual smart card** (Virtuális intelligens kártya létrehozása/megsemmisítése) és a **Diversify Admin Key** (Adminisztrációs kulcs diverzifikálása) négyzetet.

9. A **User PIN Policy** (Felhasználói PIN-házirend) területen válassza a **User Provided** (Felhasználó által megadott) lehetőséget.

10. A bal oldali panelen kattintson a **Renew Policy (Házirend megújítása) &gt; Change general settings (Általános beállítások módosítása)** parancsra. Válassza a **Reuse card on renew** (Kártya újrafelhasználása megújításkor) elemet, majd kattintson az **OK** gombra.

11. Minden házirendhez le kell tiltania az adatgyűjtési elemeket, ha a bal oldali ablaktáblán a szabályzatra kattint. Ezután jelölje be a **mintaadatok elem** melletti jelölőnégyzetet, majd kattintson az **adatgyűjtési elemek törlése** elemre, majd **az OK**gombra.

## <a name="prepare-the-cm-app-for-deployment"></a>A Tanúsítványkezelő alkalmazás üzembe helyezésének előkészítése

1. A parancssorban futtassa a következő parancsot az alkalmazás kicsomagolásához. A parancs kibontja a tartalmat egy új, Appx nevű almappába, és létrehoz egy másolatot, hogy ne módosítsa az eredeti fájlt.

    ```cmd
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2. Az „appx” mappában a CustomDataExample.xml fájlt nevezze át Custom.data névre.

3. Nyissa me a Custom.data fájlt, és szükség szerint módosítsa a paramétereket.


   |                     |                                                                                                                                                                                                          |
   |---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      MIMCM URL      |                                              A Tanúsítványkezelő konfigurálására szolgáló portál teljes tartományneve, Például: https://mimcmServerAddress/certificatemanagement                                              |
   |      ADFS URL       | Ha AD FS-t fog használni, adja meg az AD FS URL-címét, Például: <https://adfsServerSame/adfs> </br> Ha az ADFS nincs használatban, konfigurálja ezt a beállítást egy üres karakterlánccal.  Például:```<ADFS URL=""/>``` |
   |     PrivacyUrl      |                                         Megadhatja egy olyan weblap URL-címét, amely ismerteti, hogy mit tesz a tanúsítványigényléshez gyűjtött felhasználói adatokkal.                                          |
   |     SupportMail     |                                                                           Megadhat egy e-mail címet támogatási problémák esetére.                                                                           |
   | LobComplianceEnable (LOB-megfelelőség engedélyezése) |                                                                     True (igaz) vagy false (hamis) értéket állíthat be. Az alapértelmezett érték true (igaz).                                                                      |
   |  MinimumPinLength (PIN minimális hossza)   |                                                                                        Az alapértelmezett érték 6.                                                                                         |
   |      NonAdmin (Nem rendszergazda)       |           True (igaz) vagy false (hamis) értéket állíthat be. Az alapértelmezett érték false (hamis). Csak akkor módosítsa, ha a számítógépükön rendszergazdai jogosultsággal nem rendelkező felhasználók számára is lehetővé szeretné tenni a tanúsítványok igénylését és megújítását.            |

   > [!IMPORTANT]
   > Meg kell adni egy értéket az ADFS URL-címéhez. Ha nincs megadva érték, a modern alkalmazás az első használat során hibát jelez.
4. Mentse a fájlt, és zárja be a szerkesztőt.

5. A csomag aláírásával létrejön egy aláírásfájl, ezért az eredeti, AppxSignature.p7x nevű aláírásfájlt törölni kell.

6. Az AppxManifest.xml fájlt határozza meg az aláíró tanúsítvány tulajdonosnevét. Nyissa meg a fájlt a szerkesztéshez.

7. Mielőtt elkezdené a soron következő szakaszt, be kell szereznie egy aláíró tanúsítványt. Ehhez lásd az alábbi, „Intelligens kártya megújításának engedélyezése nem rendszergazda felhasználók számára az MIM 2016 Tanúsítványkezelőben” leírás 1. lépését.

8. Az &lt;Identity&gt; (Identitás) elemnél módosítsa a Publisher (Kiállító) attribútum értékét úgy, hogy az megegyezzen az aláíró tanúsítványban feltüntetett tulajdonossal; például: „CN=SUBJECT”.

9. Mentse a fájlt, és zárja be a szerkesztőt.

10. A parancssorban futtassa a következő parancsot az .appx fájl újracsomagolásához és aláírásához.

    ```cmd
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    – ahol az „app package name” a másolat létrehozásához használt név.

    ```cmd
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    Ez megadja az új .appx fájlt. A .pfx fájl megad egy helyet aláíró tanúsítványnak, valamint egy jelszót a .pfx fájl számára.

11. Az AD FS-alapú hitelesítés kezelése:

    - Indítsa el a Virtual Smart Card alkalmazást – így könnyebben megtalálhatja a következő lépéshez szükséges értékeket.

    - Ahhoz, hogy az alkalmazást ügyfélként felvehesse az AD FS-kiszolgálón és a Tanúsítványkezelőt konfigurálhassa, indítsa el a Windows PowerShellt az AD FS-kiszolgálón, és futtassa a következő parancsot: `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`

       A következő a ConfigureMimCMClientAndRelyingParty.ps1 szkript:

      ```PowerShell
       # HELP

       <#
       .SYNOPSIS
                       Configure ADFS for CM client app and server.
       .DESCRIPTION
          What the Script does:
                                       a. Registers the MIM CM client app on the ADFS server.
                                       b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                       For parameter information, see 'get-help -detailed'
       .PARAMETER redirectUri
                       The redirectUri for CM client app. Will be added to ADFS server.
                       It can be found as follows:
                       1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                       2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
       .PARAMETER serverFqdn
                       Your deployed MIM CM server’s FQDN.
       .EXAMPLE
          .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
       #>

       # Parameter declaration
       [CmdletBinding()]
       Param(
         [Parameter(Mandatory=$True)]
          [string]$redirectUri,

          [Parameter(Mandatory=$True)]
          [string]$serverFqdn
       )

       Write-Host "Configuring ADFS Objects for OAuth.."

       #Configure SSO to get persistent sign on cookie
       Set-ADFSProperties -SsoLifetime 2880

       #Configure Authentication Policy
       #Intranet to use Kerberos
       #Extranet to use U/P

       #Create Client Objects

       Write-Host "Creating Client Objects..."

       $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

       if ($existingClient -ne $null)
       {
           Write-Host "Found existing instance of the MIM CM Modern App, removing"
           Remove-ADFSClient -TargetName "MIM CM Modern App"
           Write-Host "Client object removed"
       }

       Write-Host "Adding Client Object for MIM CM Modern App client"
       Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
       Write-Host "Client Object for MIM CM Modern App client Created"

       #Create Relying Parties
       Write-Host "Creating Relying Party Objects"

       $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
       if ($existingRp -ne $null)
       {
           Write-Host "Found existing instance of the MIM CM Service RP, removing"
           Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
           Write-Host "RP object Removed"
       }

       $authorizationRules =
       "@RuleTemplate = `"AllowAllAuthzRule`"
       => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

       $issuanceRules =
       "@RuleTemplate = `"LdapClaims`"
       @RuleName = `"Emit UPN and common name`"
       c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
       => issue(store = `"Active Directory`", types =
       (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
       `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
       `";userPrincipalName,cn;{0}`", param = c.Value);

       @RuleTemplate = `"PassThroughClaims`"
       @RuleName = `"Pass through Windows Account Name`"
       c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

       Write-Host "Creating RP Trust for MIM CM Service"
       Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
       Write-Host "RP Trust for MIM CM Service has been created"
       ```

    - Módosítsa a redirectUri és a serverFQDN értékeit.

    - A redirectURI (átirányítási URI) megtalálásához a Virtual Smart Card alkalmazásban nyissa meg az alkalmazásbeállítások panelét, és kattintson a **Beállítások** lehetőségre. Az átirányítási URI-nek az AD FS-kiszolgáló címsora alatt kell szerepelnie. Az URI csak akkor jelenik meg, ha be van állítva egy AD FS-kiszolgálói cím.

    - A serverFQDN (kiszolgáló teljes tartományneve) az MIMCM-kiszolgáló teljes számítógépneve önmagában.

    - A **ConfigureMIimCMClientAndRelyingParty. ps1** parancsfájllal kapcsolatos segítségért futtassa a következőt: </br> 
      ```Powershell
      get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1
      ```

## <a name="deploy-the-app"></a>Az alkalmazás üzembe helyezése

A Tanúsítványkezelő alkalmazás telepítésekor a Letöltőközpontból töltse le a MIMDMModernApp_&lt;verziószám&gt;_AnyCPU_Test.zip fájlt, és bontsa ki a teljes tartalmát. A telepítő az .appx fájl. Az alkalmazás a Windows áruházbeli alkalmazásoknál megszokott módszerekkel telepíthető, például [System Center Configuration Managerrel](https://technet.microsoft.com/library/dn613840.aspx) vagy [Intune-nal](https://technet.microsoft.com/library/dn613839.aspx) is közvetlen telepítéssel – a felhasználóknak tehát a Vállalati portálon kell elérniük azt, vagy leküldéssel is telepíthető közvetlenül a számítógépükre.

## <a name="next-steps"></a>További lépések

- [Profil sablonok konfigurálása](https://technet.microsoft.com/library/cc708656)
- [Intelligenskártya-alkalmazások kezelése](https://technet.microsoft.com/library/cc708681)
