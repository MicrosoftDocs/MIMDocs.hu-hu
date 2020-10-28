---
title: Intelligens kártya szabályzatának beolvasása | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e8e8b2ac7dcf8f72d4989d458272d78b8367d923
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760613"
---
# <a name="get-smart-card-policy"></a>Intelligens kártya szabályzatának beolvasása
Beolvassa a profil sablonjának házirendjét a megadott munkafolyamathoz. Ezeket az adatkéréseket a rendszer a kérelem létrehozásakor használja. A munkafolyamat-házirend határozza meg, hogy az ügyfél milyen típusú adatkérések létrehozásához szükséges. Az adatok tartalmazhatnak különböző adatgyűjtési elemeket, megjegyzéseket kérhetnek, és egy egyszeri jelszavas szabályzatot is.

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
típus| Kötelező. A kért házirend típusa. A lehetséges értékek a következők: "beléptetés", "duplikált", "OfflineUnblock", "OnlineUpdate", "megújítani", "helyreállítás", "RecoverOnBehalf", "Recover", "Visszavonás", "Visszavonás", "TemporaryEnroll" és "Feloldás".

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
Sikeres művelet esetén egy [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) objektumon alapuló házirend-objektumot ad vissza. A házirend-objektum legalább a következő táblázatban szereplő tulajdonságokat tartalmazza, de a kért házirendtől függően további tulajdonságokat is tartalmazhat. Egy igénylési házirendre vonatkozó kérelem például egy [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy) objektumot ad vissza. További információkért tekintse meg a kérelemben a (z) {Type} paraméterhez társított Policy objektum dokumentációját. A különböző típusú házirend-objektumok dokumentációja a [Microsoft. CLM. Shared. ProfileTemplates névtér](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates) dokumentációjában található.

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
