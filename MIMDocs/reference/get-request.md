---
title: Get kérelem | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1bc42eb0fb1e54a3425586350ae5ad20495534c5
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835705"
---
# <a name="get-request"></a>Kérelem kérése
Egy vagy több megadott Microsoft Identity Manager (felügyeleti pont) tanúsítvány-kezelési (CM) kérelem beolvasása.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Metódus  |URL-cím kérése  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{id}

### <a name="url-parameters"></a>URL-paraméterek

Tulajdonság| Leírás
---------|--------
id| Választható. A lekérdezni kívánt kérelem GUID azonosítója.

### <a name="query-parameters"></a>Lekérdezési paraméterek

Tulajdonság| Leírás
---------|--------
targetuser| Választható. A kérelem céljának felhasználója. Ha nincs megadva cél, a rendszer az összes kérelmet a célként megadott felhasználótól függetlenül visszaadja. <br/><br/>**Megjegyzés**: jelenleg csak a "current" érték támogatott.
status| Választható. A lekérdezni kívánt kérelem állapotát jelzi. A lehetséges állapotok a következők: "jóváhagyva", "megszakítva", "befejezett", "tagadás", "végrehajtás", "sikertelen", "nincs" és "függő". <br/>Ha nem ad meg állapotot, a rendszer az összes kérelmet visszaadja a visszaadott állapottól függetlenül.

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
Sikeres művelet esetén egy vagy több [kérelem](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx) -objektumot ad vissza a következő tulajdonságokkal:

Név | Leírás
-----|------------
Megjegyzés | Az MIM CM kérelemhez társított Megjegyzés.
Befejeződött | Az MIM CM kérés befejezésének időpontja.
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
Állapot | Az MIM CM kérelem állapota.
Elküldve | Az MIM CM kérelem elküldésének időpontja.
TargetUserUuid | A MIM CM kérelemhez tartozó cél felhasználó identitása.
UUID | A MIM CM kérelem azonosítója.

## <a name="example"></a>Példa
Ez a szakasz egy példát mutat be a megadott MIM CM kérelmek beszerzésére.

### <a name="example-request-1"></a>Példa: 1. kérelem

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1
```

### <a name="example-response-1"></a>Példa: 1. Válasz

```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":3,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"00000000-0000-0000-0000-000000000000",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"00000000-0000-0000-0000-000000000000",
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

### <a name="example-request-2"></a>Példa: 2. kérelem

```
GET /certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1
```

### <a name="example-response-2"></a>Példa: 2. Válasz

```
HTTP/1.1 200 OK

{
    "Uuid":"0c96d73f-967b-420e-854a-43ad2a1504bc",
    "RequestType":5,
    "Status":3,
    "Flags":2,
    "DataCollection":[
        {
            "Name":"Sample Data Item",
            "Value":null
        }
    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:40:33.133Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```     

## <a name="see-also"></a>Lásd még

- [Microsoft. CLM. kiépítés. FindOperations. FindRequest metódus](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests.aspx)
- [Microsoft. CLM. Shared. RequestPermission enumerálás](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requestpermission.aspx)
- [Microsoft. CLM. Shared. requests. Request osztály](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)
