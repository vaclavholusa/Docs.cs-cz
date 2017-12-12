---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: "Zpracování chyb ASP.NET | Microsoft Docs"
author: Erikre
description: "Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro jsme..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: d5d89a6a82c91b915d61ddc3c350ea0935511c07
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-error-handling"></a>Zpracování chyb ASP.NET
====================
Podle [Erik Reitan](https://github.com/Erikre)

[Stáhnout adresář Wingtip Toys ukázkového projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronická kniha (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro Web. Visual Studio 2013 [projekt pomocí zdrojového kódu C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) dispozici je pro tento kurz řady.


V tomto kurzu budete upravovat ukázkovou aplikaci adresář Wingtip Toys zpracování chyb a protokolování chyb. Zpracování chyb vám umožní aplikaci pro pohodlné zpracování chyb a zobrazovat chybové zprávy, odpovídajícím způsobem. Protokolování chyb umožňují najít a opravit chyby, které mají došlo k chybě. V tomto kurzu vychází předchozí kurzu "Adresa URL směrování" a je součástí řady kurz adresář Wingtip Toys.

## <a name="what-youll-learn"></a>Získáte informace:

- Postup přidání globální chyba zpracování konfigurace aplikace.
- Postup přidání zpracování na úrovni kódu aplikace, stránky a chyb.
- Jak protokolovat chyby pro pozdější revize.
- Postupy: zobrazení chybové zprávy, které není ohrozit zabezpečení.
- Postup implementace protokolování chyb moduly protokolování chyb a obslužné rutiny (ELMAH).

## <a name="overview"></a>Přehled

Aplikace ASP.NET musí být schopen zpracovávat chyby, ke kterým došlo během provádění konzistentním způsobem. Technologie ASP.NET používá modul CLR (CLR), který poskytuje způsob oznamování chyb aplikace jednotným způsobem. Když dojde k chybě, je vyvolána výjimka. Výjimku je žádné chyby, podmínku nebo neočekávanému chování, který zjistí aplikace.

V rozhraní .NET Framework, je objekt, který dědí z výjimka `System.Exception` třídy. Z oblasti kódu, kde došlo k potížím, je vyvolána výjimka. Výjimka je předán zásobníkem volání na místě, kdy aplikace poskytuje kód pro ošetření výjimky. Pokud aplikace není zpracovat výjimku, v prohlížeči je nucen se zobrazí podrobnosti o chybě.

Jako osvědčený postup zpracování chyb v na úrovni kódu ve `Try` / `Catch` / `Finally` bloků kódu. Pokuste se umístit tyto bloky, takže uživatel může opravit problémy v kontextu, ve kterém k nim dojde. Pokud bloky zpracování chyb příliš daleko, ze kterých došlo k chybě, bude obtížné uživatelům poskytnout informace, které potřebují k vyřešení problému.

### <a name="exception-class"></a>Exception – třída

Třídy výjimek je základní třída, ze které dědí výjimky. Většina objektů výjimek jsou instancemi třídy některé odvozené třídy výjimek, například `SystemException` třídy, `IndexOutOfRangeException` třídy, nebo `ArgumentNullException` třídy. Třídy výjimek, jako má vlastnosti, `StackTrace` vlastnost, `InnerException` vlastnost a `Message` vlastnost, která obsahují podrobné informace o chybě, která došlo k chybě.

### <a name="exception-inheritance-hierarchy"></a>Hierarchie dědičnosti výjimek

Modul runtime obsahuje základní sadu výjimek odvozených z `SystemException` třídu, která modul runtime vyvolá, když je došlo k výjimce. Většina tříd, které dědí z třídy výjimky, jako `IndexOutOfRangeException` třídy a `ArgumentNullException` třídy, neimplementuje další členy. Proto nejdůležitější informace o výjimce naleznete v hierarchii výjimky, název výjimky a informace obsažené v výjimce.

### <a name="exception-handling-hierarchy"></a>Hierarchie zpracování výjimek

V aplikaci webových formulářů ASP.NET může být ke zpracování výjimek na základě konkrétní zpracování hierarchie. Na následujících úrovních může zpracovat výjimku:

- Úrovni aplikace
- Úrovně stránky
- Úroveň kódu

Když aplikace zpracovává výjimky, můžete další informace o výjimku, která dědí z třídy výjimek často načíst a zobrazit uživateli. Kromě aplikací, stránky a úroveň kódu může také zpracovat výjimky na úrovni modulu HTTP a pomocí služby IIS vlastní obslužnou rutinu.

### <a name="application-level-error-handling"></a>Zpracování chyb aplikace úrovně

Výchozí chyby na úrovni aplikace dokáže zpracovat, změnou konfigurace aplikace nebo přidáním `Application_Error` obslužné rutiny v *Global.asax* vaší aplikace.

Přidáním dokáže zpracovat výchozí chyby a chyby protokolu HTTP `customErrors` části k *Web.config* souboru. `customErrors` Část vám pomůže určit výchozí stránku, která uživatelé budou přesměrováni na, když dojde k chybě. Také umožňuje zadat jednotlivé stránky pro konkrétní stav kód chyby.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Bohužel se při použití konfigurace přesměrovat uživatele na jinou stránku, není nutné podrobnosti o chybě, která došlo k chybě.

Však depeše chyb vzniklých kdekoli v aplikaci tak, že přidáte kód, který `Application_Error` obslužné rutiny v *Global.asax* souboru.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Zpracování událostí chyb úrovně stránky

Obslužnou rutinu úrovně stránky vrátí uživatele na stránku, kde došlo k chybě, ale nejsou spravovány instance ovládacích prvků, už nebude nic na stránce. Pokud chcete zadat podrobnosti o chybě pro uživatele aplikace, musíte na stránku konkrétně napsat podrobnosti o chybě.

Obvykle použijete obslužnou rutinu chyby na úrovni stránky k protokolování neošetřené chyby nebo provést uživatele na stránku, která může zobrazovat užitečné informace.

Tento příklad kódu ukazuje obslužnou rutinu pro událost chyby v webovou stránku ASP.NET. Tato obslužná rutina zachytí všechny výjimky, které již nejsou zpracovány v rámci `try` / `catch` blokuje na stránce.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Po zpracování chyby, je třeba ji zrušit voláním `ClearError` metodu objektu Server (`HttpServerUtility` třída), jinak se zobrazí chybu, která má dříve došlo k chybě.

### <a name="code-level-error-handling"></a>Zpracování chyb na úrovni kódu

Try-catch – příkaz se skládá z bloku try následuje jeden nebo více catch klauzule, které určují obslužné rutiny pro různé výjimky. Když je vyvolána výjimka, modul CLR (CLR) hledá catch příkaz, který zpracovává výjimku. Pokud aktuálně prováděné metoda neobsahuje blok catch, modul CLR vypadá na metodu, která volá aktuální metoda a tak dále, zásobníkem volání. Pokud se nenajde žádný blok catch, modul CLR zobrazí uživateli zprávu neošetřené výjimky a zastavuje spuštění programu.

Následující příklad kódu ukazuje běžný způsob použití `try` / `catch` / `finally` se budou zpracovávat chyby.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

Blok try ve výše uvedeném kódu obsahuje kód, který musí být chráněn proti možnou výjimkou. Blok je provést, dokud je vyvolána výjimka, nebo blok byla úspěšně dokončena. Pokud má jedna `FileNotFoundException` výjimky nebo `IOException` dojde k výjimce, provádění se přenese na jinou stránku. Potom kód součástí nakonec bloku proveden, zda došlo k chybě, nebo ne.

## <a name="adding-error-logging-support"></a>Přidání podpory protokolování chyb

Před přidáním Chyba při zpracování na adresář Wingtip Toys ukázkovou aplikaci, budou navíc podporovat protokolování chyb přidáním `ExceptionUtility` třídy k *logiku* složky. Tímto způsobem, pokaždé, když aplikace zpracovává k chybě, podrobnosti o chybě se zařadí do souboru protokolu chyb.

1. Klikněte pravým tlačítkem myši *logiku* složku a potom vyberte **přidat**  - &gt; **novou položku**.   
 **Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **Visual C#**  - &gt; **kód** skupiny šablony na levé straně. Pak vyberte **třída**ze středu seznamu a pojmenujte ji **ExceptionUtility.cs**.
3. Zvolte **přidat**. Zobrazí se nový soubor třídy.
4. Nahraďte stávající kód s následujícími službami:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Když dojde k výjimce, výjimky, je možné zapsat do souboru protokolu výjimka při volání `LogException` metoda. Tato metoda přebírá dva parametry, objekt výjimky a řetězec obsahující podrobnosti o zdroji výjimky. Výjimka protokol je zapsán do *ErrorLog.txt* v soubor *aplikace\_Data* složky.

### <a name="adding-an-error-page"></a>Přidání chybová stránka

V adresář Wingtip Toys ukázkovou aplikaci jednu stránku se použije k zobrazení chyb. Chybové stránky je určena pro zobrazovat zabezpečené chybovou zprávu pro uživatele tohoto webu. Pokud je uživatel vývojář provádění požadavku HTTP obsluhované místně na počítači tam, kde platný kód, informace o dalších chybách bude zobrazovat na chybové stránce.

1. Klikněte pravým tlačítkem na název projektu (**adresář Wingtip Toys**) v **Průzkumníku řešení** a vyberte **přidat**  - &gt; **nová položka**.   
 **Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **Visual C#**  - &gt; **webové** skupiny šablony na levé straně. Vyberte ze seznamu střední **webové formuláře se stránkou předlohy**a pojmenujte ji **ErrorPage.aspx**.
3. Klikněte na tlačítko **přidat**.
4. Vyberte *Site.Master* souboru jako stránky předlohy a potom vyberte **OK**.
5. Nahraďte stávající značky s následujícími službami:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Nahraďte stávající kód modelu code-behind (*ErrorPage.aspx.cs*) tak, aby se následujícím způsobem:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Když se zobrazí chybová stránka, `Page_Load` se spustí obslužnou rutinu události. V `Page_Load` obslužnou rutinu, je určit umístění, kde byla chyba nejprve zpracována. Pak je určen poslední chybu, která došlo k chybě při volání `GetLastError` metodu objektu Server. Pokud výjimka již neexistuje, vytvoří se k obecné výjimce. Poté Pokud požadavek HTTP byl proveden místně, tak se zobrazují všechny podrobnosti o chybě. V takovém případě pouze v místním počítači spuštění webové aplikace se zobrazí tyto podrobnosti o chybě. Po je zobrazené informace o chybě, chyba je přidán do souboru protokolu a je chyba odstraněna ze serveru.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Zobrazení nezpracované chybové zprávy pro aplikaci

Přidáním `customErrors` části k *Web.config* souboru, můžete rychle řešit jednoduché chyb vzniklých v celé aplikaci. Můžete také určit způsob zpracování chyb na základě jejich hodnoty kódu stavu, jako je například 404 - Soubor nebyl nalezen.

#### <a name="update-the-configuration"></a>Aktualizace konfigurace

Aktualizujte konfiguraci přidáním `customErrors` části k *Web.config* souboru.

1. V **Průzkumníku řešení**, najít a otevřít *Web.config* soubor v kořenovém adresáři adresář Wingtip Toys ukázkové aplikace.
2. Přidat `customErrors` části k *Web.config* souboru v rámci `<system.web>` uzlu následujícím způsobem:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Uložit *Web.config* souboru.

`customErrors` Části určuje režim, který je nastaven na "On". Určuje také `defaultRedirect`, která sděluje aplikace stránku, která má přejít při dojde k chybě. Kromě toho jste přidali na konkrétní chyby element, který určuje způsob zpracování chybu 404, pokud stránka nebyla nalezena. Později v tomto kurzu přidáte, že další zpracování, chyb zaznamená podrobnosti o chybě na úrovni aplikace.

#### <a name="running-the-application"></a>Spuštění aplikace

Můžete spustit nyní aplikaci zobrazíte aktualizované trasy.

1. Stiskněte klávesu **F5** ke spuštění ukázkové aplikace adresář Wingtip Toys.  
 Otevře se prohlížeč a ukazuje *Default.aspx* stránky.
2. Zadejte následující adresu URL do prohlížeče (nezapomeňte použít **vaše** číslo portu):  
    `https://localhost:44300/NoPage.aspx`
3. Zkontrolujte *ErrorPage.aspx* zobrazí v prohlížeči. 

    ![Zpracování chyb ASP.NET - stránka nebyla nalezena chyba](aspnet-error-handling/_static/image1.png)

Při požadavku *NoPage.aspx* stránky, který neexistuje, chybové stránky se zobrazí jednoduché chybovou zprávu a podrobné informace o chybě pokud další podrobnosti jsou k dispozici. Ale pokud uživatel si vyžádal neexistující stránky ze vzdáleného umístění, chybovou stránku by jenom zobrazit chybová zpráva červeně.

### <a name="including-an-exception-for-testing-purposes"></a>Včetně výjimku pro účely testování

Chcete-li ověřit funkci aplikaci po chybě dojde, můžete vytvořit chybové stavy úmyslně technologie ASP.NET. V ukázkové aplikaci adresář Wingtip Toys vyvolá výjimku testovací výjimka při načtení stránky výchozí Pokud chcete zobrazit, co se stane.

1. Otevřete kódu z *Default.aspx* stránka v sadě Visual Studio.   
 *Default.aspx.cs* se zobrazí stránka kódu.
2. V `Page_Load` obslužná rutina, přidejte kód tak, aby obslužná rutina vypadat takto:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Je možné vytvořit různé různé typy výjimek. Ve výše uvedeném kódu vytváříte `InvalidOperationException` při *Default.aspx* načtení stránky.

#### <a name="running-the-application"></a>Spuštění aplikace

Můžete spustit aplikace, abyste viděli, jak aplikaci ošetří výjimku.

1. Stiskněte klávesu **CTRL + F5** ke spuštění ukázkové aplikace adresář Wingtip Toys.  
 Aplikace, vyvolá výjimkou InvalidOperationException. 

    > [!NOTE] 
    > 
    > Je nutné stisknout **CTRL + F5** k zobrazení stránky, aniž by vás do kódu zobrazíte zdroji této chyby v sadě Visual Studio.
2. Zkontrolujte *ErrorPage.aspx* zobrazí v prohlížeči. 

    ![Zpracování chyb ASP.NET - chybová stránka](aspnet-error-handling/_static/image2.png)

Jak vidíte v podrobnosti o chybě, výjimka byla zachytí aplikace `customError` tématu *Web.config* souboru.

### <a name="adding-application-level-error-handling"></a>Přidání zpracování chyb na úrovni aplikace

Místo depeše výjimky pomocí `customErrors` tématu *Web.config* souboru, kde získáte málo informací o výjimce, můžete depeše chyby na úrovni aplikace a načíst podrobnosti o chybě.

1. V **Průzkumníku řešení**, najít a otevřít *Global.asax.cs* souboru.
2. Přidat **aplikace\_chyba** obslužné rutiny, které se zobrazí se následující:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Když dojde k chybě v aplikaci `Application_Error` je volána obslužná rutina. V této obslužné rutiny je poslední výjimka načíst a zkontrolovat. Pokud neošetřené výjimky a výjimky obsahuje podrobnosti o vnitřní výjimce (tedy `InnerException` není null), aplikace přenáší provádění chybovou stránku kde se zobrazují podrobnosti o výjimce.

#### <a name="running-the-application"></a>Spuštění aplikace

Můžete spustit aplikace zobrazíte další podrobnosti poskytované zpracování výjimek na úrovni aplikace.

1. Stiskněte klávesu **CTRL + F5** ke spuštění ukázkové aplikace adresář Wingtip Toys.  
 Vyvolá aplikace `InvalidOperationException` .
2. Zkontrolujte *ErrorPage.aspx* zobrazí v prohlížeči. 

    ![Zpracování chyb ASP.NET - chyba na úrovni aplikace](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Přidání zpracování chyb na úrovni stránky

Můžete přidat zpracování chyby na úrovni stránky na stránku buď pomocí přidání `ErrorPage` atribut `@Page` direktivy stránky, nebo přidáním `Page_Error` obslužné rutiny události do kódu stránky. V této části přidáte `Page_Error` obslužné rutiny události, který přenese na provedení *ErrorPage.aspx* stránky.

1. V **Průzkumníku řešení**, najít a otevřít *Default.aspx.cs* souboru.
2. Přidat `Page_Error` obslužná rutina tak, aby kódu se zobrazí jako řídí:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Když dojde k chybě na stránce `Page_Error` je volána obslužná rutina události. V této obslužné rutiny je poslední výjimka načíst a zkontrolovat. Pokud `InvalidOperationException` dojde, `Page_Error` obslužné rutiny události přenáší provádění chybovou stránku kde se zobrazují podrobnosti o výjimce.

#### <a name="running-the-application"></a>Spuštění aplikace

Můžete spustit nyní aplikaci zobrazíte aktualizované trasy.

1. Stiskněte klávesu **CTRL + F5** ke spuštění ukázkové aplikace adresář Wingtip Toys.  
 Vyvolá aplikace `InvalidOperationException` .
2. Zkontrolujte *ErrorPage.aspx* zobrazí v prohlížeči. 

    ![Zpracování chyb ASP.NET - chyba úrovně stránky](aspnet-error-handling/_static/image4.png)
3. Zavřete okno prohlížeče.

### <a name="removing-the-exception-used-for-testing"></a>Odebrání výjimky použít pro testování

Povolit adresář Wingtip Toys ukázkové aplikace fungovat bez způsobení výjimky, které jste přidali dříve v tomto kurzu, odeberte výjimky.

1. Otevřete kódu z *Default.aspx* stránky.
2. V `Page_Load` obslužnou rutinu, odeberte kód, který vyvolá výjimku, aby obslužná rutina vypadat takto:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Přidání protokolování chyb na úrovni kódu

Jak je uvedeno v tomto kurzu, můžete přidat příkazy try/catch k pokusu o spuštění části kódu, a zpracování první chyba, k níž dojde. V tomto příkladu bude pouze zapisovat podrobnosti o chybě do souboru protokolu chyb, tak, aby chyba si můžete zobrazit později.

1. V **Průzkumníku řešení**v *logiku* složky, vyhledejte a otevřete *PayPalFunctions.cs* souboru.
2. Aktualizace `HttpCall` metoda tak, aby se zobrazí kód jako následuje:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Výše uvedený kód zavolá metodu `LogException` metoda, která je součástí `ExceptionUtility` třídy. Jste přidali *ExceptionUtility.cs* třída soubor *logiku* složku dříve v tomto kurzu. `LogException` Metoda přebírá dva parametry. První parametr je objekt výjimky. Druhý parametr je řetězec slouží k rozpoznání zdroji této chyby.

### <a name="inspecting-the-error-logging-information"></a>Zkontrolujte informace o chybě protokolování

Jak už bylo zmíněno dříve, můžete určit, nejprve nutné opravit chyby, které ve vaší aplikaci v protokolu chyb. Samozřejmě se zaznamená pouze chyby, které byly do pasti a zapisovat do protokolu chyb.

1. V **Průzkumníku řešení**, najít a otevřít *ErrorLog.txt* v soubor *aplikace\_Data* složky.   
 Je nutné vybrat "**zobrazit všechny soubory**" možnost nebo "**aktualizovat**" možnost z horní části **Průzkumníku řešení** zobrazíte *ErrorLog.txt*souboru.
2. Zkontrolujte protokol chyb, které se zobrazí v sadě Visual Studio: 

    ![Zpracování chyb ASP.NET - ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Bezpečné chybové zprávy

Je **důležité si uvědomit,** , když vaše aplikace zobrazí chybové zprávy, neměla by publikovat informace, které uživatel se zlými úmysly může být užitečné v útocích vaší aplikace. Například pokud se aplikace pokusí neúspěšně k zápisu databázi, neměla by zobrazit chybovou zprávu, která obsahuje uživatelské jméno, které používá. Z tohoto důvodu se zobrazí červeně obecnou chybovou zprávu pro uživatele. Všechny další chybové informace se zobrazí pouze pro vývojáře v místním počítači.

## <a name="using-elmah"></a>Pomocí ELMAH

ELMAH (Chyba protokolovací moduly a obslužné rutiny) je nástrojem protokolování chyb, kterou je připojit vaše aplikace ASP.NET jako balíčku NuGet. ELMAH poskytuje následující možnosti:

- Protokolování neošetřených výjimek.
- Chcete-li zobrazit celý protokol recoded neošetřených výjimek na webové stránce.
- Na webové stránce, chcete-li zobrazit podrobnosti o jednotlivých zaznamenána výjimka.
- E-mailové oznámení o jednotlivé chyby v době, kdy nastane.
- Informační kanál RSS posledních 15 chyby z protokolu.

Než začnete pracovat s ELMAH, musíte jej nainstalovat. Toto je snadno pomocí *NuGet* balíček Instalační služby. Jak je uvedeno výše v této série kurz, NuGet je rozšíření sady Visual Studio, který lze snadno nainstalovat a aktualizovat opensourcové knihovny a nástroje v sadě Visual Studio.

1. V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**  - &gt; **spravovat balíčky NuGet pro řešení**. 

    ![Zpracování chyb ASP.NET - správa balíčků NuGet pro řešení](aspnet-error-handling/_static/image6.png)
2. **Spravovat balíčky NuGet** dialogové okno se zobrazí v sadě Visual Studio.
3. V **spravovat balíčky NuGet** dialogové okno, rozbalte seznam **Online** na levé straně a potom vyberte **nuget.org**. Potom, najít a nainstalovat **ELMAH** balíčku ze seznamu dostupných balíčků v režimu online. 

    ![Zpracování chyb ASP.NET - ELMA balíček NuGet](aspnet-error-handling/_static/image7.png)
4. Musíte mít internetové připojení ke stažení balíčku.
5. V **vyberte projekty** dialogové okno zkontrolujte, zda **Northwind** výběr je vybrána a potom klikněte na **OK**. 

    ![Zpracování chyb ASP.NET - vyberte projekty dialogové okno](aspnet-error-handling/_static/image8.png)
6. Klikněte na tlačítko **Zavřít** v **spravovat balíčky NuGet** dialogové okno v případě potřeby.
7. Pokud Visual Studio požaduje znovu načíst všechny otevřené soubory, vyberte možnost "**Ano všem**".
8. Balíček ELMAH přidává položky pro sebe sama v *Web.config* soubor v kořenovém adresáři projektu. Pokud Visual Studio se zeptá, pokud chcete znovu načíst upravenou *Web.config* souboru, klikněte na tlačítko **Ano**.

ELMAH je nyní připravena k uložení všechny neošetřené chyby, ke kterým dochází.

### <a name="viewing-the-elmah-log"></a>Zobrazení protokolu ELMAH

Zobrazení protokolu ELMAH je jednoduché, ale nejprve vytvoříte nezpracovanou výjimku, která se zaznamená do protokolu ELMAH.

1. Stiskněte klávesu **CTRL + F5** ke spuštění ukázkové aplikace adresář Wingtip Toys.
2. Chcete-li k neošetřené výjimce zapisovat do protokolu ELMAH, přejděte v prohlížeči na následující adresu URL (s použitím vaše číslo portu):  
    `https://localhost:44300/NoPage.aspx`Zobrazí se chybová stránka.
3. Chcete-li zobrazit protokol ELMAH, přejděte v prohlížeči na následující adresu URL (s použitím vaše číslo portu):  
    `https://localhost:44300/elmah.axd`

    ![Zpracování chyb ASP.NET - ELMAH v protokolu chyb](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Souhrn

V tomto kurzu jste se naučili o zpracování chyb na úrovni aplikace, na úrovni stránky a úrovni kódu. Naučili jste se protokolování zpracovávaný a neošetřené chyby pro pozdější revize. Jste přidali nástroje ELMAH zajistit protokolování výjimky a oznámení do vaší aplikace pomocí nástroje NuGet. Kromě toho jste se naučili o význam bezpečné chybové zprávy.

## <a name="tutorial-series-conclusion"></a>Závěru kurzu řady

*Děkujeme, že následující společně. Ať Tato sada kurzy pomohly při seznámení s webovými formuláři ASP.NET. Pokud potřebujete další informace o funkcích webových formulářů, které jsou k dispozici v technologii ASP.NET 4.5 a Visual Studio 2013, přečtěte si* [ *ASP.NET a nástroje Web Tools pro Visual Studio 2013 verze poznámky* ](../../../../visual-studio/overview/2013/release-notes.md)  *. Navíc nezapomeňte podívejte se na kurz uvedený v*   ***další kroky *** části a defintely vyzkoušení* [ *bezplatnou zkušební verzi Azure* ](https://azure.microsoft.com/pricing/free-trial/)*.*

![Děkujeme - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Další kroky

Další informace o nasazení webové aplikace do služby Microsoft Azure, najdete v části [nasazení zabezpečení ASP.NET webové formuláře aplikace s členství, OAuth a SQL Database na webovou stránku Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Bezplatná zkušební verze

[Microsoft Azure – bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/)  
 Publikování webu do služby Microsoft Azure se ušetříte čas, Údržba a náklady. Je rychlý postup nasazení webové aplikace do Azure. Pokud potřebujete spravovat a monitorovat webové aplikace, Azure nabízí celou řadu nástrojů a služeb. Správa dat, provoz, identity, zálohování, zasílání zpráv, média a výkonu v Azure. A všechny tyto je součástí velmi nákladově efektivní přístup.

## <a name="additional-resources"></a>Další prostředky

[Podrobnosti o chybě protokolování s monitorování stavu technologie ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Potvrzení

Děkujeme, že následující osob, které významně přispěli k obsahu tento kurz řady chci:

- [Alberto Poblacion, MVP &amp; MCT, Španělsko](https://mvp.microsoft.com/en-us/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Nizozemsko](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier, USA](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slovinsko](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brazílie](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Carlosu dos Santos, Brazílie](http://www.carloscds.net/)
- [Dave Campbell, USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael křížky, USA](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Jan Pope
- [Prodejci Mitchel, USA](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portugalsko](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [TIM Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tní Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Komunitní příspěvky

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
 Visual Studio 2012 související ukázka kódu na webu MSDN: [navigační adresář Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
 Visual Studio 2012 související ukázka kódu na webu MSDN: [ASP.NET 4.5 Web Forms kurzu řady v jazyce Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo - technické cílovou skupinu Přispěvatel společnosti Microsoft (twitter: @driazevedo)  
 Visual Studio 2012 překlad: [Iniciando com technologie ASP.NET Web Forms 4.5 - Parte 1 - Introdução e Visão General](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

>[!div class="step-by-step"]
[Předchozí](url-routing.md)
