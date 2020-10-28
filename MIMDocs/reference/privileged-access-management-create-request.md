---
title: PAM-kérelem létrehozása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d58155232b3f99f32df9cb6b4c2e344d0cbad278
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760763"
---
# <a name="create-pam-request"></a>PAM-kérelem létrehozása
Egy emelt szintű fiók használja a PAM-szerepkörhöz való jogosultságszint-emeléshez.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
POST     |/api/pamresources/pamrequests

### <a name="query-parameters"></a>Lekérdezési paraméterek

Paraméter | Leírás
--------|-------------
Indoklás | Választható. A jogosultságszint-emelési kérelem felhasználó által megadott oka.
Szerepkörazonosítónak | Kötelező. Annak a PAM-szerepkörnek az egyedi azonosítója (GUID), amelyhez a-t emelni kell.
RequestedTTL | Kötelező. A kért lejárati idő másodpercben.
RequestedTime | Választható. A jogosultságok megemelésének ideje.  
v | Választható. Az API verziója. Ha nem tartalmazza, a rendszer az API aktuális (legutóbb kiadott) verzióját használja. További információ: [verziószámozás a PAM REST API szolgáltatásban](privileged-access-management-rest-api-service-details.md#versioning).

>[!NOTE]
>Az **indoklást** , a **szerepkörazonosítónak** , a **RequestedTTL** és a **RequestedTime** paramétereket a kérelem törzsében, nem pedig lekérdezési paraméterekként adhatja meg. A **v** paraméter csak lekérdezési paraméterként adható meg.

### <a name="request-headers"></a>Kérésfejlécek
A gyakori kérelmek fejléceit lásd: [http-kérelem és válasz-fejlécek](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) a *PAM REST API szolgáltatás részleteiben* .

### <a name="request-body"></a>A kérés törzse
Választható. Az **indoklás** , a **szerepkörazonosítónak** , a **RequestedTTL** és a **RequestedTime** paraméterek a kérelem törzsének tulajdonságaiként adhatók meg ahelyett, hogy megadják őket az URL-lekérdezési karakterláncban.

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
A sikeres válasz egy PAM-kérelem objektumot tartalmaz, amely a következő tulajdonságokkal rendelkezik:

Tulajdonság | Leírás
--------|-------------
RequestID | A PAM-kérelem egyedi azonosítója (GUID).
CreatorID | A kérést létrehozó fiók egyedi azonosítója (GUID) a rendszerkiszolgálói szolgáltatásban.
Indoklás | A Jogosultságszint-emelés oka.
CreationTime (Létrehozás ideje) | A kérelem létrehozásának időpontja.
CreationMethod | A kérelem létrehozásához használt metódus.
ExpirationTime | A kérelem lejárati ideje.
Szerepkörazonosítónak| A PAM-szerepkör egyedi azonosítója (GUID).
RequestedTTL | A kért lejárati időkorlát másodpercben.
RequestedTime | A Jogosultságszint-emelés kért ideje.
RequestStatus | A kérelem állapota. A lehetséges értékek a következők: "processing", "Active", "Closed", "zárás", "lejárt", "PendingApproval", "PendingMFA" és "Elutasítva".

## <a name="example"></a>Példa
Ez a szakasz a PAM-kérések létrehozására vonatkozó példákat tartalmaz.

### <a name="example-request-1"></a>Példa: 1. kérelem

```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```

### <a name="example-response-1"></a>Példa: 1. Válasz

```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

### <a name="example-request-2"></a>Példa: 2. kérelem

```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```

### <a name="example-response-2"></a>Példa: 2. Válasz

```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
