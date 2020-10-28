---
title: Microsoft Identity Manager 2016-portál testreszabásai | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 144ce2344327f16f60269b4f505c30fd470e46da
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760811"
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Microsoft Identity Manager 2016-portál testreszabása

A Microsoft Identity Manager 2016 (weblapon) a jelszó-portálok kijelölt elemeit testreszabhatja, beleértve a szalagcím emblémáját, a karakterlánc-erőforrásokat és a lépcsőzetes stíluslapokat is.

>[!WARNING]
>Mindig törölje a böngésző gyorsítótárát, ha bármilyen CSS-testreszabást végez.

A következő elemek használatosak a rendszerállapot-nyilvántartó 2016 jelszó-regisztrálási és-visszaállítási portálok testreszabásához:

- Testreszabások mappa: ez az a mappa, amelyet a webalkalmazás az alapértelmezett beállítások használata előtt a 2016-ben ellenőrzi. Minden testre szabott portálhoz testreszabási mappa szükséges. A testreszabásokat csak ebben a mappában kell végrehajtani, mert a telepítési folyamat nem írja felül ezt a mappát a frissítések, a módosítási mód telepítése vagy a javítási mód telepítésekor.

- Strings. Resources: ez egy XML-alapú fájl, amely lehetővé teszi a portálon megjelenő karakterláncok módosítását. A fájlnak a testreszabások mappában kell lennie.

- Style. CSS: a portálok által a testreszabáshoz használt lépcsőzetes stíluslap. Hozza létre és módosítsa ezt a stíluslapot az embléma módosításához. A stíluslap teljes tartalmát a saját testreszabásával is lecserélheti.

A jelszó-regisztrálási és a jelszó-visszaállítási portálok testreszabásával kapcsolatos részletes útmutatásért lásd 2016: tesztkörnyezet-útmutató

>[!WARNING]
>Ahhoz, hogy a rendszer képes legyen felismerni a testreszabott módosításokat, újra kell indítania az IIS-t `iisreset` .


## <a name="customizations-folder"></a>Testreszabások mappa

Indításkor a rendszer az alapértelmezett értékek használata előtt megkeresi a karakterláncok. Resources fájlt a testreszabások mappában. Hozzon létre egy testreszabási mappát a testreszabni kívánt portál könyvtárában (azaz a jelszó-regisztrációs portálon vagy a jelszó-visszaállítási portálon). Ha mindkét portált testre szeretné szabni, hozzon létre egy testreszabási mappát az alábbi két helyen:

- `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Registration Portal`

- `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Reset Portal`

Testreszabási mappa létrehozása a jelszó-regisztrációs portálhoz:

1. Tallózással keresse meg a `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Registration Portal` mappát.
   
2. Hozzon létre egy **testreszabások** nevű mappát.

Testreszabási mappa létrehozása a jelszó-visszaállítási portálhoz:

1. Tallózással keresse meg a `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Reset Portal` mappát.
   
2. Hozzon létre egy **testreszabások** nevű mappát.


## <a name="custom-strings-in-the-stringsresources-file"></a>Egyéni karakterláncok a karakterláncok. Resources fájlban

A portál felhasználói felületének számos karakterlánca testreszabható egy Strings. Resources fájl létrehozásával és a fájl a testreszabások mappájába való hozzáadásával. Hozzon létre egy karakterlánc. Resources fájlt minden testre szabni kívánt portálhoz.

Testreszabott karakterláncok létrehozása a karakterláncok. Resources fájlban:

