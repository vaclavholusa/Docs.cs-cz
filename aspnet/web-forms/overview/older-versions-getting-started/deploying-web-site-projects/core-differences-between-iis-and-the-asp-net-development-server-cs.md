---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
title: Základní rozdíly mezi službou IIS a serveru ASP.NET Development Server (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Při testování aplikace ASP.NET místně, je pravděpodobné, že používáte webový Server ASP.NET Development. Provozním webu je však pravděpodobně pow...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 13a5a423-9235-4dde-b408-2fd10f791d63
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
msc.type: authoredcontent
ms.openlocfilehash: f4284a46fbae9ed609554b77c4e19f936b80595d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839684"
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-c"></a>Hlavní rozdíly mezi službou IIS a serveru ASP.NET Development Server (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_cs.pdf)

> Při testování aplikace ASP.NET místně, je pravděpodobné, že používáte webový Server ASP.NET Development. Provozním webu je však pravděpodobně s podporou služby IIS. Existují určité rozdíly mezi jak tyto webové servery zpracovávat požadavky a tyto rozdíly mohou mít následky důležité. Tento kurz zkoumá některé informace poskytnuty rozdíly.


## <a name="introduction"></a>Úvod

Pokaždé, když uživatel navštíví aplikaci ASP.NET jeho prohlížeč odešle požadavek na web. Tento požadavek je neexistoval software webového serveru, který koordinuje s modulem runtime ASP.NET pro vygenerování a vrátí obsah pro požadovaný prostředek. [**můžu** nternetu **můžu** nformation **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) jsou sada služeb, které poskytují běžné internetové funkce pro Windows servery. Služba IIS je nejčastěji používanou webový server pro aplikace ASP.NET v produkčním prostředí; Pravděpodobně je webový software serveru zprostředkovateli webového hostitele používá k obsluze aplikace ASP.NET. IIS také slouží jako webový server software ve vývojovém prostředí, i když to zahrnuje instalaci služby IIS a jeho konfigurace správně.


Vývojový Server ASP.NET je možnost alternativní webového serveru pro vývojové prostředí; dodává se sadou a je integrována do sady Visual Studio. Pokud webová aplikace je nakonfigurovaná pro použití služby IIS, serveru ASP.NET Development Server automaticky spustit a používat jako webový server poprvé, navštivte webovou stránku z Visual Studia. Jsme vytvořili v ukázkové webové aplikace [ *určující, co soubory musí být nasazeny* ](determining-what-files-need-to-be-deployed-cs.md) kurzu byly i souboru na základě systému webové aplikace, které nebyly nakonfigurovány na používání služby IIS. Proto při návštěvě některý z těchto webů v sadě Visual Studio se používá vývojový Server ASP.NET.

V ideálním vývojovém a produkčním prostředí by být identické. Ale jak jsme probírali v předchozí kurzu není, prostředí mít rozdílné nastavení konfigurace. Používání jiných webových serverový software v prostředích přidá jiné proměnné, které musí vzít v úvahu při zavádění aplikace. Tento kurz se zaměřuje na klíčové rozdíly mezi službou IIS a serveru ASP.NET Development Server. Kvůli těmto rozdílům se situací, které vyvolá výjimku, kód, který běží bezchybně ve vývojovém prostředí, nebo jak se bude chovat jinak při spuštění v produkčním prostředí.

## <a name="security-context-differences"></a>Rozdíly kontextu zabezpečení

Pokaždé, když je webový software server zpracovává příchozí požadavek přidruží této žádosti s kontextem konkrétní zabezpečení. Tyto informace kontextu zabezpečení se používá v operačním systému k určení akce, které jsou přípustné v požadavku. Stránky ASP.NET může například obsahovat kód, který se zaznamená do souboru na disku nějaké zprávu. Pro tuto stránku ASP.NET a spouštět bez chyb, kontext zabezpečení musí mít odpovídající oprávnění na úrovni systému souborů, konkrétně oprávnění na tento soubor k zápisu.

Vývojový Server ASP.NET přidruží příchozí žádosti o kontext zabezpečení aktuálně přihlášeného uživatele. Pokud jste přihlášeni k pracovní ploše jako správce, stránek ASP.NET, serveru ASP.NET Development Server obsluhuje, bude mít přístupová práva jako správce. ASP.NET požadavků zpracovaných službou IIS ale přidružené k účtu konkrétní počítač. Ve výchozím nastavení účet počítače síťové služby se používá službou IIS verze 6 a 7, nicméně zprostředkovateli webového hostitele může konfigurovat jedinečný účet pro každého zákazníka. Zprostředkovateli webového hostitele a co víc, má udělené pravděpodobně omezená oprávnění k tomuto účtu počítače. Net výsledkem je, že máte webové stránky, které spustit bez chyb ve vývojovém prostředí, ale generovat výjimky týkající se povolení když jsou hostované v provozním prostředí.

K zobrazení tohoto typu chyby v akci, jsem vytvořil stránku recenzí webové stránce, která vytvoří soubor na disku, který ukládá datum a čas naposledy někdo zobrazit *naučit sami technologie ASP.NET 3.5 za 24 hodin* zkontrolovat. Pokud chcete postup sledovat, otevřete `~/Tech/TYASP35.aspx` stránku a přidejte následující kód, který `Page_Load` obslužné rutiny události:

[!code-csharp[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample1.cs)]

> [!NOTE]
> [ `File.WriteAllText` Metoda](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) vytvoří nový soubor, pokud neexistuje a pak zapíše zadaný obsah do něj. Pokud soubor již existuje, přepíše existující obsah.


Dále přejděte *naučit sami technologie ASP.NET 3.5 za 24 hodin* stránku revize knihy ve vývojovém prostředí pomocí serveru ASP.NET Development Server. Za předpokladu, že jste přihlášení k počítači pomocí účtu, který má odpovídající oprávnění k vytvoření a úprava textového souboru ve webovém kořenovém adresáři aplikace recenze knihy zobrazí stejná jako předtím, ale pokaždé, když je stránka navštívené datum a čas a uživatele  IP adresa je uložen v `LastTYASP35Access.txt` souboru. Přejděte v prohlížeči do tohoto souboru. měli byste vidět zprávu podobně jako na obrázku 1.


[![Textový soubor obsahuje poslední datum a čas byla Navštívená recenze knihy](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image1.png)

**Obrázek 1**: textový soubor obsahuje poslední datum a čas byla Navštívená recenze knihy ([kliknutím ji zobrazíte obrázek v plné velikosti](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image3.png))


Nasazení webové aplikace do produkčního prostředí a potom navštivte hostovanou *naučit sami technologie ASP.NET 3.5 za 24 hodin* stránku revize knihy. V tomto okamžiku by měla buď najdete na stránce Kontrola knihy jako normální nebo chybovou zprávu zobrazenou na obrázku 2. Někteří poskytovatelé webového hostitele udělit oprávnění k zápisu do anonymní účet počítače ASP.NET, ve kterém bude případ stránce fungovat bez chyb. Pokud však zprostředkovateli webového hostitele zakazuje přístup pro zápis pro anonymní účet pak [ `UnauthorizedAccessException` výjimky](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) se vyvolá, když `TYASP35.aspx` stránku pokusí se zapsat aktuální datum a čas `LastTYASP35Access.txt` souboru.


[![Výchozí účet počítače používaný službou IIS nemá oprávnění k zápisu do systému souborů](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image4.png)

**Obrázek 2**: The výchozí počítače účet používán IIS nemá není mají oprávnění k zápisu do systému souborů ([kliknutím ji zobrazíte obrázek v plné velikosti](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image6.png))


Dobrou zprávou je, že většina poskytovatelů webového hostitele obsahuje nějaké oprávnění nástroj, který vám umožní určit oprávnění systému souborů na vašem webu. Povolit anonymní přístup zápisu účtu technologie ASP.NET do kořenového adresáře a pak otevírat stránku revize knihy. (V případě potřeby požádejte poskytovatele hostitele webových žádostí o pomoc na tom, jak udělit oprávnění k zápisu do výchozího účtu technologie ASP.NET.) Tentokrát by se měly načíst stránky bez chyb a `LastTYASP35Access.txt` úspěšně vytvoří soubor.

Zkuste okamžitě následuje, protože serveru ASP.NET Development Server pracuje v kontextu zabezpečení než služby IIS, je možné, že stránek ASP.NET, které čtení nebo zápis do systému souborů, čtení z nebo zápis do protokolu událostí Windows nebo číst nebo zapisovat Windows  registru bude fungovat podle očekávání na vývoj, ale generují výjimky v produkčním prostředí. Při vytváření webové aplikace, které se nasadí do sdílené webové hostitelské prostředí, čtení nebo zápis do protokolu událostí nebo registru Windows. Také poznamenejte si všechny stránky technologie ASP.NET, které čtení nebo zápis do systému souborů, jako je nutné udělit pro čtení a zápis oprávnění pro příslušné složky v produkčním prostředí.

## <a name="differences-on-serving-static-content"></a>Rozdíly v obsluhuje statický obsah

Další základní rozdíl mezi službou IIS a serveru ASP.NET Development Server je, jak se zpracovávají požadavky na statický obsah. Každý požadavek, který přichází do serveru ASP.NET Development Server, pro stránky ASP.NET, image nebo soubor jazyka JavaScript zpracovává modul runtime ASP.NET. Ve výchozím nastavení vyvolá IIS pouze modul runtime ASP.NET při přijetí požadavku pro prostředek technologie ASP.NET, jako je webová stránka ASP.NET, webové služby a tak dále. Požadavky na statický obsah – obrázky, soubory šablon stylů CSS, Javascriptové soubory, soubory PDF, soubory ZIP a podobně - načtením službou IIS bez zapojení modul runtime ASP.NET. (To je možné dáte pokyn, aby služba IIS pro práci s modulem runtime ASP.NET při obsluhuje statický obsah, v tomto kurzu pro další informace najdete v části "Provádění ověřování pomocí formulářů a ověřování adresy URL na statické soubory pomocí služby IIS 7")

Modul runtime ASP.NET provede několik kroků k vygenerování požadovaného obsahu, včetně (určení žadatel) ověřování a autorizace (určení, jestli žadatel nemá oprávnění k zobrazení požadovaný obsah). Oblíbené formu ověřování je *ověřování pomocí formulářů*, ve které je uživatel identifikován po zadání přihlašovacích údajů – obvykle uživatelského jména a hesla – do textových polí na webové stránce. Po ověření přihlašovacích údajů, jsou uloženy na web *ověřovací lístek* souborů cookie v prohlížeči uživatele, který se pošle při každé následné žádosti na web a které se používají k ověření uživatele. Kromě toho je možné určit *autorizace adres URL* pravidla, která určují, co uživatelé lze nebo nelze přistupovat ke konkrétní složce. Počet webů ASP.NET pomocí ověřování pomocí formulářů a autorizace adres URL pro podporu uživatelských účtů a definovat části webu, které jsou pouze dostupné pro ověřené uživatele nebo, které uživatelům patřícím do určité role.

> [!NOTE]
> Pro důkladné přezkoumání ASP. NET pro ověřování pomocí formulářů, autorizace adres URL a další funkce související s účtem uživatele nezapomeňte se podívat Můj [kurzy o zabezpečení webu](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Vezměte v úvahu web, který podporuje uživatelské účty pomocí ověřování na základě formulářů a má složku, která pomocí autorizace adres URL, konfigurace umožňuje pouze ověřeným uživatelům. Představte si, že tato složka obsahuje stránek ASP.NET a soubory PDF a že cílem je, že jenom ověření uživatelé mohou zobrazit tyto soubory PDF.

Co se stane, když návštěvník pokusí zobrazit některý z těchto souborů PDF zadáním adresy URL přímo do adresního řádku svého prohlížeče? Pokud chcete zjistit, můžeme vytvořit novou složku na webu knihy revize, přidat některé soubory PDF a konfigurace lokality tak, aby autorizace adres URL můžete zakázat anonymní uživatelé od dalších návštěv této složky. Pokud se stažení ukázkové aplikace zobrazí, že jsem vytvořil složku s názvem `PrivateDocs` a přidali PDF z mé [kurzy o zabezpečení webu](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (jak těsně!). `PrivateDocs` Také obsahuje složku `Web.config` soubor, který určuje autorizačních pravidel adres URL anonymním uživatelům odepřít:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample2.xml)]

A konečně jsem nakonfiguroval webové aplikace pro použití ověřování pomocí formulářů prostřednictvím aktualizace `Web.config` soubor v kořenovém adresáři nahrazení:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample3.xml)]

Pomocí:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample4.xml)]

