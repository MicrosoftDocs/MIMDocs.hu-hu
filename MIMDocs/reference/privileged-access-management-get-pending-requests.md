---
title: Függőben lévő PAM-kérelmek beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0ec4388ce5d5f748d9f779819d0a2d0bdec06791
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760774"
---
# <a name="get-pending-pam-requests"></a>Függőben lévő PAM-kérések beolvasása
Egy rendszerjogosultságú fiók használja a jóváhagyást igénylő függőben lévő kérések listájának visszaadásához.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

### <a name="query-parameters"></a>Lekérdezési paraméterek

Paraméter | Leírás
----------|--------------
$filter | Választható. Adja meg egy szűrési kifejezés függőben lévő PAM-kérelmének tulajdonságait a válaszok szűrt listájának visszaadásához. A támogatott operátorokkal kapcsolatos további információkért lásd: [Szűrés a PAM REST API szolgáltatás részletei](privileged-access-management-rest-api-service-details.md#filtering).
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
A sikeres válasz tartalmazza a PAM-kérelmek jóváhagyási objektumainak listáját a következő tulajdonságokkal:

Tulajdonság | Leírás
---------|-------------
RoleName | Annak a szerepkörnek a megjelenítendő neve, amelyhez jóváhagyás szükséges.
Requestor (Kérelmező) | A jóváhagyandó kérelmező felhasználóneve.
Indoklás | A felhasználó által megadott indoklás.
RequestedTTL | A kért lejárati idő másodpercben.
RequestedTime | A Jogosultságszint-emelés kért ideje.
CreationTime (Létrehozás ideje) | A kérelem létrehozásának időpontja.
FIMRequestID | Egyetlen elemet ("value") tartalmaz a PAM-kérelem egyedi azonosítójával (GUID).
RequestorID | Egyetlen elemet ("value") tartalmaz a PAM-kérést létrehozó Active Directory fiók egyedi azonosítójával (GUID).
ApprovalObjectID | Egyetlen elemet ("value") tartalmaz a jóváhagyási objektum egyedi azonosítójával (GUID).

## <a name="example"></a>Példa
Ez a szakasz egy példát mutat be a függőben lévő PAM-kérelmek beszerzésére.

### <a name="example-request"></a>Példa: kérelem

```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
