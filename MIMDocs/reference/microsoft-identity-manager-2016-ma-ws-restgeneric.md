---
title: Webszolgáltatási összekötő munkafolyamati útmutatója a REST APIhoz | Microsoft Docs
description: Ez a cikk egy REST API minta üzembe helyezését ismerteti.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 9b1a65d6604f434d3619ad7964caa8ce202092a0
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835954"
---
# <a name="web-service-connector-workflow-guide-for-a-rest-api-sample"></a>Webszolgáltatási összekötő munkafolyamati útmutatója REST API minta

Ez a cikk egy minta-REST API üzembe helyezését ismerteti a webszolgáltatás-konfigurációs eszköz REST API webes adatforrással való átjárásához.

## <a name="prerequisites"></a>Előfeltételek

A minta használatához a következő előfeltételek szükségesek:

- A webszolgáltatás-konfigurációs eszköz telepítve van.
- A REST adatforrás-minta szolgáltatás telepítve van. Töltse le és telepítse a mintát innen: (lásd itt).

<!-- No link provided for "see here" -->
<!-- Should Note go with bullet point #2 -->

>[!NOTE]
>A JSON-adatfájlnak egyetlen objektumot kell tartalmaznia, amely egy tömböt tartalmazó tulajdonságot tartalmaz.

<!-- Should JSON be exactly as-is or just a sample -->
<!-- Should JSON be part of Note content -->
```JSON
{

"EmployeeList":[

{"id":"1","employee_name":"Albano","employee_salary":"22213","employee_age":"37","profile_image":""},{"id":"2","employee_name":"Albano","employee_salary":"22213","employee_age":"37","profile_image":""}

]

}
```

## <a name="configure-rest-project-discovery-in-the-web-service-configuration-tool"></a>REST-projekt felderítésének konfigurálása a webszolgáltatás konfigurációs eszközében
A következő lépések bemutatják, hogyan hozhat létre új projektet az adatforráshoz a webszolgáltatás konfigurációs eszközében.

1. Nyissa meg a webszolgáltatás konfigurációs eszközét. Megnyílik egy üres SOAP-projekt.

   ![Webszolgáltatás-konfigurációs eszköz](media/microsoft-identity-manager-2016-ma-ws-restgeneric/web-service-configuration-tool.png)

2. Válassza a **fájl**  >  **új**  >  **Rest-projekt** lehetőséget.

   ![Új REST-projekt létrehozása](media/microsoft-identity-manager-2016-ma-ws-restgeneric/new-project.png)
   <!-- Image shows SOAP project selected, not REST project -->

3. A bal oldalon válassza a **Rest-projekt** elemet, majd kattintson a **Hozzáadás** gombra.

   ![A REST-projekt kiválasztása](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-project.png)

4. A következő lapon adja meg a következő információkat:

   - Az új webszolgáltatás neve
   - Cím (REST API URL elérési útja)
   - Névtér
   - Biztonsági mód (hitelesítés típusa)

   ![REST-szolgáltatás](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-service.png)
    
   A következő képernyőn példákat láthat ezekre az értékekre:
    
   ![Példa a REST-szolgáltatás értékeire](media/microsoft-identity-manager-2016-ma-ws-restgeneric/restsample.png)

   A **biztonsági mód** beállítása _none_ értékre. Állítsa be a **címeket** az Azure-ban üzemeltetett JSON-kiszolgálóra.

5. Válassza az **OK** lehetőséget. A webszolgáltatások konfigurációs eszközében felsorolt REST-projekt.

   ![REST-projekt a Web Services konfigurációs eszközében](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-discovery.png)

6. A következő lépés a REST API hívás definiálása és a Windows Communication Foundation (WCF) hívásának lefordítása.

   1. Bontsa ki a **Rest projektet** , és válassza ki a _RESTSAMPLE_ szolgáltatást.

   2. Válassza a **Hozzáadás** lehetőséget. A rendszer két érték hozzáadását kéri:
   
      ![Adja meg a REST-szolgáltatás értékeit](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-service-highlights.png)
      
      1. Adja meg a **nevet**. Ez a lépés 3ként van megjelölve a képernyőképen.
      2. Adja meg a **címeket**. Ez a lépés 4ként van megjelölve a képernyőképen.
      3. Válassza az **OK** lehetőséget. A rendszer hozzáad egy REST-erőforrást a _RESTSAMPLE_ szolgáltatás leírásához.

7. Az **erőforrások** mezőben válassza ki az IMÉNT hozzáadott Rest-erőforrást. Adja hozzá a következő metódust:

   ![REST metódus hozzáadása az erőforráshoz](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-method.png)
   <!-- How does this dialog appear, by selecting Edit? -->

