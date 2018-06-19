---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: Základní rozdíly mezi službou IIS a vývojový Server ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Při testování aplikací ASP.NET místně, pravděpodobné, že používáte webový vývojový Server ASP.NET. Produkční webu je však pravděpodobně pow...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 47b1959f9b92d161da0476b274c8154333ad80dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887545"
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>Základní rozdíly mezi službou IIS a vývojový Server ASP.NET (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> Při testování aplikací ASP.NET místně, pravděpodobné, že používáte webový vývojový Server ASP.NET. Produkční webu je však pravděpodobně napájený služby IIS. Existují určité rozdíly mezi jak tyto webové servery zpracovávat požadavky a tyto rozdíly může mít důležité důsledky. V tomto kurzu jsou zde popsány některé více podstatný rozdíly.


## <a name="introduction"></a>Úvod

Vždy, když uživatel navštíví aplikace ASP.NET jeho prohlížeč odešle požadavek na web. Tento požadavek je převzata pomocí softwaru webového serveru, který koordinuje s modulem runtime ASP.NET pro vygenerování a vrátí obsah pro požadovaný prostředek. [**I** nternetu **I** nformation **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) je sada služeb, které zajišťují běžné internetový funkce pro Servery Windows. Služba IIS je nejčastěji používanou webový server pro aplikace ASP.NET v produkčním prostředí; je pravděpodobně používán zprostředkovateli webového hostitele vaši aplikaci ASP.NET software webového serveru. Služby IIS lze také použít jako software webového serveru ve vývojovém prostředí, i když to zahrnuje, instalací služby IIS a nakonfigurovat jej správně.


Vývojový Server ASP.NET je možnost alternativní webového serveru pro vývojové prostředí; se dodává s a je integrována do sady Visual Studio. Pokud webová aplikace je nakonfigurovaná pro použití služby IIS, vývojový Server ASP.NET je automaticky spustit a používat jako webový server při první návštěvě webové stránky v sadě Visual Studio. Jsme vytvořili zpět v ukázkové webové aplikace [ *určení co soubory musí být nasazeny* ](determining-what-files-need-to-be-deployed-vb.md) kurzu byly oba souboru na základě systému webové aplikace, které nebyly nakonfigurovány na použití služby IIS. Proto při návštěvě některý z těchto webů z v sadě Visual Studio se používá vývojový Server ASP.NET.

V ideálním by identické vývoj a produkční prostředí. Ale jsme popsané v předchozí kurzu není pro prostředí do mají odlišné nastavení konfigurace. Pomocí softwaru jiný webový server v prostředích přidá jiné proměnné, které musí vzít v úvahu při nasazení aplikace. Tento kurz popisuje hlavní rozdíly mezi službou IIS a vývojový Server ASP.NET. Z důvodu tyto rozdíly existují scénáře, kde kód, který běží bezchybně splnit ve vývojovém prostředí vyvolá výjimku, nebo se chová odlišně při spuštění v produkčním prostředí.

## <a name="security-context-differences"></a>Rozdíly kontext zabezpečení

Vždy, když webový server software zpracovává příchozí žádosti přidruží této žádosti kontextu konkrétní zabezpečení. Tyto informace kontextu zabezpečení se používá v operačním systému určit, jaké akce jsou přípustné požadavkem. Například stránka technologie ASP.NET může obsahovat kód, který zaznamenává některé zprávy do souboru na disku. V pořadí pro tuto stránku ASP.NET provést bez chyby musí mít odpovídající oprávnění na úrovni systému souborů, a to oprávnění u tohoto souboru pro zápis kontext zabezpečení.

Vývojový Server ASP.NET přidruží příchozí žádosti o kontext zabezpečení aktuálně přihlášeného uživatele. Pokud jste přihlášeni k pracovní ploše jako správce, stránky ASP.NET obsloužených vývojový Server ASP.NET, budou mít stejné oprávnění jako správce. Služba IIS zpracovává požadavky ASP.NET jsou však přidružený k účtu konkrétní počítač. Ve výchozím nastavení počítač účet síťové služby se používá službou IIS verze 6 a 7, i když zprostředkovateli webového hostitele systému mohl nakonfigurovat jedinečný účet pro každého zákazníka. Navíc vám webového hostitele pravděpodobně přidělil poskytovatel omezené oprávnění k tomuto účtu počítače. Net výsledkem je, že máte webové stránky, které se provedou bez chyb ve vývojovém prostředí, ale Generovat související autorizace výjimky, pokud je hostováno v provozním prostředí.

K zobrazení tohoto typu chyby v akci, vytvořili jste na stránce na webu knihy recenze, který vytvoří soubor na disku, který ukládá poslední datum a čas, někdo zobrazit *naučit sami technologie ASP.NET 3.5 za 24 hodin* zkontrolovat. Chcete-li sledovat, otevřete `~/Tech/TYASP35.aspx` stránky a přidejte následující kód, který `Page_Load` obslužné rutiny události:

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> [ `File.WriteAllText` Metoda](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) vytvoří nový soubor, pokud neexistuje a pak zapíše zadaný obsah do něj. Pokud soubor již existuje, se přepíše existující obsah.


Potom přejděte *naučit sami technologie ASP.NET 3.5 za 24 hodin* kniha kontrolní stránku ve vývojovém prostředí pomocí vývojový Server ASP.NET. Za předpokladu, že jste se přihlásili k počítači pomocí účtu, který má odpovídající oprávnění k vytvoření a změna textového souboru na webu zobrazí kořenového adresáře aplikace recenze knihy stejná jako předtím, ale pokaždé, když je stránky navštívili, datum a čas a uživatele  IP adresa je uložen v `LastTYASP35Access.txt` souboru. Přejděte v prohlížeči do tohoto souboru; měli byste vidět zprávu podobnou té na obrázku 1.


[![Textový soubor obsahuje poslední datum a čas byl navštívené recenze knihy&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**Obrázek 1**: textový soubor obsahuje poslední datum a čas byl navštívené recenze knihy ([Kliknutím zobrazit obrázek v plné velikosti](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))


Nasazení webové aplikace do produkčního prostředí a potom navštivte hostované *naučit sami technologie ASP.NET 3.5 za 24 hodin* kniha kontrolní stránku. V tomto okamžiku by měl buď naleznete na stránce Kontrola kniha jako normální nebo chybovou zprávu na obrázku 2. Někteří poskytovatelé webového hostitele udělit oprávnění k zápisu do anonymní účet počítače ASP.NET, ve kterém bude fungovat stránce – případ typu bez chyby. Pokud ale poskytovatel webového hostitele zakáže oprávnění k zápisu pro anonymní účet pak [ `UnauthorizedAccessException` výjimka](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) se vyvolá, když `TYASP35.aspx` stránku pokusí se zapsat aktuální datum a čas `LastTYASP35Access.txt` souboru.


[![Výchozí účet počítače používaný službou IIS, nemá oprávnění k zápisu do systému souborů](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**Obrázek 2**: výchozí počítače účet využíván službou IIS nebude není mít oprávnění k zápisu do systému souborů ([Kliknutím zobrazit obrázek v plné velikosti](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))


Dobrá zpráva je, že většina webové hostitele zprostředkovatele nějaká oprávnění nástroj, který vám umožní určit oprávnění systému souborů ve vašem webu. Povolit anonymní přístup zápisu účet ASP.NET do kořenového adresáře a pak pokroku kontrolní stránce adresáře. (V případě potřeby kontaktujte poskytovatele webového hostitele pro pomoc na postup udělení oprávnění k zápisu do výchozího účtu ASP.NET.) Nyní by se měly načíst stránku bez chyby a `LastTYASP35Access.txt` soubor by měl být úspěšně vytvořen.

Proveďte rychle tady je, protože je ASP.NET Development Server pracuje v jiném kontextu zabezpečení jiný než služby IIS, je možné, že vaše stránek ASP.NET, které číst nebo zapisovat do systému souborů číst z nebo zapisovat do protokolu událostí systému Windows nebo číst nebo zapisovat do systému Windows  registru bude fungovat podle očekávání na vývoj ale generování výjimek na produkční. Při vytváření webové aplikace, které se nasadí do sdílené webové hostování prostředí, číst nebo zapisovat do protokolu událostí nebo registru systému Windows. Také si poznamenejte všech stránek ASP.NET, které číst nebo zapisovat do systému souborů, jak je budete muset poskytnout pro čtení a zápis oprávnění na příslušné složky v produkčním prostředí.

## <a name="differences-on-serving-static-content"></a>Rozdíly v poskytování statického obsahu

Další základní rozdíl mezi službou IIS a vývojový Server ASP.NET je, jak se zpracovávají požadavky na statický obsah. Každý požadavek, který jste dostali do vývojový Server ASP.NET pro stránku ASP.NET, bitovou kopii nebo soubor JavaScript zpracovává modulem runtime ASP.NET. Ve výchozím nastavení IIS pouze volá modulem runtime ASP.NET, kdy požadavek odeslán na zdroj technologie ASP.NET, jako je například webovou stránku ASP.NET, webové služby a tak dále. Požadavky na statický obsah - bitové kopie, soubory šablon stylů CSS, JavaScript soubory, soubory PDF, soubory ZIP a like - jsou načteny službou IIS bez zapojení modulem runtime ASP.NET. (Ho je možné dáte pokyn, aby služba IIS pro práci s modulem runtime ASP.NET při poskytování statického obsahu, v tomto kurzu Další informace najdete v části "Divadelní ověřování pomocí formulářů a ověřování adresy URL na statické soubory pomocí služby IIS 7".)

Modul runtime ASP.NET provede několik kroky pro vygenerování požadovaný obsah, včetně ověřování (Identifikace žadatel) a autorizaci (určení toho, jestli žadatel má oprávnění k zobrazení požadovaný obsah). Oblíbené formu ověřování je *ověřování pomocí formulářů*, ve kterém se uživatel identifikuje zadáním svých přihlašovacích údajů – obvykle uživatelské jméno a heslo - do textových polí na webové stránce. Po ověření přihlašovacích údajů, jsou uloženy na webu *lístek ověřování* souborů cookie v prohlížeči uživatele, která se posílá webu s každou další požadavek a co se používá k ověření uživatele. Kromě toho je možné zadat *autorizace adres URL* pravidla, která stanovují, kterými se uživatelé můžou nebo nelze získat přístup k určité složce. Mnoho webů ASP.NET pomocí formulářů ověřování a autorizace adres URL pro podporu uživatelské účty a definovat části lokality, které jsou přístupné pro ověřené uživatele nebo uživatelů, kteří patří do určité role.

> [!NOTE]
> Pro důkladné posouzení ASP. Ověřování pomocí formulářů, autorizace adres URL a další funkce souvisejícím s účtem uživatele na NET nezapomeňte podívejte se na Moje [kurzy zabezpečení webu](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Vezměte v úvahu web, který podporuje uživatelské účty pomocí ověřování pomocí formulářů a má složku, která pomocí autorizace adres URL, konfigurace pouze umožňuje ověřeným uživatelům. Představte si, že tato složka obsahuje stránek ASP.NET a soubory PDF a že je záměr, pouze ověření uživatelé zobrazit tyto soubory PDF.

Co se stane, pokud jeden z těchto souborů PDF zobrazíte zadáním adresy URL přímo v panelu Adresa svého prohlížeče pokusů o návštěvníka? Pokud chcete zjistit, můžeme vytvořte novou složku na webu knihy recenze, přidejte některé soubory PDF a nakonfigurujte webu, aby používal autorizace adres URL zakázat anonymní uživatelé návštěvu této složky. Pokud si stáhnout ukázkové aplikace se zobrazí, která byla vytvořena složku s názvem `PrivateDocs` a přidat PDF z mé [kurzy zabezpečení webu](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (jak těsně!). `PrivateDocs` Složka také obsahuje `Web.config` soubor, který určuje autorizačních pravidel adres URL tak, aby odepřel anonymní uživatelé:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

Nakonec I webové aplikaci, aby používala ověřování pomocí formulářů při aktualizaci souboru Web.config v kořenovém adresáři nakonfigurovat nahrazení:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

Pomocí:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

Pomocí vývojový Server ASP.NET, přejděte na web a zadejte adresu URL přímé jeden ze souborů PDF v panelu Adresa prohlížeče. Pokud jste si stáhli web přidružený tento kurz, který se adresa URL by měla vypadat podobně jako: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Zadáte tuto adresu URL do adresního řádku způsobí, že prohlížeč odeslat požadavek na vývojový Server ASP.NET pro soubor. Vývojový Server ASP.NET rukou vypnout modulem runtime ASP.NET pro zpracování požadavku. Vzhledem k tomu, že jsme ještě nepřihlásili a protože `Web.config` v `PrivateDocs` je konfigurována tak, aby odepřel anonymní přístup, modulem runtime ASP.NET automaticky přesměruje na přihlašovací stránku nám `Login.aspx` (viz obrázek 3). Když uživatel přesměrování na přihlašovací stránky, zahrnuje ASP.NET `ReturnUrl` parametr řetězce dotazu, který určuje stránce uživatel se pokoušel o zobrazení. Po úspěšném přihlášení uživatele mohou být vráceny na tuto stránku.


[![Neoprávnění uživatelé jsou automaticky přesměrováni na stránku pro přihlášení](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**Obrázek 3**: neoprávnění uživatelé jsou automaticky přesměrováni na stránku pro přihlášení ([Kliknutím zobrazit obrázek v plné velikosti](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))


Nyní si ukážeme, jak to chová na produkční. Nasaďte aplikaci a jeden PDF v zadejte přímá adresa URL `PrivateDocs` složky v produkčním prostředí. Tento parametr vyzve prohlížeč tak, aby poslat žádost o služby IIS pro soubor. Protože je požadován statický soubor, služba IIS načte a vrátí soubor bez vyvolání modulem runtime ASP.NET. V důsledku toho se žádná adresa URL autorizace kontrola provést; obsah údajně privátní PDF jsou přístupné všem uživatelům, který zná přímá adresa URL k souboru.


[![Anonymní uživatelé můžou stahovat soubory PDF privátní zadáním přímá adresa URL k souboru](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**Obrázek 4**: anonymní uživatelé můžete stáhnout privátní PDF soubory podle zadání přímá adresa URL k souboru ([Kliknutím zobrazit obrázek v plné velikosti](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Provádění ověřování pomocí formulářů a ověřování adresy URL na statické soubory pomocí služby IIS 7

Existuje několik postupů, které můžete použít k ochraně statický obsah před neoprávněnými uživateli. Službu IIS 7 zavedená *integrovaného kanálu*, který uzavře manželství pracovní postup služby IIS s modulem runtime ASP.NET pracovního postupu. Stručně řečeno můžete určit, aby služba IIS vyvolání modulem runtime ASP.NET ověřování a autorizace moduly všechny příchozí požadavky (včetně statický obsah, jako jsou soubory PDF). Obraťte se na svého poskytovatele webového hostitele a zjistěte, jak nakonfigurovat web používání integrovaného kanálu.

Jakmile služby IIS byl nakonfigurován pro použití integrovaného kanálu přidejte následující kód do `Web.config` soubor v kořenovém adresáři:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

Tento kód dá pokyn k použití ověřování a autorizace moduly založený na technologii ASP.NET IIS 7. Znovu nasaďte aplikaci a potom znovu navštivte soubor PDF. Tuto chvíli, kdy služba IIS zpracuje požadavek nabízí logiku ověřování a autorizace modulem runtime ASP.NET příležitost ke kontrole žádosti. Protože pouze ověření uživatelé mají oprávnění k zobrazení obsahu v `PrivateDocs` složky, anonymní návštěvníka se automaticky přesměruje na přihlašovací stránku (odkazuje zpět na obrázku 3).

> [!NOTE]
> Pokud váš poskytovatel hostitele webu používá aplikace služby IIS 6 nelze použít funkci integrovaného kanálu. Jeden alternativní řešení je uvést soukromé dokumenty ve složce, která zakáže přístup protokolu HTTP (například `App_Data`) a pak vytvořit stránku k obsluze tyto dokumenty. Tato stránka může být například `GetPDF.aspx`a je předána název PDF prostřednictvím parametru řetězce dotazu. `GetPDF.aspx` Stránky by nejdříve ověřte, zda uživatel má oprávnění k zobrazení souboru a pokud ano, využije [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) metodu pro odeslání zpět do klienta, který obsah požadovaný soubor PDF. Tento postup by také fungovat pro službu IIS 7, pokud není chcete povolit integrovaného kanálu.


## <a name="summary"></a>Souhrn

Webové aplikace v produkčním prostředí jsou hostované pomocí softwaru společnosti Microsoft IIS webového serveru. Ve vývojovém prostředí ale aplikace může být hostovaný pomocí služby IIS nebo vývojový Server ASP.NET. V ideálním případě stejný software webový server by měl použít v obou prostředích, protože pomocí různých softwaru přidá jiné proměnné v kombinaci. Ale snadné použití vývojový Server ASP.NET je lákavou volbou ve vývojovém prostředí. Dobrá zpráva je, že jsou pouze několik základní rozdíly mezi službou IIS a ASP.NET Development Server, a pokud jste si vědomi tyto rozdíly můžete provést kroky k zajištění, že aplikace funguje a funguje stejně způsobem bez ohledu na to prostředí.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Integrace technologie ASP.NET se službou IIS 7.0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Fóra ověřování ASP.NET pomocí všech typů obsahu ve službě IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (Video)
- [Webové servery v aplikaci Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Předchozí](common-configuration-differences-between-development-and-production-vb.md)
> [další](deploying-a-database-vb.md)
