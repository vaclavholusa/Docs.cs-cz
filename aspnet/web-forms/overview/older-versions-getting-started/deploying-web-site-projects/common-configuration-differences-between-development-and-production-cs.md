---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: Běžné konfigurace rozdíly mezi vývojovým týmem a produkční (C#) | Microsoft Docs
author: rick-anderson
description: V dřívějších kurzy jsme nasadili náš web tak, že zkopírujete všechny relevantní soubory z vývojového prostředí do produkčního prostředí. Ale i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 2694e0dba774a5bca13b9acc6b14c3e47226a064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="common-configuration-differences-between-development-and-production-c"></a>Běžné konfigurace rozdíly mezi vývojovým týmem a produkční (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhnout PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> V dřívějších kurzy jsme nasadili náš web tak, že zkopírujete všechny relevantní soubory z vývojového prostředí do produkčního prostředí. Však není existovat konfigurace rozdíly mezi prostředí, které vyžaduje, aby každé prostředí jedinečný soubor Web.config. V tomto kurzu prozkoumá rozdíly obvyklá konfigurace a zjistí strategie pro uchovávání informací samostatné konfigurace.


## <a name="introduction"></a>Úvod


Posledních dvou kurzy projít nasazení jednoduché webové aplikace. [ *Nasazení vaší lokality pomocí klienta FTP* ](deploying-your-site-using-an-ftp-client-cs.md) kurz vám ukázal, jak zkopírovat potřebné soubory z vývojového prostředí až produkční, použijte samostatné klienta FTP. Předchozí kurzu [ *nasazení vaše lokality pomocí sady Visual Studio*](deploying-your-site-using-visual-studio-cs.md), hledá v nasazení pomocí sady Visual Studio nástroj Kopírovat web a možností publikovat. V obou kurzech každý soubor v produkčním prostředí se kopie souboru ve vývojovém prostředí. Však není pro konfigurační soubory v provozním prostředí, abyste lišit od těch ve vývojovém prostředí. Konfigurace webové aplikace je uložená v `Web.config` souboru a obvykle obsahují informace o externím prostředkům, například databáze, webové a e-mailu servery. Také stanoví chování aplikace v určitých situacích, jako je postup, chcete-li provést, když dojde k neošetřené výjimce.

Při nasazení webové aplikace je důležité informace o správné konfiguraci skončili v provozním prostředí. Ve většině případů `Web.config` soubor ve vývojovém prostředí nelze zkopírovat do produkčního prostředí jako-je. Místo toho přizpůsobená verze `Web.config` musí být nahrán do produkčního prostředí. V tomto kurzu stručně zkontroluje některé z běžnějších konfigurace rozdíly; taky shrnuje některé techniky pro údržbu informace o konfiguraci se mezi těmito prostředími.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Obvyklá konfigurace rozdíly mezi vývoje a v produkčním prostředí

`Web.config` Soubor obsahuje širokou konfigurační informace pro aplikace ASP.NET. Některé z těchto informací je stejný bez ohledu na prostředí. Například nastavení ověřování a autorizačních pravidel adres URL jako slovo v `Web.config` souboru `<authentication>` a `<authorization>` elementy jsou obvykle stejné bez ohledu na prostředí. Ale jiné informace o konfiguraci – například informace o externích zdrojů - se obvykle liší v závislosti na prostředí.

Typickým příkladem informace o konfiguraci, která se liší podle prostředí jsou řetězce připojení databáze. Když webové aplikace komunikuje se serverem databáze, musíte nejprve vytvořit připojení, a který je dosaženo pomocí [připojovací řetězec](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). I když je možné zakódovat připojovací řetězec databáze přímo do webové stránky nebo kód, který se připojuje k databázi, je nejvhodnější pro jeho umístění `Web.config`na [ `<connectionStrings>` element](https://msdn.microsoft.com/library/bf7sd233.aspx) tak, aby připojovací řetězec informace jsou v jednom centralizovaného umístění. Často se do jiné databáze používá během vývoje, než se používá v produkčním prostředí; informace o připojovacím řetězci v důsledku toho musí být jedinečný pro každé prostředí.

> [!NOTE]
> Budoucí kurzy prozkoumejte nasazení aplikací datové od této chvíle jsme budete podrobně specifické údaje o tom, jak databázové připojovací řetězce jsou uložené v konfiguračním souboru.


O záměrné chování vývoj a produkční prostředí se podstatně liší. Webovou aplikaci ve vývojovém prostředí se vytváří, otestovat a vyladěnou podle malá skupina vývojářů. V produkčním prostředí tu samou aplikaci přístupu pomocí mnoha různým uživatelům současně. Technologie ASP.NET obsahuje řadu funkcí, které pomoci vývojářům při testování a ladění aplikace, ale tyto funkce by mělo být zakázáno z důvodů zabezpečení v provozním prostředí a výkonu. Podívejme se na několik taková nastavení konfigurace.

### <a name="configuration-settings-that-impact-performance"></a>Nastavení konfigurace s dopadem na výkon

Když je navštívené stránky ASP.NET pro první (nebo při prvním po se změnila), je nutno jeho deklarativní převést na třídu a tato třída musí být zkompilovány. Pokud webová aplikace používá automatickou kompilaci pak třídy kódu stránky musí být zkompilovány příliš. Můžete nakonfigurovat širokou možnosti kompilace prostřednictvím `Web.config` souboru [ `<compilation>` element](https://msdn.microsoft.com/library/s10awwz0.aspx).

Atribut ladění je jedním z nejdůležitějších atributů `<compilation>` elementu. Pokud `debug` atribut je nastaven na hodnotu true, pak kompilované sestavení obsahovat symboly ladění, které jsou potřeba při ladění aplikace v sadě Visual Studio. Ale symboly ladění zvětšete velikost sestavení a stanovit požadavky další paměť při spuštění kódu. Kromě toho, když `debug` atribut je nastaven na hodnotu true, žádný obsah vrácený `WebResource.axd` není v mezipaměti, což znamená, že pokaždé, když uživatel navštíví stránky, bude nutné znovu stáhnout statický obsah vrácený `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` je integrované obslužná rutina HTTP zavedená v technologii ASP.NET 2.0, ovládací prvky serveru použít k načtení vložené prostředky, jako jsou soubory skriptu, bitové kopie, soubory šablon stylů CSS a jiný obsah. Další informace o tom, `WebResource.axd` funguje a jak můžete použít pro přístup vložené prostředky ze vaše vlastní serverové ovládací prvky, najdete v části [přístup k vložených prostředků prostřednictvím adresu URL pomocí `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


`<compilation>` Elementu `debug` je obvykle nastavena na hodnotu "true" ve vývojovém prostředí. Ve skutečnosti tento atribut musí být nastaven na hodnotu true, pokud chcete ladit webové aplikace; Pokud se pokusíte ladění aplikace ASP.NET v sadě Visual Studio a `debug` atribut je nastaven na hodnotu "false", Visual Studio se zobrazí zpráva vysvětlení, že nemůžete dokud ladit aplikace `debug` je nastavena na hodnotu "true" a bude nabídka, aby pro vás provedl tuto změnu.

Měli byste **nikdy** mít `debug` nastaven atribut na hodnotu "true" v produkčním prostředí kvůli jeho dopad na výkon. Podrobnější informace k tomuto tématu, najdete v části [Scott Guthrie](https://weblogs.asp.net/scottgu/)na příspěvku na blogu [nemáte spustit produkční ASP.NET aplikace s `debug="true"` povoleno](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Vlastní chyby a trasování

Když dojde k neošetřené výjimce v aplikaci ASP.NET bubliny až do okamžiku jednu ze tří akcí se stane modulu runtime:

- Zobrazí se chybová zpráva obecný modul runtime. Tato stránka informuje uživatele, že existuje došlo k chybě modulu runtime, ale neobsahuje žádné podrobnosti o této chybě.
- Zobrazí se zprávou výjimky podrobnosti, což zahrnuje informace o výjimku, která se právě vyvolala.
- Zobrazí se vlastní chybovou stránku, která je stránka technologie ASP.NET, kterou vytvoříte, zobrazí jakékoli zprávy, které očekáváte.

Co se stane při krátkodobém nezpracovanou výjimku odvíjí `Web.config` souboru [ `<customErrors>` části](https://msdn.microsoft.com/library/h0hfz6fc.aspx).

Při vývoji a testování aplikace pomáhá zobrazíte podrobnosti jakékoli výjimky v prohlížeči. Zobrazuje podrobnosti o výjimce v aplikaci na produkční je však představuje potenciální bezpečnostní riziko. Kromě toho je unflattering a umožňuje neprofesionálně webu. V ideálním případě by v případě neošetřené výjimky webové aplikace ve vývojovém prostředí se zobrazí podrobnosti výjimky při stejnou aplikaci v produkčním prostředí se zobrazí vlastní chybovou stránku.

> [!NOTE]
> Výchozí hodnota `<customErrors>` část nastavení zobrazuje podrobnosti o výjimce zprávy jenom v případě, že na stránce přístupu prostřednictvím místního hostitele a zobrazí se chybová stránka Obecné runtime jinak. Tato akce není ideální, ale je zajištění vědět, že výchozí chování není odhalit podrobnosti výjimky pro návštěvníky není místní. Prozkoumá budoucí kurz `<customErrors>` části podrobněji a ukazuje, jak mají vlastní chybovou stránku zobrazí, když dojde k chybě v produkčním prostředí.


Jiné funkce ASP.NET, které jsou užitečné při vývoji je trasování. Trasování, pokud je povoleno, zaznamenává informace o jednotlivých příchozích požadavků a obsahuje speciální webové stránky, `Trace.axd`, k zobrazení nedávné podrobnosti požadavku. Můžete zapnout a konfigurovat trasování prostřednictvím [ `<trace>` element](https://msdn.microsoft.com/library/6915t83k.aspx) v `Web.config`.

Pokud povolíte trasování Ujistěte se, že to je zakázané v produkčním prostředí. Informace o trasování zahrnuje soubory cookie, data relace a další potenciálně citlivé informace, a proto je důležité pro vypnutí trasování v produkčním prostředí. Dobrá zpráva je, že ve výchozím nastavení, trasování je zakázáno a `Trace.axd` souboru je k dispozici pouze prostřednictvím místního hostitele. Změníte-li tato výchozí nastavení v vývoj Ujistěte se, že jsou vypnuté zpět v produkčním prostředí.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Techniky pro uchovávání informací samostatné konfigurace

S jinou konfiguraci nastavení v prostředí vývoje a produkční komplikuje procesu nasazení. V předchozích dvou kurzech procesu nasazení zahrnuta, všechny potřebné soubory kopírování z vývojového do produkčního prostředí, ale tento postup funguje jenom v případě, že informace o konfiguraci je stejná v obou prostředích. Existují různé postupy nasazení aplikace s různými informace o konfiguraci. Pojďme katalogu některé z těchto možností pro hostované webové aplikace.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Ruční nasazení konfiguračního souboru produkční prostředí

Nejjednodušší způsob je udržovat dvě verze `Web.config` souboru: jeden pro vývojové prostředí a jeden pro produkční prostředí. Nasazení lokality do produkčního prostředí zahrnuje kopírování všech souborů na provozním serveru ve vývojovém prostředí *s výjimkou* pro `Web.config` souboru. Místo toho produkční prostředí specifické pro `Web.config` soubor zkopírován do produkčního prostředí.

Tento přístup není velmi složité, ale lze snadno implementovat, protože informace o konfiguraci změní zřídka. Je nejvhodnější pro aplikace s malé vývojový tým, které jsou hostované na jednom webovém serveru a jejichž informace o konfiguraci se zřídka mění. Většina chráněném je při nasazování ručně pomocí klienta FTP samostatné soubory aplikace. Při použití nástroje Kopírovat web nebo možností publikovat sady Visual Studio budete muset nejdřív vyměnit specifické pro nasazení `Web.config` soubor s jedním specifické pro produkční před nasazením a pak je Prohodit zpět po dokončení nasazení.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Změna konfigurace během sestavení nebo procesu nasazení

Diskuse doposud mít předpokládá procesem sestavení a nasazení ad hoc. Mnoho větší softwaru projekty mají více stanovení procesy, které používá open source, zvětšil domovské nebo nástroje třetích stran. Pro tyto projekty můžete přizpůsobit pravděpodobně procesu sestavení nebo nasazení odpovídajícím způsobem upravit informace o konfiguraci, než je vložena do produkčního prostředí. Pokud vytvoříte webovou aplikaci pomocí [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), nebo nějaký jiný nástroj sestavení je pravděpodobně přidat krok sestavení změnit `Web.config` souboru produkční specifická nastavení. Pracovní postup nasazení může prostřednictvím kódu programu připojit k řízení zdrojového serveru a načíst odpovídající `Web.config` souboru.

Široce závislosti na nástroje a pracovní postup se liší skutečné přístup pro získávání informací o správné konfiguraci do produkčního prostředí. Jako takový jsme nebude pustíte do dále v tomto tématu. Pokud jsou pomocí Oblíbené sestavení nástroje, například nástroje MSBuild nebo NAnt najdete články nasazení a kurzy specifická pro tyto nástroje prostřednictvím vyhledávání na webu.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Správa konfigurace rozdíly prostřednictvím nasazení webového projektu doplňku

V 2006 Společnost Microsoft vydala webové vývoj projektu doplňku pro Visual Studio 2005. Doplněk pro Visual Studio 2008 byla vydána v 2008. Add-In umožňuje vývojářům ASP.NET vytvořit samostatné nasazení webového projektu spolu s jejich projekt webové aplikace, která, pokud vytvořené, explicitně zkompiluje webové aplikace a zkopíruje soubory, které chcete nasadit do místní výstupního adresáře. Projekt webové aplikace pomocí nástroje MSBuild na pozadí.

Ve výchozím nastavení, vývojového prostředí pro `Web.config` soubor zkopírován do výstupního adresáře, ale můžete nastavit nasazení webového projektu k přizpůsobení

informace o konfiguraci, která se zkopírují do tohoto adresáře následujícími způsoby:

- Prostřednictvím `Web.config` souboru nahrazení části, ve kterém můžete zadat oddílu, který má nahradit a soubor XML, který obsahuje Nahrazovací text.
- Tím, že poskytuje cestu ke zdrojové externí konfigurační soubor. Vybraná tato možnost zkopíruje webového projektu nasazení konkrétní `Web.config` souboru do výstupního adresáře (místo `Web.config` soubor použitý ve vývojovém prostředí).
- Přidáním vlastní pravidla k souboru MSBuild používané webovým projektem nasazení.

Nasazení webové aplikace sestavení webového projektu nasazení a poté zkopírujte soubory ze složky projektu výstup do produkčního prostředí.

Další informace o použití nasazení webového projektu podívejte se na [v tomto článku webové projekty nasazení](https://msdn.microsoft.com/magazine/cc163448.aspx) z vydání duben 2007 [Časopis MSDN](https://msdn.microsoft.com/magazine/default.aspx), nebo se podívejte na odkazy v části Další čtení na konce tohoto kurzu.

> [!NOTE]
> Nasazení webového projektu nelze použít s Visual Web Developer, protože webový projekt nasazení je implementovaný jako Visual Studio Add-In a Visual Studio Express Edition (včetně Visual Web Developer) nepodporují doplňků.


## <a name="summary"></a>Souhrn

Externí zdroje a chování při vývoji webové aplikace se obvykle liší od Pokud stejná aplikace je v produkčním prostředí. Například databázové připojovací řetězce, možnosti kompilace a chování, když dojde k neošetřené výjimce běžně liší mezi prostředími. Proces nasazení musí zohlednit tyto rozdíly. Jak již bylo zmíněno v tomto kurzu, je nejjednodušší je ručně zkopírovat soubor alternativní konfigurací do produkčního prostředí. Je možné více elegantní řešení, když používáte Web nasazení projektu doplňku nebo s více formalizovanou procesu sestavení nebo nasazení, který zvládne takové úpravy.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Vysvětlení připojovací řetězce](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [Připojení k databázi řetězce @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Nespouštět produkční aplikace ASP.NET s `debug="true"` povoleno](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Řádně neodpovídá na požadavky neošetřených výjimek - zobrazení uživatelsky přívětivý chybové stránky](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Jak na: použití projekt webové nasazení sady Visual Studio 2008?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Nastavení klíče konfigurace při nasazení databází](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008 nasazení projekty stažením z webu](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Visual Studio 2005 nasazení projekty stažením z webu](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Projekty nasazení webu 2008 VS](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [VS 2008 webového projektu podpora nasazení vydání](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Projekty nasazení webu](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Předchozí](deploying-your-site-using-visual-studio-cs.md)
> [další](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
