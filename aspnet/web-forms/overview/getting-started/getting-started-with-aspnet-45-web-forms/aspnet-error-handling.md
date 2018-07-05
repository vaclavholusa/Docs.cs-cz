---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: Zpracování chyb v ASP.NET | Dokumentace Microsoftu
author: Erikre
description: V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 pro jsme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 4db91929b78e882b2a8a95fb36ba97db4d29d240
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392718"
---
<a name="aspnet-error-handling"></a>Zpracování chyb v ASP.NET
====================
podle [Erik Reitan](https://github.com/Erikre)

[Stáhněte si ukázkový projekt Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronickou knihu (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu se zdrojovým kódem jazyka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) je k dispozici v této sérii kurzů.


V tomto kurzu budete upravovat ukázkové aplikace Wingtip Toys zahrnovat zpracování chyb a protokolování chyb. Zpracování chyb vám umožní aplikaci řádně zpracování chyb a odpovídajícím způsobem zobrazení chybové zprávy. Protokolování chyb vám umožní vyhledat a opravit chyby, ke kterým došlo. V tomto kurzu vychází z předchozí kurz o službě "Směrování adres URL" a je součástí série kurzů na adresář Wingtip Toys.

## <a name="what-youll-learn"></a>Co se dozvíte:

- Jak přidat globální zpracování chyb, aby konfigurace vaší aplikace.
- Postup přidání úrovni aplikace, stránky a kód pro zpracování chyb.
- Jak k protokolování chyb pro pozdější kontrolu.
- Jak zobrazit chybové zprávy, které nejsou ohrozit zabezpečení.
- Způsob implementace protokolování chyb chyba protokolovací moduly a obslužné rutiny (ELMAH).

## <a name="overview"></a>Přehled

Aplikace ASP.NET musí být schopna zpracovávat chyby, ke kterým dochází při provádění konzistentním způsobem. Technologie ASP.NET používá modul CLR (CLR), který poskytuje způsob, jak oznamování chyb aplikace jednotným způsobem. Když dojde k chybě, je vyvolána výjimka. Výjimka je všechny chyby, podmínky nebo neočekávané chování, které aplikace dojde.

V rozhraní .NET Framework je výjimka, která dědí z objektu `System.Exception` třídy. Výjimka je vyvolána z oblasti kódu, kde došlo k problému. Výjimka je předána v zásobníku volání na místo, kdy aplikace poskytuje kód pro zpracování výjimky. Pokud aplikace ke zpracování výjimky, musí v prohlížeči zobrazit podrobnosti o chybě.

Jako osvědčený postup, zpracování chyb v na úrovni kódu v `Try` / `Catch` / `Finally` bloky v rámci vašeho kódu. Pokuste se umístit tyto bloky, takže uživatel může opravit problémy v kontextu, ve kterém k nim dojde. Pokud se zpracování chyb bloky jsou příliš daleko od kdy došlo k chybě, bude obtížnější uživatelům poskytnout informace, které potřebují k vyřešení problému.

### <a name="exception-class"></a>Exception – třída

Třídy výjimek je základní třída, ze kterého dědí výjimky. Většina objektů výjimek jsou instance třídy výjimek, některé z odvozených tříd, jako `SystemException` třídy, `IndexOutOfRangeException` třídy, nebo `ArgumentNullException` třídy. Třídy výjimek má vlastnosti, například `StackTrace` vlastnost, `InnerException` vlastnost a `Message` vlastnost, která obsahují podrobné informace o chyby, ke které došlo k.

### <a name="exception-inheritance-hierarchy"></a>Hierarchie dědičnosti výjimek

Modul runtime obsahuje základní sadu výjimky vyplývající z `SystemException` třídu, která modul runtime vyvolá výjimku, když došlo k výjimce. Většina tříd, které dědí z třídy výjimek, jako `IndexOutOfRangeException` třídy a `ArgumentNullException` třídy, neprovádějte implementaci další členy. Proto nejdůležitější informace o výjimce najdete v hierarchii výjimky, název výjimky a informace obsažené ve výjimce.

### <a name="exception-handling-hierarchy"></a>Hierarchie zpracování výjimek

V aplikaci webových formulářů ASP.NET může zpracování výjimek na základě konkrétní zpracování hierarchie. Výjimku lze zpracovat na následujících úrovních:

- Úrovni aplikace
- Úrovně stránky
- Úroveň kódu

Když aplikace zpracovává výjimky, můžete další informace o výjimce, která se dědí z třídy výjimek často načíst a zobrazit uživateli. Kromě aplikací, stránky a úroveň kódu můžete také zpracování výjimek na úrovni modulu protokolu HTTP a pomocí služby IIS vlastní obslužnou rutinu.

### <a name="application-level-error-handling"></a>Zpracování chyba na úrovni aplikace

Výchozí chyby na úrovni aplikace můžete zpracovat buď tak, že upravíte konfiguraci vaší aplikace, nebo tak, že přidáte `Application_Error` obslužné rutiny v *Global.asax* souboru aplikace.

Přidáním dokáže zpracovat výchozí chyby a chyby protokolu HTTP `customErrors` části *Web.config* souboru. `customErrors` Část umožňuje určit výchozí stránku, která uživatele přesměruje vás to při výskytu chyby. Umožňuje také můžete zadat jednotlivé stránky pro konkrétní stav. kód chyby.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Bohužel při použití konfigurace k přesměruje uživatele na jinou stránku, není nutné podrobnosti chyby, ke které došlo k chybě.

Však můžete zachytávat chyby, ke kterým dochází kdekoli ve vaší aplikaci tak, že přidáte kód, který `Application_Error` obslužné rutiny v *Global.asax* souboru.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Zpracování události chyb stránky

Úrovni stránky obslužná rutina vrátí uživatele na stránku tam, kde došlo k chybě, ale nejsou zachovány instance ovládacích prvků, už bude něco na stránce. Uživatel aplikace poskytnout podrobnosti o chybě, musíte konkrétně vypsat podrobnosti o chybě na stránku.

Obvykle použijete obslužná rutina chyby na úrovni stránky k protokolování neošetřené chyby nebo abyste mohli uživatele na stránku, která může zobrazovat užitečné informace.

Tento příklad kódu ukazuje obslužnou rutinu pro událost chyby v webovou stránku ASP.NET. Tato obslužná rutina zachytává všechny výjimky, které již nejsou zpracovány v rámci `try` / `catch` blokuje na stránce.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Po zpracování chyby, je nutné zrušit zavoláním `ClearError` metodu objektu serveru (`HttpServerUtility` třídy), jinak se zobrazí chyba, ke kterým došlo dříve.

### <a name="code-level-error-handling"></a>Zpracování chyb na úrovni kódu

Příkaz try-catch se skládá z bloku try, následovaný jednou nebo více catch klauzule, které určují obslužné rutiny pro různé výjimky. Když je vyvolána výjimka, modul CLR (CLR) hledá příkaz catch, který zpracovává tuto výjimku. Pokud aktuálně prováděné metody obsahuje zachytávací blok, modul CLR zjistí metodu, která volá aktuální metody a tak dále, výše v zásobníku volání. Pokud se nenajde žádný blok catch, CLR zobrazí zprávu neošetřená výjimka na uživatele a zastaví provádění programu.

Následující příklad kódu ukazuje běžným způsobem použití `try` / `catch` / `finally` zpracování chyb.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

Ve výše uvedeném kódu bloku try obsahuje kód, který musí být chráněný proti možnou výjimkou. Blok je udělat, dokud je vyvolána výjimka nebo blok bylo úspěšně dokončeno. Pokud `FileNotFoundException` výjimky nebo `IOException` dojde k výjimce, provedení je převedena na jinou stránku. Potom kód obsažený v nakonec je proveden blok, zda došlo k chybě, nebo ne.

## <a name="adding-error-logging-support"></a>Přidání podpory protokolování chyb

Před přidáním do ukázkové aplikace Wingtip Toys pro zpracování chyb, které přidáte podporu protokolování chyb tak, že přidáte `ExceptionUtility` třídu *logiky* složky. Tímto způsobem, pokaždé, když aplikace se stará o chybě, podrobnosti o chybě se přidají do souboru protokolu chyb.

1. Klikněte pravým tlačítkem myši *logiky* složku a pak vyberte **přidat**  - &gt; **nová položka**.   
   **Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **Visual C#**  - &gt; **kód** šablony skupiny na levé straně. Vyberte **třídy**uprostřed seznamu a pojmenujte ho **ExceptionUtility.cs**.
3. Zvolte **přidat**. Zobrazí se nový soubor třídy.
4. Nahraďte stávající kód následujícím kódem:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Když dojde k výjimce výjimky lze zapsat do souboru protokolu výjimka při volání `LogException` metody. Tato metoda přebírá dva parametry, objekt výjimky a řetězec obsahující podrobnosti o zdrojem výjimky. Výjimka protokol je zapsán do *ErrorLog.txt* soubor *aplikace\_Data* složky.

### <a name="adding-an-error-page"></a>Přidat chybovou stránku

V ukázkové aplikaci Wingtip Toys jednu stránku se použije k zobrazení chyb. Chybová stránka slouží k zobrazení zabezpečených chybovou zprávu uživatelům webu. Pokud uživatel je vývojář provedení požadavku HTTP, která je obsluhuje místně na počítači, kde je kód umístěn, další podrobnosti o chybách se zobrazí na chybové stránce.

1. Klikněte pravým tlačítkem na název projektu (**Wingtip Toys**) v **Průzkumníka řešení** a vyberte **přidat**  - &gt; **nová položka**.   
   **Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **Visual C#**  - &gt; **webové** šablony skupiny na levé straně. Vyberte ze seznamu střední **webové formuláře se stránkou předlohy**a pojmenujte ho **ErrorPage.aspx**.
3. Klikněte na tlačítko **přidat**.
4. Vyberte *Site.Master* soubor jako hlavní stránky a pak zvolte **OK**.
5. Nahraďte existující kód následujícím kódem:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Nahraďte stávající kód modelu code-behind (*ErrorPage.aspx.cs*) tak, aby vypadal takto:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Když se zobrazí chybová stránka, `Page_Load` obslužná rutina události je proveden. V `Page_Load` obslužnou rutinu, určuje umístění, kde byla chyba poprvé zpracována. Nakonec poslední chyby, ke které došlo je určeno volání `GetLastError` metoda objektu serveru. Pokud výjimka již neexistuje, vytvoří se obecná výjimka. Potom Pokud požadavek HTTP byl proveden místně, jsou uvedeny všechny podrobnosti o chybě. Jenom místní počítač spuštění webové aplikace v tomto případě se zobrazí tyto podrobnosti o chybě. Jakmile se zobrazí informace o chybě, chyba se přidá do souboru protokolu a je chyba odstraněna ze serveru.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Zobrazení nezpracované chybové zprávy pro aplikaci

Přidáním `customErrors` části *Web.config* soubor, můžete rychle zpracovávat jednoduché chyby, ke kterým dochází v celé aplikaci. Můžete také určit způsob zpracování chyb podle hodnoty v buňce stavový kód, jako je například 404 – Soubor nebyl nalezen.

#### <a name="update-the-configuration"></a>Aktualizace konfigurace

Aktualizovat konfiguraci tak, že přidáte `customErrors` části *Web.config* souboru.

1. V **Průzkumníka řešení**, najít a otevřít *Web.config* souboru v kořenovém adresáři ukázkové aplikace Wingtip Toys.
2. Přidat `customErrors` části *Web.config* souborů v rámci `<system.web>` uzel následujícím způsobem:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Uložit *Web.config* souboru.

`customErrors` Část určuje režim, ve kterém je nastavena na hodnotu "Na". Určuje také `defaultRedirect`, které řekne aplikaci, na stránce, které má přejít, když dojde k chybě. Kromě toho jste přidali konkrétní chyba element, který určuje, jak řešit chyby 404 při stránka nebyla nalezena. Později v tomto kurzu přidáte další chyba zpracování, které zaznamenají podrobnosti o chybě na úrovni aplikace.

#### <a name="running-the-application"></a>Spuštění aplikace

Můžete spustit nyní aplikaci zobrazíte aktualizace tras.

1. Stisknutím klávesy **F5** ke spuštění ukázkové aplikace Wingtip Toys.  
 V prohlížeči se otevře a zobrazí *Default.aspx* stránky.
2. Zadejte následující adresu URL do prohlížeče (nezapomeňte použít **vaše** číslo portu):  
    `https://localhost:44300/NoPage.aspx`
3. Zkontrolujte *ErrorPage.aspx* zobrazí v prohlížeči. 

    ![Zpracování chyb v ASP.NET – stránka nebyla nalezena chyba](aspnet-error-handling/_static/image1.png)

Pokud si vyžádáte *NoPage.aspx* stránky, která neexistuje, chybovou stránku zobrazí jednoduché chybové zprávy a informace o podrobné informace o chybě pokud jsou k dispozici další podrobnosti. Nicméně pokud uživatel si vyžádal neexistující stránku ze vzdáleného umístění, chybovou stránku by pouze zobrazit chybovou zprávu červeně.

### <a name="including-an-exception-for-testing-purposes"></a>Včetně výjimku pro účely testování

Chcete-li ověřit, jak vaše aplikace bude fungovat při chybě dochází, můžete vytvořit chybové stavy záměrně v technologii ASP.NET. V ukázkové aplikaci Wingtip Toys se vyvolat výjimku testu při načtení stránky výchozí Pokud chcete zobrazit, co se stane.

1. Otevřete kódu z *Default.aspx* stránky v sadě Visual Studio.   
   *Default.aspx.cs* použití modelu code-behind stránka bude zobrazena.
2. V `Page_Load` obslužnou rutinu, přidat kód tak, aby obslužná rutina se zobrazí takto:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Je možné vytvořit různé druhy výjimek. Ve výše uvedeném kódu při vytváření `InvalidOperationException` při *Default.aspx* načtení stránky.

#### <a name="running-the-application"></a>Spuštění aplikace

Můžete spustit aplikaci, abyste viděli, jak aplikace zpracovává výjimku.

1. Stisknutím klávesy **CTRL + F5** ke spuštění ukázkové aplikace Wingtip Toys.  
 Aplikace, vyvolá výjimku InvalidOperationException. 

    > [!NOTE] 
    > 
    > Je třeba stisknutím **CTRL + F5** zobrazíte stránku, aniž by vás to do kódu a zobrazení zdroje chyby v sadě Visual Studio.
2. Zkontrolujte *ErrorPage.aspx* zobrazí v prohlížeči. 

    ![Zpracování chyb v ASP.NET – chybová stránka](aspnet-error-handling/_static/image2.png)

Jak je vidět v podrobnostech o chybě, byla výjimka zachycena ve `customError` tématu *Web.config* souboru.

### <a name="adding-application-level-error-handling"></a>Přidání zpracování chyb na úrovni aplikace

Místo depeše výjimky pomocí `customErrors` tématu *Web.config* souboru, kde získáte pár informací o výjimce, můžete zachytávat chyby na úrovni aplikace a načíst podrobnosti o chybě.

1. V **Průzkumníka řešení**, najít a otevřít *Global.asax.cs* souboru.
2. Přidat **aplikace\_chyba** obslužná rutina tak, že se zobrazí takto:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Při výskytu chyby v aplikaci `Application_Error` obslužná rutina volána. V této rutině je poslední výjimka načíst a zkontrolovat. Pokud neošetřené výjimky a výjimky obsahuje podrobnosti o vnitřní výjimce (to znamená, `InnerException` nemá hodnotu null), aplikace přenese vykonávání na chybovou stránku ve kterém se zobrazují podrobnosti o výjimce.

#### <a name="running-the-application"></a>Spuštění aplikace

Můžete spustit aplikaci, abyste viděli další chybové údaje poskytnuté zpracování výjimek na úrovni aplikace.

1. Stisknutím klávesy **CTRL + F5** ke spuštění ukázkové aplikace Wingtip Toys.  
 Aplikace vygeneruje `InvalidOperationException` .
2. Zkontrolujte *ErrorPage.aspx* zobrazí v prohlížeči. 

    ![Zpracování chyb v ASP.NET – chyba na úrovni aplikace](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Přidání zpracování chyb na úrovni stránky

Můžete přidat chyby na úrovni stránky zpracování na stránku s použitím přidání `ErrorPage` atribut `@Page` direktivy stránky, nebo tak, že přidáte `Page_Error` obslužnou rutinu události do kódu stránky. V této části, které přidáte `Page_Error` obslužná rutina události, který přenese provádění do *ErrorPage.aspx* stránky.

1. V **Průzkumníka řešení**, najít a otevřít *Default.aspx.cs* souboru.
2. Přidat `Page_Error` obslužná rutina tak, aby se zobrazí jako modelu code-behind řídí:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Když se vyskytne chyba na stránce, `Page_Error` obslužná rutina události je volána. V této rutině je poslední výjimka načíst a zkontrolovat. Pokud `InvalidOperationException` dojde, `Page_Error` obslužná rutina události přenese vykonávání na chybovou stránku ve kterém se zobrazují podrobnosti o výjimce.

#### <a name="running-the-application"></a>Spuštění aplikace

Můžete spustit nyní aplikaci zobrazíte aktualizace tras.

1. Stisknutím klávesy **CTRL + F5** ke spuštění ukázkové aplikace Wingtip Toys.  
 Aplikace vygeneruje `InvalidOperationException` .
2. Zkontrolujte *ErrorPage.aspx* zobrazí v prohlížeči. 

    ![Zpracování chyb v ASP.NET – chyby na úrovni stránky](aspnet-error-handling/_static/image4.png)
3. Zavřete okno prohlížeče.

### <a name="removing-the-exception-used-for-testing"></a>Odebrání výjimky použít pro testování

Pokud chcete povolit ukázkové aplikace Wingtip Toys schopné fungovat bez vyvolání výjimky, které jste přidali dříve v tomto kurzu, odeberte výjimku.

1. Otevřete kódu z *Default.aspx* stránky.
2. V `Page_Load` obslužnou rutinu, odeberte kód, který vyvolá výjimku, tak, aby obslužná rutina se zobrazí takto:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Přidání protokolování chyb na úrovni kódu

Jak už bylo zmíněno dříve v tomto kurzu, můžete přidat try/catch – příkazy pokus o spuštění části kódu a zpracování první chyba, ke které dochází. V tomto příkladu pouze napíšete podrobnosti o chybě pro soubor protokolu chyb tak, aby chyba si můžete prohlédnout později.

1. V **Průzkumníku řešení**v *logiky* složky, najít a otevřít *PayPalFunctions.cs* souboru.
2. Aktualizace `HttpCall` metodu tak, aby se zobrazí kód jako následující:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Výše uvedené volání kódu `LogException` metodu, která je součástí `ExceptionUtility` třídy. Jste přidali *ExceptionUtility.cs* souboru třídy *logiky* složky dříve v tomto kurzu. `LogException` Metoda přebírá dva parametry. První parametr je objekt výjimky. Druhý parametr je řetězec sloužící k rozpoznání zdroje chyby.

### <a name="inspecting-the-error-logging-information"></a>Kontrola informace o protokolování chyb

Jak už bylo zmíněno dříve, můžete určit, které chyby v aplikaci by měla být stanovena nejprve v protokolu chyb. Samozřejmě se zaznamená pouze chyby, které byla zachycena a zapisují do protokolu chyb.

1. V **Průzkumníku řešení**, najít a otevřít *ErrorLog.txt* soubor *aplikace\_Data* složky.   
 Budete muset vybrat "**zobrazit všechny soubory**" možnost nebo "**aktualizovat**" možnost od začátku **Průzkumníka řešení** zobrazíte *ErrorLog.txt*souboru.
2. Zkontrolujte protokol chyb, které se zobrazí v sadě Visual Studio: 

    ![Zpracování chyb v ASP.NET – ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Bezpečné chybové zprávy

Je **důležité si uvědomit** , že pokud vaše aplikace zobrazí chybové zprávy, neměla by okamžitě informace, které uživatel se zlými úmysly může být užitečné v útocích vaší aplikace. Například pokud se aplikace pokusí neúspěšně k zápisu do databáze, by neměla zobrazí chybovou zprávu, která obsahuje uživatelské jméno, které používá. Z tohoto důvodu se zobrazí červeně obecné chybové zprávy pro uživatele. Všechny podrobnosti o chybě se zobrazí pouze pro vývojáře v místním počítači.

## <a name="using-elmah"></a>Pomocí knihovny ELMAH

Knihovny ELMAH (Chyba protokolovací moduly a obslužné rutiny) je nástrojem protokolování chyb, které modulu plug-in do aplikace ASP.NET jako balíček NuGet. ELMAH poskytuje následující možnosti:

- Protokolování neošetřených výjimek.
- Webová stránka, chcete-li zobrazit celý protokol zaznamenaném neošetřených výjimek.
- Webové stránky, chcete-li zobrazit úplné podrobnosti o každé protokoluje výjimku.
- E-mailové oznámení o jednotlivých chyb v době, kdy k ní dojde.
- Informační kanál RSS posledních 15 chyby z protokolu.

Než začnete pracovat s ELMAH, je nutné nainstalovat. Je to snadné použití *NuGet* balíček Instalační služby. Jak je uvedeno výše v této sérii kurzů, NuGet je rozšíření sady Visual Studio, který umožňuje snadno instalovat a aktualizovat open source knihoven a nástrojů v sadě Visual Studio.

1. V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**  - &gt; **spravovat balíčky NuGet pro řešení**. 

    ![Zpracování chyb v ASP.NET – spravovat balíčky NuGet pro řešení](aspnet-error-handling/_static/image6.png)
2. **Spravovat balíčky NuGet** dialogové okno se zobrazí v rámci sady Visual Studio.
3. V **spravovat balíčky NuGet** dialogového okna rozbalte **Online** na levé straně a pak vyberte **nuget.org**. Potom, najít a nainstalovat **ELMAH** balíčku ze seznamu dostupných balíčků online. 

    ![Zpracování chyb v ASP.NET – ELMA balíček NuGet](aspnet-error-handling/_static/image7.png)
4. Je potřeba mít připojení k Internetu a stáhněte balíček.
5. V **vyberte projekty** dialogové okno pole, ujistěte se, že **Northwind** výběr je vybrána a potom klikněte na **OK**. 

    ![Zpracování chyb v ASP.NET – vyberte projekty](aspnet-error-handling/_static/image8.png)
6. Klikněte na tlačítko **Zavřít** v **spravovat balíčky NuGet** dialogové okno v případě potřeby.
7. Pokud aplikace Visual Studio vyžaduje znovu načíst všechny otevřené soubory, vyberte možnost "**Ano všem**".
8. Balíček ELMAH přidá položky pro sebe sama v *Web.config* soubor v kořenové složce vašeho projektu. Pokud aplikace Visual Studio se zeptá, jestli chcete znovu načíst upravené *Web.config* souboru, klikněte na tlačítko **Ano**.

ELMAH je nyní připraven k uložení všechny neošetřené chyby, ke kterým dochází.

### <a name="viewing-the-elmah-log"></a>Zobrazení protokolu ELMAH

Zobrazení protokolu ELMAH je jednoduché, ale nejprve vytvoříte neošetřená výjimka, která se zaznamená do protokolu ELMAH.

1. Stisknutím klávesy **CTRL + F5** ke spuštění ukázkové aplikace Wingtip Toys.
2. Neošetřená výjimka zapsat do protokolu ELMAH, přejděte v prohlížeči na následující adresu URL (pomocí své číslo portu):  
    `https://localhost:44300/NoPage.aspx` Zobrazí se chybová stránka.
3. Chcete-li zobrazit protokol ELMAH, přejděte v prohlížeči na následující adresu URL (pomocí své číslo portu):  
    `https://localhost:44300/elmah.axd`

    ![Zpracování chyb v ASP.NET – ELMAH protokol chyb](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Souhrn

V tomto kurzu jste se dozvěděli o zpracování chyb na úrovni aplikace, kód úrovni a úrovni stránky. Naučili jste se protokolování chyb zpracovaných a nezpracovaných pro pozdější kontrolu. Přidali jste nástroje ELMAH poskytnout protokolování výjimek a oznámení do vaší aplikace pomocí nástroje NuGet. Kromě toho jste se dozvěděli o důležitosti bezpečné chybové zprávy.

## <a name="tutorial-series-conclusion"></a>Uzavření série kurzů

*Děkujeme, že následující společně. Doufám, že této sérii kurzů přispěl blíže seznamují s webových formulářů ASP.NET. Pokud potřebujete další informace o funkcích webových formulářů v ASP.NET 4.5 a Visual Studio 2013 k dispozici, najdete v článku* [ *ASP.NET and Web Tools pro Visual Studio 2013 – poznámky k* ](../../../../visual-studio/overview/2013/release-notes.md)  *. Také je potřeba podívejte se na kurz podle* ***další kroky *** oddílu a defintely vyzkoušet* [ *bezplatnou zkušební verzi Azure* ](https://azure.microsoft.com/pricing/free-trial/)*.*

![Děkujeme vám – Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Další kroky

Další informace o nasazení webové aplikace do Microsoft Azure, přečtěte si téma [nasazení zabezpečení technologie ASP.NET webové formuláře aplikace s Membership, OAuth a SQL Database na web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Bezplatná zkušební verze

[Microsoft Azure – bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/)  
 Publikování webu ve službě Microsoft Azure vám ušetří čas, údržby a náklady. Je rychlý postup nasazení webové aplikace do Azure. Pokud potřebujete spravovat a monitorovat webové aplikace Azure nabízí celou řadu nástrojů a služeb. Správa dat, provoz, identitu, zálohování, zasílání zpráv, média a výkonu v Azure. A to vše je k dispozici v nákladově velmi efektivní přístup.

## <a name="additional-resources"></a>Další prostředky

[Protokolování podrobností o chybách pomocí monitorování stavu v ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Potvrzení

Chci vám chceme poděkovat následující uživatelů, kteří provedli významné příspěvky k obsahu v této sérii kurzů:

- [Alberto Poblacion, MVP &amp; MCT, Španělsko](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Nizozemsko](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier, USA](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slovinsko](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brazílie](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Carlos dos Santos, Brazílie](http://www.carloscds.net/)
- [Dave Campbell, USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael křížky, USA](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike Pope
- [Prodejci Mitchel, USA](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portugalsko](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Petr Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Komunitní příspěvky

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Visual Studio 2012 související ukázku kódu na webu MSDN: [Wingtip Toys navigace](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Visual Studio 2012 související ukázku kódu na webu MSDN: [řady kurz ASP.NET 4.5 webové formuláře v jazyce Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo – Microsoft technické cílové skupiny přispěvatelů (twitter: @driazevedo)  
  Visual Studio 2012 překlad: [Iniciando com Introdução ASP.NET Web Forms 4.5 - Parte 1 - e Visão General](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Předchozí](url-routing.md)
