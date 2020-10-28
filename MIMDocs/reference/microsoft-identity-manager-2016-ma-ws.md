---
title: Az általános webszolgáltatás-összekötő áttekintése | Microsoft Docs
description: Az általános webszolgáltatás-összekötő konfigurációjának és követelményeinek áttekintése.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: b0d4cc9ac9b9ac080038d632df0c02c4c244e3d6
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760799"
---
# <a name="overview-of-the-generic-web-service-connector"></a>Az általános webszolgáltatás-összekötő áttekintése

A webszolgáltatás-összekötő a webszolgáltatási műveleteken keresztül integrálja az identitásokat a Microsoft Identity Manager (2016-as) SP1 szervizcsomaggal. Az összekötő megköveteli, hogy a webszolgáltatás-projektfájl a megfelelő adatforrással kapcsolódjon. Ez a projekt a [Microsoft letöltőközpontból](https://go.microsoft.com/fwlink/?LinkID=235883) tölthető le, valamint [dokumentációt](https://www.microsoft.com/en-us/download/details.aspx?id=29943) is tartalmaz, amely a-összekötő Oracle-sel, Oracle PeopleSoft és SAP-vel való használatára használható. A webszolgáltatás-konfigurációs eszköz használatával is létrehozhatja.

Amikor a WsConfig-szinkronizálási szolgáltatás meghívja a webszolgáltatás-összekötőt, betölti a konfigurált projektfájlt ( **WsConfig** -fájl). Ez a fájl segít felismerni az adatforrás végpontját, amelyet a kapcsolat létrehozásához kell használni. A fájl azt is jelzi, hogy a munkafolyamat végre kell hajtania egy webalkalmazás-művelet végrehajtásához. A konfigurált munkafolyamatok végrehajtásához a webszolgáltatás-összekötő kihasználja a .NET 4 munkafolyamat-alaprendszer futási idejének motorját.

![Munkafolyamat](media/microsoft-identity-manager-2016-ma-ws/workflow.png)

## <a name="web-service-layers"></a>Webszolgáltatás-rétegek

Két fő réteget használ a webszolgáltatás-kezelő ügynök (MA) megoldásának megvalósításához: 

- Webszolgáltatás-konfigurációs eszköz
- A munkafolyamat .NET 4,0-mel implementált futásidejű összekötő

## <a name="supported-data-sources-for-web-service-discovery"></a>A webszolgáltatás-felderítés által támogatott adatforrások

A webszolgáltatás konfigurációs eszköze a következő funkciókat valósítja meg:

- SOAP-felderítés: lehetővé teszi a rendszergazda számára, hogy a célként megadott webszolgáltatás által elérhető WSDL-elérési utat adja meg. A felderítés az üzemeltetett webszolgáltatások fastruktúráját fogja létrehozni a belső végpont (ok)/Operations és a művelet meta-adatainak leírásával együtt. Az elvégezhető felderítési műveletek száma nincs korlátozva (lépésről lépésre). A felderített műveletek később az összekötő műveleteinek az adatforráson (Importálás/exportálás/jelszó) való konfigurálását végrehajtó műveletek folyamatát használják.

- REST-felderítés: lehetővé teszi a rendszergazda számára a REST-szolgáltatás részleteit, azaz a szolgáltatási végpontot, az erőforrás elérési útját, a metódust és a paraméter részleteit A felhasználók korlátlan számú Rest-szolgáltatást vehetnek igénybe. A REST-szolgáltatások adatai a Project fájlban lesznek tárolva ```discovery.xml``` ```wsconfig``` . Ezeket később a felhasználó fogja használni a REST-webszolgáltatások tevékenységének konfigurálásához a munkafolyamatban.

- Összekötő terület sémájának konfigurációja: lehetővé teszi, hogy a rendszergazda konfigurálja az összekötő terület sémáját. A séma konfigurációja tartalmazni fogja egy adott implementációhoz tartozó objektumtípusok és attribútumok listáját. A rendszergazda megadhatja azokat az objektumtípusokat, amelyeket a webszolgáltatások már támogatnak. A rendszergazda az összekötő terület sémájának részét képező attribútumokat is kiválaszthatja.

- Műveleti folyamat konfigurációja: a Munkafolyamat-tervező felhasználói felülete a FIM-műveletek (Importálás/exportálás/jelszó) megvalósításának konfigurálásához egy objektumtípus által elérhetővé tett webszolgáltatás-műveleti funkciókon keresztül, például:

    - Paraméterek hozzárendelése az összekötő területéről a webszolgáltatási függvényekhez.
    - Paraméterek hozzárendelése webszolgáltatás-függvényekhez az összekötő területére.

## <a name="resources-generated-by-the-web-service-configuration-tool"></a>A webszolgáltatás-konfigurációs eszköz által létrehozott erőforrások

A webszolgáltatás-konfigurációs eszköz létrehozza a szükséges erőforrásokat a teljes funkcionalitású webszolgáltatás (MA) konfigurálásához, amely a következőket tartalmazza:

- Összekötő terület sémája: a séma konfigurációját tartalmazó bináris fájl. A rendszer a felhasználói felületen keresztül importálja a fájlt a ```Get Schema``` kapcsolaton keresztül, amikor az ma a FIM szinkronizálási felhasználói felületén van konfigurálva. Ezt követően a rendszer átalakítja a ECMA2 séma formátumú objektummá.

- Munkafolyamatok: munkafolyamat-definíciók sorozata. A webszolgáltatás a webszolgáltatások futási időben használják a megfelelő művelet végrehajtásához.

- WCF konfigurációs fájl: a felderítési művelet által létrehozott konfigurációs fájl. A fájl tartalmazza azokat a kötéseket és végpontokat, amelyek az adatforrással kapcsolatos webszolgáltatás-művelet meghívásához szükségesek.

- Adategyezmény-szerelvény: mivel a webszolgáltatás-összekötő mostantól támogatja a SOAP és a REST szolgáltatást is, a által létrehozott adategyezmények eltérőek lesznek a generated.dll fájlban.

- SOAP-szerelvény: a WSDL-bemenet elemzésekor a webszolgáltatás konfigurációs eszköze adategyezmény-típusokat hoz létre, amelyek a webszolgáltatás műveletei által a távoli szolgáltatással folytatott kommunikációhoz használt adatstruktúrák. Ezeket a szerződési típusokat a rendszer a távoli adatforrás-entitások számára is felhasználja az Objektumtípus attribútum-hozzárendeléshez.

- REST-szerelvény: a mintavételi kérelem – válasz a REST-alapú webszolgáltatásra, a konfigurációs eszköz létrehozza a típusokat (osztályokat), amelyeket a rendszer a webszolgáltatások webszolgáltatás hívási tevékenységével folytatott kommunikációhoz használ a munkafolyamatban. Minden kérés/válasz a saját névterében lesz definiálva. A névtér szintaxisa \< szolgáltatásnév \> . \< ResourceName \> . \< MethodName \> . [ Kérelem/válasz]. Az egyes kérések/válaszok különálló névtérbe való becsomagolása segít csökkenteni a problémákat az ismétlődő típus (osztály) neve miatt.

![Munkafolyamat](media/microsoft-identity-manager-2016-ma-ws/workflow2.png)

### <a name="project-file-type"></a>Projektfájl típusa

A webszolgáltatás MA tömörített fájlba (ZIP-formátumba) lett mentve, a felhasználó és a "WsConfig" fájlkiterjesztés által megadott névvel. A "WsConfig" fájlkiterjesztés regisztrálva van, és a telepítő által a webszolgáltatás konfigurációs eszközéhez van társítva. Meglévő MA-projektek megnyithatók, módosíthatók és menthetők. Előfordulhat, hogy a rendszer a FIM synchronization Service Extensions mappába vagy bármely más helyre menti őket. Az Objektumtípus és az attribútumok módosításaihoz szinkronizálás szükséges a FIM oldalon.  A konfigurációs eszköz egy több példányból álló alkalmazás, amely MA (k) létrehozására és módosítására szolgál.

## <a name="supported-security-modes"></a>Támogatott biztonsági üzemmódok

A REST/SOAP webszolgáltatási alkalmazás biztonságossá tehető egy webkiszolgálón, például az IIS-en keresztül. Az alkalmazás lehetővé teszi a felhasználó számára a biztonsági mód kiválasztását, ahogy az az alábbi ábrán is látható. A biztonsági üzemmódok közé tartozik az alapszintű, a kivonatoló, a tanúsítvány, a Windows vagy a none.

![Biztonsági üzemmódok](media/microsoft-identity-manager-2016-ma-ws/security-mode.png)

## <a name="supported-data-types"></a>Támogatott adattípusok

A következő adattípusok támogatottak:

- SOAP (örökölt): a SOAP-adattípus támogatott az [MSDN-cikkben](https://msdn.microsoft.com/library/ms995800.aspx)leírtak szerint. Csak a Business Application Programming Interface (BAPI)-verem támogatását biztosítjuk. A SOAP-sablonok a [Microsoft letöltőközpontban](https://www.microsoft.com/en-us/download/details.aspx?id=51495)érhetők el.
- REST (nem ODATA): HTTP protokoll alapú összekötő/web.

## <a name="next-steps"></a>Következő lépések 

- [A webszolgáltatás-konfigurációs eszköz telepítése](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP telepítési útmutató](microsoft-identity-manager-2016-ma-ws-soap.md)
- [REST telepítési útmutató](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Webszolgáltatás MA – konfiguráció](microsoft-identity-manager-2016-ma-ws-maconfig.md)
