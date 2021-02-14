---
title: A PAM üzembe helyezése, 4. lépés – A MIM telepítése | Microsoft Docs
description: A MIM szolgáltatás és -portál telepítése és konfigurálása saját Privileged Access Management-kiszolgálón és munkaállomásokon.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/09/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8f81f592beff9ab952d1a42760e06a4f622316e8
ms.sourcegitcommit: 0e2b4b47a8050737c78e3b0ad088358e5de7e929
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395469"
---
# <a name="step-4--install-mim-components-on-pam-server-and-workstation"></a>4. lépés – MIM-összetevők telepítése PAM-kiszolgálóra és -munkaállomásra

> [!div class="step-by-step"]
> [«3](step-3-prepare-pam-server.md) 
>  . lépés [5. lépés»](step-5-establish-trust-between-priv-corp-forests.md)

A MIM szolgáltatásnak és portálnak, illetve a mintaportál webalkalmazásának telepítéséhez a PAMSRV kiszolgálón PRIV\Rendszergazda jogosultsággal kell bejelentkeznie.

  > [!NOTE]
  > Tartományi rendszergazdai jogosultsággal kell rendelkeznie. Ha az alábbi parancsokat nem tartományi rendszergazdaként futtatja, a következő lépésben nem tudja végrehajtani a megbízhatósági ellenőrzéseket.

Ha letöltötte a MIM szolgáltatást, bontsa ki a tömörített MIM-telepítőcsomagot egy új mappába.

## <a name="run-the-service-and-portal-install-program"></a>Indítsa el a Service and Portal (Szolgáltatás és portál) telepítőprogramot.

Az útmutatást követve végezze el a telepítést.

1. A telepíteni kívánt összetevők közül mindenképpen jelölje ki a MIM Service szolgáltatást (a Privileged Access Managementtel együtt, de a MIM Reporting nélkül), valamint a MIM Portal (MIM portál) komponenst.  

   ![Egyéni telepítés – képernyőkép](./media/PAM_GS_MIM_2015_Service_Portal.png)

2. A közös szolgáltatások és a MIM-adatbázis kapcsolatának konfigurálásakor válassza a **Create a new database** (Új adatbázis létrehozása) beállítást.

   > [!NOTE]
   > Ha a MIM szolgáltatást többször, magas rendelkezésre állás céljából telepíti, minden további telepítésnél a **Use an existing database** (Meglévő adatbázis használata) lehetőséget állítsa be.

3. Levelezőkiszolgálóval való kapcsolat konfigurálásakor a levelezőkiszolgálónál adja meg egy Exchange- vagy SMTP-kiszolgáló állomásnevét a CORP környezethez (ha nem rendelkezik levelezési kiszolgálóval, használja a corpdc.contoso.local nevet), és törölje a jelet a **Use SSL** (SSL használata), illetve a **Mail Server is Exchange Server 2007 or Exchange Server 2010** (A levelezési kiszolgáló az Exchange Server 2007 vagy az Exchange Server 2010) jelölőnégyzetből.

4. Válassza egy új önaláírt tanúsítvány létrehozásának lehetőségét.

5. Állítsa be az alábbi fiókhitelesítő adatokat:
   - Service Account Name (Szolgáltatásfiók neve) – *MIMService*  
   - Szolgáltatásfiók jelszava: <em>Pass@word1</em> (vagy a 2. lépésben létrehozott jelszó)  
   - Service Account Domain (Szolgáltatásfiók-tartomány): *PRIV*  
   - Szolgáltatás e-mail-fiókja: <em>MIMService@priv.contoso.local</em>  

6. A szinkronizálási kiszolgáló állomásnevénél fogadja el az alapértelmezett beállításokat, a MIM Management Agent Account (MIM-kezelőügynök fiókja) mezőben pedig adja meg a *PRIV\MIMMA* fiókot. Egy megjelenő üzenet arra figyelmezteti, hogy a MIM szinkronizálási szolgáltatása nem létezik. Erre a figyelmeztetésre azért van szükség, mert ebben a forgatókönyvben nem használja a következőt:.

7. A *PAMSRV* beállításánál adja meg a MIM szolgáltatás kiszolgálójának címét.

8. Állítsa be `http://pamsrv.priv.contoso.local:82` a SharePoint-webhelycsoport URL-címét.

9. A regisztrációs portál URL-címét hagyja üresen.

10. Jelölje be a tűzfalon az 5725-ös és az 5726-os port megnyitására szolgáló négyzetet, valamint azt, amelyik az összes hitelesített felhasználónak hozzáférést biztosít a MIM-portál webhelyéhez.

