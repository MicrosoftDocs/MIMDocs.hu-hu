---
title: Microsoft Identity Manager 2016 SP1 – terminológia | Microsoft Docs
description: A Microsoft Identity Manager 2016 SP1 verzióban hivatkozott kifejezések átfogó listája.
keywords: Terminológia
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/28/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: fimguy
ms.suite: ems
ms.openlocfilehash: afe167cdcd6ca548ef34e802f5606bee6ba5b31e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760895"
---
# <a name="microsoft-identity-manager-2016-sp1-terminology"></a>Microsoft Identity Manager 2016 SP1 – terminológia

Ez a dokumentum a Microsoft Identity Manager 2016 SP1-ben hivatkozott kifejezések átfogó listáját tartalmazza.

## <a name="a"></a>A

**Active Directory csoport ellenőrzése** : a 2016-es eljárásban implementált eljárás, amely a csoport fiókneve egyediségét biztosítja Active Directory tárolt tartományon belül.

**műveleti munkafolyamat** : műveletet végrehajtó munkafolyamat. Ide tartoznak az értesítő e-mailek küldése, valamint a rendszerállapot-kezelő szolgáltatás adatbázisának módosításai.

**tevékenység** : a munkafolyamat-tevékenységek a Windows WORKFLOW Foundation (WF) munkafolyamatok alapszintű építőelemei. Magában foglalja azt a logikát, amely a munkafolyamatok létrehozásakor és futtatásakor a tervezéskor és a futási időben is megkezdődött.

**tevékenység szerelvénye** : A. DLL vagy egy. A munkafolyamat-tevékenység logikáját megvalósító .NET-szerelvényt tartalmazó EXE-fájl.

**Anchor** : egy vagy több olyan objektumtípus egyedi attribútuma, amely nem változik, és a csatlakoztatott adatforrás azon objektumát jelöli, amelyhez az összekötő területe objektum csatolva van (például egy alkalmazott száma vagy egy felhasználói azonosító).

**jóváhagyás** : a jóváhagyás olyan munkafolyamat-döntési pont, amely a munkafolyamatban való továbblépés előtt egy személy engedélyének beszerzésére használható.

**jóváhagyó e-mail** : Ha egy kérelem véglegesítése előtt jóváhagyásra van szükség, a rendszer egy jóváhagyási e-mailt küld az azonosított jóváhagyóknak.

**jóváhagyási kérelem** : jóváhagyást igénylő kérelem. Például egy jóváhagyási tevékenység feldolgozásának részeként a következő e-mail üzenet küldhető egy jóváhagyónak: 2016.

**jóváhagyási válasz** : egy jóváhagyási kérelemre adott válasz. Információt tartalmaz arról, hogy a kérést jóváhagyták-e vagy sem. Például egy e-mail-üzenet, amely az Outlook rendszerhez készült rendszerállapot-bővítményből érkezik a jóváhagyási kérelemre adott válaszban.

**jóváhagyások keresési mappája** : azok a keresési mappák, amelyeket az Outlookhoz készült, a felhasználói fiókhoz tartozó beépülő modul hozott létre, és amely lehetővé teszi a felhasználó számára a függőben lévő és a jóváhagyott jóváhagyások megtekintését, valamint a jóváhagyási kérelmek

**jóváhagyási küszöbérték** : a feldolgozás folytatására irányuló kérelem engedélyezéséhez szükséges pozitív jóváhagyási válaszüzenetek száma.

**jóváhagyó** : az a személy, aki jóváhagyja a kérést, hogy folytassa a következő lépéssel. A jóváhagyási kérések üzeneteit kapják meg, ha a rendszer a következőt használja az Outlookhoz. Lásd még a "eszkalációs jóváhagyó" bejegyzést.

**attribútum folyamata** : Ez határozza meg, hogy az attribútumok milyen értékkel áramlanak a fakiszolgálói szolgáltatás és más külső rendszerek között.

**hitelesítési tevékenység** : egy munkafolyamat-tevékenység, amely érvényesíti a felhasználó identitását. Például a jelszó-visszaállítási kapu és az intelligens kártyás hitelesítési kapu. Lásd még a "QA Gate" és a "zárolási kapu" bejegyzést.

**hitelesítési kérdés** : egy párbeszédpanel, amely megköveteli a felhasználótól, hogy adjon meg egy választ a webszolgáltatási 2016-es hitelesítésre. Tegyük fel például, hogy a felhasználó válaszol a jelszavuk alaphelyzetbe állítására.

**hitelesítési kérdés tevékenysége** : egy Windows Workflow Foundation tevékenység, amely egy felhasználó számára kiállított kihívás konfigurálására szolgál a webhelyhez tartozó 2016-hitelesítéshez.

**engedélyezési munkafolyamat** : olyan tevékenységek munkafolyamata, amelyeknek a kérésnek az adatbázisba való véglegesítése előtt el kell végeznie. Ilyenek például az adatellenőrzés és a jóváhagyás.
<br/>

## <a name="c"></a>C#

**regisztrációs attribútum törlése** : ez az attribútum törli a hitelesítési munkafolyamathoz társított regisztrációt. Egy kérdés és válasz kihívás esetében például a válaszokat a következő helyen tárolja a rendszer: a rendszer a webhelyre 2016 a regisztrációs adat formájában. Ha a regisztráció törlése jelölőnégyzet be van jelölve, és a rendszer menti a munkafolyamatot, a rendszer törli a regisztrációs adatforgalmi listát, hogy a felhasználók újra regisztráljanak.

**számított tag (vagy tag)** : a manuálisan felügyelt tagok és a szűrők kombinációjából kiszámított erőforrások írásvédett készlete.

**Connector** : az összekötő területének egy olyan objektuma, amely egy csatlakoztatott adatforrásban lévő objektumot jelöl, és amely jelenleg előre definiált szabályokkal van összekapcsolva a metaverse-objektummal. A Metadirectory összekötő objektumokat használ a csatlakoztatott adatforrás és a metaverse közötti attribútumérték szinkronizálásához.

**összekötő-szűrő** : olyan szabály, amellyel megakadályozható, hogy az összekötő terület objektumai a metaverse-objektumokhoz legyenek csatolva.

**összekötő területe** : olyan átmeneti terület, amely a kijelölt objektumok és attribútumok egy csatlakoztatott adatforrásban való ábrázolását tartalmazza. Az összekötő terület objektum az összekötő azon területe, amelyet a rendszer a csatlakoztatott adatforrásból származó adatok importálásával vagy a többhelyes adatforrásban lévő szabályok használatával hoz létre, és a különböző csatlakoztatott adatforrásokban lévő új objektumokat hoz létre. Ezek az objektumok olyan attribútumokat tartalmaznak, amelyek a csatlakoztatott adatforrásban lévő megfelelő objektumokból importálhatók vagy exportálhatók.

**Count XPath** : XPath-kifejezés, amely az erőforrás megjelenítendő neve után zárójelek között megjelenítendő numerikus értéket adja vissza.

**feltételeken alapuló tag** : az erőforrások írásvédett készlete, amely a statikus csoporttagok és a szűrők kombinációjával lett kiszámítva.

**feltételeken alapuló tagság** : az a csoport, amelyben a csoport tagságát egy szűrő határozza meg. Lásd még a "statikus tagság" bejegyzést.

**erdők közötti tag** : egy olyan biztonsági csoport tagja, amelynek felhasználói fiókja egy másik erdőben található, a csoport fiókból.

erdők **közötti csoportos számítás** : olyan beépített tevékenység, amely egy csoport erdőbeli tagjai között egy olyan, az erdőhöz társított idegen rendszerbiztonsági tag (FSP) készletben helyezkedik el, amelyben a csoport található.


