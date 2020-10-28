---
title: Munkafolyamat-házirend beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: be636205-c1f0-457c-982e-e17478cf0889
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1928bfd4dff75b1484a8a232e41c742842fa01b9
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760600"
---
# <a name="get-workflow-policy"></a>Munkafolyamat-szabályzat beolvasása
Lekérdezi egy adott munkafolyamat profil-sablonjának házirendjét. A rendszer az adatkérések létrehozásakor használja fel az adatgyűjtést. A munkafolyamat-házirend határozza meg, hogy az ügyfél milyen típusú adatkérések létrehozásához szükséges. Az adatok tartalmazhatnak különböző adatgyűjtési elemeket, megjegyzéseket kérhetnek, és egy egyszeri jelszavas szabályzatot is.

>[!NOTE]
>A cikkben szereplő URL-címek az API telepítése során kiválasztott állomásnévhez képest, például: `https://api.contoso.com` .

## <a name="request"></a>Kérés

Módszer  |URL-cím kérése  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type}

### <a name="url-parameters"></a>URL-paraméterek

Paraméter| Leírás
--------|-------------
id| Kötelező. A profil sablonjának megfelelő GUID-azonosító, amelyet a szabályzat Kinyer.
típus| Kötelező. A kért szabályzat típusa. A lehetséges értékek a következők: "beléptetés", "duplikált", "OfflineUnblock", "OnlineUpdate", "megújítás", "helyreállítás", "RecoverOnBehalf", "Recover", "Visszavonás", "Visszavonás", "TemporaryEnroll" és "Feloldás".

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
403 | Forbidden
204 | Nincs tartalom
500 | Belső hiba

### <a name="response-headers"></a>Válaszfejlécek
Gyakori válasz-fejlécek esetén lásd: [http-kérés és-válasz fejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) *cm REST API szolgáltatás részletei* .

### <a name="response-body"></a>Választörzs
Sikeres művelet esetén egy [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) objektumon alapuló házirend-objektumot ad vissza. A házirend-objektum legalább a következő táblázatban szereplő tulajdonságokat tartalmazza, de a kért házirendtől függően további tulajdonságokat is tartalmazhat. Egy igénylési házirendre vonatkozó kérelem például egy [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy.aspx) objektumot ad vissza. További információkért tekintse meg a kérelemben a (z) {Type} paraméterhez társított Policy objektum dokumentációját. A különböző típusú házirend-objektumok dokumentációja a [Microsoft. CLM. Shared. ProfileTemplates névtér](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx) dokumentációjában található.

Tulajdonság | Leírás
---------|------------
ApprovalsNeeded | Azon jóváhagyások száma, amelyek szükségesek a Forefront Identity Manager (FIM) tanúsítványkezelő (CM) kérelmekhez a házirendhez.
AuthorizedApprover | A Szabályzathoz tartozó FIM CM-kérelmek jóváhagyására jogosult felhasználók biztonsági leírója.
AuthorizedEnrollmentAgent | Azon felhasználók biztonsági leírója, akik regisztrációs ügynökként működhetnek a házirendhez.
AuthorizedInitiator | Azon felhasználók biztonsági leírója, akik FIM CM-kérelmeket indíthatnak a szabályzathoz.
CollectComments | Logikai érték, amely azt jelzi, hogy engedélyezve van-e a Megjegyzés-gyűjtemény a Szabályzathoz tartozó FIM CM-kérelmek esetében.
CollectRequestPriority | Logikai érték, amely azt jelzi, hogy engedélyezve van-e a kérelem prioritási gyűjteménye a Szabályzathoz tartozó FIM CM-kérelmek esetében.
DefaultRequestPriority | A házirendhez tartozó FIM CM-kérelmek alapértelmezett prioritása.
Dokumentumok | A házirendhez konfigurált házirend-dokumentumok.
Engedélyezve | Logikai érték, amely azt jelzi, hogy a házirend engedélyezve van-e.
EnrollAgentRequired | Logikai érték, amely azt jelzi, hogy a Szabályzathoz tartozó FIM CM-kérelmek esetében beléptetési ügynökök szükségesek-e.
OneTimePasswordPolicy | A házirendhez tartozó FIM CM-kérelmek egyszeri jelszavainak terjesztési módszere.
Személyre szabás | Az intelligens kártya személyre szabási beállításai a szabályzathoz.
PolicyDataCollection | A Szabályzathoz társított adatgyűjtési elemek.
SelfServiceEnabled | Logikai érték, amely azt jelzi, hogy engedélyezve van-e a Szabályzathoz tartozó FIM CM-kérelmek önkiszolgáló kezdeményezése.

## <a name="example"></a>Példa
Ez a szakasz egy példa arra, hogy beolvassa a profil sablonjának házirendjét egy munkafolyamathoz. 

### <a name="example-request-1"></a>Példa: 1. kérelem

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```

### <a name="example-response-1"></a>Példa: 1. Válasz

```
HTTP/1.1 200 OK

... body coming soon
```       

### <a name="example-request-2"></a>Példa: 2. kérelem

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```

### <a name="example-response-2"></a>Példa: 2. Válasz

```
HTTP/1.1 200 OK

... body coming soon
```       

## <a name="see-also"></a>Lásd még

- [Microsoft. CLM. Shared. ProfileTemplates. ProfileTemplatePolicy osztály](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Microsoft. CLM. Shared. ProfileTemplates névtér](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