1. A Jegyzettömbben másolja ki és illessze be a következő kódot a karakterláncok. Resources fájlba. Mentse a fájlt a portál testreszabások mappájába.

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```

2.  A `<!-- Customizations begin here -->` kód szakasza alatt módosítsa az adatnevet úgy, hogy az megfeleljen a testreszabni kívánt karakterláncoknak. Adja meg a címkék közötti karakterlánc értékét `<value></value>` . Tekintse meg a testreszabható karakterláncok következő részeit, valamint az alapértelmezett értékeket.

>[!NOTE]
>A Strings. Resources fájl a Language-semleges. Nyelvspecifikus testreszabott karakterláncok létrehozásához telepíteni kell a nyelvi csomagot, és a fájlt karakterlánc formátumban kell menteni `<language>-<culture>.resources` . Az angol nyelvi kulturális környezet esetében például a fájlnév **karakterláncok. en-us. Resources** .


## <a name="portal-strings"></a>Portál sztringek
A következő táblázat a testreszabott portálon található karakterláncokat tartalmazza:

<!-- The default values are actual UI strings and should not copy edited. -->

| Portál karakterláncának neve | Alapértelmezett érték |
|---|---|
| AboutLinkText                                            | Névjegy  |
| ButtonCancel                                             | Mégse |
| ButtonFinish                                             | Befejezés |
| ButtonNext                                               | Következő   |
| ButtonOk                                                 | OK     |
| CancelFinishedMessage                                    | A munkamenet már nem aktív. Az alábbi hivatkozásra kattintva lezárhatja az ablakot, vagy újraindíthatja. |
| CancelFinishedTitle                                      | A munkamenet véget ért |
| ErrorDescription_3000                                    | Hiba történt. Próbálkozzon újra, és ha a probléma továbbra is fennáll, forduljon az ügyfélszolgálathoz vagy a rendszergazdához. (3000-es hiba) |
| ErrorDescription_3001                                    | Győződjön meg arról, hogy helyesen adja meg a felhasználónevet. Ha továbbra sem tudja alaphelyzetbe állítani a jelszavát, kérjen segítséget az ügyfélszolgálattól. (3001-es hiba) |
| ErrorDescription_3002                                    | A munkamenet véget ért. Térjen vissza a kezdőlapra, és kezdje újra. (3002-es hiba) |
| ErrorDescription_3003                                    | A Forefront Identity Manager nem ismeri fel az aktuális felhasználói fiókot. Forduljon az ügyfélszolgálathoz vagy a rendszergazdához. (3003-es hiba) |
| ErrorDescription_3004                                    | Ön nem jogosult a jelszó-visszaállításra való regisztrációra. Forduljon az ügyfélszolgálathoz vagy a rendszergazdához. (3004-es hiba) |
| ErrorDescription_3005                                    | Egy vagy több megadott válasz nem felel meg a jelszó-regisztráció során megadott válaszoknak. A jelszó alaphelyzetbe állításához az Ön által megadott válaszokat meg kell egyeznie a regisztráláskor megadott válaszokkal. Kezdje újra a kezdőlapon, vagy forduljon az ügyfélszolgálathoz vagy a rendszergazdához. (3005-es hiba) |
| ErrorDescription_3006                                    | A megadott jelszó helytelen. A jelszó-visszaállításhoz való regisztrációhoz meg kell adnia a megfelelő jelszót. (3006-es hiba) |
| ErrorDescription_3007                                    | A jelszó alaphelyzetbe állítása átmenetileg nem engedélyezett. Próbálkozzon újra később, vagy segítségért forduljon az ügyfélszolgálathoz vagy a rendszergazdához. (3007-es hiba) |
| ErrorDescription_3008                                    | Hiba történt. Próbálkozzon újra, és ha a probléma továbbra is fennáll, forduljon az ügyfélszolgálathoz vagy a rendszergazdához. (3008-es hiba) |
| ErrorDescription_3009                                    | A bemenet nem megengedett formátumú szöveget tartalmaz. Próbálkozzon újra más bevitelsel, vagy forduljon az ügyfélszolgálathoz vagy a rendszergazdához. (3009-es hiba) |
| ErrorDescription_3010_Registration                       | A parancsfájl nincs engedélyezve a böngészőben. Engedélyezze a parancsfájlok futtatását, és térjen vissza a jelszó-regisztrálási kezdőlapra, vagy forduljon az ügyfélszolgálathoz segítségért. |
| ErrorDescription_3010_Reset                              | A parancsfájl nincs engedélyezve a böngészőben. Engedélyezze a parancsfájlok futtatását, és térjen vissza a jelszó-visszaállítás kezdőlapjára, vagy forduljon az ügyfélszolgálathoz segítségért. |
| ErrorDescription_3011                                    | Ez a hely cookie-kat használ. Konfigurálja a böngészőt, hogy fogadja a cookie-kat, és próbálkozzon újra, vagy forduljon az ügyfélszolgálathoz segítségért. |
| ErrorDescription_3012                                    | A beírt érték nem egyezik az Önnek elküldett biztonsági kóddal. Megpróbálhatja újból visszaállítani a jelszavát, vagy segítségért forduljon az ügyfélszolgálathoz.  |
| ErrorDescription_3013                                    | Nem sikerült elküldeni a biztonsági kódot. Segítségért forduljon az ügyfélszolgálathoz. |
| ErrorMessageDomainUsernameFormat                         | A felhasználónevet a megfelelő formátumban adja meg. |
| ErrorMessageDomainUsernameRequired                       | A folytatáshoz adjon meg egy felhasználónevet. |
| ErrorMessagePasswordRequired                             | Adjon meg egy jelszót. |
| ErrorMessagePasswordsDoNotMatch                          | Győződjön meg arról, hogy mindkét jelszó egyezik. |
| ErrorPageDefaultHeading                                  | Alkalmazáshiba <br/>**Megjegyzés** : a fejlécet a "=" és a hibaüzenet követi.
| ErrorPageServerTime                                      | Kiszolgáló időpontja: {0:T} <br/>**Megjegyzés** : {0} a kivétel kifogásának ideje. A "nem" érték azt eredményezi, hogy az átadott idő "hosszú idő" formátumban jelenjen meg, amely az órát, a percet és a másodpercet mutatja. Az AM/PM megjelölése az aktuális kulturális környezettől függően is megjelenik. |
| ErrorPageTitle                                           | Forefront Identity Management – jelszavas hiba | 
| ErrorTitle_3000                                          | Hiba |
| ErrorTitle_3001                                          | Hozzáférés megtagadva |
| ErrorTitle_3002                                          | A munkamenet véget ért |
| ErrorTitle_3003                                          | Ismeretlen felhasználó |
| ErrorTitle_3004                                          | Jogosulatlan felhasználó |
| ErrorTitle_3005                                          | A válaszok nem egyeznek |
| ErrorTitle_3006                                          | Helytelen jelszó |
| ErrorTitle_3007                                          | A hozzáférés megtagadva átmenetileg |
| ErrorTitle_3008                                          | Kommunikációs hiba |
| ErrorTitle_3009                                          | Tiltott bevitel |
| ErrorTitle_3010                                          | Böngésző-konfigurációs hiba |
| ErrorTitle_3011                                          | Böngésző-konfigurációs hiba |
| ErrorTitle_3012                                          | Az ellenőrzés nem sikerült |
| ErrorTitle_3013                                          | Nem sikerült elküldeni a biztonsági kódot |
| FinalizeRegistrationHeading1                             | Ha bármikor vissza kell állítania a jelszavát: |
| FinalizeRegistrationSubHeading1                          | Lépjen a jelszó alaphelyzetbe állítása portálra |
| FinalizeRegistrationSubHeading2                          | Személyazonosság ellenőrzése |
| FinalizeRegistrationSubHeading3                          | Új jelszó kiválasztása |
| FinishingDescription                                     | Új jelszó kiválasztása |
| FinishingTitle                                           | Jelszó alaphelyzetbe állítása: |
| GotoPortalPrefix                                         | Ugrás ide |
| GotoPortalSuffix                                         | Kezdőlap |
| LabelTroubleshootingLinkText                             | Részletek megtekintése |
| LoadingText                                              | Betöltés folyamatban... |
| NoScriptTagErrorMessage                                  | A parancsfájl nincs engedélyezve a böngészőben. Engedélyezze a parancsfájlok futtatását, térjen vissza a kezdőlapra, vagy forduljon az ügyfélszolgálathoz segítségért.  |
| PasswordResetOperationGeneralErrorMessage                | Hiba történt a jelszó alaphelyzetbe állítására tett kísérlet során. |
| PasswordResetOperationPolicyViolationErrorMessage        | A jelszó nem felel meg a szervezete jelszavas házirendjeinek. |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Hiba történt a jelszó alaphelyzetbe állítása közben, a felhasználó nem módosíthatja a jelszót. |
| PrivacyStatement                                         | Adatvédelmi nyilatkozat |
| RegistrationDescription                                  | Self-Service jelszó-regisztráció |
| RegistrationMission                                      | Ha elfelejti a jelszavát, az ügyfélszolgálat meghívása nélkül is visszaállíthatja. |
| RegistrationPageTitle                                    | Forefront Identity Management – jelszó-regisztráció |
| RegistrationSteps                                        | A regisztrációs folyamat elindításához kattintson a Next (tovább) gombra. |
| RegistrationSuccessDescription                           | Most regisztrált |
| RegistrationSuccessTitle                                 | Befejezve: |
| RegistrationWelcomeTitle                                 | Jelszó-regisztrálás: |
| ResetDescription                                         | Önkiszolgáló jelszóváltoztatás |
| ResetEnterNamePrompt                                     | Adja meg az alábbi felhasználónevet |
| ResetEnterPassword                                       | Adjon meg egy új jelszót: |
| ResetExample1                                            | `contoso\mmeyers` |
| ResetExample2                                            | `mmeyers\@contoso.com` |
| ResetExamples                                            | Példák: |
| ResetPageTitle                                           | Forefront Identity Management-jelszó alaphelyzetbe állítása |
| ResetReenterPassword                                     | Írja be újra a jelszót: |
| ResetSuccessDescription                                  | A jelszó vissza lett állítva |
| ResetSuccessTitle                                        | Sikeres |
| ResetUseNewPassword                                      | Most már használhatja az új jelszót a bejelentkezéshez. |
| ResetUsernameTextFormat                                  | (Jelszó alaphelyzetbe állítása {0} ) <br/>**Megjegyzés** : {0} a felhasználó bejelentkezése. |
| ResetWelcomeTitle                                        | Jelszó alaphelyzetbe állítása: |
| TroubleshootingEmailSubject                              | FIM-kérelem feldolgozási hibájának részletei |
| TroubleshootingLabelAttributes                           | Attribútumok: |
| TroubleshootingLabelCloseButton                          | Bezárás |
| TroubleshootingLabelCopyToClipboard                      | Másolás a vágólapra |
| TroubleshootingLabelCorrelationId                        | Korrelációs azonosító: |
| TroubleshootingLabelDetails                              | Részletek: |
| TroubleshootingLabelPostCopyClipboardMessage             | A rendszer átmásolta az adatokat a vágólapra. |
| TroubleshootingLabelRequestId                            | Kérelemazonosító: |
| TroubleshootingLabelSendEmail                            | Adatok küldése e-mailben |
| TroubleshootingLabelSource                               | Ok: |
| TroubleshootingLabelViewRequestDetails                   | Kérelem részleteinek megtekintése |
| TroubleshootingLinkText                                  | Hibaelhárítási információk |


## <a name="authentication-gate-strings"></a>Hitelesítési kapu karakterláncai
A következő táblázat a hitelesítő sztringek testreszabható karakterláncait mutatja be:

<!-- The default values are actual UI strings and should not copy edited. -->

| Hitelesítési kapu karakterláncának neve | Alapértelmezett érték |
|---|---|
| OTPEmailRegistraionEmailTextboxLabel                  | E-mail cím: |
| OTPEmailRegistrationEmailRequiredErrorMessage         | Az e-mail-cím mező nem lehet üres. |
| OTPEmailRegistrationFooterReadOnly                    | Az e-mail-cím frissítéséhez kövesse a szervezet által meghatározott folyamatot, vagy hívja meg az ügyfélszolgálatot. |
| OTPEmailRegistrationFooterReadWrite                   | Az e-mail-címet a szervezet a Forefront Identity Managerben tárolja. |
| OTPEmailRegistrationGateTitle                         | E-mail-cím ellenőrzése |
| OTPEmailRegistrationHeaderReadOnly                    | Ha bármikor vissza kell állítania a jelszavát, a rendszer egy ellenőrző biztonsági kódot küld az e-mail-címre. Ha az alább látható e-mail-cím nem megfelelő, akkor frissítenie kell ezt az önkiszolgáló jelszó-visszaállítás használatához. |
| OTPEmailRegistrationHeaderReadWrite                   | Adja meg az e-mail-címét alább. Ha bármikor vissza kell állítania a jelszavát, a rendszer egy ellenőrző kódot küld az e-mail-címre. |
| OTPEmailResetGateTitle                                | Személyazonosság ellenőrzése: E-mail |
| OTPEmailResetHeader                                   | Adja meg az alábbi biztonsági kódot. A rendszer elküldte a biztonsági kódot a vállalatnál regisztrált e-mail-címre. |
| OTPRegularExpressionErrorMessage                      | A megadott érték nem felel meg a várt formátumnak. |
| OTPResetOneTimePasswordRequiredErrorMessage           | A biztonsági kód mező nem lehet üres. |
| OTPResetVerificationLabel                             | Biztonsági kód: |
| OTPSmsRegistrationFooterReadOnly                      | A mobil telefonszámának frissítéséhez kövesse a szervezet által meghatározott folyamatot, vagy hívja meg az ügyfélszolgálatot. |
| OTPSmsRegistrationFooterReadWrite                     | A mobil telefonszámot a szervezet a Forefront Identity Managerben tárolja. |
| OTPSmsRegistrationGateTitle                           | Mobiltelefon ellenőrzése |
| OTPSmsRegistrationHeaderReadOnly                      | Ha bármikor vissza kell állítania a jelszavát, a rendszer egy ellenőrző biztonsági kódot küld a mobiltelefonjára. Ha az alább látható Mobiltelefonszám nem megfelelő, akkor frissítenie kell ezt az önkiszolgáló jelszó-visszaállítás használatához. |
| OTPSmsRegistrationHeaderReadWrite                     | Adja meg a mobil telefonszámát alább. Ha bármikor vissza kell állítania a jelszavát, a rendszer egy ellenőrző kódot küld a mobiltelefonjára. |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | A mobil telefonszám mező nem lehet üres. |
| OTPSmsRegistrationSMSTextBoxLabel                     | Mobil telefon: |
| OTPSmsResetGateTitle                                  | Személyazonosság ellenőrzése: mobiltelefon |
| OTPSmsResetHeader                                     | Adja meg az alábbi biztonsági kódot. A rendszer biztonsági kódot kapott a vállalatnál regisztrált mobiltelefonra. |
| PasswordGateDescriptionText                           | Adja meg az aktuális jelszót, majd kattintson a Next (tovább) gombra. |
| PasswordGateErrorMessagePasswordRequired              | Adja meg a jelenlegi jelszavát. |
| PasswordGateGateTitle                                 | Aktuális jelszava |
| PasswordGatePasswordLabelText                         | Jelszó: |
| PasswordGateUsernameTextFormat                        | `<i>` (a következőként van bejelentkezve: `<b>{0}</b>` ) `</i>` |
| QAGateErrorNotEnoughQuestionsAnswered                 | Legalább egy kérdésre válaszolnia kell {0} . |
| QAGateIncorrectAnswer                                 | A válaszok nem megfelelőek. |
| QAGatePrivacyNotice                                   | Az Ön által megadott válaszokat a szervezet a Forefront Identity Managerben tárolja. |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | Legalább a regisztrálni kívánt kérdésre válaszolnia kell {0} . |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Egy vagy több válasz nem felel meg a szabályzatnak. |
| QAGateRegistrationThisAnswerValidationFailed          | Ez a válasz nem felel meg a szabályzatnak. |
| QAGateRegistrationTitle                               | Válaszok regisztrálása |
| QAGateResetNumberOfQuestionsExplanation_Format        | {0}A következő kérdésekre kell válaszolnia {1} . |
| QAGateResetTitle                                      | Személyazonosság ellenőrzése: küldje el válaszait  |


## <a name="custom-logo-banners"></a>Egyéni embléma bannerek

A portál oldalain található alapértelmezett szalagcím testreszabható a szervezet számára.

Az embléma szalagcímének testreszabása:

1. Hozza létre saját egyéni szalagcímeit, és mentse őket. png fájlként. A fájloknak meg kell felelniük a következő javaslatoknak:

   - Méret: 490 x 50 képpont.
   - Bit mélység: 32 képpont.

2. Másolja a fájlokat a testreszabások mappába a testre szabni kívánt portálon.

3. Hozzon létre egy Style. css fájlt az egyes mappákban. Mutasson a fájlra a portál és az új embléma testreszabások mappájába. Szükség szerint módosíthatja az embléma nevét, például: `/Customizations/contosologo.png` . A CSS-nek a következő kódhoz hasonlóan kell kinéznie:

   `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4. Ha az Internet Explorer 6,0-et használja, alternatív, nem transzparens emblémát kell megadnia, és hozzá kell adnia a következő kódot a Style. CSS-hez:

   `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

   A CSS-nek a következő kódhoz hasonlóan kell kinéznie:
   
   `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`


## <a name="custom-images-for-smartphones"></a>Egyéni rendszerképek az okostelefonokhoz

Testreszabhatja az okostelefonok emblémáját. 

Egy okostelefonhoz tartozó rendszerkép testreszabása:

1. Hozza létre a lemezképeket, és mentse őket. png fájlként. A fájloknak meg kell felelniük a következő javaslatoknak:

   - Méret: 190 X 50 képpont.
   - Bit mélység: 32 képpont.

2. Másolja a fájlokat a testreszabások mappába a testre szabni kívánt portálon.

3. Hozzon létre egy Style. css fájlt az egyes mappákban. Mutasson a fájlra a portál és az új embléma testreszabások mappájába. Szükség szerint módosíthatja az embléma nevét, például: `/Customizations/contosologo.png` . A CSS-nek a következő kódhoz hasonlóan kell kinéznie:

   ```css
   @media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="custom-style-sheets"></a>Egyéni stíluslapok

