---
title: Tanúsítványkezelő REST API szolgáltatás részletei | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a2dd84e121217772a8831653b2e4790436c32ec
ms.sourcegitcommit: c214bb0b1373b65b1c9c215379fd820ab0c13f0f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/14/2020
ms.locfileid: "92761002"
---
# <a name="certificate-management-rest-api-service-details"></a>Tanúsítványkezelő REST API szolgáltatás részletei
A következő szakaszok ismertetik a Microsoft Identity Manager (felügyeleti pont) tanúsítványkezelő (CM) REST API részleteit.

## <a name="architecture"></a>Architektúra 
MIM CM REST API hívásokat a vezérlők kezelik. A következő táblázat tartalmazza a vezérlők teljes listáját és azon környezet mintáit, amelyekben használhatók.


|                  Tartományvezérlő                   |                                                                                                                                                           Minta útvonal                                                                                                                                                           |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           CertificateDataController           |                                                                                                                                         /api/v1.0/requests/{requestid}/certificatedata/                                                                                                                                          |
| CertificateRequestGenerationOptionsController |                                                                                                                                                  /api/v1.0/requests/{requestid}                                                                                                                                                  |
|            CertificatesController             |                                                                                                                /api/v1.0/smartcards/{smartcardid}/certificates <br/> /api/v1.0/profiles/{profileid}/certificates                                                                                                                 |
|             OperationsController              |                                                                                                                  /api/v1.0/smartcards/{smartcardid}/operations <br/> /api/v1.0/profiles/{profileid}/operations                                                                                                                   |
|              PoliciesController               |                                                                                                                                   /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                                                   |
|              ProfilesController               |                                                                                                                                                     /api/v1.0/profiles/{id}                                                                                                                                                      |
|          ProfileTemplatesController           |                                                                                               /api/v1.0/profiletemplates/{id} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                |
|              RequestsController               |                                                                                                                                         /api/v1.0/requests/{id} <br/> /api/v1.0/requests                                                                                                                                         |
|             SmartcardsController              | /api/v1.0/requests/{requestid}/smartcards/{id}/diversifiedkey <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/serverproposedpin <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/authenticationresponse <br/> /api/v1.0/requests/{requestid}/smartcards/{id} <br/> /api/v1.0/smartcards/{id} <br/> /api/v1.0/smartcards |
|       SmartcardsConfigurationController       |                                                                                                                             /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards                                                                                                                              |

## <a name="http-request-and-response-headers"></a>HTTP-kérelem és-válasz fejlécei
Az API-nak küldött HTTP-kérelmeknek tartalmaznia kell a következő fejléceket (ez a lista nem teljes):

Fejléc | Leírás
-------|------------
Engedélyezés | Kötelező. A tartalom a hitelesítési módszertől függ. A metódus konfigurálható, és a Windows beépített hitelesítése (WIA) vagy Active Directory összevonási szolgáltatások (AD FS) (ADFS) alapján végezhető el.
Content-Type | Kötelező, ha a kérelem törzse van. Kell lennie `application/json` .
Content-Length | Kötelező, ha a kérelem törzse van. 
Cookie | A munkamenet cookie-je. A hitelesítési módszertől függően kötelező lehet.


A HTTP-válaszok a következő fejléceket tartalmazzák (ez a lista nem teljes):

Fejléc | Leírás
-------|------------
Content-Type | Az API mindig visszatér `application/json` .
Content-Length | A kérés törzsének hossza (ha van), bájtban megadva.


## <a name="api-versioning"></a>API-verziószámozás 
A CM REST API aktuális verziója 1,0. A verzió a szegmensben közvetlenül a következő `/api` URI-ban szereplő szegmenst követi: `/api/v1.0` . A verziószám megváltoztatja az API-felület jelentős módosításait.


## <a name="enable-the-api"></a>Az API engedélyezése 
A `<ClmConfiguration>` Web.config fájl szakasza új kulccsal bővült:

```
<add key="Clm.WebApi.Enabled" value="false" />
```

Ez a kulcs határozza meg, hogy a CM REST API elérhető-e az ügyfelek számára. Ha a kulcs "false" (hamis) értékre van állítva, a rendszer nem hajtja végre az API útvonal-hozzárendelését az alkalmazás indításakor. Az API-végpontokra irányuló további kérések egy HTTP 404 hibakódot adnak vissza. Alapértelmezés szerint a kulcs a "Letiltva" értékre van állítva.

>[!NOTE]
>Az érték "true" értékűre való módosítása után ne felejtse el futtatni az **IISReset parancsot** a kiszolgálón.

## <a name="enable-tracing-and-logging"></a>Nyomkövetés és naplózás engedélyezése 
A MIM CM REST API minden olyan HTTP-kérelem nyomkövetési adatát bocsátja ki, amelyet elküld. A nyomkövetési adatok részletességi szintjét a következő konfigurációs érték beállításával állíthatja be:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```

## <a name="error-handling-and-troubleshooting"></a>Hibakezelés és hibaelhárítás 
Ha kivételek történnek a kérelmek feldolgozásakor, a MIM CM REST API egy HTTP-állapotkódot ad vissza a webes ügyfélnek. Gyakori hibák esetén az API egy megfelelő HTTP-állapotkódot és egy hibakódot ad vissza. 

A kezeletlen kivételeket a rendszer a 500-as `HttpResponseException` http-állapotkód ("belső hiba") és az eseménynaplóban és a MIM cm nyomkövetési fájlban is nyomon követi. Minden kezeletlen kivételt az eseménynaplóba kell írni egy megfelelő korrelációs AZONOSÍTÓval. A korrelációs azonosítót a rendszer a hibaüzenetben szereplő API-felhasználónak is elküldi. A hiba megoldásához a rendszergazda az eseménynaplóban keresheti meg a megfelelő korrelációs azonosítót és a hiba részleteit.

>[!NOTE]
>A MIM CM REST API felhasználásának eredményeképpen létrejött, hibáknak megfelelő stack-nyomkövetéseket a rendszer nem küldi vissza az ügyfélnek. A rendszer biztonsági okokból nem küldi vissza a nyomkövetést az ügyfélnek.
