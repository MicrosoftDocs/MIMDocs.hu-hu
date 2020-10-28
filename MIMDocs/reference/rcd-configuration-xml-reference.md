---
title: Az erőforrás-vezérlő megjelenítési konfigurációjának XML-referenciája | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e0912d09a0fd180be784b3eeefc17d5fd0449a3a
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760918"
---
# <a name="resource-control-display-configuration-xml-reference"></a>Erőforrás-vezérlő megjelenítési konfigurációs XML-hivatkozása

Az erőforrás-vezérlés megjelenítési konfigurációs (RCDC) erőforrásai olyan felhasználó által definiált erőforrások, amelyek segítségével szabályozható, hogy a Microsoft Identity Manager 2016 SP1 () adattárban lévő más erőforrások hogyan jelenjenek meg a felhasználói felületen (UI) a végfelhasználó számára. Minden RCDC-erőforrás tartalmaz egy XML-konfigurációs fájlt, amelyet a felhasználói felületi szöveg és a felhasználói felület vezérlőelem hozzáadására, módosítására vagy eltávolítására válthat. Míg a RCDC 2016 SP1 számos alapértelmezett erőforrást biztosít, egyéni RCDC-erőforrásokat is létrehozhat egyéni erőforrásokhoz. További információ a RCDC felhasználói felületének a FIM-portálon való használatáról: [Bevezetés a FIM-portál konfigurálásához és testreszabásához](http://go.microsoft.com/fwlink/?LinkID=165848) a FIM dokumentációjában.


## <a name="known-issues"></a>Ismert problémák

Az alapértelmezett érték számos RCDC vezérlőelemben nem támogatott.

Ebben a kiadásban az erőforrás-vezérlőkben az alapértelmezett értékek beállítása nem támogatott, kivéve a kapcsoló gomb vezérlőelemet. Ezt a problémát megkerülheti egy legördülő listához egy olyan alapértelmezett érték megadásával, amely nincs olyan értékhez társítva, amely kényszeríti a felhasználót, hogy módosítsa a kijelölést. Ha más vezérlőkkel szeretné megkerülni a problémát, egy engedélyezési munkafolyamattal kell megadnia az alapértelmezett értéket a kérelem elküldésekor.

## <a name="basic-structure"></a>Alapszintű struktúra

Egy RCDC-erőforrás XML-adatkészlete egyetlen **ObjectControlConfiguration** XML-elemet tartalmaz.

>[!NOTE]
>A teljes XSD-séma esetében lásd <a href="#appendix-a">: A függelék: alapértelmezett XSD-séma</a>.

A **ObjectControlConfiguration** elem XSD-sémája a következő:

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```

A **ObjectControlConfiguration** elem a következő elemeket tartalmazza:

- **ObjectDataSource** : ez az elem határozza meg az erőforrás-vezérlő (RC) által használt adatforrás-osztály TypeName paraméterét. A leírás és a séma definíciójának megtekintéséhez tekintse meg a jelen dokumentum következő adatforrások című szakaszát. Egy **ObjectControlConfiguration** -elem legfeljebb 32 csomópontot tartalmazhat a **ObjectDataSource** elemből.

- **XmlDataSource** : ez egy egyszerű adatforrás, amelyet leggyakrabban az összefoglalás oldal kialakításának megadására használnak. A leírás és a séma definíciójának megtekintéséhez tekintse meg a jelen dokumentum következő adatforrások című szakaszát. Egy **ObjectControlConfiguration** : az elem legfeljebb 32 csomópontot tartalmazhat a **XmlDataSource** elemben.

- **Panel** : a rendszergazda testreszabhatja a RCDC oldal elrendezését, ha módosítja a panel elemein belüli elemeket. További információt a jelen dokumentum későbbi részében, a panel szakaszban talál. Egy **ObjectControlConfiguration** elemnek csak egy panel elemből kell állnia.

- **Események** : a rendszergazdák nem biztosíthatnak testreszabott kódokat, ez a funkció korlátozott. Ez az az esemény, amelyet egy panel vagy egy vezérlő bocsáthat ki, az állapot változása alapján. További információ: a jelen dokumentum későbbi részében található események szakasz. Egy **ObjectControlConfiguration** elem opcionálisan egy **Event** elemet is tartalmazhat. Általánosságban elmondható, hogy az egyéni **események** használata csak akkor támogatott, ha a későbbi fejlesztésekben nem kifejezetten fejlesztettük őket.

## <a name="data-sources"></a>Adatforrások

A Microsoft Identity Manager adatforrásokat használ a felhasználói felületi összetevőkhöz való adatkötéshez. Ez segít megkönnyíteni az adatok elkülönítését a bemutató rétegből. A RCDC erőforrás-konfigurációs adatforrások két típusa létezik: **ObjectDataSource** és **XmlDataSource** .

-   A **ObjectDataSources** olyan Microsoft .net osztályt ad meg, amely az RC-nek szolgáltatja az adatkészletet. A rendelkezésre álló ObjectDataSources rögzített készlete biztosított, hogy a rendszergazda a RCDCs készítésekor is használni tudja a használatot.

-   A **XMLDataSources** egyszerű módot biztosít az XML-alapú adatszerkezetek kialakítására, és a rendszergazdák használhatják a testreszabott adatmennyiséget. Az XML-adatnak közvetlenül a RCDC kell megadnia, kivéve, ha a beépített, előre definiált XML-struktúrát használja. A beépített XML-struktúra az RC összefoglaló lapjainak létrehozásához használatos.

A RCDC az adatforrásokat a RCDC megadott felhasználói felületi vezérlők attribútumaihoz kötheti a felhasználói felület létrehozásához.

### <a name="objectdatasource-elements"></a>ObjectDataSource elemek

A Microsoft Identity Manager a következő táblázatban található általános adatforrás-típusokat biztosítja, amelyek az összes erőforrástípus számára elérhetők (kivéve, ha ez szerepel).

| TypeName | Leírás | Kétirányú kötés | Kötési szintaxis |
|---|---|---|---|
| PrimaryResourceObjectDataSource | Ez a létrehozott, szerkesztett vagy megtekintett FIM 2010-erőforrást jelöli. A kötési karakterlánc elérési útja az attribútum neve. Az erőforrás típusát a RCDC TargetObjectType attribútuma adja meg, nem pedig a RCDC.ConfigurationData attribútumban. | Igen | `[AttributeName]` a neve alapján megadott Object attribútum értéke. |
| PrimaryResourceDeltaDataSource  | Ez az adatforrás létrehozza a különbözeti XML-t, amely összehasonlítja az eredeti állapotot és a FIM 2010-erőforrás aktuális állapotát. A generált különbözeti XML-t az RC összegző vezérlő használja, hogy a felhasználói felület a felhasználó által küldött kérelem számára legyen megjelenítve. | Nem | `DeltaXml` Ez az összegző vezérlővel együtt használható a különbözet megjelenítéséhez. |
| PrimaryResourceRightsDataSource | Ez az adatforrás a FIM 2010-erőforrás egyes attribútumaihoz biztosít beágyazott jogokat. Ez lehetővé teszi, hogy az RC előre meghatározza, hogy a felhasználó milyen engedélyekkel rendelkezik az adott attribútumon, majd megfelelően jelenítse meg az adott attribútum felhasználói felületét. | Nem | `[AttributeName]` |
| SchemaDataSource                | Ez az adatforrás használható a sémával kapcsolatos információk eléréséhez, például a megjelenítendő név, a leírás, az attribútum megadása kötelező, valamint az erőforrástípus adatai. | Nem | `[AttributeName].Required` Logikai érték, amely azt jelzi, hogy az attribútumnak érvényes értékkel kell rendelkeznie. <br/> `[AttributeName].DisplayNameString` A kötés megjelenítendő nevét jelző érték. <br/> `[AttributeName].DescriptionString` A kötés leírását jelző érték. <br/>`[AttributeName].StringRegexString` egy kötés karakterláncos Regexjét jelző érték. <br/> `[AttributeName].DisplayName` <br/> `[AttributeName].Description` <br/> `[AttributeName].IntegerValueMinimum` <br/>`[AttributeName].IntegerValueMaximum` <br/>`[AttributeName].LocalizedAllowedValues` |
| DomainDataSource                | Ez az adatforrás tartományok enumerálását biztosítja a tartományi konfigurációs erőforrások alapján. Ezt az adatforrást csak a RCDCs és a felhasználói erőforrások esetében lehet használni. | Igen | Tartomány |

Az alábbi példa olyan RCDC kódrészletet mutat be, amely három adatforrást köt össze a UocTextBox vezérlővel egy csoport Description (Leírás) attribútumának szerkesztéséhez:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource-element"></a>XMLDataSource elem

**XMLDataSource** elemek használatával megadhatja azokat az egyéni adatértékeket, amelyeket a RCDC használhat egy adott erőforráshoz. Ebben az esetben az XML-adatértékeket meg kell adni a RCDC. Alternatív megoldásként az adatforrás használható egy beépített XML-adatstruktúra hivatkozására, amely az összegző lapok felhasználói felületét jeleníti meg. Ön határozza meg, hogy milyen típusú **XMLDataSource** kell használni a RCDC.


| TypeName | Leírás | Kétirányú kötés | Kötési szintaxis |
|---|---|---|---|
| **XMLDataSource**            | Az adatforrás XML-adatforrást jelöl. Az adattípusok XSL vagy Embedded XSL formátumban is lehetnek:<ul><li>XSL formátum a Microsoft.IdentityManagement.WebUI.Controls.dllban:<br/>```<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement. WebUI.Controls.Resources.DefaultSummary.xsl"> </my:XmlDataSource>```</li><li>Beágyazott XSL-formátum:<br/>```<my:XmlDataSource my:Name="RequestStatusTransformXsl"><xsl:stylesheet version="1.0" xmlns:xsl=http://www.w3.org/1999/XSL/Transform xmlns:msxsl="urn:schemas-microsoft-com:xslt"></xsl:stylesheet></my:XmlDataSource>```</li></ul>  | Nem | `Xpath[;namespaces]`ahol `Xpath` egy érvényes XML XPath a szükséges Megjegyzés kiválasztásához, leggyakrabban "/" (root). `namespaces` az előtag = URI karakterláncok választható listája. A karakterláncot pontosvesszővel kell elválasztani, ha az XPath a névtérbeli XML-fájlon való működéshez szükséges. |
| **ReferenceDeltaDataSource** | Az adatforrás a többértékű hivatkozási attribútumok különbözeteit jelöli. Csak a Group és a set RCDC esetében használatos. <br/> Bár az adatforrás nem korlátozódik a csoportokra vagy a készletekre, a RCDC-gazdagépen programkód-módosításokat kell elküldenie az ilyen különbözetek beküldéséhez. Jelenleg a Group és a set az egyetlen olyan gazdagép, amely felismeri ezt az adatforrást.  | Igen | `[AttributeName].Add` ahol a a `[AttributeName]` hivatkozási attribútum és a visszaadott adatértékek jelentik a Delta-kiegészítéseket.<ul><li>Például: `[ReferenceAttribute].Add`</li><li>Például: `<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}"/>`</li></ul>`[AttributeName].Remove` ahol a a `[AttributeName]` hivatkozási attribútumot jelöli, és a visszaadott adatértékek a különbözeti eltávolítások. <br/> DeltaXml <!-- Is bold formatting needed for DeltaXml? --> |
|**RequestDetailsDataSource**| Az adatforrás a kérelmek objektumainak RequestParameter attribútumát jelöli. A paraméter beállítja a többértékű attribútumoknál megjelenítendő attribútumérték maximális számát, amelyet a rendszer csak a RCDC használ. `<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />`| Nem | DeltaXml |
|**RequestStatusDataSource**| Az adatforrás a kérelmek objektumainak **RequestStatusDetails** attribútumát jelöli. Csak a RCDC esetében használatos. | Nem | DeltaXml |

Egyéni XML-adatforrás definiálásához használja a következő XML-kódot:

 ```XML
<my:XmlDataSource my:Name="MyCustomData" >
     %Insert custom, properly formatted XML data here%
</my:XmlDataSource>
```

A beépített összefoglalás-vezérlő XSL használatához adja meg az adatforrást az alábbiak szerint:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

Ha egyéni erőforrástípus RCDC hoz létre, ezzel a módszerrel automatikusan megjelenítheti az adott egyéni erőforrás összefoglaló lapját.

Az alábbi példa bemutatja, hogyan hozhat létre összefoglalás fület a RCDC, a **PrimaryResourceDeltaDataSource** elemet használva a **XMLDataSource** elemmel a beépített XSL használatával:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

Alternatív megoldásként a felhasználó lecserélheti a korábban megadott XmlDataSource elemet a következő formátummal az összefoglalás lap testreszabott elrendezésének definiálásához. Hivatkozásként az alapértelmezett FIM 2010 Summary XSL a B függelék: alapértelmezett Summary XSL, a jelen dokumentum későbbi részében található.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Adatforrások sémája
A következő XSD-séma létrehozza a két típusú adatforrást:

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="event-element"></a>Event elem
Az **Event** elem határozza meg a vezérlő változó állapotát. A funkció bővíthetősége korlátozott, mert nem írhat egyéni függvényt (kezelőt) annak meghatározásához, hogy a viselkedés milyen hatással van az esemény indítására. Ugyanezt az eseményt a panel elemében is használhatja. További információt a jelen dokumentum későbbi részében, a panel szakaszban talál.

Az Event elem XSD-sémája a következő:

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```

Egy **esemény** üres elem, és a következő attribútumokkal rendelkezik:

- **Név** : ez az esemény egyedi neve. A **ObjectControlConfiguration** egyetlen támogatott esemény a betöltési esemény. Ez az esemény akkor aktiválódik, amikor a lapot először betöltik.

- **Kezelő** : Ez a kezelő egyedi neve. Az esemény indításakor általában egy program metódust kell meghívni a vezérlő állapotának módosítására. A következő esetek nem támogatottak:

   - Meglévő kezelő eltávolítása egy meglévő vezérlőből.
   - Új kezelő létrehozása.
   - Kezelő csatolása meglévő vagy új vezérlőelemhez.

Az alábbi példa egy **esemény** elemet mutat be:

```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

## <a name="panel-element"></a>Panel elem
A **panel** elem a RCDC elrendezésének alapvető eleme. A panel elemének XSD-sémája a következő:

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

A **panel** elem ismétlődő elemet tartalmaz, **csoportosítást** . További információt a jelen dokumentum csoportosítási szakasza tartalmaz.

A panel elemének jellemzői a következők:

- **Name (név** ): a panel neve. Ez egy kötelező, karakterlánc típusú attribútum.

- **DisplayAsWizard** : ez az attribútum jelenleg elavult. A RCDC megfelelő VerbContext attribútuma szabályozza, hogy az erőforrás-elrendezés varázsló módban vagy Tab módban van-e. Ha a beállítás értéke 0 (létrehozási mód), akkor a varázsló mód is. Ellenkező esetben tabulátor módban van. További információ: Bevezetés a FIM-portál konfigurálásához és testreszabásához a dokumentációban.

- **Caption** : ez az attribútum jelenleg elavult. A felhasználó megadhatja a lapok feliratait úgy, hogy olyan csoportot is megad, amely csak fejléc-információkat tartalmaz. További információt a jelen dokumentum csoportosítási szakasza tartalmaz.

- **AutoValidate** : ez egy opcionális logikai attribútum. Ha az igaz értékre van állítva, a rendszer az aktuális lapon lévő összes vezérlőn aktiválja az ellenőrzést. Ha hiányzik az attribútum, alapértelmezés szerint igaz értékre van állítva. A szolgáltatás a válaszban tulajdonsággal együtt használható. További információ: "válaszban" a jelen dokumentum későbbi részében.

## <a name="grouping-element"></a>Csoportosítási elem
A **csoportosítási** elem a panel teljes elrendezését határozza meg. Olyan tárolóként működik, amely az egyes vezérlőket különböző területekhez és lapokhoz csoportosítja. A csoportosítási elem XSD-sémája a következő:

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```

Háromféle **csoportosítási** elem létezik:

- **Fejléc csoportosítása** : a fejlécek csoportosítása nem kötelező. Egy **panelen** csak egy fejléc-csoportosítás lehet. A panel tetején egy fejléc jelenik meg feliratként. Ebben a csoportosításban csak egy UocCaptionControl lehet használni. A fejlécek csoportosítására példát a minta szakasza tartalmaz.

- **Tartalom csoportosítása** : legalább egy tartalom-csoportosításra van szükség. Egy panelen több tartalom csoportosítása is lehetséges. A tartalom csoportosítása egy RCDC lap fő tartalmaként jelenik meg. Minden tartalom csoportosítása egy lapon jelenik meg ugyanabban a panelen, és 1 – 256 vezérlővel rendelkezhet. Tekintse meg a példák szakaszt a **tartalom csoportosítására** .

- **Összesítő csoportosítás** : az összegző csoportosítás nem kötelező. Egy panelen csak egyetlen összegző csoportosítás lehet. Egy összefoglaló csoportosítás megjelenik egy panel utolsó lapján. Egy összegző csoportosításban csak egy **UocHtmlSummary** -vezérlő használható a kérés elküldése előtt a felhasználó által végrehajtott módosítások megjelenítéséhez. Tekintse meg az összefoglalás csoportosítására vonatkozó példát a példák szakaszban.

Mindegyik csoportosítási típus a következő elemeket tartalmazza:

- **Súgó** : ez az elem Súgó szöveget tartalmaz egy lapon. Azt is megteheti, hogy hozzáad egy, a laphoz tartozó súgófájl hivatkozását.

- **Vezérlők** : az elemmel kapcsolatos további információkért tekintse meg a jelen dokumentum vezérlő szakaszát. Minden csoportosításhoz a csoportosítás típusától függően 1 – 256 vezérlőnek kell tartoznia.

- **Események** : az elemmel kapcsolatos további információkért tekintse meg a jelen dokumentum események szakaszát. Az egyes Csoportosítások lehetőségként egy eseményt is tartalmazhatnak. A csoportosítási elemekben támogatott események a következők:

    - **BeforeLeave** : ezt az eseményt akkor indítja el a rendszer, ha a felhasználó készen áll egy lap egy tartalmi csoportosításban való elhagyására.
    - **AfterEnter** : ez az esemény akkor aktiválódik, ha a felhasználó készen áll egy lap megadására egy tartalom-csoportosításban.

A csoportosítás a következő hét attribútumot tartalmazhatja:

- **Név** : Ez a csoport kötelező neve. A **névnek** egyedinek kell lennie a **panelen** belül.

- **Caption** : a **felirat** fejléc-feliratként jelenik meg a fejlécben. Megjelenik a tartalom vagy az összesítő csoport lap felirata.

- **Leírás** : egy nem kötelező karakterlánc-attribútum, a **Leírás** csak akkor működik, ha egy tartalom-csoportosításban van használatban. Ezzel az elemmel részletesen megadhatja a végfelhasználónak az ugyanazon a lapon található információkat.

    >[!NOTE]
    >Ha ez az attribútum egy összegző csoportosításban van használatban, az XML érvénytelennek tekintendő. Ha ezt az attribútumot használja egy fejléc-csoportosításban, az XML-fájl érvényes, de figyelmen kívül lesz hagyva.
    >

- **Engedélyezve** : egy opcionális logikai attribútum, amely engedélyezve van, értéke TRUE (igaz), ha hiányzik. Ha az engedélyezve érték hamis, a végfelhasználó letiltott lapot lát. Ez az attribútum csak a tartalmi csoportosításban működik.

    >[!NOTE]
    >Ha ez az attribútum egy összegző csoportosításban van használatban, az XML érvénytelennek tekintendő. Ha ezt az attribútumot használja egy fejléc-csoportosításban, az XML-fájl érvényes, de figyelmen kívül lesz hagyva.
    >

- **Látható** : az attribútum hamis értékre állításával elrejtheti az RCDC lapjait vagy annak fejlécét. Alapértelmezés szerint ez a nem kötelező, logikai típusú attribútum igaz értékre van állítva. Ez az attribútum csak a tartalom csoportosításakor működik.

    >[!NOTE]
    >Ha egy panelen csak egyetlen tartalom van csoportosítva, ez a funkció nem működik. Ha a panelen egynél több tartalom van csoportosítva, az a korábban leírtaknak megfelelően viselkedik.

- **IsHeader** : ez az attribútum egy nem kötelező, logikai attribútum, amely meghatározza, hogy a csoportosítás a fejlécek csoportosítása-e. Ha ez az attribútum nincs megadva, az értéke hamis.

- **IsSummary** : ez egy nem kötelező, logikai attribútum, amely meghatározza, hogy a csoportosítás összesítő csoportosítás-e. Ha ez az attribútum nincs megadva, az értéke hamis.

### <a name="examples-for-types-of-grouping-elements"></a>Példák a csoportosítási elemek típusaira
Ez a szakasz példákat tartalmaz a groups elemre.

#### <a name="example-header-grouping"></a>Példa: fejléc csoportosítása
Az alábbi ábrán egy minta fejléc-csoportosítás látható:

![Fejléc csoportosítása](media/rcd-configuration-xml-reference/image005.jpg)

A következő XML egy minta fejléc-csoportosítást hoz létre. Az XML-ben a fejléc csoportosítása a "minta fejlécének csoportosítása" feliratú szöveg.

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

#### <a name="example-content-grouping"></a>Példa: tartalom csoportosítása
Az alábbi ábrán egy példaként szolgáló tartalom csoportosítása látható:

![Tartalom csoportosítása](media/rcd-configuration-xml-reference/image007.jpg)

A következő XML egy minta tartalmú csoportosítást hoz létre. Az XML-ben a tartalom csoportosítása a "Sample Content grouping" feliratot tartalmazó rész.

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

#### <a name="example-summary-grouping"></a>Példa: összefoglalás csoportosítása

Az alábbi ábrán egy minta összegző csoportosítás látható:

![Összefoglalás csoportosítása](media/rcd-configuration-xml-reference/image010.jpg)

A következő XML egy minta összegző csoportosítást hoz létre. Az XML-ben az összesítő csoportosítás a "Sample Summary grouping" feliratú szöveg.

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```

### <a name="help-element"></a>Súgó elem
A **Súgó** elem egy csoportba vagy egy vezérlőelem-elembe is felvehető választható elemként. Ha egy csoportosításban van használatban, az első elemnek kell lennie. Szöveges segítséget nyújt a végfelhasználóknak a pontos információk megadásához. A következő XSD-séma a Súgó elemhez tartozik:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```

A következő XML-mintakód létrehoz egy Súgó elemet:

```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control-element"></a>Vezérlőelem elem
Egy csoportosítási elem egy vagy több **vezérlő** elemet tartalmaz. A vezérlőelemek a RCDC fő elemei. A csoportosítási elem testreszabható a benne található különböző vezérlőelem-elemek definiálásával. A következő XSD-séma a vezérlő elemhez tartozik:

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

A vezérlő elem a következő elemeket tartalmazza:

- **Súgó** : ezt az elemet a rendszer figyelmen kívül hagyja. Csak a csoportosításban működik.

- **CustomProperties** : ez az elem nem támogatott.

- **Beállítások** : ez az elem csak a **UocDropDownList** vagy a **UocRadioButtonList** vezérlőkkel együtt használható. Nem működik más vezérlőkkel. Az elem struktúrájáért tekintse meg a jelen dokumentum beállítások szakaszát. Tekintse meg a jelen dokumentum egyes vezérlők szakaszát, és tekintse meg, hogyan használja a vezérlők a beállításokat.

- **Gombok** : ez az elem csak a **UocListView** vezérlővel együtt használható. Más vezérlők esetében nem működik. További információkért tekintse meg a jelen dokumentum UocListView című szakaszát.

- **Tulajdonságok** : ez az elem minden vezérlőben használható a vezérlők további viselkedésének megadásához. Az elemmel kapcsolatos további információkért tekintse meg a jelen dokumentum tulajdonságok szakaszát.

- **Események** : az elem struktúrája a jelen dokumentum korábbi, események szakaszában található. Tekintse meg a jelen dokumentum egyes vezérlők szakaszát, amelyből megtudhatja, hogy mely események vannak használatban a vezérlőben.

A vezérlő elem a következő 10 attribútumot tartalmazhatja:

- **Név** : Ez a vezérlő neve. A vezérlőelemek nevének az egyes paneleken belül egyedinek kell lennie. Ez egy kötelező, karakterlánc típusú attribútum.

- **TypeName** : ez az attribútum határozza meg, hogy milyen típusú vezérlő van. Ez egy kötelező, karakterlánc típusú attribútum. Tekintse meg a jelen dokumentum egyes vezérlők szakaszát minden egyes vezérlőelem nevénél.

- **Felirat** : ezt az attribútumot használhatja a vezérlő feliratának felvételéhez. A felirat általában a vezérlőelem által megjelenített vagy beléptetésre kerülő adatmegjelenített név. Explicit módon megadhatja a felirat értékét, vagy megadhatja azt a séma attribútum megjelenítendő neve információval. A felirat a normál méretű vezérlőelem bal szélső oldalán jelenik meg. Ha egy vezérlőelem a teljes képernyőre kiterjed, megjelenik a felirat a vezérlőn. Ez egy opcionális, karakterlánc típusú attribútum. További információ az adatforrások attribútummal vagy tulajdonság értékkel való kötéséről: Properties (Tulajdonságok) szakasz.

    Az alábbi példa azt szemlélteti, hogyan lehet explicit módon használni a feliratokat:

    ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
    ```

    Az alábbi példa bemutatja, hogyan használható egy felirat egy adatforrással. Ha a dokumentumban korábban bemutatott adatforrás sablonját használta, az adatforrás séma. Azt javasoljuk, hogy az attribútum DisplayName paraméterét egy Caption attribútummal kösse.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```

- **Engedélyezve** : ez egy nem kötelező, logikai típusú attribútum. Ha ezt az attribútumot hamis értékre állítja, akkor a felhasználó letilthatja a vezérlőt. Az alapértelmezett érték TRUE (igaz).

- **Látható** : ez egy nem kötelező, logikai típusú attribútum. Ez az attribútum a teljes vezérlő elrejtésére használható. Az alapértelmezett érték TRUE (igaz).

- **Leírás** : használja ezt a nem kötelező karakterlánc-attribútumot, hogy tartalmazzon egy leírást, amely segít a végfelhasználónak megérteni, hogy mit kell tenni a vezérlőben, vagy mi a vezérlő. Explicit módon megadhatja a Leírás értékét, vagy összekapcsolhatja azt a séma attribútumának leírási adataival.

    A leírás egy normál méretű vezérlőelem bal szélső oldalán jelenik meg a felirat alatt. Ha egy vezérlőelem a teljes képernyőre kiterjed, a leírás megjelenik a vezérlő tetején a felirat alatt. Az adatforrások attribútummal vagy tulajdonsággal való kötésével kapcsolatos információkért tekintse meg a jelen dokumentum tulajdonságok szakaszát.

    Az alábbi példa azt szemlélteti, hogyan használható a Leírás explicit módon:

    ```XML
    <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
    ```

    Ez a példa azt szemlélteti, hogyan használható a leírás egy adatforrással. Ha a dokumentumban korábban bemutatott adatforrás sablonját használta, az adatforrás **séma** . Javasoljuk, hogy az attribútum **leírását** egy Description attribútummal kösse.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
    ```

- **ExpandArea** : ez az attribútum azt jelzi, hogy a vezérlő a teljes képernyőre nyúlik-e. Ez egy nem kötelező, logikai típusú attribútum. Az alapértelmezett érték false (hamis).

    >[!NOTE]
    >A felirat és a Leírás attribútumok le vannak tiltva, ha az attribútum értéke TRUE (igaz). A UocLabel vezérlőelem használatával megadhatja a kibontott vezérlő feliratát.
    >

- **Tipp** : ez egy opcionális, karakterlánc típusú attribútum. A hint attribútum szövege segít a végfelhasználónak eldönteni, hogy mi a vezérlőelem érvényes bemenete. A tipp megjelenik a vezérlő alatt.

- **AutoPostback** : ez egy nem kötelező, logikai típusú attribútum. Az alapértelmezett érték a hamis. Ha hamis értékre van állítva, akkor előfordulhat, hogy az oldal frissítése nem frissíti a vezérlőt. A AutoPostback-vel kapcsolatos információkért keresse meg a Microsoft ASP.NET UI Control tulajdonságát ugyanazon a néven.

- **RightsLevel** : ez egy opcionális, karakterlánc típusú attribútum. Ezt az attribútumot csak egy adatforrással rendelkező beágyazott jogokkal lehet kötni. A vezérlő dinamikusan be van kapcsolva vagy le van tiltva a felhasználó jogai alapján. További információ az adatforrások attribútummal vagy tulajdonság értékkel való kötéséről: a jelen dokumentum tulajdonságok szakasza.

    Ez a példa azt szemlélteti, hogyan használható egy **RightsLevel** attribútum egy adatforrással. Ha a dokumentum korábbi részében bemutatott adatforrás sablonját használta, az adatforrás **jogosultságokat** tartalmaz. Használja az attribútum nevét elérési útnak.
    <!--- no example provided -->

### <a name="property-element"></a>Tulajdonság eleme
Az egyes vezérlők viselkedésének további testreszabásához használhatja a **Property** elemet. A tulajdonság egy üres elem. A következő XSD-séma a tulajdonság elemhez tartozik:

```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```

Minden tulajdonsághoz a következő két kötelező attribútum tartozik:

- **Név** : Ez a karakterlánc típusú attribútum a tulajdonság egyedi neve. A különböző vezérlők eltérő tulajdonságokkal rendelkeznek. Vannak olyan általános tulajdonságok, amelyeket az összes vezérlő használhat. Ha többet szeretne megtudni arról, hogy mely nevek érhetők el egy adott vezérlőelem esetében, tekintse meg a jelen dokumentum általános tulajdonságok és egyéni vezérlők szakaszát.

- **Érték** : Ez a tulajdonság értéke. Az érték adattípusa attól függ, hogy melyik tulajdonsághoz van hozzárendelve. Tekintse meg a következő szakaszt az adott tulajdonságok megengedett formátuma beállításnál.


#### <a name="bind-property-with-data-source-content"></a>A kötés tulajdonsága az adatforrás tartalmával
Bizonyos tulajdonságok egy adatforrásból származó információkkal köthetők. Ezt a kötést a következő karakterlánc-formátummal végezheti el. Tekintse meg a jelen dokumentum egyes vezérlők szakaszában található egyéni tulajdonságok leírását, amelyből megtudhatja, hogyan köthető a tulajdonságok egy adatforráshoz.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

   Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}

   SourceExpression:= “Source=” + [ObjectDataSourceName]

   PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

   ModeExpression:= “Mode=” + [ModeChoice]

   ModeChoice:= “OneWay”|”TwoWay”

   ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

   AttributeName:= valid schema attribute name from the data source.

   AttributePropertyName:= valid property name of a schema attribute from the data source.
```

Az alábbi XML-kód azt mutatja be, hogyan köthető egy adatforrás egy **tulajdonság** eleméhez:

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
```


<h2 id="common-properties">Közös tulajdonságok</h2>

Az ebben a dokumentumban megadott összes RCDC-vezérlő rendelkezhet az ebben a szakaszban ismertetett általános tulajdonságokkal. Ezeket a tulajdonságokat a különböző vezérlőkre jellemző egyéb tulajdonságokkal is használhatja.

- **Kötelező** : Ez a tulajdonság azt jelzi, hogy a mező vagy kötelező mező vagy választható mező. Egy kötelező mezőt ki kell tölteni egy értékkel. A karakterlánc-bevitel nem támogatja az üres értéket. Egy nem kötelező mező üres maradhat. Ha ez a mező egy kötelezően kitöltendő értékkel rendelkező mező, a bemeneti vezérlő tetején egy hibaüzenet jelenik meg. Explicit módon megadhatja, hogy egy mező kötelező vagy nem kötelező. A mezőt egy attribútum és egy erőforrástípus közötti adott kötés sémájának adataival is összekapcsolhatja. Alapértelmezés szerint, ha ez a tulajdonság hiányzik, az azt jelenti, hogy a vezérlő egy opcionális beviteli vezérlő.

    A következő példa egy explicit értéket használ ehhez a tulajdonsághoz:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```

    Ez egy példa, amely dinamikus adatforrást használ ehhez a tulajdonsághoz. Ha a dokumentum előző szakaszában bemutatott adatforrás sablonját használta, az adatforrás séma. `<attribute name>.Required`Elérési útként való használata.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```

- **Readonly** : Ha ezt a tulajdonságot igaz értékre állítja, a végfelhasználó csak olvasható módban tudja megtapasztalni a vezérlőt. Ez egy nem kötelező, logikai típusú attribútum. Az alapértelmezett érték false (hamis). Előfordulhat azonban, hogy a tulajdonság viselkedését felülírja a felhasználó által a vezérlővel kötött adatkötések típusa. Ha például egy felhasználó nem rendelkezik a mező frissítéséhez szükséges jogosultságokkal, és a mező beágyazott jogokkal van kötve, a felhasználó csak olvasható módban látja el az adattípust, még ha ez a tulajdonság hamis értékre van állítva.

- **Válaszban** : Ez a tulajdonság határozza meg a vezérlő értékére vonatkozó korlátozásokat. A tulajdonságérték formátuma a .NET StringRegex standard által támogatott formátumok. További információ: .net- [keretrendszer reguláris kifejezések](http://go.microsoft.com/fwlink/?LinkId=165361). Ha a vezérlő egy érték beírására szolgál, akkor a rendszer a tulajdonságban megadott korlátozás szerint ellenőrzi az értéket, amikor a felhasználó az aktuális oldal elhagyását kísérli meg. A hibaüzenet a vezérlő azon felül jelenik meg, amely érvénytelen bemenettel rendelkezik. A felhasználó explicit módon megadhat egy sztring reguláris kifejezést. A felhasználó az adott attribútum sémájának adataival is köthető. Alapértelmezés szerint, ha ez a tulajdonság hiányzik, az azt jelenti, hogy a vezérlő nem ellenőrzi a bemeneti karakterláncok korlátozásait.

    A következő példa egy explicit értéket használ ehhez a tulajdonsághoz:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```

    Ez egy példa, amely dinamikus adatforrást használ ehhez a tulajdonsághoz. Ha a dokumentumban korábban bemutatott adatforráshoz használt sablont használta, az adatforrás séma. Használja a `<attribute name>.StringRegex` as elérési utat.

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```

- **Látható** : ez egy nem kötelező, logikai típusú attribútum. Ez az attribútum a teljes vezérlő elrejtésére használható. Az alapértelmezett érték TRUE (igaz).


<h3 id="options-element">Beállítások elem</h3>

A beállítások elem egy vagy több **lehetőség** alcsomópontot tartalmaz. A **Options** elemet csak a **UocRadioButtonList** és a **UocDropDownList** vezérlők használják. A vezérlőelemek használatáról további részleteket a jelen dokumentum egyes vezérlők szakasza tartalmaz.

A következő XSD-séma a Options elemhez tartozik:

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```

A **Options** elem a következő tulajdonságokkal rendelkezik:

- **Érték** : Ez a karakterlánc típusú kötelező attribútum. Az Value attribútumnak egyedinek kell lennie az adott vezérlőn belül. Csak A – Z, kis-és nagybetűket nem megkülönböztető karakterek használhatók.

- **Caption** : Ez a kötelező attribútum az egyes lehetőségek megjelenítendő neve.

- **Tipp** : ez egy opcionális attribútum. Ezzel az attribútummal további információkat és tippeket adhat a végfelhasználónak.


## <a name="environment-variables"></a>Környezeti változók

A következő környezeti változók bármelyik RCDC-konfigurációban használhatók:

| Változó | Leírás |
|---|---|
| `<LoginID>`       | Megjeleníti az aktuálisan bejelentkezett felhasználó AZONOSÍTÓját.           |
| `<LoginDomain>`   | Megjeleníti az aktuálisan bejelentkezett felhasználó tartományát.       |
| `<Today>  `       | Az aktuális dátum és idő megjelenítése                                |
| `<FromToday_nnn>` | Az aktuális dátumot, valamint `nnn` az időpontot jeleníti meg, ahol `nnn` az egész szám.  |
| `<ObjectID> `     | A RCDC elsődleges erőforrás-azonosítója.                                     |
| `<Attribute_xxx>` | A RCDC elsődleges erőforrás megadott attribútumát adja vissza (XXX). |


## <a name="debug-xml-configuration-files"></a>XML-konfigurációs fájlok hibakeresése

Ha egy RCDC XML-konfigurációs fájljait fejleszt vagy módosít, a hibák csökkentése érdekében az XML-fájlnak az XSD-fájlokkal való érvényesítését egy szerkesztővel, például a Microsoft Visual Studio használatával végezheti el. További információ: [Bevezetés a Visual Studio 2005-es XML-eszközeibe](http://go.microsoft.com/fwlink/?LinkID=74512).


## <a name="customize-help-files"></a>Súgófájlok testreszabása

Ha új erőforrásokat és attribútumokat hoz létre, érdemes lehet frissíteni a meglévő súgófájlokat a FIM Portalon a testreszabott erőforrások tartalmával. A FIM-portálon lévő súgófájlok. htm formátumúak, és manuálisan is szerkeszthetők. Az egyéni attribútumok létrehozásával kapcsolatos további információkért lásd: Bevezetés az egyéni erőforrás-és attribútumok kezeléséhez a FIM 2010 dokumentációjában.

>[!IMPORTANT]
>A HTML formázási és szerkesztési alapjaival kapcsolatos információkat ebben a cikkben nem találja. A felhasználóknak ismerniük kell a HTML-fájlok szerkesztésének módját.

### <a name="location-of-help-files"></a>Súgófájlok helye
A Microsoft Identity Management 2016 SP1 portál összes súgófájl a Rendszerfelügyeleti `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html` webszolgáltatások kiszolgálójának mappájában található.

### <a name="locate-a-specific-help-file"></a>Egy adott súgófájl megkeresése
A FIM-portál összes súgófájl egy globálisan egyedi azonosítóval (GUID) van elnevezve. Az egyéni erőforrás helyes fájljának megkeresése:

1. A FIM-portálon nyissa meg a használni kívánt portálon a súgófájl lapot.

2. Kattintson a jobb gombbal a súgófájl, és válassza a **Tulajdonságok** lehetőséget.

3. Jelölje ki és másolja a `<GUID\>.htm` fájlt az **URL-cím** mezőbe.

4. Keresse meg azt a mappát, ahol a súgófájlok tárolódnak, és keresse meg a fájlt.


## <a name="add-content-for-attribute-in-existing-grouping-element"></a>Tartalom hozzáadása az attribútumhoz a meglévő csoportosítási elemben
Leíró tartalom hozzáadása egy meglévő csoportosítási elemen belüli új attribútumhoz (TAB):

1. Azonosítsa és keresse meg a megfelelő súgófájlokat.

2. HTML-szerkesztő használatával nyissa meg a fájlt.

3. Keresse meg, hová szeretné hozzáadni a tartalmat. Ez általában egy további bekezdés, például:

    `<p xmlns="">A new paragraph with customized information.</p>`

    Egy meglévő listához beszúrt elem is lehet, például:

    ```
    <li class="unordered"><b>First Name</b> – The first name of the User.<br>
    <li class="unordered"><b>Last Name</b> - The last name of the User.<br>
    <li class="unordered"><b>Added a new line</b><br>
    ```

## <a name="add-content-for-existing-grouping-element"></a>Tartalom hozzáadása a meglévő csoportosítási elemhez
A FIM-portál lapjai többsége több csoportosítási elemet (vagy fület) tartalmaz, és a kapcsolódó súgófájlok könyvjelzővel ellátott szakaszokkal rendelkeznek, amelyek az egyes csoportosítási elemekhez kapcsolódnak. A HTML-könyvjelzők a szakaszban vannak megadva. Például ez a "felhasználói adatok" lap "a felhasználó létrehozása" lapjának a FIM Portalon:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

A rendszer a konfigurációs adatxml-fájlban lévő csoportosítási elem **WorkInfo** hivatkozik a **felhasználó-létrehozási RCDC konfigurálásához** . A `\<GUID\>.htm` fájl neve és a könyvjelző a (z) `my:Link` paraméterben van megadva:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

### <a name="simple-control-samples"></a>Egyszerű vezérlési minták
Ez a szakasz mintákat tartalmaz a különböző egyszerű szöveges vezérlők létrehozásához.

Az alábbi ábrán néhány egyszerű szöveges vezérlőelem látható különböző módokon:

![Egyszerű szövegmező vezérlők](media/rcd-configuration-xml-reference/image016.gif)

A következő kódrészlet hozza létre az első Text-Box vezérlőelemet, amely az összes attribútumhoz és tulajdonsághoz explicit szöveget használ:

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->
```

A következő kódrészlet hozza létre a második szövegmező vezérlőelemet, amely dinamikus kötési technikát használ a vezérlő másik adatforrással való összekapcsolásához:

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```

A következő kódrészlet létrehozza a harmadik kibontott címkét és a Text-Box vezérlőelemet:

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

A következő kódrészlet létrehozza a negyedik letiltott Text-Box vezérlőt. Bár ez a vezérlő nem jeleníti meg a letiltott állapot és az engedélyezett állapot közötti látható különbséget, a felhasználó már nem írhat be adatbevitelt a szövegmezőbe.

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```


## <a name="individual-controls"></a>Egyéni vezérlők

Ez a szakasz a Microsoft Identity Manager 2016 SP1 által biztosított egyes vezérlőket dokumentálja.

### <a name="uocbutton"></a>UocButton

**Név** : UocButton

**Leírás** : ez egy egyszerű gomb vezérlőelem, amely bizonyos műveletek elindítására használható. Mivel azonban nem adhatja meg a saját kezelőjét, a vezérlő használata korlátozott.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **Text** : Ez a tulajdonság határozza meg a gombon megjelenő szöveget. Ez egy opcionális, karakterlánc típusú attribútum. A szöveg egy explicit karakterlánc-értéket vesz fel.

**Események** :

- **OnButtonClicked** : az esemény a gombra kattintáskor lesz kibocsátva.

**Példa** :

![UocButton-vezérlő](media/rcd-configuration-xml-reference/image017.png)

A következő XML-szegmens egy egyszerű UocButton vezérlőt hoz létre:

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```


### <a name="uoccaptioncontrol"></a>UocCaptionControl

**Név** : UocCaptionControl

**Leírás** : Ez a vezérlő egy RCDC lap feliratának megjelenítésére szolgál. Ez a vezérlő csak egyetlen vezérlőelemként használható a fejlécek csoportosításában. Ha más kontextusban használja, a renderelési problémákat vagy a portál hibáját okozhatja.

**Mód** : csak olvasható (egyirányú szűrési)

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **MaxHeight:** Ez a tulajdonság határozza meg az ikon maximális magasságát a felirat szakaszban. Ez a tulajdonság nem kötelező. Ez a tulajdonság képpontban egész értéket vesz fel. Az alapértelmezett érték 32 képpont.

**Események** :

- Nincsenek események ehhez a vezérlőhöz.

**Példa** :

![UocCaptionControl-vezérlő](media/rcd-configuration-xml-reference/image020.jpg)

A következő kódrészlet létrehoz egy **fejléc feliratot** :

```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->
```

A következő kódrészlet egy **explicit tartalom-feliratot** hoz létre:

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

A következő kódrészlet a **megjelenítendő név** dinamikus feliratát hozza létre:

```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```


### <a name="uoccheckbox"></a>UocCheckBox

**Név** : UocCheckBox

**Leírás** : ez egy egyszerű jelölőnégyzetes vezérlő. Azt javasoljuk, hogy a felhasználó a vezérlőt logikai típusú adattípussal kösse. Ez a vezérlő csak olvasható vezérlőelemként vagy frissíthető vezérlőelemként használható a-hez kötődő információk alapján.

>[!NOTE]
>Ebben a kiadásban, ha a jelölőnégyzet vezérlőelemet használja a szerkesztési módban egy logikai attribútum megjelenítéséhez, ha az attribútumhoz korábban nincs hozzárendelve érték, akkor az erőforrás-vezérlő **hamis** értéket ad hozzá az attribútumhoz, amikor az **OK gombra** kattint a szerkesztési módban. A következő lépésekkel mindig létrehozhat egy logikai attribútumot, amely azt feltételezi, hogy a nem létező érték megegyezik a **Hamis értékkel** , vagy más vezérlők, például egy választógomb használata a logikai attribútumok esetében.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **DefaultValue** : ez egy nem kötelező, logikai típusú tulajdonság. Az alapértelmezett érték false (hamis). Ez a mező a jelölőnégyzet alapértelmezett viselkedését határozza meg. Ezt explicit módon megadhatja.

- **Bejelölve** : ez egy nem kötelező, logikai típusú tulajdonság. Az alapértelmezett érték false (hamis). Ez az érték felülírja a DefaultValue tulajdonságot, ha az a DefaultValue értékkel együtt szerepel. Ez a mező a jelölőnégyzet viselkedését határozza meg. A DefaultValue-hez hasonlóan ez explicit módon vagy a kiszolgálóról származó adatokkal is megadható.

- **Text** : ez egy opcionális, karakterlánc típusú attribútum. A szöveg a jelölőnégyzet jobb oldalán jelenik meg. Ezzel a tulajdonsággal adhat meg olyan szöveget, amely további információkat biztosít a végfelhasználó számára.

**Események** :

- **CheckedChanged** : Ha a jelölőnégyzet állapota megváltozik, az eseményt a rendszer kibocsátja.

**Példa** :

A következő példában egyéni kötés jön létre az egyéni erőforrástípus és a **IsConfigurationType** attribútum között. Az XML az egyéni erőforrástípus RCDC van használatban.

![UocCheckBox-vezérlő](media/rcd-configuration-xml-reference/image022.png)

A következő kódrészlet **dinamikus jelölőnégyzetet** hoz létre, ahogy az előző ábrán a dinamikus jelölőnégyzet is látható. Ez a típusú kötés sokoldalúbb és hasznos, mint egy explicit jelölőnégyzet. Az attribútumnak az aktuális erőforrás-típushoz kell tartoznia.

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**Név** : UocCommonMultiValueControl

**Leírás** : ez egy többsoros szövegbeviteli vezérlőelem, amely támogatja a speciális karakterlánc-formázást. A többértékű bejegyzések között minden érték pontosvesszővel van elválasztva egymástól (;) vagy egy sortörés a szövegmezőben. Javasoljuk, hogy a vezérlőt a többértékű, a rövid karakterlánc és az egész szám típusú adatmennyiséggel társítsa. Ez a vezérlő a írásvédett és a frissíthető módot is támogatja.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **Adattípus** : ez egy kötelező, karakterlánc típusú attribútum. Ezt **karakterláncként, egész számként** vagy **datetime** típusúként is megadhatja. Az attribútum a Schema attribútum **adattípus** tulajdonságával is köthető. A többértékű hivatkozási típust a **UOCListView** vagy a **UOCIdentityPicker** kezelheti. A többértékű logikai érték nem támogatott adattípusú.

- **Sorok** : ez egy opcionális, integer típusú attribútum. Megadhatja a mező magasságát a karakterek számában. Az alapértelmezett érték értéke 1.

- **Oszlopok** : ez egy opcionális, egész típusú attribútum. Megadhatja, hogy a mező hány széles legyen a karakterek számában. Az alapértelmezett érték 20.

- **Érték** : ez egy opcionális, karakterlánc típusú attribútum. Ezt az attribútumot csak adatforrással lehet kötni.

**Események** :

- **ValueListChanged** : ez az esemény akkor aktiválódik, ha a vezérlő aktuális értéke megváltozik.

**Példa**

A következő példában egy **AMultiValueString** nevű Többértékű karakterlánc-attribútum jön létre, és az egyéni erőforrástípus van kötve. Ez a példa csak a kötés létrehozása után működik.

![UocCommonMultiValueControl-vezérlő](media/rcd-configuration-xml-reference/image024.jpg)

A következő kódrészlet egy **UocCommonMultiValueControl** vezérlőt hoz létre:

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```


### <a name="uocdatetimecontrol"></a>UocDateTimeControl

**Név** : UocDateTimeControl

**Leírás** : Ez hasonló a szövegmező vezérlőelemhez, de a **Leírás** csak egy bizonyos formátumot fogad el. A csak olvasható módban úgy tűnik, mint egy címkét. A támogatott bemeneti karakterlánc formátuma a jelen szakasz **DateTimeFormat** tulajdonságában található.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **DateTimeFormat** : ez egy opcionális, karakterlánc típusú attribútum. A támogatott formátumok a **datetime** és a **DateOnly** . Az alapértelmezett érték a **datetime** formátumra van beállítva.

  - **Datetime** : az attribútum formátuma HH/NN/ÉÉÉÉ óó: PP: SS am.
  - **DateOnly** : az attribútum hh/nn/éééé formátumban van formázva.

    >[!NOTE]
    >A **datetime** és a **DateOnly** formátum is támogatott, függetlenül attól, hogy melyik felhasználó adta meg a különbséget.
    >

- **Érték** : ez egy opcionális, karakterlánc típusú attribútum. Ezt az attribútumot erőforrás-adatforráshoz kell kötni. Ennek az attribútumnak az értékének a megfelelő datetime formátumnak kell megfelelnie.

**Események** :

- **DateTimeChanged** : a DateTime érték megváltozásakor az esemény következik be.

**Példa** :

![UocDateTimeControl-vezérlő](media/rcd-configuration-xml-reference/image027.jpg)

A következő kódrészlet az első **datetime** vezérlőelemet állítja elő.

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```

A következő kódrészlet a második **datetime** vezérlőt állítja elő. Ha használta a mintakód használatát az adatforrások szakaszban, a **expirationtime tulajdonságok** attribútum az összes erőforrástípus kötve van. Ezért használhatja a következő kóddal:

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```


### <a name="uocdropdownlist"></a>UocDropDownList

**Név** : UocDropDownList

**Leírás** : ez egy egyszerű legördülő lista vezérlőelem. Ezzel a vezérlővel választhatók ki a lehetőségek egy meghatározott készletből. A vezérlőhöz tartozó string, Integer, datetime és Boolean adattípusok megfelelő jelöltek.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **ValuePath** : az Value attribútum ItemSource-ből való beolvasására szolgáló tulajdonság. Ha a ItemSource egyéniként van megadva, az érték elérési útja értékre van állítva. A beállítás elemében lévő Value (érték) mezővel kötődik, az ebben a szakaszban leírtak szerint.

- **CaptionPath** : az Value attribútum ItemSource-ből való beolvasására szolgáló tulajdonság. Ha a ItemSource egyéniként van megadva, az érték elérési útja felirat. A beállítás elem Caption mezőjéhez kötődik, az ebben a szakaszban leírtak szerint.

- **HintPath** : az Value attribútum ItemSource-ből való beolvasására szolgáló tulajdonság. Ha a ItemSource egyéniként van megadva, az érték elérési útja a tipp. A beállítás elemében a tipp mezőhöz kötődik, az ebben a szakaszban leírtak szerint.

- **ItemSource** : a listában szereplő választási lehetőségeket definiáló ListControlItems gyűjteménye. A felhasználó explicit módon beállíthatja ezt az egyéni értékre, és használhatja a kapcsoló elemet a jelen szakaszban leírtak szerint a karakterlánc értékének megadásához.

- **SelectedValue** : a jelenleg kijelölt érték. Ez egy kötelező, karakterlánc típusú tulajdonság. Ezt a tulajdonságot az adatforrásból származó karakterlánc-adatok kötik.

**Események** :

- **SelectedIndexChanged** : az esemény akkor következik be, amikor a legördülő mezőben a kijelölés változik.

**Beállítások** :

Egy **Options** elem struktúrájához lásd: <a href="#options-element">Options elem</a>.

- **Érték** : az egyetlen beállítási elem értéke bármely olyan sztring lehet, amely annak az adatforrásnak a érvényes bemenetét adja meg, amelyhez a vezérlő kötődik.

- **Caption** : a felirat lehet bármilyen karakterlánc-érték.

- **Tipp** : a mutató bármilyen sztring lehet.

**Példa** :

![UocDropDownList-vezérlő](media/rcd-configuration-xml-reference/image030.jpg)


![UocDropDownList vezérlőelem beállításai](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
>A minta működéséhez egy meglévő, karakterlánc típusú attribútum- **hatókört** kell kötnie a RCDC által érintett egyéni erőforrás-típussal.


A következő kódrészlet létrehoz egy legördülő listát:

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

**Név** : UocFileDownload

**Leírás** : Ez a vezérlő hiperhivatkozást tartalmaz. Ha a hiperhivatkozásra kattint, megjelenik a Windows Save file (fájl mentése) oldal. A felhasználó mentheti a fájlt a helyi meghajtóra. Az Open (Megnyitás) lehetőség akkor is támogatott, ha az Internet Explorer megjeleníti a fájlformátumot. A vezérlő használatára javasolt adattípusok a formázott karakterláncok (XML) és a bináris típusok.

>[!NOTE]
>Microsoft Identity Manager 2016 SP1 ezen kiadásában a felhasználónak be kell jelölnie az Internet Explorer ablakát, amelyben megnyitotta a fájlt, majd frissítenie kell a lapot. Az Internet Explorer ablakának frissítése után a felhasználó elindíthatja a letöltést, hogy mentse vagy nyissa meg újra ugyanazt a fájlt az eredeti ablakban.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **Text (szöveg** ): ez egy opcionális, karakterlánc típusú attribútum, amely meghatározza a hiperhivatkozás szövegét. A felhasználó meghatározhat egy explicit karakterláncot ehhez a tulajdonsághoz.

- **Érték** : ez egy kötelező attribútum. Meghatározza az attribútum kötését azon a kiszolgálón, amelynek a tartalmát le szeretné tölteni.

- **PromptedFileName** : ez egy opcionális, karakterlánc típusú attribútum. Ez a fájl neve, amelyet a rendszer a letöltött fájl mentésekor javasol a felhasználó számára.

- **ContentType** : ez egy kötelező, karakterlánc típusú attribútum. Ez az a fájltípus, amelyet az adatment. A szöveg vagy a bináris a két támogatott karakterlánc-beállítás. Ha ez a szöveg, a visszatérési érték hosszú sztringnek tekintendő. Ellenkező esetben a bináris esetében a visszatérési érték bájt []. Ha a szöveg be van jelölve, a felhasználó lehetőségként hozzáadhat egy utótagot a szöveg típusának megadásához. Például a Text/XML érvénytelen.

>[!NOTE]
>Ha az ehhez a vezérlőhöz kötött érték üres, a vezérlőből hiányzik a letöltési művelet elindítására szolgáló hivatkozás. Ennek az az oka, hogy semmi nem tölthető le.

**Események** :

- Nincsenek események ehhez a vezérlőhöz.

**Példa** :

![UocFileDownload-vezérlő](media/rcd-configuration-xml-reference/image035.png)

>[!NOTE]
>A forrásfájl feltöltése előtt a felhasználónak létre kell hoznia egy kötést az egyéni erőforrástípus és a meglévő ConfigurationData attribútum között.

A következő kódrészlet egy fájl letöltésének vezérlőjét hozza létre:

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**Név** : UocFileUpload

**Leírás** : Ez a vezérlő egy szövegmezőt tartalmaz, amely megjeleníti a feltölteni kívánt helyi fájl helyét, a tallózási fájl gombot és a feltöltés gombot. Amikor a végfelhasználó rákattint egy Tallózás gombra, megjelenik egy Windows Open File (fájl megnyitása) ablak. A végfelhasználó kijelölhet egy fájlt a helyi meghajtón a feltöltéshez. Ha a fájl ki van választva, a fájl helye megjelenik a szövegmezőben. Ha a feltöltés gombra kattint, a rendszer feltölti a fájlt az ügyféloldali helyi adatforrásba. A fájl tartalma még nincs elküldve a kiszolgálónak. A vezérlő használatára javasolt adattípusok a következők: formázott karakterlánc (XML) vagy bináris típusok.

>[!NOTE]
>A feltöltési folyamatnak vagy az állapotnak nincs jele. Ha a fájl fel van töltve a helyi adatforrásba, a szövegmező törlődik.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **Érték** : ez egy kötelező attribútum. Meghatározza a séma attribútum kötését azon a kiszolgálón, amelyhez az adatfeltöltés történik.

- **ContentType** : ez egy opcionális, karakterlánc típusú attribútum. Ez az adattípus, amelyet a fájl a kiszolgálón ment. Ezt beállíthatja szövegre vagy binárisra. Ha a tulajdonság hiányzik, az alapértelmezett érték a bináris.

- **MaxFileSize** : ez egy opcionális, karakterlánc típusú attribútum. A MaxFileSize határozza meg, hogy mekkora méretű a feltöltött fájl mérete. Alapértelmezés szerint, ha a tulajdonság hiányzik, a maximális méret 1 megabájt (MB).

- **PromptedForNoValue** : ez egy opcionális, karakterlánc típusú attribútum. Meghatározza azt a szöveget, amely megjelenik a felhasználó számára, ha egy fájl feltöltése nem történik meg.

**Események** :

- **FileUploaded** : ez az esemény a fájl sikeres feltöltésekor lesz kibocsátva.

**Példa** :

![UocFileUpload-vezérlő](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
>A következő mintakód működéséhez létre kell hoznia egy új, ABinaryAttribute nevű bináris típusú attribútumot, majd létre kell hoznia egy új kötést az egyéni erőforrástípus és az attribútum között.

A következő kódrészlet létrehoz egy feltöltési vezérlőt:
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```


### <a name="uocfilterbuilder"></a>UocFilterBuilder

**Név** : UocFilterBuilder

**Leírás** : ez egy összetett vezérlő, amely lehetővé teszi a felhasználó számára, hogy megjelenítse a 2016-es XPath-kifejezést. Egyes XPath-kifejezések nem támogatottak. A Filter Builder használatával kapcsolatos további információkért tekintse meg a Filter Builder súgóját.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **PermittedObjectTypes** : Ez a szűrő-szerkesztő SELECT utasításában megjelenítendő erőforrástípusok listáját határozza meg. A Filter Builder használatával kapcsolatos további információkért tekintse meg a Filter Builder súgóját. A karakterlánc a ResourceTypeA (ResourceTypeB) formátumban van, ahol minden erőforrástípus vesszővel elválasztva (",").

- **Érték** : ez az az érték, amellyel a Filter Builder jelenik meg. Csak a karakterlánc típusú, XPath-kifejezést tartalmazó kötések támogatottak. A Filter attribútum a vezérlő kötéséhez ajánlott attribútum.

- **PreviewButtonVisible** : ez egy nem kötelező, logikai típusú tulajdonság. Ha ez a tulajdonság hamis értékre van állítva, a felhasználó nem lát egy előnézeti gombot. Az alapértelmezett érték TRUE (igaz). Ez a gomb a lista nézet vezérlőelemmel együtt használható egy XPath-kifejezés eredményeinek megtekintéséhez.

- **ExcludeGroupMembership** : ez egy logikai tulajdonság. Ha ez a tulajdonság TRUE (igaz) értékre van állítva, nem hozhat létre olyan szűrőt, amely \< hivatkozási attribútumot használ \> (például ResourceId) a \< Group objektum tagja \> . Más szóval, ha ez a tulajdonság TRUE (igaz) értékre van beállítva, nem hozhat létre olyan szűrőt, amely a csoporttagság könyvtárat használja.

- **PreviewButtonCaption** : ez egy opcionális karakterlánc. Ha a PreviewButtonVisible értéke TRUE (igaz), akkor ezzel a tulajdonsággal testreszabott szöveget adhat a gombnak. A szöveg megjelenik az előnézet gombon.

**Események** :

- **OnFilterChanged** : ez az esemény akkor aktiválódik, amikor a Filter Builder tartalma megváltozik.

**Példa** :

![UocFilterBuilder-vezérlő](media/rcd-configuration-xml-reference/image044.png)

Az alábbi mintakód egy UOCLabel vezérlőt, egy egyszerű PermittedObjectTypes és egy előnézeti listanézet-nézetet tartalmaz. Mutasson a listanézet ListFilter tulajdonságra, és a Filter Builder Value tulajdonságot ugyanarra az adatforrás-attribútumra, hogy összekapcsolja a kettőt.

>[!NOTE]
>A mintakód használata előtt hozzon létre egy új kötést egy meglévő Filter attribútum és egy egyéni erőforrástípus között.

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


### <a name="uochtmlsummary"></a>UocHtmlSummary

**Név** : UocHtmlSummary

**Leírás** : ezzel a vezérlőelemmel definiálhat egy összegző lapot egy RCDC-oldalon. Ez az összefoglaló lap akkor jelenik meg, ha a végfelhasználó kérelmet küld. Ez a vezérlő csak összegző csoportosításban használható, és csak az egyetlen vezérlő lehet. Erősen ajánlott a megadott mintakód használata.

>[!NOTE]
>A vezérlőt nem tesztelték széles körben.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **ModificationsXml** : ezt a tulajdonságot {Binding Source = Delta, Path = DeltaXml} formátumban kell megadni, ahol a különbözet a ObjectDataSource konfigurációs fejlécében van definiálva.

- **TransformXsl** : Ez a tulajdonság {Binding Source = SummaryTransformXsl, Path =/} formátumban van formázva, ahol a SummaryTransformXsl a XmlDataSource konfigurációs fejlécében van definiálva.

**Események** :

- Nincsenek események ehhez a vezérlőhöz.

**Példa** :

A vezérlő mintájának megtekintéséhez tekintse meg a jelen dokumentum csoportosítási elemének az összefoglalás csoportosítására vonatkozó példáját.


### <a name="uochyperlink"></a>UocHyperLink

**Név** : UocHyperLink

**Leírás** : ez egy egyszerű hiperhivatkozás-vezérlő. A vezérlő használatával hiperhivatkozásként jeleníthet meg információkat.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **ObjectReference** : ez egy opcionális, hivatkozási típusú tulajdonság. Ha az ebben a tulajdonságban definiált GUID-azonosító érvényes erőforrásra hivatkozik, a hiperhivatkozás lehetővé teszi a végfelhasználó számára az erőforrás elérését. Ez kölcsönösen kizárható a NavigateUrl tulajdonsággal.

- **Text** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ennek a tulajdonságnak a használatával megadhatja a hiperhivatkozásként megjelenő szöveget.

- **NavigateUrl** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ezzel a tulajdonsággal határozhatja meg a hiperhivatkozásra mutató teljes elérési út URL-címét. Ez kölcsönösen kizárható a ObjectReference tulajdonsággal.

**Események** :

- Nincsenek események ehhez a vezérlőhöz.

**Példa** :

![UocHyperLink-vezérlő](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
>A következőhöz való csatolásához érvényes GUID-azonosítóra van szükség az erőforráshoz. Ebben az esetben a második hiperhivatkozás egy érvényes GUID azonosítóval jön létre. Az első az egyik webhely lehet.

A következő kódrészlet létrehoz egy átirányítási hiperhivatkozást:

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->
```

A következő kódrészlet létrehoz egy hivatkozást, amely egy erőforrásra hivatkozik. Az explicit hivatkozás helyébe a {kötési forrás = objektum, a Path = Creator} kifejezés használható, hogy ezt egy adatforrással lehessen kötni. Ez csak akkor érvényes, ha az erőforrás felettese létezik, és ez egy hivatkozási típusú érték.

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```


### <a name="uocidentitypicker"></a>UocIdentityPicker

**Név** : UocIdentityPicker

**Leírás** : Ez a vezérlő egy választható feloldási mezőből és egy tallózási ablakból áll. A választható feloldási mező egy opcionális szövegmezőből áll, ahol megadhatja az identitást, a feloldás gombot az identitás feloldásához, valamint egy Tallózás gombot egy előugró ablak megadásához. A Tallózás ablak lehetővé teszi, hogy a felhasználó kiválassza az identitásokat egy lista-nézet vezérlőelem segítségével. A Tallózás ablak kiválasztott identitása megjelenik a feloldási mezőben.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **UsageKeywords** : ez egy opcionális karakterlánc-tulajdonság. Megadhatja az erőforrás-választóban használandó keresési hatókörök listáját a SearchScopeConfiguration struktúra által támogatott használati kulcsszavak listájának megadásával, ahol minden kulcsszót aposztróf (') választ el egymástól.

- **Filter** : ez egy opcionális karakterlánc-tulajdonság. A felhasználó egy XPath-kifejezést biztosít az erőforrás-választó hatóköréhez, hogy csak azokat az elemeket jelenítse meg, amelyek illeszkednek a megadott hatókörbe. Ez a tulajdonság kölcsönösen kizárható a UsageKeywords tulajdonsággal. A keresési hatókör alkalmazása esetén ennek a tulajdonságnak nincs hatása.

- **ResultObjectType** : ez egy opcionális karakterlánc-tulajdonság. Az erőforrástípus a felugró párbeszédpanelek listájában lévő erőforrások megjelenítésére szolgál. Ezt a szűrőt használva az Identity Picker azonosítja, hogy a szűrő milyen típusú erőforrást ad vissza, és ennek megfelelően jeleníti meg az adattípust. Ez a tulajdonság kölcsönösen kizárható a UsageKeywords tulajdonsággal. A keresési hatókör alkalmazása esetén ennek nincs hatása. Az ehhez a tulajdonsághoz elfogadott karakterlánc bármely egyedi, érvényes, erőforrás típusú név, például személy. Ha a szűrőnek több erőforrástípust kell visszaadnia, az erőforrást használja a rendszer.

- **PreviewTitle** : ez az előnézeti cím, amelyet a listanézet használ. A tulajdonsággal kapcsolatos információkért tekintse meg a UocListView szakaszt.

- **ListViewTitle** : ez egy opcionális karakterlánc-tulajdonság. Ennek a tulajdonságnak a segítségével megadhatja a listanézet tetején látható szöveget címként.

- **Érték** : ez egy opcionális karakterlánc-tulajdonság. Azt javasoljuk, hogy ezt egy Schema attribútummal társítsa az érték egy adatforrással való összekapcsolásához.

- **Mode (mód** ): ez egy opcionális karakterlánc-tulajdonság. Ezzel a tulajdonsággal határozható meg, hogy az Identity Picker vagy több identitás közül választhat-e ki egy értéket. A SingleResult és a MultipleResult az engedélyezett értékek. Alapértelmezés szerint a SingleResult értékre van állítva.

- **ObjectTypes** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Megadhatja azon erőforrástípusok listáját, amelyekkel a végfelhasználó feloldja a bejegyzéseket az Identity Picker feloldási mezőben. A lista az erőforrás típusú nevek listáját tartalmazza, vesszővel elválasztva: ",".

- **AttributesToSearch** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Megadhatja azon attribútumok listáját, amelyeket a rendszer az Identity választóban lévő elem feloldásához használ, ahol a lista a séma attribútumainak listája, vesszővel elválasztva: ",". Ha például a AttributesToSearch értékre van állítva `DisplayName, Alias` , a felhasználó a vagy a elemmel kereshet `DisplayName = \<search value\>` `Alias=\<search value\>` . Az itt megadott attribútumok nevének érvényes attribútumnak kell lennie az érték tulajdonságban megadott adatforrás cél típusú erőforrásaiban. A cél erőforrástípus a ObjectTypes mezőben található. Az összes attribútumnak érvényesnek kell lennie a ObjectTypes mezőben hivatkozott egyes erőforrástípusok esetében.

- **ColumnsToDisplay** : ez egy nem kötelező, karakterlánc típusú tulajdonság. A felhasználó megjeleníti a séma-attribútumok nevét, vesszővel elválasztva a következőt:. Az itt definiált attribútumok teszik elérhetővé az Identity Picker listanézet oszlopát.

- **Sorok** : ez egy opcionális, integer tulajdonság. Csak akkor működik, ha a mód MultipleResult értékre van állítva. Ezzel a tulajdonsággal állíthatja be a feloldási szövegmező magasságát a karakteres egységekben megadott méretre.

- **MainSearchScreenText** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ez a testreszabott szöveg, amely akkor jelenik meg, amikor a keresés fut a Tallózás ablakban.

**Események** :

- **SelectedObjectChanged** : ezt az eseményt akkor bocsátja ki a rendszer, amikor a felhasználó megváltoztatja a kiválasztott erőforrásokat.

**Példa** :

![UocIdentityPicker-vezérlés SingleResult módban](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
>Ahhoz, hogy ez a minta működjön, létre kell hoznia egy új kötést a Manager-attribútum és bármely olyan egyéni erőforrástípus között, amelyre ez az XML vonatkozik.

A következő kódrészlet SingleResult módban hoz létre egy identitás-választót a szűrő-és ResultObjectType tulajdonságok használatával a RCDC részeként:

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```

Az alábbi ábrán egy MultipleResult módban található Identity Picker látható:

![UocIdentityPicker-vezérlés MultipleResult módban](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
>A mintakód működéséhez kötnie kell a ExplicitMember attribútumot (egy többértékű hivatkozási attribútumot) az egyéni erőforrás típushoz. Keresési hatóköröket hozhat létre a UsageKeyword tulajdonsággal, amely személyre és csoportra van beállítva.

A következő kódrészlet MultipleResult módban hoz létre egy identitás-választót:

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

**Név** : UocLabel

**Leírás** : ez egy egyszerű, csak olvasható, szöveg feliratú vezérlőelem. Javasoljuk, hogy ezt a vezérlőt csak olvasható információk megjelenítésére használja.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **Text (szöveg** ): ez egy karakterlánc típusú attribútum. Ezt a tulajdonságot explicit karakterlánc-érték megadásával vagy adatforrással való kötéssel határozhatja meg. A tulajdonság értékét hozzárendelő minta kötés {kötési forrás = objektum, elérési út = \< érvényes attribútum neve \> .

A UocLabel vezérlőhöz tartozó minta esetében lásd: egyszerű vezérlés az egyszerű vezérlő minták szakaszban.


### <a name="uoclistview"></a>UocListView

**Név** : UocListView

**Leírás** : ez egy speciális lista-vezérlőelem. Egyszerű listanézet, választható egyszerű keresés, választható speciális keresési vezérlő, választható kiválasztási előnézet és egy művelet gomb sávja. A választható egyszerű keresés egy keresési hatókörből és egy egyszerű keresési szövegmezőből áll. A speciális keresési vezérlő egy szűrő-szerkesztő. A listanézet megjeleníti az erőforrások előzetesen megjelenített listáját. A vezérlőben található keresési eredményektől érkező keresési eredményeket is megjelenítheti. A művelet gomb sáv határozza meg, hogy milyen műveleteket lehet végrehajtani a listanézet kiválasztása alapján. A kiválasztási előnézet mező megjeleníti, hogy mely elemek vannak kiválasztva a listanézet alapján.

>[!IMPORTANT]
>A UocListView egyértékű hivatkozási attribútumokkal nem működik. Csak többértékű hivatkozási attribútumokkal használható. Az egyértékű hivatkozási attribútumok esetében lásd: UocIdentityPicker ebben a dokumentumban.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **SelectedValue** : ez egy opcionális, karakterlánc típusú tulajdonság, amely egy többértékű hivatkozási attribútumhoz van kötve, elfogadva a GUID-formázott karakterláncok listáját.

- **PageSize** : ez egy nem kötelező egész tulajdonság. A felhasználó megadhatja, hogy hány bejegyzés fér el egy lapon a listanézet vezérlőelemben. Az alapértelmezett érték 10 bejegyzés. Minden pozitív egész szám érvényes.

- **UsageKeyword** : ez egy nem kötelező, karakterlánc típusú tulajdonság. A felhasználó megadhatja azoknak a kulcsszavaknak a listáját, amelyek meghatározzák, hogy milyen keresési hatókört használ a lista – keresés vezérlőelem. Keresési hatóköri erőforrások találhatók a FIM 2010-kiszolgálón. A UsageKeyword nevű SearchScopeConfiguration-struktúra attribútuma a keresési hatókörök csoportjának csoportosítására szolgál. A lista nézet a kulcsszavak listáját használja fel. Az egyes kulcsszavak vesszővel (,) vannak elválasztva. Ez a lista nézetben megjeleníteni kívánt keresési hatókörben használt használati kulcsszó. Ez csak akkor érvényes, ha a ShowSearchControl tulajdonság értéke TRUE (igaz).

- **SearchControlAutoPostback** : ez egy nem kötelező logikai tulajdonság. Állítsa a tulajdonság értékét True értékre a AutoPostBack végrehajtásához a keresés indításakor. Alapértelmezés szerint a SearchControlAutoPostback hamis értékre van állítva.

- **EmptyResultText** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Alapértelmezés szerint a beállítás értéke nem elem, de bármilyen karakterlánc értékre állítható. Ez a szöveg akkor jelenik meg, ha a keresési eredmény üres.

- **ButtonHeight** : ez egy opcionális, integer típusú tulajdonság. A tulajdonság értékének értékeként adja meg a pozitív egész értéket. Ez a tulajdonság határozza meg a műveleti sávban lévő gombok magasságát képpontban megadva. Az alapértelmezett érték 32 képpont.

- **ButtonWidth** : ez egy opcionális, integer típusú tulajdonság. A tulajdonság értékének értékeként adja meg a pozitív egész értéket. Ez a tulajdonság határozza meg a műveleti sávban lévő gombok szélességét képpontban megadva. Az alapértelmezett érték 32 képpont.

- **CaptionImageMaxHeight** : ez egy opcionális, integer típusú tulajdonság. Állítsa a tulajdonság értékét bármely pozitív egész számra. Ez a tulajdonság határozza meg az opcionális feliratok maximális ikonjának magasságát. Az alapértelmezett érték 32 képpont.

- **CaptionImageMaxWidth** : ez egy opcionális, integer típusú tulajdonság. Állítsa a tulajdonság értékét bármely pozitív egész számra. Ez a tulajdonság határozza meg az opcionális feliratok ikonjának maximális szélességét. Az alapértelmezett érték 32 képpont.

- **CaptionImageUrl** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ez a tulajdonság egy URL-címet határoz meg, amely a képfelirat képként megjelenő képre hivatkozik.

- **PreviewTitle** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ezzel a tulajdonsággal határozhatja meg a kijelölési előnézet mező tetején megjelenő szöveget.

- **EnableSelection** : ez egy nem kötelező, logikai típusú tulajdonság. Ezzel a tulajdonsággal határozhatja meg, hogy a listanézet kiválasztási módban van-e. Ha a listanézet kiválasztó módban van, a listanézet bal oldali oszlopában megjelenik a jelölőnégyzetek egyik oszlopa, és megjelenik a lista nézetének alján látható kiválasztási előnézet mező. A tulajdonság alapértelmezett értéke TRUE (igaz) értékre van állítva.

- **SingleSelection** : ez egy nem kötelező, logikai típusú tulajdonság. Ha a kijelölési mód be van kapcsolva a listanézet számára, akkor ez az érték TRUE (igaz) értékűre van állítva, hogy a végfelhasználó csak egy elemet válasszon ki a listából. Alapértelmezés szerint ennek a tulajdonságnak az értéke false (hamis) értékre van állítva. Ez azt jelenti, hogy alapértelmezés szerint a végfelhasználó több elemet is kijelölhet a listából.

- **RedirectUrl** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ezzel a tulajdonsággal adhatja meg, hogy melyik lapra kell átirányítani, amikor egy hiperhivatkozásos elem kattint a listában. Ez az URL-cím olyan helyőrzőket tartalmazhat, amelyek a tényleges értékkel lesznek lecserélve a futtatókörnyezet során. A helyőrzők a következők:

    - {0} Objektumtípus
    - {1} objectID
    - {2} displayName

- **ShowTitleBar** : ez egy nem kötelező, logikai típusú tulajdonság. Ezzel a tulajdonsággal adhatja meg, hogy látható-e a címsor. Ennek a tulajdonságnak az alapértelmezett értéke hamis.

- **ShowActionBar** : ez egy nem kötelező, logikai típusú tulajdonság. Ezzel a tulajdonsággal adhatja meg, hogy látható legyen-e a műveleti sáv. A tulajdonság alapértelmezett értéke TRUE (igaz).

- **ShowPreview** : ez egy nem kötelező, logikai típusú tulajdonság. Ezzel a tulajdonsággal adhatja meg, hogy látható legyen-e az előnézet területe. A tulajdonság alapértelmezett értéke TRUE (igaz).

- **ShowSearchControl** : ez egy nem kötelező, logikai típusú tulajdonság. Ezzel a tulajdonsággal adhatja meg, hogy látható legyen-e a keresési vezérlő. A tulajdonság alapértelmezett értéke TRUE (igaz).

- **ResultObjectType** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ezzel a tulajdonsággal adhatja meg a keresési eredmények várt objektumának típusát. Ennek a tulajdonságnak az alapértelmezett értéke az erőforrás. Ha a keresési eredmény több erőforrástípust tartalmaz, ezt az értéket erőforrásként kell megadni.

- **ColumnsToDisplay** : ez egy nem kötelező tulajdonság. Ezzel a tulajdonsággal adhatja meg, hogy mely attribútumok jelenjenek meg a listanézet számára oszlopokként. Ennek a tulajdonságnak az alapértelmezett értéke a DisplayName, a ResourceType. Minden oszlopot egy attribútum rendszerneve képvisel. Minden oszlop vesszővel van elválasztva. Nem kell megadnia a tulajdonság értékét, ha a listanézet a kiválasztási módban van használatban. A kiválasztási módban az oszlop beállítása a jelenleg kiválasztott keresési hatókör SearchScopeColumn attribútumáról származik.

- **ListFilter** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ez az az XPath, amely a listanézet megjelenítésére szolgál, és csak akkor érvényes, ha a ShowSearchControl tulajdonság hamis értékre van beállítva. Ha ez az érték meg van adva, a listanézet ezt a tulajdonságot használja a lekérdezésekhez, és a listanézet nem kiválasztási módban van. A szűrőt az erőforrás egy karakterlánc-attribútumához lehet kötni:

    `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>`

    vagy egy olyan karakterlánc, amely egy előre definiált környezeti változót tartalmaz:

    `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

- **TargetAttribute** : ez egy elavult tulajdonság. Az értéknek egy többértékű hivatkozott attribútum rendszerneveként kell lennie. Javasoljuk, hogy ez a tulajdonság ne legyen többé használatban. Például a Group Management szolgáltatásban a használata helyett:

    `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>`

    Használja

    `<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>`

- **ItemClickBehavior** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ezzel a tulajdonsággal adhatja meg, hogy szeretné-e, ha a lista nézet elemre kattint egy kiszolgáló visszaküldésének elindításához vagy az elem részletes nézetének megjelenítéséhez. Két beállításhalmaz támogatott: ModelessDialog és Server. Az alapértelmezett érték a ModelessDialog.

- **SearchOnLoad** : ez egy nem kötelező, logikai típusú tulajdonság, amely megadja, hogy a lista nézet vezérlőelemnek kell-e lekérdezni a terhelést. Ez a tulajdonság csak akkor alkalmazható, ha a listanézet kiválasztó módban van. A tulajdonság alapértelmezett értéke TRUE (igaz). Kikapcsolhatja, ha a felhasználónak általában szöveget kell beírnia a keresésbe, hogy értelmes eredményt kapjon. Ebben az esetben a listanézet először egy üzenetet jelenít meg, amely közli a felhasználóval, hogy hogyan végezhet keresést. A szöveget a következő tulajdonságokkal lehet testreszabni:

- **MainSearchScreenText** : Ez a választható, karakterlánc típusú tulajdonság csak akkor érvényes, ha a SearchOnload értéke TRUE (igaz). Ezzel a tulajdonsággal testreszabható a listanézet közepén megjelenő szöveg, ha a listanézet nem keres automatikusan. Ennek a tulajdonságnak az alapértelmezett értéke az erőforrásoknak a kereséssel való megkeresése az előzőekben leírtak szerint. Megadhat egy értéket, hogy a szöveg jobban legyen a forgatókönyvhöz kapcsolódóan.

- **SubSearchScreenText** : Ez a választható, karakterlánc típusú tulajdonság a **MainSearchScreenText** tulajdonság után megjelenő szöveg testreszabására szolgál. Általában nem kell értéket megadnia ehhez a tulajdonsághoz, ha további útmutatást szeretne adni a listanézet használatáról.

**Események** :

- Nincsenek események ehhez a vezérlőhöz.

**Példa** :

A listanézet-lista UocFilterBuilder-vezérlővel való használatáról a jelen dokumentum korábbi, UocFilterBuilder-mintáit ismertető cikkben talál példákat. A UocListView a Filter Builder nélkül is használható.


### <a name="uocnumericbox"></a>UocNumericBox

**Név** : UocNumericBox

**Leírás** : ez egy egyszerű szövegmező, amely csak egész értékeket vesz igénybe. Ez a vezérlő a írásvédett és a frissíthető módot is támogatja.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **MaxValue** : ez egy opcionális, integer típusú tulajdonság. Ezzel a tulajdonsággal határozhatja meg a vezérlő ügyféloldali érvényesítését. A végfelhasználó által megadott érték nem lépheti túl ezt az értéket. Megadhat egy explicit egész számot, vagy összekapcsolhatja azt egy adatforrásból a {bind Source = Schema, Path = IntegerMaximum} használatával.

- **MinValue** : ez egy opcionális, integer típusú tulajdonság. Ezzel a tulajdonsággal határozhatja meg a vezérlő ügyféloldali érvényesítését. A végfelhasználó által megadott érték nem lehet kisebb ennél az értéknél. Megadhat egy explicit egész számot, vagy összekapcsolhatja azt egy adatforrásból a {bind Source = Schema, Path = IntegerMinimum} használatával.

- **DefaultValue** : ez egy opcionális, integer típusú tulajdonság. Ezzel a tulajdonsággal határozhatja meg a vezérlő alapértelmezett értékét, ha a vezérlőt új adatértékek létrehozásához használja. Ez az érték csak statikus egész számra adható meg.

- **Érték** : ez egy opcionális, integer típusú tulajdonság. Ha egy adatforrásból származó egész típusú adatokkal társítja ezt, akkor az adott attribútum értéke jelenik meg, amikor a rendszer betölti a lapot, majd a küldés után menti az adatforrásba.

**Események** :

- **TextChanged** : ezt az eseményt akkor bocsátja ki a rendszer, ha a vezérlőn belüli aktuális érték megváltozik.

**Példa** :

![UocNumericBox-vezérlő](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
>Az alábbi mintakód az első numerikus mezőt hozza létre. A numerikus mező nem kapcsolódik adatforráshoz vagy bármely séma-adathoz.

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->
```

A következő mintakód a második numerikus mezőt hozza létre.

>[!NOTE]
>Ahhoz, hogy ez a minta működjön, először létre kell hoznia egy AnIntegerAttribute nevű új, egész típusú attribútumot, és hozzá kell kötnie az egyéni erőforrás típusával.

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

**Név** : UocPictureBox

**Leírás** : Ez a vezérlő a kép, a bináris típusú adattípusok megjelenítésére szolgál. Javasoljuk, hogy a vezérlőt bináris típusú adattípussal használja. A kép megjeleníthető egy megadott képurl-cím, bináris típusú adatok vagy a képtípust tartalmazó attribútum forrása alapján.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **ImageUrl** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Adja meg a cél kép URL-címét.

- **MaxHeight** : ez egy opcionális, karakterlánc típusú tulajdonság. Meghatározza a képpontban megjelenítendő képek maximális magasságát.

- **MaxWidth** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Meghatározza a képpontban megjelenítendő képméret maximális szélességét.

- **Képadatok átvitele** : ez egy bináris típusú tulajdonság. Ezzel a tulajdonsággal kötést hozhat létre egy adatforrást a megjelenített képpel. A kötött adatforrásnak bináris típusúnak kell lennie. Ezzel a mezővel explicit módon állíthatja be a képet úgy, hogy bájt [] formátumban adja meg az adattípust.

- **ImageResource** : ez egy opcionális, bináris típusú tulajdonság.

- **AlternativeText** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ez a tulajdonság alternatív szövegként jelenik meg, ha a kép nem jeleníthető meg.

**Események** :

- Nincsenek események ehhez a vezérlőhöz.

**Példa** :

>[!NOTE]
>A minta használatához meglévő képadatokkal kell kötnie a vezérlőt.

A következő kódrészlet létrehoz egy képmező vezérlőelemet, amely az adatforrást a vezérlővel köti össze:

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

A következő kódrészlet létrehoz egy képmező vezérlőelemet, amely az URL-képet a vezérlővel köti össze:

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```


### <a name="uocradiobuttonlist"></a>UocRadioButtonList

**Név** : UocRadioButtonList

**Leírás** : ez egy egyszerű, lehetőség-gomb lista. A lehetőségek kölcsönösen kizárják egymást ezen a listán. Ez a vezérlő akkor javasolt, ha a felhasználók öt vagy kevesebb lehetőség közül választhatnak. Ellenkező esetben a UOCListView használata javasolt.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **ValuePath** : az érték elérési útja értékre van állítva. A beállítás elemében lévő Value (érték) mezővel kötődik, az ebben a szakaszban leírtak szerint.

- **CaptionPath** : az érték elérési útjának értéke Caption. A beállítás elemében az ebben a részben leírtak szerint kötődik a "Caption" mezőhöz.

- **HintPath** : az érték elérési útja a tipp. A beállítás elemében a tipp mezőhöz kötődik, az ebben a szakaszban leírtak szerint.

- **SelectedValue** : a jelenleg kijelölt érték. Ez egy kötelező, karakterlánc típusú tulajdonság. Ez a tulajdonság az adatforrásból származó karakterlánc-adatokkal kötődik.

**Események** :

- **SelectedIndexChanged** : az esemény a kiválasztott választógomb megváltozásakor következik be.

- **CheckedChanged** : Ha a választógomb megváltoztatja az állapotát, az eseményt kibocsátja a rendszer.

**Beállítások** :

A vezérlőnek csak két **beállítási** eleme lehet. Egy **Options** elem struktúrájához lásd: <a href="#options-element">Options elem</a>.

- **Érték** : egy Option elem Value mezőjének igaz vagy hamis értéket kell beállítania.

- **Caption** : ez bármilyen sztring lehet.

- **Tipp** : ez lehet bármilyen karakterlánc-érték.

**Példa** :

![UocRadioButtonList-vezérlő](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
>Ahhoz, hogy ez a minta működjön, létre kell hoznia egy új logikai attribútumot, a ABooleanAttribute és az egyéni erőforrástípus kötését.

A következő kódrészlet létrehoz egy választókapcsoló-listát:

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton

**Név** : UocSimpleRadioButton

**Leírás** : ez egy egyszerű, kapcsoló gombos vezérlő. A vezérlő használata hasonló egy egyszerű jelölőnégyzethez. Két választógomb látható egymás mellett, a szöveg feliratozásával. A vezérlő logikai típusú adattípusra való kötése ajánlott.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **TrueText** : ez egy nem kötelező, karakterlánc típusú tulajdonság. A választókapcsoló kiválasztásakor megjelenő szöveg.

- **FalseText** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ez a szöveg jelenik meg, ha a beállítás gomb nincs kiválasztva.

- **SelectedItem** : ez egy nem kötelező, logikai típusú tulajdonság. Ez az érték azt jelzi, hogy a beállítás gomb van kiválasztva. Ez egy adatforrásból származó logikai típusú adatokkal is köthető. Az alapértelmezett érték false (hamis).

**Események** :

- **CheckedChanged** : Ha a választókapcsoló nem jelölt állapotra változik, vagy fordítva, a rendszer ezt a jelet adja meg.

**Példa** :

![UocSimpleRadioButton-vezérlő](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
>A minta működéséhez létre kell hoznia egy új logikai attribútumot, amelyet ABooleanAttribute kell létrehoznia, és hozzá kell kötnie az egyéni erőforrástípust. A RCDC-adattípust ugyanarra az egyéni erőforrástípus alkalmazza.

A következő kódrészlet létrehoz egy kapcsoló gombot:

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

**Név** : UocTextBox

**Leírás** : ez egy egyszerű szövegmező, amely támogatja a String típusú bevitelt. Azt javasoljuk, hogy a vezérlőt a karakterlánc típusú adattípusokkal való kötéshez használja.

**Tulajdonságok** :

- Az összes gyakori tulajdonság: ezekkel a tulajdonságokkal kapcsolatban lásd: <a href="#common-properties">Általános tulajdonságok</a>.

- **MaxLength** : ez egy opcionális, integer típusú attribútum. Ez a tulajdonság határozza meg a karakterlánc-bevitel maximális hosszát. Ennek a tulajdonságnak az alapértelmezett értéke 128 karakter.

- **Text** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ez a szöveg jelenik meg a szövegmezőben. Megadhat egy explicit karakterláncot, amely megjelenik a szövegmezőben a vezérlő kezdeti betöltése során, vagy egy karakterlánc-típus sémájának attribútumához kötődik.

- **Sorok** : ez egy opcionális, integer típusú tulajdonság. Ez a tulajdonság határozza meg a szövegmező magasságát a karakteres egységben. Az alapértelmezett érték egy karakter.

- **Oszlopok** : ez egy opcionális, integer típusú tulajdonság. Ez a tulajdonság határozza meg a szövegmező szélességét a karakteres egységekben. Az alapértelmezett érték 20 karakter.

- **Tördelés** : ez egy nem kötelező, logikai típusú tulajdonság. Ha a tulajdonság értékét True értékre állítja, a felhasználó engedélyezi a sortörési funkciót a szövegmezőben. A tulajdonság alapértelmezett értéke TRUE (igaz) értékre van állítva.

- **UniquenessValidationXPath** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Érvényes FIM XPath szűrő kifejezést fogad, és biztosítja, hogy a felhasználó által megadott érték egyedi legyen a szűrő hatókörében lévő erőforrásokon belül. Ha például azt szeretné, hogy a felhasználó a FIM Service DB összes levelezésre engedélyezett biztonsági csoportjában a megjelenítendő név egyedi legyen, az XPath-t kell használnia `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’` . Az érvényesítési művelet akkor történik meg, amikor a felhasználó elhagyja a lapot. Ez a tulajdonság csak az erőforrások létrehozásához használható RCDC támogatott.

- **UniquenessErrorMessage** : ez egy nem kötelező, karakterlánc típusú tulajdonság. Ez a karakterlánc egy hibaüzenetet jelenít meg, ha a UniquenessValidationXPath érvényesítése sikertelen, és lehet explicit szöveg vagy karakterlánc-erőforrás változó. Ha ez a tulajdonság nincs megadva, a sikertelen ellenőrzéshez tartozó alapértelmezett hibaüzenet a következő: "% VALUE% már létezik. Próbálkozzon egy másikkal. "

**Események** :

- **TextChanged** : ez az esemény a szövegmezőben megjelenő szöveg megváltozásakor lesz kibocsátva.

**Példa** :

A vezérlő teljes mintájának megtekintéséhez tekintse meg az egyszerű vezérlő mintáit ismertető szakaszt.


<br/>
<h2 id="appendix-a">A függelék: alapértelmezett XSD-séma</h2>

Ez a szakasz a teljes XSD-sémát mutatja a Microsoft Identity Manager 2016 SP1-ben biztosított összes alapértelmezett RCDCs.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```

<br/>
<h2 id="appendix-b">B függelék: alapértelmezett összegző XSL</h2>

Ez a szakasz a teljes összegző XSL-t mutatja, amelyet a Microsoft Identity Manager 2016 SP1 biztosít.

```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```
