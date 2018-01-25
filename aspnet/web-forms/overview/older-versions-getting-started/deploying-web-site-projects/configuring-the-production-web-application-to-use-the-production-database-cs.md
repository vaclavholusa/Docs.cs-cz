---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: "Konfigurace webové aplikace produkční použití provozní databázi (C#) | Microsoft Docs"
author: rick-anderson
description: "Jak je popsáno v dřívější kurzy, není pro konfigurační informace pro liší vývoj a produkční prostředí. Toto je es..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 21eac6a4d829795f02eeeca5f9870b1ab8132d08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>Konfigurace webové aplikace produkční použití provozní databázi (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> Jak je popsáno v dřívější kurzy, není pro konfigurační informace pro liší vývoj a produkční prostředí. To platí hlavně pro datové webové aplikace, jako databázové připojovací řetězce liší vývoj a produkční prostředí. Tento tento kurz popisuje způsoby konfigurace provozním prostředí, abyste zahrnují odpovídající připojovací řetězec podrobněji.


## <a name="introduction"></a>Úvod

Řízené daty webové aplikace obvykle používat jiné databázi v vývoj než kdy v produkčním prostředí. Pro aplikace hostované poskytovatelem webového hostitele a vyvinutý místně vývoj databáze obvykle umístěny v počítači s vývojáře hostitelem provozní databáze je databázový server při webového hostingu budovy společnosti s. Nasazení datové webové aplikace zahrnuje kopírování databáze vývoj do serveru provozní databáze. V předchozích kurzu jsme se podívali na způsoby, jak provést tento krok.

Webová aplikace používá informace v *připojovací řetězec* navázat spojení s databází. Připojovací řetězec, který je obvykle uložen v `Web.config`, určuje název databázového serveru, název databáze, kontext zabezpečení a další informace. Protože databáze používá webové aplikace závisí na tom, zda webová aplikace běží v vývoj nebo v provozním prostředí, se musí lišit připojovací řetězce mezi těmito dvěma prostředími.

Je běžné pro konfigurační informace pro liší vývoj a produkční prostředí. *Běžné konfigurace rozdíly mezi vývoj a produkční* kurzu popsané techniky pro údržbu na samostatné konfiguračních informací mezi těmito dvěma prostředími, jakož i stručný popis databázové připojovací řetězce. Tento tento kurz popisuje způsoby konfigurace provozním prostředí, abyste zahrnují odpovídající připojovací řetězec podrobněji.

## <a name="examining-the-connection-string-information"></a>Zkoumání informace o připojovacím řetězci

