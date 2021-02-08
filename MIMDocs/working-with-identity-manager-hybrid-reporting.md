---
title: Hibrid jelentéskészítés használata az Azure-ban a Identity Manager 2016 használatával | Microsoft Docs
description: Itt tájékozódhat arról, hogyan kombinálhatja a helyszíni és a felhőalapú adatokat hibrid jelentések formájában az Azure-ban, és hogyan kezelheti és jelenítheti meg ezeket a jelentéseket.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 2/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: 0b0a3adcf1651f6a431eec02488e0ab75347b3ca
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835574"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Hibrid jelentéskészítés használata az Identity Managerben

Ez a cikk azt ismerteti, hogyan kombinálhatja a helyszíni és a Felhőbeli adattartalmakat az Azure hibrid jelentéseivel, valamint hogyan kezelheti és tekintheti meg ezeket a jelentéseket.

## <a name="available-hybrid-reports"></a>Rendelkezésre álló hibrid jelentések
A Azure Active Directory (Azure AD)-ben elérhető első három Microsoft Identity Manager jelentés a következő:

- **Jelszó-visszaállítási tevékenység**: megjeleníti az egyes példányokat, amikor egy felhasználó az önkiszolgáló jelszó-visszaállítás (SSPR) használatával végrehajtotta a jelszó-visszaállítást, és megadja a hitelesítéshez használt kapukat vagy metódusokat.

- **Jelszó-visszaállítási regisztráció**: minden alkalommal megjelenik, amikor a felhasználó regisztrálja magát a SSPR és a hitelesítéshez használt metódusokat. A metódusok például mobiltelefon-telefonszámok vagy kérdések és válaszok lehetnek.
   > [!NOTE]
   > A *jelszó-visszaállítási regisztrációs* jelentések esetében nem történik KÜLÖNBSÉGTÉTEL az SMS-kapu és az MFA-kapu között. Mindkettő mobiltelefonos módszernek számít.

