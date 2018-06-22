---
title: Potvrzení účtu a obnovení hesla v ASP.NET Core
author: rick-anderson
description: Naučte se vytvářet aplikace ASP.NET Core pomocí e-mailu potvrzení a heslo resetovat.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: db41dd47518fa8b35c006b3291068e7724cf6cca
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275077"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="770cb-103">Potvrzení účtu a obnovení hesla v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="770cb-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="770cb-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="770cb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="770cb-105">Tento kurz ukazuje, jak vytvořit aplikaci ASP.NET Core pomocí e-mailu potvrzení a heslo resetovat.</span><span class="sxs-lookup"><span data-stu-id="770cb-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="770cb-106">Tento kurz je určen **není** začátku tématu.</span><span class="sxs-lookup"><span data-stu-id="770cb-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="770cb-107">Měli byste se seznámit s:</span><span class="sxs-lookup"><span data-stu-id="770cb-107">You should be familiar with:</span></span>

* [<span data-ttu-id="770cb-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="770cb-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="770cb-109">Ověřování</span><span class="sxs-lookup"><span data-stu-id="770cb-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="770cb-110">Potvrzení účtu a obnovení hesla</span><span class="sxs-lookup"><span data-stu-id="770cb-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="770cb-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="770cb-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="770cb-112">V tématu [tento PDF soubor](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) pro verze ASP.NET Core MVC 1.1 a 2.x.</span><span class="sxs-lookup"><span data-stu-id="770cb-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="770cb-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="770cb-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="770cb-114">Vytvořte nový projekt ASP.NET Core pomocí rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="770cb-114">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="770cb-115">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="770cb-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

* <span data-ttu-id="770cb-117">`--auth Individual` Určuje šablonu projektu na jednotlivé uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="770cb-117">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="770cb-118">V systému Windows, přidejte `-uld` možnost.</span><span class="sxs-lookup"><span data-stu-id="770cb-118">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="770cb-119">Určuje, že místo SQLite by použít LocalDB.</span><span class="sxs-lookup"><span data-stu-id="770cb-119">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="770cb-120">Spustit `new mvc --help` získání nápovědy na tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="770cb-120">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="770cb-121">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="770cb-121">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="770cb-122">Pokud používáte rozhraní příkazového řádku nebo SQLite, spusťte následující příkazy v příkazovém okně:</span><span class="sxs-lookup"><span data-stu-id="770cb-122">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="770cb-123">`--auth Individual` Určuje šablonu projektu na jednotlivé uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="770cb-123">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="770cb-124">V systému Windows, přidejte `-uld` možnost.</span><span class="sxs-lookup"><span data-stu-id="770cb-124">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="770cb-125">Určuje, že místo SQLite by použít LocalDB.</span><span class="sxs-lookup"><span data-stu-id="770cb-125">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="770cb-126">Spustit `new mvc --help` získání nápovědy na tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="770cb-126">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="770cb-127">Alternativně můžete vytvořit nový projekt ASP.NET Core pomocí sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="770cb-127">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="770cb-128">V sadě Visual Studio vytvořte novou **webové aplikace** projektu.</span><span class="sxs-lookup"><span data-stu-id="770cb-128">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="770cb-129">Vyberte **jádro ASP.NET 2.0**.</span><span class="sxs-lookup"><span data-stu-id="770cb-129">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="770cb-130">**.NET core** je vybrána na následujícím obrázku, ale můžete vybrat **rozhraní .NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="770cb-130">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="770cb-131">Vyberte **změna ověřování** a nastavte na **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="770cb-131">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="770cb-132">Ponechte výchozí **úložiště uživatelských účtů v aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="770cb-132">Keep the default **Store user accounts in-app**.</span></span>

![Dialogové okno Nový projekt zobrazující "Jednotlivých uživatelských účtů radio" vybrali](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="770cb-134">Otestovat novou registraci uživatele</span><span class="sxs-lookup"><span data-stu-id="770cb-134">Test new user registration</span></span>

<span data-ttu-id="770cb-135">Spuštění aplikace, vyberte **zaregistrovat** propojit a zaregistrovat uživatele.</span><span class="sxs-lookup"><span data-stu-id="770cb-135">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="770cb-136">Postupujte podle pokynů ke spuštění migrace Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="770cb-136">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="770cb-137">V tomto okamžiku je pouze ověření na e-mailu [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="770cb-137">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="770cb-138">Po odeslání registrace, jste přihlášení do aplikace.</span><span class="sxs-lookup"><span data-stu-id="770cb-138">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="770cb-139">Později v tomto kurzu se kód aktualizuje, nelze noví uživatelé přihlásit, dokud ověřena e-mailu.</span><span class="sxs-lookup"><span data-stu-id="770cb-139">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="770cb-140">Zobrazení Identity databáze</span><span class="sxs-lookup"><span data-stu-id="770cb-140">View the Identity database</span></span>

<span data-ttu-id="770cb-141">V tématu [pracovat s SQLite v projektu ASP.NET MVC základní](xref:tutorials/first-mvc-app-xplat/working-with-sql) pokyny o tom, jak zobrazit databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="770cb-141">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="770cb-142">Pro sadu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="770cb-142">For Visual Studio:</span></span>

* <span data-ttu-id="770cb-143">Z **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="770cb-143">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="770cb-144">Přejděte na **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="770cb-144">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="770cb-145">Klikněte pravým tlačítkem na **dbo. AspNetUsers** > **zobrazení dat**:</span><span class="sxs-lookup"><span data-stu-id="770cb-145">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Kontextové nabídky pro tabulku AspNetUsers v Průzkumníku objektů systému SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="770cb-147">Poznámka: v tabulce `EmailConfirmed` pole je `False`.</span><span class="sxs-lookup"><span data-stu-id="770cb-147">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="770cb-148">Můžete chtít tento e-mail znovu použít v dalším kroku při aplikace odešle e-mail s potvrzením.</span><span class="sxs-lookup"><span data-stu-id="770cb-148">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="770cb-149">Klikněte pravým tlačítkem myši na řádek a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="770cb-149">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="770cb-150">Odstranění e-mailový alias usnadní v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="770cb-150">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="770cb-151">Vyžadovat protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="770cb-151">Require HTTPS</span></span>

<span data-ttu-id="770cb-152">V tématu [vyžadují protokol HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="770cb-152">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="770cb-153">Požadovat potvrzení e-mailu</span><span class="sxs-lookup"><span data-stu-id="770cb-153">Require email confirmation</span></span>

<span data-ttu-id="770cb-154">Je osvědčeným postupem potvrďte e-mailu nové registrace uživatele.</span><span class="sxs-lookup"><span data-stu-id="770cb-154">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="770cb-155">E-mailem potvrzení pomáhá ověřte, že nejsou zosobnění někdo jiný (to znamená, že nebyly zaregistrována někoho jiného e-mailu).</span><span class="sxs-lookup"><span data-stu-id="770cb-155">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="770cb-156">Předpokládejme, že jste měli diskusní fórum, a chcete zabránit "yli@example.com"od registrace jako"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="770cb-156">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="770cb-157">Bez potvrzení e-mailu "nolivetto@contoso.com" může přijímat nežádoucí e-mailu z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="770cb-157">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="770cb-158">Předpokládejme, že uživatel omylem zaregistrován jako "ylo@example.com" a kdyby zaznamenali chyby v pravopisu systému "yli".</span><span class="sxs-lookup"><span data-stu-id="770cb-158">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="770cb-159">Se nebudou moci používat obnovení hesla, protože aplikace nemá správnou e-mailovou.</span><span class="sxs-lookup"><span data-stu-id="770cb-159">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="770cb-160">Potvrzení e-mailu poskytuje jen omezenou ochrany ze robotů.</span><span class="sxs-lookup"><span data-stu-id="770cb-160">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="770cb-161">Potvrzení e-mailu neposkytuje ochranu z uživatelé se zlými úmysly s mnoha e-mailové účty.</span><span class="sxs-lookup"><span data-stu-id="770cb-161">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="770cb-162">Obvykle budete chtít zabránit noví uživatelé publikování všechna data na webové stránky, než budou mít potvrzené e-mailu.</span><span class="sxs-lookup"><span data-stu-id="770cb-162">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="770cb-163">Aktualizace `ConfigureServices` tak, aby vyžadovala potvrzen e-mailu:</span><span class="sxs-lookup"><span data-stu-id="770cb-163">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="770cb-164">`config.SignIn.RequireConfirmedEmail = true;` zabraňuje registrovaní uživatelé přihlásit, dokud je potvrzen e-mailu.</span><span class="sxs-lookup"><span data-stu-id="770cb-164">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="770cb-165">Nakonfigurujte poskytovatele tak e-mailu</span><span class="sxs-lookup"><span data-stu-id="770cb-165">Configure email provider</span></span>

<span data-ttu-id="770cb-166">V tomto kurzu se sendgrid vám umožňuje používá k odesílání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="770cb-166">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="770cb-167">Musíte sendgrid vám umožňuje účtu a klíč k odeslání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="770cb-167">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="770cb-168">Můžete vytvořit další poskytovatele e-mailu.</span><span class="sxs-lookup"><span data-stu-id="770cb-168">You can use other email providers.</span></span> <span data-ttu-id="770cb-169">ASP.NET Core 2.x zahrnuje `System.Net.Mail`, který umožňuje odeslat e-mailu z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="770cb-169">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="770cb-170">Doporučujeme, aby že použití sendgrid vám umožňuje nebo jinou e-mailovou službu pro odeslání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="770cb-170">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="770cb-171">SMTP je obtížné zabezpečení a nastavit správně.</span><span class="sxs-lookup"><span data-stu-id="770cb-171">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="770cb-172">[Možnosti vzor](xref:fundamentals/configuration/options) se používá pro přístup k účtu a klíč nastavení uživatele.</span><span class="sxs-lookup"><span data-stu-id="770cb-172">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="770cb-173">Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="770cb-173">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="770cb-174">Vytvořte třídu načíst klíč zabezpečení e-mailu.</span><span class="sxs-lookup"><span data-stu-id="770cb-174">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="770cb-175">Tato ukázka `AuthMessageSenderOptions` je v vytvořit třídu *Services/AuthMessageSenderOptions.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="770cb-175">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="770cb-176">Nastavte `SendGridUser` a `SendGridKey` s [nástroj tajný klíč správce](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="770cb-176">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="770cb-177">Příklad:</span><span class="sxs-lookup"><span data-stu-id="770cb-177">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="770cb-178">V systému Windows, tajný klíč správce ukládá páry klíčů/hodnota v *secrets.json* v soubor `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` adresáře.</span><span class="sxs-lookup"><span data-stu-id="770cb-178">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="770cb-179">Obsah *secrets.json* soubor není zašifrován.</span><span class="sxs-lookup"><span data-stu-id="770cb-179">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="770cb-180">*Secrets.json* souboru je uveden níže ( `SendGridKey` hodnota byla odebrána.)</span><span class="sxs-lookup"><span data-stu-id="770cb-180">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="770cb-181">Konfigurace spuštění používat AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="770cb-181">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="770cb-182">Přidat `AuthMessageSenderOptions` ke kontejneru služby na konci `ConfigureServices` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="770cb-182">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="770cb-183">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="770cb-183">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="770cb-184">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="770cb-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="770cb-185">Konfigurovat třídu AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="770cb-185">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="770cb-186">Tento kurz ukazuje, jak přidat e-mailová oznámení prostřednictvím [sendgrid vám umožňuje](https://sendgrid.com/), ale můžete odesílat e-mailu pomocí protokolu SMTP a další mechanismy.</span><span class="sxs-lookup"><span data-stu-id="770cb-186">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="770cb-187">Nainstalujte `SendGrid` balíček NuGet:</span><span class="sxs-lookup"><span data-stu-id="770cb-187">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="770cb-188">Z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="770cb-188">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="770cb-189">Z konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="770cb-189">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="770cb-190">V tématu [začněte sendgridu zadarmo](https://sendgrid.com/free/) zaregistrovat bezplatný účet sendgrid vám umožňuje.</span><span class="sxs-lookup"><span data-stu-id="770cb-190">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="770cb-191">Konfigurace sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="770cb-191">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="770cb-192">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="770cb-192">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="770cb-193">Ke konfiguraci Sendgridu, přidejte kód podobný následujícímu v *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="770cb-193">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="770cb-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="770cb-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="770cb-195">Přidejte kód v *Services/MessageServices.cs* podobný následujícímu konfigurace sendgrid vám umožňuje:</span><span class="sxs-lookup"><span data-stu-id="770cb-195">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="770cb-196">Povolit obnovení potvrzení a heslo účtu</span><span class="sxs-lookup"><span data-stu-id="770cb-196">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="770cb-197">Šablona má kód pro obnovení potvrzení a heslo účtu.</span><span class="sxs-lookup"><span data-stu-id="770cb-197">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="770cb-198">Najít `OnPostAsync` metoda v *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="770cb-198">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="770cb-199">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="770cb-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="770cb-200">Nově zaregistrovaný uživatelům zabránit v automaticky přihlášený při psaní komentářů následující řádek:</span><span class="sxs-lookup"><span data-stu-id="770cb-200">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="770cb-201">Kompletní metoda je zobrazena změněné řádek zvýrazněna:</span><span class="sxs-lookup"><span data-stu-id="770cb-201">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="770cb-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="770cb-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="770cb-203">Chcete-li povolit potvrzení účtu, zrušte komentář u následující kód:</span><span class="sxs-lookup"><span data-stu-id="770cb-203">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="770cb-204">**Poznámka:** kód znemožňuje nově zaregistrovaný uživatele automaticky přihlášený při psaní komentářů následující řádek:</span><span class="sxs-lookup"><span data-stu-id="770cb-204">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="770cb-205">Povolit obnovení hesla uncommenting kód `ForgotPassword` akce *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="770cb-205">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="770cb-206">Zrušením komentáře u prvku formuláře v *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="770cb-206">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="770cb-207">Můžete chtít odebrat `<p> For more information on how to enable reset password ... </p>` element, který obsahuje odkaz na tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="770cb-207">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="770cb-208">Zaregistrovat, potvrďte e-mailu a resetování hesla</span><span class="sxs-lookup"><span data-stu-id="770cb-208">Register, confirm email, and reset password</span></span>

<span data-ttu-id="770cb-209">Spusťte webovou aplikaci a testování potvrzení účtu a heslo pro obnovení toku.</span><span class="sxs-lookup"><span data-stu-id="770cb-209">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="770cb-210">Spusťte aplikaci a zaregistrovat nového uživatele</span><span class="sxs-lookup"><span data-stu-id="770cb-210">Run the app and register a new user</span></span>

  ![Zobrazení zaregistrovat účet webové aplikace](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="770cb-212">Zkontrolujte e-mailu pro potvrzení propojení účtu.</span><span class="sxs-lookup"><span data-stu-id="770cb-212">Check your email for the account confirmation link.</span></span> <span data-ttu-id="770cb-213">V tématu [ladění e-mailu](#debug) Pokud neobdržíte e-mail.</span><span class="sxs-lookup"><span data-stu-id="770cb-213">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="770cb-214">Kliknutím na odkaz k potvrzení e-mailu.</span><span class="sxs-lookup"><span data-stu-id="770cb-214">Click the link to confirm your email.</span></span>
* <span data-ttu-id="770cb-215">Přihlaste se pomocí vaší e-mailu a heslo.</span><span class="sxs-lookup"><span data-stu-id="770cb-215">Log in with your email and password.</span></span>
* <span data-ttu-id="770cb-216">Odhlaste se.</span><span class="sxs-lookup"><span data-stu-id="770cb-216">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="770cb-217">Zobrazení stránky Správa</span><span class="sxs-lookup"><span data-stu-id="770cb-217">View the manage page</span></span>

<span data-ttu-id="770cb-218">Vyberte jméno uživatele v prohlížeči: ![okno prohlížeče s uživatelským jménem](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="770cb-218">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="770cb-219">Možná budete muset Rozbalit navigační panel zobrazíte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="770cb-219">You might need to expand the navbar to see user name.</span></span>

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="770cb-221">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="770cb-221">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="770cb-222">Zobrazí se stránka Správa s **profil** vybrána karta.</span><span class="sxs-lookup"><span data-stu-id="770cb-222">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="770cb-223">**E-mailu** zobrazí zaškrtávací políčko označující e-mailu, bylo potvrzeno.</span><span class="sxs-lookup"><span data-stu-id="770cb-223">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![Stránka Správa](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="770cb-225">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="770cb-225">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="770cb-226">To je uvedeno dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="770cb-226">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="770cb-227">![Stránka Správa](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="770cb-227">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="770cb-228">Test resetování hesla</span><span class="sxs-lookup"><span data-stu-id="770cb-228">Test password reset</span></span>

* <span data-ttu-id="770cb-229">Pokud jste přihlášeni, vyberte **odhlášení**.</span><span class="sxs-lookup"><span data-stu-id="770cb-229">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="770cb-230">Vyberte **přihlásit** propojení a vyberte **zapomněli jste heslo?** odkaz.</span><span class="sxs-lookup"><span data-stu-id="770cb-230">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="770cb-231">Zadejte e-mailu, které jste použili k registraci účtu.</span><span class="sxs-lookup"><span data-stu-id="770cb-231">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="770cb-232">Se odeslal e-mail s odkazem pro resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="770cb-232">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="770cb-233">Zkontrolujte e-mailu a klikněte na odkaz k resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="770cb-233">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="770cb-234">Po vaše heslo bylo resetováno úspěšně, můžete přihlásit e-mailu a nové heslo.</span><span class="sxs-lookup"><span data-stu-id="770cb-234">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="770cb-235">Ladění e-mailu</span><span class="sxs-lookup"><span data-stu-id="770cb-235">Debug email</span></span>

<span data-ttu-id="770cb-236">Pokud nelze získat pracovní e-mailu:</span><span class="sxs-lookup"><span data-stu-id="770cb-236">If you can't get email working:</span></span>

* <span data-ttu-id="770cb-237">Vytvoření [konzolovou aplikaci k odesílání e-mailu](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="770cb-237">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="770cb-238">Zkontrolujte [e-mailu aktivity](https://sendgrid.com/docs/User_Guide/email_activity.html) stránky.</span><span class="sxs-lookup"><span data-stu-id="770cb-238">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="770cb-239">Zkontrolujte složky nevyžádané pošty.</span><span class="sxs-lookup"><span data-stu-id="770cb-239">Check your spam folder.</span></span>
* <span data-ttu-id="770cb-240">Zkuste jinou e-mailový alias na jinou e-mailovou zprostředkovatele (Microsoft, Yahoo, z Gmailu atd.)</span><span class="sxs-lookup"><span data-stu-id="770cb-240">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="770cb-241">Pokus o odeslání na jiné e-mailové účty.</span><span class="sxs-lookup"><span data-stu-id="770cb-241">Try sending to different email accounts.</span></span>

<span data-ttu-id="770cb-242">**Nejlepším postupem zabezpečení** je **není** použít produkční tajných klíčů v testovacích a vývojových.</span><span class="sxs-lookup"><span data-stu-id="770cb-242">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="770cb-243">Pokud publikujete aplikaci do Azure, můžete tajné klíče sendgrid vám umožňuje nastavit jako nastavení aplikace na portálu Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="770cb-243">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="770cb-244">Konfigurace systému je nastavený čtení klíčů z proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="770cb-244">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="770cb-245">Kombinování sociálních a místní přihlášení účtů</span><span class="sxs-lookup"><span data-stu-id="770cb-245">Combine social and local login accounts</span></span>

<span data-ttu-id="770cb-246">K dokončení této části, musíte nejdřív povolit externí zprostředkovatel ověřování.</span><span class="sxs-lookup"><span data-stu-id="770cb-246">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="770cb-247">V tématu [Facebook, Google a externí zprostředkovatel ověřování](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="770cb-247">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="770cb-248">Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociálních účty.</span><span class="sxs-lookup"><span data-stu-id="770cb-248">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="770cb-249">V tomto pořadí "RickAndMSFT@gmail.com" je poprvé vytvořen jako místní přihlášení; však můžete nejprve vytvořte účet jako přihlášení prostřednictvím sociální sítě a pak přidejte místní přihlášení.</span><span class="sxs-lookup"><span data-stu-id="770cb-249">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Webové aplikace: RickAndMSFT@gmail.com uživatel ověřený](accconfirm/_static/rick.png)

<span data-ttu-id="770cb-251">Klikněte na **spravovat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="770cb-251">Click on the **Manage** link.</span></span> <span data-ttu-id="770cb-252">Poznámka: externí 0 (sociální přihlášení) spojené s tímto účtem.</span><span class="sxs-lookup"><span data-stu-id="770cb-252">Note the 0 external (social logins) associated with this account.</span></span>

![Správa zobrazení](accconfirm/_static/manage.png)

<span data-ttu-id="770cb-254">Klikněte na odkaz na jinou službu přihlášení a přijímat žádosti o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="770cb-254">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="770cb-255">Na následujícím obrázku je Facebook zprostředkovatele externího ověřování:</span><span class="sxs-lookup"><span data-stu-id="770cb-255">In the following image, Facebook is the external authentication provider:</span></span>

![Spravovat seznam Facebook zobrazení externích přihlášení.](accconfirm/_static/fb.png)

<span data-ttu-id="770cb-257">Dva účty jsou spojena.</span><span class="sxs-lookup"><span data-stu-id="770cb-257">The two accounts have been combined.</span></span> <span data-ttu-id="770cb-258">Budete moci přihlásit pomocí obou.</span><span class="sxs-lookup"><span data-stu-id="770cb-258">You are able to log on with either account.</span></span> <span data-ttu-id="770cb-259">Můžete chtít uživatelům v případě, že své služby ověřování přihlášení prostřednictvím sociální sítě, jsou vypnuty nebo více pravděpodobně ztrátu přístupu k účtu mají sociálních přidat místní účty.</span><span class="sxs-lookup"><span data-stu-id="770cb-259">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="770cb-260">Povolení potvrzení účtu po lokalitu má uživatelé</span><span class="sxs-lookup"><span data-stu-id="770cb-260">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="770cb-261">Povolení potvrzení účtu v lokalitě s uživateli zamezí všichni stávající uživatelé.</span><span class="sxs-lookup"><span data-stu-id="770cb-261">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="770cb-262">Stávající uživatelé jsou uzamčen, protože nejsou potvrzen své účty.</span><span class="sxs-lookup"><span data-stu-id="770cb-262">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="770cb-263">Existující uzamčení uživatelů obejít, použijte jednu z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="770cb-263">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="770cb-264">Aktualizace databáze k označení všichni stávající uživatelé, jako je potvrzen.</span><span class="sxs-lookup"><span data-stu-id="770cb-264">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="770cb-265">Zkontrolujte stávající uživatele.</span><span class="sxs-lookup"><span data-stu-id="770cb-265">Confirm exiting users.</span></span> <span data-ttu-id="770cb-266">Například batch odesílání e-mailů s odkazy na potvrzení.</span><span class="sxs-lookup"><span data-stu-id="770cb-266">For example, batch-send emails with confirmation links.</span></span>
