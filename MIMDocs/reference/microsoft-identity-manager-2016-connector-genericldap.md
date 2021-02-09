---
title: Általános LDAP-összekötő | Microsoft Docs
description: Ez a cikk bemutatja, hogyan konfigurálhatja a Microsoft általános LDAP-összekötőjét.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.reviewer: davidste
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 06/26/2018
ms.author: billmath
ms.openlocfilehash: 5b19b4fd9d45797fcc6b02091386a27aec3c0abf
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835723"
---
# <a name="generic-ldap-connector-technical-reference"></a>Általános LDAP-összekötő – műszaki útmutató
Ez a cikk az általános LDAP-összekötőt ismerteti. A cikk a következő termékekre vonatkozik:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * A gyorsjavítási 4.1.3671.0 vagy újabb [KB3092178](https://support.microsoft.com/kb/3092178)kell használnia.

A MIM2016 és a FIM2010R2 esetében az összekötő letölthető a [Microsoft letöltőközpontból](https://go.microsoft.com/fwlink/?LinkId=717495).

Az IETF RFC-k használatára való hivatkozáskor a dokumentum a következő formátumot használja: (RFC [RFC Number]/[szakasz az RFC-dokumentumban]), például (RFC 4512/4.3).
További információkat itt talál: [https://tools.ietf.org/](https://tools.ietf.org/) . A bal oldali panelen adjon meg egy RFC-számot a **doc beolvasás** párbeszédpanelen, és ellenőrizze, hogy érvényes-e.

## <a name="overview-of-the-generic-ldap-connector"></a>Az általános LDAP-összekötő áttekintése
Az általános LDAP-összekötő lehetővé teszi a szinkronizációs szolgáltatás integrálását egy LDAP v3-kiszolgálóval.

Bizonyos műveletek és sémák, például a különbözeti importálás végrehajtásához szükséges elemek nincsenek megadva az IETF RFC-ben. Ezen műveletek esetében csak a explicit módon megadott LDAP-címtárak támogatottak.

A címtárakhoz való csatlakozáshoz a root/admin fiókot használjuk.  Ha más fiókot szeretne használni a részletesebb engedélyek alkalmazásához, érdemes áttekintenie az LDAP-címtár csapatát.

Magas szintű perspektívából a következő funkciókat támogatja az összekötő jelenlegi kiadása:

| Szolgáltatás | Támogatás |
| --- | --- |
| Csatlakoztatott adatforrás |Az összekötő minden LDAP v3-kiszolgáló (RFC 4510-kompatibilis) esetében támogatott. A következővel lett tesztelve: <li>Microsoft Active Directory Lightweight Directory-szolgáltatások (AD LDS)</li><li>Microsoft Active Directory globális katalógus (AD GC)</li><li>389 Directory-kiszolgáló</li><li>Apache Directory-kiszolgáló</li><li>IBM Tivoli DS</li><li>Isode könyvtár</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>DJ megnyitása</li><li>A DS megnyitása</li><li>Az LDAP megnyitása (openldap.org)</li><li>Oracle (korábban Sun) Directory Server Enterprise Edition</li><li>RadiantOne Virtual Directory-kiszolgáló (VDS)</li><li>Sun One Directory-kiszolgáló</li><li>Microsoft Active Directory Domain Services (AD DS)</li><ul><li>A legtöbb esetben a beépített Active Directory-összekötőt kell használnia, mivel előfordulhat, hogy egyes funkciók nem működnek</li></ul>**A jelentős ismert könyvtárak vagy szolgáltatások nem támogatottak:**<li>Microsoft Active Directory Domain Services (AD DS)<ul><li>Jelszó-módosítási értesítési szolgáltatás (PCN)</li><li>Exchange-kiépítés</li><li>Aktív szinkronizáló eszközök törlése</li><li>A nTDescurityDescriptor támogatása</li></ul></li><li>Oracle Internet Directory (OID)</li> |
| Forgatókönyvek |<li>Objektumok életciklusának kezelése</li><li>Csoportkezelés</li><li>Jelszókezelés</li> |
| Üzemeltetés |Az összes LDAP-címtárban a következő műveletek támogatottak: <li>Teljes importálás</li><li>Exportálás</li>A következő műveletek csak a megadott címtárakban támogatottak:<li>Különbözeti importálás</li><li>Jelszó beállítása, jelszó módosítása</li> |
| Séma |<li>A rendszer az LDAP-sémából észlelt sémát (RFC3673 és RFC4512/4.2)</li><li>Támogatja a strukturális osztályokat, az AUX-osztályokat és a extensibleObject objektumosztály (RFC4512/4.3)</li> |

### <a name="delta-import-and-password-management-support"></a>Különbözeti Importálás és jelszavas kezelés támogatása
Támogatott könyvtárak a különbözeti importáláshoz és a jelszavak kezeléséhez:

* Microsoft Active Directory Lightweight Directory-szolgáltatások (AD LDS)
  * A különbözeti importálás összes műveletét támogatja
  * A jelszó beállítása
* Microsoft Active Directory globális katalógus (AD GC)
  * A különbözeti importálás összes műveletét támogatja
  * A jelszó beállítása
* 389 Directory-kiszolgáló
  * A különbözeti importálás összes műveletét támogatja
  * Támogatja a jelszó beállítása és a jelszó módosítása
* Apache Directory-kiszolgáló
  * A nem támogatja a különbözeti importálást, mert ez a könyvtár nem rendelkezik állandó változásnapló-naplóval
  * A jelszó beállítása
* IBM Tivoli DS
  * A különbözeti importálás összes műveletét támogatja
  * Támogatja a jelszó beállítása és a jelszó módosítása
* Isode könyvtár
  * A különbözeti importálás összes műveletét támogatja
  * Támogatja a jelszó beállítása és a jelszó módosítása
* Novell eDirectory és NetIQ eDirectory
  * Támogatja a különbözeti Importálás Hozzáadási, frissítési és átnevezési műveleteit.
  * A nem támogatja a különbözeti importálás törlési műveleteit
  * Támogatja a jelszó beállítása és a jelszó módosítása
* DJ megnyitása
  * A különbözeti importálás összes műveletét támogatja
  * Támogatja a jelszó beállítása és a jelszó módosítása
* A DS megnyitása
  * A különbözeti importálás összes műveletét támogatja
  * Támogatja a jelszó beállítása és a jelszó módosítása
* Az LDAP megnyitása (openldap.org)
  * A különbözeti importálás összes műveletét támogatja
  * A jelszó beállítása
  * Nem támogatja a jelszó módosítását
* Oracle (korábban Sun) Directory Server Enterprise Edition
  * A különbözeti importálás összes műveletét támogatja
  * Támogatja a jelszó beállítása és a jelszó módosítása
* RadiantOne Virtual Directory-kiszolgáló (VDS)
  * A-es vagy újabb verziójúnak kell lennie
  * A különbözeti importálás összes műveletét támogatja
  * Támogatja a jelszó beállítása és a jelszó módosítása
* Sun One Directory-kiszolgáló
  * A különbözeti importálás összes műveletét támogatja
  * Támogatja a jelszó beállítása és a jelszó módosítása

### <a name="prerequisites"></a>Előfeltételek
Az összekötő használata előtt győződjön meg arról, hogy rendelkezik az alábbiakkal a szinkronizálási kiszolgálón:

* Microsoft .NET 4.5.2-keretrendszer vagy újabb

### <a name="detecting-the-ldap-server"></a>Az LDAP-kiszolgáló észlelése
Az összekötő az LDAP-kiszolgáló észlelésére és azonosítására szolgáló különféle technikákra támaszkodik. Az összekötő a legfelső szintű DSE, a szállító nevét/verzióját használja, és megvizsgálja a sémát, hogy megkeresse az egyes LDAP-kiszolgálókon ismert egyedi objektumokat és attribútumokat. Ezek az adatok, ha találhatók, az összekötő konfigurációs beállításainak előzetes kitöltésére szolgálnak.

### <a name="connected-data-source-permissions"></a>Csatlakoztatott adatforrás engedélyei
Az importálási és exportálási műveleteknek a csatlakoztatott címtárban lévő objektumokon való végrehajtásához az összekötő fióknak megfelelő engedélyekkel kell rendelkeznie. Az összekötőnek írási engedéllyel kell rendelkeznie az exportáláshoz, és olvasási engedéllyel kell rendelkeznie az importáláshoz. Az engedélyek konfigurálása a cél könyvtár felügyeleti tapasztalatain belül történik.

### <a name="ports-and-protocols"></a>Portok és protokollok
Az összekötő a konfigurációban megadott portszámot használja, amely alapértelmezés szerint 389 az LDAP és a 636 esetében az LDAPs-hez.

Az LDAPs esetében SSL 3,0 vagy TLS protokollt kell használnia. Az SSL 2,0 nem támogatott, és nem aktiválható.

### <a name="required-controls-and-features"></a>Szükséges vezérlők és szolgáltatások
Az összekötő megfelelő működéséhez az alábbi LDAP-vezérlőknek/szolgáltatásoknak elérhetőnek kell lenniük az LDAP-kiszolgálón:  
`1.3.6.1.4.1.4203.1.5.3` Igaz/hamis szűrők

Az igaz/hamis szűrőt gyakran nem az LDAP-címtárak támogatják, és a **kötelező szolgáltatások nem találhatók** a **globális oldalon** . LDAP-lekérdezések létrehozásához **vagy** szűrőkhöz használható, például több objektumtípus importálásakor. Ha egynél több objektumtípust is importálhat, akkor az LDAP-kiszolgáló támogatja ezt a funkciót.

Ha olyan könyvtárat használ, amelyben egy egyedi azonosító található, akkor a következőnek is elérhetőnek kell lennie (további információért lásd a [horgonyok konfigurálása](#configure-anchors) szakaszt):  
`1.3.6.1.4.1.4203.1.5.1` Minden operatív attribútum

Ha a címtár több objektummal rendelkezik, mint ami elfér a címtárban, akkor javasolt a lapozás használata. A lapozás működéséhez a következő lehetőségek egyikére van szükség:

**1. lehetőség:**  
`1.2.840.113556.1.4.319` pagedResultsControl

**2. lehetőség:**  
`2.16.840.1.113730.3.4.9` VLVControl  
`1.2.840.113556.1.4.473` SortControl

Ha mindkét beállítás engedélyezve van az összekötő konfigurációjában, a rendszer a pagedResultsControl használja.

`1.2.840.113556.1.4.417` ShowDeletedControl

A ShowDeletedControl csak a USNChanged különbözeti importálási metódussal használható, hogy megjelenjenek a törölt objektumok.

Az összekötő megpróbálja felderíteni a kiszolgálón lévő beállításokat. Ha a beállítások nem észlelhetők, egy figyelmeztetés jelenik meg a globális oldalon az összekötő tulajdonságaiban. Nem minden LDAP-kiszolgáló jeleníti meg az összes általa támogatott vezérlőt/funkciót, még ha ez a figyelmeztetés is fennáll, akkor az összekötő problémák nélkül működhet.

### <a name="delta-import"></a>Különbözeti importálás
A különbözeti importálás csak akkor érhető el, ha a rendszer egy támogatási könyvtárat észlelt. Jelenleg a következő módszereket használják:

* LDAP-Accesslog. Lásd: [ http://www.openldap.org/doc/admin24/overlays.html#Access naplózás](http://www.openldap.org/doc/admin24/overlays.html#Access%20Logging)
* LDAP-changelog. Lásd [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* Időbélyeg. A Novell/NetIQ eDirectory esetében az összekötő az utolsó dátum/idő használatával hozza létre a létrehozott és frissített objektumokat. A Novell/NetIQ eDirectory nem biztosít megfelelő módszert a törölt objektumok lekéréséhez. Ez a beállítás akkor is használható, ha az LDAP-kiszolgálón nincs más különbözeti importálási módszer. Ez a beállítás nem tudja importálni a törölt objektumokat.
* USNChanged. Lásd [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Nem támogatott
A következő LDAP-funkciók nem támogatottak:

* LDAP-hivatkozások kiszolgálók között (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Új összekötő létrehozása
Általános LDAP-összekötő létrehozásához a **szinkronizációs szolgáltatásban** válassza a **felügyeleti ügynök** lehetőséget, és **hozzon létre**. Válassza ki az **általános LDAP (Microsoft)** összekötőt.

![CreateConnector](./media/microsoft-identity-manager-2016-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Kapcsolatok
A kapcsolat lapon meg kell adnia a gazdagép, a port és a kötési adatokat. Attól függően, hogy melyik kötés van kiválasztva, a következő részben további információkat is megadhat.

![Kapcsolatok](./media/microsoft-identity-manager-2016-connector-genericldap/connectivity.png)

* A kapcsolat időtúllépési beállítása csak a kiszolgálóhoz való első kapcsolódáskor használatos a séma észlelése során.
* Ha a kötés névtelen, akkor sem a Felhasználónév, sem a jelszó, sem a tanúsítvány nincs használatban.
* Egyéb kötések esetén adja meg az adatokat a Felhasználónév/jelszó mezőben, vagy válasszon ki egy tanúsítványt.
* Ha Kerberost használ a hitelesítéshez, adja meg a felhasználó tartományát/tartományát is.

Az **attribútum aliasa** szövegmező a sémában a RFC4522 szintaxissal meghatározott attribútumok esetében használatos. Ezek az attribútumok nem észlelhetők a séma észlelése során, és az összekötőnek segítségre van szüksége az attribútumok azonosításához. Például a következőt kell megadnia az attribútum-aliasok mezőben, hogy helyesen azonosítsák a userCertificate attribútumot bináris attribútumként:

`userCertificate;binary`

A következő példa a konfiguráció megjelenését szemlélteti:

![Kapcsolatok](./media/microsoft-identity-manager-2016-connector-genericldap/connectivityattributes.png)

Jelölje be a **műveleti attribútumok belefoglalása a sémába** jelölőnégyzetet, hogy a kiszolgáló által létrehozott attribútumokat is tartalmazza. Ezek olyan attribútumokat tartalmaznak, mint az objektum létrehozásakor és a legutóbbi frissítés ideje.

Jelölje be **az bővíthető attribútumok belefoglalása a sémában** jelölőnégyzetet, ha a bővíthető OBJEKTUMOKAT (RFC4512/4.3) használják, és ez a beállítás lehetővé teszi, hogy minden attribútum minden objektumon használható legyen. Ha ezt a beállítást választja, a séma nagyon nagy méretűvé válik, így ha a csatlakoztatott könyvtár nem használja ezt a funkciót, a javaslat nem jelöli ki a beállítást.

### <a name="global-parameters"></a>Globális paraméterek
A globális paraméterek lapon konfigurálja a DN-t a különbözeti változási naplóba és további LDAP-funkciókra. A lap előre fel van töltve az LDAP-kiszolgáló által biztosított információkkal.

![Kapcsolatok](./media/microsoft-identity-manager-2016-connector-genericldap/globalparameters.png)

A felső szakasz a kiszolgáló által biztosított információkat (például a kiszolgáló nevét) jeleníti meg. Az összekötő azt is ellenőrzi, hogy a kötelező vezérlők szerepelnek-e a gyökérszintű DSE. Ha ezek a vezérlők nem szerepelnek a felsorolásban, a rendszer figyelmeztetést jelenít meg. Egyes LDAP-címtárak nem listázza a gyökérszintű DSE összes funkcióját, és lehetséges, hogy az összekötő probléma nélkül működik, még akkor is, ha van figyelmeztetés.

A **támogatott vezérlők** jelölőnégyzetekkel szabályozhatja az egyes műveletek viselkedését:

* Ha a fa törlése elem van kiválasztva, egy hierarchia törölve lett egy LDAP-hívással. Ha nincs kiválasztva a Tree DELETE, az összekötő szükség esetén rekurzív törlést végez.
* Ha a lapozható eredmények ki vannak választva, az összekötő egy lapozható importálást végez a futtatási lépésekben megadott mérettel.
* A VLVControl és a SortControl a pagedResultsControl alternatívája az LDAP-címtárból származó adatok olvasására.
* Ha mindhárom lehetőség (pagedResultsControl, VLVControl és SortControl) nincs kiválasztva, akkor az összekötő egyetlen műveletbe importálja az összes objektumot, ami sikertelen lehet, ha nagyméretű könyvtár.
* A ShowDeletedControl csak akkor használatos, ha a különbözeti importálási módszer a USNChanged.

A change log DN a különbözeti változási napló által használt névhasználati környezet, például: **CN = changelog**. Ezt az értéket meg kell adni, hogy el lehessen végezni a különbözeti importálást.

Az alábbi lista az alapértelmezett változási napló DNs-listáját tartalmazza:

| Címtár | Különbözeti változási napló |
| --- | --- |
| Microsoft AD LDS és AD GC |Automatikusan észlelhető. USNChanged. |
| Apache Directory-kiszolgáló |Nem érhető el. |
| Címtár 389 |Változási napló. Használandó alapértelmezett érték: **CN = changelog** |
| IBM Tivoli DS |Változási napló. Használandó alapértelmezett érték: **CN = changelog** |
| Isode könyvtár |Változási napló. Használandó alapértelmezett érték: **CN = changelog** |
| Novell/NetIQ eDirectory |Nem érhető el. Időbélyeg. Az összekötő az utolsó frissítés dátumát és idejét használja a rekordok hozzáadásához és frissítéséhez. |
| DJ/DS megnyitása |Változási napló.  Használandó alapértelmezett érték: **CN = changelog** |
| LDAP megnyitása |Hozzáférési napló. A használandó alapértelmezett érték: **CN = accesslog** |
| Oracle DSEE |Változási napló. Használandó alapértelmezett érték: **CN = changelog** |
| RadiantOne VDS |Virtuális könyvtár. A VDS-hez csatlakozó címtártól függ. |
| Sun One Directory-kiszolgáló |Változási napló. Használandó alapértelmezett érték: **CN = changelog** |

A Password attribútum annak az attribútumnak a neve, amelyet az összekötőnek használnia kell a jelszó beállításához a jelszó módosítása és a jelszó beállítása műveletekben.
Ez az érték alapértelmezés szerint **userPassword** van beállítva, de egy adott LDAP-rendszer esetén módosítható.

A további partíciók listában további névterek is hozzáadhatók, amelyeket a rendszer nem észlel automatikusan. Ez a beállítás például akkor használható, ha több kiszolgáló is létrehoz egy logikai fürtöt, és mindegyiket egyszerre kell importálni. Ahogy Active Directory több tartománya is lehet egy erdőben, de az összes tartomány egy sémával rendelkezik, ugyanezt szimulálhatja, ha a további névtereket is megadja ebben a mezőben. Az egyes névterek különböző kiszolgálókról importálhatók, és a partíciók és hierarchiák konfigurálása lapon további konfigurálást végeznek. Új sor beszerzéséhez használja a CTRL + ENTER billentyűkombinációt.

### <a name="configure-provisioning-hierarchy"></a>Kiépítési hierarchia konfigurálása
Ez a lap a DN összetevő, például a szervezeti egység (OU) leképezésére szolgál a kiépíteni kívánt objektumtípus számára, például organizationalUnit.

![Kiépítési hierarchia](./media/microsoft-identity-manager-2016-connector-genericldap/provisioninghierarchy.png)

A kiépítési hierarchia konfigurálásával beállíthatja, hogy az összekötő automatikusan hozzon létre egy struktúrát, ha szükséges. Ha például van egy névtér DC = contoso, DC = com, és egy új objektum CN = Joe, OU = Seattle, c = US, DC = contoso, DC = com kiépítve, akkor az összekötő létrehozhat egy ország típusú objektumot a US és a Seattle organizationalUnit, ha azok még nem szerepelnek a címtárban.

### <a name="configure-partitions-and-hierarchies"></a>Partíciók és hierarchiák konfigurálása
A partíciók és hierarchiák lapon jelölje ki az importálni és exportálni kívánt objektumokat tartalmazó összes névteret.

![Partíciók](./media/microsoft-identity-manager-2016-connector-genericldap/partitions.png)

Az egyes névterekhez olyan csatlakozási beállításokat is konfigurálhat, amelyek felülbírálják a kapcsolati képernyőn megadott értékeket. Ha ezek az értékek az alapértelmezett üres értékre vannak állítva, a rendszer a kapcsolati képernyő információit használja.

Kiválaszthatja azokat a tárolókat és szervezeti egységeket is, amelyeket az összekötőnek importálnia kell, és exportálnia kell.

Keresés végrehajtásakor ez a partíció összes tárolóján történik. Olyan esetekben, ahol nagy számú tároló működik, ez a viselkedés a teljesítmény romlását eredményezi.

> [!NOTE]
> Az általános LDAP-összekötő keresésének 2017. márciusi frissítésével kezdődően a hatókör csak a kiválasztott tárolóra korlátozható. Ezt a "keresés csak a kiválasztott tárolókban" jelölőnégyzet bejelölésével teheti meg, ahogy az alábbi képen is látható.

![Csak a kijelölt tárolók keresése](./media/microsoft-identity-manager-2016-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Horgonyok konfigurálása
Ez a lap mindig előre konfigurált értékkel rendelkezik, és nem módosítható. Ha meg van határozva a kiszolgáló gyártója, előfordulhat, hogy a horgony nem módosítható attribútummal van feltöltve, például egy objektum GUID azonosítója. Ha a rendszer nem észlelt vagy nem módosítható attribútummal rendelkezik, akkor az összekötő a megkülönböztető nevet használja a horgonyként.

![kapcsolatok alapjainak](./media/microsoft-identity-manager-2016-connector-genericldap/anchors.png)


Az alábbi lista felsorolja az LDAP-kiszolgálókat és a használt horgonyt:

| Címtár | Horgony attribútum |
| --- | --- |
| Microsoft AD LDS és AD GC |objectGUID |
| 389 Directory-kiszolgáló |megkülönböztető név |
| Apache-címtár |megkülönböztető név |
| IBM Tivoli DS |megkülönböztető név |
| Isode könyvtár |megkülönböztető név |
| Novell/NetIQ eDirectory |GUID |
| DJ/DS megnyitása |megkülönböztető név |
| LDAP megnyitása |megkülönböztető név |
| Oracle ODSEE |megkülönböztető név |
| RadiantOne VDS |megkülönböztető név |
| Sun One Directory-kiszolgáló |megkülönböztető név |

## <a name="other-notes"></a>Egyéb megjegyzések
Ez a szakasz az adott összekötőre vonatkozó, illetve egyéb okok miatt fontos tudnivalókat tartalmaz.

### <a name="delta-import"></a>Különbözeti importálás
Az Open LDAP Delta-vízjele UTC-dátum/idő. Emiatt a FIM szinkronizációs szolgáltatás és az Open LDAP közötti órákat szinkronizálni kell. Ha nem, akkor előfordulhat, hogy a változási napló egyes bejegyzései kimaradnak.

A Novell eDirectory esetében a különbözeti importálás egyetlen objektum törlését sem észleli. Ezért az összes törölt objektum megtalálásához rendszeresen teljes importálást kell futtatni.

A dátum/idő alapú különbözeti változási naplóval rendelkező címtárak esetében erősen ajánlott teljes importálást futtatni rendszeres időközönként. Ez a folyamat lehetővé teszi, hogy a szinkronizálási motor megkeresse és különbséget tegyen az LDAP-kiszolgáló és a jelenleg az összekötő terület között.

## <a name="troubleshooting"></a>Hibaelhárítás
* További információ az összekötők hibakeresésének engedélyezéséről: [ETW-nyomkövetés engedélyezése összekötők számára](https://go.microsoft.com/fwlink/?LinkId=335731).
