---
title: Self-Service jelszó-visszaállítás használata | Microsoft Docs
description: Ismerje meg, hogy milyen újdonságokat kínál a MIM 2016 önkiszolgáló jelszó-változtatási összetevője, például hogy miként képes együttműködni a többtényezős hitelesítéssel.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/11/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 16901978fd5b51a4a986b07e580d8162dd159575
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104130"
---
# <a name="self-service-password-reset-deployment-options"></a>Self-Service jelszó-visszaállítási telepítési lehetőségek

A [prémium szintű Azure Active Directory licenccel rendelkező](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing)új ügyfelek esetében javasoljuk, hogy az [Azure ad önkiszolgáló jelszó-visszaállítási funkciójának](/azure/active-directory/authentication/concept-sspr-howitworks) használatával adja meg a végfelhasználói élményt.  Az Azure AD önkiszolgáló jelszavának alaphelyzetbe állítása webes és Windows-integrált felhasználói élményt biztosít a felhasználók számára a saját jelszavának alaphelyzetbe állításához, és számos olyan funkciót támogat, mint a többhelyes hitelesítés, beleértve a másodlagos e-maileket és a Q&A kapukat.  Az Azure AD önkiszolgáló jelszó-visszaállítási szolgáltatásának telepítésekor a Azure AD Connect támogatja [az új jelszavak ad DSba való visszaírását](/azure/active-directory/authentication/concept-sspr-writeback), és a rendszerállapot- [módosítási értesítési szolgáltatás](deploying-mim-password-change-notification-service-on-domain-controller.md) segítségével továbbíthatja a jelszavakat más rendszerekre, például egy másik gyártó címtár-kiszolgálójához is.  A felügyeleti webszolgáltatások [jelszavas felügyeletre](infrastructure/mim2016-password-management.md) való üzembe helyezéséhez nincs szükség a rendszerbe állításhoz, illetve a felügyeleti webszolgáltatások önkiszolgáló jelszó-visszaállítási vagy regisztrációs portálok telepítéséhez.  Ehelyett az alábbi lépéseket követheti el:

- Először is, ha az Azure AD-től és a AD DStól eltérő könyvtárakba kell küldenie a jelszavakat, telepítsen központilag szinkronizált összekötőket a Active Directory tartományi szolgáltatások és a további megcélzott rendszerekhez, konfigurálja a Rendszerfelügyeleti [webszolgáltatást](deploying-mim-password-change-notification-service-on-domain-controller.md)a [jelszavas felügyelethez](infrastructure/mim2016-password-management.md) , és telepítse a
- Ezt követően, ha az Azure AD-től eltérő könyvtárakba kell küldenie a jelszavakat, konfigurálja Azure AD Connect az [új jelszavak AD DS való visszaírásához](/azure/active-directory/authentication/concept-sspr-writeback).
- Opcionálisan a [felhasználók előzetes regisztrációja](/azure/active-directory/authentication/howto-sspr-authenticationdata).
- Végül az [Azure ad önkiszolgáló jelszó-visszaállítási szolgáltatásának alaphelyzetbe állítása a végfelhasználók](/azure/active-directory/authentication/howto-sspr-deployment)számára.

Az önkiszolgáló jelszó-visszaállításhoz korábban üzembe helyezett Forefront Identity Manager (FIM) és a prémium szintű Azure Active Directory licenccel rendelkező meglévő ügyfelek esetében javasoljuk, hogy az Azure AD önkiszolgáló jelszó-visszaállításra való áttérést tervezi.  A végfelhasználókat az Azure AD önkiszolgáló jelszó-visszaállítási funkciójának átállításával anélkül változtathatja meg, hogy újra regisztrálja őket, ha [szinkronizálja vagy beállítja a PowerShell felhasználó másodlagos e-mail-címét vagy mobiltelefonszámát](/azure/active-directory/authentication/howto-sspr-authenticationdata). Miután a felhasználók regisztrálva lettek az Azure AD önkiszolgáló jelszó-visszaállításra, a rendszer leszerelheti a FIM jelszó-visszaállítási portált.