**egyéni kifejezés** : a függvények vagy attribútumok speciális módban való definiálásához használt leíró nyelv.
<br/>

## <a name="d"></a>T

**célhely (vagy cél erőforrás-definíció kérés után)** : egy készlet, amelybe az erőforrás egy olyan kérelem miatt mozog, amely megváltoztatja az adott erőforrás attribútumait.

**alapértelmezett csoport-ellenőrzési tevékenység** : egy beépített munkafolyamat-tevékenység, amely meghatározza, hogy a csoport-felügyeleti kérelmek sértik-e a 2016-es vagy Active Directory konfigurációt vagy házirendet.

**leválasztó** : az összekötő területének egy olyan objektuma, amely egy csatlakoztatott adatforrásban lévő objektumot jelöl, és jelenleg nincs a metaverse-objektumhoz társítva.

**leválasztó objektumok** : három típusú leválasztó objektum létezik: leválasztó, explicit leválasztó és szűrt leválasztó.

**megjelenítendő név** : egy erőforrás egy olyan attribútuma, amely egy felhasználói felületen jelenik meg az erőforrás azonosításához. A megjelenített névben használt értéknek egyértelműnek és emberi számára olvashatónak kell lennie. Fontos, hogy megjelenítendő nevet adjon meg, ha az erőforrást különböző webszolgáltatási portál-vezérlőkben szeretné használni, például az erőforrás-választóval.

**terjesztési csoport** : olyan erőforrások gyűjteménye, amelyek a leggyakrabban használt felhasználók és más csoportok, amelyek e-mailben küldhetnek e-mailt a csoport postaládájába.

**tartományi konfiguráció** : Active Directory tartományok modellezéséhez használt konfigurációs erőforrás.

**tartományi helyi csoport** : a tartomány helyi hatókörű csoport egy Active Directory csoport, amely egy adott tartományon belüli erőforrásokat biztosít, és amely az adott erdőből vagy bármely megbízható erdőből származó tagokat tartalmazhat.

**drop file (fájl eldobása** ): a drop-fájl egy olyan XML-naplófájl, amely az exportálás vagy az importálás lehetséges értékeit jelöli.

**dinamikus attribútumérték** : a más attribútumok alapján kiszámított attribútum értéke. A Name attribútum kiszámítása például a megadott név és a vezetéknév összefűzésével történik.

**dinamikus csoport** : az a csoport, amelynek a tagságát a rendszer automatikusan meghatározza és naprakészen 2016 tartja a következővel, hogy a csoport tartalmazza az összes olyan erőforrást (például személyeket, csoportokat, számítógépeket), amelyek az XPath használatával kifejezett feltételek alá esnek.
<br/>

## <a name="e"></a>E

**enumerálás** : a webkiszolgáló 2016 szolgáltatás által visszaadott erőforrások listája.

**eszkaláció** : Ha a megadott időn belül nem fejeződik be jóváhagyás, a jóváhagyás megtörténik, és a rendszer további jóváhagyókat ad hozzá a jóváhagyáshoz.

**eszkaláció jóváhagyója** : az a felhasználó, aki megkapja a jóváhagyási kérés üzeneteit, ha a jóváhagyók nem válaszolnak. Lásd még a "jóváhagyó" bejegyzést.

**explicit Connector** : az összekötő területének egy olyan objektuma, amely a metaverse-objektumhoz van csatolva, és nem lehet összekötő-szűrővel leválasztani. Explicit összekötő csak manuálisan hozható létre az asztalos kapcsolattal, és csak a kiépítés vagy az Ács használatával lehet leválasztani.

**explicit leválasztó** : az összekötő olyan objektuma, amely nincs a metaverse objektumához csatolva, és csak Ács használatával csatlakoztatható. Egy objektum explicit leválasztó lesz, ha kézzel leválasztja az objektumot az Ács használatával.

**Exportálás** : az Exportálás a csatlakoztatott adatforrások változásainak leküldési folyamata. Az Exportálás mindig az utolsó sikeres importáláson alapuló különbözeti művelet. A következő folyamat dolgozza fel a módosításokat, de nem dolgozza fel az összekötő területének változásait, amíg a módosításokat új importálás nem erősítette meg. A használt felügyeleti ügynök típusától függően az Exportálás által végrehajtott módosítások az attribútum, az objektum vagy az érték szintjén lehetnek.

**Exportálási attribútum** folyamata: az objektum attribútumainak a metaverse-ből az összekötő területére való fordításának folyamata. Ez a folyamat egy-az-egyhez típusú hozzárendeléseket tartalmazhat, szabályok kiterjesztését alkalmazva módosíthatja az attribútumokat, és beállíthatja a statikus attribútumok értékét. Az exportált attribútumok a csatlakoztatott adatforráshoz való következő exportáláshoz szükséges összekötő-térben vannak berendezve.

**Extensible reteszt Markup Language (XAML)** : XML-alapú nyelv, amelyben a munkafolyamat-definíciók jelennek meg.

**külső rendszer-hatókörű szűrő** : meghatározza, hogy az Ön által azonosított és szűrni kívánt erőforrások egy adott feltétel alapján legyenek meghatározva.

**külső rendszererőforrás típusa** : Ez annak az erőforrásnak a típusa a külső rendszeren, amelyhez a 2016-es rendszer-erőforrás csatlakoztatva van.

**külső rendszererőforrás-létrehozási jelző** : egy szinkronizálási szabály paramétere, amely azt jelzi, hogy létre kell-e hozni egy erőforrást az összekötő területén, ha a kapcsolati feltételek alapján az adott erőforrás nem létezik a külső rendszeren. Lásd: a be2016 erőforrás-létrehozási jelzője. <!-- Not sure what this is, should this be a term? -->

**külső rendszerhatókör** : egy olyan szinkronizációs szabály paramétere, amely egy olyan szűrőt tartalmaz, amely a szabály hatálya alá eső külső rendszeren lévő erőforrásokat mutatja.
<br/>

## <a name="f"></a>F

**Filter (szűrő** ): szűrési feltételeket tartalmazó kifejezés. Egy szűrő megegyezik egy erőforrással, ha a szűrőben szereplő összes szűrési feltétel megfelel az erőforrásnak. A 2016-as webalkalmazásban a szűrő XPath-szintaxist használ.

**szűrt leválasztó** : az összekötő területének egy olyan objektuma, amely megakadályozza, hogy a társított felügyeleti ügynök összekötő-szűrési szabályai alapján csatlakozhasson a metaverse-objektumhoz, vagy a projektben a tervbe kerüljön.

**FIM felügyeleti ügynök** : egy felügyeleti ügynök, amely szinkronizálja a 2016-es és a webkiszolgáló 2016 szinkronizációs szolgáltatását.

**FIM/felhasználói jelszó-visszaállítási ügyfélszolgáltatás** : Ez arra a proxy szolgáltatásra vonatkozik, amely a felhasználó számítógépén található, amely a (z) 2016-kiszolgálóval kommunikál.

**FIM/a jelszó-visszaállítási bővítmények** : Ez a végfelhasználó számítógépén található kódra vonatkozik, amely kibővíti a Windows-bejelentkezés funkcióit, hogy magába foglalja az önkiszolgáló jelszó-visszaállítást.

**FIM/a rendszer erőforrás-létrehozási jelzője** : egy szinkronizálási szabály paramétere, amely azt jelzi, hogy egy erőforrást létre kell-e hozni a webszolgáltatáshoz tartozó 2016 adatbázisban, ha a kapcsolati feltételek alapján az erőforrások nem léteznek. Lásd még: "külső rendszererőforrás-létrehozási jelző" bejegyzés.

