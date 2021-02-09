---
title: Intelligens kártya – változatos rendszergazdai kulcs beszerzése | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5e21cd92496fd31ec12044f3b69599bf96bef62a
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835609"
---
# <a name="get-smart-card-diversified-admin-key"></a>Intelligens kártya változatos rendszergazdai kulcsának beolvasása
Beolvassa a megadott intelligens kártyához tartozó változatos rendszergazdai kulcsot.

>[!IMPORTANT]
>Az adminisztrátori kulcsot csak akkor kell diverzifikálni, ha a profil sablonjának házirendje azt jelzi, hogy legyen.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Metódus  |URL-cím kérése  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/diversifiedkey

### <a name="url-parameters"></a>URL-paraméterek

Paraméter | Leírás
---------|------------
reqid | Kötelező. Az Microsoft Identity Manager (felügyeleti pont) tanúsítványkezelő (CM) számára megadott kérelem-azonosító.
scid | Kötelező. Az MIM CMre vonatkozó intelligens kártya azonosítója. A scid a [Microsoft. CLM. Shared. smartcards. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objektumból szerezhető be.

### <a name="query-parameters"></a>Lekérdezési paraméterek

Paraméter | Leírás
---------|------------
ATR | Választható. Az intelligens kártya válasz-visszaállítási (ATR) karakterlánca.
cardid | Kötelező. A kártya azonosítója.

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
A sikeres művelet után egy bájtos BLOBot ad vissza, amely a diverzifikált rendszergazdai kulcsot jelképezi.

## <a name="example"></a>Példa
Ez a szakasz egy példát mutat be egy intelligens kártya változatos felügyeleti kulcsának beszerzésére.

### <a name="example-request"></a>Példa: kérelem

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       

## <a name="see-also"></a>Lásd még

- [Microsoft. CLM. kiépítés. RequestOperations. CreateSmartcard metódus (karakterlánc, karakterlánc, kérelem)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
