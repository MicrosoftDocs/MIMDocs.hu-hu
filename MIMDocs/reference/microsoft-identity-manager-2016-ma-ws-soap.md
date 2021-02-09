---
title: Webszolgáltatás-összekötő munkafolyamati útmutatója a SOAP-hez | Microsoft Docs
description: Ez a cikk azt ismerteti, hogyan hozható létre új projekt a SOAP-adatforráshoz a webszolgáltatás-konfiguráló eszköz használatával.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/30/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 455988722b3aeb9e29b00696342e1800e9ad82c7
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835971"
---
# <a name="web-service-connector-workflow-guide-for-soap"></a>Webszolgáltatás-összekötő munkafolyamati útmutatója a SOAP-hez

Ez a cikk azt ismerteti, hogyan hozható létre új projekt az adatforráshoz a webszolgáltatás konfigurációs eszközében. Egy projekt létrehozásához kövesse az alábbi lépéseket.

1.  Nyissa meg a webszolgáltatás konfigurációs eszközét. Egy üres projektet nyit meg.

    ![Webszolgáltatás-konfigurációs eszköz](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-configuration-tool-01.png)

2.  Válassza a **SOAP-projekt** elemet, majd kattintson a **Hozzáadás** gombra.

    ![SOAP-projekt](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-configuration-tool-02.png)

3.  A következő lapon adja meg a következő adatokat, majd válassza a **Next (tovább**) gombot:

    - Az új webszolgáltatás neve
    - Címe (WSDL elérési útja) a közzétett szolgáltatások, végpontok és műveletek beolvasásához
    - Névtér
    - Biztonsági mód (hitelesítés típusa)
  
4.  Ebben a példában a **hitelesítő adatok** lap jelenik meg az *alapszintű* biztonsági mód követelményeivel (az előző lépésben kiválasztott mód). Ha a "None" értéket adta meg a biztonsági módhoz, a hitelesítő adatok lap nem jelenik meg. Kattintson a **Tovább** gombra.

    ![SOAP-szolgáltatás felhasználóneve és jelszava](media/microsoft-identity-manager-2016-ma-ws-soap/soap-service.png)

5.  A WSDL elérési útja a szolgáltatás adatainak lekéréséhez és a megjelenített függvények listájának megjelenítéséhez érhető el. Ha a megadott WSDL-elérési út helytelen, akkor a konfigurációs eszköz nem tudja lekérni a szolgáltatás adatait, és hibát jelez.

    ![webszolgáltatás-letöltési folyamat képernyője](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-progress.png)

6.  A felderítés elvégzése után a rendszer felsorolja a végpontot és a felderített műveleteket. Válassza a **Befejezés** gombot.

    ![SOAP szolgáltatási végpontok és felderített műveletek](media/microsoft-identity-manager-2016-ma-ws-soap/soap-service-endpoints.png)

7.  A fordítás elvégezve. A fordítás az adategyezmény-szerelvény fordításának folyamata, amely időigényes művelet lehet. A felhasználó értesítést kap a fordítási hibákról. A felderítés elvégzése után az eszköz a következő oldalt jeleníti meg:

    ![SOAP-felderítés](media/microsoft-identity-manager-2016-ma-ws-soap/soap-discovery.png)

8.  A **SOAP-projekt** bővítése és az alábbi képernyőn megadott elérhető végpont kiválasztása. Ez a képernyő felsorolja a végpont alatt deklarált műveleteket.

    ![A végpont alatt deklarált műveletek](media/microsoft-identity-manager-2016-ma-ws-soap/basic-http-binding.png)

9.  A végpontok kibontása a műveletek listáját jeleníti meg. A művelet egy végpont által deklarált függvény. Az egyes műveletek a szolgáltatáson belül elvégezhető feladatok típusát kezelik. Ez a képernyő a művelethez deklarált argumentumokat sorolja fel. Ezek az argumentumok akkor lesznek meghatározva, amikor a művelet a munkafolyamatok konfigurálásakor használatos.

    ![Kibontott végpontok](media/microsoft-identity-manager-2016-ma-ws-soap/get-employee-byid.png)