**Function** : egy olyan összetevő, amely egy szinkronizálási szabályban vagy egy munkafolyamat-definícióban is feldolgozható az adatértékek feldolgozásához.
<br/>

## <a name="g"></a>G

**Gate** : a kérelmek feldolgozásának hitelesítési fázisában használt munkafolyamat-tevékenység. Lásd még a "QA Gate" és a "zárolási kapu" bejegyzést.

**csoportos beágyazás** : egy csoport definíciójának egy mezője, amely meghatározza, hogy a csoport más csoportokat is tartalmaz-e az aktuális csoport tagjaiként.

**Csoport hatóköre** : egy csoport definíciójának, egy vagy "helyi", "globális" vagy "univerzális" mezője. További információ: Active Directory csoport hatóköre.
<br/>

## <a name="i"></a>I

**Importálás** : a csatlakoztatott adatforrások objektumainak a létrehozási, módosítási, törlési vagy ellenőrzési célokra való áthelyezésének folyamata. Az importálás lehet teljes vagy különbözeti művelet. Teljes importáláshoz a rendszer az összes kijelölt objektumot kéri a csatlakoztatott adatforrásból, és törli az összes olyan átmeneti objektumot, amelyhez az importálás során nem érkezett megfelelő objektum. Ennek eredményeképpen ez a futtatási profil lépés akkor hasznos, ha az átmeneti objektumokat az összekötő területéről kívánja megtisztítani. A csatlakoztatott adatforrásból kapott objektumok az összekötő területének megfelelően vannak elrendezve. Ahhoz, hogy a különbözeti Importálás a kívánt eredményeket adja meg, a csatlakoztatott adatforrásnak a vízjelek egyik formáját kell megvalósítania. A csatlakoztatott adatforrás a vízjel használatával jelzi, hogy mikor történt a legutóbbi változás egy objektumra vonatkozóan. A többértékű adatkészlet beolvassa a vízjelet, és meghatározza, hogy mit tartalmazzon a különbözeti importálás Erre példa a Active Directory USN.

Attribútum **importálása (IAF)** : az attribútum importálása folyamatban van egy attribútum importálása az összekötő területéről a metaverse-be. Ez a folyamat magában foglalhatja az egy-az-egyhez attribútum-hozzárendelések alkalmazását, szabályok kiterjesztésével módosíthatja az attribútumokat, vagy statikus attribútumokat állíthat be.

**rendszerkép URL-címe** : 2016 a webalkalmazás-kezelő felhasználói felületén megjelenítendő képfájl URL-címe.

**kezdeti folyamat** : a kezdeti folyamat egy olyan attribútumérték-folyamat, amely csak egyszer lesz alkalmazva, amikor az erőforrás első alkalommal jön létre. Ez azt eredményezi, hogy a rendszer csak akkor hozza létre a kezdeti jelszót, amikor első alkalommal hoz létre fiókot.

**interaktív munkafolyamat** : olyan munkafolyamat, amely a módosítást kérő felhasználótól választ kér, például további hitelesítési ellenőrzések elvégzéséhez.
<br/>

## <a name="j"></a>J

**Csatlakozás** : egy összekapcsolási folyamat egy összekötő terület objektum egy meglévő metaverse-objektummal való összekapcsolására szolgál. Az attribútum értékei csak a csatolt objektumok között áramlanak.

**Csatlakozás csoportos kérelemhez** : egy felhasználó egy csoportba való felvételére vonatkozó kérelem.
<br/>

## <a name="l"></a>L

**zárolás** : egy olyan konfigurációs beállítás a 2016-adatbázisban lévő személy-erőforráson, amely korlátozza, hogy a felhasználó hitelesítő adatokkal rendelkezik a fakiszolgáló 2016 vagy a jelszó-visszaállítás végrehajtásakor.

**zárolási kapu** : munkafolyamat-tevékenység a kérelmek feldolgozásának hitelesítési fázisában egy olyan felhasználó kizárásához, aki nem tudott hitelesíteni. Lásd még a "zárolás" és a "QA Gate" bejegyzést.

**zárolási küszöb** : ez egy egész szám típusú vezérlőelem, amely meghatározza, hogy a felhasználók hányszor tudják befejezni a hitelesítési munkafolyamatot, mielőtt kizárták őket a zárolás időtartamára.Az alapértelmezett beállítás a 3. Az alsó korlát 0, a felső korlát 99.

**zárolás időtartama** : ez egy egész szám típusú vezérlőelem, amely meghatározza, hogy az időtartam hány perc múlva legyen zárolva a felhasználó számára a zárolási küszöb megnyomása után.Az alapértelmezett beállítás 15 perc.A beállítás alsó határértéke 1, a felső korlát pedig 9999.A felső korlát lehetővé teszi, hogy a rendszergazda a felső korlátot egy napnál hosszabb ideig állítsa be.

**zárolási küszöbértékek a végleges zárolás előtt** : ez egy egész számú vezérlőelem, amely lehetővé teszi a rendszergazda számára, hogy numerikus értéket állítson be ahhoz, hogy a felhasználók hányszor tudják elérni a zárolási küszöbértéket, mielőtt véglegesen kizárták őket.  Az állandó zárolás azt jelenti, hogy a felhasználót a rendszergazdának kell feloldania. Alapértelmezés szerint ez a beállítás a 3 értékre van állítva.A beállítás tartománya 1 és 99 között van.
<br/>

## <a name="m"></a>M

**felügyeleti ügynök** : egy felügyeleti ügynök (ma) csatlakoztat egy adott csatlakoztatott adatforrást a Metadirectory. Feladata, hogy a csatlakoztatott adatforrásból a központba helyezi az adatátvitelt, és azokat a szabályokat, amelyek meghatározzák, hogy milyen jogosultságot biztosítanak az azonosító adatoknak a munkahelyen belüli és más csatlakoztatott adatforrásokban való részvétel Ha a Metadirectory lévő adatokat módosítják, a felügyeleti ügynök exportálhatja az adatokat a csatlakoztatott adatforrásokra, hogy a csatlakoztatott adatforrás szinkronizálva legyen a többhelyes adatforrással. Ügynök-alapú összekötő helyett felügyeleti ügynököt használunk.

**Felügyeleti házirend szabálya (MPR)** : a felügyeleti szabályzat szabályai (MPR) olyan mechanizmust biztosítanak, amely lehetővé teszi az üzleti feldolgozási szabályok modellezését a beérkező kérelmekhez a 2016-es kiszolgálóra. A következő engedélyekkel szabályozzák a műveletekhez szükséges engedélyeket a felügyeleti webszolgáltatások 2016-erőforrásain, valamint a kérelmek által aktivált munkafolyamatokat.
   
   Kétféle MPR létezik:
   
- Kérelem MPR: engedélyek megadása és munkafolyamatok futtatása (meghívása a kért művelet végrehajtása előtt).
- Áttérési MPR beállítása: csak munkafolyamatok futtatása (az alkalmazott állapot változására való reagálás).
   
  A MPR fő tervezési objektumai a következők:
   
- Modellezési engedélyek
- Modellezési munkafolyamatok leképezései
- Modellezési átmenetek
- A visszaható definíciók modellezése
- Nem hitelesített felhasználói hozzáférés modellezése
- Időbeli szabályzatok modellezése
- Modellezési szűrő engedélyei

**manuálisan felügyelt tag** : annak a csoportnak vagy készletnek a tagsága, amely a felhasználók, csoportok vagy egyéb erőforrások manuálisan kiválasztott listájából áll.

**metaverse** : az a központi adattár, amely az összesített azonosító adatokat több csatlakoztatott adatforrásból is tartalmazza, így egyetlen globális, integrált nézetet biztosít az összes egyesített objektumról. A metaverse nem használható táblázatként vagy nézetként az összes olyan alkalmazáshoz, amely nem a (z) rendszerbeli, mert ez a sérülés okozhatja.

