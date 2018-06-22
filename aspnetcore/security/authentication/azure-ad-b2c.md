---
title: Ověření cloudu s Azure Active Directory B2C v ASP.NET Core
author: camsoper
description: Zjistit, jak nastavit ověřování Azure Active Directory B2C pomocí ASP.NET Core.
ms.date: 01/25/2018
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: caadeec57272ee2823452ed7c4b91e7aca07c3f4
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272419"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Ověření cloudu s Azure Active Directory B2C v ASP.NET Core

Podle [Soper kamera](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) je cloudové řešení správy identit pro webové a mobilní aplikace. Služba poskytuje ověřování pro aplikace, které jsou hostované v cloudu nebo místně. Typy ověřování zahrnují jednotlivé účty, účty sociálních sítí a federovaných účty organizace. Kromě toho Azure AD B2C můžete zadat služby Multi-Factor authentication s minimální konfigurací.

> [!TIP]
> Azure Active Directory (Azure AD) Azure AD B2C je samostatný produkt nabídky. Klient služby Azure AD představuje organizaci, zatímco klienta Azure AD B2C představuje kolekci identit pro použití s aplikacemi předávajících stran. Další informace najdete v tématu [Azure AD B2C: Nejčastější dotazy (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

V tomto kurzu, zjistěte, jak:

> [!div class="checklist"]
> * Vytvoření klienta Azure Active Directory B2C
> * Zaregistrovat aplikaci v Azure AD B2C
> * Použijte sadu Visual Studio k vytvoření webové aplikace ASP.NET Core, který je nakonfigurován pro ověřování pomocí klienta Azure AD B2C
> * Nakonfigurujte zásady řídí chování klienta Azure AD B2C

## <a name="prerequisites"></a>Požadavky

Tady jsou požadované pro Tento názorný postup:

* [Předplatné Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (všechny edice)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Vytvoření klienta Azure Active Directory B2C

Vytvoření klienta Azure Active Directory B2C [popsané v dokumentaci k](/azure/active-directory-b2c/active-directory-b2c-get-started). Po zobrazení výzvy ke klientovi přidružit předplatné Azure je volitelný pro účely tohoto kurzu.

## <a name="register-the-app-in-azure-ad-b2c"></a>Zaregistrovat aplikaci v Azure AD B2C

V nově vytvořený klienta Azure AD B2C, registrace vaší aplikace pomocí [kroky v dokumentaci k](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) pod **webové aplikaci zaregistrovat** části. Zastavit **vytvoření tajného klíče klienta webové aplikace** části. Tajný klíč klienta není povinné pro účely tohoto kurzu. 

Použijte následující hodnoty:

| Nastavení                       | Hodnota                     | Poznámky                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**                      | *&lt;Název aplikace.&gt;*        | Zadejte **název** pro aplikaci, která popisuje aplikace k příjemce.                                                                                                                                 |
| **Zahrnout webovou aplikaci / webové rozhraní API** | Ano                       |                                                                                                                                                                                                    |
| **Povolit implicitního toku**       | Ano                       |                                                                                                                                                                                                    |
| **Adresa URL odpovědi**                 | `https://localhost:44300` | Adresy URL odpovědí jsou koncové body, kde Azure AD B2C vrátí všechny tokeny, které vaše aplikace vyžaduje. Visual Studio poskytuje adresa URL odpovědi pro použití. Nyní, zadejte `https://localhost:44300` k vyplnění formuláře. |
| **Identifikátor ID URI aplikace**                | Ponechat prázdné               | Nepožaduje se pro tento kurz.                                                                                                                                                                    |
| **Zahrnout nativního klienta**     | Ne                        |                                                                                                                                                                                                    |

> [!WARNING]
> Pokud nastavení adresa URL odpovědi-místního hostitele, mějte [omezení toho, co je v seznamu Adresa URL odpovědi povoleny](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url). 

Po registraci aplikace se zobrazí seznam aplikací v klientovi. Vyberte aplikaci, který byl právě zaregistrován. Vyberte **kopie** ikony napravo **ID aplikace** pole a zkopírujte ho do schránky.

Nic informace lze nastavit v tuto chvíli v klientovi Azure AD B2C, ale nechat otevřené okno prohlížeče. Po vytvoření aplikace ASP.NET Core není další konfigurace.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Vytvoření aplikace ASP.NET Core v Visual Studio 2017

Šablony sady Visual Studio webové aplikace lze nakonfigurovat pro ověřování pomocí klienta Azure AD B2C.

V sadě Visual Studio:

1. Vytvořte novou webovou aplikaci ASP.NET Core. 
2. Vyberte **webové aplikace** ze seznamu šablon.
3. Vyberte **změna ověřování** tlačítko.
    
    ![Tlačítko Změnit ověřování](./azure-ad-b2c/_static/changeauth.png)

4. V **změna ověřování** dialogovém okně, vyberte **jednotlivé uživatelské účty**a potom vyberte **připojit k existujícímu úložišti uživatele v cloudu** v rozevírací nabídce. 
    
    ![Dialogové okno Změnit ověřování](./azure-ad-b2c/_static/changeauthdialog.png)

5. Vyplňte formulář s následujícími hodnotami:
    
    | Nastavení                       | Hodnota                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Název domény**               | *&lt;název domény vašeho klienta B2C&gt;*          |
    | **ID aplikace**            | *&lt;Vložte ID aplikace ze schránky&gt;* |
    | **Cesta zpětného volání**             | *&lt;Použijte výchozí hodnotu&gt;*                       |
    | **Zásady registrace nebo přihlášení** | `B2C_1_SiUpIn`                                        |
    | **Zásady resetování hesel**     | `B2C_1_SSPR`                                          |
    | **Upravit profil zásad**       | *&lt;Ponechat prázdné&gt;*                                 |
    
    Vyberte **kopie** vedle **URI odpovědi** identifikátor URI odpovědi zkopírovat do schránky. Vyberte **OK** zavřete **změna ověřování** dialogové okno. Vyberte **OK** k vytvoření webové aplikace.

## <a name="finish-the-b2c-app-registration"></a>Dokončit registraci aplikace B2C

Vraťte do okna prohlížeče s vlastnostmi aplikace B2C stále otevřen. Změnit dočasnou **adresa URL odpovědi** zadané hodnotě starší zkopírovat ze sady Visual Studio. Vyberte **Uložit** v horní části okna.

> [!TIP]
> Pokud zkopírujete nebyla adresa URL odpovědi, můžete pomocí SSL adresu z karty ladění ve vlastnostech projektu webové a připojit **CallbackPath** z hodnoty *appSettings.JSON určený*.

## <a name="configure-policies"></a>Konfigurace zásad

Použijte postup v dokumentaci k Azure AD B2C na [vytvořit zásadu registrace nebo přihlášení](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)a potom [vytvořit zásady resetování hesel](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy). Použít ukázkové hodnoty uvedeny v dokumentaci pro **zprostředkovatelů Identity**, **atributy registrace**, a **deklarace identity aplikace**. Pomocí **spustit nyní** tlačítko otestovat, jak je popsáno v dokumentaci je volitelné.

> [!WARNING]
> Zkontrolujte názvy zásad jsou přesně tak, jak je popsáno v dokumentaci, jak tyto zásady, které byly používány v **změna ověřování** dialogové okno v sadě Visual Studio. Názvy zásad, můžete ověřit v *appSettings.JSON určený*.

## <a name="run-the-app"></a>Spuštění aplikace

V sadě Visual Studio, stiskněte klávesu **F5** sestavení a spuštění aplikace. Po spuštění webové aplikace, vyberte **přihlášení**.

![Přihlaste se k aplikaci](./azure-ad-b2c/_static/signin.png)

Prohlížeč přesměruje klienta Azure AD B2C. Přihlaste se pomocí existujícího účtu (Pokud je jeden vytvořený testování zásad) nebo vyberte **zaregistrujte si teď** k vytvoření nového účtu. **Zapomněli jste heslo?** odkaz se používá k obnovit zapomenuté heslo.

![Přihlášení k Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

Po úspěšném přihlášení, prohlížeč přesměruje do webové aplikace.

![Úspěch](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli, jak:

> [!div class="checklist"]
> * Vytvoření klienta Azure Active Directory B2C
> * Zaregistrovat aplikaci v Azure AD B2C
> * Pomocí sady Visual Studio můžete vytvořit základní webovou aplikaci ASP.NET nakonfigurovaná pro ověřování pomocí klienta Azure AD B2C
> * Nakonfigurujte zásady řídí chování klienta Azure AD B2C

Teď, když je aplikace ASP.NET Core nakonfigurovaná pro ověřování, pomocí Azure AD B2C [autorizovat atribut](xref:security/authorization/simple) umožňuje zabezpečit vaši aplikaci. Pokračujte ve vývoji aplikace metodou učení na:

* [Přizpůsobení uživatelského rozhraní Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Nakonfigurujte požadavky na složitost hesla](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Povolit službu Multi-Factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Nakonfigurovat další identity poskytovatelů, jako třeba [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [služby Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)a další.
* [Použijte rozhraní Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) načíst informace o dalších uživateli, jako je členství ve skupině, z klienta Azure AD B2C.
* [Zabezpečení webového rozhraní API pomocí Azure AD B2C ASP.NET Core](xref:security/authentication/azure-ad-b2c-webapi).
* [Volání webového rozhraní API .NET z webové aplikace .NET pomocí Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