Az Azure AD önkiszolgáló jelszó-visszaállítást a felhasználók számára még nem központilag használó ügyfelek esetében a (z) az önkiszolgáló jelszó-visszaállítási portálokat is tartalmazza.  A FIM-hez képest a 2016 a következő változásokat tartalmazza:

- A Self-Service jelszó-visszaállítási portál és a Windows bejelentkezési képernyő lehetővé teszi a felhasználók számára, hogy a jelszavuk módosítása nélkül oldják fel a fiókjaikat.
- Új hitelesítési kaput, telefonos kaput adtak hozzá a webszolgáltatáshoz. Ez lehetővé teszi a felhasználó telefonhívás útján történő használatát a Microsoft Azure Multi-Factor Authentication (MFA) szolgáltatáson keresztül.

A 4.5.26.0 2016-es kiadás az Azure Multi-Factor Authentication szoftverfejlesztői készlet (Azure MFA SDK) letöltésére támaszkodik az ügyfélen.  Az SDK elavult, és az ügyfeleknek a SSPR-t az Azure MFA-kiszolgálóval vagy az Azure AD önkiszolgáló jelszó-visszaállítási szolgáltatásával kell áthelyezniük. Ez a [cikk](working-with-mfaserver-for-mim.md) azt ismerteti, hogyan frissítheti a központi telepítéshez szükséges, önkiszolgáló jelszó-visszaállítási portált és a PAM konfigurációját az Azure multi-Factor Authentication-kiszolgáló a multi-Factor Authentication használatával.

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>A Self-Service jelszó-visszaállítási portál üzembe helyezése az Azure MFA használatával Multi-Factor Authentication

A következő szakasz azt ismerteti, hogyan telepítheti a webszolgáltatások önkiszolgáló jelszó-visszaállítási portálját az Azure MFA használatával a többtényezős hitelesítéshez.  Ezek a lépések csak azoknál az ügyfeleknél szükségesek, akik nem használják az Azure AD önkiszolgáló jelszó-visszaállítást a felhasználók számára.

A Microsoft Azure Multi-Factor Authentication egy olyan hitelesítési szolgáltatás, amely a bejelentkezési kísérletek mobilalkalmazással, telefonhívással vagy szöveges üzenettel történő megerősítését kéri a felhasználóktól. A szolgáltatás a Microsoft Azure Active Directoryval, valamint felhőalapú és helyszíni nagyvállalati alkalmazásokkal is használható.

Az Azure MFA olyan kiegészítő hitelesítési módszert kínál, amely megerősítheti a már meglévő hitelesítési folyamatokat, például a MIM által az önkiszolgáló bejelentkezési segéddel végzett hitelesítést.

Az Azure MFA használata esetén a felhasználók annak érdekében hitelesítik magukat a rendszerrel, hogy igazolják az identitásukat, amikor megpróbálnak újra hozzáférést nyerni fiókjukhoz és erőforrásaikhoz. A hitelesítés történhet SMS-ben vagy telefonhívással.   Minél erősebb a hitelesítés, annál biztosabb, hogy a hozzáférést igénylő felhasználó ténylegesen az adott identitás tulajdonosa. A hitelesítést követően a felhasználó új jelszót választhat a régi helyett.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Az MFA szolgáltatással végzett önkiszolgáló fiókzárfeloldás és jelszóváltoztatás beállításának előfeltételei

Ez a szakasz azt feltételezi, hogy letöltötte és befejezte a Microsoft Identity Manager 2016 [-es webszolgáltatások szinkronizálását,](microsoft-identity-manager-deploy.md)a (z) a következő összetevőket és szolgáltatásokat is beleértve:

-   Egy Active Directory-kiszolgálóként beállított Windows Server 2008 R2-alapú kiszolgáló, AD tartományi szolgáltatások és tartományvezérlő szerepkörrel, kijelölt („vállalati”) tartománnyal.

-   A fiókzárolásra vonatkozóan definiált csoportházirend.