10. A következő lépés az összekötő terület sémájának definiálása, amely az objektum típusának létrehozásával és az Objektumtípusok definiálásával érhető el. Válassza az **Objektumtípusok** lehetőséget, majd kattintson a **Hozzáadás** gombra. Az új ablakban adjon hozzá egy új objektumtípust, és adjon meg egy nevet. Válassza az **OK** lehetőséget.

    ![Objektumtípus definiálása](media/microsoft-identity-manager-2016-ma-ws-soap/object-types.png)

11. Az objektumtípus hozzáadása az alábbi képernyőt mutatja be.

    ![újonnan létrehozott objektum típusának megtekintése](media/microsoft-identity-manager-2016-ma-ws-soap/object-type-employee.png)

12. Az objektumtípus megfelelő panelje lehetővé teszi az attribútumok és a hozzájuk tartozó tulajdonságok fenntartását a kiválasztott objektumtípus számára. Válassza a **Hozzáadás** lehetőséget. Az attribútumok hozzáadásához új ablak nyílik meg:

    ![Attribútum és adattípus](media/microsoft-identity-manager-2016-ma-ws-soap/object-type-firstname.png)

    ![Attribútum és adattípus van kiválasztva a Anchor kapcsolóval](media/microsoft-identity-manager-2016-ma-ws-soap/employeeid-string.png)

13. Az összes szükséges attribútum hozzáadása után a következő képernyő jelenik meg:

    ![Az objektum típusa az attribútum adataival](media/microsoft-identity-manager-2016-ma-ws-soap/soap-project.png)

14. Az Objektumtípus és az attribútumok létrehozása után az olyan üres munkafolyamatokat biztosít, amelyek a Microsoft Identity Manager 2016-as (a többhelyes rendszer) szolgáltatásban végrehajtott műveletekhez nyújtanak segítséget.

    ![Az Objektumtípusok megjelenítik az alkalmazott által végrehajtható műveleteket](media/microsoft-identity-manager-2016-ma-ws-soap/object-types-operations.png)


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

![Munkafolyamat-tervező](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-workflow.png)

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


<h2 id="full-import-workflows">Teljes importálási munkafolyamat konfigurálása a webszolgáltatás konfigurációs eszközében</h2>
A következő lépések bemutatják, hogyan konfigurálhatja a SOAP-hez készült teljes importálási munkafolyamatokat a webszolgáltatás-konfigurációs eszközzel.

>[!WARNING]
>Ez a minta csak munkafolyamatot hoz létre. Szükség lehet a munkafolyamat módosításaira, például az egyéni logika használatára az API-ban.

1. Válassza ki a konfigurálni kívánt teljes importálási munkafolyamatot. Az **argumentumok** és az **importálások** már definiálva vannak, és a tevékenységekre jellemzőek. További információért tekintse meg a következő képernyőket.

   ![Teljes importálási munkafolyamat argumentumai](media/microsoft-identity-manager-2016-ma-ws-soap/arguments.png)
 
   ![Importált névterek](media/microsoft-identity-manager-2016-ma-ws-soap/imports.png)

   A hívások újrakonfigurálása után módosítsa a módosult attribútumok nevét, adja hozzá vagy módosítsa a névteret olyan változókkal, amelyek a régi névtérre hivatkozó API-és objektumtípusok visszatérési struktúrájára vonatkoznak. A jobb oldali ablaktáblában található eszközkészlet tartalmazza a konfigurációhoz szükséges egyéni munkafolyamat-specifikus tevékenységeket. Rendelje hozzá az értékeket azokhoz a változókhoz, amelyeket a logikához használni fog. Nyissa meg a központi Munkafolyamat-tervező alsó szakaszát, és deklarálja a változókat. A változókat a következő lépésben deklaráljuk.

