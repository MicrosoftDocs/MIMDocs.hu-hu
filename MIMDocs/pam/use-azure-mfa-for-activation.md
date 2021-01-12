---
title: Az Azure MFA használata a PAM aktiválásához | Microsoft Docs
description: Állítsa be az Azure MFA-t második biztonsági szintként, ha a felhasználók szerepköröket aktiválnak a Privileged Access Managementben.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: daveba
ms.date: 07/06/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.openlocfilehash: 1c26147dc1e192804011ee104989a508acb25974
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104054"
---
# <a name="using-azure-mfa-for-activation-in-mim-pam"></a>Az Azure MFA használata az aktiváláshoz a webszolgáltatási PAM-ban
> [!IMPORTANT]
> A webalkalmazás korábbi verziói az Azure Multi-Factor Authentication (MFA) szoftverfejlesztői készletet (SDK) használták az Azure MFA integrálásához.  Az Azure MFA SDK elavulttá váltása miatt a meglévő és az új szakterületi ügyfelek nem tudják többé letölteni az Azure MFA SDK-t. További információ az Azure MFA-kiszolgáló Azure MFA SDK-val való használatáról: az [Azure MFA-kiszolgáló használata a PAM-ban vagy a SSPR](../working-with-mfaserver-for-mim.md).




A PAM-szerepkörök konfigurálásakor kiválaszthatja, hogyan szeretné engedélyekkel felruházni azokat a felhasználókat, akik a szerepkör aktiválását kérik. A PAM-engedélyezés a következő választási lehetőségeket nyújtja:

- Szerepkör tulajdonosának jóváhagyása
- [Azure Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)

Ha egyik ellenőrzési mód sincs engedélyezve, a jelölt felhasználók szerepköre automatikusan aktiválódik.

A Microsoft Azure Multi-Factor Authentication (MFA) olyan hitelesítési szolgáltatás, amely a bejelentkezési kísérletek mobilalkalmazással, telefonhívással vagy SMS-sel történő megerősítését kéri a felhasználóktól. A szolgáltatás a Microsoft Azure Active Directoryval, valamint felhőalapú és helyszíni nagyvállalati alkalmazásokkal is használható. A PAM-forgatókönyv esetén az Azure MFA további hitelesítési mechanizmust biztosít. Az Azure MFA használható hitelesítésre, függetlenül attól, hogy a felhasználók hogyan hitelesítve vannak a Windows PRIV-tartományba.

> [!NOTE]
> A felügyeleti csomag által biztosított megerősített környezettel rendelkező PAM-megközelítés arra szolgál, hogy olyan elkülönített környezetekben legyen használatban, ahol az Internet-hozzáférés nem érhető el, ahol ez a konfiguráció szükséges a szabályozáshoz, vagy nagy hatással van az elkülönített környezetekre, például az offline kutatási laboratóriumokra és a leválasztott operatív technológiákra, valamint a felügyeleti és adatgyűjtési környezetekre.  Mivel az Azure MFA egy Internet-szolgáltatás, ez az útmutató kizárólag a meglévő fakiszolgálói PAM-ügyfelek vagy olyan környezetekben nyújt segítséget, ahol ez a konfiguráció szükséges a szabályozáshoz. Ha a Active Directory egy internetkapcsolattal rendelkező környezet részét képezi, további információért lásd: a [privilegizált hozzáférés biztonságossá tétele](/security/compass/overview) .

## <a name="prerequisites"></a>Előfeltételek

Ahhoz, hogy az Azure MFA-t a (z) a következővel használja:

- Internet-hozzáférés minden, a PAM megoldást használó MIM szolgáltatás esetén az Azure MFA szolgáltatás eléréséhez
- Azure-előfizetés
- Azure MFA
- prémium szintű Azure Active Directory-licencek a tagjelölt felhasználók számára
- Telefonszám az összes jelölt felhasználó esetén

## <a name="downloading-the-azure-mfa-service-credentials"></a>Az Azure MFA szolgáltatás hitelesítő adatainak letöltése

Korábban az Azure MFA-hoz való kapcsolatfelvételhez egy olyan ZIP-fájlt kell letöltenie, amely tartalmazza a többtényezős hitelesítéshez szükséges hitelesítési anyagokat. Mivel azonban az Azure MFA SDK már nem érhető el, lásd: az Azure MFA [-kiszolgáló használata a PAM-ban vagy a SSPR](../working-with-mfaserver-for-mim.md) az Azure MFA-kiszolgáló használatával kapcsolatos információkhoz.


## <a name="configuring-the-mim-service-for-azure-mfa"></a>A MIM szolgáltatás konfigurálása az Azure MFA használatára

