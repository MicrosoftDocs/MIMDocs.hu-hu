---
title: Profilbeállítások beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 62b4c336122376cd7da836b9ec6c6e09eac24729
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760445"
---
# <a name="get-profile-data"></a>Profilbeállítások beolvasása
Lekérdezi egy felhasználó szoftveres tanúsítvány-profiljainak listáját. A lista tartalmazza az aktuális felhasználó által végrehajtható lehetséges műveleteket. A rendszer ezután kezdeményezheti a kérelmek bármelyikét a megadott műveletekhez.

>[!IMPORTANT]
>A kiszolgáló csak akkor állítja be a PIN-kódot, ha a profil sablonjának házirendje azt jelzi, hogy el kell végezni. Ellenkező esetben a felhasználónak meg kell adnia a PIN-kódot.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/api/v1.0/requests/{requestid}/profiles

### <a name="url-parameters"></a>URL-paraméterek

Paraméter | Leírás
---------|------------
id | A visszaadni kívánt profil azonosítója (GUID).
Kérelemazonosító | Annak a kérésnek az azonosítója, amelyre a profilokat vissza kell adni.

### <a name="query-parameters"></a>Lekérdezési paraméterek

Paraméter | Leírás
---------|------------
status | Választható. Azokat a profilokat jelzi, amelyekhez az adatlekérdezést kéri. A lehetséges állapotok a következők: "Active", "jóváhagyva", "megszakítva", "befejezett", "tagadás", "végrehajtás", "sikertelen", "nincs" és "függő". <br/>Ha nincs megadva állapot, a rendszer az összes profilt visszaadja a visszaadott állapottól függetlenül.

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
A sikeres művelet során a a JSON-szerializált [Microsoft. CLM. Shared. profiles. Profiles](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) objektumok listáját adja vissza a következő tulajdonságokkal:

Tulajdonság | Leírás
---------|------------
AssignedUserUuid | Annak a felhasználónak az azonosítója, akihez a profil hozzá van rendelve.
Megjegyzés | A profilt leíró megjegyzés.
Zászlók | A profilt leíró jelzők.
ParentProfileUuid | A profil által lecserélt régi profil azonosítója.
PrimaryProfileUuid | Az elsődleges profil azonosítója.
ProfileOperations | Azoknak a lehetséges műveleteknek a listája, amelyeket a profil aktuális felhasználója végezhet el.
ProfileTemplateUuid | A profilt szabályozó házirendeket és beállításokat tartalmazó felhasználóiprofil-sablon azonosítója.
ProfileTemplateVersion | A profil sablonjának a profil létrehozásának időpontjában használt verziója.
status | A profil állapota.
UUID | A profil azonosítója.


## <a name="example"></a>Példa
Ez a szakasz egy példát mutat be egy felhasználó profiljának beolvasására.

### <a name="example-request"></a>Példa: kérelem

```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```

## <a name="see-also"></a>Lásd még

- [Microsoft. CLM. Shared. profiles. Profile osztály](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