2. Adja hozzá a Sequence tevékenységet. Húzza a **Sequence** Activity designert az **eszközkészletből** , és dobja el a Windows Munkafolyamat-tervező felületére. Tekintse meg a következő képernyőket. A [Sequence](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) tevékenység olyan alárendelt tevékenységek rendezett gyűjteményét tartalmazza, amelyeket az adott sorrendben hajt végre.
   
    ![Sequence tevékenység](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-sequence.png)

3. Változó hozzáadásához keresse meg a **create változót**. Írja  be a wsResponse **nevet a név** mezőbe, válassza a **változó típusa** legördülő listát, majd válassza a **Tallózás a típusok között** lehetőséget. Megjelenik egy párbeszédpanel. Válassza a **generált**  >  **alapértelmezett**  >  **Válasz** lehetőséget. A **hatókör** és az **alapértelmezett** értékek ne legyenek kiválasztva. Másik lehetőségként állítsa be ezeket az értékeket a **Tulajdonságok** nézet használatával.

   ![Alapértelmezett válasz](media/microsoft-identity-manager-2016-ma-ws-soap/default-response.png)

   ![Teljes importálási tulajdonságok](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-properties.png)

4. Most adja hozzá az összes többi változót a végső képernyőhöz.

   ![Teljes importálási változók](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-variables.png)

5. Húzzon egy több **Sequence** -tevékenység tervezőjét az **eszközkészletből** a már hozzáadott szekvenciális tevékenységen belül.

6. Húzzon egy közös **WebServiceCallActivity** **.** Ez a tevékenység a felderítés után elérhető webszolgáltatás-művelet meghívására szolgál. Ez egy egyéni tevékenység, és gyakori a különböző működési forgatókönyvekben. 

    ![Szolgáltatás neve művelet](media/microsoft-identity-manager-2016-ma-ws-soap/service-name-operation.png)

   A webszolgáltatás művelet használatához állítsa be a következő tulajdonságokat:
   
      - **Szolgáltatás neve**: adja meg a webszolgáltatás nevét.
      - **Végpont neve**: adjon meg egy végpont nevét a kiválasztott szolgáltatáshoz.
      - **Művelet neve**: adja meg a megfelelő műveletet a szolgáltatáshoz.
      - **Argumentum**: válassza az **argumentumok** lehetőséget. A következő párbeszédpanelen rendelje hozzá az argumentum értékeit az alábbi ábrán látható módon:
      
         ![Argumentumok kiosztása](media/microsoft-identity-manager-2016-ma-ws-soap/get-employeebyid.png)

         >[!IMPORTANT]
         >Az argumentum **nevét**, **irányát** vagy **típusát** ne módosítsa a párbeszédpanel használatával. Ha bármelyik érték módosul, a tevékenység érvénytelenné válik. Csak az argumentum **értékét** állítsa be. Ahogy az ábrán is látható, a *wsResponse* érték van beállítva.

7. Vegyen fel egy **foreach** -tevékenységet közvetlenül a **WebServiceCallActivity alá.** Ezzel a tevékenységgel lehet megismételni az objektumtípus összes attribútumát (a horgonyokat és a nem horgonyokat is). Ha a tevékenységet a Munkafolyamat-tervező felületére húzza, az automatikusan enumerálja az objektum összes attribútumának nevét. Állítsa be a kötelező értékeket a következő képernyőn látható módon:

   ![Webszolgáltatás hívási tevékenysége](media/microsoft-identity-manager-2016-ma-ws-soap/webservicecallactivity.png)

8. Húzzon egy **CreateCSEntryChangeScope** tevékenységet a **foreach** törzsén belül. Ezzel a tevékenységgel hozható létre a CSEntryChange objektum egy példánya a munkafolyamat-tartományban minden egyes rekordhoz az adatok cél adatforrásból való beolvasása közben. A tevékenység húzása az alábbi képernyőt jeleníti meg. A rendszer automatikusan örökli a **CreateAnchorAttribute** tevékenységeket.

    ![A CS bejegyzés módosítási hatókörének létrehozása tevékenység](media/microsoft-identity-manager-2016-ma-ws-soap/createcsentrychangescope.png)

