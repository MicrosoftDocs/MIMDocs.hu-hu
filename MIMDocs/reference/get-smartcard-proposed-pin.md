---
title: Intelligens kártya javasolt PIN-kódjának beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9d4f1444efa490f980e29440342844ac246d8a20
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760601"
---
# <a name="get-smart-card-proposed-pin"></a>Intelligens kártya javasolt PIN-kódjának beolvasása
A kiszolgáló által generált felhasználói PIN-kód beolvasása.

>[!IMPORTANT]
>A kiszolgáló csak akkor állítja be a PIN-kódot, ha a profil sablonjának házirendje azt jelzi, hogy el kell végezni. Ellenkező esetben a felhasználónak meg kell adnia a PIN-kódot.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpin

### <a name="url-parameters"></a>URL-paraméterek

Paraméter | Leírás
---------|------------
id | Az intelligens kártyás azonosító, amely a Microsoft Identity Manager (felügyeleti pont) tanúsítványkezelő (CM) számára van kialakítva. A rendszer az azonosítót a Mikroszkóp. CLM. Shared. SmartCard objektumból szerzi be.

### <a name="query-parameters"></a>Lekérdezési paraméterek

Paraméter | Leírás
---------|------------
ATR | Az intelligens kártya válasz-visszaállítási (ATR) karakterlánca.
cardid | Az intelligens kártya azonosítója.
kérdés | Egy Base-64 kódolású karakterlánc, amely az intelligens kártya által kiadott kihívásokat jelképezi.

### <a name="request-headers"></a>Kérésfejlécek
A gyakori kérelmek fejléceit lásd: [http-kérelem és válasz-fejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *cm REST API szolgáltatás részleteiben* .

### <a name="request-body"></a>A kérés törzse
Nincsenek.

## <a name="response"></a>Reagálás
Ez a szakasz a választ ismerteti.

### <a name="response-codes"></a>Reagálási kódok

Kód  |Leírás  
---------|---------
200 | OK
204 | Nincs tartalom
403 | Forbidden
500 | Belső hiba

### <a name="response-headers"></a>Válaszfejlécek
Gyakori válasz-fejlécek esetén lásd: [http-kérés és-válasz fejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) *cm REST API szolgáltatás részletei* .

### <a name="response-body"></a>Választörzs
Sikeres művelet esetén a a kiszolgáló által javasolt PIN-kódot jelölő karakterláncot ad vissza.

## <a name="example"></a>Példa
Ez a szakasz egy példát mutat be a kiszolgáló által generált felhasználói PIN-kód beszerzésére.

### <a name="example-request"></a>Példa: kérelem

```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK

... body coming soon
```       