11. A PAM REST API állomásnév maradjon üresen, a portszámot pedig állítsa be a *8086* értékre.

    ![A PAM REST API kötési információi – képernyőkép](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. A MIM PAM REST API-fiók konfigurációját úgy állítsa be, hogy ugyanazt a fiókot használja, mint a SharePoint (mivel a MIM-portál is ezen a kiszolgálón található):
    - Application Pool Account Name (Alkalmazáskészlet fiókneve): *SharePoint*  
    - Alkalmazáskészlet fiókjának jelszava: <em>Pass@word1</em> (vagy a 2. lépésben létrehozott jelszó)  
    - Application Pool Account Domain (Alkalmazáskészlet-fiók tartománya): *PRIV*  

    ![Az alkalmazáskészlet fiókjának hitelesítő adatai – képernyőkép](./media/PAM_GS_Configure_Component_Service.png)

    Elképzelhető, hogy megjelenik egy üzenet, amely arra figyelmezteti, hogy a szolgáltatásfiók aktuális konfigurációja nem biztonságos. Nem gond.

13. A MIM PAM-összetevő szolgáltatásának konfigurálása:
    - Service Account Name (Szolgáltatásfiók neve): *MIMService*
    - Szolgáltatásfiók jelszava: <em>Pass@word1</em> (vagy a 2. lépésben létrehozott jelszó)  
    - Service Account Domain (Szolgáltatásfiók-tartomány): *PRIV*

    ![A PAM-összetevő szolgáltatás fiókhitelesítő adatai – képernyőkép](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. A PAM figyelőszolgáltatásának konfigurálása:
    - Service Account Name (Szolgáltatásfiók neve) – *MIMMonitor*  
    - Szolgáltatásfiók jelszava: <em>Pass@word1</em> (vagy a 2. lépésben létrehozott jelszó)  
    - Service Account Domain (Szolgáltatásfiók-tartomány): *PRIV*  

    ![A PAM figyelőszolgáltatásának fiókhitelesítő adatai – képernyőkép](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. Az Enter Information for MIM Password Portals (A MIM-jelszóportálokra vonatkozó adatok megadása) lapon hagyja üresen a jelölőnégyzeteket, és lépjen tovább. A telepítés folytatásához kattintson a **Next** (Tovább) gombra.

A telepítés befejezése után a kiszolgáló újraindul, majd ellenőrzi, hogy aktív-e a MIM portál, illetve engedélyezi, hogy a felhasználók megtekinthessék a MIM-ben a saját objektum-erőforrásaikat.

## <a name="set-up-mim-portal-management-policy-rules"></a>A MIM portál felügyeleti házirendszabályainak beállítása

1. Miután a PAMSRV újraindult, jelentkezzen be PRIV\Rendszergazda jogosultsággal.

2. Indítsa el az Internet Explorert, és kapcsolódjon a webalkalmazás-portálhoz `http://pamsrv.priv.contoso.local:82/identitymanagement` . Előfordulhat, hogy az oldal első felkeresése hosszabb ideig tart.

3. Szükség esetén jelentkezzen be PRIV\Administrator for Internet Explorer jogosultsággal.

4. Az Internet Explorerben nyissa meg az **Internetbeállítások lehetőséget**, váltson a **Biztonság** lapra, és adja hozzá a helyet a **helyi intranet zónához** , ha még nem létezik. Zárja be az Internetbeállítások párbeszédpanelt.

5. Az Internet Explorerben a MIM portál megjelenítéséhez kattintson a **Management Policy Rules** (Felügyeleti házirendszabályok) elemre.

6. Keresse meg a **User management: Users can read attributes of their own** (Felhasználók felügyelete: A felhasználók olvashatják a saját attribútumaikat) felügyeleti házirendszabályt.

7. Jelölje ki ezt a felügyeleti házirendszabályt, majd törölje a jelet a **Policy is disabled** (A házirend le van tiltva) négyzetből, kattintson az **OK**, majd a **Submit** (Küldés) gombra.

## <a name="verify-the-firewall-connections"></a>A tűzfalkapcsolatok ellenőrzése

A tűzfalnak engedélyeznie kell a bejövő kapcsolatokat az 5725-ös, az 5726-os, a 8086-os és a 8090-es TCP-porton.

1.  Nyissa meg a (Felügyeleti eszközök közt megtalálható) **Fokozott biztonságú Windows tűzfal** lehetőséget.  
2.  Kattintson a **Bejövő szabályok** elemre.  
3.  Győződjön meg arról, hogy az alábbi két szabály szerepel a listában:  
    - Forefront Identity Manager Service (STS)
    - Forefront Identity Manager Service (Webservice)  
4.  Kattintson az **új szabály**  >  **port**  >  **TCP** elemre, és írja be az *8086* -es és a *8090*-es helyi portot. A varázsló alapértelmezett értékeit elfogadva haladjon végig a folyamaton, adjon a szabálynak egy nevet, és kattintson **Befejezés** gombra.  
5.  A varázsló befejezése után zárja be a Windows tűzfalat.

6.  Nyissa meg a **Vezérlőpultot**.  
7.  A hálózat és Internet területen válassza a **hálózati állapot és feladatok megtekintése** lehetőséget.
8.  Ellenőrizze, hogy van-e olyan aktív hálózat, amely priv. contoso. local és tartományi hálózatként van felsorolva.
9. A **Vezérlőpult ablakának** bezárásához.

## <a name="optional-set-up-the-sample-web-application"></a>Nem kötelező: a minta webalkalmazás beállítása

Ebben a szakaszban telepítheti és konfigurálhatja a webalkalmazás mintáját a fakiszolgálói PAM REST API számára.  Ezt az összetevőt csak akkor kell megtanulnia, ha szeretné megismerni, hogyan kell használni a REST API. Ha a PowerShellt a hozzáférés kéréséhez és jóváhagyásához kívánja használni, folytassa a következő szakasszal, és telepítse a kipróbálható PAM-kérő parancsmagokat.

1. A minta webalkalmazások archívumából töltse le az [Identity Management samples](https://github.com/Azure/identity-management-samples) (Identity Management-minták) csomagot .zip fájlként.

2. Bontsa ki az **identity-management-samples-master\Privileged-Access-Management-Portal\src** mappa tartalmát egy új mappába, a **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal** helyre.

3. Hozzon létre az IIS-ben egy új webhelyet MIM Privileged Access felügyeleti példaportál néven, melynek fizikai elérési útja: C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal, illetve a 8090-es port.  A webhely létrehozása a következő PowerShell-paranccsal végezhető el:

   ```PowerShell
   New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
   ```

4. Állítsa be úgy a minta webalkalmazást, hogy az a felhasználókat át tudja irányítani a MIM PAM REST API-hoz. Szövegszerkesztő, például a Jegyzettömb használatával szerkessze a **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged hozzáférés-kezelési REST API\web.config** fájlt. A **<System. webserver>** szakaszban adja hozzá a következő sorokat:

   ```XML
   <httpProtocol>
   <customHeaders>
     <add name="Access-Control-Allow-Credentials" value="true"  />
     <add name="Access-Control-Allow-Headers" value="content-type" />
     <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
   </customHeaders>
   </httpProtocol>
   ```

5. Konfigurálja a minta webalkalmazást. Szövegszerkesztőben, például a Jegyzettömbben szerkessze a **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js** fájlt. A **pamRespApiUrl** értékének beállítása a következőre: `http://pamsrv.priv.contoso.local:8086/api/pamresources/` .

6. A módosítások életbe léptetéséhez indítsa újra az IIS-t a következő paranccsal.

   ```cmd
   iisreset
   ```

7. (Nem kötelező.) Győződjön meg arról, hogy a felhasználó hitelesíthető a REST API-val. Nyisson meg egy webböngészőt a PAMSRV rendszergazdájaként.  Navigáljon a webhely URL-címére, ellenőrizze, hogy `http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/` szükség van-e, és győződjön meg arról, hogy a letöltés történik.

## <a name="install-the-mim-pam-requestor-cmdlets"></a>A MIM PAM-kérelmező parancsmagjainak telepítése

Telepítse a MIM PAM-kérelmező parancsmagjait az 1. lépésben beállított munkaállomáson.

1.  Jelentkezzen be rendszergazdaként a CORPWKSTN munkaállomáson.

2.  Töltse le az **Add-ins and extensions** (Beépülő modulok és bővítmények) csomagot a CORPWKSTN számítógépre, ha még nincs letöltve.

3.  A tömörített fájlból csomagolja ki az **Add-ins and extensions** (Beépülő modulok és bővítmények) mappát egy új mappába.

4.  Futtassa a telepítő **setup.exe** fájlját.

5.  Az egyéni telepítés során válassza ki a **PAM Client** (PAM ügyfélprogram) telepítését, de a **MIM Add-in for Outlook** (MIM beépülő modul Outlookhoz) és a **MIM Password and Authentication Extensions** (MIM jelszó- és hitelesítési bővítmények) telepítését ne jelölje be.

6.  A PAM-kiszolgáló címe mezőben válassza a PRIV-kiszolgáló neve nevet `pamsrv.priv.contoso.local` .

A telepítés befejezése után indítsa újra a CORPWKSTN munkaállomást az új PowerShell-modul regisztrálásához.

A következő lépésben a PRIV és a CORP tartomány erdői közötti megbízható kapcsolatot hozhatja létre.

> [!div class="step-by-step"]
> [«3](step-3-prepare-pam-server.md) 
>  . lépés [5. lépés»](step-5-establish-trust-between-priv-corp-forests.md)
