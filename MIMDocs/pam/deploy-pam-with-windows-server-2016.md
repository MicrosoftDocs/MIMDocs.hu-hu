---
title: A MIM Privileged Access Management telepítése a Windows Server 2016-tal | Microsoft Docs
description: A cikk tájékoztatást nyújt arról, hogy miképpen telepíthető a Privileged Access Management a Windows Server 2016-tal.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: c02904d7acb5c56e8b1e7f7a267b8d54c0a58d7a
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010557"
---
# <a name="deploy-mim-pam-with-windows-server-2016"></a>A MIM PAM telepítése a Windows Server 2016-tal


Ez a forgatókönyv lehetővé teszi, hogy a Microsoft Windows Server 2016 vagy újabb funkciói a "PRIV" erdőhöz kapcsolódó PAM-forgatókönyv esetén a (z) 2016 SP2-t használják.  A konfigurálás után a felhasználók Kerberos-jegye időkorlátossá válik a szerepkör-aktiválásban megmaradt időre.

## <a name="preparation"></a>Előkészítés

A tesztkörnyezetben legalább két virtuális gép szükséges:

-   A virtuális gép a Windows Server 2016-es vagy újabb verzióját futtató PRIV tartományvezérlőt üzemelteti.

-   A virtuális gép a Windows Server 2016-es vagy újabb verzióját (ajánlott) vagy a Windows Server 2012 R2 rendszert futtató, a fakiszolgáló szolgáltatást futtatja.

> [!NOTE]
> Ha még nincs „CORP” tartomány a tesztkörnyezetben, szükséges egy további tartományvezérlő ahhoz a tartományhoz. A „CORP” tartományvezérlő Windows Server 2016 és Windows Server 2012 R2 rendszerű is lehet.


Végezze el a telepítést az [Útmutató az első lépésekhez](privileged-identity-management-for-active-directory-domain-services.md) szerint, **az alábbi eltérésekkel**:

- Ha új CORP tartományt hoz létre, akkor az [1. lépés – A CORP tartományvezérlő előkészítése](step-1-prepare-corp-domain.md) című rész útmutatásának végrehajtása közben választhatja, hogy a CORP tartomány a Windows Server 2016 működési szintjén legyen. **Ha ezt a beállítást választja, végezze el a következő módosításokat**:

  - Ha a Windows Server 2016 adathordozójáról futtatja a telepítőt, akkor a telepítési lehetőség neve Windows Server 2016 (Server with Desktop Experience) (Windows Server 2016 (Kiszolgáló asztali felülettel)) lesz.

  - A CORP erdő és tartomány működési szintjét úgy állíthatja Windows Server 2016-ra, hogy a következők szerint a 7 értéket adja meg a tartomány és az erdő verziójaként az Install-ADDSForest parancs argumentumaként:
    ```
    Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
    ```
  - Az „Új felhasználók és csoportok létrehozása” lépésben az utolsó parancs (New-ADGroup -name 'CONTOSO\$\$\$' …) **nem szükséges, ha a CORP és a PRIV tartományvezérlője is Windows Server 2016 tartományműködési szintű**.

  - A „Naplózás konfigurálása” (8. tétel) és a „Beállításjegyzék-beállítások konfigurálása” (10. tétel) című részben ismertetett módosítások **ajánlottak, de nem szükségesek**, ha a CORP és a PRIV tartományvezérlője is Windows Server 2016 tartományműködési szintű.

