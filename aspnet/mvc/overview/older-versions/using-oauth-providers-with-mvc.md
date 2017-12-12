---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: "Pomocí poskytovatelů OAuth MVC 4 | Microsoft Docs"
author: tfitzmac
description: "V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 4, který umožňuje uživatelům přihlásit se pomocí přihlašovacích údajů z externího poskytovatele, jako je například Facebo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2013
ms.topic: article
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 965d2e740cc76838b1b4e1c618a2a6d784672fcc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-oauth-providers-with-mvc-4"></a>Pomocí poskytovatelů OAuth MVC 4
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 4, který umožňuje uživatelům přihlásit se pomocí přihlašovacích údajů z externího poskytovatele, jako je Facebook, Twitter, Microsoft nebo Google a potom integruje některé z funkcí, které z těchto zprostředkovatelů do vaší webové aplikace. Pro zjednodušení tento kurz se zaměřuje na práci s přihlašovacími údaji ze sítě Facebook.
> 
> Pokud chcete používat externí přihlašovací údaje ve webové aplikaci ASP.NET MVC 5, najdete v části [vytvořit aplikaci ASP.NET MVC 5 s použitím Facebook a Google OAuth2 a přihlašování OpenID](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Povolení tyto přihlašovací údaje ve své weby nabízí významné výhody, protože milionům uživatelů již mají účty s tyto externí zprostředkovatele. Tyto uživatele může být více nakloněné zaregistrovat pro svůj web, pokud nemají k vytvoření a zapamatovat si novou sadu přihlašovacích údajů. Navíc po přihlášení uživatele prostřednictvím jednoho z těchto poskytovatelů, můžete začlenit sociálních operace od zprostředkovatele.


## <a name="what-youll-build"></a>Co budete sestavení

V tomto kurzu jsou dva hlavní cíle:

1. Povolte uživateli přihlásit se pomocí přihlašovacích údajů od poskytovatele OAuth.
2. Načtení informací o účtu od poskytovatele a tyto informace integrovat registraci účtu pro svůj web.

I když v příkladech v tomto kurzu se zaměřit na jako zprostředkovatel ověřování pomocí služby Facebook, můžete upravit kód, který použije žádného z poskytovatelů. Kroky pro implementaci kteréhokoli poskytovatele služeb, jsou velmi podobné kroky, které se zobrazí v tomto kurzu. Pouze si všimnete významné rozdíly při provádění přímé volání rozhraní API poskytovatele nastavit.

## <a name="prerequisites"></a>Požadavky

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) nebo [sady Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Nebo

- Microsoft Visual Studio 2010 SP1 nebo [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Kromě toho toto téma předpokládá, že máte základní znalosti o architektuře ASP.NET MVC a Visual Studio. Pokud potřebujete Úvod do architektury ASP.NET MVC 4, přečtěte si [Úvod do architektury ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Vytvoření projektu

V sadě Visual Studio vytvořte novou aplikaci ASP.NET MVC 4 Web s názvem &quot;OAuthMVC&quot;. Můžete určit cílovou rozhraní .NET Framework 4.5 nebo 4.

![Vytvoření projektu](using-oauth-providers-with-mvc/_static/image1.png)

V okně Nový ASP.NET MVC 4 projekt, vyberte **Internetové aplikace** a nechte **Razor** jako zobrazovací modul.

![Vyberte Internetové aplikace.](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Povolit zprostředkovatele

Když vytvoříte webovou aplikaci MVC 4 s šablona aplikací Internet, vytvoření projektu se soubor s názvem AuthConfig.cs v aplikaci\_spouštěcí složka.

![Soubor AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

Soubor AuthConfig obsahuje kód k registraci klientů pro externí zprostředkovatele ověřování. Ve výchozím nastavení tento kód je označeno jako komentář, takže jsou povolené žádné externí zprostředkovatele.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Musí zrušením komentáře u tohoto kódu pro použití externího ověřování klienta. Zrušením komentáře u jenom zprostředkovatelé, které chcete zahrnout do vaší lokality. V tomto kurzu umožňuje pouze pověření služby Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Všimněte si v předchozím příkladu, že metoda obsahuje prázdné řetězce pro parametry registrace. Pokud se pokusíte spustit nyní aplikaci, aplikace vyvolá výjimku argument, protože pro parametry nejsou povoleny prázdné řetězce. Poskytnout platné hodnoty, je nutné zaregistrovat webové stránky s externí zprostředkovatele, jak je znázorněno v následujícím oddílu.

## <a name="registering-with-an-external-provider"></a>Registrace pomocí externího poskytovatele

K ověření uživatelů s přihlašovacími údaji z externího poskytovatele, je nutné zaregistrovat u zprostředkovatele vašeho webu. Při registraci vaší lokality, obdržíte při registraci klienta parametry (například klíč nebo id a tajný klíč). Účet musí mít s zprostředkovatele, který chcete použít.

V tomto kurzu nezobrazuje všechny kroky, které musíte provést pro registraci se tyto zprostředkovatele. Kroky nejsou obvykle obtížná. Úspěšně zaregistrovat váš web, postupujte podle pokynů uvedených v těchto lokalitách. Chcete-li začít s registrací sítě, naleznete na vývojáře pro:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Služby Twitter.](https://dev.twitter.com/)

Při registraci vaší lokality se sítí Facebook, můžete zadat &quot;localhost&quot; domény, lokality a `&quot;http://localhost/&quot;` pro adresu URL, jak je znázorněno na obrázku níže. Pomocí localhost funguje s většinu poskytovatelů, ale aktuálně nefunguje s zprostředkovatele služby Microsoft. Pro zprostředkovatele služby Microsoft musí obsahovat adresu URL webu platný.

![zaregistrovat lokality](using-oauth-providers-with-mvc/_static/image4.png)

Na předchozím obrázku se odebraly hodnoty id aplikace, tajný klíč aplikace a kontaktní e-mailu. Při registraci ve skutečnosti váš web, bude k dispozici tyto hodnoty. Můžete si uvědomit hodnoty id aplikace a tajný klíč aplikace, protože je přidáte do vaší aplikace.

## <a name="creating-test-users"></a>Vytvoření testovacích uživatelů

Pokud není paměti pomocí existujícího účtu sítě Facebook testování webu, můžete tuto část přeskočit.

Můžete snadno vytvořit testovací uživatele pro vaši aplikaci v rámci na stránce Správa aplikace Facebook. Pomocí těchto testovací účty pro přihlášení do vaší lokality. Vytvoření testovacích uživatelů kliknutím **role** v levém navigačním podokně a kliknutím na odkaz **vytvořit** odkaz.

![Vytvoření testovacích uživatelů](using-oauth-providers-with-mvc/_static/image5.png)

Facebook lokality automaticky vytvoří počet testovací účty, které požadujete.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Přidání id aplikace a tajný klíč ze zprostředkovatele

Teď, když jste obdrželi id a tajný klíč ze sítě Facebook, přejděte zpět do souboru AuthConfig a přidejte je jako hodnoty parametru. Hodnoty zobrazené níže nejsou skutečné hodnoty.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Přihlaste se pomocí externí přihlašovací údaje

To je všechno, co musíte udělat, aby povolit externí přihlašovací údaje ve vaší lokalitě. Spusťte aplikaci a klikněte na odkaz přihlášení v pravém horním rohu. Šablona automaticky rozpozná, že jako zprostředkovatel registrovali Facebook a obsahuje tlačítko pro zprostředkovatele. Když si zaregistrujete několik poskytovatelů, pro každé z nich se automaticky zahrnuty.

![externí přihlášení](using-oauth-providers-with-mvc/_static/image6.png)

V tomto kurzu není nenajdete, jak přizpůsobit přihlašovací tlačítka pro externí zprostředkovatele. Podrobnosti naleznete v tématu [přizpůsobení uživatelského rozhraní pro přihlášení, při použití účtu OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Klikněte na tlačítko Facebook k přihlášení pomocí přihlašovacích údajů služby Facebook. Při výběru jednoho z externích poskytovatelů, jsou přesměrovány do této lokality a výzva k přihlášení pomocí služby.

Následující obrázek znázorňuje přihlašovací obrazovku pro Facebook. Poznámky k, že váš účet Facebook používáte k přihlášení na web s názvem oauthmvcexample.

![ověřování Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Po přihlášení pomocí přihlašovacích údajů Facebook, na stránce informuje uživatele, že web bude mít přístup k základní informace.

![žádost o oprávnění](using-oauth-providers-with-mvc/_static/image8.png)

Po výběru **přejděte do aplikace**, uživatel musí zaregistrovat pro tuto lokalitu. Následující obrázek znázorňuje registrační stránce po má přihlášený uživatel se pomocí přihlašovacích údajů služby Facebook. Uživatelské jméno je obvykle předvyplněné s názvem od zprostředkovatele.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Klikněte na tlačítko **zaregistrovat** dokončit registraci. Zavřete prohlížeč.

Uvidíte, že byl přidán nový účet k vaší databázi. V Průzkumníku serveru, otevřete **objekt DefaultConnection** databáze a otevřete **tabulky** složky.

![tabulky databáze](using-oauth-providers-with-mvc/_static/image10.png)

Klikněte pravým tlačítkem myši **UserProfile** tabulky a vyberte **zobrazit Data tabulky**.

![Zobrazit data](using-oauth-providers-with-mvc/_static/image11.png)

Zobrazí se nový účet, který jste přidali. Podívejte se na data v **webová stránka\_OAuthMembership** tabulky. Zobrazí se další data související s externího poskytovatele pro účet, který jste právě přidali.

Pokud chcete povolit externího ověřování, budete mít. Však můžete další integrovat informace od poskytovatele do nového uživatele proces registrace, jak je znázorněno v následující části.

## <a name="create-models-for-additional-user-information"></a>Vytváření modelů pro další uživatelské informace

Protože jste si všimli v předchozích částech, není potřeba načíst žádné další informace o registraci předdefinovaný účet pro práci. Většina externí zprostředkovatelé však předat zpět Další informace o uživateli. Následující části ukazují, jak tyto informace uchovat a uložte ho do databáze. Konkrétně si ponechají hodnoty pro uživatele úplný název, identifikátor URI uživatele osobní webové stránky a hodnotu, která určuje, zda má Facebook ověřit účet.

Budete používat [migrace Code First](https://msdn.microsoft.com/en-us/data/jj591621) přidat tabulku pro ukládání informací o další uživatele. Existující databázi, přidáváte tabulky, proto musíte nejprve vytvořit snímek aktuální databázi. Vytvořením snímek aktuální databázi, můžete vytvořit později migrace, který obsahuje pouze novou tabulku. Chcete-li vytvořit snímek aktuální databáze:

1. Otevřete **Konzola správce balíčků**
2. Spusťte příkaz **enable-migrations se**
3. Spusťte příkaz **přidat migrace počáteční – IgnoreChanges**
4. Spusťte příkaz **update-database**

Nyní budete přidávat nové vlastnosti. Ve složce modely otevřete soubor AccountModels.cs a najít RegisterExternalLoginModel třídy. Třída RegisterExternalLoginModel obsahuje hodnoty, které vraťte od zprostředkovatele ověřování. Přidáte vlastnosti s názvem FullName a odkaz, jak je znázorněno dole.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Také v AccountModels.cs, přidejte novou třídu s názvem ExtraUserInformation. Tato třída reprezentuje nové tabulky, které budou vytvořeny v databázi.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

Ve třídě UsersContext přidáte dole na vytvořit vlastnost DbSet pro novou třídu zvýrazněný kód.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Nyní jste připraveni vytvořit novou tabulku. Otevřete konzolu Správce balíčků znovu a tento čas:

1. Spusťte příkaz **AddExtraUserInformation přidat migrace**
2. Spusťte příkaz **update-database**

Nová tabulka nyní v databázi existuje.

## <a name="retrieve-the-additional-data"></a>Načíst další data.

Existují dva způsoby, jak načíst data dalšího uživatele. První způsob je zachování dat uživatele, který je předán zpět, ve výchozím nastavení, při žádosti o ověření. Druhý způsob je konkrétně volání API poskytovatele a požádat o další informace. Hodnoty pro FullName a propojení se automaticky předaná zpět Facebook. Hodnotu, která určuje, zda má Facebook ověřit účet je načíst prostřednictvím volání rozhraní API sítě Facebook. Nejprve se naplní hodnoty pro FullName a odkaz, a pak později, zobrazí se hodnota ověřené.

Pokud chcete načíst data další uživatele, otevřete **AccountController.cs** souboru v **řadiče** složky.

Tento soubor obsahuje logiku pro protokolování, registrace a správy účtů. Konkrétně, Všimněte si volat metody **ExternalLoginCallback** a **ExternalLoginConfirmation**. V rámci těchto metod můžete přidat kód pro přizpůsobení operations externí přihlášení pro vaši aplikaci. První řádek **ExternalLoginCallback** metoda obsahuje:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Další uživatelská data je předán zpět v **ExtraData** vlastnost **AuthenticationResult** objekt, který je vrácen z **VerifyAuthentication** metoda. Klient sítě Facebook obsahuje následující hodnoty v **ExtraData** vlastnost:

- id
- name
- Odkaz
- pohlaví
- accesstoken

Ostatní poskytovatele budou mít podobné, ale poněkud liší data ve vlastnosti ExtraData.

Pokud uživatel je nový na váš web, bude načtení některé další data a předat data do zobrazení potvrzení. Poslední blok kódu v metodě se spustí pouze v případě, že uživatel je nový na váš web. Nahraďte tento řádek:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Tento řádek:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Tato změna jenom obsahuje hodnoty vlastností FullName a odkaz.

V **ExternalLoginConfirmation** metoda, upravit kód jak je znázorněno níže se uložit informace o další uživatele.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Úprava zobrazení

Na stránce registrace se zobrazí další uživatelská data, která je načíst ze zprostředkovatele.

V **zobrazení**/**účet** složku, otevřete **ExternalLoginConfirmation.cshtml**. Pod stávající pole pro uživatelské jméno přidejte pro FullName, odkaz a PictureLink pole.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Nyní jste připraveni téměř spusťte aplikaci a zaregistrovat nového uživatele společně s dalšími informacemi, které jsou uloženy. Musí mít účet, který již není zaregistrována webu. Můžete buď použít jiný testovací účet nebo odstraňování řádků v **UserProfile** a **webové stránky\_OAuthMembership** tabulky pro účet, chcete-li znovu použít. Odstraněním těchto řádků bude zajištěno, že účet je zaregistrován znovu.

Spusťte aplikaci a zaregistrovat nového uživatele. Všimněte si, že tuto chvíli potvrzovací stránku obsahuje více hodnot.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Po dokončení registrace, zavřete prohlížeč. Podívejte se v databázi Všimněte novými hodnotami v **ExtraUserInformation** tabulky.

## <a name="install-nuget-package-for-facebook-api"></a>Nainstalujte balíček NuGet pro rozhraní API sítě Facebook

Poskytuje Facebook [rozhraní API](https://developers.facebook.com/docs/reference/apis/) můžete volat k provádění operací. Přesměrováním odesílání požadavků HTTP nebo pomocí instalace balíčku NuGet, který usnadňuje odesílání těchto požadavků můžete volat rozhraní API sítě Facebook. Pomocí balíčku NuGet se zobrazí v tomto kurzu, ale instalace NuGet balíček není nezbytné. Tento kurz ukazuje, jak použít balíček Facebook C# SDK. Jsou i další balíčky NuGet, které pomáhají s volání rozhraní API sítě Facebook.

Z **spravovat balíčky NuGet** windows, vyberte balíček, Facebook C# SDK.

![instalovat balíček](using-oauth-providers-with-mvc/_static/image13.png)

Použijete Facebook C# SDK k volání operace vyžadující přístupový token pro daného uživatele. V další části ukazuje, jak získat přístupový token.

## <a name="retrieve-access-token"></a>Načtení přístupového tokenu

Většina externí zprostředkovatelé předat zpět token přístupu po ověření přihlašovacích údajů uživatele. Tento přístupový token je velmi důležité, protože umožňuje volání operací, které jsou dostupné jenom pro ověřené uživatele. Proto načítání a ukládání přístupový token je nezbytné Pokud byste chtěli poskytnout další funkce.

V závislosti na externího poskytovatele může být přístupový token platný pro jenom omezené množství času. K zajištění, že máte platný přístupový token, se budou načítat se pokaždé, když uživatel přihlásí a uložte ho jako hodnotu relace než uložit do databáze.

V **ExternalLoginCallback** metody přístupový token také předán zpět **ExtraData** vlastnost **AuthenticationResult** objektu. Přidejte zvýrazněný kód, který **ExternalLoginCallback** uložit v tokenu přístupu **relace** objektu. Tento kód běží pokaždé, když se uživatel přihlásí pomocí účtu sítě Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

I když tento příklad načte token přístupu ze sítě Facebook, můžete načíst tokenu přístupu z kteréhokoli externího poskytovatele služeb prostřednictvím stejný klíč s názvem &quot;accesstoken&quot;.

## <a name="logging-off"></a>Odhlášení

Výchozí hodnota **odhlášení** metoda přihlásí uživatele mimo aplikaci, ale není přihlásit uživatele z externího poskytovatele. Přihlásit uživatele mimo Facebook a zabránit tokenu z uchování po má odhlášením uživatele, můžete přidat následující zvýrazněný kód do **odhlášení** metoda v AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

Hodnota poskytují v `next` parametr je adresa URL pro použití poté, co se uživatel přihlásil ze sítě Facebook. Při testování v místním počítači, by poskytnete správným číslem portu místní lokality. V produkčním prostředí by zadejte výchozí stránku, například contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Načíst informace o uživateli, který vyžaduje přístupový token

Teď, když máte uložené přístupový token a Facebook C# SDK balíček nainstalován, můžete je použít společně k požadavku na další uživatelské informace ze sítě Facebook. V **ExternalLoginConfirmation** metody vytvoření instance **FacebookClient** třída předáním hodnota tokenu přístupu. Žádosti o hodnotu **ověřit** vlastnost pro aktuální, ověřeného uživatele. **Ověřit** vlastnost určuje, zda Facebook ověřila účet jiným způsobem, například odeslání zprávy na mobilní telefon. Uložte tato hodnota v databázi.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Musíte se znovu buď odstranit záznamy v databázi pro uživatele, nebo použijte jiný účet Facebook.

Spusťte aplikaci a zaregistrovat nového uživatele. Podívejte se na **ExtraUserInformation** tabulky a zobrazit tak hodnotu pro vlastnost ověřeno.

## <a name="conclusion"></a>Závěr

V tomto kurzu jste vytvořili lokality, který je integrován se sítí Facebook pro ověření uživatele a registrační data. Seznámili jste se výchozí chování, které jsou nastaveny pro webové aplikace MVC 4 a postup přizpůsobení této výchozí chování.

## <a name="related-topics"></a>Související témata

- [Vytvoření aplikace ASP.NET MVC pomocí ověřování a databázi SQL a nasazení do Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
