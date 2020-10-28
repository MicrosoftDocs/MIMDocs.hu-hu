---
title: BHOLD fejlesztői referenciája Microsoft Identity Manager 2016-hez | Microsoft Docs
description: BHOLD fejlesztői leírás
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: edf9f88a4d96d212cef4aa41cbad7d0b4446534f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760787"
---
# <a name="bhold-developer-reference-for-microsoft-identity-manager-2016"></a>BHOLD fejlesztői referenciája Microsoft Identity Manager 2016

A BHOLD-Core modul parancsfájl-parancsokat dolgoz fel. Ezt közvetlenül a .NET-projektben lévő bscript.dll használatával teheti meg. A webszolgáltatások b1scriptservice. asmx felületének használata is működik. 

A szkript végrehajtása előtt a parancsfájlban található összes információt össze kell gyűjteni a parancsfájl összeállításához. Ezek az információk a következő forrásokból gyűjthetők össze:

   * Felhasználói bevitel
   * BHOLD-adathalmazok
   * Alkalmazások
   * Egyéb

A BHOLD-adat a parancsfájl-objektum GetInfo funkciójával kérhető le. A BHOLD-adatbázisban tárolt összes adatmennyiséget tartalmazó parancsok teljes listája megtalálható. A bemutatott adatmennyiségre azonban a bejelentkezett felhasználó megtekintési engedélyei vonatkoznak. Az eredmény egy XML-dokumentum formájában történik, amely elemezhető.

Az információk egy másik forrása lehet a BHOLD által vezérelt alkalmazások egyike. Az alkalmazás beépülő moduljának van egy speciális funkciója, a FunctionDispatch, amely az alkalmazásspecifikus információk bemutatására használható. Ez XML-dokumentumként is jelenik meg.

Végül, ha nincs más mód, a szkript közvetlenül más alkalmazásokhoz vagy rendszerekhez is tartalmazhat parancsokat. A BHOLD-kiszolgálón található további szoftverek NoThenstallation alááshatja a teljes rendszer biztonságát.

Mindezen információk egyetlen XML-dokumentumba kerülnek, és a BHOLD parancsfájl-objektumhoz vannak rendelve. Az objektum egy előre definiált függvénnyel ötvözi ezt a dokumentumot. Az előre definiált függvény egy XSL-dokumentum, amely lefordítja a parancsfájl bemeneti dokumentumát egy BHOLD-parancs dokumentumba.

![BHOLD-parancsfájlok feldolgozása](media/mim2016-bhold-developer-reference/image001.png)

A parancsok ugyanúgy vannak végrehajtva, mint a dokumentumban. Ha egy függvény meghibásodik, a rendszer az összes végrehajtott parancsot visszaállítja.

## <a name="script-object"></a>Parancsfájl-objektum
Ez a szakasz a parancsfájl-objektum használatát ismerteti.

### <a name="retrieve-bhold-information"></a>BHOLD adatok beolvasása
A **GetInfo** függvénnyel adatokat kérhet le a BHOLD-engedélyezési rendszerből elérhető adatokból. A függvényhez szükség van egy függvény nevére, és végül egy vagy több paramétert is meg kell adni. Ha ez a függvény sikeres, a rendszer egy BHOLD objektumot vagy gyűjteményt ad vissza egy XML-dokumentum formájában.

Ha a függvény nem sikerül, a GetInfo függvény üres karakterláncot vagy hibát ad vissza. A hiba leírása és száma segítségével további információkhoz juthat a hibáról.

A "FunctionDispatch" GetInfo függvény használatával adatokat kérhet le a BHOLD rendszer által vezérelt alkalmazásból. Ehhez a függvényhez három paraméter szükséges: az alkalmazás azonosítója, a kiosztási függvény, ahogy az ASI-ban van definiálva, valamint egy XML-dokumentum, amely az ASI-ra vonatkozó támogatási információkat biztosít. Ha a függvény sikeres, az eredmény az eredmény objektum XML-formátumában érhető el.

Az alábbi kódrészlet egy egyszerű C# példa a GetInfo:

```C#
ScriptProcessor myScriptProcessor = new ScriptProcessor();
myScriptProcessor.Initializae("CORP\\b1user");
myScriptProcessor.GetInfo("OrgUnit", "1");
```

Hasonlóképpen, a **BScript** objektum a webszolgáltatás használatával is elérhető `b1scriptservice` . Ehhez egy webes referenciát kell hozzáadnia a projekthez a http:// <server> : 5151/BHOLD/Core/b1scriptservice. asmx használatával, ahol az a <server> kiszolgáló, amelyen telepítve vannak a BHOLD bináris fájljai. További információ: [webszolgáltatás-hivatkozás hozzáadása egy Visual Studio-projekthez](https://msdn.microsoft.com/library/d9w023sx(v=vs.71).aspx).

Az alábbi példa bemutatja, hogyan használhatja a **GetInfo** függvényt egy webszolgáltatásból. Ez a kód lekéri azt a szervezeti egységet, amelynek OrgID 1, majd megjeleníti a képernyőn látható szervezeti egység nevét.

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Xml;

