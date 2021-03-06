---
title: Intelligens kártya profiljainak beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 605dfe1359b5706b27682a086f039f53a5ddaa05
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835688"
---
# <a name="get-smart-card-profiles"></a>Intelligens kártya profiljainak beolvasása
Lekéri egy felhasználó intelligens kártyás profiljainak listáját. A lista tartalmazza az aktuális felhasználó által végrehajtható lehetséges műveleteket. A rendszer ezután kezdeményezheti a kérelmek bármelyikét a megadott műveletekhez.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Metódus  |URL-cím kérése  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards <br/> /CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


### <a name="url-parameters"></a>URL-paraméterek

Tulajdonság| Leírás
---------|--------
smartcarduuid | Választható. Az intelligens kártyás UUID-t Microsoft Identity Manager (felügyeleti pont) tanúsítványkezelő (CM) jelzi. Az érték a [Microsoft. CLM. Shared. smartcards. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objektum "UUID" mezőjének felel meg.

### <a name="query-parameters"></a>Lekérdezési paraméterek

Tulajdonság| Leírás
---------|--------
cardid | Választható. Az intelligens kártya UUID-je MIM CM szerint kijelölve. Az érték a [Microsoft. CLM. Shared. smartcards. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objektum "UUID" mezőjének felel meg.

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
Sikeres művelet esetén egy JSON-Serialized [Microsoft. CLM. Shared. smartcards. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objektumot ad vissza, amely a következő tulajdonságokkal rendelkezik:

Név | Leírás
-----|-----------
AssignedUserUuid | Annak a felhasználónak az azonosítója, akihez az intelligens kártya hozzá van rendelve.
ATR | A jelenleg inicializált kártyához tartozó intelligens kártya válasz-visszaállítási (ATR) karakterlánca.
Megjegyzés | Az intelligens kártyát leíró megjegyzés.
Zászlók | Az intelligens kártyát leíró jelzők.
Köztes szoftverek | Az intelligens kártya middleware-je.
ParentSmartcardUuid | Az intelligens kártya által lecserélt régi intelligens kártya azonosítója.
PermanentSmartcardUuid | Az intelligens kártyához társított állandó intelligens kártya azonosítója.
PrimarySmartcardUuid | Az elsődleges intelligens kártya azonosítója.
ProfileTemplateUuid | A profil sablonjának azonosítója, amely az intelligens kártyát szabályozó házirendeket és beállításokat tartalmazza.
ProfileTemplateVersion | A profil sablonjának verziója az intelligens kártya profiljának létrehozásakor.
Sorozatszám | Az intelligens kártya sorozatszáma.
Állapot | Az intelligens kártya állapota.
UUID | Az intelligens kártya profiljának azonosítója.

## <a name="example"></a>Példa
Ez a szakasz egy példát mutat be a felhasználók intelligens kártyás profiljainak beolvasására.

### <a name="example-request-1"></a>Példa: 1. kérelem

```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1
```

### <a name="example-response-1"></a>Példa: 1. Válasz

```
HTTP/1.1 200 OK

{
    "Uuid":"438d1b30-f3b4-4bed-85fa-285e08605ba7",
    "Status":3,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{d1ef6869-5517-42a0-8f05-16ca1a0b834d}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       

### <a name="example-request-2"></a>Példa: 2. kérelem

```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```

### <a name="example-response-2"></a>Példa: 2. Válasz

```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":2,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```     

## <a name="see-also"></a>Lásd még

- [Microsoft. CLM. Shared. Smartcards. intelligenskártya-osztály](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
