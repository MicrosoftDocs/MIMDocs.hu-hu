---
title: Privileged Access Management REST API-hivatkozás | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8a318ae5f12fd49e2ee949d81aefd86a7221e120
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760721"
---
# <a name="privileged-access-management-rest-api-reference"></a>Privileged Access Management REST API-hivatkozás
A Microsoft Identity Manager (2016) nevű új forgatókönyv Privileged Access Management (PAM). A PAM lehetővé teszi, hogy a szervezet nagyobb mértékben vezérelje a magas jogosultságú felhasználói fiókok, például a rendszer vagy a szolgáltatás-rendszergazdák hozzáférési jogait a bizalmas erőforrásokhoz. A PAM a magas jogosultsági szintű fiókok hozzáférését úgy szabályozza, hogy a hozzáférési jogosultságok megadásához korlátozott időtartamú hozzáférési jogosultságot biztosít, igény szerint (JIT).

A felhasználók kétféleképpen kérhetik a rendszerjogosultságú hozzáférési jogosultságok (Jogosultságszint-emelés) szolgáltatást a következő két módszer egyikével:

- A PAM REST API használatával.
- A PAM PowerShell New-PAMRequest parancsmag használatával.

Az útmutató témakörei a PAM-REST API ismertetik. A PowerShell-parancsmag használatával kapcsolatos további információkért tekintse meg _a tesztkörnyezet útmutatója: Privileged Access Management megjelenítése a Microsoft Identity Manager használatával_ című témakört, amely elérhető a csatlakozás helyén.

## <a name="pam-rest-api-resources-and-operations"></a>PAM REST API erőforrások és műveletek
A PAM-REST API a következő erőforrásokon működik:
- **PAM-szerepkör** : a PAM-szerepkörök felhasználói gyűjteményt társítanak a hozzáférési jogosultságok gyűjteményéhez. A hozzáférési jogosultságok a biztonsági csoportokra való hivatkozással vannak meghatározva.  Minden PAM-szerepkör tartalmazza a jelöltek nevű felhasználói fiókokat, amelyek jogosultak a PAM szerepkörhöz való jogosultságszint-emelésre. A PAM-szerepkörökön a következő műveleteket hajthatja végre:

    - [PAM-szerepkörök lekérése](privileged-access-management-get-roles.md)

- **PAM-kérelem** : az a felhasználó, aki a PAM szerepkör-hozzáférési jogosultságokat kívánja emelni, egy PAM-kérést kell küldenie, és jóváhagyást kell kérnie a jogosultságszint-emelési kéréshez. A PAM-kérelmi objektum nyomon követi a kérelem életciklusát a fakiszolgálói szolgáltatásban. A PAM-kérelmeken a következő műveleteket hajthatja végre:

    - [PAM-kérelem létrehozása](privileged-access-management-create-request.md)
    - [PAM-kérések lekérése](privileged-access-management-get-requests.md)
    - [PAM-kérelmek lezárása](privileged-access-management-close-request.md)

- **Függőben lévő PAM-kérelem** : a felhasználók által elküldött PAM-kérelmek jóváhagyására vagy elutasítására használatos. A függőben lévő PAM-kérelmeken a következő műveleteket hajthatja végre:

    - [Függőben lévő PAM-kérések lekérése](privileged-access-management-get-pending-requests.md)
    - [Függőben lévő PAM-kérelmek jóváhagyása vagy elutasítása](privileged-access-management-approve-reject-pending-request.md)

- **PAM-munkamenet** : a pam-REST API használatakor az ügyfél (például egy webböngésző) a PAM REST API végponttal rendelkező munkamenettel rendelkezik. Ebben a munkamenetben az ügyfél hitelesítve van a REST API végponton. A PAM-munkamenetekben a következő műveleteket hajthatja végre:

     - [PAM-munkamenetadatok lekérése](privileged-access-management-get-session-info.md)

A szolgáltatással kapcsolatos részletesebb információkért tekintse meg a [PAM REST API szolgáltatás részleteit](privileged-access-management-rest-api-service-details.md).

## <a name="pam-sample-portal-on-github"></a>PAM-minta portál a GitHubon
Az egyik módszer a PAM-REST API használatának megismerésére a PAM-minta portál használatával, az API-t használó webalkalmazásokkal. A PAM minta-portál kódját a [githubon található PAM-minta tárházban](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409)találja. Megtudhatja, hogyan helyezheti üzembe a mintavételi portált a PAM test Lab útmutatójában.