8. Válassza ki a REST metódust. Figyelje meg, hogy több metódust is létrehozhat ugyanabban az erőforrásban, és meghatározhatja a végrehajtás során átadott lekérdezéseket.

9. A GETALL metódushoz nem szükséges lekérdezés. Hagyja üresen a paraméter értékeit. A REST API exportálásakor vagy importálásakor a függvénytől függően meg kell adnia a minta kérését vagy a választ. Másolja és illessze be a JSON-visszaküldést a mintában való navigáláskor.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-samples.png)

10. Kattintson a **Mentés** gombra. Mentse a projektet a következőre: `C:\Program Files\Microsoft Forefront Identity Manager\2010\Synchronization Service\Extensions` . 

>[!NOTE]
>A projekt mentése után létrejön a WsConfig fájl. A konfigurációs fájl több olyan fájlt tartalmaz, amelyek a webszolgáltatás áttekintésében korábban vannak meghatározva.


## <a name="configure-object-types-in-the-web-service-configuration-tool"></a>Objektumtípusok konfigurálása a webszolgáltatás konfigurációs eszközében
A következő lépések bemutatják, hogyan konfigurálhatja az adatforráshoz tartozó objektumtípusokat a webszolgáltatás konfigurációs eszközében.

1. A következő lépés az összekötő terület sémájának definiálása. Ez az Objektumtípus létrehozásával és az Objektumtípusok definiálásával érhető el. Kattintson az **Objektumtípusok** elemre a bal oldali ablaktáblán, majd kattintson a **Hozzáadás** gombra. Ekkor megnyílik az alábbi képernyő. Adjon hozzá egy új objektumtípust, és adjon meg egy nevet. Kattintson az **OK** gombra.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/object-types.png)

2. Az objektumtípus hozzáadása az alábbi képernyőt mutatja be.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/object-type-employee.png)

3. Az objektumtípus megfelelő panelje lehetővé teszi az attribútumok és a hozzájuk tartozó tulajdonságok fenntartását a kiválasztott objektumtípus számára. A Hozzáadás gombra kattintva az alábbi képernyő jelenik meg, ahol egy attribútumot adhat hozzá.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employeeid-string.png)

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-id-string.png)

4. Az alábbi képernyő jelenik meg az összes kötelező attribútum hozzáadása után.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-object-type-02.png)

5. Az Objektumtípus és az attribútumok létrehozása után az olyan üres munkafolyamatokat biztosít, amelyek betartják a Microsoft Identity Manager-ben végrehajtott műveleteket.


## <a name="configure-workflows-in-the-web-service-configuration-tool"></a>Munkafolyamatok konfigurálása a webszolgáltatás konfigurációs eszközében

A következő lépés az Objektumtípus munkafolyamatainak konfigurálása. A munkafolyamat-fájlok olyan tevékenységek sorozata, amelyeket a webszolgáltatások összekötője futtat a futási időben. A munkafolyamatok a megfelelő rendszerállapot-művelet megvalósítására szolgálnak. A webszolgáltatás-konfiguráló eszköz segítségével négy különböző munkafolyamatot hozhat létre:

- Importálás: adatok importálása egy adatforrásból a következő két munkafolyamat-típushoz:

    - Teljes importálás: teljes importálás, amely konfigurálható.
    - Különbözeti importálás: a webszolgáltatás-konfigurációs eszköz nem támogatja.

- Exportálás: adatok exportálása a rendszerből egy csatlakoztatott adatforrásba. A művelet a következő három műveletet támogatja. Ezeket a műveleteket a követelmények alapján állíthatja be.

    - Hozzáadás
    - Törlés
    - Csere

- Password (jelszó): a felhasználó (objektumtípus) jelszavas kezelésének végrehajtása. Ehhez a művelethez két művelet érhető el:

    - Jelszó beállítása
    - Change password

- A kapcsolódás tesztelése: konfigurálja a munkafolyamatot annak ellenőrzéséhez, hogy az adatforrás-kiszolgálóval létesített kapcsolatok sikeresen létrejöttek-e.

>[!NOTE]
>Megadhatja a projekthez tartozó munkafolyamatokat, vagy letöltheti az alapértelmezett projektet a [Microsoft letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=29944).


### <a name="workflow-designer"></a>Munkafolyamat-tervező
A Munkafolyamat-tervező úgy nyitja meg a munkaterületet, hogy követelményként konfigurálja a munkafolyamatot. Minden objektumtípus (új/existing) esetében a konfigurációs eszköz biztosítja az eszköz által támogatott munkafolyamatok csomópontjait. 