-   A MIM 2016 Synchronization Service (Sync) telepítve van, és az AD-tartományhoz csatlakoztatott kiszolgálón fut.

-   A MIM 2016 szolgáltatás és -portál (az önkiszolgáló jelszó-változtatási és -visszaállítási portállal együtt) telepítve van és fut egy kiszolgálón (ez lehet a Syncet futtató kiszolgáló is).

-   A MIM Sync be van állítva az identitások szinkronizálására az AD és a MIM között, beleértve a következőket:

    -   Az Active Directory-kezelőügynök (ADMA) be van állítva az AD DS-hez való csatlakozásra, és képes identitási adatokat importálni az Active Directoryba és exportálni onnan.

    -   A MIM-kezelőügynök (MIM MA) be van állítva a FIM szolgáltatás adatbázisához való kapcsolódásra, és képes identitási adatokat importálni a FIM-adatbázisba és exportálni onnan.

    -   A MIM-portál szinkronizálási szabályai úgy vannak beállítva, hogy lehetővé teszik a felhasználói adatok szinkronizálását, és támogatják a MIM szolgáltatás szinkronizálásra épülő tevékenységeit.

-   A MIM 2016 beépülő moduljai és bővítményei – többek között a Windows-bejelentkezésbe integrált önkiszolgáló jelszó-változtatási (SSPR-) ügyfél – telepítve vannak a kiszolgálón vagy egy különálló ügyfélszámítógépen.