**figyelt postaláda** : egy olyan postaláda, amelyet a (z) 2016-es a szolgáltatás figyelőkkel fogad, és e-mailek fogadására kéri az Outlook-bővítményt.
<br/>

## <a name="n"></a>N

**értesítési tevékenység** : a kérelmek feldolgozásának műveleti fázisán belüli munkafolyamat-tevékenység, amelyben a 2016-es e-maileket küld egy vagy több felhasználónak, hogy értesítse a kérést.

**értesítési üzenet** : az értesítési tevékenység által küldött e-mail-üzenet. Lásd még az "értesítési tevékenység" bejegyzést.
<br/>

## <a name="o"></a>O

**ObjectId (ResourceId)** : olyan attribútum, amely globálisan egyedi azonosítót (GUID) tartalmaz, amelyet a rendszer az egyes erőforrásokhoz 2016 rendel a létrehozáskor. Ezt az erőforrás-azonosítót is nevezik.

**objektumazonosító** : egy X. 509 digitális tanúsítványban található mező azonosítójának, illetve egy LDAP-alapú címtárszolgáltatás attribútum típusának vagy objektumosztály értékének sorozata. Az objektumazonosítók általában szoftvergyártók és szabványok szervei vannak hozzárendelve.

**művelet típusa** : a művelet típusát a webszolgáltatáson keresztül a 2016-es webszolgáltatáson keresztül felügyelt erőforrásra kell kérni. Ez magában foglalja az erőforrások létrehozását és törlését, valamint az erőforrás-attribútumok olvasását és módosítását. Emellett a hozzáadási/eltávolítási műveletek lehetővé teszik, hogy további vezérlést alkalmazzon a módosítási műveletre, hogy csak az attribútumok és az azok eltávolításának értékét szabályozza.

**operátor** : egy szűrő olyan eleme, amely az adatértékek összehasonlítását vagy más kapcsolatát határozza meg.

forrás **beállítása (vagy cél erőforrás-definíciója a kérelem előtt)** : egy készlet, amelyben az erőforrás az adott erőforrás attribútumainak változása előtt lett betartva.
<br/>

## <a name="p"></a>P

**paraméter** : új erőforrások kiépítéséhez esetenként lehetséges, hogy egy külső forrásból, például egy felhasználótól származó attribútum-értékeket biztosítanak. Az attribútumérték paraméterként való megadása lehetővé válik egy új erőforrás sikeres létrehozása.

**Partition (partíció** ): az összekötő területének logikai mennyisége. Egy felügyeleti ügynök létrehozhat egy vagy több partíciót, amelyekkel logikailag oszthatja szét az adatcsoportokat külön logikai csoportokba. Az egyes adatmennyiségeket a rendszer külön dolgozza fel a szinkronizálás során.

**jelszó alaphelyzetbe állítása** : egy eljárás, amellyel a felhasználó jelszava módosítható egy ismert értékre, olyan helyzetekben, amikor a felhasználó elfelejtette vagy elveszítette a jelszavát. Lásd még a "regisztráció" bejegyzést.

**fázis** : minden erőforrás-létrehozási, frissítési vagy törlési kérelem feldolgozása három munkafolyamat-fázison keresztül történik. A hitelesítési fázisban a kérelmező felhasználó további hitelesítési ellenőrzése végezhető el. Az engedélyezési fázisban a szükséges jóváhagyások gyűjtése történik. A művelet fázisában a tevékenységek végrehajtása az erőforrás módosítására irányuló kérelem véglegesítése után történik.

**helyőrző objektumok** : az összekötő területének azon objektumai, amelyek a csatlakoztatott adatforrás hierarchiájának egyetlen szintjének felelnek meg. Ha például egy Active Directory erdőben lévő objektumokat szeretne szinkronizálni, importálnia kell a Active Directory objektumok elérési útját alkotó tárolókat. A példában a CN = Mikdol, OU = Users, DC = Microsoft, DC = com, helyőrző objektumok jönnek létre a DC = com és a DC = Microsoft, DC = com esetében. Emellett a helyőrző objektum olyan objektumot is jelenthet a csatlakoztatott adatforrásban, amelyhez az importált hivatkozási attribútum értéke (például az objektum, amelyre a kezelő attribútum a felhasználói objektumban hivatkozik). A helyőrző objektumok nem tartalmaznak attribútum-értékeket, és nem kapcsolhatók a metaverse-hez.

**Házirend-kezelés** : a felügyeleti webszolgáltatások 2016-ben történő felügyeletét SharePoint-alapú konzol teszi elérhetővé a házirendek létrehozásához és érvényesítéséhez. A bővíthető Windows Workflow Foundation-alapú munkafolyamatok lehetővé teszik a felhasználók számára az identitáskezelési házirendek definiálását, automatizálását és betartatását. A házirend-kezelés magában foglalja a heterogén identitás-szinkronizálást és a konzisztenciát, amelyet a hálózati operációs rendszerek, e-mailek, az adatbázisok, a címtár, az alkalmazások és a fájl-hozzáférés széles körének integrálásával érhet el.

**házirend frissítése (vagy a házirend frissítésének futtatása)** : Ha egy munkafolyamatot újra kell futtatni a készletek vagy a MPR hivatkozó változásának hatására, akkor a házirend-frissítési jelző ezt jelzi.

**prioritás** : a szinkronizálási szabályok sorrendje.

**rendszerbiztonsági tag** : a felügyeleti házirend-szabályban használt készlet határozza meg a felügyeleti házirend-szabály kiértékelését kezdeményező erőforrások (általában felhasználók) készletét.

**elsődleges készlet az erőforráshoz viszonyítva** : ez egy visszaható tulajdonság. Az érték az erőforrás-tulajdonságok egyikének kifejezésében van definiálva. A szolgáltatás a dinamikus felügyeleti szabályzatok azon szabályainak meghatározására szolgál, amelyek feltételeit a rendszer minden feldolgozott cél erőforrás kontextusában értékeli ki.

**kivetítés** : a kivetítési szabályok alapján létrehoz egy objektumot a metaverse-ben, majd automatikusan összekapcsolja az adott objektumot az összekötő terület egy meglévő objektumával.

**kiépítés** : az objektumok létrehozásának, átnevezésének és megszüntetésének folyamata az előre meghatározott összekötő helyeken a metaverse objektum változásai alapján. A kiépítési szabályok akkor állíthatók be, ha egy metaverse-objektumot módosítanak. Ezek a szabályok olyan objektumorientált műveleteket hajthatnak végre, mint például az új összekötő terület objektum létrehozása vagy a meglévő összekötő-objektumok kapcsolatának leválasztása a metaverse-objektumhoz.
<br/>

## <a name="q"></a>Q

**QA Gate** : egy munkafolyamat-tevékenység egy hitelesítési fázisban, amelyben a kérelmező felhasználónak meg kell adnia egy vagy több előre meghatározott kérdésre adott válaszokat. Ez a tevékenység általában a jelszó-visszaállításban használatos, így a felhasználó megkérdőjelezheti személyazonosságát úgy, hogy a felhasználó számára olyan előre meghatározott kérdéseket biztosít, amelyek esetében csak az adott felhasználó fogja tudni, hogy a felhasználónak meg kell adnia a megfelelő választ. Lásd még a "zárolási kapu" bejegyzést.

**QA Challenge** : olyan kihívás, amely megköveteli a felhasználótól, hogy több kérdés megválaszolásával válaszoljon a webszolgáltatási 2016-es hitelesítéshez.
<br/>

