---
title: Potvrzení účtu a obnovení hesla v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak vytvořit aplikaci ASP.NET Core s e-mailové potvrzení a resetováním hesla.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803269"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="c1e46-103">Potvrzení účtu a obnovení hesla v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1e46-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="c1e46-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="c1e46-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="c1e46-105">V tomto kurzu se dozvíte, jak vytvořit aplikaci ASP.NET Core s e-mailové potvrzení a resetováním hesla.</span><span class="sxs-lookup"><span data-stu-id="c1e46-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="c1e46-106">Tento kurz je **není** začátku tématu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="c1e46-107">Měli byste se seznámit s:</span><span class="sxs-lookup"><span data-stu-id="c1e46-107">You should be familiar with:</span></span>

* [<span data-ttu-id="c1e46-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1e46-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="c1e46-109">Ověřování</span><span class="sxs-lookup"><span data-stu-id="c1e46-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="c1e46-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c1e46-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="c1e46-111">Zobrazit [tento soubor PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) pro verze technologie ASP.NET Core MVC 1.1 a 2.x.</span><span class="sxs-lookup"><span data-stu-id="c1e46-111">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1e46-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c1e46-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="c1e46-113">Vytvořte nový projekt ASP.NET Core pomocí rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="c1e46-113">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c1e46-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c1e46-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="c1e46-115">`--auth Individual` Určuje šablonu projektu jednotlivé uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="c1e46-115">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="c1e46-116">Na Windows, přidejte `-uld` možnost.</span><span class="sxs-lookup"><span data-stu-id="c1e46-116">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="c1e46-117">Určuje, že by měl být LocalDB použít místo SQLite.</span><span class="sxs-lookup"><span data-stu-id="c1e46-117">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="c1e46-118">Spustit `new mvc --help` můžete zobrazit nápovědu pro tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="c1e46-118">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c1e46-119">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c1e46-119">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c1e46-120">Pokud používáte SQLite nebo rozhraní příkazového řádku, spusťte okno příkazového řádku následující:</span><span class="sxs-lookup"><span data-stu-id="c1e46-120">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="c1e46-121">`--auth Individual` Určuje šablonu projektu jednotlivé uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="c1e46-121">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="c1e46-122">Na Windows, přidejte `-uld` možnost.</span><span class="sxs-lookup"><span data-stu-id="c1e46-122">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="c1e46-123">Určuje, že by měl být LocalDB použít místo SQLite.</span><span class="sxs-lookup"><span data-stu-id="c1e46-123">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="c1e46-124">Spustit `new mvc --help` můžete zobrazit nápovědu pro tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="c1e46-124">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="c1e46-125">Alternativně můžete vytvořit nový projekt ASP.NET Core pomocí sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c1e46-125">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="c1e46-126">V sadě Visual Studio vytvořte nový **webovou aplikaci** projektu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-126">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="c1e46-127">Vyberte **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="c1e46-127">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="c1e46-128">**.NET core** je vybrána na následujícím obrázku, ale můžete vybrat **rozhraní .NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="c1e46-128">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="c1e46-129">Vyberte **změna ověřování** a nastavte **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="c1e46-129">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="c1e46-130">Ponechte výchozí **Store uživatelské účty v aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="c1e46-130">Keep the default **Store user accounts in-app**.</span></span>

![Dialogové okno Nový projekt zobrazuje "Jednotlivých uživatelských účtů přepínač" vybraná](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="c1e46-132">Otestovat nové registrace uživatele</span><span class="sxs-lookup"><span data-stu-id="c1e46-132">Test new user registration</span></span>

<span data-ttu-id="c1e46-133">Spusťte aplikaci, vyberte **zaregistrovat** propojit a zaregistrovat uživatele.</span><span class="sxs-lookup"><span data-stu-id="c1e46-133">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="c1e46-134">Postupujte podle pokynů pro spuštění migrace Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c1e46-134">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="c1e46-135">V tomto okamžiku je pouze ověření na e-mailu [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="c1e46-135">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="c1e46-136">Po odeslání registrace, jste přihlášení do aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1e46-136">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="c1e46-137">Později v tomto kurzu se kód aktualizuje tak, že noví uživatelé nemohou přihlásit, dokud e-mailu je potvrzená.</span><span class="sxs-lookup"><span data-stu-id="c1e46-137">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="c1e46-138">Zobrazení databáze identit</span><span class="sxs-lookup"><span data-stu-id="c1e46-138">View the Identity database</span></span>

<span data-ttu-id="c1e46-139">Zobrazit [práce s SQLite v projektu aplikace ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) pokyny o tom, jak zobrazit databázi SQLite.</span><span class="sxs-lookup"><span data-stu-id="c1e46-139">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="c1e46-140">Pro Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c1e46-140">For Visual Studio:</span></span>

* <span data-ttu-id="c1e46-141">Z **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="c1e46-141">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="c1e46-142">Přejděte do **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="c1e46-142">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="c1e46-143">Klikněte pravým tlačítkem na **dbo. AspNetUsers** > **zobrazení dat**:</span><span class="sxs-lookup"><span data-stu-id="c1e46-143">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Místní nabídku pro tabulku AspNetUsers v Průzkumníku objektů SQL serveru](accconfirm/_static/ssox.png)

<span data-ttu-id="c1e46-145">Poznámka: v tabulce `EmailConfirmed` pole je `False`.</span><span class="sxs-lookup"><span data-stu-id="c1e46-145">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="c1e46-146">Můžete chtít tento e-mail znovu použít v dalším kroku při ní odešle e-mail s potvrzením.</span><span class="sxs-lookup"><span data-stu-id="c1e46-146">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="c1e46-147">Klikněte pravým tlačítkem na řádek a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c1e46-147">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="c1e46-148">Odstraňuje se e-mailový alias usnadňuje v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="c1e46-148">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="c1e46-149">Vyžadovat protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="c1e46-149">Require HTTPS</span></span>

<span data-ttu-id="c1e46-150">Zobrazit [vyžadovalo protokol HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="c1e46-150">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="c1e46-151">Vyžádání potvrzení e-mailu</span><span class="sxs-lookup"><span data-stu-id="c1e46-151">Require email confirmation</span></span>

<span data-ttu-id="c1e46-152">Je osvědčeným postupem je potvrďte e-mailu nové registrace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c1e46-152">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="c1e46-153">E-mailu pomáhá potvrzení ověření někdo jiný, nejsou zosobnění (to znamená, že se ještě nezaregistrovali někoho jiného e-mailu).</span><span class="sxs-lookup"><span data-stu-id="c1e46-153">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="c1e46-154">Předpokládejme, že jste měli diskusní fórum, a chtěli byste zabránit "yli@example.com"registroval jako"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="c1e46-154">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="c1e46-155">Bez potvrzení e-mailu "nolivetto@contoso.com" mohli dostávat nežádoucí e-mailu z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1e46-155">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="c1e46-156">Předpokládejme, že uživatel zaregistrován nechtěně jako "ylo@example.com" a kdyby si všimli chyba "yli".</span><span class="sxs-lookup"><span data-stu-id="c1e46-156">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="c1e46-157">Se nebude moci použít obnovení hesla, protože aplikace nemá správnou e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-157">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="c1e46-158">Potvrzení e-mailu zajišťuje pouze omezenou ochranu před roboty.</span><span class="sxs-lookup"><span data-stu-id="c1e46-158">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="c1e46-159">Potvrzení e-mailu neposkytuje ochranu z uživateli se zlými úmysly s mnoha e-mailové účty.</span><span class="sxs-lookup"><span data-stu-id="c1e46-159">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="c1e46-160">Obvykle chcete novým uživatelům zabránit v účtování všechna data na webový server dříve, než potvrzeno e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-160">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="c1e46-161">Aktualizace `ConfigureServices` tak, aby vyžadovala potvrzeno e-mailu:</span><span class="sxs-lookup"><span data-stu-id="c1e46-161">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="c1e46-162">`config.SignIn.RequireConfirmedEmail = true;` zabraňuje registrovaných uživatelů přihlásit, dokud není potvrzené e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-162">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="c1e46-163">Konfigurace poskytovatele e-mailu</span><span class="sxs-lookup"><span data-stu-id="c1e46-163">Configure email provider</span></span>

<span data-ttu-id="c1e46-164">V tomto kurzu SendGrid umožňuje odesílání e-mailů.</span><span class="sxs-lookup"><span data-stu-id="c1e46-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="c1e46-165">Potřebujete účet SendGrid a klíč k odesílání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="c1e46-166">Můžete použít jiné poskytovateli e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-166">You can use other email providers.</span></span> <span data-ttu-id="c1e46-167">ASP.NET Core 2.x zahrnuje `System.Net.Mail`, který umožňuje odeslání e-mailu z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1e46-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="c1e46-168">Doporučujeme že použít SendGrid nebo jiné služby e-mailu k odeslání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="c1e46-169">SMTP je obtížné zabezpečení a zařídit správné nastavení.</span><span class="sxs-lookup"><span data-stu-id="c1e46-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="c1e46-170">[Možnosti vzor](xref:fundamentals/configuration/options) slouží k přístupu k účtu a klíč nastavení.</span><span class="sxs-lookup"><span data-stu-id="c1e46-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="c1e46-171">Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="c1e46-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="c1e46-172">Vytvoření třídy k načtení klíče zabezpečeného e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="c1e46-173">V tomto příkladu `AuthMessageSenderOptions` třída se vytvoří v *Services/AuthMessageSenderOptions.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="c1e46-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="c1e46-174">Nastavte `SendGridUser` a `SendGridKey` s [manažera tajných nástroj](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="c1e46-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="c1e46-175">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c1e46-175">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="c1e46-176">Na Windows, manažera tajných ukládá dvojice klíčů/hodnota v *secrets.json* soubor `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` adresáře.</span><span class="sxs-lookup"><span data-stu-id="c1e46-176">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="c1e46-177">Obsah *secrets.json* souboru nejsou šifrovány.</span><span class="sxs-lookup"><span data-stu-id="c1e46-177">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="c1e46-178">*Secrets.json* souboru je uveden níže ( `SendGridKey` hodnota byla odebrána.)</span><span class="sxs-lookup"><span data-stu-id="c1e46-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="c1e46-179">Konfigurace spuštění používat AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="c1e46-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="c1e46-180">Přidat `AuthMessageSenderOptions` do služby kontejneru na konci `ConfigureServices` metodu *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="c1e46-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c1e46-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c1e46-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c1e46-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c1e46-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="c1e46-183">Konfigurovat třídu AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="c1e46-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="c1e46-184">Tento kurz ukazuje, jak přidat e-mailová oznámení prostřednictvím [SendGrid](https://sendgrid.com/), ale můžete odesílat e-mailu pomocí protokolu SMTP a další mechanismy.</span><span class="sxs-lookup"><span data-stu-id="c1e46-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="c1e46-185">Nainstalujte `SendGrid` balíček NuGet:</span><span class="sxs-lookup"><span data-stu-id="c1e46-185">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="c1e46-186">Z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="c1e46-186">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="c1e46-187">V konzole Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c1e46-187">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="c1e46-188">Naleznete v tématu [začít pomocí Sendgridu zdarma](https://sendgrid.com/free/) k registraci bezplatného účtu SendGrid.</span><span class="sxs-lookup"><span data-stu-id="c1e46-188">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="c1e46-189">Konfigurace služby SendGrid</span><span class="sxs-lookup"><span data-stu-id="c1e46-189">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c1e46-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c1e46-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="c1e46-191">Ke konfiguraci služby SendGrid, přidejte kód, podobně jako v následujícím *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="c1e46-191">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c1e46-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c1e46-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="c1e46-193">Přidejte kód do *Services/MessageServices.cs* podobně jako následující konfigurace SendGrid:</span><span class="sxs-lookup"><span data-stu-id="c1e46-193">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="c1e46-194">Povolit obnovení hesla a potvrzení účtu</span><span class="sxs-lookup"><span data-stu-id="c1e46-194">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="c1e46-195">Šablona má kód pro obnovení potvrzení a heslo účtu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-195">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="c1e46-196">Najít `OnPostAsync` metoda *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="c1e46-196">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c1e46-197">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c1e46-197">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="c1e46-198">Nově zaregistrovaný uživatelům zabránit v automaticky přihlášeni tak následující řádek:</span><span class="sxs-lookup"><span data-stu-id="c1e46-198">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="c1e46-199">Úplná metoda se zobrazí s změněný řádek zvýrazněný:</span><span class="sxs-lookup"><span data-stu-id="c1e46-199">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c1e46-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c1e46-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="c1e46-201">Chcete-li povolit potvrzení účtu, Odkomentujte následující kód:</span><span class="sxs-lookup"><span data-stu-id="c1e46-201">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="c1e46-202">**Poznámka:** kód brání nově zaregistrovaný uživatel automaticky přihlášeni tak následující řádek:</span><span class="sxs-lookup"><span data-stu-id="c1e46-202">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="c1e46-203">Povolit obnovení hesla uncommenting kód `ForgotPassword` akce *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="c1e46-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="c1e46-204">Zrušením komentáře u elementu form v *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c1e46-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="c1e46-205">Můžete chtít odebrat `<p> For more information on how to enable reset password ... </p>` element, který obsahuje odkaz na tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="c1e46-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="c1e46-206">Zaregistrovat a potvrďte e-mailu a resetování hesla</span><span class="sxs-lookup"><span data-stu-id="c1e46-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="c1e46-207">Spuštění webové aplikace a testů potvrzení účtu a heslo pro obnovení toku.</span><span class="sxs-lookup"><span data-stu-id="c1e46-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="c1e46-208">Spusťte aplikaci a zaregistrovat nový uživatel</span><span class="sxs-lookup"><span data-stu-id="c1e46-208">Run the app and register a new user</span></span>

  ![Zobrazení účtu zaregistrovat webové aplikace](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="c1e46-210">Zkontrolujte svého e-mailu na odkaz pro potvrzení účtu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="c1e46-211">Zobrazit [ladění e-mailu](#debug) Pokud neobdržíte e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="c1e46-212">Klikněte na odkaz pro potvrzení e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="c1e46-213">Přihlaste se pomocí e-mailu a hesla.</span><span class="sxs-lookup"><span data-stu-id="c1e46-213">Log in with your email and password.</span></span>
* <span data-ttu-id="c1e46-214">Odhlaste se.</span><span class="sxs-lookup"><span data-stu-id="c1e46-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="c1e46-215">Zobrazení stránky Správa</span><span class="sxs-lookup"><span data-stu-id="c1e46-215">View the manage page</span></span>

<span data-ttu-id="c1e46-216">Vyberte své uživatelské jméno v prohlížeči: ![okna prohlížeče s uživatelským jménem](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="c1e46-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="c1e46-217">Potřebujete rozšířit na navigačním panelu zobrazíte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="c1e46-217">You might need to expand the navbar to see user name.</span></span>

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c1e46-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c1e46-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c1e46-220">Zobrazí se stránka Správa s **profilu** vybraná karta.</span><span class="sxs-lookup"><span data-stu-id="c1e46-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="c1e46-221">**E-mailu** zobrazí zaškrtávací políčko označující e-mailu byl potvrzen.</span><span class="sxs-lookup"><span data-stu-id="c1e46-221">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![Stránka Správa](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c1e46-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c1e46-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c1e46-224">To je uvedeno v pozdější části kurzu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-224">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="c1e46-225">![Stránka Správa](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="c1e46-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="c1e46-226">Resetování hesla testu</span><span class="sxs-lookup"><span data-stu-id="c1e46-226">Test password reset</span></span>

* <span data-ttu-id="c1e46-227">Pokud jste přihlášeni, vyberte **odhlášení**.</span><span class="sxs-lookup"><span data-stu-id="c1e46-227">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="c1e46-228">Vyberte **přihlášení** spojit a vybrat možnost **zapomněli jste heslo?** odkaz.</span><span class="sxs-lookup"><span data-stu-id="c1e46-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="c1e46-229">Zadejte e-mail, který jste použili k registraci účtu.</span><span class="sxs-lookup"><span data-stu-id="c1e46-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="c1e46-230">Odešle e-mail s odkazem k resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="c1e46-230">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="c1e46-231">Zkontrolujte e-mailu a klikněte na odkaz pro resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="c1e46-231">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="c1e46-232">Po úspěšném resetování vašeho hesla můžete přihlásit e-mailu a nové heslo.</span><span class="sxs-lookup"><span data-stu-id="c1e46-232">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="c1e46-233">Ladění e-mailu</span><span class="sxs-lookup"><span data-stu-id="c1e46-233">Debug email</span></span>

<span data-ttu-id="c1e46-234">Pokud nelze získat pracovní e-mailu:</span><span class="sxs-lookup"><span data-stu-id="c1e46-234">If you can't get email working:</span></span>

* <span data-ttu-id="c1e46-235">Vytvoření [konzolovou aplikaci k odesílání e-mailu](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="c1e46-235">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="c1e46-236">Zkontrolujte [e-mailové aktivity](https://sendgrid.com/docs/User_Guide/email_activity.html) stránky.</span><span class="sxs-lookup"><span data-stu-id="c1e46-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="c1e46-237">Zkontrolujte složku s nevyžádanou poštou.</span><span class="sxs-lookup"><span data-stu-id="c1e46-237">Check your spam folder.</span></span>
* <span data-ttu-id="c1e46-238">Zkuste jinou e-mailový alias na jinou e-mailovou zprostředkovatele (Microsoft, Yahoo, Gmail, atd.)</span><span class="sxs-lookup"><span data-stu-id="c1e46-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="c1e46-239">Pokuste se odeslat na jiné e-mailové účty.</span><span class="sxs-lookup"><span data-stu-id="c1e46-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="c1e46-240">**Z bezpečnostních důvodů** je **není** použití produkční tajných kódů v vývoj a testování.</span><span class="sxs-lookup"><span data-stu-id="c1e46-240">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="c1e46-241">Pokud publikujete aplikaci do Azure, můžete nastavit SendGrid tajné kódy jako nastavení aplikace ve webové aplikaci Azure portal.</span><span class="sxs-lookup"><span data-stu-id="c1e46-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="c1e46-242">Konfigurační systém je nastavený na klíče pro čtení z proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="c1e46-242">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="c1e46-243">Sloučit účty sociálních sítí a místní přihlášení</span><span class="sxs-lookup"><span data-stu-id="c1e46-243">Combine social and local login accounts</span></span>

<span data-ttu-id="c1e46-244">K dokončení této části, je nutné nejprve povolit externí zprostředkovatel ověřování.</span><span class="sxs-lookup"><span data-stu-id="c1e46-244">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="c1e46-245">Zobrazit [Facebook, Google a externí zprostředkovatel ověřování](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="c1e46-245">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="c1e46-246">Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociální účty.</span><span class="sxs-lookup"><span data-stu-id="c1e46-246">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="c1e46-247">V tomto pořadí "RickAndMSFT@gmail.com" se nejprve vytvoří jako místní přihlášení; však můžete nejprve vytvořte účet jako přihlášení prostřednictvím sociální sítě a pak přidat místní přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="c1e46-247">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Webová aplikace: RickAndMSFT@gmail.com uživatel byl ověřen](accconfirm/_static/rick.png)

<span data-ttu-id="c1e46-249">Klikněte na **spravovat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="c1e46-249">Click on the **Manage** link.</span></span> <span data-ttu-id="c1e46-250">Poznámka: externí 0 (přihlašování přes sociální sítě) spojená s tímto účtem.</span><span class="sxs-lookup"><span data-stu-id="c1e46-250">Note the 0 external (social logins) associated with this account.</span></span>

![Správa zobrazení](accconfirm/_static/manage.png)

<span data-ttu-id="c1e46-252">Klikněte na odkaz pro další přihlášení služby a přijímání požadavků aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1e46-252">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="c1e46-253">Na následujícím obrázku je Facebooku zprostředkovatele externího ověřování:</span><span class="sxs-lookup"><span data-stu-id="c1e46-253">In the following image, Facebook is the external authentication provider:</span></span>

![Spravovat externí přihlášení zobrazení výpisu Facebooku](accconfirm/_static/fb.png)

<span data-ttu-id="c1e46-255">Byli sloučeni dva účty.</span><span class="sxs-lookup"><span data-stu-id="c1e46-255">The two accounts have been combined.</span></span> <span data-ttu-id="c1e46-256">Budete moct přihlásit pomocí obou.</span><span class="sxs-lookup"><span data-stu-id="c1e46-256">You are able to log on with either account.</span></span> <span data-ttu-id="c1e46-257">Můžete chtít uživatelům přidat místní účty v případě nefungující Služba ověřování v jejich přihlášení prostřednictvím sociální sítě nebo spíše se jste ztratili přístup k jejich účtu na sociální síti.</span><span class="sxs-lookup"><span data-stu-id="c1e46-257">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="c1e46-258">Po lokality má uživatelům povolit potvrzení účtu</span><span class="sxs-lookup"><span data-stu-id="c1e46-258">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="c1e46-259">Povolení potvrzení účtu na webu s uživateli zamezí všichni stávající uživatelé.</span><span class="sxs-lookup"><span data-stu-id="c1e46-259">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="c1e46-260">Stávající uživatelé jsou uzamčen, protože jejich účty nejsou potvrzeny.</span><span class="sxs-lookup"><span data-stu-id="c1e46-260">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="c1e46-261">Obejít existující uzamčení uživatelů, použijte jednu z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="c1e46-261">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="c1e46-262">Aktualizujte databázi pro označení všichni stávající uživatelé, jako je potvrzen.</span><span class="sxs-lookup"><span data-stu-id="c1e46-262">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="c1e46-263">Zkontrolujte stávající uživatele.</span><span class="sxs-lookup"><span data-stu-id="c1e46-263">Confirm exiting users.</span></span> <span data-ttu-id="c1e46-264">Třeba dávkové odeslání e-mailů s potvrzovací odkazy.</span><span class="sxs-lookup"><span data-stu-id="c1e46-264">For example, batch-send emails with confirmation links.</span></span>