- Ha a Windows Server 2012 R2-t használja a CORPDC tartományvezérlő operációs rendszereként, akkor telepítenie kell a 2919442-es és a 2919355-es gyorsjavítást, [valamint a 3155495-es frissítést](https://support.microsoft.com/kb/3156418) a CORPDC számítógépre.

- Kövesse a [2. lépés – A PRIV tartományvezérlő előkészítése](step-2-prepare-priv-domain-controller.md) című szakasz útmutatását, de a következő módosításokkal:

  -   Végezze a telepítést a Windows Server 2016 adathordozójáról. A telepítési lehetőség neve Windows Server 2016 (Server with Desktop Experience) (Windows Server 2016 (Kiszolgáló asztali felülettel)) lesz.

  -   A „Szerepkörök hozzáadása” (4. tétel) szakasz végrehajtása közben **a tartománynak és az erdőnek a PowerShell-parancsok negyedik sorában szereplő verziószámát 7-re kell állítania**, hogy elérhetők legyenek a Windows Server AD később ismertetett funkciói.

      ```
      Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
      ```  

  -   A naplózási és a bejelentkezési jogok konfigurálásakor vegye figyelembe, hogy a Csoportházirend kezelése program a Windows Felügyeleti eszközök mappában található.

  -   A beállításjegyzékben a SID-előzmények áttelepítéséhez (8. tétel) szükséges beállításokat **nem kell konfigurálni, ha a PRIV tartomány Windows Server 2016 tartományműködési szintű**.

  -   A delegálás konfigurálása után, de még a kiszolgáló újraindítása előtt engedélyezze a Windows Server 2016 Active Directory nyújtotta Privileged Access Management szolgáltatásokat. Ehhez nyisson meg rendszergazdaként egy PowerShell-ablakot, és írja be a következő parancsokat.

  ```
  $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
  Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
  ```

  - A delegálás konfigurálása után, de még a kiszolgáló újraindítása előtt jogosítsa fel a rendszergazdákat és a MIM szolgáltatásfiókját árnyéktagok létrehozására és frissítésére.

    a. Nyisson meg egy PowerShell-ablakot, és írja be a ADSIEdit.

    b. Kattintson a Műveletek menü „Csatlakozás” elemére. A Kapcsolódási pont beállításnál módosítsa a névhasználati környezetet az „Alapértelmezett névhasználati környezet” beállításról a „Konfiguráció” értékre, majd kattintson az OK gombra.

    c. A csatlakozás után az ablak bal oldalán, az „ADSI Edit” felirat alatt bontsa ki a Konfiguráció csomópontot, hogy láthatóvá váljon a „CN=Configuration,DC=priv,....”  csomópont. Bontsa ki a CN=Configuration csomópontot, majd a CN=Services csomópontot.

    d. Kattintson a jobb gombbal „CN=Shadow Principal Configuration” csomópontra, majd a Tulajdonságok parancsra. Amikor megjelenik a tulajdonságok párbeszédpanelje, váltson a Biztonság lapra.

    e. Kattintson az Add (Hozzáadás) parancsra. Adja meg a „MIMService” fiókokat, valamint minden olyan további rendszergazdát, akik később használni fogják a New-PAMGroup parancsot további PAM-csoportok létrehozására. Mindegyik felhasználónál vegye fel a megengedettek listájába az „Írás”, „Az összes gyermekobjektum létrehozása” és „Az összes gyermekobjektum törlése” engedélyt. Vegye fel az engedélyeket.

    f. Váltson a speciális biztonsági beállításokra. Kattintson a Szerkesztés gombra a MIMService elérését engedélyező soron. Módosítsa az „Érvényes erre” beállítást az „Ez az objektum és a gyermekobjektumok” értékre. Frissítse az engedély beállítását, és zárja be a Biztonság párbeszédpanelt.

    : Zárja be az ADSI Edit eszközt.

  - A delegálás konfigurálása után, de még a kiszolgáló újraindítása előtt jogosítsa fel a MIM rendszergazdáit hitelesítési házirend létrehozására és frissítésére.

    a.  Nyisson meg egy rendszergazda jogú **parancssort**, és írja be a következő parancsokat (mind a négy sorban cserélje le a „mimadmin” elemet saját MIM-rendszergazdafiókjának nevére):
    ```
    dsacls "CN=AuthN Policies,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

    dsacls "CN=AuthN Policies,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:CCDC;msDS-AuthNPolicy

    dsacls "CN=AuthN Silos,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

    dsacls "CN=AuthN Silos,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


- Kövesse a [3. lépés – PAM-kiszolgáló előkészítése](step-3-prepare-pam-server.md) című szakasz útmutatását a következő eltérésekkel.

  -   Ha a Windows Server 2016-ra telepít, ne feledje, hogy az „ApplicationServer” szerepkör nem érhető el.

  -   Ha a Windows Server 2016-ra telepít, **nem lehet telepíteni a SharePoint 2013-at**.

- Kövesse a [4. lépés – MIM-összetevők telepítése PAM-kiszolgálóra és -munkaállomásra](step-4-install-mim-components-on-pam-server.md) című szakasz útmutatását a következő eltérésekkel.

  -   A MIM szolgáltatást és a PAM összetevőt telepítő felhasználónak **írási joggal kell rendelkeznie a PRIV tartományhoz az Active Directoryban**, mert a MIM telepítése létrehoz egy új, „PAM objects” nevű szervezeti egységet.

  -   Ha nincs telepítve a SharePoint, ne telepítse a MIM-portált.

- Kövesse az [5. lépés – Megbízhatósági kapcsolat létrehozása](step-5-establish-trust-between-priv-corp-forests.md) című szakasz útmutatását a következő eltérésekkel:

  - Egyirányú megbízhatósági kapcsolat létrehozásakor csak az első két PowerShell-parancsot (Get-hitelesítőadat és New-PAMTrust) hajtsa végre, ne **hajtsa végre az New-PAMDomainConfiguration parancsot**.

  - A bizalmi kapcsolat kialakítása után jelentkezzen be a PRIVDC tartományvezérlőre PRIV\\Rendszergazda felhasználóként, indítsa el a PowerShellt, és írja be a következő parancsokat:
    ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
    /usero:contoso\administrator /passwordo:Pass@word1

    netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
    /usero:contoso\administrator /passwordo:Pass@word1  

    netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
    /usero:contoso\administrator /passwordo:Pass@word1
    ```

- Az 5. tételt (a megbízhatóság ellenőrzését) **nem szükséges elvégezni, ha a CORP és a PRIV tartomány is Windows Server 2016 tartományműködési szintű**.

## <a name="more-information"></a>További információ

- [Privileged Access Management az Active Directory Domain Serviceshez](privileged-identity-management-for-active-directory-domain-services.md)
- [A MIM-környezet konfigurálása a Privileged Access Management szolgáltatáshoz](configuring-mim-environment-for-pam.md)
- [A PAM konfigurálása szkriptek használatával](sp1-pam-configure-using-scripts.md)
