---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
title: Konfigurace provozní webové aplikace pro použití provozní databáze (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Jak je popsáno v předchozích kurzech, není, informací o konfiguraci se liší mezi vývojovou a provozní prostředí. Toto je es...
ms.author: aspnetcontent
ms.date: 04/23/2009
ms.assetid: a64a7aa0-6608-449e-83bf-1ef8cceee504
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 952bdafd06143ce24e9e8beb0d6e6343400ffdc6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840697"
---
<a name="configuring-the-production-web-application-to-use-the-production-database-vb"></a>Konfigurace provozní webové aplikace pro použití provozní databáze (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_VB.zip) nebo [stahovat PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_vb.pdf)

> Jak je popsáno v předchozích kurzech, není, informací o konfiguraci se liší mezi vývojovou a provozní prostředí. To platí zejména pro datově řízených webových aplikací, jako připojovací řetězce databáze se liší mezi vývojovou a provozní prostředí. Tento kurz se věnuje způsobům konfigurace produkční prostředí, aby zahrnovalo příslušný připojovací řetězec podrobněji.


## <a name="introduction"></a>Úvod

Do jiné databáze při vývoji než když datově řízených webových aplikací obvykle používají v produkčním prostředí. Pro aplikace hostované ve zprostředkovateli webového hostitele a vyvinutý místně databázi vývoj se obvykle nachází v počítači s developer během provozní databáze hostována na databázovém serveru na webu hostování firemní zařízení s. Nasazení s daty webové aplikace zahrnuje kopírování vývoje databáze na produkční server databáze. V předchozím kurzu jsme se podívali na způsoby provedení tohoto kroku.

Webová aplikace používá informace *připojovací řetězec* k navázání připojení k databázi. Připojovací řetězec, který je obvykle uložen ve `Web.config`, určuje název databázového serveru, název databáze, kontextu zabezpečení a další informace. Protože databázi použitou vydavatelem webové aplikace závisí na tom, zda je spuštěna webová aplikace v prostředích vývojové nebo produkční prostředí, připojovací řetězce se musí lišit mezi těmito dvěma prostředími.

Není, informací o konfiguraci se liší mezi vývojovou a provozní prostředí. *Společné konfigurace rozdíly mezi vývojovou a provozní* kurzu popsané techniky pro údržbu na samostatné konfiguračních informací mezi těmito dvěma prostředími, stejně jako stručný popis databázové připojovací řetězce. Tento kurz se věnuje způsobům konfigurace produkční prostředí, aby zahrnovalo příslušný připojovací řetězec podrobněji.

## <a name="examining-the-connection-string-information"></a>Kontrola informací o připojovacím řetězci

Připojovací řetězec používaný projektem recenzí webová aplikace je uložena v konfiguračním souboru aplikace s `Web.config`. `Web.config` obsahuje speciální oddíl pro ukládání připojovacích řetězců, vhodně pojmenované [ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). `Web.config` Soubor pro web recenzí má jeden připojovacího řetězce definovaný v této části s názvem `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample1.xml)]

Připojovací řetězec – zdroj dat =. \SQLEXPRESS; AttachDbFilename = | Zabezpečení DataDirectory|\Reviews.mdf;Integrated = True; User Instance = True – s páry možnost/Hodnota, které jsou odděleny středníkem a jednotlivé možnosti a hodnoty oddělené symbolem rovná se skládá z možnosti a hodnoty, počtu. Čtyři možnosti používané v tomto připojovacím řetězci jsou:

- `Data Source` -Určuje umístění databázového serveru a název instance databázového serveru (pokud existuje). Hodnota, `.\SQLEXPRESS`, je příkladem níž se nachází server databáze a název instance. Období určuje, že databázový server je ve stejném počítači jako aplikaci. Název instance je `SQLEXPRESS`.
- `AttachDbFilename` -Určuje umístění souboru databáze. Hodnota obsahuje zástupný symbol `|DataDirectory|`, který je přeložen na úplnou cestu aplikace s `App_Data` složky v době běhu.
- `Integrated Security` -Logická hodnota, která určuje, jestli se má používat zadaného uživatelského jména a hesla při připojování k databázi (false) nebo Windows aktuální přihlašovací údaje účtu (pravda).
- `User Instance` -možnost konfigurace specifické pro SQL Server Express edice, která určuje, jestli se má povolit uživatelům bez oprávnění správce na místním počítači připojení a připojení k databázi SQL Server Express Edition. Zobrazit [instance SQL serveru Express uživatele](https://msdn.microsoft.com/library/ms254504.aspx) pro další informace o tomto nastavení.
  

Možnosti povolené připojovacího řetězce závisí na databázi se připojujete k a [ADO.NET](http://ADO.NET) použitý zprostředkovatel databáze. Například připojovací řetězec pro připojení k serveru Microsoft SQL Server se liší od databáze, která používá pro připojení k databázi Oracle. Podobně připojení k databázi serveru Microsoft SQL Server pomocí poskytovatel Sqlclienta používá řetězec jiné připojení než při použití zprostředkovatele OLE DB.

Připojovací řetězec databáze můžete vytvořit ručně pomocí web [ConnectionStrings.com](http://www.connectionstrings.com/) jako prostředek, pro jaké možnosti jsou k dispozici. Jednodušší přístup je však přidat databázi do Průzkumníka serveru v sadě Visual Studio a potom zkopírujte připojovací řetězec z okna Vlastnosti. Umožní používat tuto techniku odkazující sestavení připojovacího řetězce na produkční server databáze s.

Otevřít Visual Studio a přejděte do okna Průzkumník serveru (v aplikaci Visual Web Developer, toto okno se nazývá Průzkumník databáze). Klikněte pravým tlačítkem na možnost datová připojení a zvolte možnost Přidat připojení z místní nabídky. Tím se zobrazí Průvodce na obrázku 1. Vyberte příslušný zdroj dat a klikněte na pokračovat.


[![Zvolte možnost pro přidání nové databáze do Průzkumníka serveru](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image1.jpg) 

**Obrázek 1**: Zvolte možnost pro přidání nové databáze do Průzkumníka serveru ([kliknutím ji zobrazíte obrázek v plné velikosti](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image3.jpg))


Dále určete různé informace o připojení k databázi (viz obrázek 2). Pokud jste zaregistrovali pomocí webového hostování společnosti, které by měl mít poskytl informace o tom, jak připojit k databázi – název databázového serveru, název databáze, uživatelské jméno a heslo pro připojení k databázi a tak dále. Po zadání těchto informací klikněte na tlačítko OK, chcete-li dokončit tohoto průvodce a přidejte databáze do Průzkumníka serveru.


[![Zadejte informace o připojení k databázi](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image4.jpg) 

**Obrázek 2**: Zadejte informace o připojení k databázi ([kliknutím ji zobrazíte obrázek v plné velikosti](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image6.jpg))


Produkční databáze prostředí by měl nyní obsažena v Průzkumníku serveru. Vyberte databázi z Průzkumníku serveru a přejděte do okna Vlastnosti. Zde najdete vlastnost s názvem připojovacího řetězce připojovacím řetězcem s databází. Za předpokladu, že používáte databáze Microsoft SQL Server v produkčním prostředí a poskytovatel Sqlclienta připojovací řetězec by měl vypadat nějak takto:

<strong>Zdroj dat =<em>serverName</em>; Initial Catalog =<em>databaseName</em>; Trvalé informace o zabezpečení = True; ID uživatele =<em>uživatelské jméno</em>; Heslo =*heslo</strong>*

Kde *serverName*, *databaseName*, *uživatelské jméno*, a *heslo* jsou hodnoty pro název databázového serveru, databáze název a uživatelské jméno a heslo, které vám poskytne vaše společnost webového hostitele.

## <a name="deploying-the-book-reviews-web-application"></a>Nasazení webové aplikace revize knihy

V předchozím kurzu prošli kopírování vývoje databáze do produkčního prostředí, ale není prozkoumat nasazení aplikace řízené daty. V tomto okamžiku v provozním prostředí obsahuje databázi, ale je pomocí statické kontroly verze knihy revize aplikace. Potřebujeme k nasazení aplikace nový, s daty na produkční server spolu s informacemi o aktualizovanou konfiguraci.

Využijte k nasazení aplikace s daty z vývojového prostředí do produkčního prostředí. Tento proces je podrobně popsána v předchozích kurzech. Pokud potřebujete občerstvit, přečtěte si *nasazení webu pomocí klienta FTP* nebo *nasazení webu pomocí sady Visual Studio* kurzy. Budete muset Ujistěte se, že připojovací řetězec databáze produkční jeden použitý v produkčním prostředí, což znamená, že alternativu `Web.config` soubor musí být nasazen. Konkrétně to upravit `Web.config` soubor s `<connectionStrings>` element musí obsahovat řetězec připojení k produkční databázi a by měl vypadat nějak takto:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample2.xml)]

Všimněte si, že připojovací řetězec v `<connectionStrings>` element se stejným názvem (`ReviewsConnectionString`), ale nyní obsahuje připojovací řetězec databáze produkční místo připojovací řetězec databáze vývoje.

Pokud máte více formalizovanou pracovní postup nasazení, buď ručně změnit `Web.config` soubor má být použit řetězec připojení k produkční databázi před nasazením (zapamatování vrátit zpět k používání připojovací řetězec databáze vývoje později) nebo udržovat samostatnou `Web.config` soubor s produkčním prostředí informace o konfiguraci nahrán jako součást procesu nasazení do produkčního prostředí.

> [!NOTE]
> Pokud omylem nasazení `Web.config` soubor, který obsahuje připojovací řetězec databáze vývoj pak bude k chybě při aplikace v produkčním prostředí se pokusí připojit k databázi. Tato chyba manifesty jako `SqlException` a zobrazí se zpráva oznámení, že server nebyl nalezen nebo nebyl přístupný.


Po nasazení webu do produkčního prostředí, přejděte na web produkční prostřednictvím prohlížeče. By měl zobrazit a využívat stejné prostředí pro uživatele jako při místním spuštění aplikace řízené daty. Samozřejmě při návštěvě webu na produkční lokality využívá k tomu provozní server databáze, zatímco navštívit web ve vývojovém prostředí používá databázi ve vývoji. Obrázek 3 ukazuje *naučit sami technologie ASP.NET 3.5 za 24 hodin* zkontrolovat stránku na webu v produkčním prostředí (Poznámka: adresu URL v adresním řádku prohlížeče s).


[![Data-Driven aplikace je nyní k dispozici v produkčním prostředí.](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image7.jpg) 

**Obrázek 3**: The Data-Driven aplikace je nyní k dispozici v produkčním prostředí. ([Kliknutím ji zobrazíte obrázek v plné velikosti](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Ukládání připojovacích řetězců do samostatného konfiguračního souboru

Běžná technika pro udržování samostatné konfigurační informace o vývojovém a produkčním prostředí je mít dvě verze `Web.config`: jeden pro vývojové prostředí a druhý pro produkční prostředí. Při nasazení příslušné `Web.config` verze je možné zkopírovat do produkčního prostředí. V ideálním případě by se tento proces automatizovat jako součást pracovního postupu nasazení.

Namísto zachování dva samostatné `Web.config` soubory můžete volitelně zadat podrobnější rozdíly. Prvky, které tvoří `Web.config` soubor lze definovat v externí konfigurační soubory, které se pak odkazuje v `Web.config` souboru. Řečeno v kostce může mít jednu `Web.config` soubor pro obě prostředí, která odkazuje na soubor databaseConnectionStrings.config, který bude obsahovat připojovací řetězce v aplikaci použít a bude jedinečný pro každé prostředí. Můžu najít, že poskytuje oddělení do samostatných souborů různé informace o konfiguraci tidier a jednodušší `Web.config` souboru a další jasně popisuje rozdíly mezi vývojovou a provozní prostředí konfigurací.

Pokud chcete používat tuto techniku, začněte tím, že vytvoříte novou složku ve webové aplikaci s názvem `ConfigSections`. V dalším kroku přidejte do této nové složky s názvem databaseConnectionStrings.dev.config a databaseConnectionStrings.production.config dva soubory. V dalším kroku zkopírujte `<connectionStrings>` element z `Web.config` do databaseConnectionStrings.dev.config a databaseConnectionStrings.production.config souborů a potom změňte připojovací řetězec databaseConnectionStrings.production.config souboru tak, aby Určuje připojovací řetězec databáze produkčního prostředí. Například soubor databaseConnectionStrings.dev.config by měl obsahovat jenom `<connectionStrings>` element připojovacím řetězcem, který odkazuje na databázi vývoj:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample3.xml)]

Obdobně databaseConnectionStrings.production.config soubor by měl obsahovat pouze `<connectionStrings>` element, ale ten, který se má řetězec připojení k produkční databázi.

Vytvořte kopii souboru databaseConnectionStrings.dev.config a pojmenujte ho databaseConnectionStrings.config.

> [!NOTE]
> Můžete pojmenovat konfiguračního souboru něco jiného než databaseConnectionStrings.config, pokud d rádi používáte, jako například `connectionStrings.config` nebo `dbInfo.config`. Určitě chcete pojmenovat soubor s `.config` rozšíření jako `.config` soubory, ve výchozím nastavení, nejsou obsluhovány modul ASP.NET. Pokud zadáte soubor jiný název, třeba `connectionStrings.txt`, uživatel může odkazovat prohlížeč tak, aby [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) a zobrazit obsah souboru!


V tomto okamžiku `ConfigSections` složka by měla obsahovat tři soubory (viz obrázek 4). Soubory databaseConnectionStrings.dev.config a databaseConnectionStrings.production.config obsahují připojovací řetězce pro vývoj a provoz prostředí, v uvedeném pořadí. DatabaseConnectionStrings.config soubor obsahuje informace o připojovacím řetězci, který se použije webové aplikace za běhu. V důsledku toho databaseConnectionStrings.config soubor by měl být shodný se souborem databaseConnectionStrings.dev.config ve vývojovém prostředí, zatímco u produkčních databaseConnectionStrings.config soubor by měl být stejný jako databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image10.jpg) 

**Obrázek 4**: ConfigSections ([kliknutím ji zobrazíte obrázek v plné velikosti](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image12.jpg))


Nyní potřebujeme dáte pokyn, aby `Web.config` použít soubor databaseConnectionStrings.config pro její připojovací řetězec úložiště. Otevřít `Web.config` a nahraďte existující `<connectionStrings>` element následujícím kódem:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample4.xml)]

`configSource` Atribut určuje fyzickou cestu vzhledem k `Web.config` souboru. Pokud externí `.config` soubor je ve stejném adresáři jako `Web.config` tento atribut nastavte na název souboru `.config` souboru. Pokud ho s v podadresáři, jako je tomu u databaseConnectionStrings.config, zadejte podsložky pro vymezení názvů složek a souborů, jako je ConfigSections\databaseConnectionStrings.config pomocí zpětného lomítka.

Tato změna vývojovém a produkčním prostředí obsahovat stejné `Web.config` souboru. Jediným rozdílem je nyní databaseConnectionStrings.config souboru. Zkopírujte soubor databaseConnectionStrings.production.config do produkčního prostředí a přejmenujte ho na databaseConnectionStrings.config. Pokud v budoucnu, dojde ke změnám na produkční databáze připojovací řetězec je potřeba provést databaseConnectionStrings.production.config souboru a pak tento soubor nahrajte do produkčního prostředí, přejmenování databaseConnectionStrings.config.

> [!NOTE]
> Můžete zadat informace pro všechny `Web.config` element na samostatný soubor a použít `configSource` atribut používat jako reference v rámci `Web.config`.


## <a name="summary"></a>Souhrn

Aplikace řízené daty obvykle pomocí různých databází ve vývojovém a produkčním prostředí. V důsledku toho databázové připojovací řetězce, která je uložena v konfiguraci webové aplikace s musí být jedinečný pro každé prostředí. V tomto kurzu jsme se podívali na tom, jak zjistit připojovací řetězec databáze produkčního prostředí a způsoby, jak udržovat jedinečný připojovacího řetězce v dvě prostředí.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Připojovací řetězce a konfigurační soubory](https://msdn.microsoft.com/library/ms254494.aspx)
- [Konfigurace databáze řetězce informace @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Přesunutí nastavení ze souboru Web.config](http://www.asp101.com/tips/index.asp?id=154)
- [Technická dokumentace ke službě &lt;connectionStrings&gt; – Element](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Předchozí](deploying-a-database-vb.md)
> [další](configuring-a-website-that-uses-application-services-vb.md)
