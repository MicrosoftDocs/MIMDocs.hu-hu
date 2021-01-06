---
title: Identity Manager BHOLD verziótörténete | Microsoft Docs
description: Ez a cikk a BHOLD frissítéseinek részeként végrehajtott különféle módosításokat dokumentálja a következő dokumentumokon belül 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/1/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 4e3c247370c6c78e7814d894e9f0a6166d2e397a
ms.sourcegitcommit: 8f81767ec92e1b80658aaebb9463aa4d62396d43
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97927608"
---
# <a name="bhold-modules-version-release-history"></a>BHOLD-modulok verziójának korábbi kiadásai

A Microsoft Identity Manager csapat rendszeresen kiad a korábbi [verziókban](version-history.md) felsorolt frissítéseket. Ebből a cikkből megtudhatja, hogy nyomon követheti-e a BHOLD számára kiadott verziókat, a Microsoft Identity Manager alösszetevőjét. Ezután eldöntheti, hogy frissítenie kell-e a legújabb verzióra.

## <a name="version-60620"></a>6.0.62.0 verziója

- Állapot: október 2018
- [Letöltés](https://www.microsoft.com/download/details.aspx?id=55950)

## <a name="version-60360"></a>6.0.36.0 verziója

- Állapot: 2017. szeptember 7.

### <a name="enhancements"></a>Fejlesztések 
A BHOLD verziójának 6.0.36.0 a következő fejlesztéseket tartalmazza:

- Platform frissítése x86-ról x64-re.

### <a name="fixed-issues"></a>Megoldott problémák
A BHOLD-verzió 6.0.36.0 a következő problémákat állapította meg.

#### <a name="bhold-core"></a>BHOLD mag

- A telepítő frissített az x86-ról x64-re.
- A webes felület nem jeleníti meg az összes engedélyt.
- Javítások a B1Common-könyvtárhoz.
- A felhasználó nem tudja hozzárendelni a OrgUnit szerepkört, ha a szerepkörnek van néhány jogosultsága.
- A maximális jogosultságok nem használhatók. felhasználók száma.

#### <a name="access-management-connector"></a>Hozzáférés-kezelési összekötő

- n/a

#### <a name="bhold-integration-module"></a>BHOLD integrációs modul

- A telepítő frissített az x86-ról x64-re.
- Az ELRENDEZÉSi mappa helytelen helye.

#### <a name="bhold-model-generator"></a>BHOLD Model Generator

- A fölérendelt részlegek modelljeinek modellezése nem örökölt.

#### <a name="bhold-analytics"></a>BHOLD-elemzés

- n/a

#### <a name="bhold-attestation"></a>BHOLD igazolása

- n/a

#### <a name="bhold-reporting"></a>BHOLD-jelentéskészítés

- Az "Alkalmazottkód" egyéni attribútum nem érhető el.
- Nem sikerült betölteni a nagyméretű adatmennyiséget a 3 fájlos készlet használatával.

## <a name="next-steps"></a>Következő lépések

- [Útmutató a BHOLD-fogalmakhoz](../bhold/bhold-concepts-guide.md)
- [BHOLD telepítési útmutató](../bhold/bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](mim2016-bhold-developer-reference.md)
- [A MIM korábbi verziói](version-history.md)

