---
title: PAM-kérelem lezárása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: be7c2ff956f928a32de343fcf47b5398fdd303f0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760762"
---
# <a name="close-pam-request"></a>PAM-kérelem lezárása
A Kiemelt fiók a PAM-szerepkörhöz való jogosultságszint-emeléshez kezdeményezett kérelem lezárására használja.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
POST     |/API/pamresources/pamrequests ({kérelemazonosító)/Close

### <a name="url-parameters"></a>URL-paraméterek

Paraméter | Leírás
----------|-----------
Kérelemazonosító | A PAM-kérelemhez megadott azonosító (GUID), amely a következőként van megadva: `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'` .

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
Ez a szakasz egy példát mutat be a kérelem lezárására.

### <a name="example-request"></a>Példa: kérelem

```
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK
```       