![Munkafolyamat-tervező](media/microsoft-identity-manager-2016-ma-ws-restgeneric/full-import-configuration-workflow.png)

A Munkafolyamat-tervező a következő felhasználói felületi elemekből áll:

   - **Csomópontok a bal oldali panelen**: Ezek segítenek kiválasztani, hogy melyik munkafolyamatot szeretné megtervezni.

   - **Központi Munkafolyamat-tervező**: itt elvégezheti a munkafolyamatok konfigurálásához szükséges tevékenységek eldobását. A különböző webszolgáltatási műveletek (exportálás, importálás, jelszavas kezelés) elvégzéséhez használhatja a .NET workflow Framework 4 standard és egyéni munkafolyamat-tevékenységeit. A webszolgáltatás-konfigurációs eszköz szabványos és egyéni munkafolyamat-tevékenységeket használ. A szokásos tevékenységekről további információt a következő témakörben talál: [Activity designerek használata](https://msdn.microsoft.com/library/ee829528.aspx).

      - A központi Munkafolyamat-tervezőben egy olyan piros kör, amelynél a felkiáltójel mellett felkiáltójel található, azt jelzi, hogy a művelet el lett dobva, és nincs megfelelően és teljesen meghatározva. A pontos hiba megállapításához vigye a kurzort a vörös kör fölé. A tevékenység helyes meghatározása után a piros kör a sárga információs jelre változik.
      
      - A központi Munkafolyamat-tervezőben a sárga háromszög információs jelzése a tevékenység mellett azt jelzi, hogy a tevékenység definiálva van, de a tevékenység befejezéséhez több lehetőség is van. További információk megjelenítéséhez vigye a kurzort a sárga háromszög fölé.

   - **Eszközkészlet**: az összes eszköz, például a rendszerek és az egyéni tevékenységek, valamint az előre definiált utasítások csomagjainak megtervezése a munkafolyamat kialakításához. További információ: [eszközkészlet](https://msdn.microsoft.com/library/aa480213.aspx).
   
   - **Eszközkészletek részei**: az eszközkészlet a következő fejezetekkel és kategóriákkal rendelkezik:
   
      - **Leírás**: az eszközkészlet fejléce. Az egyik lapon elérheti az eszközkészletet és a kiválasztott munkafolyamat-tevékenység tulajdonságait. 

      - **Munkafolyamat importálása**: egyéni tevékenységek az importálási munkafolyamatok konfigurálásához.
      
      - **Munkafolyamat exportálása**: egyéni tevékenységek az exportálási munkafolyamatok konfigurálásához.
      
      - **Gyakori**: egyéni tevékenységek a munkafolyamatok konfigurálásához.
      
      - **Hibakeresés**: rendszermunkafolyamat-tevékenységek a 4. munkafolyamatban definiált hibakereséshez. Ezek a tevékenységek lehetővé teszik a munkafolyamatok nyomon követését.
      
      - **Utasítások**: rendszermunkafolyamat-tevékenységek definiálva a 4. munkafolyamatban. További információ: [using Activity Designers](https://msdn.microsoft.com/library/ee829528.aspx).

   - **Tulajdonságok**: a Tulajdonságok lapon egy adott munkafolyamat-tevékenység tulajdonságai láthatók, amelyeket a rendszer eldobott a Designer területére. A bal oldali ábrán a **hozzárendelési** tevékenység tulajdonságai láthatók. Minden tevékenység esetében a tulajdonságok eltérnek, és az egyéni munkafolyamat konfigurálásakor használatosak. Ezen a lapon megadhatja a kijelölt eszköz azon attribútumait, amelyeket a központi munkafolyamat-tervezőbe vetettek el. További információ: Properties ( [Tulajdonságok](https://msdn.microsoft.com/library/ee342461.aspx)).

   - **Feladat sávja:** A feladatsor három elemet tartalmaz: **változók**, **argumentumok** és **importálások**. Ezeket az elemeket a munkafolyamat-tevékenységekkel együtt használják. További információkért tekintse [meg a fejlesztői bevezetés a .net 4 Windows Workflow Foundation (WF)](https://msdn.microsoft.com/library/ee342461.aspx)című témakört.



## <a name="configure-a-full-import-workflow-in-the-web-service-configuration-tool"></a>Teljes importálási munkafolyamat konfigurálása a webszolgáltatás konfigurációs eszközében
A következő lépések bemutatják, hogyan konfigurálhatja a REST API teljes importálási munkafolyamatait a webszolgáltatás-konfigurációs eszköz használatával.

>[!WARNING]
>Ez a minta csak munkafolyamatot hoz létre. Szükség lehet a munkafolyamat módosításaira, például az egyéni logika használatára az API-ban.

1. Válassza ki a konfigurálni kívánt teljes importálási munkafolyamatot. Az **argumentumok** és az **importálások** már definiálva vannak, és a tevékenységekre jellemzőek. További információért tekintse meg a következő képernyőket.

   ![Teljes importálási munkafolyamat argumentumai](media/microsoft-identity-manager-2016-ma-ws-restgeneric/arguments.png)

   ![Importált névterek](media/microsoft-identity-manager-2016-ma-ws-restgeneric/imported-name-spaces.png)

   A hívások újrakonfigurálása után módosítania kell a névtér módosítására vagy hozzáadására használt attribútumok nevét, amelyek a régi névtérre hivatkozó API-k és objektumtípusok visszatérési struktúrájára vonatkoznak. A jobb oldali ablaktáblában található eszközkészlet tartalmazza a konfigurációhoz szükséges egyéni munkafolyamat-specifikus tevékenységeket. Rendelje hozzá az értékeket azokhoz a változókhoz, amelyeket a logikához használni fog. Nyissa meg a központi Munkafolyamat-tervező alsó szakaszát, és deklarálja a változókat. A változókat a következő lépésben deklaráljuk.

2. Adja hozzá a Sequence tevékenységet. Húzza a **Sequence** Activity designert az **eszközkészletből** , és dobja el a Windows Munkafolyamat-tervező felületére. Tekintse meg a következő képernyőket. A [Sequence](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) tevékenység olyan alárendelt tevékenységek rendezett gyűjteményét tartalmazza, amelyeket az adott sorrendben hajt végre.
   
    ![Sequence tevékenység](media/microsoft-identity-manager-2016-ma-ws-restgeneric/imports.png)

3. Változó hozzáadásához keresse meg a **create változót**. Írja  be a wsResponse **nevet a név** mezőbe, válassza a **változó típusa** legördülő listát, majd válassza a **Tallózás a típusok között** lehetőséget. Megjelenik egy párbeszédpanel. Válassza ki a **generált**  >  **GETALL**  >  **választ**. A **hatókör** és az **alapértelmezett** értékek ne legyenek kiválasztva. Másik lehetőségként állítsa be ezeket az értékeket a **Tulajdonságok** nézet használatával.

   ![Alapértelmezett válasz](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-list.png)

4. Húzzon egy több **Sequence** -tevékenység tervezőjét az **eszközkészletből** a már hozzáadott szekvenciális tevékenységen belül.

5. Húzzon egy közös **WebServiceCallActivity** **.** Ez a tevékenység a felderítés után elérhető webszolgáltatás-művelet meghívására szolgál. Ez egy egyéni tevékenység, és gyakori a különböző működési forgatókönyvekben. 

    ![Szolgáltatás neve művelet](media/microsoft-identity-manager-2016-ma-ws-restgeneric/full-import-operation-workflow.png)

   A webszolgáltatás művelet használatához állítsa be a következő tulajdonságokat:
   
      - **Szolgáltatás neve**: adja meg a webszolgáltatás nevét.
      - **Végpont neve**: adjon meg egy végpont nevét a kiválasztott szolgáltatáshoz.
      - **Művelet neve**: adja meg a megfelelő műveletet a szolgáltatáshoz.
      - **Argumentum**: válassza az **argumentumok** lehetőséget. A következő párbeszédpanelen rendelje hozzá az argumentum értékeit az alábbi ábrán látható módon:
      
         ![Argumentumok kiosztása](media/microsoft-identity-manager-2016-ma-ws-restgeneric/get-all.png)

         >[!IMPORTANT]
         >Az argumentum **nevét**, **irányát** vagy **típusát** ne módosítsa a párbeszédpanel használatával. Ha bármelyik érték módosul, a tevékenység érvénytelenné válik. Csak az argumentum **értékét** állítsa be. Ahogy az ábrán is látható, a *wsResponse* érték van beállítva.

6. Vegyen fel egy **foreach** -tevékenységet közvetlenül a **WebServiceCallActivity alá.** Ezzel a tevékenységgel lehet megismételni az objektumtípus összes attribútumát (a horgonyokat és a nem horgonyokat is). Ha a tevékenységet a Munkafolyamat-tervező felületére húzza, az automatikusan enumerálja az objektum összes attribútumának nevét. Állítsa be a kötelező értékeket a következő képernyőn látható módon:

   ![Webszolgáltatás hívási tevékenysége](media/microsoft-identity-manager-2016-ma-ws-restgeneric/foreach.png)

7. Bizonyos esetekben előfordulhat, hogy meg kell nyitnia a WsConfig-fájlon belüli generated.dll. Másolja ezt a WsConfig-fájlt, és nevezze át a. zip kiterjesztéssel. Nyissa meg és bontsa ki a generated.dll az előnyben részesített .NET-tükrözés eszköz használatával.

   ![Konfigurációs fájl](media/microsoft-identity-manager-2016-ma-ws-restgeneric/config-files.png)

8. Azonosítsa a _EmployeeList_ nyilvános névterét:

    ![Alkalmazotti lista kódja](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-list-code.png)

    Ezután adja hozzá ezt a visszalépést a munkafolyamat **foreach**:

    ![Alkalmazottak listájának hozzáadása a ForEach-munkafolyamathoz](media/microsoft-identity-manager-2016-ma-ws-restgeneric/foreach-employee-list.png)

9. Húzzon egy **CreateCSEntryChangeScope** tevékenységet a **foreach** törzsén belül. Ezzel a tevékenységgel hozható létre a CSEntryChange objektum egy példánya a munkafolyamat-tartományban minden egyes rekordhoz az adatok cél adatforrásból való beolvasása közben. A tevékenység húzása az alábbi képernyőt jeleníti meg. A rendszer automatikusan örökli a **CreateAnchorAttribute** tevékenységeket. Frissítse a **DN** értéket az előnyben részesített tartománynévre.

    ![A CS bejegyzés módosítási hatókörének létrehozása tevékenység](media/microsoft-identity-manager-2016-ma-ws-restgeneric/createcsentry.png)

    >[!NOTE]
    >A horgony értékei és az objektumok nevei a közzétett webszolgáltatás alapján változnak. Az ábrán egy példa látható.

10. Húzzon egy **CreateAttributeChange** tevékenységet a **CreateAnchorAttribute** tevékenység alá. Az áthúzni kívánt tevékenységek száma megegyezik a nem rögzített attribútumok számával. Lásd az alábbi ábrát.

    ![Horgony létrehozása](media/microsoft-identity-manager-2016-ma-ws-restgeneric/create-anchor-attribute.png)

    >[!NOTE]
    >A tevékenység használatához válassza ki és rendelje hozzá a megfelelő mezőket a legördülő listából, és rendelje hozzá az értékeket. A többértékű attribútumok esetében több **CreateValueChangeActivity** tevékenységet is elhelyezhet egy **CreateAttributeChangeActivity** -tevékenységen belül.

11. A projekt mentése a helyen `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` . Ezután konfigurálja a felügyeleti ügynököt a [WEBSZOLGÁLTATÁS ma konfigurációjában](microsoft-identity-manager-2016-ma-ws-maconfig.md)leírtak szerint.

    ![A REST-projekt mentése](media/microsoft-identity-manager-2016-ma-ws-restgeneric/sample-rest.png)
    
    Az alapértelmezett projekteket le kell tölteni, és a `%FIM_INSTALL_DIR\2010\Synchronization Service\Extensions` célhelyen kell menteni. A projektek ezután megjelennek a webszolgáltatás-összekötő varázslóban.
    
    A végrehajtható fájl futtatásakor a rendszer felszólítja a telepítés helyének megadására. Adja meg a mentési helyet.
    
    >[!IMPORTANT]
    >A projektfájl bármely helyről menthető és megnyitható (a végrehajtójának megfelelő hozzáférési jogosultságokkal). Csak a mappába mentett projektfájlok `Synchronization Service\Extension` választhatók ki a webszolgáltatási összekötő varázslóban, amelyet a rendszer a famodul szinkronizálási felhasználói felületén keresztül érhet el.
    
    A webszolgáltatás konfigurációs eszközét futtató felhasználónak a következő jogosultságokkal kell rendelkeznie:
    
       - Teljes hozzáférés a szinkronizációs szolgáltatás bővítményének mappájához.
       - Olvasási hozzáférés a beállításjegyzék-kulcshoz `HKLM\System\CurrentControlSet\Services\FIMSynchronizationService\Parameters` , amelyen keresztül a bővítmény mappájának elérési útja található.


## <a name="next-steps"></a>Következő lépések

- [Az általános webszolgáltatás-összekötő áttekintése](microsoft-identity-manager-2016-ma-ws.md)
- [A webszolgáltatás-konfigurációs eszköz telepítése](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP telepítési útmutató](microsoft-identity-manager-2016-ma-ws-soap.md)
- [REST telepítési útmutató](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Webszolgáltatás MA – konfiguráció](microsoft-identity-manager-2016-ma-ws-maconfig.md)