- **Önkiszolgáló csoportok tevékenység**: a felhasználók által a csoport és csoport létrehozásakor felvenni vagy törölni kívánt kísérleteket jeleníti meg.

    ![Kép: Azure hibrid jelentések – jelszó-átállítási tevékenységek](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * A jelentések jelenleg legfeljebb egy hónapos tevékenységben jelennek meg.
> * Az előző hibrid jelentéskészítő ügynököt el kell távolítani.
> * A hibrid jelentések eltávolításához távolítsa el a MIMreportingAgent.msi ügynököt.

## <a name="prerequisites"></a>Előfeltételek

* Identity Manager 2016 SP1 Identity Manager szolgáltatás, ajánlott Build [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* Egy prémium szintű Azure AD bérlő, amely rendelkezik licenccel rendelkező rendszergazdával a címtárban.

* Kimenő internetkapcsolat az Identity Manager-kiszolgálóról az Azure-ba.

## <a name="requirements"></a>Követelmények
Az Identity Manager hibrid jelentéskészítés használatának követelményeit az alábbi táblázat tartalmazza:


|                                         Követelmény                                         |                                                                                                                                                                                                                                                                                    Leírás                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        A hibrid jelentéskészítés egy prémium szintű Azure AD funkció, és prémium szintű Azure AD szükséges. </br>További információ: [Bevezetés a prémium szintű Azure ad](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium)használatába. </br>A [prémium szintű Azure ad 30 napos ingyenes próbaverziójának](https://azure.microsoft.com/trial/get-started-active-directory/)beszerzése.                                                                                                         |
|                     Az Azure AD globális rendszergazdájának kell lennie                     |                                                                   Alapértelmezés szerint csak a globális rendszergazdák telepíthetik és konfigurálhatják az ügynököket az első lépésekhez, a portál eléréséhez és az Azure-on belüli műveletek végrehajtásához. </br>**Fontos**: az ügynökök telepítésekor használt fióknak munkahelyi vagy iskolai fióknak kell lennie. Nem lehet Microsoft-fiók. További információ: [regisztráció az Azure-ba szervezetként](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| Az Identity Manager hibrid ügynöke telepítve van az egyes megcélozott Identity Manager Service-kiszolgálókon |                                                                                                                                                                                                       Az adatfogadáshoz és a figyelési és elemzési képességek biztosításához a hibrid jelentéskészítés megköveteli, hogy az ügynökök telepítve és konfigurálva legyenek a célként megadott kiszolgálókon.  </br>                                                                                                                                                                                                       |
|                    Kimenő kapcsolódás az Azure szolgáltatásvégpontokra                     | A telepítés alatt és futásidőben az ügynöknek kapcsolódnia kell az Azure-szolgáltatásvégpontokhoz. Ha a tűzfal blokkolja a kimenő kapcsolatot, győződjön meg arról, hogy az alábbi végpontok fel vannak véve az engedélyezett listára:<ul><li>\*.blob.core.windows.net </li><li>\*. servicebus.windows.net – port: 5671 </li><li>\*. adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Kimenő kapcsolat IP-címek alapján                         |                                                                                                                                                                                                                      A tűzfalakon alapuló IP-címek szűréséhez tekintse meg az [Azure IP-tartományokat](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 A kimenő forgalom SSL-ellenőrzése szűrve vagy Letiltva                 |                                                                                                                                                                                                               Előfordulhat, hogy az ügynök regisztrációs lépése vagy az adatfeltöltés művelete meghiúsul, ha a hálózati rétegben a kimenő forgalom SSL-ellenőrzése vagy leállítása történik.                                                                                                                                                                                                                |
|                      Az ügynököt futtató kiszolgálón található tűzfal portjai                       |                                                                                                                                                                                                          Az Azure szolgáltatási végpontokkal folytatott kommunikációhoz az ügynöknek a következő tűzfal-portok megnyitására van szüksége:<ul><li>443-as TCP-port</li><li>5671-es TCP-port</li></ul>                                                                                                                                                                                                          |
|          Bizonyos webhelyek engedélyezése, ha az Internet Explorer fokozott biztonsága engedélyezve van           |                                                                                Ha az Internet Explorer fokozott biztonsága engedélyezve van, az alábbi webhelyeket engedélyezni kell azon a kiszolgálón, amelyen az ügynök telepítve van:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>A szervezet Azure Active Directory által megbízhatónak tartott összevonási kiszolgálója (például: <https://sts.contoso.com> ).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Az Identity Manager Reporting Agent telepítése az Azure AD-ben
A jelentéskészítő ügynök telepítése után az Identity Manager-tevékenységből származó adatok az Identity Managerből a Windows-eseménynaplóba lesznek exportálva. Az Identity Manager jelentéskészítő ügynök feldolgozza az eseményeket, majd feltölti őket az Azure-ba. Az Azure elemzi, visszafejti és szűri az eseményeket a szükséges jelentésekhez.

1.  Az Identity Manager 2016 telepítése.

2.  Töltse le az Identity Manager Reporting Agent ügynököt, majd tegye a következőket:

    a. Jelentkezzen be az Azure AD felügyeleti portálra, majd válassza a **Active Directory** lehetőséget.

    b. Kattintson duplán arra a könyvtárra, amelyhez Ön globális rendszergazda, és rendelkeznie kell prémium szintű Azure AD előfizetéssel.

    c. Válassza a **konfiguráció**, majd a jelentéskészítő ügynök letöltése lehetőséget.

3.  A jelentéskészítő ügynök telepítése a következő módon:

    a.  Töltse le az Identity Manager szolgáltatási kiszolgálójának [MIMHReportingAgentSetup.exe-fájlját](https://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) .

    b.  Futtassa a `MIMHReportingAgentSetup.exe` parancsot. 

    c.  Futtassa az ügynök telepítőjét.

    d.  Győződjön meg arról, hogy az Identity Manager jelentéskészítő ügynök szolgáltatás fut.

    e.  Indítsa újra a Identity Manager szolgáltatást.

4.  Ellenőrizze, hogy az Identity Manager jelentéskészítő ügynök működik-e az Azure-ban.

    Jelentési adatok létrehozásához használja az Identity Manager önkiszolgáló jelszó-visszaállítási portált a felhasználó jelszavának alaphelyzetbe állításához. Győződjön meg arról, hogy a jelszó alaphelyzetbe állítása sikeresen befejeződött, majd ellenőrizze, hogy az Azure AD felügyeleti portálon megjelenjenek-e az adatcsere.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Hibrid jelentések megtekintése a Azure Portal

1.  Jelentkezzen be a [Azure Portalba](https://portal.azure.com/) a bérlőhöz tartozó globális rendszergazdai fiókkal.

2.  Válassza a **Azure Active Directory** lehetőséget.

3.  Az előfizetéshez elérhető címtárak listájában válassza ki a bérlői könyvtárat.

4.  Válassza a **naplók** lehetőséget.

5.  Győződjön meg arról, hogy a **Kategória** legördülő listában be van jelölve a **webkiszolgáló szolgáltatás** .

> [!IMPORTANT]
> Eltarthat egy ideig, amíg az Identity Manager naplózási adatai megjelennek a Azure Portal.

## <a name="stop-creating-hybrid-reports"></a>A hibrid jelentések létrehozásának leállítása
Ha le szeretné állítani a jelentéskészítési naplózási adatok feltöltését az Identity Managerből az Azure AD-be, távolítsa el a hibrid jelentéskészítő ügynököt. A Windows programok telepítése és törlése eszköz használatával távolítsa el az Identity Manager hibrid jelentéskészítést.

## <a name="windows-events-used-for-hybrid-reporting"></a>A hibrid jelentéskészítéshez használt Windows-események
Az Identity Manager által generált eseményeket a Windows eseménynaplója tárolja. Az eseményeket a **eseménynaplóban** tekintheti meg az **alkalmazás és szolgáltatások naplók**  >  **Identity Manager-kérelmek naplójának** kiválasztásával. Az egyes Identity Manager-kérelmeket a rendszer a JSON-struktúra Windows-eseménynaplójában lévő eseményként exportálja. Exportálhatja az eredményt a biztonsági információ és az Event Management (SIEM) rendszerébe.

|Eseménytípus|ID (Azonosító)|Esemény részletei|
|--------------|------|-----------------|
|Tájékoztatás|4121|Az összes kérelem adatait tartalmazó Identity Manager-esemény adatai.|
|Tájékoztatás|4137|Az Identity Manager-esemény 4121-bővítménye, ha túl sok az adatok egyetlen eseményhez. Az esemény fejléce a következő formátumban jelenik meg: `"Request: <GUID> , message <xxx> out of <xxx>` .|
