---
title: A PAM üzembe helyezése, 7. lépés – felhasználói hozzáférés | Microsoft Docs
description: Utolsó lépésként biztosítson ideiglenes rendszerjogosultságot egy felhasználónak, hogy tesztelhesse, sikeres volt-e a Privileged Access Management üzembe helyezése.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/17/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.openlocfilehash: 0d7e9a1111e41008a989ff2bfd52c9d79debf104
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104037"
---
# <a name="step-7--elevate-a-users-access"></a>7. lépés – Felhasználó jogosultságszintjének emelése

> [!div class="step-by-step"]
> [« 6. lépés](step-6-transition-group-to-pam.md)


Ebben a lépésben azt mutatjuk be, hogyan kérhet egy felhasználó hozzáférést egy szerepkörhöz a MIM-en keresztül.

## <a name="verify-that-jen-cannot-access-the-privileged-resource"></a>Győződjön meg róla, hogy Ilona nem tud hozzáférni a privilegizált erőforráshoz

Emelt szintű jogosultságok nélkül Ilona nem férhet hozzá a CORP erdőben található privilegizált erőforrásokhoz.

1. Jelentkezzen ki a CORPWKSTN munkaállomásról, hogy megszüntessen minden gyorsítótárazott, nyitott kapcsolatot.
2. Jelentkezzen be a CORPWKSTN munkaállomásra a *CONTOSO\Ilona* felhasználónévvel, és váltson át **asztali** megjelenítésre.
3. Nyisson meg egy DOS parancssort.
4. Írja be a `dir \\corpwkstn\corpfs` parancsot. Meg kell jelennie **A hozzáférés megtagadva** hibaüzenetnek.
5. Hagyja nyitva a parancssor ablakát.

## <a name="request-privileged-access-from-mim"></a>Kérjen emelt szintű hozzáférést a MIM-ből.

> [!NOTE]
> Javasoljuk, hogy a munkaállomás legyen Kiemelt munkaállomás (PAW).  További információ: az [eszközök biztonságossá tétele](/security/compass/privileged-access-devices).

1. PRIVWKSTN, bejelentkezés PRIV\priv.jen.
2. Kattintson a **Start** gombra, majd a **Futtatás** parancsra, és írja be a **PowerShell.exe**.
3. Írja be a következő parancsot.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Amikor a rendszer kéri, írja be a PRIV.Ilona fiók jelszavát. Megnyílik egy új parancssori ablak.
3. A PowerShell-ablak megjelenésekor írja be a következő parancsokat.

    > [!NOTE]
    > Miután futtatta ezeket a parancsokat, az alábbi lépések mindegyike időérzékennyé válik.

    ```PowerShell
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Ha ez befejeződött, zárja be a PowerShell ablakát.
5. A DOS parancsablakba írja be az alábbi parancsot.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Írja be a PRIV.Ilona fiók jelszavát. Megnyílik egy új parancssori ablak.

## <a name="validate-the-elevated-access"></a>Az emelt szintű hozzáférés ellenőrzése.
Az újonnan megnyílt ablakba írja be az alábbi parancsokat.

```cmd
whoami /groups
dir \\corpwkstn\corpfs
```

Ha a dir parancs végrehajtása nem sikerül, és megjelenik **A hozzáférés megtagadva** hibaüzenet, ellenőrizze újra a megbízhatósági kapcsolatot.

## <a name="activate-the-privileged-role"></a>Az emelt szintű szerepkör aktiválása

Aktiválja a szerepkört a PAM-mintaportálon keresztüli emelt szintű hozzáférés-igényléssel.

1. A CORPWKSTN munkaállomáson győződjön meg arról, hogy a CORP\Ilona felhasználónéven van bejelentkezve.
2. Egy DOS parancsablakba írja be az alábbi parancsot.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Amikor a rendszer kéri, írja be a PRIV.Ilona fiók jelszavát. Megnyílik egy új böngészőablak.
4. Navigáljon a `http://pamsrv.priv.contoso.local:8090` oldalra, és ellenőrizze, hogy látható-e egy weblap a mintául szolgáló portálról.
5. Az Internet Explorerben válassza az **eszközök**  >  **Internetbeállítások lehetőséget** , majd kattintson a **Biztonság** fülre.
6. Kattintson a **helyi intranet zóna**  >  **helyek**  >  **speciális** lehetőségre, majd adja hozzá a webhelyet a zónához.
7. Zárja be az **Internetbeállítások** párbeszédpanelt.
8. A bal oldali lapon kattintson az **Aktiválás** elemre. Válassza ki a **PAM-szerepkör** lehetőséget, majd kattintson az **Aktiválás** elemre.

> [!Note]
> Ebben a környezetben megismerkedhet a [Privileged Access Management REST API-referencia](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference) témakörben ismertetett PAM REST API-t használó alkalmazások fejlesztésével.

## <a name="summary"></a>Összefoglalás

Az útmutató lépéseinek végrehajtását követően egy olyan Privileged Access Management-forgatókönyvet ismerhet meg, amelyben a felhasználók emelt szintű jogosultságai csak korlátozott ideig érvényesek, és a védett erőforrásokhoz egy elkülönített, emelt jogosultsági szintű fiókon keresztül férhetnek hozzá. Amint a jogosultságszint-emelés időtartama lejár, a rendszerjogosultságú fiók már nem tud hozzáférni a védett erőforrásokhoz. Annak eldöntése, hogy mely biztonsági csoportok kaphatnak kiemelt szerepköröket, a PAM-rendszergazda feladata. A hozzáférési jogoknak a Privileged Access Management-rendszerbe való áttelepítését követően a korábban az eredeti felhasználói fiókok számára biztosított hozzáférések csak akkor lesznek érvényesek, ha a felhasználó egy speciális, rendszerjogosultságú fiókkal bejelentkezik, és újra megkéri ezeket az engedélyeket. Ennek eredményeképpen a magas jogosultsági szintű csoportokhoz tartozó csoporttagságok csak korlátozott ideig használhatók eredményesen.

> [!div class="step-by-step"]
> [« 6. lépés](step-6-transition-group-to-pam.md)
