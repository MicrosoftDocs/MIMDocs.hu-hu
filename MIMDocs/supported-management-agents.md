---
title: Támogatott összekötők | Microsoft Docs
description: Összekötők használatával kezelheti a felügyeleti csomag és a csatlakoztatott adatforrások közötti adatátvitelt.
keywords: ''
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
author: EugeneSergeev
manager: daveba
editor: ''
reviewer: markwahl-msft
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/23/2019
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 157fd8d2a6b4296899f90c661e12ba6e19743d0f
ms.sourcegitcommit: 22fa4dac943a0c6b0815b711bd1996f77a390e7c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92174527"
---
# <a name="connect-to-your-directories"></a>Csatlakozás a címtárakhoz

Az összekötők az adott csatlakoztatott adatforrásokhoz kapcsolódnak Microsoft Identity Manager SP1 (a webszolgáltatáshoz). A csatlakoztatott adatforrásból az összekötő adatokat helyez át az MIM alkalmazásba. Ha az MIM alkalmazásban az adatok módosulnak, az összekötő exportálja az adatokat a csatlakoztatott adatforráshoz, hogy szinkronizálja azt az MIM alkalmazással. Általában elmondható, hogy minden csatlakoztatott címtárhoz legalább egy összekötő tartozik.

A Forefront Identity Manager szoftver az összekötőket kezelőügynöknek nevezte. Ez a megnevezés továbbra is használatos néhány cikkben és a termék egyes részeiben, de mindkét kifejezés ugyanazt a fogalmat fedi.

Ez a cikk & azokat az összekötőket ismerteti, amelyek a 2,0-as verzióban támogatottak, de az bővíthető kapcsolat-es összekötője lehetővé teszi, hogy még több adatforráshoz kapcsolódjon. Egyes partnerek saját összekötőket hoztak létre, melyek teljes listája megtalálható a [FIM 2010: Partnerek kezelőügynökei](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) wikicikkben.

## <a name="supported-connectors-in-mim-2016-sp1"></a>Támogatott összekötők a 2016 SP1 rendszerben

| Összekötő neve | A csatlakoztatott adatforrás támogatott verziói & technikai hivatkozások |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory a Windows Server 2012-2019 |
| Active Directory Lightweight Directory-szolgáltatások (ADLDS) | Active Directory Lightweight Directory-szolgáltatások (ADLDS) |
| Active Directory globális címlista (GAL) | Active Directory globális címlista (GAL) az Exchange 2013-2019 |
| Extensible Connectivity 2.0 | Bármely hívás- vagy fájlalapú adatforrás |
| FIM szolgáltatás | Webszolgáltatások. Vegye figyelembe, hogy a fakiszolgálói szinkronizációs szolgáltatásnak és a webszolgáltatásnak azonos verziójúnak kell lennie. |
| IBM DB2 Universal Database | IBM DB2 9,5-es vagy 9,7-es verzió; IBM DB2 OLEDB v 9.5 FP5 vagy v 9.7 FP1 <br/> Általános SQL-összekötő használata a későbbi verziókhoz|
| IBM Directory Server | IBM Tivoli Directory Server 6.x <br/> Általános LDAP-összekötő használata a későbbi verziókhoz|
| Novell eDirectory | Novell eDirectory 8.7.3, 8.8.5 és 8.8.6 <br/> Általános LDAP-összekötő használata a későbbi verziókhoz|
| Oracle Database | Oracle Database 10g vagy 11g; 64 bites ügyfél <br/> Általános SQL-összekötő használata a későbbi verziókhoz|
| Microsoft SQL Server | SQL Server 2012 – 2017 <br/> Általános SQL-összekötő használata későbbi verziókban vagy SQL Azure|
| Oracle (korábban Sun és Netscape) Directory Server kiszolgálók | Sun Directory Server 6.x, 7.x és Oracle 11<br/> Általános LDAP-összekötő használata a későbbi verziókhoz |
| [Windows PowerShell-összekötő](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 vagy újabb |
| [Microsoft Azure Active Directory-összekötő](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory (új központi telepítések esetén nem ajánlott) |
| [Általános LDAP-összekötő](https://msdn.microsoft.com/library/dn510997.aspx) | [LDAP v3-as kiszolgáló (RFC 4510 szabványnak megfelelő)](reference/microsoft-identity-manager-2016-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| [Általános SQL-összekötő](reference/microsoft-identity-manager-2016-connector-genericsql.md) | [Az összekötő az összes 64 bites ODBC-illesztővel támogatott](reference/microsoft-identity-manager-2016-connector-genericsql.md#overview-of-the-generic-sql-connector) |
| [Lotus Domino-összekötő](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes kiadás v 8.5. x, v 9.0. x |
| [SharePoint Services-összekötő UPA](https://msdn.microsoft.com/library/dn511003.aspx) | SharePoint Server 2013 – 2019 felhasználói profil szolgáltatásalkalmazás (UPA) alkalmazásával |
| [Web Services-összekötő](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5,0 vagy 6,0; Oracle PeopleSoft 9,1; Oracle inaktív 12,1 és más SOAP és REST API-k](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Attribútum-érték párokból álló szövegfájl](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Attribútum-érték párokból álló szövegfájlok |
| [Tagolt szövegfájl](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Tagolt szövegfájlok |
| [Directory Services Mark-up Language (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Rögzített szélességű szövegfájl](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Rögzített szélességű szövegfájlok |
| [LDAP Data Interchange formátum (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange formátum (LDIF) |
| [Microsoft Graph-összekötő](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Kapcsolódó témakörök

[A FIM 2010 R2 kezelőügynökei](https://technet.microsoft.com/library/jj133885.aspx)