## <a name="r"></a>R

**véletlenszerű jelszó beállításai** : Ez a beállítás határozza meg, hogy hány karakter szükséges a jelszó beállításához a külső címtárban.

**hivatkozási attribútum típusa** : az attribútum típusa, amelyben az attribútum értékei a ObjectId (globálisan egyedi azonosítók) attribútumának értékei a 2016-as egyéb erőforrások esetében.

**hivatkozási integritás** : egy olyan korlátozás a ObjectId 2016-ben, amelyben a hivatkozási attribútum nem rendelkezhet olyan erőforrás-azonosítóval, amely törölve lett.

**regisztráció** : a felhasználók önkiszolgáló jelszó-visszaállításának konfigurálására szolgáló eljárás. Lásd még a "QA Gate" bejegyzést.

**újbóli regisztráció** : a hitelesítő adat regisztrációjának frissítése a webszolgáltatási 2016-ben, jellemzően a jelszó-visszaállítási regisztrációra vonatkozó rendszergazdai házirend módosítása után szükséges.

**kapcsolat létrehozása** : egy szinkronizálási szabály konfigurációs jelzői, amely meghatározza, hogy a rendszer automatikusan létrehozza-e az erőforrásokat a webszolgáltatási 2016-ben vagy a külső rendszeren, ha az erőforrások hiányoznak.

**kapcsolati feltételek** : olyan szinkronizációs szabály beállítása, amely a többhelyes kiszolgáló erőforrásainak és a külső rendszerekben lévő erőforrások egyeztetésére szolgál.

**kapcsolat leállítása** : azt jelzi, hogy a más külső rendszerekhez kapcsolódó erőforrásokat le kell-e kapcsolni (és esetleg törölni kell), ha a szinkronizálási szabály már nem érvényes.

**kérelmek kezelése** : a felhasználók a beküldött kérelmek és a társított munkafolyamatok kezelésével és kezelésével kapcsolatos képességeket is használhatják.

**kérelem-felügyeleti házirend-szabály (RMPR)** : a rendszer kiértékeli és alkalmazza a bejövő kérelmeket a műveletek végrehajtásához. A KÉSZLETÁTMENET elsődlegesen a többpontos hozzáférési szabályzatok definícióinak létrehozásához használatosak. Más szóval a kérés kezelésének válasza. A RMPR konfigurálásakor a kérelmező egy művelet végrehajtására tervezett készlettel rendelkezik.

   A webkiszolgálói architektúra hat különböző műveletet határoz meg, amelyeket a RMPR a következőhöz lehet definiálni:
   
- Erőforrásokat hozhat létre.
- Erőforrás törlése.
- Erőforrás beolvasása.
- Adjon hozzá egy értéket egy többértékű attribútumhoz.
- Érték eltávolítása többértékű attribútumból.
- Egyetlen értékkel rendelkező attribútum módosítása.
   
  RMPR megadásakor ki kell választania legalább az egyiket a hat művelet közül. A művelet mindig a kérelmező környezetében van definiálva. Minden feltételhez meg kell határozni egy célt. Egy adott célra alkalmazott művelet a cél erőforrás állapotának átváltását eredményezheti. A rendszer mindig meghívja a RMPR a kért művelet végrehajtása előtt. A feltétel céljának hatékony jellemzéséhez két különböző állapotot kell konfigurálnia:
   
- Cél erőforrás-definíciója a kérelem előtt: a cél állapota a kérelem alkalmazása előtt.
- A cél erőforrás-definíciója a kérelem után: a kérelem állapota a kérés alkalmazása után.
   
  Azt határozza meg, hogy mindkét állapotot meg kell-e határozni attól függően, hogy a RMPR melyik művelethez van definiálva. A létrehozási műveletben a kért erőforrás nem rendelkezik Kezdeti állapottal. Ezért csak a cél erőforrás-definíciót kell konfigurálnia a kérelem után a létrehozási művelethez.
   
  Az olvasási vagy törlési művelet nem okoz állapot-átállást. A két művelet esetében csak a cél erőforrás-definíciót kell megadnia a kérelem előtt.
   
  A módosítási művelet vagy az összes egyéb művelet esetében mindkét állapotot konfigurálnia kell, amelyeknek ugyanaz az értéke, ha nem történik meg az állapot átalakulása.
   
  A kapcsolódó erőforrásokat a kérelmezőhöz viszonyítva (például a kérelmező saját felhasználói objektumával, a célként megadott felhasználó kezelőjével vagy a célcsoport tulajdonosával) lehet kifejezni.
   
  A feltételre adott válasz legegyszerűbb formája a kért művelet végrehajtásához szükséges engedélyek megadása. Az engedélyek megadása mellett más műveleteket is megadhat egy RMPR feltételre adott válaszként. A webkiszolgálói architektúrában ezek a műveletek munkafolyamatok formájában vannak meghatározva. Abban az esetben, ha egy adott RMPR feldolgozása történik, előfordulhat, hogy a rendszer nem rendelkezik elegendő információval az engedélyek megadásához. Ebben az esetben további hitelesítési és engedélyezési lépéseket is megadhat a RMPR, amelyek az adott kérelmet végző személyre vonatkoznak. Ha például engedélyt szeretne adni a kért művelet elvégzésére, szükség lehet a felhasználó manuális beavatkozására a művelet jóváhagyásához.
   
  Az alábbi ábra egy RMPR teljes architektúráját ismerteti:
   
  ![RMPR architektúra](media/mim-2016-sp1-terms/8f5054080d3f8bd9c73d93a5e3ed51f9.gif)
   
  Amikor új kérési objektumot hoznak létre a webalkalmazásban, a rendszer lekérdezi a konfigurált Készletátmenet a megfelelő objektumokhoz a kérés feltételeinek és a felügyeleti Készletátmenet konfigurált feltételek összehasonlításával. Ha a megfelelő Készletátmenet találhatók, azok az üzenetsor-kezelési kérelem objektumra lesznek alkalmazva. A következő ábra ezt a folyamatot ismerteti:
   
  ![Az egyező Készletátmenet találhatók és alkalmazhatók az üzenetsor-kezelési kérelem objektumra.](media/mim-2016-sp1-terms/65bdb65aaffa55a90707ff6f7b13632d.gif)
   
  A webkiszolgálói portálon explicit módon meg kell adni a műveletekre vonatkozó engedélyeket. Más szóval, hacsak egy RMPR nem biztosít, az erőforrásokon végrehajtott összes művelet megtagadva. Minden kérelem-objektumhoz legalább egy olyan RMPR szükséges, amely engedélyt ad a kért művelet végrehajtásához a célhelyen.

**kérelem objektum** : amikor egy felhasználó elvégez egy feladatot a rendszerkiszolgálói portálon vagy az Outlook rendszerhez készült felhasználói felületi beépülő modulban, a kérelem objektumként jelenik meg. A kérelem objektumai kényelmes jelentéskészítési mechanizmust jelentenek a rendszerben végzett tevékenységekhez.

   Minden kérelem-objektum rendelkezik ezekkel az összetevőkkel:
   
- Kérelmező: egy művelet végrehajtását kérő erőforrás.
- Művelet: az a művelet, amelyet a kérelmező végre szeretne hajtani.
- Cél: az erőforrás, amely a kért művelet célja.
   
  Logikailag a kérelem objektum a következő utasítás implementációja:
   
  `The requester attempts to perform the following operation on this target...`
      
  Az alábbi ábra egy kérelem objektum általános architektúráját ismerteti:
   
  ![Objektumarchitektúra kérése](media/mim-2016-sp1-terms/788e9b3a4fcddb78ff4a05dcb5e61192.gif)
   
  Minden kérelem objektumhoz tartozik egy Status tulajdonság, amely jelzi a feldolgozási állapotot. A kérelmek feldolgozásához manuális interakcióra lehet szükség a kérelem teljesítéséhez. Előfordulhat például, hogy egy csoport tulajdonosának manuálisan jóvá kell hagynia egy másik felhasználó kérését egy csoporthoz való csatlakozáshoz. A manuális interakció mellett azt is beállíthatja, hogy egy adott kérés automatikusan feldolgozzon az emberi beavatkozás szükségessége nélkül.
   
