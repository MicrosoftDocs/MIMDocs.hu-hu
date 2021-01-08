---
title: A kiépítési rendszer PAM-telepítési parancsfájljai
description: Ez a lap a Microsoft Identity Manager parancsfájlok használatával történő konfigurálásával kapcsolatos cikkek sorozatának részét képezi. Tartalma a környezettel kapcsolatos előfeltételeket ismerteti.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: e7eea2c72df3ca9893acc5f6989afa9c488f384e
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010455"
---
# <a name="mim-pam-deployment-scripts"></a>A kiépítési rendszer PAM-telepítési parancsfájljai

A 2016-es szervizcsomaggal elvégezhető Service Pack 1 üzembe helyezési parancsfájlokat tartalmaz, amelyek megkönnyítik a PAM üzembe helyezését. Ezek a parancsfájlok a letöltőközpontban érhetők el. A parancsfájlok használatának megkísérlése előtt fontos, hogy megfeleljen az alábbi követelményeknek:

1. Az operációs rendszer minden kiszolgálón legalább Windows Server 2012 R2.
2. A DNS-t úgy kell konfigurálni, hogy engedélyezze a névfeloldást a tartományvezérlők és az összetevő-kiszolgálók között.
3. A telepítő bináris fájloknak helyben elérhetőnek kell lenniük a kijelölt kiszolgálókon az SQL, a SharePoint és a MIM telepítéséhez.
4. A környezet három dedikált (fizikai vagy virtuális) gépet tartalmaz, amelyeken külön fut a CORPDC, a PRIVDC és a PAMSERVER.
5. Az érvényesítési beállításhoz dedikált munkaállomásra van szükség.

>[!NOTE]
>Amennyiben bármi probléma lépne a szkriptek végrehajtása során, érdemes áttekintenie a naplókat. A szkriptek naplói mind az %AppData%\MIMPAMInstall helyen vannak tárolva. Tömörítse a mappát egy Zip-fájlba, és küldje el a művelet és a hiba részleteivel együtt a támogatási eset részeként.

Készen áll a PAM üzembehelyezési szkriptek használatára? Kezdje [A PAM konfigurálása szkriptek használatával](./pam/sp1-pam-configure-using-scripts.md) lépéssel.
