---
title: Intelligens kártya kiosztása egy kérelemhez | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a8acf3defc2087fc6ea08bf8063579058465fcc8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760505"
---
# <a name="assign-a-smart-card-to-a-request"></a>Intelligens kártya kiosztása egy kérelemhez
A megadott intelligens kártya kötése a megadott kéréshez. Intelligens kártya kötése után a kérés csak a megadott kártyával hajtható végre.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

### <a name="url-parameters"></a>URL-paraméterek
Nincsenek.

### <a name="request-headers"></a>Kérésfejlécek
A gyakori kérelmek fejléceit lásd: [http-kérelem és válasz-fejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *cm REST API szolgáltatás részleteiben* .

### <a name="request-body"></a>A kérés törzse
A kérelem törzse a következő tulajdonságokat tartalmazza:

Tulajdonság | Leírás
---------|-----------
kérelemazonosító | Az intelligens kártyához kötni kívánt kérelem azonosítója.
cardid | Az intelligens kártya CardID.
ATR | Az intelligens kártya válasz-visszaállítási (ATR) karakterlánca.


## <a name="response"></a>Reagálás
Ez a szakasz a választ ismerteti.

### <a name="response-codes"></a>Reagálási kódok

Kód  |Leírás  
---------|---------
201 | Létrehozva
204 | Nincs tartalom
403 | Forbidden
500 | Belső hiba

### <a name="response-headers"></a>Válaszfejlécek
Gyakori válasz-fejlécek esetén lásd: [http-kérés és-válasz fejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) *cm REST API szolgáltatás részletei* .

### <a name="response-body"></a>Választörzs
Sikeres művelet esetén egy URI-t ad vissza az újonnan létrehozott intelligenskártya-objektumhoz.

## <a name="example"></a>Példa
Ez a szakasz egy intelligens kártya kötésének példáját ismerteti.

### <a name="example-request"></a>Példa: kérelem

```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```

## <a name="see-also"></a>Lásd még

- [Microsoft. CLM. kiépítés. RequestOperations. CreateSmartcard metódus (karakterlánc, karakterlánc, kérelem)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