A jelszó-portálok elrendezését és stílusát testreszabott lépcsőzetes stíluslap (CSS) használatával módosíthatja.

Testreszabott CSS használata:

1. Hozza létre a testreszabott CSS-fájlokat, és mentse őket Style. css néven.

2. Másolja a fájlokat a testreszabások mappába a testre szabni kívánt portálon.

Az alábbi kód egy Style. css-fájl alapszintű példája:

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .

title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```

>[!IMPORTANT]
>Ahhoz, hogy a rendszer képes legyen felismerni a testreszabott módosításokat, újra kell indítania az IIS-t `iisreset` .

A Style. css fájl fejlettebb példája a következő kód. Ez a fájl a portálok ezen eszközökön való megjelenítésére szolgáló okostelefonra vagy Apple iPadre vonatkozó információkat tartalmaz.

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/

#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>Gyakori testreszabási problémák

A következő táblázat felsorolja azokat a gyakori problémákat, amelyek a FIM szolgáltatás és a webalkalmazás-portál frissítése során fordulhatnak elő.

| Probléma | Feloldás |
|---|---|
| Egy karakterlánc-testreszabást végeztem, de a felhasználói felületen nem volt látható.                       | Karakterlánc-testreszabások a karakterláncokban. az erőforrásoknak az IIS újraindítását kell futtatnia `iisreset` . |
| A karakterláncok létrehozása után az erőforrások megváltoznak, nem látok semmilyen karakterlánc-változást.          | A karakterláncok. az erőforrások formátuma valószínűleg helytelen formátumú, és a portál figyelmen kívül hagyhatja. Az Eseménynaplóban keresse meg **Windows Logs** a  >  Forefront Identity Managert a Windows **-Naplók alkalmazás és szolgáltatások** naplóban  >  **Forefront Identity Manager** . |
| A Style. css hozzáadásakor a rendszer első alkalommal nem látom a stílusom változásait a portálon.     | Amikor először bevezet egy Style. css fájlt, futtatnia kell `iisreset` . |
| Az új stílusok a Style. CSS-ben lesznek hozzáadva vagy módosítva, de a böngésző nem látja a módosításokat. | Törölje a böngésző gyorsítótárát, és frissítse az oldalt. Keresse meg a CSS-szintaxist. |
| Közvetlenül Megváltoztattam a CSS `<path_to_sspr_portal>\css\*.css` -mappa vagy a szalagcím emblémájának tartalmát `<path_to_sspr_portal>\images\fimlogo.png` . Elveszítettem ezeket a módosításokat a frissítéskor. | A felhasználók nem módosíthatják közvetlenül ezeket a fájlokat. Csak a testreszabások mappa használatával adjon meg egy szalagcím emblémát, és csak a Style. css CSS-stílusú testreszabásokat hajtson végre. A testreszabások mappát szándékosan nem írják felül a főbb frissítések. Ne használjon olyan eszközöket, mint a ILSpy és a tükröző, hogy megváltoztassa a sztringeket a portál szerelvényekben. A sztrings. Resources paranccsal felülbírálhatja az alapértelmezett karakterláncokat. A szerelvényeket a rendszer a frissítéskor váltja fel.  |
| A szalagcím emblémája nem jelenik meg a portálon. Továbbra is megjelenik a FIM-embléma.                  | A rendszerkép neve/elérési útja a Style. css érvénytelen, vagy a böngésző gyorsítótára nem lett törölve.  |
| A szalagcím emblémája csúnyanak tűnik az Internet Explorer 6-os verziójában.                                          | Adjon meg egy nem transzparens képet az Internet Explorer 6 számára a Style. css fájlhoz tartozó képhez tartozó stílussal. |