9.  A DN kifejezés értékének beállítása a következőképpen: `‘string.Concat ("Employee",item.EmployeeID)’` . Állítsa be az _AlkalmazottKód_ **AnchorValue** az **"Convert. tostring (tétel) értékre. Alkalmazottkód) "**. Állítsa be  a ObjectTypeName _alkalmazottként_. A módosítások végrehajtása után a következő képernyő jelenik meg:

    ![Az alkalmazott AZONOSÍTÓjának beolvasása](media/microsoft-identity-manager-2016-ma-ws-soap/get-employeebyid.png)

    >[!NOTE]
    >A horgony értékei és az objektumok nevei a közzétett webszolgáltatás alapján változnak. Az ábrán egy példa látható.

10. Húzzon egy **CreateAttributeChange** tevékenységet a **CreateAnchorAttribute** tevékenység alá. Az áthúzni kívánt tevékenységek száma megegyezik a nem rögzített attribútumok számával. Lásd az alábbi ábrát.

    ![Horgony létrehozása](media/microsoft-identity-manager-2016-ma-ws-soap/create-anchor.png)

11. Húzzon **CreateValueChangeActivity** az **CreateAttributeChange** tevékenységen belül, és állítsa be az attribútum értékét az alábbi képernyőn.

    ![Attribútum módosítása](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-change.png)

    >[!NOTE]
    >A tevékenység használatához válassza ki és rendelje hozzá a megfelelő mezőket a legördülő listából, és rendelje hozzá az értékeket. A többértékű attribútumok esetében több **CreateValueChangeActivity** tevékenységet is elhelyezhet egy **CreateAttributeChangeActivity** -tevékenységen belül.

12. Egy attribútum feltételeinek hozzáadásához vegyen fel egy **IF** tevékenységet az alábbi ábrán látható módon:

    ![Ha a tevékenység](media/microsoft-identity-manager-2016-ma-ws-soap/if.png)

13. Végül adjon hozzá egy **hozzárendelési** tevékenységet, és állítsa be a kifejezést a következő ábrán látható módon:

    ![Tevékenység kiosztása és a kifejezés beállítása](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-change-email.png)

