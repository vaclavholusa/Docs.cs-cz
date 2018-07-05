---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
title: Nejčastější rozdíly mezi vývojovou a provozní (VB) konfigurací | Dokumentace Microsoftu
author: rick-anderson
description: V předchozích kurzech jsme nasadili našeho webu tak, že zkopírujete všechny relevantní soubory z vývojového prostředí do produkčního prostředí. Ale můžu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 548e75f6-4d6c-4cb4-8da8-417915eb8393
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
msc.type: authoredcontent
ms.openlocfilehash: 0e587853e77c1d6e21e787aae417c0978b1b957d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362736"
---
<a name="common-configuration-differences-between-development-and-production-vb"></a>Běžné konfigurace rozdíly mezi vývojovou a provozní (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhnout PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_vb.pdf)

> V předchozích kurzech jsme nasadili našeho webu tak, že zkopírujete všechny relevantní soubory z vývojového prostředí do produkčního prostředí. To však není existovat konfigurace rozdíly mezi prostředími, které vyžaduje, aby každé prostředí mají jedinečný soubor Web.config. V tomto kurzu zkoumá Typická konfigurace rozdíly a zkoumá strategie pro udržování samostatné konfigurační informace.


## <a name="introduction"></a>Úvod


Poslední dva kurzy prošli nasazení jednoduché webové aplikace. [ *Nasazení webu pomocí klienta FTP* ](deploying-your-site-using-an-ftp-client-vb.md) kurz vám ukázal, jak používat samostatný klient FTP pro kopírování potřebné soubory z vývojového prostředí až po produkční. Předchozím kurzu [ *nasazení webu pomocí sady Visual Studio*](deploying-your-site-using-visual-studio-vb.md), podívali se na nasazení pomocí nástroje pro kopírování webu sady Visual Studio a možností publikovat. V obou kurzech každý soubor v produkčním prostředí se kopie souboru ve vývojovém prostředí. To však není, konfigurační soubory v produkčním prostředí tak, aby se liší od těch, které ve vývojovém prostředí. Konfigurace webové aplikace je uložena v `Web.config` souboru a obvykle obsahuje informace o externích zdrojů, jako je například databáze, web a servery e-mailu. Obsahuje také chování aplikace v určitých situacích, jako je například kurz akce se má provést při dojde k neošetřené výjimce.

Při nasazení webové aplikace je důležité informace o správné konfiguraci ukládaly do produkčního prostředí. Ve většině případů `Web.config` soubor ve vývojovém prostředí nelze zkopírovat do produkčního prostředí jako-je. Místo toho přizpůsobenou verzi `Web.config` musí být odeslán do produkčního prostředí. V tomto kurzu stručně kontroly některé z běžnějších rozdíly konfigurací; taky shrnuje některé postupy pro udržování různých konfiguračních informací mezi prostředími.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Typická konfigurace rozdíly mezi vývojovou a provozní prostředí

`Web.config` Soubor obsahuje celé řady různých doprovodných informace o konfiguraci aplikace ASP.NET. Některé tyto informace o konfiguraci je stejný bez ohledu na prostředí. Například nastavení ověřování a autorizačních pravidel adres URL států v `Web.config` souboru `<authentication>` a `<authorization>` prvky jsou obvykle stejné bez ohledu na prostředí. Ale dalších konfiguračních údajů – například informace o externí prostředky – obvykle se liší v závislosti na prostředí.

