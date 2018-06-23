---
title: Cloudové ověřování v rozhraní web API s Azure Active Directory B2C v ASP.NET Core
author: camsoper
description: Zjistit, jak nastavit ověřování Azure Active Directory B2C pomocí webového rozhraní API ASP.NET Core. Otestujte ověření webového rozhraní API s Postman.
ms.author: casoper
ms.date: 01/25/2018
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: c56efda28c668b8f88d28334705b4c26f288870f
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314159"
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a>Cloudové ověřování v rozhraní web API s Azure Active Directory B2C v ASP.NET Core

Podle [Soper kamera](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) je cloudové řešení správy identit pro webové a mobilní aplikace. Služba poskytuje ověřování pro aplikace, které jsou hostované v cloudu nebo místně. Typy ověřování zahrnují jednotlivé účty, účty sociálních sítí a federovaných účty organizace. Kromě toho Azure AD B2C můžete zadat služby Multi-Factor authentication s minimální konfigurací.

> [!TIP]
> Azure Active Directory (Azure AD) a Azure AD B2C je samostatný produkt nabídky. Klient služby Azure AD představuje organizaci, zatímco klienta Azure AD B2C představuje kolekci identit pro použití s aplikacemi předávajících stran. Další informace najdete v tématu [Azure AD B2C: Nejčastější dotazy (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Vzhledem k tomu, že webová rozhraní API bez uživatelského rozhraní, nepodařilo přesměruje uživatele na zabezpečené tokenu služby jako Azure AD B2C. Místo toho rozhraní API je předán token nosiče z volající aplikace, která je již ověřen uživatele s Azure AD B2C. Rozhraní API pak ověří daný token bez zásahu uživatele direct.

V tomto kurzu, zjistěte, jak:

> [!div class="checklist"]
> * Vytvoření klienta Azure Active Directory B2C.
> * Zaregistrujte webového rozhraní API v Azure AD B2C.
> * Pomocí sady Visual Studio k vytvoření webového rozhraní API nakonfigurovaný pro ověřování pomocí klienta Azure AD B2C.
> * Nakonfigurujte zásady řídí chování klienta Azure AD B2C.
> * Použití Postman k simulaci webovou aplikaci, která uvede přihlašovací dialogové okno, načte token a použije k vytvoření žádosti o proti webové rozhraní API.

## <a name="prerequisites"></a>Požadavky

Tady jsou požadované pro Tento názorný postup:

* [Předplatné Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (všechny edice)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Vytvoření klienta Azure Active Directory B2C

Vytvoření klienta Azure AD B2C [popsané v dokumentaci k](/azure/active-directory-b2c/active-directory-b2c-get-started). Po zobrazení výzvy ke klientovi přidružit předplatné Azure je volitelný pro účely tohoto kurzu.

## <a name="configure-a-sign-up-or-sign-in-policy"></a>Nakonfigurujte zásady registrace nebo přihlášení

Použijte postup v dokumentaci k Azure AD B2C na [vytvořit zásadu registrace nebo přihlášení](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy). Název zásady **SiUpIn**.  Použít ukázkové hodnoty uvedeny v dokumentaci pro **zprostředkovatelů Identity**, **atributy registrace**, a **deklarace identity aplikace**. Pomocí **spustit nyní** tlačítko k testování zásad, jak je popsáno v dokumentaci je volitelné.

## <a name="register-the-api-in-azure-ad-b2c"></a>Registrace rozhraní API v Azure AD B2C

V nově vytvořený klienta Azure AD B2C, registraci vašeho rozhraní API pomocí [kroky v dokumentaci k](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api) pod **zaregistrovat webového rozhraní API** části.

Použijte následující hodnoty:

| Nastavení                       | Hodnota               | Poznámky                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Jméno**                      | *&lt;Název rozhraní API&gt;*  | Zadejte **název** pro aplikaci, která popisuje aplikace k příjemce.                     |
| **Zahrnout webovou aplikaci / webové rozhraní API** | Ano                 |                                                                                        |
| **Povolit implicitního toku**       | Ano                 |                                                                                        |
| **Adresa URL odpovědi**                 | `https://localhost` | Adresy URL odpovědí jsou koncové body, kde Azure AD B2C vrátí všechny tokeny, které vaše aplikace vyžaduje. |
| **Identifikátor ID URI aplikace**                | *api*               | Identifikátor URI nepotřebuje přeložit na fyzickou adresu. Pouze musí být jedinečný.     |
| **Zahrnout nativního klienta**     | Ne                  |                                                                                        |

Po registraci rozhraní API se zobrazí seznam aplikací a rozhraní API v klientovi. Vyberte rozhraní API, který byl právě zaregistrován. Vyberte **kopie** ikony napravo **ID aplikace** pole a zkopírujte ho do schránky. Vyberte **publikovaná obory** a ověření výchozího *user_impersonation* nachází oboru.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Vytvoření aplikace ASP.NET Core v Visual Studio 2017

Šablony sady Visual Studio webové aplikace lze nakonfigurovat pro ověřování pomocí klienta Azure AD B2C.

V sadě Visual Studio:

1. Vytvořte novou webovou aplikaci ASP.NET Core. 
2. Vyberte **webového rozhraní API** ze seznamu šablon.
3. Vyberte **změna ověřování** tlačítko.

    ![Tlačítko Změnit ověřování](./azure-ad-b2c-webapi/change-auth-button.png)

4. V **změna ověřování** dialogovém okně, vyberte **jednotlivé uživatelské účty**a potom vyberte **připojit k existujícímu úložišti uživatele v cloudu** v rozevírací nabídce. 

    ![Dialogové okno Změnit ověřování](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. Vyplňte formulář s následujícími hodnotami:

    | Nastavení                       | Hodnota                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Název domény**               | *&lt;název domény vašeho klienta B2C&gt;*          |
    | **ID aplikace**            | *&lt;Vložte ID aplikace ze schránky&gt;* |
    | **Zásady registrace nebo přihlášení** | `B2C_1_SiUpIn`                                        |

    Vyberte **OK** zavřete **změna ověřování** dialogové okno. Vyberte **OK** k vytvoření webové aplikace.

Visual Studio vytvoří webové rozhraní API s řadičem s názvem *ValuesController.cs* , který vrací hodnoty pevně pro požadavky GET. Třída je upraven pomocí [autorizovat atribut](xref:security/authorization/simple), takže všechny požadavky vyžadují ověřování.

## <a name="run-the-web-api"></a>Spuštění webové rozhraní API

V sadě Visual Studio spusťte rozhraní API. Visual Studio spustí prohlížeč odkazoval na adresy URL kořenového adresáře rozhraní API. Poznamenejte si adresu URL na panelu Adresa a nechte rozhraní API spuštěná na pozadí.

> [!NOTE]
> Vzhledem k tomu, že není k dispozici žádný řadič definované pro adresy URL kořenového adresáře, prohlížeč zobrazí chybu 404 (stránka nebyla nalezena). Toto je očekávané chování.

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>Použití Postman k získání tokenu a testování rozhraní API

[Postman](https://getpostman.com/postman) je nástroj pro testování webové rozhraní API. V tomto kurzu simuluje Postman webové aplikace, který přistupuje k webové rozhraní API jménem uživatele.

### <a name="register-postman-as-a-web-app"></a>Zaregistrujte se jako webovou aplikaci Postman

Vzhledem k tomu, že Postman simuluje webovou aplikaci, která můžou získat tokeny z klienta Azure AD B2C, se musí být zaregistrován v klientovi jako webovou aplikaci. Zaregistrovat pomocí Postman [kroky v dokumentaci k](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) pod **zaregistrovat webové aplikace** části. Zastavit **vytvoření tajného klíče klienta webové aplikace** části. Tajný klíč klienta není povinné pro účely tohoto kurzu. 

Použijte následující hodnoty:

| Nastavení                       | Hodnota                            | Poznámky                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Jméno**                      | Postman                          |                                 |
| **Zahrnout webovou aplikaci / webové rozhraní API** | Ano                              |                                 |
| **Povolit implicitního toku**       | Ano                              |                                 |
| **Adresa URL odpovědi**                 | `https://getpostman.com/postman` |                                 |
| **Identifikátor ID URI aplikace**                | *&lt;ponechat prázdné&gt;*            | Nepožaduje se pro tento kurz. |
| **Zahrnout nativního klienta**     | Ne                               |                                 |

Nově zaregistrovaný webové aplikace potřebuje oprávnění pro přístup k webové rozhraní API jménem uživatele.  

1. Vyberte **Postman** v seznamu aplikací a pak vyberte **přístup pomocí rozhraní API** z nabídky na levé straně.
2. Vyberte **+ přidat**.
3. V **vyberte rozhraní API** rozevíracího seznamu, vyberte název webové rozhraní API.
4. V **vyberte obory** rozevíracího seznamu, ujistěte se, jsou vybrané všechny obory.
5. Vyberte **Ok**.

Všimněte si aplikaci Postman ID aplikace, jako je potřeba získat token nosiče.

### <a name="create-a-postman-request"></a>Vytvořit žádost o Postman

Spusťte Postman. Ve výchozím nastavení, zobrazí Postman **vytvořit nový** dialogové okno po spuštění. Pokud se nezobrazí dialogovém okně, vyberte **+ nový** tlačítko v levé horní části.

Z **vytvořit nový** dialogové okno:

1. Vyberte **požadavku**.

    ![Tlačítko požadavku](./azure-ad-b2c-webapi/postman-create-new.png)

2. Zadejte *získat hodnoty* v **název žádosti o** pole.
3. Vyberte **+ vytvoření kolekce** vytvořit novou kolekci pro ukládání žádosti. Název kolekce *ASP.NET Core kurzy* a potom vyberte na značku zaškrtnutí.

    ![Vytvořit novou kolekci](./azure-ad-b2c-webapi/postman-create-collection.png)

4. Vyberte **uložit do ASP.NET Core kurzy** tlačítko.

### <a name="test-the-web-api-without-authentication"></a>Test webové rozhraní API bez ověřování

Pokud chcete ověřit, že webové rozhraní API vyžaduje ověřování, nejprve proveďte žádost bez ověřování.

1. V **zadejte adresu URL požadavku** zadejte adresu URL pro `ValuesController`. Adresa URL je stejné, jako je zobrazena v prohlížeči s **rozhraní api nebo hodnotami** připojí. Příkladem může být `https://localhost:44375/api/values`.
2. Vyberte **odeslat** tlačítko.
3. Všimněte si, je stav odpovědi *401 – Neověřeno*.

    ![odpovědi 401 neoprávněný](./azure-ad-b2c-webapi/postman-401-status.png)

### <a name="obtain-a-bearer-token"></a>Získání tokenu nosiče

Chcete-li požadavek na ověřeného webovému rozhraní API, je požadovaná token nosiče. Postman usnadňuje přihlásit k klienta Azure AD B2C a získat token.

1. Na **autorizace** ve **typ** rozevíracího seznamu vyberte **OAuth 2.0**. V **přidat autorizační data do** rozevíracího seznamu vyberte **záhlaví požadavku**. Vyberte **získání tokenu přístupu nové**.

    ![Karta autorizace s nastavením](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. Dokončení **získat nové přístup TOKENU** dialogové okno následujícím způsobem:


   |                Nastavení                 |                                             Hodnota                                             |                                                                                                                                    Poznámky                                                                                                                                     |
   |----------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      <strong>Název tokenu</strong>       |                                  <em>&lt;Název tokenu&gt;</em>                                  |                                                                                                                   Zadejte popisný název pro daný token.                                                                                                                    |
   |      <strong>Typ udělení</strong>       |                                           Implicitní                                            |                                                                                                                                                                                                                                                                              |
   |     <strong>Adresa URL zpětného volání</strong>      |                               `https://getpostman.com/postman`                                |                                                                                                                                                                                                                                                                              |
   |       <strong>Ověřování adresy URL</strong>        | `https://login.microsoftonline.com/tfp/<tenant domain name>/B2C_1_SiUpIn/oauth2/v2.0/authorize` |                                                                                                  Nahraďte <em>&lt;název domény klienta&gt;</em> s názvem domény klienta.                                                                                                  |
   |       <strong>ID klienta</strong>       |                <em>&lt;Zadejte aplikaci Postman <b>ID aplikace</b>&gt;</em>                 |                                                                                                                                                                                                                                                                              |
   |     <strong>Tajný klíč klienta</strong>     |                                 <em>&lt;ponechat prázdné&gt;</em>                                  |                                                                                                                                                                                                                                                                              |
   |         <strong>Rozsah</strong>         |         `https://<tenant domain name>/<api>/user_impersonation openid offline_access`         | Nahraďte <em>&lt;název domény klienta&gt;</em> s názvem domény klienta. Nahraďte <em>&lt;rozhraní api&gt;</em> s názvem projektu webového rozhraní API. Můžete taky ID aplikace. Vzor adresy URL je: <em>https://{tenant}.onmicrosoft.com/{app_name_or_id}/{scope název}</em>. |
   | <strong>Ověření klienta</strong> |                                Odeslání pověření klienta v textu                                |                                                                                                                                                                                                                                                                              |


3. Vyberte **žádosti o Token** tlačítko.

4. Postman otevře nové okno obsahující klienta Azure AD B2C přihlašovací dialogové okno. Přihlaste se pomocí existujícího účtu (Pokud je jeden vytvořený testování zásad) nebo vyberte **zaregistrujte si teď** k vytvoření nového účtu. **Zapomněli jste heslo?** odkaz se používá k obnovit zapomenuté heslo.

5. Po úspěšném přihlášení, okno se zavře a **SPRAVOVAT přístupové TOKENY** otevře se dialogové okno. Posuňte se dolů dolní a vyberte **použití tokenu** tlačítko.

    ![Kde najít tlačítko "Použití tokenu"](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>Test webové rozhraní API s ověřováním

Vyberte **odeslat** tlačítko Odeslat požadavek znovu. Tato doba, je stav odpovědi *200 OK* a datové části JSON je viditelný v odpovědi **textu** kartě.

![Stav datové části a úspěch](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli, jak:

> [!div class="checklist"]
> * Vytvoření klienta Azure Active Directory B2C.
> * Zaregistrujte webového rozhraní API v Azure AD B2C.
> * Pomocí sady Visual Studio k vytvoření webového rozhraní API nakonfigurovaný pro ověřování pomocí klienta Azure AD B2C.
> * Nakonfigurujte zásady řídí chování klienta Azure AD B2C.
> * Použití Postman k simulaci webovou aplikaci, která uvede přihlašovací dialogové okno, načte token a použije k vytvoření žádosti o proti webové rozhraní API.

Pokračujte ve vývoji metodou učení k rozhraní API:

* [Zabezpečení ASP.NET Core webovou aplikaci pomocí Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* [Volání webového rozhraní API .NET z webové aplikace .NET pomocí Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
* [Přizpůsobení uživatelského rozhraní Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Nakonfigurujte požadavky na složitost hesla](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Povolit službu Multi-Factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Nakonfigurovat další identity poskytovatelů, jako třeba [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [služby Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)a další.
* [Použijte rozhraní Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) načíst informace o dalších uživateli, jako je členství ve skupině, z klienta Azure AD B2C.
