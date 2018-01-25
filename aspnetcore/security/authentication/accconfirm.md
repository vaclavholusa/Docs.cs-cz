---
title: "Potvrzení účtu a obnovení hesla v ASP.NET Core"
author: rick-anderson
description: "Ukazuje, jak vytvořit aplikaci ASP.NET Core pomocí e-mailu potvrzení a heslo resetovat."
ms.author: riande
manager: wpickett
ms.date: 12/1/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: bc9febc41d0637be9f83a02799d360489f257849
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="3ce03-103">Potvrzení účtu a obnovení hesla v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ce03-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="3ce03-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="3ce03-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="3ce03-105">Tento kurz ukazuje, jak vytvořit aplikaci ASP.NET Core pomocí e-mailu potvrzení a heslo resetovat.</span><span class="sxs-lookup"><span data-stu-id="3ce03-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="3ce03-106">Vytvořte nový projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ce03-106">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ce03-107">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3ce03-107">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3ce03-108">Tento krok platí pro Visual Studio v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3ce03-108">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="3ce03-109">Najdete v části Další pokyny rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="3ce03-109">See the next section for CLI instructions.</span></span>

<span data-ttu-id="3ce03-110">Tento kurz vyžaduje Visual Studio 2017 Preview 2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3ce03-110">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="3ce03-111">V sadě Visual Studio vytvořte nový projekt webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ce03-111">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="3ce03-112">Vyberte **jádro ASP.NET 2.0**.</span><span class="sxs-lookup"><span data-stu-id="3ce03-112">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="3ce03-113">Následující obrázek zobrazit **.NET Core** vybrána, ale můžete vybrat **rozhraní .NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="3ce03-113">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="3ce03-114">Vyberte **změna ověřování** a nastavte na **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="3ce03-114">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="3ce03-115">Ponechte výchozí **úložiště uživatelských účtů v aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="3ce03-115">Keep the default **Store user accounts in-app**.</span></span>

