---
title: Az intelligens kártyák önkiszolgáló megújítása rendszergazdai hozzáférés nélkül a Microsoft Identity Managerben | Microsoft Docs
description: Megtudhatja, hogyan regisztrálhat intelligens kártyákat a számítógépükhöz rendszergazdai hozzáféréssel nem rendelkező felhasználók számára, hogy használhassák a Tanúsítványkezelőt.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c6e4e03524983944ee25343c24fa8247d770af27
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835660"
---
# <a name="enroll-smart-cards-for-non-administrators"></a>Intelligens kártyák regisztrálása nem rendszergazdák számára
Ha a felhasználó nem helyi rendszergazda, akkor alapesetben nem jogosult intelligens kártyát regisztrálni a saját számítógépén. A következő folyamat bemutatja, miként lehet áthidalni ezt a korlátozást.

## <a name="enabling-smart-card-renewal-for-non-admins-in-mim-2016-certificate-manager"></a>Intelligens kártya megújításának engedélyezése nem rendszergazda felhasználók számára az MIM 2016 Tanúsítványkezelőben

1.  **Csomagolja ki az appx-fájlt**

    Igényeljen aláíró tanúsítványt. Kövesse a következő leírást: [Sign Windows 8 applications using an internal PKI](https://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx) (Windows 8-alkalmazások aláírása belső nyilvános kulcsú infrastruktúrával). A „Sign the Application” (Az alkalmazás aláírása) lépésnél álljon meg. Nevezze el az exportált PFX-fájlt. Exportáljon egy CER-fájlba is, majd importálja az ügyfélre az új aláíró tanúsítvány CER-fájljával.

    Az appx-fájl kicsomagolásához futtassa a következő parancsot:

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **Módosítsa a konfigurációs fájlt**

    Nevezze át a CustomDataExample.xml fájlt custom.data névre. A Tanúsítványkezelő alkalmazás ezt a fájlnevet fogja keresni.

    A custom.data fájl szerkesztésével módosítsa a következőket:

    1.  A &lt;NonAdmin&gt; elemben a Value (Érték) attribútumnál adjon meg True (Igaz) értéket.

    2.  Mentse a fájlt, és zárja be a szerkesztőt.

    3.  Törölje az AppxSignature.p7x fájlt.

    4.  Szerkessze az AppxManifest.xml nevű fájlt.

    5.  Az &lt; Identity (identitás &gt; ) elemnél módosítsa a közzétevő attribútum értékét az aláíró tanúsítvány tárgyára, például: "CN = ABCD"

        A tulajdonos ugyanaz legyen, mint az alkalmazás aláírásához használt aláíró tanúsítvány tulajdonosa.

    6.  Mentse a fájlt, és zárja be a szerkesztőt.

3.  **Csomagolja újra és írja alá az alkalmazáscsomagot (appx-fájl)**

    Az appx-fájl csomagolásához és aláírásához futtassa a következő parancsot:

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    `signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  Duplikálja a profilsablont, és állítsa be a kezdő adminisztrációs kulcsot a MIM-kiszolgáló konfigurálásához:

    1.  Rendszergazdai jogosultsággal jelentkezzen be a Tanúsítványkezelő portálra.

    2.  Lépjen az **Administration (Felügyelet)** &gt; **Manage Profile Templates (Profilsablonok kezelése)** területre, és győződjön meg arról, hogy a létrehozott profil melletti négyzet be van jelölve, majd kattintson a Copy a selected profile template (Kijelölt profilsablon másolása) lehetőségre.

    3.  Írja be a profilsablon nevét, adja hozzá a „nonAdmin” elemet, majd kattintson az **OK** gombra.

    4.  Amikor a profilsablon általános beállításai megjelennek, görgessen teljesen le, majd a **Smart card Configuration** (Intelligens kártya konfigurációja) területen kattintson a **Change Settings** (Beállítások módosítása) lehetőségre.

    5.  Az **Admin key initial value (hex)** (Adminisztrációs kulcs kezdeti értéke [hex]) mezőbe írja be az alapértelmezett adminisztrációs kulcsot: „010203040506070801020304050607080102030405060708”.

    6.  Görgessen le, majd kattintson az **OK** gombra.

5.  **Hozzon létre egy nem rendszergazdai fiókot az ügyfélszámítógépen**

    A nem rendszergazda felhasználók nem hozhatják létre a virtuális intelligens kártyát a TPM-modulon, ezért létre kell hoznia azt számukra.

6.  **Hozzon létre egy virtuális intelligens kártyát a TpmVscMgr paranccsal**

    Továbbra is rendszergazdaként az alábbi parancs futtatásával hozzon létre egy üres virtuális intelligens kártyát a kívánt számítógépen. Ez az Intune-ban, az SCCM-ben vagy csoportházirendekkel is lehetséges.

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **Telepítse a Tanúsítványkezelő alkalmazást a nem rendszergazdai fiókban**

8.  **Indítsa el a Tanúsítványkezelőt, és regisztráljon egy virtuális intelligens kártyát**
