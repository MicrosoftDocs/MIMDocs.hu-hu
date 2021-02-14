---
title: A PAM szoftverkövetelményei | Microsoft Docs
description: A Privileged Access Management sikeres üzembe helyezéséhez szükséges hardver- és szoftverkövetelmények
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/09/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8d3c266c78df21c6ddb24f62618621c55820bd6f
ms.sourcegitcommit: 0e2b4b47a8050737c78e3b0ad088358e5de7e929
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395452"
---
# <a name="hardware-and-software-requirements"></a>Hardver- és szoftverkövetelmények

Privileged Access Management a mögöttes szoftveres platformokra vonatkozó követelményeken felül nincs hardverkövetelmények. Ügyeljen rá, hogy rendelkezésre álljon elegendő memória vagy lemezterület, és hogy elérhető legyen a hálózati kapcsolat.

> [!IMPORTANT]
> Ez a cikk egy elszigetelt hálózaton található alapszintű telepítés minimális követelményeit ismerteti. Nem célja a teljesítmény, a skálázhatóság és a magas rendelkezésre állás bemutatása, és nem ismerteti a nagyvállalati vagy üzemi környezetben végzett telepítésekhez javasolt topológiát.  Ha a Active Directory egy internetkapcsolattal rendelkező környezet részét képezi, a további tudnivalókat lásd: az emelt [szintű hozzáférés biztonságossá tétele](/security/compass/overview) című útmutató.

## <a name="installing-from-software-packages"></a>Telepítés szoftvercsomagokból

A következő szoftver letölthető a TechNet Evaluation Center vagy az MSDN webhelyéről:

- Microsoft Identity Manager 2016
  - Szolgáltatás és portál: tartalmazza a MIM szolgáltatáshoz és a MIM-portálhoz, illetve a PAM-telepítéshez szükséges telepítőfájlokat
  - Beépülő modulok és bővítmények: tartalmazzák a kérelmező PowerShell-parancsmagok telepítőjét

A GitHubról a következő opcionális szoftvereket töltheti le:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): minta webalkalmazást tartalmaz a REST API

## <a name="required-software"></a>Szükséges szoftverek

- Windows Server 2016
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 vagy SQL Server 2014

## <a name="hardware-requirements"></a>Hardverkövetelmények

Minden PAM-összetevő esetében tekintse át a szoftverek rendszerkövetelményeit.

A CORPDC géphez:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) vagy újabb

A CORPWKSTN géphez:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

A PRIVDC géphez:

- Windows Server 2016

A PAMSRV géphez:

- Windows Server 2016
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) vagy [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
