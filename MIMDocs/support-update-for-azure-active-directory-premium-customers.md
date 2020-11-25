---
title: Támogatási frissítés prémium szintű Azure AD-ügyfelekhez Microsoft Identity Manager használatával | Microsoft Docs
description: Ez a cikk azt ismerteti, hogy prémium szintű Azure AD ügyfelek hogyan kaphatnak támogatást a 2021. január 21. után.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 6/9/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
reviewer: markwahl-msft
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 26dcaf121c4fd980d296ffee893af3ca66249c6c
ms.sourcegitcommit: dae61d97c9db5402d35e2757a1ce844d16236032
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96035616"
---
# <a name="support-update-for-azure-ad-premium-customers-using-microsoft-identity-manager"></a>Támogatási frissítés prémium szintű Azure AD ügyfelek számára a Microsoft Identity Manager használatával

A következőkre vonatkozik: prémium szintű Azure AD, Microsoft Identity Manager (webalkalmazás)

Prémium szintű Azure AD ügyfelek esetében a standard szintű támogatás az Azure AD-integrációt engedélyező [Microsoft Identity Manager 2016 Service Pack 2](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016)vagy újabb szervizcsomagok adott összetevői esetében 2020. júniustól 2021 kezdődően érhető el. Ez a Microsoft Identity Manager meglévő támogatásán felül már a [rögzített életciklus-szabályzaton](https://docs.microsoft.com//lifecycle/policies/fixed) és a [vállalkozások támogatásának](https://support.microsoft.com/help/4341255)csomagjain keresztül is elérhető.

A standard szintű támogatást a következő, a rendelkezésre álló csomag-összetevők tartalmazzák:
- A rendszer-szinkronizálási szolgáltatás és a jelszó-módosítási értesítési szolgáltatás (PCN)
- A tárház szolgáltatás és portál, beépülő modulok és bővítmények, adattárház-támogatás parancsfájljai és nyelvi csomagjai
- A rendszerösszekötők

Ezek a rendszerállapot-azonosítók az Azure AD-vel Active Directory és kiterjesztéssel, a Azure AD Connecton keresztül, a helyszíni HR rendszerből vagy más adatforrásokból kiépített felhasználókkal és csoportokkal tölthetők fel. Ez biztosítja, hogy a helyszíni rendszerekkel prémium szintű Azure AD használó ügyfelek továbbra is támogatottak legyenek a helyszíni rendszerekről az Azure AD-be való Migrálás során. 

## <a name="opening-a-support-request-in-the-azure-portal"></a>Támogatási kérelem megnyitása a Azure Portal

A Microsoft Identity Manager további támogatási lehetőségként prémium szintű Azure AD ügyfeleink támogatást kérhetnek a Microsoft Identity Manager 2016 Service Pack 2 fent említett összetevőiről, vagy egy későbbi gyorsjavításról vagy frissítésről az Azure Portalon keresztül.

Az ügyfél létrehozhat egy Azure-támogatási kérelmet az [Azure-támogatási kérelem létrehozásával kapcsolatos](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)utasítások alapján:
1. Válassza ki a *probléma típusát: technikai*
1. Váltás az *összes szolgáltatás* megjelenítéséhez
1. a Azure Active Directory alatti szolgáltatások listájában válassza a *felhasználó kiépítés és szinkronizálás* lehetőséget.
1. a *probléma típusának kiválasztása: Microsoft Identity Manager (* weblapon)
1. Válassza ki a *probléma altípusát*: *Összekötők*, *szolgáltatás és portál* vagy *szinkronizáló motor*

![A Support request létrehozása](media/azure-active-directory-new-support-request.png)

A rendszer a rendszerállapot-összetevők listáját *Azure Active Directory felhasználó üzembe helyezése és szinkronizálása* során felmerülő problémák típusaként sorolja fel a Azure Portal.

A Azure Portal által megnyitott kérelmek esetében a standard szintű támogatás a Microsoft Identity Manager 2016 Service Pack 2, illetve egy [újabb gyorsjavítás vagy frissítés](reference/version-history.md): szinkronizációs szolgáltatás, jelszó-módosítási értesítési szolgáltatás (PCN), összekötők, szolgáltatás és portál, beépülő modulok és bővítmények, adatraktár-támogatási parancsfájlok és nyelvi csomagok esetében érhető el prémium szintű Azure ad ügyfeleknek.

## <a name="other-support-options"></a>Egyéb támogatási lehetőségek

A 2016-es verzióban megjelent a 4.6.34.0 Build, amely az 2019-es verzióban jelent meg. Javasoljuk, hogy az ügyfelek teljes körűen támogatott szervizcsomaggal maradjanak, így biztosítva, hogy a termékük legújabb és legbiztonságosabb verzióját használják. További információ: [szervizcsomag-életciklusra](https://support.microsoft.com/help/17138)vonatkozó szabályzat.

Továbbra is elérhetők maradnak azok az ügyfelek, akik még nem rendelkeznek Azure-támogatással vagy előfizetéssel olyan csomagra, amely prémium szintű Azure Active Directoryt tartalmaz A támogatási szabályzatot a [rögzített életciklus-szabályzat](https://docs.microsoft.com/lifecycle/policies/fixed) írja le a Microsoft Identity Manager 2016-es verzió [támogatási életciklusában](https://support.microsoft.com/lifecycle/search?alpha=microsoft%20identity%20manager%202016)megadott dátumokkal.

Az Azure-támogatás mellett számos más támogatási lehetőség is használható a szervezetek számára a támogatás megszerzéséhez. Ha például Microsoft Professional-támogatást használ, [létrehozhat egy új támogatási kérést](https://support.microsoft.com/supportforbusiness/productselection). A megfelelő rendszerelem-összetevő kiválasztása:
1. a termékcsalád *biztonságának* kiválasztása
1. Válassza ki a termék *Identity Manager 2016*
