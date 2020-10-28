---
title: Profil állapotával kapcsolatos műveletek beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 35246570b52285f916b74785c17ce55f2967fb7d
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760649"
---
# <a name="get-profile-state-operations"></a>Profil állapotával kapcsolatos műveletek beolvasása
Lekéri a megadott profil aktuális felhasználója által végrehajtható lehetséges műveletek listáját. A rendszer ezután kezdeményezheti a kérelmek bármelyikét a megadott műveletekhez.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/operations <br/>/CertificateManagement/api/v1.0/smartcards/{id}/operations

### <a name="url-parameters"></a>URL-paraméterek

Paraméter | Leírás
---------|------------
id | A profil vagy intelligens kártya azonosítója (GUID).

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
Sikeres művelet esetén az intelligens kártyán a felhasználó által végrehajtható lehetséges műveletek listáját adja vissza. A lista a következő műveletek tetszőleges számát tartalmazhatja: **OnlineUpdate** , **megújítás** , **helyreállítás** , **RecoverOnBehalf** **,** **kivonás, visszavonás** és **feloldás** .

## <a name="example"></a>Példa
Ez a szakasz egy példát mutat be a profil állapotának beolvasására az aktuális felhasználó számára.

### <a name="example-request"></a>Példa: kérelem

```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