14. A projekt mentése a helyen `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .
    
    Az alapértelmezett projekteket le kell tölteni, és a `%FIM_INSTALL_DIR\2010\Synchronization Service\Extensions` célhelyen kell menteni. A projektek ezután megjelennek a webszolgáltatás-összekötő varázslóban.
    
    A végrehajtható fájl futtatásakor a rendszer felszólítja a telepítés helyének megadására. Adja meg a mentési helyet.
    
    >[!IMPORTANT]
    >A projektfájl bármely helyről menthető és megnyitható (a végrehajtójának megfelelő hozzáférési jogosultságokkal). Csak a mappába mentett projektfájlok `Synchronization Service\Extension` választhatók ki a webszolgáltatási összekötő varázslóban, amelyet a rendszer a famodul szinkronizálási felhasználói felületén keresztül érhet el.
    
    A webszolgáltatás konfigurációs eszközét futtató felhasználónak a következő jogosultságokkal kell rendelkeznie:
    
       - Teljes hozzáférés a szinkronizációs szolgáltatás bővítményének mappájához.
       - Olvasási hozzáférés a beállításjegyzék-kulcshoz `HKLM\System\CurrentControlSet\Services\FIMSynchronizationService\Parameters` , amelyen keresztül a bővítmény mappájának elérési útja található.


## <a name="configure-export-workflows-in-the-web-service-configuration-tool"></a>Az exportálási munkafolyamatok konfigurálása a webszolgáltatás konfigurációs eszközében
A következő részekben bemutatjuk, hogyan exportálhatja a munkafolyamatokat a webszolgáltatás konfigurációs eszközének használatával.

<h3 id="attribute-change-anchor">Munkafolyamatok hozzáadása</h3>
Az exportálási munkafolyamatok hozzáadásához kövesse ezeket a lépéseket a webszolgáltatás konfigurációs eszközében.

1. Válassza ki a konfigurálni kívánt munkafolyamat exportálását. Az **Exportálás** területen válassza a **Hozzáadás** lehetőséget. Az **argumentumok** és az **importálások** már definiálva vannak, és a tevékenységekre jellemzőek. Tekintse meg a következő képernyőket a hivatkozáshoz.

    ![Hozzáadás](media/microsoft-identity-manager-2016-ma-ws-soap/add.png)

2. Adja hozzá a **Sequence** tevékenységet. Húzza a **Sequence** Activity designert az **eszközkészletből** , és dobja el a Windows Munkafolyamat-tervező felületére. A [Sequence](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) tevékenység olyan alárendelt tevékenységek rendezett gyűjteményét tartalmazza, amelyeket az adott sorrendben hajt végre. Válassza a **változó létrehozása** lehetőséget. Rendelje hozzá az értékeket azokhoz a változókhoz, amelyeket a logikához használni fog.

    ![Exportálás](media/microsoft-identity-manager-2016-ma-ws-soap/export-add.png)

    >[!NOTE]
    >A változók hozzáadásának lépéseit a <a href="#full-import-workflows">teljes importálási munkafolyamatok</a>létrehozásának szakasza ismerteti.

3. Húzzon egy **foreach** tevékenységet a már hozzáadott **szekvenciális** tevékenységen belül, és ismételje meg a horgony attribútum értékeit.

4. Válassza a **Tulajdonságok** lehetőséget, és állítsa be az **értékeket** az alábbi képernyőn. Itt **objectToExport** argumentum.

    ![A ForEach tevékenység tulajdonságainak beállítása](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-sequence.png)

5. **DisplayName** beállítása **foreach \<AnchorAttribute\>**

   ![Megjelenítendő név beállítása](media/microsoft-identity-manager-2016-ma-ws-soap/add-sequence.png)

6. A **TypeArgument** beállítása `Microsoft.MetadirectoryServices.AnchorAttribute` .

   ![Adja meg a Type argumentumot](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-anchor.png)

7. Adjon hozzá egy **kapcsoló** tevékenységet a **AnchorAttribute** **foreach** törzsén belül.

   ![Kapcsoló tevékenység hozzáadása](media/microsoft-identity-manager-2016-ma-ws-soap/select-net-types.png)

8. Adjon hozzá egy kifejezést az alábbi képernyőn látható módon.

   ![Kifejezés hozzáadása](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch.png)

9. Válassza az **új eset hozzáadása** lehetőséget, és adjon meg egy értéket az **AlkalmazottKód** számára. Húzzon egy **Sequence (folyamat** ) tevékenységet, és a benne lévő **hozzárendelési** tevékenységet adja hozzá.

    ![Új eset hozzáadása és a sorszámhoz rendelése](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-employeeid.png)

10. Rendelje hozzá a **to** és a **Value** tulajdonságot a **hozzárendelési** tevékenységhez.

    ![A to és Value tulajdonságok kiosztása](media/microsoft-identity-manager-2016-ma-ws-soap/anchor-attribute-name.png)

11. A **foreach** tevékenység a horgony értékeit használja. Adjon hozzá egy másik **foreach** tevékenységet a nem rögzített értékek hozzárendeléséhez. Ebben a példában a **AttributeChange** horgonyt használjuk.

    ![Másik ForEach-tevékenység hozzáadása a AttributeChange-horgonysal](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-change.png)

12. Adjon hozzá egy **kapcsoló** tevékenységet a **AttributeChange** -horgony **foreach** törzsén belül.   

    ![Kapcsoló tevékenység hozzáadása a AttributeChange-horgonyhoz](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-name-wrapper.png)

13. Adjon hozzá egy kifejezést az alábbi képernyőn látható módon.

    ![Kifejezés hozzáadása a kapcsoló tevékenységhez](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-expression.png)

14. Válassza az **új eset hozzáadása** lehetőséget, és adjon meg egy értéket a **FirstName** számára. Húzzon egy **Sequence (folyamat** ) tevékenységet, és a benne lévő **hozzárendelési** tevékenységet adja hozzá. Rendelje hozzá a **to** és a **Value** tulajdonságot a **hozzárendelési** tevékenységhez.

    ![Új eset hozzáadása a sorozatban](media/microsoft-identity-manager-2016-ma-ws-soap/switch-firstname.png)

15. Adja meg a szükséges attribútumok értékeit, például a **LastName**, az **e-mail** és így tovább. 

    ![Értékek hozzáadása a szükséges attribútumokhoz](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch-anchor.png)

16. Az **általános** területen húzzon egy **WebServiceCallActivity** , és állítsa be az **argumentumok** **értékeit** .

    ![Webszolgáltatás hívási tevékenységének hozzáadása és az értékek beállítása](media/microsoft-identity-manager-2016-ma-ws-soap/add-employee-attribute.png)

    >[!IMPORTANT]
    >Az argumentum **nevét**, **irányát** vagy **típusát** ne módosítsa a párbeszédpanel használatával. Ha bármelyik érték módosul, a tevékenység érvénytelenné válik. Csak az argumentum **értékét** állítsa be. Ahogy az ábrán is látható, a *wsResponse* érték van beállítva.

17.  Végül vegyen fel egy **IF** tevékenységet a webszolgáltatási művelet által visszaadott válaszok vizsgálatához.

Befejeződött az exportálási munkafolyamat létrehozása a **hozzáadási** művelettel:

![Befejezett exportálási munkafolyamat](media/microsoft-identity-manager-2016-ma-ws-soap/create-csentry-change.png)

A projekt mentése a helyen `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


