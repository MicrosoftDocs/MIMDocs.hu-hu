---
title: PAM-kérelmek beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 620eebd6-e4c3-473b-b824-ee6cfe83e509
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0eeaee3a6355229de94bcc390ad07da3e00b829f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760738"
---
# <a name="get-pam-requests"></a>PAM-kérések beolvasása
Egy rendszerjogosultságú fiók használja a korábban közzétett PAM-kérések előzményeinek visszaadásához.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
GET     |/api/pamresources/pamrequests

### <a name="query-parameters"></a>Lekérdezési paraméterek

Paraméter | Leírás
----------|--------------
$filter | Választható. Adja meg a PAM-kérések tulajdonságait egy szűrési kifejezésben a válaszok szűrt listájának visszaadásához. A támogatott operátorokkal kapcsolatos további információkért lásd: [Szűrés a PAM REST API szolgáltatás részletei](privileged-access-management-rest-api-service-details.md#filtering).
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
A sikeres válasz tartalmazza a PAM-kérelmek objektumainak listáját a következő tulajdonságokkal:

Tulajdonság | Leírás
--------|-------------
RequestID | A PAM-kérelem egyedi azonosítója (GUID).
CreatorID | Egy egyedi azonosító (GUID) a PAM-kérést létrehozó Active Directory-fiókhoz.
Indoklás | A Jogosultságszint-emelés oka.
DisplayName | A PAM-kérelem megjelenítendő neve a (z) webhelyen.
CreationTime (Létrehozás ideje) | A kérelem létrehozásának időpontja.
CreationMethod | A kérelem létrehozásához használt metódus.
ExpirationTime | A kérelem lejárati ideje.
Szerepkörazonosítónak| A PAM-szerepkör egyedi azonosítója (GUID).
RequestedTTL | A kért lejárati időkorlát másodpercben.
RequestedTime | A Jogosultságszint-emelés kért ideje.
RequestedStatus | A kérelem állapota. A lehetséges értékek: "aktív", "lezárva", "Bezárás", "lejárt", "PendingApproval" és "visszautasítva".

## <a name="example"></a>Példa
Ez a szakasz egy példát mutat be a közzétett PAM-kérelmek beszerzésére.

### <a name="example-request"></a>Példa: kérelem

```
GET /api/pamresources/pamrequests?$filter=CreationTime%20gt%20datetime'2015-06-12T04:49:32.431Z'%20and%20CreationTime%20lt%20datetime'2015-07-13T04:49:32.432Z' HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests",
    "value":[
        {
            "RequestId":"b22e1343-9a2b-4e33-a70a-1bb7b2d405b9",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-06-23T11:34:38.58Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-06-23T12:34:38.847Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-23T11:34:36.417Z",
            "RequestStatus":"Expired"
        },
        {
            "RequestId":"3a98d1c7-524d-4b72-9da7-bd9f907eab55",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Reason for Request",
            "CreationTime":"2015-07-12T04:35:14.433Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T04:43:51.95Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:35:00Z",
            "RequestStatus":"Closed"
        },
        {
            "RequestId":"f5e80be1-e9a3-42c4-81f8-4be5a4a429f4",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-07-12T04:48:17.46Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T05:48:17.853Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-12T04:48:14.057Z",
            "RequestStatus":"Active"
        },
        {
            "RequestId":"b0f0ddc0-c809-4770-9d39-0d706f97a2de",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"",
            "CreationTime":"2015-06-30T07:01:13.147Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-30T07:01:13.119Z",
            "RequestStatus":"Rejected"
        },
        {
            "RequestId":"5ec10e61-cdd1-404e-a18e-740467d87dbf",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Example Reason",
            "CreationTime":"2015-07-12T04:49:09.963Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:50:00Z",
            "RequestStatus":"PendingApproval"
        }
    ]
}
```       