Připojovací řetězec použitý kniha recenze webová aplikace je uložené v konfiguračním souboru aplikace s `Web.config`. `Web.config`obsahuje speciální oddíl pro ukládání připojovacích řetězců, vhodně pojmenované [ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). `Web.config` Soubor pro web knihy recenze má jeden připojovacího řetězce definovaný v této části s názvem `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

Připojovací řetězec - zdroj dat =. \SQLEXPRESS; AttachDbFilename = | Zabezpečení DataDirectory|\Reviews.mdf;Integrated = True; Uživatelské Instance = True – se skládá z řady možnosti a hodnoty, s páry možnost hodnota oddělená středník a jednotlivé možnosti a hodnota oddělená symbolem rovná. Čtyři možnosti, které používá v tomto připojovacím řetězci jsou:

- `Data Source`-Určuje umístění databázového serveru a název instance databázového serveru (pokud existuje). Hodnota, `.\SQLEXPRESS`, je třeba kde je server databáze a název instance. Období určuje, že databázový server je ve stejném počítači jako aplikaci; Název instance je `SQLEXPRESS`.
- `AttachDbFilename`-Určuje umístění souboru databáze. Hodnota obsahuje zástupný symbol `|DataDirectory|`, který je přeložen na úplnou cestu aplikace s `App_Data` složky za běhu.
- `Integrated Security`-Logická hodnota, která označuje, zda pomocí zadaného uživatelského jména a hesla při připojování k databázi (false) nebo aktuální Windows přihlašovací údaje účtu (true).
- `User Instance`-možnost konfigurace specifické pro SQL Server Express Edition, která určuje, jestli se má povolit uživatelům bez oprávnění správce v místním počítači připojení a připojení k databázi systému SQL Server Express Edition. V tématu [instance služby SQL Server Express uživatele](https://msdn.microsoft.com/library/ms254504.aspx) Další informace o tomto nastavení.
  

Možnosti řetězec povolených připojení závisí na databázi, ke kterému se připojujete a databáze zprostředkovatele ADO.NET používá. Například připojovací řetězec pro připojení k serveru Microsoft SQL Server databáze se liší od kterého se připojit k databázi Oracle. Připojení k databázi Microsoft SQL Server pomocí poskytovatel Sqlclienta, používá jiný připojovací řetězec než při použití zprostředkovatele OLE DB.

Připojovací řetězec databáze můžete vytvořit ručně pomocí lokality jako [ConnectionStrings.com](http://www.connectionstrings.com/) jako prostředek pro jaké možnosti jsou dostupné. Ale snadnější přístup je pro tuto databázi přidat do Průzkumníka serveru v sadě Visual Studio a potom se získat připojovací řetězec v okně Vlastnosti. Umožní s použití této druhé techniky pro tvorbu připojovací řetězec k databázi provozním serveru.

Otevřete Visual Studio a potom přejděte do okna Průzkumníka serveru (v aplikaci Visual Web Developer, toto okno se nazývá Průzkumník databáze). Klikněte pravým tlačítkem na možnost připojení dat a vyberte možnost Přidat připojení v místní nabídce. Otevře Průvodce na obrázku 1. Vyberte příslušný zdroj dat a klikněte na tlačítko Pokračovat.


[![Vyberte, chcete-li přidat novou databázi do Průzkumníka serveru](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**Obrázek 1**: vyberte, chcete-li přidat novou databázi do Průzkumníka serveru ([Kliknutím zobrazit obrázek v plné velikosti](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))


Dále určit různé informace o připojení databáze (viz obrázek 2). Při registraci pomocí vašeho webového hostingu společnosti by měl mít poskytnou informace o tom, jak připojit k databázi – název databázového serveru, název databáze, uživatelské jméno a heslo použít k připojení k databázi a tak dále. Po zadání těchto informací klikněte na tlačítko OK k dokončení tohoto průvodce a přidejte databázi do Průzkumníka serveru.


[![Zadejte informace o připojení databáze](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**Obrázek 2**: Zadejte informace o připojení databáze ([Kliknutím zobrazit obrázek v plné velikosti](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))


Databázi produkčním prostředí by měl být teď uvedený v Průzkumníku serveru. Vyberte databázi z Průzkumníka serveru a přejít do okna vlastností. Zde najdete vlastnost s názvem připojovací řetězec databáze s připojovacím řetězcem. Za předpokladu, že používáte databáze Microsoft SQL Server na produkční a poskytovatel Sqlclienta připojovací řetězec by měl vypadat takto:

**Zdroj dat =*serverName*; Počáteční katalog =*databaseName*; Zachovat bezpečnostní údaje = True; ID uživatele =*uživatelské jméno*; Heslo = * heslo***

Kde *serverName*, *databaseName*, *uživatelské jméno*, a *heslo* jsou s hodnotami pro název databázového serveru, databáze název a uživatelské jméno a heslo, které vám poskytne vaše společnost webového hostitele.

## <a name="deploying-the-book-reviews-web-application"></a>Nasazení webové aplikace recenze adresáře

Předchozí kurzu provedl kopírování databáze vývoj do produkčního prostředí, ale prozkoumat není nasazení aplikace řízené daty. V tomto okamžiku v provozním prostředí obsahuje databázi, ale je pomocí statické recenze verze aplikace zkontroluje adresáře. Musíme nasazení nové, datové aplikace na provozním serveru společně s informacemi aktualizovanou konfiguraci.

Za chvíli nasazení aplikace řízené daty z vývojového prostředí do produkčního prostředí. Tento proces se podrobně popsané v předchozí kurzy. Pokud potřebujete aktualizačního programu, najdete v článku *nasazení webu pomocí klienta FTP* nebo *nasazení vašeho webu pomocí sady Visual Studio* kurzy. Je nutné zajistit, že připojovací řetězec provozní databáze je ten, který je používat v provozním prostředí, což znamená, že alternativní `Web.config` souboru musí být nasazen. Konkrétně, toto nastavení upravit `Web.config` soubor s `<connectionStrings>` elementu musí obsahovat připojovací řetězec databáze produkční a by měl vypadat podobně jako následující:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

Všimněte si, že připojovací řetězec v `<connectionStrings>` element se stejným názvem (`ReviewsConnectionString`), ale nyní obsahuje připojovací řetězec databáze produkční místo vývoj připojovací řetězec databáze.

Pokud máte více formalizovanou pracovní postup nasazení, ručně upravte `Web.config` soubor se má použít připojovací řetězec databáze produkční před nasazením (Nezapomeňte se vrátit zpět na použití připojovacího řetězce databáze vývoj později) nebo udržovat samostatné `Web.config` soubor s informace o konfiguraci produkční prostředí, který získá nahrán do produkčního prostředí jako součást procesu nasazení.

> [!NOTE]
> Pokud neúmyslně nasadíte `Web.config` soubor, který obsahuje připojovací řetězec databáze vývoj pak bude k chybě při aplikace na produkční pokusí připojit k databázi. Tato chyba manifesty jako `SqlException` zprávou vytváření sestav, že server nebyl nalezen nebo není přístupný.


Jakmile webu byla nasazena do produkčního prostředí, přejděte na produkční web prostřednictvím prohlížeče. Měli byste v tématu a získejte stejné prostředí pro uživatele jako při spuštění aplikace řízené daty místně. Samozřejmě při návštěvě webu v provozní lokality používá technologii serveru provozní databáze, zatímco navštěvující web ve vývojovém prostředí používá databázi ve vývoji. Obrázek 3 ukazuje *naučit sami technologie ASP.NET 3.5 za 24 hodin* zkontrolujte stránku z webu v provozním prostředí (Všimněte si adresu URL v adresním řádku prohlížeče s).


[![Data-Driven aplikace je nyní k dispozici na produkční!](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**Obrázek 3**: The Data-Driven aplikace je nyní k dispozici na produkční! ([Kliknutím zobrazit obrázek v plné velikosti](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Ukládání připojovacích řetězců v samostatných konfiguračního souboru

Běžné techniky pro uchovávání informací samostatné konfigurace na vývoj a produkční prostředí má mít dvě verze `Web.config`: jeden pro vývojové prostředí a jeden pro produkční prostředí. V době nasazení příslušné `Web.config` verze je možné zkopírovat do produkčního prostředí. V ideálním případě by se tento proces automatizované jako součást pracovního postupu nasazení.

Místo zachování dva samostatné `Web.config` soubory můžete volitelně zadejte podrobnější rozdíly. Prvky, které tvoří `Web.config` soubor lze definovat v externí konfigurační soubory, které se pak odkazuje v `Web.config` souboru. Stručně řečeno může mít jeden `Web.config` soubor pro obě prostředí, která odkazuje na databaseConnectionStrings.config soubor, který bude obsahovat připojovací řetězce, používá aplikace a bude jedinečný pro každé prostředí. Lze zjistit, že poskytuje tidier oddělení různé informace o konfiguraci do samostatných souborů a jednodušší `Web.config` souboru a další jasně popisuje konfigurace rozdíly mezi vývoj a produkční prostředí.

Chcete-li použít tuto metodu, začněte vytvořením novou složku ve webové aplikaci s názvem `ConfigSections`. Dál přidejte dva soubory do této nové složky s názvem databaseConnectionStrings.dev.config a databaseConnectionStrings.production.config. Zkopírujte `<connectionStrings>` element z `Web.config` do databaseConnectionStrings.dev.config a databaseConnectionStrings.production.config souborů a potom změňte připojovací řetězec databaseConnectionStrings.production.config souboru tak, aby Určuje připojovací řetězec provozní databáze. Například soubor databaseConnectionStrings.dev.config by měl obsahovat jenom na `<connectionStrings>` element s připojovací řetězec, který odkazuje na vývoj pro databázi:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

Podobně databaseConnectionStrings.production.config soubor by měl obsahovat jenom `<connectionStrings>` elementu, ale ten, který má připojovací řetězec provozní databáze.

Vytvořit kopii souboru databaseConnectionStrings.dev.config a pojmenujte ji databaseConnectionStrings.config.

> [!NOTE]
> Můžete pojmenovat konfiguračního souboru něco jiného než databaseConnectionStrings.config, pokud d spokojeni, jako například `connectionStrings.config` nebo `dbInfo.config`. Je však nutné název souboru s `.config` rozšíření jako `.config` soubory se ve výchozím nastavení, není obsloužených modul ASP.NET. Pokud zadáte název souboru něco jiného, třeba `connectionStrings.txt`, uživatel může bod prohlížeč tak, aby [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) a zobrazit obsah souboru!


V tomto okamžiku `ConfigSections` složka by měla obsahovat tři soubory (viz obrázek 4). Soubory databaseConnectionStrings.dev.config a databaseConnectionStrings.production.config obsahují připojovací řetězce pro vývoj a produkční prostředí, v uvedeném pořadí. DatabaseConnectionStrings.config soubor obsahuje informace o připojovacím řetězci, který se použije webová aplikace za běhu. V důsledku toho databaseConnectionStrings.config soubor by měl být stejný jako soubor databaseConnectionStrings.dev.config ve vývojovém prostředí, zatímco u produkčních databaseConnectionStrings.config soubor by měl být stejný jako databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**Obrázek 4**: ConfigSections ([Kliknutím zobrazit obrázek v plné velikosti](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))


Nyní potřebujeme dáte pokyn, aby `Web.config` chcete použít soubor databaseConnectionStrings.config pro jeho připojovací řetězec úložiště. Otevřete `Web.config` a nahradit existující `<connectionStrings>` element s následující:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

`configSource` Atribut určuje fyzickou cestu vzhledem k `Web.config` souboru. Pokud externí `.config` soubor je ve stejném adresáři jako `Web.config` pak nastavte tento atribut na název souboru `.config` souboru. Pokud se s v podadresáři, jako je tomu u databaseConnectionStrings.config, zadejte název podsložky pro vymezení názvů složek a souborů, jako je ConfigSections\databaseConnectionStrings.config pomocí zpětné lomítko.

S Tato úprava vývoj a produkční prostředí obsahovat stejné `Web.config` souboru. Jediným rozdílem je nyní soubor databaseConnectionStrings.config. Zkopírujte soubor databaseConnectionStrings.production.config do produkčního prostředí a přejmenujte jej na databaseConnectionStrings.config. Pokud v budoucnu, se změní na produkční připojovací řetězec databáze potřebujete k dosažení databaseConnectionStrings.production.config souboru a pak tento soubor nahrajte do produkčního prostředí, přejmenování ho databaseConnectionStrings.config.

> [!NOTE]
> Můžete určit informace pro všechny `Web.config` element na samostatný soubor a použít `configSource` atribut k odkazování tento soubor z uvnitř `Web.config`.


## <a name="summary"></a>Souhrn

Datové aplikace obvykle pomocí různých databází ve vývoji a produkční prostředí. Databázové připojovací řetězce, uložené v konfiguraci webové aplikace s v důsledku toho musí být jedinečný pro každé prostředí. V tomto kurzu jsme se podívali na určení připojovací řetězec databáze produkční a způsoby, jak udržovat informace o jedinečných připojovacím řetězci v dvě prostředí.

Radostí programování!

#### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Připojovací řetězce a konfigurační soubory](https://msdn.microsoft.com/library/ms254494.aspx)
- [Konfigurace databáze řetězce informace @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Přesunutí nastavení ze souboru Web.config](http://www.asp101.com/tips/index.asp?id=154)
- [Technická dokumentace k &lt;connectionStrings&gt; – Element](https://msdn.microsoft.com/library/bf7sd233.aspx)

>[!div class="step-by-step"]
[Předchozí](deploying-a-database-cs.md)
[další](configuring-a-website-that-uses-application-services-cs.md)
