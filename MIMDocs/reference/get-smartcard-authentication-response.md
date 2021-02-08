---
title: Intelligens kártya hitelesítési válaszának beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 62811bd5d225f981ded6a6439584c2b7730251c1
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835591"
---
# <a name="get-smart-card-authentication-response"></a>Intelligens kártya hitelesítési válaszának beolvasása
Az alap kriptográfiai szolgáltató (CSP) hitelesítési kihívásra adott válasz beolvasása.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Metódus  |URL-cím kérése  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse

### <a name="url-parameters"></a>URL-paraméterek

Paraméter | Leírás
---------|------------
reqid | Kötelező. Az Microsoft Identity Manager (felügyeleti pont) tanúsítványkezelő (CM) számára megadott kérelem-azonosító.
scid | Kötelező. Az MIM CMre vonatkozó intelligens kártya azonosítója. A scid a [Microsoft. CLM. Shared. smartcards. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objektumból szerezhető be.

### <a name="query-parameters"></a>Lekérdezési paraméterek

Paraméter | Leírás
---------|------------
ATR | Választható. Az intelligens kártya válasz-visszaállítási (ATR) karakterlánca.
cardid | Kötelező. Az intelligens kártya azonosítója.
kérdés | Kötelező. Egy Base-64 kódolású karakterlánc, amely az intelligens kártya által kiadott kihívásokat jelképezi.
vegyes | Kötelező. Logikai jelző, amely azt jelzi, hogy az intelligens kártya rendszergazdai kulcsa diverzifikálva lett-e.

### <a name="request-headers"></a>Kérésfejlécek
A gyakori kérelmek fejléceit lásd: [http-kérelem és válasz-fejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *cm REST API szolgáltatás részleteiben*.

### <a name="request-body"></a>A kérés törzse
Nincsenek.

## <a name="response"></a>Reagálás
Ez a szakasz a választ ismerteti.

### <a name="response-codes"></a>Reagálási kódok

Code  |Description  
---------|---------
200 | OK
204 | Nincs tartalom
403 | Forbidden
500 | Belső hiba

### <a name="response-headers"></a>Válaszfejlécek
Gyakori válasz-fejlécek esetén lásd: [http-kérés és-válasz fejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) *cm REST API szolgáltatás részletei*.

### <a name="response-body"></a>Választörzs
A sikeres művelet után egy bájtos BLOBot ad vissza, amely a kérdéses választ jelöli.

## <a name="example"></a>Példa
Ez a szakasz egy példát mutat be arra, hogy a rendszer választ kapjon egy alapszintű CSP hitelesítési kihívásra.

### <a name="example-request"></a>Példa: kérelem

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       

## <a name="see-also"></a>Lásd még

- [Microsoft.Clm.Provision.ExecuteOperations. GetBaseCspResponse metódus](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
