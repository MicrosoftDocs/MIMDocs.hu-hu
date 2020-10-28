---
title: Függőben lévő PAM-kérelem jóváhagyása vagy elutasítása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 743c28b343c374d3406600751b379e8a4695c2d0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760775"
---
# <a name="approve-or-reject-a-pending-pam-request"></a>Függőben lévő PAM-kérelem jóváhagyása vagy elutasítása
Egy kiemelt fiók használja a PAM-szerepkörhöz való jogosultságszint-emelésre irányuló kérelem jóváhagyására, bezárására vagy elutasítására.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
POST     |/api/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/api/pamresources/pamrequeststoapprove({approvalId)/Reject

### <a name="url-parameters"></a>URL-paraméterek

Paraméter | Leírás
----------|-----------
approvalId | A PAM-beli jóváhagyási objektum azonosítója (GUID), amely a következőként van megadva: `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'` .

### <a name="query-parameters"></a>Lekérdezési paraméterek

Paraméter | Leírás
----------|--------------
v | Választható. Az API verziója. Ha nem tartalmazza, a rendszer az API aktuális (legutóbb kiadott) verzióját használja. További információ: [verziószámozás a PAM REST API szolgáltatásban](privileged-access-management-rest-api-service-details.md#versioning).


### <a name="request-headers"></a>Kérések fejlécei
A gyakori kérelmek fejléceit lásd: [http-kérelem és válasz-fejlécek](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) a *PAM REST API szolgáltatás részleteiben* .

### <a name="request-body"></a>A kérés törzse
Nincsenek.

## <a name="response"></a>Reagálás
Ez a szakasz a választ ismerteti.

### <a name="response-codes"></a>Reagálási kódok

Kód  |Leírás  
---------|---------
200 | OK
401 | Nem engedélyezett
403 | Forbidden
408 | Kérelem időtúllépése   
500 | Belső kiszolgálóhiba
503 | A szolgáltatás nem érhető el

### <a name="response-headers"></a>Válaszfejlécek
A gyakori kérelmek fejléceit lásd: [http-kérelem és válasz-fejlécek](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) a *PAM REST API szolgáltatás részleteiben* .

### <a name="response-body"></a>Választörzs
Nincsenek.

## <a name="example"></a>Példa
Ez a szakasz egy példát mutat be egy PAM-szerepkörhöz való jogosultságszint-emelésre irányuló kérelem jóváhagyására.

### <a name="example-request"></a>Példa: kérelem

```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK
```       
