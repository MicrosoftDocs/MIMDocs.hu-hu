---
title: A minta beléptetési útmutatója | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5a560e68ea820211caa1dd0a40a0143f44d5f7fb
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760378"
---
# <a name="sample-enrollment-walkthrough"></a>Példa a beléptetési útmutatóra
Ez a cikk a virtuális intelligens kártya önkiszolgáló regisztrációjának végrehajtásához szükséges lépéseket ismerteti. Egy automatikusan jóváhagyott kérelmet jelenít meg egy felhasználó által beállított PIN-kóddal.

1. Az ügyfél hitelesíti a felhasználót, majd lekéri azon profil-sablonok listáját, amelyeket a hitelesített felhasználó regisztrálhat:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    
2. Az ügyfél megjeleníti az eredményül kapott listát a felhasználó számára. A felhasználó kiválasztja a "virtuális intelligens kártya VPN" nevű vSC-profilt (Virtual Smart Card), valamint az UUID-t `97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1` .

3. Az ügyfél az előző lépésben visszaadott UUID használatával kérdezi le a kiválasztott felhasználóiprofil-sablon beléptetési szabályzatát:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```

4. Az ügyfél elemzi a visszaadott szabályzatban szereplő **datacollection objektumot** mezőt, és megállapítja, hogy egy "kérés oka" nevű egyetlen adatgyűjtő elem jelenik meg. Az ügyfél emellett azt is megállapítja, hogy a **collectComments** jelző értéke false (hamis), így nem kéri a felhasználót, hogy adjon meg semmilyen értéket.

5. Miután a felhasználó megadta a tanúsítványok megkövetelésének okát, az ügyfél létrehoz egy kérelmet:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```

6. A kiszolgáló sikeresen létrehozta a kérést, és visszaadja a kérelem URI-JÁT az ügyfélnek:  `/CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5` .

7. Az ügyfél lekéri a kérés objektumot a visszaadott URI meghívásával:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```

8. Az ügyfél ellenőrzi, hogy a kérelemben szereplő **State** tulajdonság jóváhagyva értékre van-e állítva. A kérelem végrehajtása megkezdődhet.

9. Az ügyfél megvizsgálja a kérést, hogy van-e olyan intelligens kártya, amely már társítva van a kérelemhez a **newsmartcarduuid** paraméter tartalmának elemzésével.

10. Mivel csak üres GUID-azonosítót tartalmaz, az ügyfélnek a MIM CM által még nem használt meglévő kártyát kell használnia, vagy létre kell hoznia egyet, ha a profil sablonja virtuális intelligens kártyákhoz van konfigurálva.

11. Mivel az utóbbit a beléptetésre alkalmas profilok kezdeti lekérdezése (1. lépés) alapján jelezte az ügyfélnek, az ügyfélnek most létre kell hoznia egy virtuális intelligens kártyás eszközt.

12. Az ügyfél lekéri az intelligens kártya házirendjét a profil sablonból. A 3. lépésben kiválasztott sablon UUID-azonosítóját használja:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```

13. Az intelligens kártya házirendje tartalmazza a kártyához tartozó alapértelmezett rendszergazdai kulcsot a **DefaultAdminKeyHex** tulajdonságban. Az intelligens kártya létrehozásakor az ügyfélnek be kell állítania az intelligens kártya kezdeti rendszergazdai kulcsát erre a kulcsra.  
14. Az intelligens kártyás eszköz létrehozásakor az ügyfélnek hozzá kell rendelnie a kéréshez:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```

15. A kiszolgáló válaszol az ügyfélre egy URI-val az újonnan létrehozott intelligenskártya-objektumhoz: `api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644` .

16. Az ügyfél lekéri az intelligens kártyás objektumot:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```

17. Ha ellenőrzi a **diversifyadminkey** jelző értékét a 12. lépésben beszerzett intelligenskártya-házirendben, az ügyfél tudja, hogy diverzifikálja a felügyeleti kulcsot.

18. Az ügyfél lekéri a javasolt rendszergazdai kulcsot:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```

19. Az ügyfélnek rendszergazdai jogosultsággal kell rendelkeznie a kártyához a rendszergazdai kulcs megadásához. Ehhez az ügyfél hitelesítési kihívást kér az intelligens kártyától, és elküldi azt a kiszolgálónak:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    A kiszolgáló a kérdéses választ a HTTP-válasz törzsében küldi el. Keresse meg a hexadecimális Challenge karakterláncot, konvertálja Base64 értékre, és adja át paraméterként az URL-címben. Az URL-cím egy másik választ ad vissza, amelyet vissza kell alakítani hexadecimális formátumra.

20. A kiszolgáló elküldi a kérdéses választ a HTTP-válasz törzsében, és az ügyfél ezt használja a rendszergazdai kulcs változatossá tételéhez.

21. Az ügyfél megállapítja, hogy a felhasználónak meg kell adnia a kívánt PIN-kódját az intelligens kártya házirendjének **UserPinOption** mezőjének vizsgálatával.

22. Miután a felhasználó megadta a kívánt PIN-kódot egy párbeszédpanelen, az ügyfél egy kérdés-válasz típusú hitelesítést hajt végre a 19. lépésben leírtak szerint, és az egyetlen különbség az, hogy a diverzifikált jelzőt igaz értékre kell állítani:

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```

23. Az ügyfél a kiszolgálótól kapott választ használva állítja be a kívánt felhasználói PIN-kódot.

24. Az ügyfél most már készen áll a tanúsítványkérelmek létrehozására. Lekérdezi a profil sablon tanúsítvány-létrehozási paramétereit a kulcsok/kérelmek generálási módjának meghatározásához.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```

25. A kiszolgáló egyetlen JSON-szerializált **CertificateRequestGenerationOptions** -objektummal válaszol. Az objektum tartalmazza a kulcs exportálásának paramétereit, a tanúsítvány rövid nevét, a kivonatolási algoritmust, a kulcs algoritmusát, a kulcs méretét és így tovább. Az ügyfél ezeket a paramétereket használja a tanúsítványkérelem létrehozásához.

26. A rendszer elküld egy tanúsítványkérelmet a kiszolgálónak. Az ügyfél emellett olyan PFX-jelszót is megadhat, amelyet a PFX-Blobok visszafejtéséhez kell használni, amikor a tanúsítványsablon megadja a tanúsítványoknak a HITELESÍTÉSSZOLGÁLTATÓN történő archiválását, azaz a HITELESÍTÉSSZOLGÁLTATÓ létrehozza a kulcspárt, majd elküldi azt az ügyfélnek. Előfordulhat, hogy az ügyfél megjegyzéseket is felvesz.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```

27. Néhány másodperc elteltével a kiszolgáló egyetlen, JSON-szerializált **Microsoft. CLM. Shared. Certificate** objektummal válaszol. A **isPkcs7** jelző ellenőrzésével az ügyfél megtudhatja, hogy ez a válasz nem pfx-blob. Az ügyfél kibontja a blobot Base64 kódolással a PKCS7 karakterlánc használatával, és telepíti azt.

28. Az ügyfél befejezettként jelöli meg a kérést.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
