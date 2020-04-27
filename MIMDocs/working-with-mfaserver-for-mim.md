---
title: A PAM-vagy SSPR-forgatókönyvek aktiválása az Azure Multi-Factor Authentication-kiszolgáló használatával | Microsoft Docs
description: Állítsa be az Azure Multi-Factor Authentication-kiszolgálót második biztonsági rétegként, amikor a felhasználók Privileged Access Management és az önkiszolgáló jelszó-visszaállítási szolgáltatásban aktiválják a szerepköröket.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 1dd87db01a5c1100c8206d82bedf96a8a5e702ad
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044310"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Az Azure Multi-Factor Authentication-kiszolgáló használata a PAM vagy a SSPR aktiválásához
A következő dokumentum azt ismerteti, hogyan állíthatja be az Azure MFA-kiszolgálót második biztonsági rétegként, amikor a felhasználók szerepköröket aktiválnak Privileged Access Management vagy az önkiszolgáló jelszó-visszaállítás során.

> [!IMPORTANT]
> Az Azure Multi-Factor Authentication szoftverfejlesztői készlet elavult felmondásának bejelentése miatt az Azure MFA SDK a meglévő ügyfelek számára a 2018. november 14-én esedékes kivonulási időpontig lesz támogatott. Az új ügyfelek és a jelenlegi ügyfelek többé nem tölthetik le az SDK-t a klasszikus Azure portálon keresztül. A letöltéshez el kell érnie az Azure ügyfélszolgálatát, hogy megkapja a generált MFA szolgáltatás hitelesítő adatait tartalmazó csomagot.

Az alábbi cikk azokat a konfigurációs frissítést és lépéseket ismerteti, amelyek lehetővé teszik az Azure MFA SDK-ból az Azure-Multi-Factor Authentication-kiszolgálóre való áttérést.

## <a name="prerequisites"></a>Előfeltételek

Az Azure Multi-Factor Authentication-kiszolgáló a (z)-vel való használatához a következőkre van szüksége:

- Internet-hozzáférés az Azure MFA szolgáltatással való kapcsolatfelvételhez az egyes SSPR-szolgáltatásokból vagy a PAM-t és a-t biztosító MFA-kiszolgálóról
- Azure-előfizetés
- A telepítés már használja az Azure MFA SDK-t
- Azure Active Directory Premium licenc vagy valamilyen alternatív, Azure MFA-licencet biztosító megoldás a jelölt felhasználóknál
- Telefonszám az összes jelölt felhasználó esetén
- A 4,5 vagy nagyobb a bejelentések [korábbi verzióinak](./reference/version-history.md) megtekintése

## <a name="azure-multi-factor-authentication-server-configuration"></a>Azure Multi-Factor Authentication-kiszolgáló konfiguráció 
> [!NOTE] 
> A konfigurációban telepítenie kell egy érvényes SSL-tanúsítványt az SDK-hoz. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>1. lépés: az Azure Multi-Factor Authentication-kiszolgáló letöltése az Azure Portalról 
Jelentkezzen be a [Azure Portalba](https://portal.azure.com/) , és töltse le az Azure MFA-kiszolgálót.
![mfaserver-mim_downloadmfa használata](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>2. lépés: aktiválási hitelesítő adatok előállítása
Használja az **aktiválási hitelesítő adatok létrehozása a használati** hivatkozás létrehozásához az aktiválási hitelesítő adatok létrehozásához. Miután létrehozott egy mentést a későbbi használatra.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>3. lépés: az Azure Multi-Factor Authentication-kiszolgáló telepítése
Miután letöltötte a kiszolgálót, [telepítse](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) azt.  Az aktiválási hitelesítő adatokra lesz szükség. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>4. lépés: az SDK-t futtató IIS-Webalkalmazás létrehozása
1. Nyissa meg ![az IIS-kezelőt a-mfaserver-for-mim_iis. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Hozzon létre egy "MFASDK" nevű új webhelyet, csatolja egy üres ![könyvtárhoz, amely a-mfaserver-for-mim_sdkweb használatával működik. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Nyissa meg Multi-Factor Authentication konzolt, és kattintson a ![Web Service SDK Working-with-mfaserver-for-mim_sdkinstall lehetőségre. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Ha a varázslók a konfiguráción keresztül kattintanak, válassza a "MFASDK" és az App Pool elemet.

> [!NOTE] 
> A varázslónak létre kell hoznia egy felügyeleti csoportot. További információ az Azure > > MFA Azure Multi-Factor Authentication-kiszolgáló dokumentációjában található.

5. Ezután importálnia kell a Multi-Factor Authentication-konzolon a "felhasználók" lehetőséget. Kattintson az "Importálás Active Directoryból" b elemre. Navigáljon a szolgáltatásfiók "contoso\mimservice" c néven. Kattintson az "Importálás" és a " ![Bezárás" Work-with-mfaserver-for-mim_importmimserviceaccount lehetőségre. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Szerkessze az Multi-Factor Authentication-konzolon ![a-mfaserver-for-mim_enableserviceaccount használatával történő engedélyezéshez szükséges a rendszerfiók-szolgáltatásfiókot. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Frissítse az IIS-hitelesítést a "MFASDK" webhelyről. Először le fogjuk tiltani a "Névtelen hitelesítés" beállítást, majd engedélyezzük a Windows-hitelesítés használatát " ![Working with-mfaserver-for-mim_iisconfig. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Utolsó lépés: adja hozzá a "PhoneFactor admins" ![Working with-mfaserver-for-mim_addservicetomfaadmin. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>A fakiszolgálói szolgáltatás konfigurálása az Azure Multi-Factor Authentication-kiszolgáló 

### <a name="step-1-patch-server-to-452020"></a>1. lépés: a 4.5.202.0-kiszolgáló javítása
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>2. lépés: a "C:\Program Files\Microsoft Forefront Identity Manager\2010\service mappában" mappában található MfaSettings. xml fájl biztonsági mentése és megnyitása

### <a name="step-3-update-the-following-lines"></a>3. lépés: a következő sorok frissítése
1. A következő konfigurációs bejegyzések sorainak eltávolítása/törlése <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD></CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. Frissítse vagy adja hozzá a következő sorokat a MfaSettings. xml fájlhoz. <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Indítsa újra a fakiszolgáló szolgáltatást és tesztelje az Azure Multi-Factor Authentication-kiszolgáló-t.

> [!NOTE] 
> Ha vissza szeretné állítani a beállítást, cserélje le a MfaSettings. xml fájlt a biztonságimásolat-fájlra a 2. lépésben


## <a name="see-also"></a>Lásd még

-    [Azure Multi-Factor Authentication-kiszolgáló – első lépések](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Mi az Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Az egyéni Multi-Factor Authentication API használata a PAM vagy a SSPR aktiválásához](Working-with-custommfaserver-for-mim.md)
- [A rendszerfrissítési csomag verziószáma](./reference/version-history.md)
