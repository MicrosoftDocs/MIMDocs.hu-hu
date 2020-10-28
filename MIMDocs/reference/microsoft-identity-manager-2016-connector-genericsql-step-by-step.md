---
title: Általános SQL-összekötő lépésről lépésre | Microsoft Docs
description: Ez a cikk egy egyszerű HR-rendszer részletes lépéseit ismerteti az általános SQL-összekötő használatával.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/02/2018
ms.author: billmath
ms.openlocfilehash: 1343fc7604789985c219f6bbae526df72a0baa5b
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760577"
---
# <a name="generic-sql-connector-step-by-step"></a>Általános SQL-összekötő – részletes útmutató
Ez a témakör egy lépésenkénti útmutató. Létrehoz egy egyszerű minta HR-adatbázist, és azt használja a felhasználók és a csoporttagság importálásához.

## <a name="prepare-the-sample-database"></a>A mintaadatbázis előkészítése
SQL Server rendszert futtató kiszolgálón futtassa az [a függelékben](#appendix-a)található SQL-parancsfájlt. Ez a szkript létrehoz egy GSQLDEMO nevű mintaadatbázis-adatbázist. A létrehozott adatbázishoz tartozó objektummodell a következő képhez hasonlít:  
![Objektummodell](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/objectmodel.png)

Hozzon létre egy felhasználót is, amelyet használni szeretne az adatbázishoz való kapcsolódáshoz. Ebben az útmutatóban a felhasználó neve FABRIKAM\SQLUser, és a tartományban található.

## <a name="create-the-odbc-connection-file"></a>Az ODBC-kapcsolatfájl létrehozása
Az általános SQL-összekötő az ODBC használatával csatlakozik a távoli kiszolgálóhoz. Először létre kell hoznia egy, az ODBC-kapcsolódási adatokat tartalmazó fájlt.

1. Indítsa el az ODBC felügyeleti segédprogramot a kiszolgálón:  
   ![ODBC](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc.png)
2. Válassza a TAB **File DSN** elemet. Kattintson a **Hozzáadás...** elemre.  
   ![ODBC1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc1.png)
3. A beépített illesztőprogram jól működik, ezért válassza ki, majd kattintson a Tovább gombra **>** .  
   ![ODBC2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc2.png)
4. Adjon nevet a fájlnak, például **GenericSQL** .  
   ![ODBC3](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc3.png)
5. Kattintson a **Finish** (Befejezés) gombra.  
   ![ODBC4](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc4.png)
6. A kapcsolódás konfigurálásának ideje. Adja meg az adatforrás megfelelő leírását, és adja meg a SQL Server-t futtató kiszolgáló nevét.  
   ![ODBC5](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc5.png)
7. Válassza ki az SQL-hitelesítés módját. Ebben az esetben a Windows-hitelesítést használjuk.  
   ![ODBC6](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc6.png)
8. Adja meg a mintaadatbázis nevét ( **GSQLDEMO** ).  
   ![ODBC7](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc7.png)
9. Minden alapértelmezett érték megtartása ezen a képernyőn. Kattintson a **Finish** (Befejezés) gombra.  
   ![ODBC8](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc8.png)
10. Annak ellenőrzéséhez, hogy minden a várt módon működik-e, kattintson az **adatforrás tesztelése** elemre.  
    ![ODBC9](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc9.png)
11. Győződjön meg arról, hogy a teszt sikeres.  
    ![ODBC10](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc10.png)
12. Az ODBC-konfigurációs fájlnak most már láthatónak kell lennie a Fájl-DSN-ben.  
    ![ODBC11](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc11.png)

Most már rendelkezik a szükséges fájllal, és megkezdheti az összekötő létrehozását.

## <a name="create-the-generic-sql-connector"></a>Az általános SQL-összekötő létrehozása
1. A Synchronization Service Manager felhasználói felületén válassza az **Összekötők** és **Létrehozás** lehetőséget. Válassza az **általános SQL (Microsoft)** lehetőséget, és adjon meg egy leíró nevet.  
   ![Connector1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector1.png)
2. Keresse meg az előző szakaszban létrehozott DSN-fájlt, és töltse fel a-kiszolgálóra. Adja meg az adatbázishoz való kapcsolódáshoz szükséges hitelesítő adatokat.  
   ![Connector2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector2.png)
3. Ebben az útmutatóban egyszerűvé tesszük a számunkra, és azt is tegyük fel, hogy két objektumtípus, **felhasználó** és **csoport** van.
   ![Connector3](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector3.png)
4. Az attribútumok megtalálásához azt szeretnénk, hogy az összekötő felderítse ezeket az attribútumokat úgy, hogy megtekinti a táblázatot. Mivel a **felhasználók** számára fenntartott szó van az SQL-ben, szögletes zárójelben ([]) kell megadnia azt.  
   ![Connector4](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector4.png)
5. A Anchor attribútum és a DN attribútum definiálásának ideje. A **felhasználók** számára a következő két attribútum kombinációját használjuk: Felhasználónév és Alkalmazottkód. A **csoport** esetében a csoportnév (nem reális a valós életben, de ebben a bemutatóban működik) is használható.
   ![Connector5](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector5.png)
6. Nem minden attribútum-típus észlelhető az SQL-adatbázisban. A Reference attribútum típusa nem lehet konkrét. A csoport objektumtípus esetében a OwnerID és a MemberID módosítani kell a hivatkozást.  
   ![Connector6](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector6.png)
7. Az előző lépésben a hivatkozási attribútumokként kiválasztott attribútumok esetében az objektum típusa a következőre hivatkozik:. Ebben az esetben a felhasználói objektum típusa.  
   ![Connector7](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector7.png)
8. A globális paraméterek lapon válassza a **vízjel** lehetőséget a Delta-stratégia elemnél. Adja meg az **éééé-hh-nn óó: PP: mm** dátum/idő formátumát is.
   ![Connector8](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector8.png)
9. A **partíciók és hierarchiák konfigurálása** lapon válassza ki mindkét objektumtípust.
   ![Connector9](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector9.png)
10. Az **Objektumtípusok kiválasztása** és az **attribútumok kiválasztása** lapon válassza ki a két objektumtípust és az összes attribútumot. A **horgonyok konfigurálása** lapon kattintson a **Befejezés** gombra.

## <a name="create-run-profiles"></a>Futtatási profilok létrehozása
1. A Synchronization Service Manager felhasználói felületen válassza az **Összekötők** lehetőséget, és **konfigurálja a futtatási profilokat** . Kattintson az **új profil** elemre. Kezdjük a **teljes importálással** .  
   ![Runprofile1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile1.png)
2. Válassza ki a **teljes importálás típust (csak fázis)** .  
   ![Runprofile2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile2.png)
3. Válassza a partíció **objektum = felhasználó** lehetőséget.  
   ![Runprofile3](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile3.png)
4. Válassza a **tábla** lehetőséget, és írja be a **[felhasználók]** elemet. Görgessen le a többértékű objektumtípus szakaszhoz, és adja meg az értékeket az alábbi képen látható módon. A lépés mentéséhez válassza a **Befejezés** lehetőséget.  
   ![Runprofile4a](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile4a.png)  
   ![Runprofile4b](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile4b.png)  
5. Válassza az **új lépés** lehetőséget. Ezúttal válassza az **objektum = csoport** elemet. Az utolsó lapon használja a konfigurációt az alábbi képen látható módon. Kattintson a **Finish** (Befejezés) gombra.  
   ![Runprofile5a](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile5a.png)  
   ![Runprofile5b](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile5b.png)  
6. Opcionális: Ha szeretné, további futtatási profilokat is beállíthat. Ebben az útmutatóban csak a teljes importálást használja a rendszer.
7. A futtatási profilok módosításának befejezéséhez kattintson **az OK** gombra.

## <a name="add-some-test-data-and-test-the-import"></a>Adjon hozzá néhány tesztoldalt, és tesztelje az importálást
Adja meg a mintaadatbázis egyes tesztelési adatait. Ha elkészült, válassza a **Futtatás** és a **teljes importálás** lehetőséget.

Az alábbi két telefonszámmal rendelkező felhasználó és egy csoport néhány taggal.  
![CS1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a>„A” függelék
**A mintaadatbázis létrehozásához szükséges SQL-szkript**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