![Dialogové okno Nový projekt zobrazující "Jednotlivých uživatelských účtů radio" vybrali](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ce03-117">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3ce03-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ce03-118">Tento kurz vyžaduje Visual Studio 2017 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3ce03-118">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="3ce03-119">V sadě Visual Studio vytvořte nový projekt webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ce03-119">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="3ce03-120">Vyberte **změna ověřování** a nastavte na **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="3ce03-120">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

![Dialogové okno Nový projekt zobrazující "Jednotlivých uživatelských účtů radio" vybrali](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="3ce03-122">Vytvoření projektu .NET core rozhraní příkazového řádku pro systému macOS a Linux</span><span class="sxs-lookup"><span data-stu-id="3ce03-122">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="3ce03-123">Pokud používáte rozhraní příkazového řádku nebo SQLite, spusťte následující příkazy v příkazovém okně:</span><span class="sxs-lookup"><span data-stu-id="3ce03-123">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="3ce03-124">`--auth Individual`Určuje šablonu na jednotlivé uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="3ce03-124">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="3ce03-125">V systému Windows, přidejte `-uld` možnost.</span><span class="sxs-lookup"><span data-stu-id="3ce03-125">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="3ce03-126">`-uld` Možnost vytvoří připojovací řetězec databáze LocalDB, nikoli databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="3ce03-126">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="3ce03-127">Spustit `new mvc --help` získání nápovědy na tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="3ce03-127">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="3ce03-128">Otestovat novou registraci uživatele</span><span class="sxs-lookup"><span data-stu-id="3ce03-128">Test new user registration</span></span>

<span data-ttu-id="3ce03-129">Spuštění aplikace, vyberte **zaregistrovat** propojit a zaregistrovat uživatele.</span><span class="sxs-lookup"><span data-stu-id="3ce03-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="3ce03-130">Postupujte podle pokynů ke spuštění migrace Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3ce03-130">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="3ce03-131">V tomto okamžiku je pouze ověření na e-mailu [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atribut.</span><span class="sxs-lookup"><span data-stu-id="3ce03-131">At this  point, the only validation on the email is with the [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="3ce03-132">Po odeslání registrace jste přihlášení do aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ce03-132">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="3ce03-133">Později v tomto kurzu Změníme to, které noví uživatelé nelze přihlásit, dokud ověřena e-mailu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-133">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="3ce03-134">Zobrazení Identity databáze</span><span class="sxs-lookup"><span data-stu-id="3ce03-134">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="3ce03-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3ce03-135">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="3ce03-136">Z **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="3ce03-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="3ce03-137">Přejděte na **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="3ce03-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="3ce03-138">Klikněte pravým tlačítkem na **dbo. AspNetUsers** > **zobrazení dat**:</span><span class="sxs-lookup"><span data-stu-id="3ce03-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Kontextové nabídky pro tabulku AspNetUsers v Průzkumníku objektů systému SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="3ce03-140">Poznámka: `EmailConfirmed` pole je `False`.</span><span class="sxs-lookup"><span data-stu-id="3ce03-140">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="3ce03-141">Můžete chtít tento e-mail znovu použít v dalším kroku při aplikace odešle e-mail s potvrzením.</span><span class="sxs-lookup"><span data-stu-id="3ce03-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="3ce03-142">Klikněte pravým tlačítkem myši na řádek a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3ce03-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="3ce03-143">Odstranění e-mailu alias teď bude jednodušší v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="3ce03-143">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="3ce03-144">SQLite</span><span class="sxs-lookup"><span data-stu-id="3ce03-144">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="3ce03-145">V tématu [práce s SQLite v projektu ASP.NET MVC základní](xref:tutorials/first-mvc-app-xplat/working-with-sql) pokyny o tom, jak zobrazit databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="3ce03-145">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="3ce03-146">Požadovat protokol SSL a nastavení služby IIS Express pro protokol SSL</span><span class="sxs-lookup"><span data-stu-id="3ce03-146">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="3ce03-147">V tématu [vynucování SSL](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="3ce03-147">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="3ce03-148">Požadovat potvrzení e-mailu</span><span class="sxs-lookup"><span data-stu-id="3ce03-148">Require email confirmation</span></span>

<span data-ttu-id="3ce03-149">Je osvědčeným postupem potvrďte e-mailu nové registrace uživatele ověřit, že nejsou zosobnění někdo jiný (to znamená, že nebyly zaregistrována někoho jiného e-mailu).</span><span class="sxs-lookup"><span data-stu-id="3ce03-149">It's a best practice to confirm the email of a new user registration to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="3ce03-150">Předpokládejme, že jste měli diskusní fórum, a chcete zabránit "yli@example.com"od registrace jako"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="3ce03-150">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="3ce03-151">Bez potvrzení e-mailu "nolivetto@contoso.com" může získat nežádoucí e-mailu z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ce03-151">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="3ce03-152">Předpokládejme, že uživatel omylem zaregistrován jako "ylo@example.com" a kdyby zaznamenali chyby v pravopisu systému "yli", se nebudou moci používat obnovení hesla, protože aplikace nemá správnou e-mailovou.</span><span class="sxs-lookup"><span data-stu-id="3ce03-152">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="3ce03-153">Potvrzení e-mailu poskytuje jen omezenou ochranu z robotů a neposkytuje ochranu proti určené odesílatelům nevyžádané pošty, kteří mají mnoho aliasy pracovní e-mailu, které můžete použít k registraci.</span><span class="sxs-lookup"><span data-stu-id="3ce03-153">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="3ce03-154">Obvykle budete chtít zabránit noví uživatelé publikování všechna data na webové stránky, než budou mít potvrzené e-mailu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="3ce03-155">Aktualizace `ConfigureServices` tak, aby vyžadovala potvrzen e-mailu:</span><span class="sxs-lookup"><span data-stu-id="3ce03-155">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ce03-156">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3ce03-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ce03-157">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3ce03-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="3ce03-158">Předchozí řádek zabrání registrovaní uživatelé protokolována, dokud je potvrzen e-mailu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-158">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="3ce03-159">Ale daného řádku nezabrání noví uživatelé se přihlášení po jejich registraci.</span><span class="sxs-lookup"><span data-stu-id="3ce03-159">However, that line doesn't prevent new users from being logged in after they register.</span></span> <span data-ttu-id="3ce03-160">Ve výchozím kódu přihlásí uživatel po jejich registraci.</span><span class="sxs-lookup"><span data-stu-id="3ce03-160">The default code logs in a user after they register.</span></span> <span data-ttu-id="3ce03-161">Po odhlášení se nebudou moct přihlásit znovu dokud jejich registraci.</span><span class="sxs-lookup"><span data-stu-id="3ce03-161">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="3ce03-162">Později v tomto kurzu Změníme kódu, takže nově zaregistrovaný uživatele jsou **není** přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3ce03-162">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="3ce03-163">Nakonfigurujte poskytovatele tak e-mailu</span><span class="sxs-lookup"><span data-stu-id="3ce03-163">Configure email provider</span></span>

<span data-ttu-id="3ce03-164">V tomto kurzu se sendgrid vám umožňuje používá k odesílání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="3ce03-165">Musíte sendgrid vám umožňuje účtu a klíč k odeslání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="3ce03-166">Můžete vytvořit další poskytovatele e-mailu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-166">You can use other email providers.</span></span> <span data-ttu-id="3ce03-167">ASP.NET Core 2.x zahrnuje `System.Net.Mail`, který umožňuje odeslat e-mailu z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ce03-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="3ce03-168">Doporučujeme, aby že použití sendgrid vám umožňuje nebo jinou e-mailovou službu pro odeslání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-168">We recommend you use SendGrid or another email service to send email.</span></span>

<span data-ttu-id="3ce03-169">[Možnosti vzor](xref:fundamentals/configuration/options) se používá pro přístup k účtu a klíč nastavení uživatele.</span><span class="sxs-lookup"><span data-stu-id="3ce03-169">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="3ce03-170">Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3ce03-170">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="3ce03-171">Vytvořte třídu načíst klíč zabezpečení e-mailu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-171">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="3ce03-172">Tato ukázka `AuthMessageSenderOptions` je v vytvořit třídu *Services/AuthMessageSenderOptions.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="3ce03-172">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="3ce03-173">Nastavte `SendGridUser` a `SendGridKey` s [nástroj tajný klíč správce](../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="3ce03-173">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="3ce03-174">Příklad:</span><span class="sxs-lookup"><span data-stu-id="3ce03-174">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="3ce03-175">V systému Windows, tajný klíč správce ukládá vaše páry klíčů a hodnoty v *secrets.json* soubor v adresáři %APPDATA%/Microsoft/UserSecrets/ < WebAppName-userSecretsId >.</span><span class="sxs-lookup"><span data-stu-id="3ce03-175">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="3ce03-176">Obsah *secrets.json* soubor není zašifrován.</span><span class="sxs-lookup"><span data-stu-id="3ce03-176">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="3ce03-177">*Secrets.json* souboru je uveden níže ( `SendGridKey` hodnota byla odebrána.)</span><span class="sxs-lookup"><span data-stu-id="3ce03-177">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="3ce03-178">Konfigurace spuštění používat AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="3ce03-178">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="3ce03-179">Přidat `AuthMessageSenderOptions` ke kontejneru služby na konci `ConfigureServices` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="3ce03-179">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ce03-180">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3ce03-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ce03-181">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3ce03-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="3ce03-182">Konfigurovat třídu AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="3ce03-182">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="3ce03-183">Tento kurz ukazuje, jak přidat e-mailová oznámení prostřednictvím [sendgrid vám umožňuje](https://sendgrid.com/), ale můžete odesílat e-mailu pomocí protokolu SMTP a další mechanismy.</span><span class="sxs-lookup"><span data-stu-id="3ce03-183">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="3ce03-184">Nainstalujte `SendGrid` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="3ce03-184">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="3ce03-185">Z konzoly Správce balíčků, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3ce03-185">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="3ce03-186">V tématu [začněte sendgridu zadarmo](https://sendgrid.com/free/) zaregistrovat bezplatný účet sendgrid vám umožňuje.</span><span class="sxs-lookup"><span data-stu-id="3ce03-186">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="3ce03-187">Konfigurace sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="3ce03-187">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ce03-188">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3ce03-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="3ce03-189">Přidejte kód v *Services/EmailSender.cs* podobný následujícímu konfigurace sendgrid vám umožňuje:</span><span class="sxs-lookup"><span data-stu-id="3ce03-189">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ce03-190">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3ce03-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="3ce03-191">Přidejte kód v *Services/MessageServices.cs* podobný následujícímu konfigurace sendgrid vám umožňuje:</span><span class="sxs-lookup"><span data-stu-id="3ce03-191">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="3ce03-192">Povolit obnovení potvrzení a heslo účtu</span><span class="sxs-lookup"><span data-stu-id="3ce03-192">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="3ce03-193">Šablona má kód pro obnovení potvrzení a heslo účtu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-193">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="3ce03-194">Najít `[HttpPost] Register` metoda v *AccountController.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="3ce03-194">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ce03-195">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3ce03-195">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3ce03-196">Nově zaregistrovaný uživatelům zabránit v automaticky přihlášený při psaní komentářů následující řádek:</span><span class="sxs-lookup"><span data-stu-id="3ce03-196">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="3ce03-197">Kompletní metoda je zobrazena změněné řádek zvýrazněna:</span><span class="sxs-lookup"><span data-stu-id="3ce03-197">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

<span data-ttu-id="3ce03-198">Poznámka: Předchozí kód se nezdaří, pokud budete implementovat `IEmailSender` a odesílat emaily s prostým textem.</span><span class="sxs-lookup"><span data-stu-id="3ce03-198">Note: The previous code will fail if you implement `IEmailSender` and send a plain text email.</span></span> <span data-ttu-id="3ce03-199">V tématu [tento problém](https://github.com/aspnet/Home/issues/2152) pro další informace a řešení.</span><span class="sxs-lookup"><span data-stu-id="3ce03-199">See [this issue](https://github.com/aspnet/Home/issues/2152) for more information and a workaround.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ce03-200">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3ce03-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ce03-201">Zrušte komentář kódu, chcete-li povolit potvrzení účtu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-201">Uncomment the code to enable account confirmation.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="3ce03-202">Poznámka: Jsme se také brání nově zaregistrovaný uživatele automaticky přihlášený při psaní komentářů následující řádek:</span><span class="sxs-lookup"><span data-stu-id="3ce03-202">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="3ce03-203">Povolit obnovení hesla uncommenting kód `ForgotPassword` akce v *Controllers/AccountController.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="3ce03-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="3ce03-204">Zrušením komentáře u prvku formuláře v *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3ce03-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="3ce03-205">Můžete chtít odebrat `<p> For more information on how to enable reset password ... </p>` element, který obsahuje odkaz na tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="3ce03-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="3ce03-206">Zaregistrovat, potvrďte e-mailu a resetování hesla</span><span class="sxs-lookup"><span data-stu-id="3ce03-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="3ce03-207">Spusťte webovou aplikaci a testování potvrzení účtu a heslo pro obnovení toku.</span><span class="sxs-lookup"><span data-stu-id="3ce03-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="3ce03-208">Spusťte aplikaci a zaregistrovat nového uživatele</span><span class="sxs-lookup"><span data-stu-id="3ce03-208">Run the app and register a new user</span></span>

 ![Zobrazení zaregistrovat účet webové aplikace](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="3ce03-210">Zkontrolujte e-mailu pro potvrzení propojení účtu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="3ce03-211">V tématu [ladění e-mailu](#debug) Pokud neobdržíte e-mail.</span><span class="sxs-lookup"><span data-stu-id="3ce03-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="3ce03-212">Kliknutím na odkaz k potvrzení e-mailu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="3ce03-213">Přihlaste se pomocí vaší e-mailu a heslo.</span><span class="sxs-lookup"><span data-stu-id="3ce03-213">Log in with your email and password.</span></span>
* <span data-ttu-id="3ce03-214">Odhlaste se.</span><span class="sxs-lookup"><span data-stu-id="3ce03-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="3ce03-215">Zobrazení stránky Správa</span><span class="sxs-lookup"><span data-stu-id="3ce03-215">View the manage page</span></span>

<span data-ttu-id="3ce03-216">Vyberte jméno uživatele v prohlížeči: ![okno prohlížeče s uživatelským jménem](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="3ce03-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="3ce03-217">Možná budete muset Rozbalit navigační panel zobrazíte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3ce03-217">You might need to expand the navbar to see user name.</span></span>

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ce03-219">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3ce03-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3ce03-220">Zobrazí se stránka Správa s **profil** vybrána karta.</span><span class="sxs-lookup"><span data-stu-id="3ce03-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="3ce03-221">**E-mailu** zobrazí zaškrtávací políčko označující e-mailu, bylo potvrzeno.</span><span class="sxs-lookup"><span data-stu-id="3ce03-221">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![Stránka Správa](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ce03-223">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3ce03-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ce03-224">Budeme mluvit o tuto stránku později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-224">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="3ce03-225">![Stránka Správa](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="3ce03-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="3ce03-226">Test resetování hesla</span><span class="sxs-lookup"><span data-stu-id="3ce03-226">Test password reset</span></span>

* <span data-ttu-id="3ce03-227">Pokud jste přihlášeni, vyberte **odhlášení**.</span><span class="sxs-lookup"><span data-stu-id="3ce03-227">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="3ce03-228">Vyberte **přihlásit** propojení a vyberte **zapomněli jste heslo?** odkaz.</span><span class="sxs-lookup"><span data-stu-id="3ce03-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="3ce03-229">Zadejte e-mailu, které jste použili k registraci účtu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="3ce03-230">Odešle e-mail s odkazem pro resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="3ce03-230">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="3ce03-231">Zkontrolujte e-mailu a klikněte na odkaz k resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="3ce03-231">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="3ce03-232">Po vaše heslo bylo resetováno úspěšně, můžete se přihlásit s e-mailu a nové heslo.</span><span class="sxs-lookup"><span data-stu-id="3ce03-232">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="3ce03-233">Ladění e-mailu</span><span class="sxs-lookup"><span data-stu-id="3ce03-233">Debug email</span></span>

<span data-ttu-id="3ce03-234">Pokud nelze získat pracovní e-mailu:</span><span class="sxs-lookup"><span data-stu-id="3ce03-234">If you can't get email working:</span></span>

* <span data-ttu-id="3ce03-235">Zkontrolujte [e-mailu aktivity](https://sendgrid.com/docs/User_Guide/email_activity.html) stránky.</span><span class="sxs-lookup"><span data-stu-id="3ce03-235">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="3ce03-236">Zkontrolujte složky nevyžádané pošty.</span><span class="sxs-lookup"><span data-stu-id="3ce03-236">Check your spam folder.</span></span>
* <span data-ttu-id="3ce03-237">Zkuste jinou e-mailový alias na jinou e-mailovou zprostředkovatele (Microsoft, Yahoo, z Gmailu atd.)</span><span class="sxs-lookup"><span data-stu-id="3ce03-237">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="3ce03-238">Vytvoření [konzolovou aplikaci k odesílání e-mailu](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="3ce03-238">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="3ce03-239">Pokus o odeslání na jiné e-mailové účty.</span><span class="sxs-lookup"><span data-stu-id="3ce03-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="3ce03-240">**Poznámka:** nejlepším způsobem zabezpečení nechcete použít produkční tajných klíčů v testovacích a vývojových.</span><span class="sxs-lookup"><span data-stu-id="3ce03-240">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="3ce03-241">Pokud publikujete aplikaci do Azure, můžete tajné klíče sendgrid vám umožňuje nastavit jako nastavení aplikace na portálu Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ce03-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="3ce03-242">Konfigurace systému je instalační program načíst klíče z proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="3ce03-242">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="3ce03-243">Zakázat přihlášení při registraci</span><span class="sxs-lookup"><span data-stu-id="3ce03-243">Prevent login at registration</span></span>

<span data-ttu-id="3ce03-244">S aktuální šablony, jakmile uživatel dokončí registračním formuláři se přihlášeni (ověřený).</span><span class="sxs-lookup"><span data-stu-id="3ce03-244">With the current templates, once a user completes the registration form, they're logged in (authenticated).</span></span> <span data-ttu-id="3ce03-245">Chcete obecně potvrzení e-mailu před jejich protokolování.</span><span class="sxs-lookup"><span data-stu-id="3ce03-245">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="3ce03-246">V následující části jsme se změnit kód tak, aby vyžadovala noví uživatelé mít potvrzené e-mailu předtím, než jste přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="3ce03-246">In the section below, we will modify the code to require new users have a confirmed email before they're logged in.</span></span> <span data-ttu-id="3ce03-247">Aktualizace `[HttpPost] Login` akce v *AccountController.cs* soubor s následující zvýrazněný změny.</span><span class="sxs-lookup"><span data-stu-id="3ce03-247">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

<span data-ttu-id="3ce03-248">**Poznámka:** nejlepším způsobem zabezpečení nechcete použít produkční tajných klíčů v testovacích a vývojových.</span><span class="sxs-lookup"><span data-stu-id="3ce03-248">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="3ce03-249">Pokud publikujete aplikaci do Azure, můžete tajné klíče sendgrid vám umožňuje nastavit jako nastavení aplikace na portálu Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ce03-249">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="3ce03-250">Konfigurace systému je instalační program načíst klíče z proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="3ce03-250">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="3ce03-251">Kombinování sociálních a místní přihlášení účtů</span><span class="sxs-lookup"><span data-stu-id="3ce03-251">Combine social and local login accounts</span></span>

<span data-ttu-id="3ce03-252">Poznámka: Tato část se týká pouze ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="3ce03-252">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="3ce03-253">Pro technologii ASP.NET základní 2.x najdete v tématu [to](https://github.com/aspnet/Docs/issues/3753) problém.</span><span class="sxs-lookup"><span data-stu-id="3ce03-253">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="3ce03-254">K dokončení této části, musíte nejdřív povolit externí zprostředkovatel ověřování.</span><span class="sxs-lookup"><span data-stu-id="3ce03-254">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="3ce03-255">V tématu [povolení ověřování pomocí služby Facebook, Google a ostatní externího poskytovatele](social/index.md).</span><span class="sxs-lookup"><span data-stu-id="3ce03-255">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="3ce03-256">Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociálních účty.</span><span class="sxs-lookup"><span data-stu-id="3ce03-256">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="3ce03-257">V tomto pořadí "RickAndMSFT@gmail.com" je poprvé vytvořen jako místní přihlášení; však můžete nejprve vytvořte účet jako přihlášení prostřednictvím sociální sítě a pak přidejte místní přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3ce03-257">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Webové aplikace: RickAndMSFT@gmail.com uživatel ověřený](accconfirm/_static/rick.png)

<span data-ttu-id="3ce03-259">Klikněte na **spravovat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="3ce03-259">Click on the **Manage** link.</span></span> <span data-ttu-id="3ce03-260">Poznámka: externí 0 (sociální přihlášení) spojené s tímto účtem.</span><span class="sxs-lookup"><span data-stu-id="3ce03-260">Note the 0 external (social logins) associated with this account.</span></span>

![Správa zobrazení](accconfirm/_static/manage.png)

<span data-ttu-id="3ce03-262">Klikněte na odkaz na jinou službu přihlášení a přijímat žádosti o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3ce03-262">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="3ce03-263">Na následujícím obrázku je Facebook zprostředkovatele externího ověřování:</span><span class="sxs-lookup"><span data-stu-id="3ce03-263">In the image below, Facebook is the external authentication provider:</span></span>

![Spravovat seznam Facebook zobrazení externích přihlášení.](accconfirm/_static/fb.png)

<span data-ttu-id="3ce03-265">Dva účty jsou spojena.</span><span class="sxs-lookup"><span data-stu-id="3ce03-265">The two accounts have been combined.</span></span> <span data-ttu-id="3ce03-266">Bude moct přihlásit pomocí buď účtu.</span><span class="sxs-lookup"><span data-stu-id="3ce03-266">You will be able to log on with either account.</span></span> <span data-ttu-id="3ce03-267">Můžete chtít uživatelům v případě, že jejich sociální přihlášení ověřovací služby, jsou vypnuty nebo více pravděpodobně ztrátu přístupu k účtu mají sociálních přidat místní účty.</span><span class="sxs-lookup"><span data-stu-id="3ce03-267">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they've lost access to their social account.</span></span>