**kérelem-feldolgozási modell** : a (z) a (z)-beli kérelmek feldolgozási modellje három fő szakaszból áll:

- 1. fázis: hitelesítés
- 2. fázis: Engedélyezés
- 3. fázis: művelet
   
  A munkafolyamatok, amelyek mindegyike egy vagy több tevékenységet tartalmaz, csatolható mindegyik fázishoz, és egyetlen kérelem végrehajtásának kontextusában is futtatható. Egy kérés elindítható egyetlen felhasználói hívásból a webszolgáltatási végpontok egyikére, vagy egy olyan felhasználón keresztül, amely egy kérést hoz létre a webkiszolgálói portálon.
   
  Az alábbi ábrán a kérelmek feldolgozási összetevőinek kapcsolata látható:
   
  ![Kérelem-feldolgozási összetevők kapcsolata](media/mim-2016-sp1-terms/0e57445d8b9a3216c31add80eb26493f.gif)
   
  A kérelmek feldolgozása a következő sorrendben történik:
   
- Kérelem objektumának létrehozása: a (z) 2016-es webszolgáltatási végpontok egyikének hívása, vagy a webszolgáltatási portálon keresztül kezdeményezett kérelem miatt egy kérelem-objektum jön létre.
   
- MPR kiértékelése: a kérelmező jogosultságai ellenőrzik a művelet érvényességét, és elvégzik a megfelelő munkafolyamatok számítását. A rendszer minden MPR-objektumhoz ellenőrzi a kérelmet. Egy MPR való leképezéshez a kért művelethez tartozó MPR összes vonatkozó mezőjét meg kell egyeznie. Ebbe beletartozik a kérelmező, a művelet, a cél erőforrás és az attribútumok. Ha az összes ilyen feltétel, beleértve az érintett attribútumokat is, igaz a bejövő kérelem esetében, akkor a megfelelő MPR egyeztetve van a kérelemmel. A kérelemnek legalább egy olyan MPR le kell képeznie, amely az engedélyt a definíciójának részeként biztosítja. Ha ez igaz, a kérés a kérelmek feldolgozásának engedélyek ellenőrzési fázisán halad át. Ha ez nem igaz, a kérelem meghiúsul. A rendszer azt is meghatározza, hogy milyen átmenetek tartoznak a kérelembe, és megkeresi az összes kapcsolódó készlet átmenet-alapú MPR.
   
- Hitelesítés: a determinisztikus-alapú 2016-es hitelesítési munkafolyamatokat egy időben, nem pedig a kérelmező identitásának megerősítéséhez futtatják.
   
- Engedélyezés: a (z) 2016 rendszer megerősíti a kérelmező engedélyét a kért művelet végrehajtásához a kérelemben megadott erőforráson. Az összes függő engedélyezési munkafolyamat párhuzamosan fut, de a rendszer nem küldi el az összes munkafolyamatot, ha az összes munkafolyamat be lett töltve, és az összes sikeres művelet befejeződött.
   
- Feldolgozás: a (z) 2016 a kért műveletet hajtja végre a webalkalmazás-áruházban.
   
- Művelet: a 2016-es webalkalmazás végrehajtja a kért művelet miatt végrehajtandó folyamatokat. Az összes műveleti munkafolyamat párhuzamosan fut. Az olvasási műveletek nem rendelkeznek a feldolgozásra alkalmazott munkafolyamatokkal. Ez magában foglalja a RMPR konfigurált munkafolyamatokat, valamint a munkafolyamatokat a set Transition-based MPR.
   
  >[!NOTE]
  >A szinkronizálási fiók által kezdeményezett kérelmek megkerülik a rájuk vonatkozó összes hitelesítési és engedélyezési munkafolyamatot. A rendszer alkalmazza a kapcsolódó műveleti munkafolyamatokat.

**kérelmező** : annak a felhasználónak vagy szolgáltatásnak az identitása, aki kérelmet küldött a 2016-nek.

**kérelmező hatóköre** : olyan felhasználók konfigurált gyűjteménye, akik küldhetnek kérelmet. "Mindenki" lehet, vagy egy szűrő által definiált felhasználók egy adott halmaza.

**erőforrás** : egy adott erőforrástípus egy példánya a 2016-ben. Minden erőforrást egyedileg azonosít a ObjectID (ResourceID) attribútuma.

**erőforrás-vezérlő megjelenítési konfigurációja (RCDC)** : a RCDCs olyan konfigurációs erőforrások, amelyek segítségével az erőforrás-vezérlő (RC) felhasználói felülete beállítható egy adott erőforrástípus létrehozásához a 2016-es webszolgáltatásban.

**erőforrás jelenlegi készlete** : a felügyeleti házirend szabályainak (MPR) a feltétel definíciójának része. A cél erőforrásainak gyűjteménye a kérelem fogadásakor. Az olvasási, törlési és módosítási műveletek típusára vonatkozik.

**erőforrás végső készlete** : a feltétel definíciójának egy része a felügyeleti házirend szabályaiban. A cél erőforrásainak gyűjteménye a kérelem feldolgozása után. Csak a műveletek létrehozási és módosítási típusaira vonatkozik.

**erőforrás-hierarchia** : egy címtárszolgáltatás esetében az erőforrás-bejegyzés hierarchiája egy névhasználati környezet és az adott erőforrás-bejegyzés alapja közötti címtár-bejegyzések gyűjteménye.

**erőforrás hatóköre** : azon erőforrások összessége, amelyekről kérést lehet küldeni.

**erőforrástípus** : egy olyan séma része, amely meghatározza egy erőforrás megjelenítését a webszolgáltatási 2016-ben.

**erőforrástípus-hozzárendelés** : egy olyan erőforrástípus közötti kapcsolat, amely az erőforrást a fakiszolgálói 2016-ben és egy, a metaverse-ban az erőforrást képviselő erőforrás-osztályban jelöli.

**szerepkör** : a hozzáférési jogosultságok kezeléséhez használt, szervezet által hozzárendelt rendszerbiztonsági tag.

**szabályok bővítmény** : a szabályok kiterjesztése egy dinamikus csatolású függvénytár (. dll), amely az adatkezelési szabályok meghatározott készletét tartalmazza. A funkciók kiterjesztéséhez a szabályok kiterjesztéseit használhatja a szinkronizálások során. A szabályok bővítmény használatával például összekapcsolhatja a két forrás attribútumainak adatait, és átadhatja őket egy cél attribútumba (például `sn` és `givenName` into `displayName` ).

**futtatási előzmények** : olyan statisztikai készlet, amely egy felügyeleti ügynök egyetlen futtatásának eredményeit jeleníti meg.

**futtatási profil** : a futtatási profil olyan lépéseket mutat be, amelyek meghatározzák, hogyan futtathat egy felügyeleti ügynököt és konfigurációs beállításokat, amely meghatározza, hogyan fut a felügyeleti ügynök. A felügyeleti ügynök több futtatási profillal is rendelkezhet, amelyeket a felügyeleti ügynök tárol. A futtatási profil legalább egy futtatási profil lépésből áll.
<br/>

## <a name="s"></a>S

