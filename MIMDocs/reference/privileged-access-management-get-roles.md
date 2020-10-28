---
title: PAM-szerepkörök beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f802dc22019f1ad97fc5cce8c9ed2e000349dc58
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760894"
---
# <a name="get-pam-roles"></a>PAM-szerepkörök beolvasása
Egy kiemelt fiók használja azon PAM-szerepkörök listázásához, amelyekhez a fiók egy jelölt.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
GET     |/api/pamresources/pamroles

### <a name="query-parameters"></a>Lekérdezési paraméterek

Paraméter | Leírás
----------|--------------
$filter | Választható. Adja meg a PAM szerepkör bármely tulajdonságát egy szűrési kifejezésben a válaszok szűrt listájának visszaadásához. A támogatott operátorokkal kapcsolatos további információkért lásd: [Szűrés a PAM REST API szolgáltatás részletei](privileged-access-management-rest-api-service-details.md#filtering).
v | Választható. Az API verziója. Ha nem tartalmazza, a rendszer az API aktuális (legutóbb kiadott) verzióját használja. További információ: [verziószámozás a PAM REST API szolgáltatásban](privileged-access-management-rest-api-service-details.md#versioning).

### <a name="request-headers"></a>Kérésfejlécek
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
A sikeres válasz egy vagy több PAM-szerepkör gyűjteményét tartalmazza, amelyek mindegyike a következő tulajdonságokkal rendelkezik:

Tulajdonság | Leírás
--------|-------------
Szerepkörazonosítónak | A PAM-szerepkör egyedi azonosítója (GUID).
DisplayName | A PAM-szerepkör megjelenítendő neve a fakiszolgálói szolgáltatásban.
Leírás | A PAM-szerepkör leírása a fakiszolgáló szolgáltatásban.
TTL | A szerepkör hozzáférési jogosultságának maximális lejárati ideje másodpercben.
AvailableFrom | A legkorábbi napszak, amikor egy kérés aktiválva van.
AvailableTo | A legutóbbi nap, amikor egy kérés aktiválva van.
MFAEnabled | Logikai érték, amely azt jelzi, hogy az ehhez a szerepkörhöz tartozó aktiválási kérések MFA-kihívást igényelnek-e.
ApprovalEnabled | Logikai érték, amely azt jelzi, hogy a szerepkörhöz tartozó aktiválási kérelmek jóváhagyását igényli-e a szerepkör tulajdonosa.
AvailabilityWindowEnabled | Logikai érték, amely azt jelzi, hogy a szerepkör csak a megadott időintervallumban aktiválható-e.

## <a name="example"></a>Példa
Ez a szakasz a PAM-szerepkörök beszerzésére mutat példát.

### <a name="example-request"></a>Példa: kérelem

```
GET /api/pamresources/pamroles HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
