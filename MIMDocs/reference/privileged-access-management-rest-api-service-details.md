---
title: PAM REST API szolgáltatás részletei | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00a2f4d9c44747d50139655d368e42b11fbd388c
ms.sourcegitcommit: c214bb0b1373b65b1c9c215379fd820ab0c13f0f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/14/2020
ms.locfileid: "92760990"
---
# <a name="pam-rest-api-service-details"></a>PAM REST API szolgáltatás részletei
A következő részekben a Microsoft Identity Manager (a () Privileged Access Management (PAM) REST API részleteit tárgyaljuk.

<h2 id="http-request-and-response-headers">HTTP-kérelmek fejlécei</h2>

Az API-nak küldött HTTP-kérelmeknek tartalmaznia kell a következő fejléceket (ez a lista nem teljes):

Fejléc | Leírás
-------|------------
Engedélyezés | Kötelező. A tartalom a hitelesítési módszertől függ, amely konfigurálható, és a WIA (integrált Windows-hitelesítés) vagy ADFS-alapú lehet.
Content-Type | Kötelező, ha a kérelem törzse van. Értékre kell állítani `application/json` .
Content-Length | Kötelező, ha a kérelem törzse van. 
Cookie | A munkamenet cookie-je. A hitelesítési módszertől függően kötelező lehet.

## <a name="http-response-headers"></a>HTTP-válasz fejlécei

A HTTP-válaszoknak tartalmaznia kell a következő fejléceket (ez a lista nem teljes):

Fejléc | Leírás
-------|------------
Content-Type | Az API mindig visszatér `application/json` .
Content-Length | A kérés törzsének hossza (ha van), bájtban megadva.

## <a name="versioning"></a>Verziókezelés 
Az API jelenlegi verziója 1. Az API-verzió a kérelem URL-címében egy lekérdezési paraméterrel adható meg, ahogy az alábbi példában is látható: `http://localhost:8086/api/pamresources/pamrequests?v=1` Ha a kérelemben nincs megadva a verzió, a rendszer az API legutóbb kiadott verziójában hajtja végre a kérelmet. 

## <a name="security"></a>Biztonság 
Az API-hoz való hozzáféréshez integrált Windows-hitelesítés (IWA) szükséges. Ezt a konfigurációt az IIS-ben manuálisan kell konfigurálni, mielőtt Microsoft Identity Manager (webszolgáltatások) telepítését.

A HTTPS (TLS) támogatott, de manuálisan kell konfigurálni az IIS-ben. További információkért lásd: **SSL (SSL) implementálása a FIM-portálon** a [9. lépés: a FIM 2010 R2 telepítése utáni feladatok](https://technet.microsoft.com/library/hh322875.aspx) végrehajtása a FIM 2010 R2 tesztkörnyezet telepítése útmutatóban. 

Új SSL-kiszolgálói tanúsítvány létrehozásához futtassa a következő parancsot a Visual Studio parancssorába:

```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
A parancs létrehoz egy önaláírt tanúsítványt, amely az SSL-t használó webalkalmazások tesztelésére használható egy webkiszolgálón, amelyen az URL-cím található `test.cwap.com` . A beállítás által meghatározott OID `-eku` azonosítja a tanúsítványt SSL-kiszolgálói tanúsítványként. A tanúsítvány tárolása a **saját** tárolóban történik, és a számítógép szintjén érhető el. A tanúsítványt a mmc.exe Tanúsítványok beépülő moduljában exportálhatja.

## <a name="cross-domain-access-cors"></a>Tartományok közötti hozzáférés (CORS) 
A CORS támogatott, de manuálisan kell konfigurálni az IIS-ben. Adja hozzá a következő elemeket az üzembe helyezett API-web.config fájlhoz az API konfigurálásához a tartományok közötti hívások engedélyezéséhez: 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```

## <a name="error-handling"></a>Hibakezelés 
Az API a hibák feltételeit jelző HTTP-hibaüzeneteket ad vissza. A hibák OData megfelelőek. A következő táblázat az ügyfélnek visszaadott hibakódokat tartalmazza:

HTTP-állapotkód | Leírás
-----------------|------------
401 | Nem engedélyezett 
403 | Forbidden 
408 | Kérelem időtúllépése   
500 | Belső kiszolgálóhiba 
503 | A szolgáltatás nem érhető el 

## <a name="filtering"></a>Szűrés 
A PAM REST API kérelmek tartalmazhatnak szűrőket a válaszba foglalandó tulajdonságok megadásához. A szűrési szintaxis OData-kifejezéseken alapul.

A szűrők megadhatják a PAM-kérések tulajdonságait, a PAM-szerepköröket. vagy függőben lévő PAM-kérések. Például: *expirationtime tulajdonságok* , *DisplayName* vagy bármely más érvényes tulajdonsága egy PAM-kérelem, PAM-szerepkör vagy függőben lévő kérelem.

Az API a következő operátorokat támogatja a szűrési kifejezésekben: *és* , *EQUAL* , *NotEqual* , *GreaterThan* , *LessThan* , *GreaterThenOrEqueal* és *LessThanOrEqual* . 

A következő példák a szűrőket tartalmazzák:

- Ez a kérelem az adott dátumok közötti összes PAM-kérést visszaadja: `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime'2015-01-09T08:26:49.721Z' and ExpirationTime lt datetime'2015-02-10T08:26:49.722Z' `
 
- Ez a kérelem visszaadja a PAM szerepkört az "SQL file Access" megjelenítendő névvel: `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq 'SQL File Access' `