**keresési mappa** : Tekintse meg a "jóváhagyások keresési mappa" bejegyzést.

**keresési hatókör** : egy adott keresési környezet tulajdonságait adja meg, amelyet a felhasználó a webszolgáltatási 2016-portálon végezhet. Egy felhasználó például kiválaszthat egy keresési hatókört a legördülő listából a "minden felhasználó", "minden terjesztési lista", "a saját függőben lévő jóváhagyások" listáról, és a keresési eredményeket a felhasználó által megadott keresési kifejezéseken felül a feltételeknek megfelelő elemekre korlátozhatja.

**biztonsági leíró** : egy olyan struktúra és társított adat, amely tartalmazza a biztonságos erőforrás biztonsági információit. A biztonsági leíró azonosítja az erőforrás tulajdonosát és elsődleges csoportját. Tartalmazhat olyan DACL-t is, amely az erőforráshoz való hozzáférést vezérli, valamint egy, az erőforrás elérésére irányuló kísérletek naplózását vezérlő hozzáférés-vezérlési listákat.

**rendszerbiztonsági tag** : biztonsági felügyelethez használt identitás, például egy olyan felhasználói fiók, aki hitelesítheti magát a szolgáltatásban.

**biztonsági jogkivonat** : egy protokoll-elem, amely a hitelesítési és engedélyezési adatokat továbbítja a hitelesítő adatok alapján. A webszolgáltatások protokolljaiban egy biztonsági jogkivonat XML-elemként jelenik meg egy SOAP-fejlécben a WS-Security által definiált módon.

**biztonsági jogkivonat szolgáltatás** : a WS-Trust protokollt implementáló szolgáltatás, amely a biztonsági jogkivonatok cseréje alapján kezeli az ügyfelek és a szolgáltatások közötti megbízhatósági kapcsolatot.

**szekvenciális munkafolyamat** : a 2016-es teljes munkafolyamatok a Windows Workflow Foundation szekvenciális munkafolyamatból származnak. Egy szekvenciális sorrendben több munkafolyamatot tartalmaz.

**szolgáltatásfiók** : egy Windows-szolgáltatáshoz hozzárendelt Windows-fiók, amelyet a felhasználó nem használ a számítógép-rendszerbe való bejelentkezéshez. A webszolgáltatások rendszerfiókját jelöli.

**Set** : erőforrások névvel ellátott gyűjteménye. Jellemzően a szabályok alapján rendezi az erőforrásokat. Egy készlet tagsága manuálisan – felügyelt vagy feltételes. Ez azt jelenti, hogy manuálisan adhat hozzá erőforrásokat egy készlethez, és meghatározhatja azokat a feltételeket, amelyek automatikusan hozzáadnak erőforrásokat egy készlethez egy szűrő utasítás alapján. Ha egy erőforrás megfelel a szűrési feltételeknek, a rendszer automatikusan hozzáadja a kapcsolódó készlethez.

Az **áttérési felügyeleti házirend szabályának (TMPR) beállítása** : egy készlet tagságának változásaira alkalmazott felügyeleti házirend-szabály. Állítsa be a Tmpr műveleti munkafolyamatokat, ha az objektum a MPR egy megadott készletére vagy onnan való áttérésre van állítva.

   Kétféle Tmpr létezik:
   
- Áttérés a-ben: egy erőforrás az áttérési készlet tagjává válik.
- Áttérés: egy erőforrás elhagyja az átmeneti készletet.
   
   >[!NOTE]
   >Egy átmeneti készlet törlésekor a rendszer a törlést kiváltási eseményként kezeli az érintett objektumokra vonatkozóan.
      
  A válasz az alkalmazott állapot változására reagál. A kapcsolódó MPR meghívásakor a feltétel már alkalmazva van. Ez azt jelenti, hogy az érintett erőforrások már átkerültek egy átmeneti készletbe vagy onnan. A Tmpr esetében a válasz célja nem a kért műveletre való reagálás, hanem az alkalmazott műveletre adott válasz meghatározása. Más szóval, ha a beállított átmenet-alapú MPR, nem számít, hogy milyen állapotot értek el. Az állapot változásának következményei a mérvadóak.
   
  Amikor beállítja a set Transition-based MPR a webalkalmazásban, a következő három beállítást kell megadnia:
   
- Átmeneti készlet
- Átmenet típusa
- Szabályzat-munkafolyamatok
   
  A házirend-munkafolyamatok azokat a folyamatokat definiálják, amelyeket az állapot változásakor kell meghívni. Az állapot-alapú MPR leggyakoribb felhasználási esetei a jogosultságok megadása vagy visszavonása, valamint a külső adatforrások kiépítése és megszüntetése.
   
  Az alábbi ábra egy beállított átmenet-alapú MPR teljes architektúráját ismerteti:

  ![TMPR-architektúra beállítása](media/mim-2016-sp1-terms/e12154d35b48cec676f160930ff33662.gif)
  
  Az áttérésen alapuló MPR beállítása kérések szerint történik. Ha egy RMPR feldolgozza és jóváhagyja a kérelmet, a fakiszolgálói szolgáltatás azt is meghatározza, hogy egy jóváhagyott kérelem állapot-átállást eredményez-e, valamint hogy létezik-e olyan állapot-átállási MPR, amely kezeli az állapot változását.
  
  Az alábbi ábra egy kérelem és egy beállított átmenet-alapú MPR közötti kapcsolatot ismerteti:
  
  ![Egy kérelem és egy beállított TMPR közötti kapcsolat](media/mim-2016-sp1-terms/426be3a3a5380720fa1eaa3e7df360df.gif)

**SID** : egy felhasználói fiók, csoportfiók vagy bejelentkezési munkamenet azonosítására szolgáló egyedi érték.

**SOAP** : a szoftver összetevői közötti strukturált információk cseréjére szolgáló protokoll.

**szinkronizálás** : a kiválasztott információk több adatforrásban való megőrzésének folyamata a szerződésben. A szinkronizálás kizárólag a következő kereteken belüli objektumok műveleteire vonatkozik. A szinkronizálás lehet egy művelet a teljes adathalmazon, amelynek a felügyeleti ügynökön vagy Delta műveleten van definiálva, mint a legutóbbi ismert művelet óta történt változások. A szinkronizálási futtatási profil lépés a bejövő és kimenő szinkronizálási folyamatokat határozza meg.

   A szinkronizálási futtatási profil lépéseinek két altípusa van:
   
- Különbözeti szinkronizálás
- Teljes szinkronizálás
   
  A különbözeti szinkronizálás során a rendszer csak az importált objektumokat dolgozza fel, amelyek az importálás függőben lévőként megjelölt átmeneti objektumok. Ez a futtatási profil csak azokat az objektumokat dolgozza fel, amelyek módosításai függőben vannak, de az előző szinkronizálás futtatása során nem voltak feldolgozva.
   
  A különbözeti szinkronizálás két előre definiált futtatási profilban használatos, és mindegyikben némileg másképp viselkedik. Az első futtatási profil a különbözeti szinkronizálás, amely nem végez importálást a csatlakoztatott forrásokból, de az összekötő terület összes objektuma ki lesz értékelve, és a függőben lévő módosításokkal rendelkező objektumok feldolgozása megtörténik. A második futtatási profil a különbözeti Importálás és a különbözeti szinkronizálás együttese. Ez a futtatási profil csak azokat az objektumokat és attribútumokat importálja a csatlakoztatott adatforrásból, amelyek értékei módosultak a felügyeleti ügynök legutóbbi futtatása óta. A felügyeleti ügynök szabályait a rendszer csak azokra az objektumokra alkalmazza újra, amelyeken függőben lévő változások történtek a különbözeti importálásból. A rendszer nem értékeli ki azokat az objektumokat, amelyek nem változnak az adott különbözeti importálásból.
   
  A teljes szinkronizálás során a rendszer kiértékeli és alkalmazza a szinkronizálási szabályokat az összekötői térben lévő összes átmeneti objektumra. Teljes szinkronizálást kell kezdeményezni, amikor egy adott környezet szabályaira vonatkozó módosításokat alkalmaztak. Az összekötő terület objektumainak számától függően ez lehet egy idő-és erőforrás-igényes művelet, ezért a szinkronizálási szabályok gyakori módosításait el kell kerülni az éles környezetben.