namespace bhold_console
{
    class Program
    {
        static void Main(string[] args)
        {
             var bholdService = new BHOLDCORE.B1ScriptService();
             bholdService.Url = "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx";
             string orgname= "";

             if (args.Length == 3)
             {
                 //Use explicit credentials from command line
                 bholdService.UseDefaultCredentials = false;
                 bholdService.Credentials = new System.Net.NetworkCredential(args[0], args[1], args[2]);
                 bholdService.PreAuthenticate = true;
             }
             else
             {
                 bholdService.UseDefaultCredentials = true;
                 bholdService.PreAuthenticate = true;
             }

             //Load BHOLD information into an xml document and loop through document to find the bholdDescription value
             var myOrgUnit = new System.Xml.XmlDocument();
             myOrgUnit.LoadXml(bholdService.GetInfo("OrgUnit","1","","");

            XmlNodeList myList = myOrgUnit.SelectNodes(("//item");

            foreach (XmlNode myNode in myList)
            {
                for (int i = 0; i < myNode.ChildNodes.Count; i++)
                {
                    if (myNode.ChildNodes[i].InnerText.ToString() == "bholdDescription")
                    {
                        orgname = myNode.ChildNodes[i + 1].InnerText.ToString();
                    }
                }
            }

            System.Console.WriteLine("The Organizational Unit Name is: " + orgname);

        }
    }
}
```

A következő VBScript-példa a webszolgáltatást használja a SOAP-n keresztül, és a GetInfo-t használja. A SOAP 1,1, a SOAP 1,2 és a HTTP POST esetében további példákat talál a BHOLD által felügyelt hivatkozás szakaszban, vagy közvetlenül egy böngészőből navigálva megtekintheti őket.

```VB
Dim SOAPRequest
Dim SOAPParameters
Dim SOAPResponse
Dim xmlhttp

Set xmlhttp = CreateObject("Microsoft.XMLHTTP")

xmlhttp.open "POST", "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx", False, "CORP\Administrator", "abc123*2k"

xmlhttp.setRequestHeader "Content-type", "text/xml; charset=utf-8"
xmlhttp.setRequestHeader "SOAPAction", "http://B1/B1ScriptService/GetInfo"

SOAPRequest = "<?xml version='1.0' encoding='utf-8'?> <soap:Envelope" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:xsi=""http://" & vbCRLF
SOAPRequest = SOAPRequest & " www.w3.org/2001/XMLSchema-instance""" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:xsd=""http://www.w3.org/2001/XMLSchema""" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:soap=""http://schemas.xmlsoap.org/soap/envelope/"">" & vbCRLF
SOAPRequest = SOAPRequest & " <soap:Body>" & vbCRLF
SOAPRequest = SOAPRequest & " <GetInfo xmlns=""http://B1/B1ScriptService"">" & vbCRLF
SOAPRequest = SOAPRequest & " <functionName>OrgUnit</functionName>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter1>1</parameter1>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter2></parameter2>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter3></parameter3>" & vbCRLF
SOAPRequest = SOAPRequest & " </GetInfo>" & vbCRLF
SOAPRequest = SOAPRequest & " </soap:Body>" & vbCRLF
SOAPRequest = SOAPRequest & " </soap:Envelope>"
MsgBox SOAPRequest

xmlhttp.send SOAPRequest 

SOAPResponse = xmlhttp.responseText

MsgBox SOAPResponse
```

## <a name="execute-scripts"></a>Parancsfájlok végrehajtása

A **BScript** objektum **ExecuteScript** funkciója a parancsfájlok végrehajtásához használható. Ehhez a függvényhez két paraméter szükséges. Az első paraméter a parancsfájl által használandó egyéni adatokat tartalmazó XML-dokumentum. A második paraméter a használni kívánt előre definiált parancsfájl neve. A BHOLD előre definiált InIn a következőnek kell lennie: XSL-dokumentum, amelynek a neve megegyezik a függvény nevével, de az. xsl kiterjesztéssel.

Ha a függvény nem sikerül, a ExecuteScript függvény a False (hamis) értéket adja vissza. A hiba leírását és számát felhasználhatja, hogy megtudja, mi volt a probléma. A következő példa a ExecuteXML web Method használatát szemlélteti. Ez a metódus a ExecuteScript hívja meg.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Sample
{
    class Program
    {
        static void Main(string[] args)
        {
            var bholdService = new BHOLDCORE.B1ScriptService();
            bholdService.Url = "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx";

            if (args.Length == 3)
            {
                //Use explicit credentials from command line
                bholdService.UseDefaultCredentials = false;
                bholdService.Credentials = new System.Net.NetworkCredential(args[0], args[1], args[2]);
                bholdService.PreAuthenticate = true;
            }
            else
            {
                bholdService.UseDefaultCredentials = true;
                bholdService.PreAuthenticate = true;
            }
            System.Console.WriteLine( "Add user #3 to role #44, result: {0}", bholdService.ExecuteXml(roleAddUser("44", "3")) );
            System.Console.WriteLine("Add user D1 to role 'MR-OU2 No Users', result: {0}", bholdService.ExecuteXml(roleAddUser("MR-OU2 No Users", "D1")));

        }

        private static System.Xml.XmlNode roleAddUser(string roleId, string userId)
        {
            var script = new System.Xml.XmlDocument();
            script.LoadXml(string.Format("<functions>"
                                        +"  <function name='roleadduser' roleid='{0}' userid='{1}' />"
                                        +"</functions>",
                                        roleId,
                                        userId)
                           );
            return script.DocumentElement;
```

## <a name="bholdscriptresult"></a>BholdScriptResult

Ez a GetInfo függvény a **executescript** függvény végrehajtása után érhető el. A függvény egy XML formátumú karakterláncot ad vissza, amely tartalmazza a teljes végrehajtási jelentést. A parancsfájl csomópontja a végrehajtott parancsfájl XML-struktúráját tartalmazza.

Minden olyan függvénynél, amely a parancsfájl végrehajtása során meghiúsul, a csomópontok neve lesz hozzáadva. A rendszer hozzáadja a ExecuteXML és a hibát a dokumentum végéhez az összes generált azonosító hozzáadásával.

Figyelje meg, hogy csak a hibát tartalmazó függvények lettek hozzáadva. A "0" hibaszám azt jelzi, hogy a függvény nincs végrehajtva. 

```
<Bhold>
  <Script>
    <functions>
      <function name="OrgUnitadd" description="OrgUnit1" parentid="root" orgtypeid="root" return="@ID@"/>     
      <function name="roleaddOrgUnit" OrgUnitid="OrgUnit1" roleid="Role1_OrgUnit1" return="@ID@"/>      
    </functions>
  </Script>
  <Function>
    <Name>OrgUnitadd</Name>
    <ExecutedXML>
      <function name="OrgUnitadd" description="OrgUnit1" parentid="root" orgtypeid="root" return="@ID@"/>
    </ExecutedXML>
    <Error Number="5" Description="Violation of UNIQUE KEY constraint 'IX_OrgUnits'. Cannot insert duplicate key in object 'dbo.OrgUnits'.
The statement has been terminated."/>
  </Function>
  <Function>
    <Name>roleaddOrgUnit</Name>
    <ExecutedXML>
      <function name="roleaddOrgUnit" OrgUnitid="OrgUnit1" roleid="Role1_OrgUnit1" return="@ID@"/>
    </ExecutedXML>
    <Error Number="0" Description=""/>
  </Function>
  <IDS>
    <ID name="@ID@">35</ID>
  </IDS>
</Bhold>
```

## <a name="id-parameters"></a>AZONOSÍTÓ paraméterek
Az azonosító paraméterek speciális kezelést kapnak. A nem numerikus értékek keresési értékként használatosak a megfelelő entitások kereséséhez a BHOLD-adattárban. Ha a keresési érték nem egyedi, akkor a rendszer visszaadja az első entitást, amely megfelel a keresési értéknek.

Ha meg szeretné különböztetni a numerikus keresési értékeket az azonosítók közül, lehetséges, hogy előtagot használ. Ha a keresési érték első hat karakterének értéke "no_id:", akkor ezek a karakterek a kereséshez használt érték előtt el lesznek megfosztotva. A (z) "%" SQL helyettesítő karakterek is használhatók.

A keresési érték a következő mezőket használja:

| AZONOSÍTÓ típusa   | Keresőmező |
|---|---|
| OrgUnitID | Leírás  |
| Szerepkörazonosítónak    | Leírás  |
| taskID    | Leírás  |
| userID    | DefaultAlias |

## <a name="script-access-and-permissions"></a>Parancsfájl-hozzáférés és-engedélyek
A parancsfájlok végrehajtásához a Active Server lapokon kiszolgálóoldali kód használható. Ezért a parancsfájlhoz való hozzáférés a következő lapok elérését jelenti. A BHOLDrendszer az egyéni lapok belépési pontjaival kapcsolatos információkat tart fenn. Ez az információ tartalmazza a Kezdőlap és a függvény leírását (több nyelvet is támogatnia kell).

A felhasználók jogosultak az egyéni lapok megadására és szkriptek végrehajtására. Az egyes belépési pontok feladatként jelennek meg. Minden olyan felhasználó, aki ezt a feladatot egy szerepkörön vagy egy egységen keresztül szerezte be, képes végrehajtani a megfelelő függvényt.

A menüben egy új függvény mutatja be a felhasználó által végrehajtható összes egyéni funkciót. Mivel egy parancsfájl műveleteket hajthat végre a BHOLD rendszerben a bejelentkezett felhasználótól eltérő identitás esetén. Engedélyt adhat egy adott művelet végrehajtásához anélkül, hogy a felügyeletet bármely objektumon át kellene adni. Ez például olyan alkalmazottak számára lehet hasznos, akik csak új ügyfeleket adhatnak meg a vállalatnak. Ezek a parancsfájlok önregisztráló lapok létrehozására is használhatók.

## <a name="command-script"></a>Parancs parancsfájlja
A parancs parancsfájlja a BHOLD rendszer által végrehajtott függvények listáját tartalmazza. A lista egy XML-dokumentumban van írva, amely megfelel a következő definícióknak:


|   Parancs parancsfájlja   |              `<functions>functions</functions>`               |
|--------------------|---------------------------------------------------------------|
|     funkciók      |                      függvény {Function}                      |
|      függvény      | <függvény neve = "függvénynév" functionParameters [Return] (/> \| > parameterList </function>) |
|    Függvénynév    | A következő szakaszokban leírtak szerint érvényes függvény neve. |
| functionParameters |                     { functionParameter }                     |
| functionParameter  |               parameterName = "parameterValue"                |
|   parameterName    |                    Egy érvényes paraméter neve.                    |
|   parameterValue   |                      @variable@ \| érték                      |
|       value        |                   Egy érvényes paraméterérték.                    |
|   parameterList    |          <parameters> Paraméterelemben  </parameters>          |
|   Paraméterelemben    | <parameter name="parameterName"> parameterValue </parameter>  |
|       visszatérési       |                      Return = " @variable @"                      |
|      változó      |                    Egy egyéni változó neve.                    |

Az XML a következő speciális karaktereket fordítja le:

| XML | Karakter |
|---|---|
| ```&amp;```  | & |
| ```&lt;```   | < |
| ```&gt;```   | > |
| ```&quot;``` | " |
| ```&apos;``` | ' |

Ezek az XML-karakterek az azonosítókban is használhatók, de nem ajánlottak.

A következő kód egy érvényes, három függvényt tartalmazó parancssori dokumentumot mutat be:

```
<functions>

   <functionname="OrgUnitAdd" parentID="34" description="Acme Inc." orgtypeID="5" return="@UnitID@" />

   <function name="UserAdd" description="John Doe" alias="jdoe" languageID="1" OrgUnitID="@UnitID@" />

   <function name="TaskAddFile" taskID="93" path="/customers/purchase">
      <parameters>
         <parameter name="history"> True</parameter>
      </parameters>
   </function>

</functions>
```

A **OrgUnitAdd** függvény egy UnitID nevű változóban tárolja a létrehozott egység azonosítóját. Ezt a változót a hozzáadási függvény bemenetként használja a rendszer. A függvény visszatérési értéke nincs használatban. A következő szakaszok ismertetik az összes rendelkezésre álló funkciót, a szükséges paramétereket és azok visszatérési értékeit.

## <a name="execute-functions"></a>Függvények végrehajtása
Ez a szakasz a végrehajtás függvények használatát ismerteti.

### <a name="abaattributeruleadd"></a>ABAAttributeRuleAdd
Hozzon létre egy új attribútum-szabályt egy adott attribútum típuson. Az attribútumok szabályait csak egyetlen attribútum típusához lehet csatolni.

A megadott attribútum-szabály az összes lehetséges attribútum típushoz csatolható. 

A Szabálytípus nem módosítható a "ABAattributeruletypeupdate" paranccsal. Az attribútum leírásának egyedinek kell lennie.


|    Argumentumok    |                                                                                                                                                                                                                  Típus                                                                                                                                                                                                                  |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   Leírás   |                                                                                                                                                                                                                  Szöveg                                                                                                                                                                                                                  |
|    Szabálytípus     | Határozza meg az attribútum szabályának típusát. Az attribútum-szabály típusától függően más argumentumokat is tartalmaznia kell. A következő szabálytípus-értékek érvényesek:<ul><li>0: reguláris kifejezés ("value" argumentum hozzáadása).</li><li>1: érték (adja hozzá a "operátor" és az "érték" argumentumot).</li><li>2: az értékek listája.</li><li>3: tartomány (adja hozzá a "rangemin" és a "RangeMax" argumentumot).</li><li>4: Age (argumentumok hozzáadása "operátor" és "érték").</li></ul> |
|  InvertResult   | ```["0"|"1"|"N"|"Y"]``` |
| AttributeTypeID |                                                                                                                                                                                                                  Szöveg                                                                                                                                                                                                                  |

| Nem kötelező argumentumok | Típus |
|---|---|
| Operátor | Szöveg <br>**Megjegyzés** : ez az argumentum kötelező, ha a **szabálytípus** értéke 1 vagy 4. A lehetséges értékek: "=", "<" vagy ">". Az XML-címkéknek " &gt; " > és "" értékkel kell rendelkezniük a következőhöz &lt; : "<". |
| RangeMin | Szám <br>**Megjegyzés** : ez az argumentum kötelező, ha a **szabálytípus** 3. |
| RangeMax | Szám <br>**Megjegyzés** : ez az argumentum kötelező, ha a **szabálytípus** 3. |
| Érték    | Szöveg <br>**Megjegyzés** : ez az argumentum kötelező, ha a **szabálytípus** 0, 1 vagy 4. Az argumentumnak numerikus vagy alfanumerikus értéknek kell lennie. |
| Visszatérési típus AttributeRuleID | Szöveg   |


### <a name="applicationadd"></a>applicationadd
Létrehoz egy új alkalmazást, és visszaadja az új alkalmazás AZONOSÍTÓját.

| Argumentumok | Típus |
|---|---|
| leírás |      |
| gép     |      |
| modul      |      |
| parameter   |      |
| protokoll    |      |
| username    |      |
| jelszó    |      |
| svroleID (nem kötelező)                | Ha ez az argumentum nem létezik, a rendszer az aktuális felhasználó felettesi szerepkörét használja. |
| Applicationaliasformula (nem kötelező) | Az alias képlettel aliast hozhat létre a felhasználó számára, ha az alkalmazáshoz hozzá van rendelve. Az alias akkor jön létre, ha a felhasználó már nem rendelkezik aliassal ehhez az alkalmazáshoz. Ha nincs megadva érték, a rendszer a felhasználó alapértelmezett aliasát használja aliasként az alkalmazáshoz. A képlet formátuma a következő: ` [<<objecttype>>.<<nameofobjecttypeattribute>>(startindexoffset,length offset)]` . Az eltolás nem kötelező. Csak a felhasználói és az alkalmazási attribútumok használhatók. Szabad szöveget lehet használni. A fenntartott karakterek a bal oldali szögletes zárójel ([) és a jobb oldali szögletes zárójel (]). Például: `[Application.bholdDescription]\[User.bholdDefAlias(1,5)]`. |
| Visszatérési típus | Az új alkalmazás azonosítója. |

### <a name="attributesetvalue"></a>AttributeSetValue
Az objektum típusához csatlakozó attribútum típusának értékét állítja be. Az objektum típusának és az attribútum típusának egyedinek kell lennie.

| Argumentumok | Típus |
|---|---|
| ObjectTypeID     | Szöveg |
| ObjectID         | Szöveg |
| AttributeTypeID  | Szöveg |
| Érték            | Szöveg |
| Visszatérési típus      | Típus |

### <a name="attributetypeadd"></a>AttributeTypeAdd
Beszúr egy új attribútum típusát/tulajdonság típusát.


|        Argumentumok        |                                               Típus                                                |
|-------------------------|---------------------------------------------------------------------------------------------------|
|       DataTypeID        |                                               Szöveg                                                |
| Leírás (= Identity) | Szöveg <br>**Megjegyzés** : a foglalt szavak nem használhatók, beleértve az "a", a "frm", az "id", a "usr" és a "bhold" karaktereket. |
|        MaxLength        |                                       Szám az [1,.., 255]                                        |
| ListOfValues (logikai)  |                                          ```["0"|"1"|"N"|"Y"]```                               |
|      DefaultValue       |                                               Szöveg                                                |
|       Visszatérési típus       |                                               Típus                                                |
|     AttributeTypeID     |                                               Szöveg                                                |

### <a name="attributetypesetadd"></a>AttributeTypeSetAdd
Beszúr egy új attribútum típusú készletet. Megköveteli, hogy az attribútum típusának leírása egyedi legyen.

| Argumentumok | Típus |
|---|---|
| Leírás (= Identity) |Szöveg |
| Visszatérési típus | Típus |
| AttributeTypeSetID | Szöveg |

### <a name="attributetypesetaddattributetype"></a>AttributeTypeSetAddAttributeType
Egy új attribútum típusának beszúrása egy meglévő attribútumérték-készletbe. Az attribútum típusának és az attribútum típusának egyedinek kell lennie.


|     Argumentumok      |                              Típus                              |
|--------------------|----------------------------------------------------------------|
| AttributeTypeSetID |                              Szöveg                              |
|  AttributeTypeID   |                              Szöveg                              |
|       Rendelés        |                             Szám                             |
|     Helyazonosító     | Szöveg <br>**Megjegyzés** : a hely "csoport" vagy "single" lehet. |
|     Kötelező      |                    ```["0"|"1"|"N"|"Y"]```                  |
|    Visszatérési típus     |                              Típus                              |

### <a name="objecttypeaddattributetypeset"></a>ObjectTypeAddAttributeTypeSet
Egy objektumtípus típusú attribútum hozzáadását adja hozzá. Az objektum típusának és az attribútum típusának egyedinek kell lennie. Az Objektumtípusok a következők: rendszer, OrgUnit, felhasználó, feladat.

| Argumentumok | Típus |
|---|---|
| ObjectTypeID | Szöveg | 
| AttributeTypeSetID | Szöveg | 
| Rendelés | Szám | 
| Látható | <ul><li>0: az attribútum típusának beállítása látható.</li><li>2: az attribútum típusa akkor látható, ha a **További információ** gomb van kiválasztva.</li><li>1: a set attribútum típusa nem látható.</li></ul> | 
| Visszatérési típus | Típus | 

### <a name="orgunitadd"></a>OrgUnitadd
Létrehoz egy új szervezeti egységet, és visszaadja az új szervezeti egység AZONOSÍTÓját.

| Argumentumok | Típus |
|---|---|
| leírás | | 
| orgtypeID | | 
| parentID | | 
| OrgUnitinheritedroles (nem kötelező) | | 
| Visszatérési típus | Típus | 
| Az új egység azonosítója | A OrgUnitinheritedroles paraméter <br>értéke Igen vagy nem. | 

### <a name="orgunitaddsupervisor"></a>OrgUnitaddsupervisor
Készítse el a felhasználót egy szervezeti egység felettesének.

| Argumentumok | Típus |
|---|---|
| svroleID | A userID azonosító is használható. Ebben az esetben az alapértelmezett felügyelői szerepkör van kiválasztva. Az alapértelmezett felügyelői szerepkör neve például **__svrole** , amelyet egy szám követ. A userID azonosító a visszamenőleges kompatibilitáshoz használható. | 
| OrgUnitID | | 

### <a name="orgunitadduser"></a>OrgUnitadduser
A felhasználó egy szervezeti egység tagja legyen.

| Argumentumok | Típus |
|---|---|
| userID | | 
| OrgUnitID | | 

### <a name="orgunitdelete"></a>OrgUnitdelete
Szervezeti egység eltávolítása.

| Argumentumok | Típus |
|---|---|
| OrgUnitID | | 

### <a name="orgunitdeleteuser"></a>OrgUnitdeleteuser
Eltávolít egy felhasználót egy szervezeti egység tagjaként.

| Argumentumok | Típus |
|---|---|
| userID | | 
| OrgUnitID | | 

### <a name="roleadd"></a>roleadd
Új szerepkört hoz létre.

| Argumentumok | Típus |
|---|---|
| Leírás | | 
| svrole | | 
| svroleID (nem kötelező) | Ha ez az argumentum nem létezik, akkor a rendszer az aktuális felhasználó felettesi szerepkörét használja. | 
| ContextAdaptable (nem kötelező) | ```["0","1","N","Y"]``` | 
| MaxPermissions (nem kötelező) | Egész szám | 
| MaxRoles (nem kötelező) | Egész szám | 
| MaxUsers (nem kötelező) | Egész szám | 
| Visszatérési típus | Típus | 
| Az új szerepkör azonosítója | | 

### <a name="roleaddorgunit"></a>roleaddOrgUnit
Szerepkört rendel egy szervezeti egységhez.


|    Argumentumok    |                                      Típus                                      |
|-----------------|--------------------------------------------------------------------------------|
|    OrgUnitID    |                                     Szerepkörazonosítónak                                     |
| inheritThisRole | a "true" vagy a "false" érték azt jelzi, hogy a szerepkör az alapul szolgáló egységeket javasolja-e. |

### <a name="roleaddrole"></a>roleaddrole
Szerepkört rendel egy másik szerepkör alszerepköréhez.

| Argumentumok | Típus |
|---|---|
| Szerepkörazonosítónak | | 
| subRoleID | | 

### <a name="roleaddsupervisor"></a>roleaddsupervisor
Felhasználó felettesének tétele a szerepkörben.

| Argumentumok | Típus |
|---|---|
| svroleID | A userID azonosító is használható. Ebben az esetben az alapértelmezett felügyelői szerepkör van kiválasztva. Az alapértelmezett felügyelői szerepkör neve például **__svrole** , amelyet egy szám követ. A userID azonosító a visszamenőleges kompatibilitáshoz használható. | 
| Szerepkörazonosítónak | | 

### <a name="roleadduser"></a>roleadduser
Szerepkört rendel egy felhasználóhoz. A szerepkör nem lehet a környezethez alkalmazkodó szerepkör, ha nincs megadva contextID.

| Argumentumok | Típus |
|---|---|
| userID | | 
| Szerepkörazonosítónak | | 
| durationType (nem kötelező) | A "Free", az "Hours" és a "Days" értékeket is tartalmazhatja. | 
| durationLength (nem kötelező) | Kötelező, ha a durationType "Hours" vagy "Days". tartalmaznia kell az egész értéket, amely azt az órát vagy napot jelöli, ameddig a szerepkör hozzá van rendelve egy felhasználóhoz. | 
| indítás (nem kötelező) | A szerepkör hozzárendelésének dátuma és időpontja. Ha ez az attribútum nincs megadva, a rendszer azonnal hozzárendeli a szerepkört. A dátumformátum éééé-hh-NNTóó: NN: mm, ahol csak az év, a hónap és a nap szükséges. például a "2004-12-11" és a "2004-11-28T08:00" érvényes értékek. | 
| Befejezés (nem kötelező) | A szerepkör visszavonásának dátuma és időpontja. Ha a durationType és a durationLength értéket adja meg, a rendszer figyelmen kívül hagyja ezt az értéket. A dátumformátum éééé-hh-NNTóó: NN: mm, ahol csak az év, a hónap és a nap szükséges. például a "2004-12-11" és a "2004-11-28T08:00" érvényes értékek. | 
| linkreason | Az indítás, a befejezés vagy az időtartam megadásakor kötelező, máskülönben figyelmen kívül hagyva. | 
| contextId (nem kötelező) | A szervezeti egység azonosítója, csak a kontextushoz alkalmazkodó szerepkörökhöz szükséges. | 

### <a name="roledelete"></a>roledelete
Egy szerepkör törlése.

| Argumentumok | Típus |
|---|---|
| Szerepkörazonosítónak | | 

### <a name="roledeleteuser"></a>roledeleteuser
Eltávolítja a szerepkör-hozzárendelést egy felhasználóhoz. Ez a parancs visszavonja a felhasználó által örökölt szerepköröket.

| Argumentumok | Típus |
|---|---|
| userID | | 
| Szerepkörazonosítónak | | 
| contextID (nem kötelező) |  | 

### <a name="roleproposeorgunit"></a>roleproposeOrgUnit
Egy olyan szerepkört javasol, amely egy OrgUnit tagjaihoz és OrgUnits rendel hozzá.

| Argumentumok | Típus |
|---|---|
| OrgUnitID | | 
| Szerepkörazonosítónak | | 
| durationType (nem kötelező) | A "Free", az "Hours" és a "Days" értékeket tartalmazhatja. | 
| durationLength | Akkor szükséges, ha a durationType "Hours" vagy "Days" értéknek kell lennie, és tartalmaznia kell azt az órák számát vagy a napokat, ameddig a szerepkör hozzá van rendelve egy felhasználóhoz. | 
| durationFixed | a "true" vagy a "false" érték azt jelzi, hogy a szerepkör hozzárendelése egy felhasználóhoz egyenlő-e a durationLength. | 
| inheritThisRole | a "true" vagy a "false" érték azt jelzi, hogy a szerepkör az alapul szolgáló egységeket javasolja-e. | 

### <a name="taskadd"></a>taskadd
Létrehoz egy új feladatot, és visszaadja az új feladat AZONOSÍTÓját.

| Argumentumok | Típus |
|---|---|
| applicationID | | 
| leírás | Legfeljebb 254 karakterből álló szöveg. | 
| feladatnév | Legfeljebb 254 karakterből álló szöveg. | 
| tokenGroupID | | 
| svroleID (nem kötelező) | Ha ez az argumentum nem létezik, akkor a rendszer az aktuális felhasználó felettesi szerepkörét használja. | 
| contextAdaptable (nem kötelező) | ```["0","1","N","Y"]``` | 
| kiépítés (nem kötelező) | ```["0","1","N","Y"]``` | 
| auditaction (nem kötelező) | <ul><li>0: ismeretlen (alapértelmezett)</li><li>1: ReportOnly</li><li>2: AlertAppAll</li><li>3: AlertAppObsolete</li><li>4: AlertAppMissing</li><li>5: EnforceAppAll</li><li>6: EnforceAppObsolete</li><li>7: EnforceAppMissing</li><li>8: AlertEnforceAppAll</li><li>9: AlertEnforceAppObsolete</li><li>10: AlertEnforceAppMissing</li><li>11: nem Portálos</li></ul> | 
| auditalertmail (nem kötelező) | A könyvvizsgáló elküldi az ezzel az engedéllyel kapcsolatos riasztásokat az e-mail-címre. Ha ez az argumentum nem jelenik meg, akkor a rendszer az auditor riasztási e-mail címét használja. | 
| MaxRoles (nem kötelező) | Egész szám | 
| MaxUsers (nem kötelező) | Egész szám | 
| Visszatérési típus | Az új feladat azonosítója. | 

### <a name="taskadditask"></a>taskadditask
Azt jelzi, hogy két feladat nem kompatibilis.

| Argumentumok | Típus |
|---|---|
| taskID | | 
| taskID2 | | 

### <a name="taskaddrole"></a>taskaddrole
Feladatot rendel hozzá egy szerepkörhöz.

| Argumentumok | Típus |
|---|---|
| Szerepkörazonosítónak | | 
| taskID | | 

### <a name="taskaddsupervisor"></a>taskaddsupervisor
Felhasználó felettesének tétele a feladathoz.

| Argumentumok | Típus |
|---|---|
| svroleID | A userID azonosító is használható. Ebben az esetben az alapértelmezett felügyelői szerepkör van kiválasztva. Az alapértelmezett felügyelői szerepkör neve például **__svrole** , amelyet egy szám követ. A userID azonosító a visszamenőleges kompatibilitáshoz használható. | 
| taskID | | 

### <a name="useradd"></a>hozzáadási
Létrehoz egy új felhasználót, és visszaadja az új felhasználó AZONOSÍTÓját.

| Argumentumok | Típus |
|---|---|
| leírás | | 
| alias | | 
| nyelvazonosítóval | <ul><li>1: angol</li><li>2: holland</li></ul> | 
| OrgUnitID | | 
enddate (nem kötelező) |A dátumformátum éééé-hh-NNTóó: NN: mm, ahol csak az év, a hónap és a nap szükséges. például a "2004-12-11" és a "2004-11-28T08:00" érvényes értékek. | 
| Letiltva (nem kötelező) | <ul><li>0: engedélyezve</li><li>1: letiltva</li></ul> | 
| MaxPermissions (nem kötelező) | Egész szám | 
| MaxRoles (nem kötelező) | Egész szám | 
| Visszatérési típus | Az új felhasználó azonosítója. | 

### <a name="useraddrole"></a>UserAddRole
Felhasználói szerepkört hoz létre.
<!--- missing content -->

| Argumentumok | Típus |
|---|---|
|  | | 

### <a name="userdeleterole"></a>UserDeleteRole 
Töröl egy felhasználói szerepkört.
<!--- missing content -->

| Argumentumok | Típus |
|---|---|
|  | | 

### <a name="userupdate"></a>Userupdate
Egy felhasználó frissítése.

| Argumentumok | Típus |
|---|---|
| UserID | | 
| Leírás (nem kötelező)|  | 
| language | <ul><li>1: angol</li><li>2: holland</li></ul> | 
| userDisabled (nem kötelező) |<ul><li>0: engedélyezve</li><li>1: letiltva</li></ul> | 
| UserEndDate (nem kötelező) | A dátumformátum éééé-hh-NNTóó: NN: mm, ahol csak az év, a hónap és a nap szükséges. például a "2004-12-11" és a "2004-11-28T08:00" érvényes értékek. | 
| firstName (nem kötelező) | | 
| middleName (nem kötelező) | | 
| lastName (nem kötelező) | | 
| maxPermissions (nem kötelező) | Egész szám |
| maxRoles (nem kötelező) | Egész szám | 


## <a name="getinfo-functions"></a>GetInfo függvények
Az ebben a szakaszban ismertetett függvények a BHOLD rendszeren tárolt információk beolvasására használhatók. Minden függvény hívható a GetInfo függvény használatával a BScript objektumból. Egyes objektumok paramétereket igényelnek. A visszaadott adat a megtekintési engedélyek és a bejelentkezett felhasználó felügyelt objektumaira vonatkozik.

### <a name="getinfo-arguments"></a>GetInfo argumentumai

| Név | Leírás |
|---|---|
| alkalmazások | Az alkalmazások listáját adja vissza. | 
| attributetypes | Attribútum-típusok listáját adja vissza. | 
| orgtypes | A szervezeti egység típusok listáját adja vissza. | 
| OrgUnits | A szervezeti egységek attribútumai nélkül adja vissza a szervezeti egységek listáját. | 
| OrgUnitproposedroles | A szervezeti egységhez kapcsolódó javasolt szerepkörök listáját adja vissza. | 
| OrgUnitroles | Az adott szervezeti egység közvetlenül társított szerepköreinek listáját adja vissza. | 
| Objecttypeattributetypes | | 
| engedélyek | | 
| permissionusers | | 
| szerepkörök | A szerepkörök listáját adja vissza. | 
| roletasks | A megadott szerepkör feladatainak listáját adja vissza. | 
| feladatok | A BHOLD által ismert összes feladatot adja vissza. | 
| felhasználók | A felhasználók listáját adja vissza. | 
| usersroles | Az adott felhasználó társított felettes szerepköreinek listáját adja vissza. | 
| userpermissions | Az adott felhasználó engedélyeinek listáját adja vissza. | 

### <a name="orgunit-info"></a>OrgUnit-információ

| Name (Név) | Paraméterek | Visszatérési típus |
|---|---|---|
| OrgUnit | OrgUnitID | OrgUnit | 
| OrgUnitasiattributes | OrgUnitID | Gyűjtemény | 
| OrgUnits| Filter (nem kötelező), proptypeid (nem kötelező)<br> Megkeresi azokat az egységeket, amelyek tartalmazzák a proptype **szűrő** részében leírt karakterláncot a **proptypeid** . Ha ez az azonosító nincs megadva, a szűrő az egység leírására vonatkozik. Ha nincs megadva szűrő, a rendszer az összes látható egységet visszaadja. | Gyűjtemény | 
| OrgUnitOrgUnits | OrgUnitID | Gyűjtemény | 
| OrgUnitparents | OrgUnitID | Gyűjtemény | 
| OrgUnitpropertyvalues | OrgUnitID | Gyűjtemény | 
| OrgUnitproptypes |  | Gyűjtemény | 
| OrgUnitusers | OrgUnitID | Gyűjtemény | 
| OrgUnitproposedroles | OrgUnitID | Gyűjtemény | 
| OrgUnitroles | OrgUnitID | Gyűjtemény | 
| OrgUnitinheritedroles | OrgUnitID | Gyűjtemény | 
| OrgUnitsupervisors | OrgUnitID | Gyűjtemény | 
| OrgUnitinheritedsupervisors| OrgUnitID | Gyűjtemény | 
| OrgUnitsupervisorroles | OrgUnitID | Gyűjtemény | 

### <a name="role-information"></a>Szerepkör adatai

| Name (Név) | Paraméterek | Visszatérési típus |
|---|---|---|
| szerepkör | Szerepkörazonosítónak | Objektum | 
| szerepkörök | szűrő (nem kötelező) | Gyűjtemény | 
| roleasiattributes | Szerepkörazonosítónak | Gyűjtemény | 
| roleOrgUnits | Szerepkörazonosítónak | Gyűjtemény | 
| roleparentroles | Szerepkörazonosítónak | Gyűjtemény | 
| rolesubroles | Szerepkörazonosítónak | Gyűjtemény | 
| rolesupervisors | Szerepkörazonosítónak| Gyűjtemény | 
| rolesupervisorroles | Szerepkörazonosítónak | Gyűjtemény | 
| roletasks | Szerepkörazonosítónak | Gyűjtemény | 
| roleusers | Szerepkörazonosítónak | Gyűjtemény | 
| rolesupervisorroles | Szerepkörazonosítónak | Gyűjtemény | 
| proposedroleOrgUnits | Szerepkörazonosítónak | Gyűjtemény | 
| proposedroleusers | Szerepkörazonosítónak | Gyűjtemény | 

### <a name="permission---task-information"></a>Engedély – feladat adatai

| Name (Név) | Paraméterek | Visszatérési típus |
|---|---|---|
| engedéllyel | TaskID | Engedély | 
| engedélyek | szűrő (nem kötelező) | Gyűjtemény | 
| permissionasiattributes | TaskID | Gyűjtemény | 
| permissionattachments | TaskID | Gyűjtemény | 
| permissionattributetypes | - | Gyűjtemény | 
| permissionparams | TaskID | Gyűjtemény | 
| permissionroles | TaskID | Gyűjtemény | 
| permissionsupervisors | TaskID | Gyűjtemény | 
| permissionsupervisorroles | TaskID | Gyűjtemény | 
| permissionusers | TaskID | Gyűjtemény | 
| feladat | TaskID | Feladat | 
| feladatok| szűrő (nem kötelező) | Gyűjtemény | 
| taskattachments | TaskID | Gyűjtemény | 
| taskparams | TaskID | Gyűjtemény | 
| taskroles | TaskID | Gyűjtemény | 
| tasksupervisors | TaskID | Gyűjtemény | 
| tasksupervisorroles | TaskID | Gyűjtemény | 
| taskusers | TaskID | Gyűjtemény | 

### <a name="user-information"></a>Felhasználói adatok

| Name (Név) | Paraméterek | Visszatérési típus |
|---|---|---|
| felhasználó! | UserID | Felhasználó | 
| felhasználók | Filter (nem kötelező), attributetypeid (nem kötelező) <br>Megkeresi azokat a felhasználókat, akik a AttributeType által megadott attributetypeid a szűrő által megadott karakterláncot tartalmazzák. Ha ez az azonosító nincs megadva, a szűrő a felhasználó alapértelmezett aliasára vonatkozik. Ha nincs megadva szűrő, a rendszer az összes látható felhasználót visszaadja. Például:<ul><li>`GetInfo("users")` az összes felhasználót adja vissza.</li><li>`GetInfo("users", "%dmin%")` a "DMin" karakterláncot tartalmazó összes felhasználót adja vissza az alapértelmezett aliasban.<br></li><li>Tegyük fel, hogy a felhasználók egy nevű további attribútummal rendelkeznek `"City".GetInfo("users", "%msterda%", "City")` . Ez a hívás visszaadja a "msterda" karakterláncot tartalmazó összes felhasználót a City attribútumban.</li></ul> | UserCollection | 
| usersapplications | UserID | Gyűjtemény | 
| Userpermissions | UserID | Gyűjtemény | 
| userroles | UserID | Gyűjtemény | 
| usersroles | UserID | Gyűjtemény | 
| userstasks | UserID | Gyűjtemény | 
| usersunits | UserID | Gyűjtemény | 
| usertasks | UserID | Gyűjtemény | 
| userunits | UserID | Gyűjtemény | 

### <a name="return-types"></a>Visszatérési típusok
Ebben a szakaszban a GetInfo függvény visszatérési típusait mutatjuk be.

| Name (Név) | Visszatérési típus |
|---|---|
| Gyűjtemény |```=<ITEMS>{<ITEM description="..." id="..." />}</ITEMS>``` | 
| Objektum |```=<ITEM type="…" description="..." />``` | 
| OrgUnit |```= <ITEM id="…" description="..." orgtype="..." parent="..."> <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Engedély |```= <ITEM id="…" description="…" name="…" tokengroup="…" application="…" > <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Szerepkörök |```= <ITEMS> {<ITEM id="…" description="…" />} </ITEMS>``` | 
| Role |```= <ITEM id="…" description="… " > <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Feladat | Megtekintési engedély | 
| Felhasználók | ```= <ITEMS> {<ITEM description="…" id="…" alias="…" />} </ITEMS>``` | 
| Felhasználó |```= <ITEM id="…" description="…" alias="…" firstname="…" lastname="…" uuid="…" language="…"> <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 

## <a name="script-sample"></a>Parancsfájl mintája

Egy vállalat BHOLD-kiszolgálóval rendelkezik, és egy automatikus parancsfájlt szeretne, amely új ügyfeleket hoz létre. A vállalattal és a vásárlási kezelőjével kapcsolatos információk testreszabott weblapokon jelennek meg. Minden ügyfél a modellben egységként jelenik meg az egység ügyfelei számára. A Purchase Manager ezen egység felettesének is tagja. Létrejön egy szerepkör, amely lehetővé teszi a tulajdonosoknak, hogy megvásárolják az új ügyfél nevét.

Ez az ügyfél azonban nem létezik az alkalmazásban. Az ASI FunctionDispatch egy speciális függvényt alkalmaz, amely új felhasználói fiókot hoz létre a vásárlási alkalmazásban. Minden ügyfélhez tartozik egy ügyfél típusa.

A lehetséges típusokat a FunctionDispatch függvény is bemutatja. Az AA kiválasztja a megfelelő típust az új ügyfél számára.

Hozzon létre egy szerepkört és feladatot a vásárlási jogosultságok bemutatásához. Az ASI a valódi vásárlási jogosultságot fájlként mutatja be `/customers/customer id/purchase` . Ezt a fájlt össze kell kapcsolni az új feladattal.

Az adatokat gyűjtő Active Server lap így néz ki:

```VB
<%@ Language=VBScript %>
<% Option Explicit %>
<html>
<body>
<form action="MySubmit.asp" method=post>
<input type="hidden" name="OrgUnitID" 
     value="<% = Request("ID") %>">
Company <input type="text" name="Description"> <br>
Type <select name="OrgType">
<%Dim oOrgType
For Each oOrgType on bscript.getinfo("Orgtypes") %>
<option value="<% = oOrgType.OrgTypeID %>">
<% = oOrgType.Description %>
</option> <%
Next %>
</select>  <br>
Manager <input type="text" name=" manager"> <br>
Alias <input type=" text" name=" alias"> <br>
e-mail <input type=" text" name=" email"> <br>
<input type="submit">
</form>
</body>
</html>
```

Az összes testreszabott oldalnak a megfelelő információkra vonatkozó kérést kell tennie, és létre kell hoznia egy XML-dokumentumot a kért információkkal. Ebben a példában a MySubmit lap átalakítja az XML-dokumentumba tartozó adatfájlokat, hozzárendeli azt a **b1script. Paraméterek** objektum, és végül meghívja a `b1script.ExecuteScript("MyScript")` függvényt.

Az alábbi bemeneti szkript a következő példát mutatja be:

```
<customer>
<description>ACME inc.</description>
<orgtype>5<orgtype>
<name>John Doe</name>
<alias>jdoe</alias>
<email>jdoe@acme.com</email>
</customer>
```

Ez a bemeneti parancsfájl nem tartalmaz parancsokat a BHOLD. Ennek az az oka, hogy ezt a parancsfájlt nem közvetlenül a BHOLD hajtja végre. Ehelyett ez egy előre definiált függvény bemenete. Ez az előre definiált függvény lefordítja ezt az objektumot egy BHOLD-parancsokat tartalmazó XML-dokumentumra. Ez a mechanizmus visszatartja a felhasználótól, hogy parancsfájlokat küldjön a BHOLD rendszernek, amely olyan függvényeket tartalmaz, amelyeket a felhasználó nem futtathat, például a setUser és a functions egy ASI-ra történő kiosztását.

```
<?xml version="1.0" encoding="utf-8" ?> 
- <functions xmlns="http://tempuri.org/BscriptFunctions.xsd">

  <function name="roleadduser" roleid="" userid="" /> 
  <function name="roledeleteuser" roleid="" userid="" /> 
  </functions>
```

## <a name="next-steps"></a>Következő lépések
- [Útmutató a BHOLD-fogalmakhoz](../bhold/bhold-concepts-guide.md)
- [A BHOLD korábbi verziói](version-bhold-history.md)
