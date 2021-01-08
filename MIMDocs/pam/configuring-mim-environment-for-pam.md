---
title: A MIM 2016 konfigurálása a Privileged Access Management szolgáltatás használatához | Microsoft Docs
description: Ütemterv a MIM telepítéséhez és konfigurálásához a Privileged Access Management szolgáltatáshoz.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 64ed3184ee38102f488c61bf0b41bc0fec34b049
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010607"
---
# <a name="configure-the-mim-environment-for-privileged-access-management"></a>A MIM-környezet konfigurálása a Privileged Access Management szolgáltatáshoz

Az erdők közötti hozzáférés beállítását, az Active Directory és a Microsoft Identity Manager telepítését és konfigurálását, illetve a szükséges időben való hozzáférésre irányuló kérelem bemutatását hét lépésben hajthatja végre.

Ezek a lépések úgy vannak felépítve, hogy segítségükkel a tesztkörnyezetet az alapoktól tudja elkezdeni és felépíteni. Ha a PAM-t egy meglévő környezetre alkalmazza, használhatja a *contoso* tartományhoz tartozó saját tartományvezérlőket vagy felhasználói fiókokat ahelyett, hogy újakat hozna létre a példáknak megfelelően.

1. Készítse elő a *CORPDC* kiszolgálót tartományvezérlőként, a *CORPWKSTN* gépet pedig tag munkaállomásként történő használatra.

2. Készítse elő a *PRIVDC* kiszolgálót tartományvezérlőként történő használatra.

3.  Készítse elő a *PAMSRV* kiszolgálót a *PRIV* erdőben.

4.  Telepítse a MIM-összetevőket a *PAMSRV* kiszolgálóra, illetve a parancsmagokat egy *CONTOSO* erdőbeli tag munkaállomásra, és készítse elő őket a Privileged Access Management szolgáltatáshoz.

5.  Hozzon létre megbízható kapcsolatot a *PRIV* és a *CONTOSO* erdő között.

6.  Védett erőforrásokhoz és tagfiókokhoz hozzáféréssel rendelkező privilegizált biztonsági csoportok előkészítése a szükséges időben történő (just-in-time) felügyelet elvére épülő Privileged Access Management szolgáltatásra.

7.  Védett erőforrásokhoz való privilegizált, emelt szintű hozzáférés kérelmezésének, fogadásának és felhasználásának bemutatása.

> [!div class="step-by-step"]
> [Indítás »](step-1-prepare-corp-domain.md)
