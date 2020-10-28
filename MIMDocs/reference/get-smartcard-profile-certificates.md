---
title: Intelligens kártya vagy profil tanúsítványának beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 206bde70-4af8-46fa-9c0b-f574745b0977
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e089caa78dd9a52517f2cae3fa42176225c73b5
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760396"
---
# <a name="get-smart-card-or-profile-certificates"></a>Intelligens kártya vagy profil tanúsítványának beolvasása
Lekéri egy adott intelligens kártyához vagy szoftver-profilhoz társított tanúsítványok listáját.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/certificates <br/>/CertificateManagement/api/v1.0/smartcards/{id}/certificates

### <a name="url-parameters"></a>URL-paraméterek

Paraméter | Leírás
---------|------------
id | A profil vagy az intelligens kártya azonosítója (GUID).

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
Sikeres művelet esetén a a JSON-szerializált [Microsoft. CLM. Shared. Certificates. X509ClmCertificate](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx) objektumok listáját adja vissza a következő tulajdonságokkal:

Név | Leírás
-----|------------
ArchivedOnCa | Logikai érték, amely azt jelzi, hogy a tanúsítvány archiválva van-e a hitelesítésszolgáltatón (CA).
CertificateType | A tanúsítvány típusa.
IsKeyHistory | Logikai érték, amely azt jelzi, hogy a tanúsítvány kulcsfontosságú előzmény-tanúsítvány-e.
Sorozatszám | A tanúsítvány sorozatszáma.
TemplateCommonName | A tanúsítvány köznapi neve.
Ujjlenyomat | A Tanúsítvány ujjlenyomata.

## <a name="example"></a>Példa
Ez a szakasz egy intelligens kártyához vagy szoftver-profilhoz társított tanúsítványok beszerzésére mutat példát.

### <a name="example-request"></a>Példa: kérelem

```
GET /certificatemanagement/api/v1.0/smartcards/5badfea3-de31-4837-99f9-8249515a5473/certificates HTTP/1.1
```

### <a name="example-response"></a>Példa: válasz

```
HTTP/1.1 200 OK

[
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01052AFA01313FB77AC00010000B010",
        "Thumbprint":"C52B0C5FB8AAD31A5B239FF2712ED14122D67D30"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B011AB48AE7D664ED5D900010000B011",
        "Thumbprint":"E7C4324896271BE869544FF28AA2B1BF3B3BDFCF"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01ABCF2D38A0CCEAD8F00010000B01A",
        "Thumbprint":"D9293B5414C644888444541B64631E90F2612425"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B1F02865CF2C3A9DD96D00010000B1F0",
        "Thumbprint":"6615DDC8603DBA789D724502627682F37D0FC2D0"
    }
]
```       

## <a name="see-also"></a>Lásd még

- [Microsoft. CLM. kiépítés. FindOperations. FindCertificates metódus](https://msdn.microsoft.com/library/microsoft.clm.provision.findoperations.findcertificates.aspx)
- [Microsoft. CLM. Shared. Certificates. X509ClmCertificate osztály](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx)
