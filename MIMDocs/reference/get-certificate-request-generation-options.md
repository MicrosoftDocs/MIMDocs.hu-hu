---
title: Tanúsítványkérelem-létrehozási beállítások beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d8baab788c07dfb8c009857b45e38eb662f98199
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760481"
---
# <a name="get-certificate-request-generation-options"></a>Tanúsítványkérelem-létrehozási beállítások beolvasása
Az ügyféloldali tanúsítványkérelem létrehozásához szükséges paramétereket kéri le.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

| Módszer |                                       URL-cím kérése                                        |
|--------|------------------------------------------------------------------------------------------|
|  GET   | /CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions |

### <a name="url-parameters"></a>URL-paraméterek

Paraméter | Leírás
--------|--------------
kérelemazonosító| Kötelező. Annak a MIM CM-kérésnek a GUID azonosítója, amelyhez a tanúsítványkérelem létrehozási paramétereit be kell olvasni.

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
A gyakori kérelmek fejléceit lásd: [http-kérelem és válasz-fejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *cm REST API szolgáltatás részleteiben* .

### <a name="response-body"></a>Választörzs
Sikeres művelet esetén a CertificateRequestGenerationOptions-objektumok listáját adja vissza. Minden CertificateRequestGenerationOptions objektum egyetlen tanúsítványkérelem, amelyet az ügyfélnek meg kell hoznia. Minden objektum a következő tulajdonságokkal rendelkezik:

Tulajdonság| Leírás
--------|-----------
Exportálható | Egy érték, amely megadja, hogy a kérelemhez létrehozott titkos kulcs exportálható-e.
FriendlyName | A regisztrált tanúsítvány megjelenítendő neve.
HashAlgorithmName | A tanúsítványkérelem aláírásának létrehozásakor használt kivonatoló algoritmus.
KeyAlgorithmName | A nyilvános kulcsú algoritmus.
KeyProtectionLevel | Az erős kulcsú védelem szintje.
KeySize | A létrehozandó titkos kulcs mérete (bit).
KeyStorageProviderNames | A titkos kulcs létrehozásához használható elfogadható kulcstároló-szolgáltatók (KSP-EK) listája. Ha az első KSP nem használható a tanúsítványkérelem létrehozásához, akkor a megadott KSP bármelyike felhasználható, amíg az egyik sikeres.
Kulcshasználat | A tanúsítványkérelem számára létrehozott titkos kulcs által végrehajtható művelet. Az alapértelmezett érték az aláírás.
Tárgy | A tulajdonos neve.

>[!NOTE]
>További információ ezekről a tulajdonságokról: [Windows. Security. kriptográfia. Certificates. CertificateRequestProperties osztály](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) leírása. Ne feledje, hogy az osztály és a CertificateRequestGenerationOptions objektumok között nincs egy az egyhez típusú levelezés.
>

## <a name="example"></a>Példa
Ez a szakasz egy példa arra, hogy beolvassa a tanúsítványkérelem generálási beállításait.

### <a name="example-request"></a>Példa: kérelem

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```  