Ehhez a forgatókönyvhöz szükséges, hogy a felhasználókhoz és az Azure MFA-hoz való előfizetéshez szükségesek legyenek a faalapú ügyféllicencek.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>A MIM és a többtényezős hitelesítés együttműködésének előkészítése
Állítsa be a MIM Sync szolgáltatást a jelszó-átállítási és fiókfeloldási funkció támogatásához. További információért lásd: [A FIM beépülő moduljainak és bővítményeinek telepítése](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [A FIM SSPR telepítése](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Az SSPR hitelesítési kapui](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx), illetve [Tesztlabor-útmutató az SSPR-hez](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx).


### <a name="update-the-configuration-file"></a>A konfigurációs fájl frissítése

> [!NOTE]
> Ez a szakasz az Azure MFA SDK által biztosított ZIP-fájlt használó korábbi útmutatáson alapul. Ehelyett használja az [Azure multi-Factor Authentication-kiszolgáló YSE](working-with-mfaserver-for-mim.md)című cikkben található útmutatást.


1. A MIM-et telepítő felhasználói fiókkal jelentkezzen be arra a számítógépre, ahol a MIM szolgáltatás telepítve van.

2. Hozzon létre egy új könyvtárat a MIM szolgáltatás telepítési mappájában, például: **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. A Windows Intézővel keresse meg a **\pf\certs** mappát az előző szakaszban letöltött ZIP-fájlban, és másolja a **cert_key.p12** fájlt az új mappába.

4.  Az SDK ZIP-fájlban a **\pf** mappában nyissa meg a **pf_auth.cs** fájlt.

5.  Keresse meg a következő három paramétert: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![Kép: A pf_auth.cs fájl kódja](media/MIM-SSPR-pFile.png)

6.  A **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service** mappában nyissa meg az **MfaSettings.xml** fájlt.

7.  A pf_aut.cs fájlból a `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` paraméterek értékeit másolja a megfelelő XML-elemekbe az MfaSettings.xml fájlban.

8.  Az SDK ZIP-fájlban a \pf\certs mappából bontsa ki a **cert_key.p12** fájlt, majd az MfaSettings.xml fájl `<CertFilePath>` XML-elemében adja meg a kicsomagolt fájl teljes elérési útját.

9. A `<username>` elemben adjon meg egy tetszőleges felhasználónevet.

10. A `<DefaultCountryCode>` elemben adja meg az alapértelmezett országkódját. Országkód nélkül telefonszám bejegyzése esetén a rendszer ezt az országkódot fogja használni. Amennyiben a felhasználó nemzetközi országkóddal rendelkezik, azt fel kell tüntetni a regisztrált telefonszámban.

11. Mentse az MfaSettings.xml fájlt, ezen a néven, ugyanarra a helyre.

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>A Telefonos kapu vagy az Egyszeri SMS-jelszó kapu beállítása

1.  Az Internet Explorerben lépjen a MIM-portálra, hitelesítse magát a MIM-rendszergazdai fiókkal, majd a bal oldali navigációs sávon kattintson a **Workflows** (Munkafolyamatok) elemre.

    ![Kép: Navigáció a MIM-portálon](media/MIM-SSPR-workflow.jpg)

2.  Jelölje be a **Password Reset AuthN Workflow** (Jelszó-változtatási hitelesítési munkafolyamat) négyzetet.

    ![Kép: Munkafolyamatok a MIM-portálon](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Kattintson az **Activities** (Tevékenységek) lapra, majd görgessen le az **Add Activity** (Tevékenység felvétele) elemhez.

4.  Válassza a **Phone Gate** (Telefonos kapu) vagy a **One-Time Password SMS Gate** (Egyszeri SMS-jelszavas kapu) elemet, majd kattintson a **Select** (Kiválasztás) lehetőségre, végül pedig az **OK** gombra.

Megjegyzés: Ha az Azure MFA-kiszolgálót vagy egy másik szolgáltatót használ, amely maga hozza létre az egyszeri jelszót, győződjön meg arról, hogy a fent konfigurált hossz mező megegyezik az MFA-szolgáltató által generált hosszsal.  Az Azure MFA-kiszolgáló hosszának 6-ának kell lennie.  Az Azure MFA-kiszolgáló saját szöveges üzenetet is létrehoz, így az SMS-üzenet figyelmen kívül lesz hagyva.

A szervezeti felhasználók ezután már regisztrálhatnak a jelszóváltoztatásra.  A folyamat során meg kell adniuk munkahelyi vagy mobiltelefonszámukat, hogy a rendszer tudja, hol keresheti őket (vagy küldhet nekik SMS-t).

#### <a name="register-users-for-password-reset"></a>Felhasználók regisztrálása jelszóváltoztatásra

1.  A felhasználók elindítanak egy webböngészőt, és megnyitják a felhasználói felület jelszavának visszaállítására szolgáló regisztrációs portált.  (A portál jellemzően Windows-hitelesítésre van konfigurálva.)  Identitása megerősítéséhez a portálon ismét meg kell adnia felhasználónevét és jelszavát.

    Be kell lépnie a jelszó-regisztrálási portálra, és hitelesítenie kell magát a felhasználónevével és jelszavával.

2.  A **Phone Number** (Telefonszám) vagy a **Mobile Phone** (Mobiltelefonszám) mezőben meg kell adnia az országkódot, majd egy szóközzel elválasztva a telefonszámot, majd a **Next** (Tovább) gombra kell kattintania.

    ![Kép: Telefonszám ellenőrzése a MIM-ben](media/MIM-SSPR-PhoneVerification.JPG)

    ![Kép: Mobiltelefonszám ellenőrzése a MIM-ben](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>Hogyan működik mindez a felhasználóknál?
Most, hogy minden be van állítva és fut, bizonyára tudni szeretné, hogy mit kell tennie a felhasználónak, ha például közvetlenül a szabadsága előtt megváltoztatta a jelszavát, majd visszatérve rájön, hogy teljesen elfelejtette az új jelszót.

A felhasználó kétféleképpen használhatja a jelszó-változtatási és fiókfeloldási funkciót: a Windows bejelentkezési képernyőjéről vagy az önkiszolgáló portálról.

Ha a MIM beépülő moduljait és bővítményeit olyan tartományhoz csatlakoztatott számítógépen telepíti, amely a szervezeti hálózaton keresztül a MIM szolgáltatáshoz csatlakozik, a felhasználó az asztali bejelentkezési környezetből megváltoztathatja az elfelejtett jelszavát.  Ezen a folyamaton a következő lépésekkel haladhat végig.

#### <a name="windows-desktop-login-integrated-password-reset"></a>A Windows bejelentkezési felülettel integrált jelszó-átállítási funkció

1.  Ha a felhasználó többször is helytelen jelszót ad meg, a bejelentkezési képernyőn megjelenik a **Nem tud bejelentkezni?** hivatkozás. .

    ![Kép: Bejelentkezési képernyő](media/MIM-SSPR-problemsloggingin.JPG)

    Ha erre a hivatkozásra kattint, megjelenik a MIM-jelszó átállítása képernyő, amelyen megváltoztathatja a jelszavát vagy feloldhatja a fiók zárolását.

    ![Kép: MIM-jelszó átállítása](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  A rendszer hitelesítésre kéri a felhasználót. Ha a többtényezős hitelesítés be lett állítva, a felhasználó telefonhívást kap.

3.  Ebben az esetben az Azure MFA szolgáltatás a háttérben felhívja azt a telefonszámot, amelyet a felhasználó megadott, amikor a szolgáltatásra regisztrált.

4.  Amikor a felhasználó felveszi a telefont, a rendszer arra kéri, hogy nyomja meg a kettőskereszt (#) gombot a készüléken. A felhasználónak ezután a **Next** (Tovább) gombra kell kattintania a portálon.

    Ha más kapuk is be lettek állítva, a rendszer a következő képernyőkön további információk megadására kéri a felhasználót.

    > [!NOTE]
    > Ha felhasználó türelmetlen, és még a # gomb megnyomása előtt a **Next** (Tovább) gombra kattint, a hitelesítés meghiúsul.

5.  Sikeres hitelesítést követően a felhasználó két lehetőség közül választhat: feloldhatja a fiók zárolását és megtarthatja a jelenlegi jelszavát, vagy új jelszót állíthat be.

6.  Ebben az esetben a felhasználónak kétszer meg kell adnia az új jelszót a jelszóváltoztatás érvényesítéséhez.

#### <a name="access-from-the-self-service-portal"></a>Elérés az önkiszolgáló portálról

1.  A felhasználó böngészőben a **jelszó-változtatási portálra** lép, megadja a felhasználónevét, majd a **Next** (Tovább) gombra kattint.

    Ha a többtényezős hitelesítés be lett állítva, a felhasználó telefonhívást kap. Ebben az esetben az Azure MFA szolgáltatás a háttérben felhívja azt a telefonszámot, amelyet a felhasználó megadott, amikor a szolgáltatásra regisztrált.

    Amikor a felhasználó felveszi a telefont, a rendszer arra kéri, hogy nyomja meg a kettőskereszt (#) gombot a készüléken. A felhasználónak ezután a **Next** (Tovább) gombra kell kattintania a portálon.

2.  Ha más kapuk is be lettek állítva, a rendszer a következő képernyőkön további információk megadására kéri a felhasználót.

    > [!NOTE]
    > Ha felhasználó türelmetlen, és még a # gomb megnyomása előtt a **Next** (Tovább) gombra kattint, a hitelesítés meghiúsul.

3.  A felhasználónak választania kell, hogy a jelszavát szeretné megváltoztatni vagy a fiók zárolását kívánja feloldani. Ha a fiók zárolásának feloldását választja, a fiók zárolása megszűnik.

    ![Kép: MIM Bejelentkezési segéd – Fiókzárolás feloldása](media/MIM-SSPR-accountUnlock.JPG)

4.  Sikeres hitelesítést követően a felhasználó két lehetőség közül választhat: megtarthatja a jelenlegi jelszavát, vagy új jelszót állíthat be.

5.  ![Kép: Fiók zárolásának sikeres feloldása a MIM-ben](media/MIM-SSPR-account-unlock.JPG)

6.  Ha a felhasználó úgy dönt, hogy megváltoztatja a jelszavát, a módosításhoz kétszer be kell írnia az új jelszót, majd a **Next** (Tovább) gombra kell kattintania.
