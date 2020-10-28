---
title: A webszolgáltatások kitalálása eszköz telepítése | Microsoft Docs
description: Ez a cikk a webszolgáltatás-konfigurációs eszköz telepítésének lépéseit ismerteti.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 4b9d2463ea30839c2ea4e2a3427d057c925183e8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760540"
---
# <a name="install-the-web-service-configuration-tool"></a>A webszolgáltatás-konfigurációs eszköz telepítése

A webszolgáltatás-összekötő és az alapértelmezett projektek a [Microsoft letöltőközpontból](https://www.microsoft.com/en-us/download/details.aspx?id=51495)érhetők el.

A **webszolgáltatás-összekötő MSI** két szolgáltatást tesz elérhetővé:

- Webszolgáltatás-összekötő futtatókörnyezete: telepíti az alapszintű összekötőt, az összekötő függőségeit és a csomagolt összekötőt.
- Webszolgáltatás-konfigurációs eszköz: telepíti a webszolgáltatás konfigurációs eszközét.

![Telepítővarázsló-összekötő beállításai](media/microsoft-identity-manager-2016-ma-ws-install/connector-installation-options.png)

A konfigurációs eszköz a szinkronizálási szolgáltatás telepítése nélkül is telepíthető. Ez lehetővé teszi egy különálló számítógép konfigurálását.

## <a name="default-projects"></a>Alapértelmezett projektek

A webszolgáltatások összekötője további alapértelmezett projekteket is tartalmaz. Ezek az önkicsomagoló EXE-fájlokként érhetők el. A webszolgáltatás-összekötő projekt a követelménynek megfelelően tölthető le.

A telepítés befejezése után a bináris fájljait tartalmazó különböző összetevők a rendszeren az alábbi mappában vannak telepítve.

| Tartalom | Hely |
|---|---|
| Webszolgáltatás-összekötő futtatókörnyezete           | % Program Files% \\ Microsoft Forefront Identity Management \\ 2010 \\ szinkronizációs &nbsp; szolgáltatás \\ bővítmények |
| Webszolgáltatás-összekötő projekt           | % Program Files% \\ Microsoft Forefront Identity Management \\ 2010 \\ szinkronizációs &nbsp; szolgáltatás \\ bővítmények |
| Csomagolt összekötő                      | % Program Files% \\ Microsoft Forefront Identity Management \\ 2010 \\ szinkronizációs &nbsp; szolgáltatás \\ UIShell \\ kódokat \\ PackagedMAs |
| Webszolgáltatás-konfigurációs eszköz          | % Program Files% \\ Microsoft Forefront Identity Management \\ 2010 \\ szinkronizációs &nbsp; szolgáltatás \\ UIShell \\ webszolgáltatás &nbsp; &nbsp; konfigurációja <br/>**Megjegyzés** : ez az alapértelmezett telepítési hely. Ezt a helyet a telepítés során is módosíthatja. |
| Webszolgáltatás-projektfájl                | A felhasználó kijelölhet bármely célmappát a fájl kibontásához, de a kibontott projektfájl (. A WsConfig) csak akkor látható a FIM Sync felhasználói felületén, ha a projektfájlt Kinyeri a FIM **Extensions** mappába. A kibontott projektfájl bármely helyen látható a webszolgáltatás konfigurációs eszközén. |


## <a name="additional-permissions"></a>További engedélyek

A projektfájl bármely helyről menthető és megnyitható (a végrehajtó megfelelő hozzáférési jogosultságokkal); azonban csak a mappába mentett projektfájlok `Synchronization Service\Extension` választhatók ki a webszolgáltatás-összekötő varázslóban, amely a FIM Sync felhasználói felületén keresztül érhető el.

A webszolgáltatás konfigurációs eszközét futtató felhasználónak a következő jogosultságokkal kell rendelkeznie:

- Olvasási/írási engedélyek a szinkronizációs szolgáltatás bővítmény mappájához.
- Olvasási hozzáférés a következő beállításkulcs-kulcshoz: **HKLM \\ System \\ CurrentControlSet \\ Services \\ FIMSynchronizationService \\ Paraméterek** .