### <a name="delete-workflows"></a>Munkafolyamatok törlése
A webszolgáltatások konfigurációs eszközében a következő lépésekkel törölheti az exportálási munkafolyamatokat.

1. Válassza ki a konfigurálni kívánt munkafolyamat exportálását. Az **Exportálás** területen válassza a **Törlés** lehetőséget. Az **argumentumok** és az **importálások** már definiálva vannak, és a tevékenységekre jellemzőek. Tekintse meg a következő képernyőket a hivatkozáshoz.

   ![Munkafolyamat-törlési munkafolyamatok exportálása](media/microsoft-identity-manager-2016-ma-ws-soap/export-delete.png)

2. Adja hozzá a **Sequence** tevékenységet. Válassza a **változó létrehozása** lehetőséget. Rendelje hozzá az értékeket azokhoz a változókhoz, amelyeket a logikához használni fog.

   ![Szekvenciális tevékenység hozzáadása](media/microsoft-identity-manager-2016-ma-ws-soap/sequence-variables.png)

   >[!NOTE]
   >A változók hozzáadásának lépéseit a <a href="#full-import-workflows">teljes importálási munkafolyamatok</a>létrehozásának szakasza ismerteti.

3. Húzzon egy **foreach** tevékenységet a már hozzáadott **szekvenciális** tevékenységen belül, és ismételje meg a horgony attribútum értékeit.

4. Válassza a **Tulajdonságok** lehetőséget, és állítsa be az **értékeket** az alábbi képernyőn. Itt **objectToExport** argumentum.

   ![A ForEach tevékenység tulajdonságainak beállítása](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-object-to-export.png)

5. A **DisplayName** beállítása a következőképpen `ForEach\<AnchorAttribute\>` :

   ![Megjelenítendő név beállítása](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-anchor-type.png)

