---
title: Profil-sablonok beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9f14853b3895dece1098270d35439d6c4999be2b
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760421"
---
# <a name="get-profile-templates"></a>Profil sablonok beolvasása
Lekéri azon profil-sablonok listáját, amelyekhez a megadott felhasználó regisztrálhat. Ez a metódus a profil sablonjának korlátozott nézetét adja vissza. A profil sablonjának a visszaadott adatokat elegendőnek kell lennie ahhoz, hogy a kérelmező felhasználó döntse el, hogy melyik felhasználóiprofil-sablont kell regisztrálnia. Ha nincs megadva munkafolyamat és engedély, a rendszer a felhasználó számára látható összes felhasználóiprofil-sablont visszaadja.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates? \[ targetuser\] 

### <a name="url-parameters"></a>URL-paraméterek

Paraméter| Leírás
--------|-------------
targetuser| Választható. Megadja a célként megadott felhasználó számára a profil sablonjainak visszaadását. Ha nincs megadva, a rendszer az aktuális felhasználó identitását használja. <br/><br/>**Megjegyzés** : jelenleg csak az aktuális felhasználó támogatott.

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
500 | Belső hiba

### <a name="response-headers"></a>Válaszfejlécek
Gyakori válasz-fejlécek esetén lásd: [http-kérés és-válasz fejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) *cm REST API szolgáltatás részletei* .

### <a name="response-body"></a>Választörzs
Sikeres művelet esetén a a következő tulajdonságokkal rendelkező ProfileTemplateLimitedView-objektumok listáját adja vissza:

Tulajdonság| Típus| Leírás
--------|-----|--------
Name (Név)| sztring| A profil sablon megjelenítendő neve.
Description| sztring| A profil sablonjának leírása.
UUID| Guid| A profil sablonjának azonosítója.
IsSmartcardProfileTemplate| logikai| Azt jelzi, hogy a sablon intelligens kártyás profil-sablon-e.
IsVirtualSmartcardProfileTemplate| logikai| Azt jelzi, hogy a profil sablonjának szüksége van-e virtuális intelligens kártyára.

## <a name="example"></a>Példa
Ez a szakasz egy példát mutat be a megadott felhasználó profiljához tartozó sablonok listájának beszerzésére.

### <a name="example-request"></a>Példa: kérelem

```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]
```       
