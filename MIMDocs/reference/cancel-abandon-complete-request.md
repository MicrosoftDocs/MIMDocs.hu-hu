---
title: Kérelem megszakítása, lemondása vagy befejezése | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2016
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e971309963968ddd52ef44644fb9319738df6242
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760492"
---
# <a name="cancel-abandon-or-complete-a-request"></a>Kérelem megszakítása, lemondása vagy befejezése
A (Microsoft Identity Manager) tanúsítványkezelő (CM) kérésének megjelölése befejezettként, megszakítva vagy elhagyva.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
PUT     |/CertificateManagement/api/v1.0/requests/{id}

### <a name="url-parameters"></a>URL-paraméterek

Tulajdonság| Leírás
---------|--------
id| Kötelező. A befejezési kérelem GUID azonosítója.


### <a name="request-headers"></a>Kérésfejlécek
A gyakori kérelmek fejléceit lásd: [http-kérelem és válasz-fejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *cm REST API szolgáltatás részleteiben* .

### <a name="request-body"></a>A kérés törzse
A kérelem törzse a következő tulajdonságokat tartalmazza:

Tulajdonság | Leírás
---------|-----------
status | Az az állapot, amelyre a kérést be kell állítani. A lehetséges értékek: "befejezett", "megszakított" vagy "elhagyott".


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
Sikeres művelet esetén egy [Microsoft. CLM. Shared. requests. Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) objektumot ad vissza. Az objektum a befejezettként megjelölt MIM CM kérelmet írja le. Az objektum a következő tulajdonságokat tartalmazza:

Név | Leírás
-----|------------
Megjegyzés | Az MIM CM kérelemhez társított Megjegyzés.
Befejezve | Az MIM CM kérés befejezésének időpontja.
Datacollection objektumot | Az MIM CM kérelemhez társított adatgyűjtési elemek.
DataCollectionFlags | Az MIM CM kérelem adatgyűjtési lehetőségei.
Zászlók | A MIM CM kérelemhez társított beállítások.
IsDataCollectionComplete | Logikai érték, amely azt jelzi, hogy az adatgyűjtés befejeződött-e az MIM CM kérelemhez.
IsEnrollmentAgent | Logikai érték, amely azt jelzi, hogy a MIM CM kérelem futtatásához szükség van-e egy regisztrációs ügynökre.
IsSmartcard | Logikai érték, amely azt jelzi, hogy az MIM CM kérelem intelligens kártyás kérelem vagy felhasználóiprofil-kérelem.
NewProfileUuid | Az MIM CM kérelem által létrehozott új felhasználóiprofil identitása.
NewSmartcardUuid | Az MIM CM kérelem által hozzárendelt új intelligens kártya identitása.
OldProfileUuid | Annak a szoftver-profilnak az identitása, amelyhez a MIM CM kérelmet létrehozták.
OldSmartcardUuid | Annak az intelligens kártyának az identitása, amelyhez a MIM CM kérelmet létrehozták.
OriginatorUserUuid | Az MIM CM-kérelmet kezdeményező felhasználó identitása.
Prioritás | Az MIM CM-kérelem prioritása.
ProfileTemplateUuid | Annak a profil-sablonnak az identitása, amelyhez a MIM CM kérelmet létrehozták.
RequestType | A MIM CM kérelem típusa.
SecurityDescriptor | Az MIM CM-kérelem biztonsági leírója.
status | Az MIM CM kérelem állapota.
Elküldve | Az MIM CM kérelem elküldésének időpontja.
TargetUserUuid | A MIM CM kérelemhez tartozó cél felhasználó identitása.
UUID | A MIM CM kérelem azonosítója.

## <a name="example"></a>Példa
Ez a szakasz egy példát mutat be a kérelem befejezettként való megjelölésére.

### <a name="example-request"></a>Példa: kérelem

```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    "status":"Completed"
}
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```

## <a name="see-also"></a>Lásd még

- [Microsoft.Clm.Provision.ExecuteOperations. Complete metódus](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft. CLM. Shared. requests. Request osztály](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