6. Állítsa be a **TypeArgument** a következőképpen `Microsoft.MetadirectoryServices.AnchorAttribute` : 

   ![Adja meg a Type argumentumot](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-type-argument.png)

7. Adjon hozzá egy **kapcsoló** tevékenységet a **AnchorAttribute** **foreach** törzsén belül.

   ![Kapcsoló tevékenység hozzáadása](media/microsoft-identity-manager-2016-ma-ws-soap/select-net-type.png)

8. Adjon hozzá egy kifejezést az alábbi képernyőn látható módon.

   ![Kifejezés hozzáadása](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch.png)

9. Válassza az **új eset hozzáadása** lehetőséget, és adjon meg egy értéket az **AlkalmazottKód** számára. Húzzon egy **Sequence (folyamat** ) tevékenységet, és a benne lévő **hozzárendelési** tevékenységet adja hozzá.

   ![Új eset hozzáadása és a sorszámhoz rendelése](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-default.png)

10. Rendelje hozzá a **to** és a **Value** tulajdonságot a **hozzárendelési** tevékenységhez.

    ![A to és Value tulajdonságok kiosztása](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-attribute-flow.png)

11. Az **általános** területen húzzon egy **WebServiceCallActivity** , és állítsa be az **argumentumok** **értékeit** .

    ![Webszolgáltatás hívási tevékenységének hozzáadása és az értékek beállítása](media/microsoft-identity-manager-2016-ma-ws-soap/delete-employee.png)

    >[!IMPORTANT]
    >Az argumentum **nevét**, **irányát** vagy **típusát** ne módosítsa a párbeszédpanel használatával. Ha bármelyik érték módosul, a tevékenység érvénytelenné válik. Csak az argumentum **értékét** állítsa be. Ahogy az ábrán is látható, az *AlkalmazottKód* érték van beállítva.

12. Végül vegyen fel egy **IF** tevékenységet, hogy ellenőrizze a webszolgáltatási művelet által visszaadott válaszokat.

Befejeződött az exportálási munkafolyamat törlése a **törlési** művelettel:

![Az exportálási munkafolyamat törölve](media/microsoft-identity-manager-2016-ma-ws-soap/create-csentry-change.png)

<!-- Image of completed Delete operation is missing from document -->

A projekt mentése a helyen `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


### <a name="replace-workflows"></a>Munkafolyamatok cseréje
Cserélje le az exportálási munkafolyamatokat a webszolgáltatás-konfigurációs eszköz alábbi lépéseivel.

1. Válassza ki a konfigurálni kívánt munkafolyamat exportálását. Az **Exportálás** területen válassza a **Csere** elemet. Az **argumentumok** és az **importálások** már definiálva vannak, és a tevékenységekre jellemzőek. Lásd az alábbi képernyőt a hivatkozáshoz.

   ![Munkafolyamat cseréje](media/microsoft-identity-manager-2016-ma-ws-soap/replace.png)

2. Adja hozzá a **Sequence** tevékenységet.

3. Húzzon egy **foreach** tevékenységet a következőhöz: **\<AnchorAttribute> .**

4. Adjon hozzá egy másik **foreach \<AttributeChange>** tevékenységet a nem rögzített értékek hozzárendeléséhez.

5. Végül a képernyő az alábbi ábrához hasonlóan néz ki. A tevékenység konfigurálására vonatkozó utasításokat az <a href="#attribute-change-anchor">exportálási munkafolyamatok hozzáadását</a>ismertető szakaszban találja.

   ![ForEach és Anchor attribútummal](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch-anchor.png)

6. Az **általános** területen húzzon egy **WebServiceCallActivity** , és állítsa be az **argumentumok** **értékeit** .

   ![Webszolgáltatás hívási tevékenységének hozzáadása és az értékek beállítása](media/microsoft-identity-manager-2016-ma-ws-soap/wsresponse.png)

   >[!IMPORTANT]
   >Az argumentum **nevét**, **irányát** vagy **típusát** ne módosítsa a párbeszédpanel használatával. Ha bármelyik érték módosul, a tevékenység érvénytelenné válik. Csak az argumentum **értékét** állítsa be. Ahogy az ábrán is látható, az *alkalmazott* érték van beállítva.

7. Végül vegyen fel egy **IF** tevékenységet a webszolgáltatási művelet által visszaadott válaszok vizsgálatához.

Befejeződött az exportálási munkafolyamat cseréje a **replace** művelettel:

![Az exportálási munkafolyamat cseréje](media/microsoft-identity-manager-2016-ma-ws-soap/operation-name-update-employee.png)

A projekt mentése a helyen `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