1.  Rendszergazdaként vagy a MIM-et telepítő felhasználói fiókkal jelentkezzen be arra a számítógépre, amelyre a MIM szolgáltatás telepítve van.

2.  Hozzon létre egy új mappát abban a könyvtárban, ahová a MIM szolgáltatás telepítve van, például: ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  A Windows Intéző használatával navigáljon az ```pf\certs``` előző szakaszban letöltött zip-fájl mappájába. Másolja a fájlt ```cert\_key.p12``` az új könyvtárba.

4.  A Windows Intéző segítségével navigáljon a ```pf``` zip mappájába, és nyissa meg a fájlt ```pf\_auth.cs``` egy szövegszerkesztőben, például a Jegyzettömbben.

5. Keresse meg a következő három paramétert: ```LICENSE\_KEY``` , ```GROUP\_KEY``` , ```CERT\_PASSWORD``` .

![Értékek másolása a pf\_auth.cs fájlból – képernyőkép](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Nyissa meg a Jegyzettömbben az **MfaSettings.xml** fájlt, amely a ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service``` mappában található.

7. A pf\_auth.cs fájlból másolja a LICENSE\_KEY, GROUP\_KEY és a CERT\_PASSWORD paraméter értékét az MfaSettings.xml fájl megfelelő XML-elemeibe.

8. Az **<CertFilePath>** XML-elemben adja meg a \_ korábban kibontott CERT Key. P12 fájl teljes elérési útját.

9. A **<username>** elemben adjon meg egy tetszőleges felhasználónevet.

10. A **<DefaultCountryCode>** elemben adja meg az országkódot a felhasználók tárcsázásához, például: 1 a Egyesült Államok és Kanada számára. Erre a számra abban az esetben van szükség, ha a felhasználók országkód nélkül megadott telefonszámmal vannak regisztrálva. Ha a felhasználó telefonszáma a vállalat számára beállított országhívószámtól különböző előhívószámmal hívható, az adott országkódnak szerepelnie kell a regisztrált telefonszámban.

11. Mentse és írja felül az **MfaSettings.xml** fájlt a MIM szolgáltatás mappában (```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service```).

> [!NOTE]
> A folyamat végén győződjön meg róla, hogy az **MfaSettings.xml** fájl, illetve annak minden példánya, valamint a ZIP-fájl nyilvánosan nem olvasható.

## <a name="configure-pam-users-for-azure-mfa"></a>PAM-felhasználók konfigurálása az Azure MFA számára

Azure MFA-alapú hitelesítést igénylő szerepkört aktiváló felhasználó esetén a felhasználó telefonszámát tárolni kell a MIM szolgáltatásban. Ez az attribútum kétféleképpen állítható be.

Első lehetőség: a `New-PAMUser` parancs egy telefonszám-attribútumot másol a felhasználó CORP tartománybeli címtárbejegyzéséből a MIM szolgáltatás adatbázisába. Ezt a műveletet egyszer kell elvégezni.

Második lehetőség: a `Set-PAMUser` parancs frissíti a telefonszám-attribútumot a MIM szolgáltatás adatbázisában. Az alábbi parancs például egy létező PAM-felhasználó telefonszámát cseréli le a MIM szolgáltatásban. A szám címtárbeli bejegyzése nem változik.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

## <a name="configure-pam-roles-for-azure-mfa"></a>PAM-szerepkörök konfigurálása az Azure MFA számára

Ha már az adott PAM-szerepkörre jelölt összes felhasználó telefonszáma tárolva van a MIM szolgáltatás adatbázisában, a szerepkör konfigurálható az Azure MFA hitelesítés megkövetelésére. Ez a `New-PAMRole` vagy a `Set-PAMRole` paranccsal tehető meg. Példa:

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Egy adott szerepkör esetében az „-MFAEnabled 0” paraméternek a `Set-PAMRole` parancsban való megadásával tiltható le az Azure MFA.

## <a name="troubleshooting"></a>Hibaelhárítás

A következő események a Privileged Access Management eseménynaplójában jelenhetnek meg:

| ID (Azonosító)  | Súlyosság | Létrehozója | Description |
|-----|----------|--------------|-------------|
| 101 | Hiba       | MIM szolgáltatás            | A felhasználó nem végezte el az Azure MFA hitelesítést (például nem vette fel a telefont) |
| 103 | Tájékoztatás | MIM szolgáltatás            | A felhasználó aktiválás közben hajtotta végre az Azure MFA hitelesítést                       |
| 825 | Figyelmeztetés     | A PAM figyelőszolgáltatása | A telefonszám megváltozott                                |

## <a name="next-steps"></a>Következő lépések

- [Mi az Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