Pomocí serveru ASP.NET Development Server, přejděte na web a zadejte adresu URL s přímým přístupem do jednoho souboru PDF v adresním řádku vašeho prohlížeče. Pokud jste si stáhli webu spojený s tímto kurzem, adresa URL by měla vypadat podobně jako: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Zadáte tuto adresu URL do adresního řádku způsobí, že prohlížeč odeslat požadavek na serveru ASP.NET Development Server k souboru. Požadavek na modul runtime ASP.NET ke zpracování do rukou serveru ASP.NET Development Server. Protože jsme ještě nepřihlásili a protože `Web.config` v `PrivateDocs` složky je konfigurován k odepření anonymní přístup, modul runtime ASP.NET automaticky přesměruje na přihlašovací stránku nám `Login.aspx` (viz obrázek 3). Když uživatel přesměrování na přihlašovací stránce portálu, technologie ASP.NET obsahuje `ReturnUrl` parametr řetězce dotazu, který označuje stránce uživatel se pokusil k zobrazení. Po úspěšném přihlášení uživatele mohou být vráceny na tuto stránku.


[![Neoprávnění uživatelé jsou automaticky přesměrováni na stránku pro přihlášení](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image7.png)

**Obrázek 3**: neoprávnění uživatelé jsou automaticky přesměrováni na stránku pro přihlášení ([kliknutím ji zobrazíte obrázek v plné velikosti](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image9.png))


Nyní Podíváme se, jak to chová v produkčním prostředí. Nasaďte aplikaci a zadejte adresu URL s přímým přístupem na jeden z dokumentů PDF v `PrivateDocs` složky v produkčním prostředí. Tento parametr vyzve prohlížeče k odeslání souboru žádosti o služby IIS. Protože je požadován statický soubor, služba IIS načte a vrátí soubor bez vyvolání modul runtime ASP.NET. V důsledku toho došlo provedené; žádné kontroly autorizace adresy URL obsah údajně privátní PDF jsou přístupné všem uživatelům, kteří ví přímá adresa URL k souboru.


[![Anonymní uživatelé můžou stahovat soubory PDF privátní tak, že zadáte přímá adresa URL k souboru](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image10.png)

**Obrázek 4**: anonymní uživatelé si můžou stáhnout privátní PDF souborů ve vstupu přímá adresa URL k souboru ([kliknutím ji zobrazíte obrázek v plné velikosti](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Provádí se ověřování pomocí formulářů a ověřování adresy URL na statické soubory pomocí služby IIS 7

Existuje několik postupů, které můžete použít k ochraně statický obsah před neoprávněnými uživateli. Službu IIS 7 zavedené *integrovaného kanálu*, který manželství pracovní postup služby IIS s ASP.NET runtime pracovního postupu. Řečeno v kostce můžete dát pokyn služby IIS, který má být vyvolán modul runtime ASP.NET ověřování a autorizace moduly všechny příchozí žádosti (včetně statický obsah, jako jsou soubory PDF). Obraťte se na váš web hostitele poskytovatel a zjistěte, jak nakonfigurovat svůj web pomocí integrovaného kanálu.

Jakmile službu IIS není nakonfigurovaná k použití integrovaného kanálu přidejte následující kód k `Web.config` souboru v kořenovém adresáři:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample5.xml)]

Tento kód nastaví službu IIS 7, aby používal ověřování a autorizace moduly založených na technologii ASP.NET. Znovu nasaďte aplikaci a pak znovu navštívit souboru PDF. Tentokrát, když služba IIS zpracuje požadavek poskytuje modul runtime ASP.NET logiku ověřování a autorizace příležitost zkontrolovat žádost. Protože pouze ověřeným uživatelům oprávnění k zobrazení obsahu `PrivateDocs` složky, anonymní návštěvníka automaticky přesměruje na přihlašovací stránku (vrátit zpět k obrázek 3).

> [!NOTE]
> Pokud váš poskytovatel webového hostitele se pořád používá služby IIS 6 nelze použít funkci integrovaného kanálu. Jeden alternativní řešení, je vložit soukromé dokumenty ve složce, která zakazuje přístup protokolu HTTP (například `App_Data`) a pak vytvořit stránku k poskytování těchto dokumentů. Tato stránka může volat `GetPDF.aspx`a je předán název PDF prostřednictvím parametru řetězce dotazu. `GetPDF.aspx` Stránka by nejdřív ověřte, že uživatel má oprávnění k zobrazení souboru a pokud ano, byste použili [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) metoda obsah požadovaný soubor PDF odeslat zpět klientovi. Tuto techniku by také fungovat pro službu IIS 7, pokud, aby k povolení integrovaného kanálu.


## <a name="summary"></a>Souhrn

Použití softwaru společnosti Microsoft IIS webového serveru jsou hostovaným webovým aplikacím v produkčním prostředí. Ve vývojovém prostředí ale aplikace může být hostované pomocí služby IIS nebo vývojový Server ASP.NET. V ideálním případě stejný software webového serveru by měl použít v obou prostředích, protože pomocí různých softwarových přidá jiné proměnné v kombinaci. Ale díky jednoduchosti použití serveru ASP.NET Development Server díky tomu je atraktivní výběru ve vývojovém prostředí. Dobrou zprávou je, že existují jenom pár podstatné rozdíly mezi službou IIS a serveru ASP.NET Development Server, a pokud si nejste vědomi tyto rozdíly můžete provést kroky k zajištění, že aplikace funguje a funguje stejně způsobem bez ohledu na to prostředí.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Integrace ASP.NET se službou IIS 7.0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Použití ověřování ASP.NET pomocí fóra se všemi typy obsahu ve službě IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (Video)
- [Webové servery v aplikaci Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Předchozí](common-configuration-differences-between-development-and-production-cs.md)
> [další](deploying-a-database-cs.md)
