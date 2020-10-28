---
title: Webszolgáltatás-összekötő REST API App Service minta | Microsoft Docs
description: Útmutató a példaként szolgáló REST JSON-kiszolgáló megvalósításához az Azure-ban
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/28/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: deb743fcbe4bdd155c1b0c4a31e24af0e8da8a4e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760835"
---
# <a name="web-service-connector-rest-api-app-service-sample"></a>Webszolgáltatás-összekötő REST API App Service minta

Ez a telepítési útmutató segítséget nyújt a minta REST JSON-kiszolgáló üzembe helyezéséhez az Azure-ban. Ezt a mintát használhatja a webszolgáltatás-összekötő konfigurálásának és megismerésének megkönnyítésére.

- A mintához a Microsoft Visual Studio 2017 szükséges.
- A példában szereplő natív modul Path (NMP) csomagjai ezt a [JSON-kiszolgálót](https://github.com/typicode/JSON-server) használják a githubon.
- Töltse le a [mintakódt](https://github.com/fimguy/SAMPLEREST) a githubról, és telepítse a mintakód Azure app Service.

## <a name="deploy-the-json-server-sample"></a>A JSON-kiszolgálói minta üzembe helyezése

1. A kód letöltése után nyissa meg a Solution fájlt a [Visual Studio 2017](https://www.visualstudio.com/downloads/)-ben.

2. A megoldás üzembe helyezéséhez válassza ki a projektet, majd kattintson a jobb gombbal, és válassza a **Közzététel** lehetőséget:

    ![A megoldás közzététele](media/microsoft-identity-manager-2016-ma-ws-restsample/publish-project.png)

3. Válassza ki a központi telepítéshez használandó App Service:

    ![Válassza ki a App Service](media/microsoft-identity-manager-2016-ma-ws-restsample/app-service.png)

4. Válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy új erőforráscsoportot:

    ![Válasszon erőforráscsoportot](media/microsoft-identity-manager-2016-ma-ws-restsample/resource-group.png)

5. Használja a App Service meglévő nevét, majd válassza a **Létrehozás** lehetőséget:

    ![Az App Service létrehozása](media/microsoft-identity-manager-2016-ma-ws-restsample/create.png)

    Létrejön a App Service.

6. A App Service közzétételéhez válassza a **Közzététel** lehetőséget:

    ![A App Service közzététele](media/microsoft-identity-manager-2016-ma-ws-restsample/publish.png)

7. A App Service közzétételét követően a REST API minta és a hozzá tartozó webhely az alapértelmezett böngészőben indul el:

    ![Példa REST API és webhelyre](media/microsoft-identity-manager-2016-ma-ws-restsample/sample-rest-api.png)

Most már beállíthatja a telepítést a [Rest telepítési útmutatóban](microsoft-identity-manager-2016-ma-ws-restgeneric.md)leírtak szerint.


## <a name="modify-the-sample"></a>A minta módosítása

A JSON-és a REST API-minta módosításához hajtsa végre a módosításokat a fájl **db.JS** , majd frissítse az üzemelő példányt:

![A fájl db.JSfrissítése](media/microsoft-identity-manager-2016-ma-ws-restsample/db-json.png)


## <a name="next-steps"></a>Következő lépések

- [Az általános webszolgáltatás-összekötő áttekintése](microsoft-identity-manager-2016-ma-ws.md)
- [A webszolgáltatás-konfigurációs eszköz telepítése](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP telepítési útmutató](microsoft-identity-manager-2016-ma-ws-soap.md)
- [REST telepítési útmutató](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Webszolgáltatás MA – konfiguráció](microsoft-identity-manager-2016-ma-ws-maconfig.md)
