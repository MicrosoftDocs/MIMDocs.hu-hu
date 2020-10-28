---
title: Webszolgáltatás-összekötő konfigurációs lehetőségei | Microsoft Docs
description: Ez a cikk a webszolgáltatás-konfigurációs eszköz telepítéséhez szükséges lépéseket ismerteti.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 3/27/2020
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.reviewer: markwahl-msft
ms.assetid: ''
ms.openlocfilehash: 34c83427b6dfb3084976aebf29c019d8228f8247
ms.sourcegitcommit: d21963c1fba6dc908bec5eaadc54e3395a8ef8c3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/10/2020
ms.locfileid: "92761026"
---
# <a name="web-service-connector-configuration-options"></a>Webszolgáltatások összekötőjének konfigurálási lehetőségei
Ez a cikk ismerteti az új webszolgáltatás-összekötő konfigurálásának lépéseit, illetve a meglévő webszolgáltatás-összekötők módosítását a Microsoft Identity Manager (webszolgáltatások) szinkronizálási szolgáltatás felhasználói felületén.

>[!IMPORTANT]
>A cikk lépéseinek megkísérlése előtt töltse le és telepítse a [webszolgáltatás-összekötőt](https://www.microsoft.com/download/details.aspx?id=51495) .

## <a name="configure-the-web-service-connector-in-the-synchronization-service"></a>A webszolgáltatás-összekötő konfigurálása a szinkronizációs szolgáltatásban

Létrehozhat egy új webszolgáltatás-összekötőt a felügyeleti ügynök tervezőjével. Az összekötő létrehozása után több futtatási profilt is meghatározhat különböző feladatok végrehajtásához. Egy meglévő összekötő konfigurálásakor a felügyeleti ügynök tervezője lapon a megfelelő lapra kattintva módosíthatja a feladatokat. Az új webszolgáltatás-összekötő konfigurálásához kövesse az alábbi lépéseket.

1. Nyissa meg Microsoft Identity Manager 2016 szinkronizációs szolgáltatást. Az **eszközök** menüben válassza a **felügyeleti ügynökök** lehetőséget.

2. A **műveletek** menüben válassza a **Létrehozás** elemet. Megnyílik a felügyeleti ügynök tervezője.

3. A **felügyeleti ügynök Designer** területén, a **felügyeleti ügynök** területen válassza a **webszolgáltatás (Microsoft)** lehetőséget. Ezután válassza a **tovább** lehetőséget.

    ![Felügyeleti ügynök létrehozása](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma.png)

4. A **kapcsolat** képernyőn válassza ki az alapértelmezett **webszolgáltatás-összekötő projektet** . Adja meg a **gazdagép** és a **port** értékeit. Ezután válassza a **tovább** lehetőséget.

    ![Kapcsolat létrehozása a felügyeleti ügynökhöz](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-connectivity.png)

5. Adja meg a **globális paramétereket** . A gazdagéphez való csatlakozáshoz használja a webszolgáltatás rendszergazdájától beszerzett bejelentkezési hitelesítő adatokat. Ezután válassza a **tovább** lehetőséget.

    ![A felügyeleti ügynök globális paramétereinek megadása](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-global-parameters.png)

    - Ha az adatforrás helye figyeli a nyári mentést, és az adatforrás úgy van konfigurálva, hogy automatikusan igazodjanak a nyári mentési beállításokhoz, ellenőrizze, **hogy az adatforrás úgy van-e konfigurálva, hogy automatikusan módosítsa az órát a nyári** időszámításhoz.
    - Ha az összekötőről szeretné elindítani a teszt csatlakozási munkafolyamatot, ellenőrizze a **Csatlakozás tesztelése** lehetőséget.

6. A következő képernyőn válassza az **alapértelmezett** lehetőséget a címtárpartíciók **kiválasztásához** . Ezután válassza a **tovább** lehetőséget.

    ![Partíciók létrehozása a felügyeleti ügynökhöz](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-partitions.png)

7. Az **Objektumtípusok kiválasztása** képernyőn válassza ki a használni kívánt objektumtípust. Alapértelmezés szerint a webszolgáltatás-összekötő két objektumtípust támogat: az **alkalmazottakat** és a **felhasználókat** . Ezután válassza a **tovább** lehetőséget.

    ![Válassza ki az objektumtípust](media/microsoft-identity-manager-2016-ma-ws-maconfig/select-object-types.png)

8. Az **attribútumok kiválasztása** lapon válassza ki az összes kötelező attribútumot a kiválasztott objektumokhoz és attribútumokhoz, amelyekkel dolgoznia kell. Ezután válassza a **tovább** lehetőséget.

    ![Az objektumok attribútumainak kiválasztása](media/microsoft-identity-manager-2016-ma-ws-maconfig/select-attributes.png)

9. A **horgonyok konfigurálása** lapon adja meg a horgony attribútumait. Ezután válassza a **tovább** lehetőséget.

    ![A horgonyok konfigurálása](media/microsoft-identity-manager-2016-ma-ws-maconfig/configure-anchors.png)

10. Az **összekötő-szűrő beállítása** lapon adja meg az **összekötő szűrőjét** . Ezután válassza a **tovább** lehetőséget.

    ![Összekötő-szűrő meghatározása](media/microsoft-identity-manager-2016-ma-ws-maconfig/configure-connector-filter.png)

11. A **csatlakozási és leképezési szabályok konfigurálása** lapon adja meg az illesztési és leképezési szabályokat. Új csatlakoztatási szabály és leképezési szabály létrehozásához válassza az **új illesztési szabály** és az **új leképezési szabály** lehetőséget. Ezután válassza a **tovább** lehetőséget.

    ![Csatlakozási és leképezési szabályok meghatározása](media/microsoft-identity-manager-2016-ma-ws-maconfig/join-projection.png)

12. A következő lapon konfigurálja az attribútum folyamatát. A kiválasztott objektumtípusok attribútumaihoz meg kell adni a **leképezés típusát** és a **flow irányát** . Ezután válassza a **tovább** lehetőséget.

    ![Az attribútum folyamatának konfigurálása](media/microsoft-identity-manager-2016-ma-ws-maconfig/attribute-flow.png)

13. Adja meg az objektumokra alkalmazandó megszüntetés típusát. Ezután válassza a **tovább** lehetőséget.

    ![A megszüntetés típusának megadása](media/microsoft-identity-manager-2016-ma-ws-maconfig/deprovisioning.png)

14. Importálási folyamat esetén a **bővítmények konfigurálása** lap le van tiltva. A bővítmények exportálási folyamatokhoz való konfigurálásához először a **speciális** leképezési típust kell kiválasztania az **attribútum-folyamat konfigurálása** lapon.

    ![Bővítmények konfigurálása](media/microsoft-identity-manager-2016-ma-ws-maconfig/extensions.png)

15. Kattintson a **Finish** (Befejezés) gombra.

Az összekötő most már konfigurálva van:

![Az összekötő konfigurálása befejeződött](media/microsoft-identity-manager-2016-ma-ws-maconfig/sync-manager.png)

Egy összekötő konfigurálása után a futtatási profilok konfigurálásával konfigurálhatja a **futtatási profilok konfigurálása** lehetőséget.

## <a name="additional-steps"></a>További lépések

Tanúsítványalapú hitelesítés használata esetén szükség van egy további módosításra, miután a webszolgáltatás-konfigurációs eszköz létrehoz egy WSConfig-fájlt, mielőtt a fájlt importálja a webszolgáltatás-összekötő projektbe a webhelyhez tartozó szinkronizációs szolgáltatásban.

Tanúsítványalapú hitelesítés engedélyezése:

- A projekt konfigurálása egyszerű hitelesítés használatára a webszolgáltatás konfigurációs eszközében
- Hozza létre a my_project. wsconfig fájl másolatát, és nevezze át a következőre: my_project.zip
- Nyissa meg ezt az archívumot, és módosítsa generated.config fájlt az alapszintű hitelesítés a tanúsítványalapú hitelesítéssel való lecseréléséhez (az alább megadott példa)
- Cserélje le generated.config fájlt my_project.zip, és nevezze át my_project_updated. wsconfig
- My_project_updated. wsconfig kiválasztása felügyeleti ügynök létrehozásához a fakiszolgálói szinkronizációs kiszolgálón

Keresse meg az alábbi generated.config-mintát tanúsítványalapú hitelesítéssel:

```xml
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
        <appSettings>
            <add key="SoapAuthenticationType" value="Certificate"/>
        </appSettings>
        <system.serviceModel>
            <bindings>
                <wsHttpBinding>
                    <binding name="binding">
                        <security mode="Transport">
                            <transport clientCredentialType="Certificate"/>
                        </security>
                    </binding>
                </wsHttpBinding>
            </bindings>
            <client>
                <endpoint address="https://myserver.local.net:8011/sap/bc/srt/scs/sap/zsapconnect?sap-client=800"
                    binding="wsHttpBinding" bindingConfiguration="binding"
                    contract="SAPCONNECTOR.ZSAPConnect" name="binding"/>
            </client>
            <behaviors>
                <endpointBehaviors>
                    <behavior name="endpointCredentialBehavior">
                        <clientCredentials>
                            <clientCertificate findValue="my.certificate.name.local.net"
                                storeLocation="LocalMachine"
                                storeName="My"
                                x509FindType="FindBySubjectName"/>
                        </clientCredentials>
                    </behavior>
                </endpointBehaviors>
            </behaviors>
        </system.serviceModel>
    </configuration>
```

## <a name="next-steps"></a>Következő lépések

- [A webszolgáltatás-konfigurációs eszköz telepítése](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP telepítési útmutató](microsoft-identity-manager-2016-ma-ws-soap.md)
- [REST telepítési útmutató](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Webszolgáltatás MA – konfiguráció](microsoft-identity-manager-2016-ma-ws-maconfig.md)