Typickým příkladem informací o konfiguraci, která se liší podle prostředí jsou řetězce připojení databáze. Když webové aplikace komunikuje se serverem databáze musí nejprve navázat připojení a, který se dosahuje prostřednictvím [připojovací řetězec](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). I když je možné pevně kódovat připojovací řetězec databáze přímo do webových stránek nebo kód, který se připojuje k databázi, je nejlepší umístit `Web.config`společnosti [ `<connectionStrings>` element](https://msdn.microsoft.com/library/bf7sd233.aspx) tak, aby připojovací řetězec informace jsou v jednom centralizované umístění. Často jinou databázi se používá během vývoje. než se používá v produkčním prostředí; informace o připojovacím řetězci v důsledku toho musí být jedinečný pro každé prostředí.

> [!NOTE]
> Budoucí kurzy prozkoumání, nasazení aplikace řízené daty, v tomto okamžiku začneme budete zabývat, jaké jsou specifikace jak databázové připojovací řetězce jsou uložené v konfiguračním souboru.


Zamýšlené chování vývojovém a produkčním prostředí se podstatně liší. Webové aplikace ve vývojovém prostředí se vytváří, otestovat a ladění malá skupina vývojářů. V produkčním prostředí se tu samou aplikaci návštěvě mnoho různých souběžných uživatelů. Technologie ASP.NET obsahuje řadu funkcí, které pomůžou vývojářům při testování a ladění aplikace, ale tyto funkce by mělo být zakázáno pro výkon a z bezpečnostních důvodů v produkčním prostředí. Podívejme se na několik těchto nastavení konfigurace.

### <a name="configuration-settings-that-impact-performance"></a>Nastavení konfigurace, které ovlivňují výkon

Při návštěvě stránky ASP.NET pro první (nebo prvním spuštění po došlo k jeho změně), jeho deklarativním označení musí být převeden do třídy a tato třída musí být zkompilovány. Pokud webová aplikace používá automatické kompilace na stránce použití modelu code-behind třídy musí ke kompilaci, příliš. Můžete nakonfigurovat celé řady různých doprovodných možnosti kompilace prostřednictvím `Web.config` souboru [ `<compilation>` element](https://msdn.microsoft.com/library/s10awwz0.aspx).

Atribut ladění je jedním z vašich nejdůležitějších atributů `<compilation>` elementu. Pokud `debug` atribut je nastaven na "true", zkompilovaná sestavení zahrnout symboly ladění, které jsou potřeba při ladění aplikace v sadě Visual Studio. Ale symboly ladění zvětšují velikost sestavení a ukládat požadavky na další paměť při spuštění kódu. Kromě toho, kdy `debug` atribut je nastaven na "true" veškerý obsah vrácený `WebResource.axd` neukládají do mezipaměti, což znamená, že pokaždé, když uživatel navštíví stránku bude potřeba znovu si stáhněte statický obsah vrácený `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` integrované obslužná rutina HTTP přišla v technologii ASP.NET 2.0, který ovládací prvky serveru použijte k načtení vložených prostředků, jako jsou soubory skriptu, obrázky, soubory šablon stylů CSS a další obsah. Další informace o tom, `WebResource.axd` funguje a jak ho použít pro přístup k vložené prostředky z vašich vlastních serverových ovládacích prvků naleznete v tématu [přístup k vložené prostředky prostřednictvím adresu URL pomocí `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


`<compilation>` Elementu `debug` atribut je obvykle nastaven na hodnotu "true" ve vývojovém prostředí. Ve skutečnosti tento atribut musí být nastaven na "true", aby bylo možné ladit webové aplikace; Pokud se pokusíte ladit aplikace ASP.NET ze sady Visual Studio a `debug` atribut je nastaven na hodnotu "false", Visual Studio se zobrazí zpráva vysvětlením, že aplikace nejde ladit, dokud `debug` atribut je nastaven na "true" a bude nabídka je k provedení této změny za vás.

Měli byste **nikdy** mít `debug` nastavený atribut na hodnotu "true" v produkčním prostředí z důvodu jeho dopad na výkon. Podrobnější informace o tomto tématu najdete [Scott Guthrie](https://weblogs.asp.net/scottgu/)pro blogový příspěvek [není spustit produkční ASP.NET aplikace s `debug="true"` povoleno](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Vlastní chyby a trasování

Když dojde k neošetřené výjimce v aplikaci ASP.NET bubliny až do modulu runtime, alespoň jednu ze tří akcí se stane:

- Zobrazí se chybová zpráva obecný modul runtime. Tato stránka informuje uživatele, aby došlo k chybě modulu runtime, ale neposkytuje žádné podrobnosti o chybě.
- Zobrazí se podrobnosti o zprávu o výjimce, který obsahuje informace o výjimce, která byla právě vydána.
- Zobrazí se vlastní chybovou stránku, což je stránka technologie ASP.NET, který vytvoříte, který zobrazuje všechny zprávy, které očekáváte.

Co se stane, že i v případě neošetřené výjimce závisí `Web.config` souboru [ `<customErrors>` části](https://msdn.microsoft.com/library/h0hfz6fc.aspx).

Při vývoji a testování aplikace umožňuje zobrazit podrobnosti o jakoukoli výjimku v prohlížeči. Zobrazuje podrobnosti o výjimce v aplikaci v produkčním prostředí je však možné bezpečnostní riziko. Kromě toho je unflattering a díky neprofesionálně webu. V ideálním případě by v případě neošetřené výjimce webové aplikace ve vývojovém prostředí se zobrazí podrobnosti o této výjimky během stejné aplikace v produkčním prostředí se zobrazí vlastní chybové stránky.

> [!NOTE]
> Výchozí hodnota `<customErrors>` část nastavení se zobrazí podrobnosti o výjimce zprávy pouze v případě, že na stránce přístupu prostřednictvím místního hostitele a v opačném případě se zobrazí chybová stránka obecný modul runtime. To není ideální, ale je ujištěním vědět, že výchozí chování není zobrazit podrobnosti o výjimce pro jiné než místní návštěvníky. Budoucí kurz zkoumá `<customErrors>` části podrobněji a ukazuje, jak mají vlastní chybové stránky zobrazí, když dojde k chybě v produkčním prostředí.


Další funkcí technologie ASP.NET, které jsou užitečné při vývoji se trasování. Trasování, pokud je povoleno, zaznamenává informace o jednotlivých příchozích požadavků a poskytuje zvláštní webové stránky, `Trace.axd`, pro zobrazení Podrobnosti o poslední žádosti. Můžete zapnout a nakonfigurovat trasování prostřednictvím [ `<trace>` element](https://msdn.microsoft.com/library/6915t83k.aspx) v `Web.config`.

Pokud povolíte trasování Ujistěte se, že to je zakázané v produkčním prostředí. Informace o trasování zahrnuje soubory cookie, data relace a další potenciálně citlivé informace, a proto je důležité pro vypnutí trasování v produkčním prostředí. Dobrou zprávou je, že ve výchozím nastavení, trasování je zakázáno a `Trace.axd` soubor je k dispozici pouze prostřednictvím místního hostitele. Je-li změnit toto výchozí nastavení při vývoji Ujistěte se, že jsou vypnuté zpět v produkčním prostředí.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Techniky pro zachování informací o samostatné konfiguraci

S různá nastavení konfigurace ve vývojovém a produkčním prostředí komplikuje proces nasazení. V předchozích kurzech dvě proces nasazení týká, všechny potřebné soubory kopírování z vývojového do produkčního prostředí, ale tento přístup funguje, pouze pokud informace o konfiguraci jsou stejné v obou prostředích. Existuje řada různých technik pro nasazení aplikace s různými informace o konfiguraci. Pojďme katalogu některé z těchto možností pro hostované webové aplikace.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Ruční nasazení konfiguračního souboru produkční prostředí

Nejjednodušším způsobem je udržovat dvě verze `Web.config` souboru: jeden pro vývojové prostředí a druhý pro produkční prostředí. Nasazení webu do produkčního prostředí zahrnuje kopírování všech souborů na produkční server ve vývojovém prostředí *s výjimkou* pro `Web.config` souboru. Místo toho na produkční prostředí specifické pro `Web.config` souboru má být zkopírováno do produkčního prostředí.

Tento přístup není velmi složité, ale snadno implementovat, protože informace o konfiguraci mění jen zřídka. Je nejvhodnější pro aplikace s malý vývojový tým, které jsou hostované na jednom webovém serveru a jejichž informace o konfiguraci se zřídka mění. Bylo nejvíce chráněném při ruční nasazení aplikace soubory pomocí samostatného klienta FTP. Při použití nástroje pro kopírování webu sady Visual Studio nebo možností publikovat budete muset nejdřív použít specifické pro nasazení `Web.config` souboru s kategorií specifické pro produkční před nasazením a po dokončení nasazení je prohodit.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Změna konfigurace během sestavování nebo procesu nasazení

Diskuse doposud jste předpokládá, že proces sestavení a nasazení ad-hoc. Větší mnoha softwarových projektech mají více stanovení procesy, které zajišťují použití open sourcové uživatelské, nebo nástrojů třetích stran. Tyto projekty můžete pravděpodobně přizpůsobit proces sestavení nebo nasazení odpovídajícím způsobem upravit informace o konfiguraci, předtím, než se vloží do produkčního prostředí. Pokud vytváříte webové aplikace pomocí [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), nebo nějaký jiný nástroj sestavení, je pravděpodobně přidat krok sestavení změnit `Web.config` soubor zahrnout nastavení specifická pro produkční. Váš pracovní postup nasazení může programově připojit k serveru správy zdrojů a načíst odpovídající `Web.config` souboru.

Skutečný přístup k zařazení správné konfigurační informace do produkčního prostředí se liší běžně podle nástroje a pracovního postupu. V důsledku toho jsme se pustíte do dále v tomto tématu. Pokud používáte sestavení oblíbené nástroje, jako je MSBuild nebo NAnt najdete články nasazení a kurzy specifická pro tyto nástroje prostřednictvím vyhledávání na webu.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Správa konfigurace rozdíly pomocí nasazení webu projektu doplňku

V 2006 Společnost Microsoft vydala Add-In vývoj webového projektu pro Visual Studio 2005. Doplněk pro sadu Visual Studio 2008 byla vydána v roce 2008. Add-In umožňuje pro vývojáře využívající technologii ASP.NET vytvořit samostatný webový projekt nasazení společně s jejich projektu webové aplikace, po sestavení, explicitně zkompiluje webové aplikace a zkopíruje soubory, které chcete nasadit do místního výstupního adresáře. Projekt webové aplikace pomocí nástroje MSBuild na pozadí.

Ve výchozím nastavení, vývojové prostředí společnosti `Web.config` soubor je zkopírován do výstupního adresáře, ale můžete nastavit nasazení webového projektu přizpůsobení

informace o konfiguraci, který se zkopíruje do tohoto adresáře následujícími způsoby:

- Prostřednictvím `Web.config` souborů nahrazení oddílu, ve kterém můžete zadat oddílu, který má nahradit a soubor XML, který obsahuje náhradní text.
- Zadáním cesty k souboru zdrojového externí konfigurace. Tato možnost aktivní, webový projekt nasazení zkopíruje konkrétní `Web.config` soubor do výstupního adresáře. (místo `Web.config` soubor se používá ve vývojovém prostředí).
- Přidáním vlastních pravidel do souboru MSBuild používaný nasazení webového projektu.

Nasazení webové aplikace sestavení projektu nasazení webu a pak zkopírujte soubory ze složky výstup projektu do produkčního prostředí.

Další informace o použití projektu nasazení webu, projděte si [v tomto článku projekty nasazení webu](https://msdn.microsoft.com/magazine/cc163448.aspx) od vydání duben 2007 [Zpravodaj MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx), nebo se obraťte na odkazy v části Další čtení na účelem tohoto kurzu.

> [!NOTE]
> Nasazení webového projektu nelze použít s aplikaci Visual Web Developer, protože nasazení webového projektu je implementovaný jako Visual Studio Add-In a Visual Studio Express (včetně edice Visual Web Developer) nepodporují Add-Ins.


## <a name="summary"></a>Souhrn

Externí prostředky a chování při vývoji webové aplikace se obvykle liší od kdy je tu samou aplikaci v produkčním prostředí. Například databázové připojovací řetězce, možnosti kompilace a chování, když dojde k neošetřené výjimce běžně lišit mezi prostředími. Proces nasazení musí přizpůsobení těchto rozdílů. Jak jsme probírali v tomto kurzu, nejjednodušším přístupem je ručně zkopírovat soubor alternativní konfigurace do produkčního prostředí. Elegantnější řešení je možné při použití na webové nasazení projektu doplňku nebo s více formalizovanou proces sestavení nebo nasazení, který zvládne tyto úpravy.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Vysvětlení připojovacích řetězců](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [Databázové připojovací řetězce @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Nelze spustit produkční aplikace ASP.NET s `debug="true"` povoleno](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Řádně zpracování neošetřené výjimky - zobrazení uživatelsky přívětivé chybové stránky](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Postup: použití projekt webové nasazení sady Visual Studio 2008?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Nastavení konfigurace klíče při nasazení databáze](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008 nasazení projekty stažením z webu](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Visual Studio 2005 nasazení projekty stažením z webu](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Projekty nasazení webu 2008 VS](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [VS 2008 webové nasazení projektu podporu všeobecně dostupné](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Projekty nasazení webu](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Předchozí](deploying-your-site-using-visual-studio-vb.md)
> [další](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