## <a name="debug-activities"></a>Hibakeresési tevékenységek
A következő egyéni tevékenységek segítenek a munkafolyamat-sablon hibakeresésében.

### <a name="log-activity"></a>Napló tevékenység
A **log** tevékenység a szöveges üzenetek naplófájlba írására szolgál. További információ: [naplózás](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx).

>[!NOTE]
>Ha nem tud egyszerűen hibakeresést végezni a munkafolyamatban, próbálkozzon a munkafolyamatok hibakeresésével az éles környezetben.  

A **napló** tevékenység használatához állítsa be a következő tulajdonságokat: A tulajdonságok akkor láthatók, ha kiválasztja a tevékenységet a Munkafolyamat-tervezőben, és megtekinti a tevékenység **tulajdonságait** .
<!-- Properties missing from document -->
<!-- Image of Log activity GUI missing from document -->

### <a name="writeline-activity"></a>WriteLine-tevékenység
Az **WriteLine** -tevékenységgel szöveges üzeneteket írhat a szolgáltató írója számára. Ha nincs elérhető író [, a rendszer a](http://127.0.0.1:47873/help/1-7016/ms.help?method=page&id=T%3ASYSTEM.ACTIVITIES.STATEMENTS.WRITELINE&product=VS&productVersion=100&topicVersion=100&locale=EN-US&topicLocale=EN-US&embedded=true) parancssorablakba írja a szöveget.

<!-- Image of WriteLine activity GUI missing from document -->

A szövegmezőben írja be az üzenetet, amelyet látni szeretne az író céljában.

>[!IMPORTANT]
>A konzol ablaka nem használható ehhez a tevékenységhez. Ehhez a feladathoz használjon egy másik ablak kimenet-írót.

Az **WriteLine** -tevékenység használatához állítsa be a következő tulajdonságokat: A tulajdonságok akkor láthatók, ha kiválasztja a tevékenységet a Munkafolyamat-tervezőben, és megtekinti a tevékenység **tulajdonságait** .

- **Naplózási szint**: a log értékbe írandó tartalom mennyiségét határozza meg. Lehetséges értékek:

    - Magas: írja a **LogText** üzenetet a naplófájlba, ha a napló súlyossága magas értékre van állítva.
    - Részletes: írja be a **LogText** üzenetet a naplófájlba, ha a napló súlyossága részletes értékre van állítva.
    - Letiltva: ne írjon a naplófájlba.
- **LogText**: a naplóba írandó szöveges tartalmat adja meg.
- **Címke**: címkét szúr be a szövegbe a naplóban írt tartalom típusának azonosításához. A lehetséges értékek a következők: hiba, nyomkövetés vagy figyelmeztetés.

<!-- log severity is not defined in this document -->


## <a name="next-steps"></a>Következő lépések

- [Az általános webszolgáltatás-összekötő áttekintése](microsoft-identity-manager-2016-ma-ws.md)
- [A webszolgáltatás-konfigurációs eszköz telepítése](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP telepítési útmutató](microsoft-identity-manager-2016-ma-ws-soap.md)
- [REST telepítési útmutató](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Webszolgáltatás MA – konfiguráció](microsoft-identity-manager-2016-ma-ws-maconfig.md)