**szinkronizációs szűrő** : egy szűrő, amely megakadályozza, hogy a metaverse erőforrásai át legyenek ruházva a webkiszolgáló 2016-adatbázisba.

**szinkronizálási szabály** : erőforrás-információk átvitelére szolgáló szabály a rendszerállapot-kiszolgáló (például a bemásolási modul szinkronizálási motorja) és a csatlakoztatott külső rendszer között.
<br/>

## <a name="t"></a>T

**időbeli szabályzat** : a beállított átmeneti MPR, amely egy időbeli készlethez van kötve. A házirendet az időpontra alkalmazza a rendszer, mivel az objektumok a készletbe való áttéréskor és onnan kívülre kerülnek, az időbeli készlet definíciója alapján.

**Időbeli beállítás** : egy olyan set objektum típusa, amely relatív dátumokon alapul. Az időbeli készletek olyan mechanizmust biztosítanak, amely teljes mértékben automatizálhatja a készletbe való áttérés folyamatát az idő múlása alapján. Egy időbeli készlet például meghatározható minden olyan csoport esetében, amely egy héttel a mai naptól lejár. A rendszer automatikusan kiértékeli az objektumokat a rendszeren, és hozzáadja őket a készlethez napi rendszerességgel. Az egyéb példák lehetővé teszik az időhivatkozások dinamikus definícióit, például a "x nap a mai naptól" alapuló szűrőt.

**időzített esemény** : egy konfigurált időintervallum lejárta után megjelenő átmeneti esemény, vagy egy adott dátum és idő elérésekor.

**időtúllépés** : az az időszak, amelyben a (z) 2016-es webszolgáltatásra vár jóváhagyási válaszokat, amíg meg nem történik a tevékenység.

**átváltási készlet** : egy beállított áttérési felügyeleti házirend-szabály definíciójában használt készlet. A rendszer alkalmazza a házirendet a set tagság változásaira, amelyek a TMPR konfigurációjától függően a készletbe beérkező vagy onnan távozó objektumok lehetnek.
<br/>

## <a name="u"></a>U

**zárolt csoport** : az a csoport, amelyben a csoport tagsága a csoport tulajdonosától eltérő felhasználók által módosítható.

**univerzális csoport** : az univerzális hatókörű csoportok egy Active Directory csoport, amely egy adott erdő tagjait tartalmazhatja. Az univerzális csoportok bármilyen tartományhoz vagy erdőhöz hozzárendelhetők. A terjesztési listák jellemzően univerzális hatókörrel rendelkeznek.Az univerzális hatókörű biztonsági csoportok az azonos erdőben lévő erőforrásokat is biztonságossá tehetik.

**frissítési kérelem** : egy erőforrás attribútumainak módosításához szükséges kérelem.

**használati kulcsszó** : a használati kulcsszó segítségével határozható meg, hogy mely keresési hatókörök jelenjenek meg egy adott laphoz a portál felhasználói felületén. A felhasználói felület minden listanézet lapja nulla vagy több használati kulcsszót határoz meg, az adott oldal felhasználói felülete pedig tartalmazza a megfelelő kulcsszavakat tartalmazó összes keresési hatókört. Keresési hatókörök létrehozásakor az ügyfelek a keresési hatókörben nulla vagy több kulcsszót is meghatározhatnak, így testre szabhatja, hogy mely keresési hatókörök jelenjenek meg a felhasználói felületen lévő adott oldalon. A rendszer azt is felhasználja, hogy meghatározza, melyik Kezdőlap erőforrás és navigációs sáv erőforrás jelenik meg a felhasználók számára. A séma felügyeletében is használatban van, hogy megvédje és felcímkézje a Rendszerfelügyeleti webszolgáltatások különböző összetevői által igényelt séma-elemeket.
<br/>

## <a name="w"></a>W

**webportál** : egy, a webkiszolgáló, például az IIS összetevője által megvalósított felhasználói felület.

**webszolgáltatás** : egy HTTP-alapú protokoll használatával megvalósított szolgáltatás protokoll-illesztőfelülete.

**munkafolyamat** : a munkafolyamatok olyan elemi egységek összessége, amelyek egy valós folyamatot leíró modellként vannak tárolva. A munkafolyamatok lehetővé teszik a munkaelemek sorrendje és a függő kapcsolatok leírását. Ez a munka a modellből az elejétől a végéig halad, és a tevékenységeket személyek vagy rendszerfüggvények is elvégezték. Más szóval, ha egy kérelemre adott válasz összetett feldolgozást igényel, a lépések egy munkafolyamat-objektumba vannak beágyazva. A munkafolyamatok opcionális összetevők, és szorosan kötődnek a MPR. A munkafolyamatok határozzák meg a MPR feldolgozása során felmerülő tevékenységeket vagy tevékenységeket. A webszolgáltatása számos alapértelmezett munkafolyamatot telepít, amelyek vagy egy egyéni munkafolyamat alapjaként használhatók.

   A munkafolyamat-tevékenységekre vonatkozó példák a következők:

- Automatikus e-mail-üzenet küldése a jóváhagyás igényléséhez.
- A felhasználók által az Egyéni keresés során megtekinthető attribútumok korlátozása.
- Új csoport érvényesítése AD DS vagy a webszolgáltatási irányelvek alapján.
- Az objektum hozzáadása vagy eltávolítása egy szinkronizálási szabály hatókörében.
   
  Ahhoz, hogy a környezet összes feldolgozási követelményét kezelni lehessen, a következő három típusú munkafolyamatot határozza meg:
   
- Hitelesítés: további felhasználói identitás-ellenőrzést hajt végre a kérelem folytatása előtt
- Engedélyezés: a kérelmeket olyan tevékenységek sorozatából hajtja-e meg, mint például a szükséges külső jóváhagyás beszerzése a kérelem feldolgozása előtt.
- Művelet: az eredeti kérelem sikeres befejeződése után dolgozza fel az összes további tevékenységet.
   
  Ez a három munkafolyamat a kérelem feldolgozási modelljének részét képezi.

**munkafolyamat-definíció** : a munkafolyamat-definíciót a Windows WORKFLOW Foundation (WF) által definiált xoml formátumban tárolja a rendszer. Ez határozza meg a tevékenységeket, a tevékenységek paramétereit, valamint a futtatandó sorrendet.

**Munkafolyamat-tervező** : a munkafolyamatok építésére szolgáló tervezési idő.

**munkafolyamat-gazdagép** : az a kiszolgáló-összetevő, amely a munkafolyamatok futtatásával foglalkozik. A (z) 2016-es webszolgáltatásban 2016 a webalkalmazás-kiszolgáló a munkafolyamatok gazdagépe.

**munkafolyamat-példány** : egy munkafolyamat-definíció futó példánya egy kérelem hatására.

**munkafolyamat-kezelés** : a munkafolyamatok tervezéséhez, valamint azok kezeléséhez és felügyeletéhez használható, a 2016-es felügyeleti webszolgáltatások funkciója. A munkafolyamat-kezelés a Munkafolyamat-tervező, a kérelmek kezelése és a munkafolyamat-gazdagépből áll.
