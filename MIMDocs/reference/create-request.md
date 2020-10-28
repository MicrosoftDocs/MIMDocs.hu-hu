---
title: Kérelem létrehozása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 51016951f9dec22e2a40b44a2b76a840192b518f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760672"
---
# <a name="create-request"></a>Kérelem létrehozása
Hozzon létre egy Microsoft Identity Manager (felügyeleti pont) tanúsítványkezelő (CM) kérelmet.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

### <a name="url-parameters"></a>URL-paraméterek
Nincsenek.

### <a name="request-headers"></a>Kérésfejlécek
A gyakori kérelmek fejléceit lásd: [http-kérelem és válasz-fejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *cm REST API szolgáltatás részleteiben* .

### <a name="request-body"></a>A kérés törzse
A kérelem törzse a következő tulajdonságokat tartalmazza:

Tulajdonság | Leírás
---------|-----------
profiletemplateuuid | Kötelező. Annak a profil-sablonnak a GUID azonosítója, amelyhez a felhasználó létrehozta a kérést.
datacollection objektumot | Kötelező. Név-érték párok gyűjteménye, amely a tanúsítvány-igénylő által megadott adatmennyiséget jelképezi. A szükséges adatok gyűjtését a profil sablonjának munkafolyamat-házirendjéből kérheti le. Üres gyűjtemény is megadható.
cél | Választható. Annak a célként megadott felhasználónak a GUID azonosítója, amelyhez a kérést létre kívánja hozni. Ha nincs megadva, a cél alapértelmezett értéke az aktuális felhasználó.
típus | Kötelező. A létrehozandó kérelem típusa. Az elérhető kérelmek típusai közé tartozik a "regisztráció", a "duplikált", a "OfflineUnblock", a "OnlineUpdate", a "megújítás", a "helyreállítás", a "RecoverOnBehalf", a "visszaállítás", a "Visszavonás", a "Visszavonás", a "TemporaryCards" és a "letiltás".<br/><br/>**Megjegyzés** : az összes kérelem típusa nem támogatott az összes profil sablonban. Például nem adhatja meg a feloldási műveletet egy felhasználóiprofil-sablonon.
megjegyzés | Kötelező. A felhasználó által megadható megjegyzések. A munkafolyamat-házirend meghatározza, hogy szükség van-e a Megjegyzés tulajdonságra. Üres karakterlánc is megadható.
prioritású | Választható. A kérelem prioritása. Ha nincs megadva, a rendszer az alapértelmezett kérés prioritását használja a profil sablonjának beállításai alapján.


## <a name="response"></a>Reagálás
Ez a szakasz a választ ismerteti.

### <a name="response-codes"></a>Reagálási kódok

Kód  |Leírás  
---------|---------
201 | Létrehozva
403 | Forbidden
500 | Belső hiba

### <a name="response-headers"></a>Válaszfejlécek
Gyakori válasz-fejlécek esetén lásd: [http-kérés és-válasz fejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) *cm REST API szolgáltatás részletei* .

### <a name="response-body"></a>Választörzs
Sikeres művelet esetén az újonnan létrehozott kérelem URI-JÁT adja vissza.

## <a name="example"></a>Példa
Ez a szakasz a beléptetési és feloldási kérelmek létrehozásához nyújt példát.

### <a name="example-request-1"></a>Példa: 1. kérelem

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```

### <a name="example-response-1"></a>Példa: 1. Válasz

```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```

### <a name="example-request-2"></a>Példa: 2. kérelem

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```

### <a name="example-response-2"></a>Példa: 2. Válasz

```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

### <a name="example-request-3"></a>Példa: 3. kérelem

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```

## <a name="see-also"></a>Lásd még

- [Microsoft.Clm.Provision.RequestOperations.InitiateEnroll metódus](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock metódus](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRecover metódus](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRetire metódus](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateUnblock metódus](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
