---
title: A Microsoft Graph Microsoft Identity Manager-összekötője | Microsoft Docs
description: A Microsoft Graph Microsoft Identity Manager-összekötő lehetővé teszi a külső felhasználói AD-fiók életciklus-felügyeletét. Ebben a forgatókönyvben a szervezet felkérte a vendégeket az Azure AD-címtárba, és hozzáférést kíván biztosítani a vendégeknek a helyszíni Windows-hitelesítéshez vagy a Kerberos-alapú alkalmazásokhoz
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 63ebdbc229c7d2dabbbb5a06f7dbbd5f3d367796
ms.sourcegitcommit: cd503e8e9933d39d6fbf894c7d27bf9566301ac8
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/13/2020
ms.locfileid: "88168354"
---
# <a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Microsoft Graph Microsoft Identity Manager-összekötő


## <a name="summary"></a>Összefoglalás 


A [Microsoft Graph Microsoft Identity Manager-összekötő](https://go.microsoft.com/fwlink/?LinkId=717495) további integrációs forgatókönyveket tesz lehetővé prémium szintű Azure ad ügyfelek számára.  Az informatikai felületek a [beMicrosoft Graph API](https://developer.microsoft.com/en-us/graph/)v1-től és a Beta-tól kapott további objektumokat a fakiszolgálói szinkronizálási szolgáltatásban   .

## <a name="scenarios-covered"></a>Érintett forgatókönyvek


### <a name="b2b-account-lifecycle-management"></a>B2B-fiókok életciklusának kezelése


A Microsoft Graph Microsoft Identity Manager-összekötő kezdeti forgatókönyve összekötő, amely segít automatizálni AD DS fiók életciklus-felügyeletét a külső felhasználók számára. Ebben a forgatókönyvben a szervezet a Azure AD Connect használatával szinkronizálja az alkalmazottakat az Azure AD-be, és az Azure AD-címtárban is meghívja a vendégeket a AD DS. A vendég egy külső felhasználói objektumban való meghívása a szervezet Azure AD-címtárában történik, amely nem része a szervezet AD DSjának. Ezt követően a szervezet hozzáférést kíván biztosítani a felhasználóknak a helyszíni Windows integrált hitelesítéshez vagy a Kerberos-alapú alkalmazásokhoz az [Azure ad](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) -alkalmazásproxy vagy más átjáró-mechanizmusok segítségével. Az Azure AD-alkalmazásproxy megköveteli, hogy minden felhasználó saját AD DS fiókkal rendelkezzen az azonosítási és delegálási célból.  

Ha meg szeretné tudni, hogyan konfigurálhatja a címtár-szinkronizálást úgy, hogy AD DS fiókokat automatikusan hozzon létre és tartsa karban a vendégek számára, a jelen cikkben szereplő utasítások elolvasása után folytassa az Azure [ad Business-to-Business (B2B) és az Azure Application proxyval 2016 együttműködve](~/microsoft-identity-manager-2016-graph-b2b-scenario.md)a következő cikkben szereplő utasításokat:  Ez a cikk az összekötőhöz szükséges szinkronizálási szabályokat mutatja be.

### <a name="other-identity-management-scenarios"></a>Más Identitáskezelés-kezelési forgatókönyvek


Az összekötő más, a felhasználók, csoportok és kapcsolattartási objektumok létrehozására, olvasására, frissítésére és törlésére is használható az Azure ad-ben, az Azure ad-vel való felhasználói és csoportos szinkronizáláson túl. A lehetséges forgatókönyvek kiértékelése során vegye figyelembe a következőket: ez az összekötő nem üzemeltethető olyan forgatókönyvben, amely átfedésben van, tényleges vagy lehetséges szinkronizálási ütközést eredményez egy Azure AD Connect központi telepítéssel.  [Azure ad Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594)   a helyszíni címtárak és az Azure AD integrálásának ajánlott módja a felhasználók és csoportok szinkronizálása a helyszíni címtárakból az Azure AD-be.  Azure AD Connect sokkal több szinkronizálási funkcióval rendelkezik, és olyan forgatókönyveket tesz lehetővé, mint például a jelszó és az eszközök visszaírási, amelyek nem használhatók a famodul által létrehozott objektumok esetében. Ha az adatok bekerülnek AD DSba, például győződjön meg róla, hogy ki van zárva a Azure AD Connect az objektumoknak az Azure AD-címtárba való visszaállítására tett kísérletből.  Az összekötő nem használható olyan Azure AD-objektumok módosítására is, amelyeket a Azure AD Connect hozott létre.



## <a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Felkészülés a Microsoft Graph-összekötő használatára

### <a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Az összekötő engedélyezése az Azure AD-címtárban található objektumok lekéréséhez vagy kezeléséhez


1.  Az összekötőhöz létre kell hozni egy webalkalmazás-/API-alkalmazást az Azure AD-ben, hogy az engedélyt kaphat a megfelelő engedélyekkel az Azure AD-objektumokon való működéshez Microsoft Graphon keresztül.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

1. kép Új alkalmazásregisztráció

2.  A Azure Portal nyissa meg a létrehozott alkalmazást, és mentse el az alkalmazás AZONOSÍTÓját, amelyet később az MA kapcsolati lapján kell használni:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

2. kép Alkalmazásazonosító

3.  Új ügyfél-titkos kód generálása az összes beállítás – kulcsok megnyitásával \> . Állítsa be a kulcs leírását, és válassza ki a szükséges időtartamot. Mentse a módosításokat. A lap kihagyása után a titkos érték nem lesz elérhető.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

3. kép Új ügyfél titka

4.  Adja hozzá a "Microsoft Graph API"-t az alkalmazáshoz a "szükséges engedélyek" megnyitásával.

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

4. kép Új API hozzáadása

Az alkalmazásnak hozzá kell adni a következő engedélyeket, hogy az a forgatókönyvtől függően a "Microsoft Graph API" használatát engedélyezze:

| Művelet objektummal | Engedély szükséges                                                                  | Engedély típusa |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Csoport importálása          | `Group.Read.All` vagy `Group.ReadWrite.All`                                                | Alkalmazás     |
| Felhasználó importálása           | `User.Read.All`, `User.ReadWrite.All` , `Directory.Read.All` vagy`Directory.ReadWrite.All` | Alkalmazás     |

A szükséges engedélyekkel kapcsolatos további részletek [itt](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)találhatók.

5. Adja meg az alkalmazás számára a szükséges engedélyeket.


## <a name="installing-the-connector"></a>Az összekötő telepítése


6.  Az összekötő telepítése előtt győződjön meg arról, hogy a szinkronizálási kiszolgálón a következők vannak: 

 - Microsoft .NET 4.5.2-keretrendszer vagy újabb
 - Microsoft Identity Manager 2016 SP1, és a gyorsjavítás 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) vagy újabb verzióját kell használnia.

7. A Microsoft Graph-összekötő a Microsoft Identity Manager 2016 SP1-hez készült más összekötők mellett letölthető a [Microsoft letöltőközpontból](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  A webkiszolgáló-szinkronizálási szolgáltatás újraindítása.
 
## <a name="connector-configuration"></a>Összekötő konfigurációja



9.  A Synchronization Service Manager felhasználói felületén válassza az **Összekötők**   és **Létrehozás**lehetőséget.
Válassza a **Graph (Microsoft)** lehetőséget   , hozzon létre egy összekötőt, és adjon meg egy leíró nevet.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. A rendszerkiszolgálói szinkronizációs szolgáltatás felhasználói felületén határozza meg az alkalmazás AZONOSÍTÓját és a generált ügyfél titkos kulcsát. A fakiszolgálói szinkronizálásban konfigurált minden egyes felügyeleti ügynöknek saját alkalmazással kell rendelkeznie az Azure AD-ben, hogy ne futtassa párhuzamosan az importálást ugyanahhoz az alkalmazáshoz.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

5. kép Kapcsolat lap

A kapcsolat lap (kép 5) tartalmazza a használt Graph API verziót és a bérlő nevét. Az ügyfél-azonosító és az ügyfél-titkos kulcs az Azure AD-ben létrehozandó WebAPI-alkalmazás alkalmazás-AZONOSÍTÓját és kulcs értékét jelöli.

11. Végezze el a szükséges módosításokat a globális paraméterek oldalon:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

6. kép Globális paraméterek lap

A globális paraméterek lap a következő beállításokat tartalmazza:

- DateTime Format – a EDM. DateTimeOffset típusú attribútumokhoz használt formátum. Az importálás során a rendszer az összes dátumot karakterlánccá konvertálja az adott formátumban. A set Format tulajdonság minden olyan attribútumra érvényes, amely a dátumot menti.

 - HTTP-időkorlát (másodperc) – időtúllépés másodpercben, amelyet a rendszer a WebAPI alkalmazáshoz való minden HTTP-hívás során használni fog.

 - Jelszó kényszerített módosítása a létrehozott felhasználó számára a következő bejelentkezéskor – ez a beállítás az exportálás során létrehozandó új felhasználó számára lesz felhasználva. Ha a beállítás engedélyezve van, akkor a [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) tulajdonság értéke TRUE (igaz) lesz, ellenkező esetben hamis lesz.

## <a name="configuring-the-connector-schema-and-operations"></a>Az összekötő sémájának és műveleteinek konfigurálása


12.   Konfigurálja a sémát.  Az összekötő az alábbi objektumtípusok listáját támogatja:

-   Felhasználó

    -   Teljes/különbözeti importálás

    -   Exportálás (Hozzáadás, frissítés, törlés)

-   Csoport

    -   Teljes/különbözeti importálás

    -   Exportálás (Hozzáadás, frissítés, törlés)


Az attribútumok által támogatott típusok listája:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset`(karakterlánc az összekötő térben)

-   `microsoft.graph.directoryObject`(hivatkozás az összekötő területére a támogatott objektumok bármelyikéhez)

-   `microsoft.graph.contact`

A többértékű attribútumok (gyűjtemény) a fenti listából bármelyik típushoz is támogatottak.

Az összekötő a (z) " `id` " attribútumot használja a Anchor és a DN számára az összes objektumra vonatkozóan.  Ezért az átnevezés nem szükséges, mert Graph API nem teszi lehetővé az objektum számára az "id" attribútum módosítását.


## <a name="access-token-lifetime"></a>Hozzáférési jogkivonat élettartama


Egy Graph-alkalmazáshoz hozzáférési jogkivonat szükséges a Graph API eléréséhez. Egy összekötő minden importálási iterációhoz új hozzáférési jogkivonatot kér (az importálási iteráció az oldalméret méretétől függ). Például:

-   Az Azure AD 10000 objektumot tartalmaz

-   Az összekötőben konfigurált oldalméret 5000

Ebben az esetben két iteráció lesz az importálás során, amelyek mindegyike 5000 objektumot ad vissza a szinkronizáláshoz. Így az új hozzáférési jogkivonat kétszer fog megjelenni.

Az exportálás során új hozzáférési jogkivonatot kell kérni minden olyan objektumhoz, amelyet hozzá kell adni/frissíteni/törölni kell.

## <a name="troubleshooting"></a>Hibaelhárítás


**Naplók engedélyezése**

Ha problémák merülnek fel a gráfban, a rendszer a naplókat felhasználhatja a probléma lokalizálása érdekében. Így a Nyomkövetések ugyanúgy engedélyezhetők, [mint az általános összekötők esetében](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Vagy csak a következők hozzáadásával `miiserver.exe.config` (belső `system.diagnostics/sources` szakasz):

```
\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

\<add initializeData="ConnectorsLog"
type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,
Culture=neutral, PublicKeyToken=b77a5c561934e089"
name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,
DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>
```
>[!NOTE]
>Ha a "felügyeleti ügynök futtatása külön folyamatban" beállítás engedélyezve van, akkor `dllhost.exe.config` a helyett a következőt kell használni: `miiserver.exe.config` .

**A hozzáférési jogkivonat lejárt, hiba**

Az összekötő a 401-as HTTP-hiba miatt nem engedélyezett, üzenet: "hozzáférési jogkivonat lejárt".

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

7. kép "A hozzáférési jogkivonat lejárt." Hiba

A probléma oka lehet a hozzáférési jogkivonat élettartamának konfigurálása az Azure-oldalról. Alapértelmezés szerint a hozzáférési jogkivonat 1 óra elteltével lejár. A lejárati idő növeléséhez tekintse meg [ezt a cikket](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Példa erre az [Azure ad PowerShell-modul nyilvános előzetes kiadásának](https://www.powershellgallery.com/packages/AzureADPreview) használatával

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy-definition \@ ({"TokenLifetimePolicy": {"version": 1, **"AccessTokenLifetime": "5:00:00"**}} ")-DisplayName" OrganizationDefaultPolicyScenario "-IsOrganizationDefault \$ igaz-type" TokenLifetimePolicy "

## <a name="next-steps"></a>További lépések

- [Graph Explorer, a HTTP-hívással kapcsolatos problémák elhárításához]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [A Microsoft Graph verziószámozása, támogatása és megváltoztatási szabályzatai](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- Microsoft Graph Microsoft Identity Manager- [összekötő letöltése](https://go.microsoft.com/fwlink/?LinkId=717495) 
 [A többplatformos B2B végpontok közötti üzembe helyezése]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
