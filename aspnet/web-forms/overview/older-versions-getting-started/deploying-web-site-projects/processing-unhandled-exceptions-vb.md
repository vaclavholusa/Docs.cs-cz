---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: "Zpracování neošetřených výjimek (VB) | Microsoft Docs"
author: rick-anderson
description: "Když dojde k chybě za běhu na webovou aplikaci v produkčním prostředí je důležité upozornit vývojář a do protokolu chyba, takže může být zjištěna v la..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: c1653cb3a8c684fd3d4fc2a039a947cfb5d42400
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="processing-unhandled-exceptions-vb"></a>Zpracování neošetřených výjimek (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_12_VB.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial12_ErrorHandling_vb.pdf)

> Když dojde k chybě za běhu na webovou aplikaci v produkčním prostředí je důležité, upozornit vývojáře a do protokolu chyba, takže může být zjištěna později v čase. V tomto kurzu poskytuje přehled o tom, jak ASP.NET zpracovává chyby za běhu a vypadá na jeden způsob, jak mají vlastní kód provést vždy, když neošetřené výjimce bubliny až modulem runtime ASP.NET.


## <a name="introduction"></a>Úvod

Když dojde k neošetřené výjimce v aplikaci ASP.NET, bubliny až modul runtime ASP.NET, který vyvolá `Error` událostí a zobrazí se příslušná chybová stránka. Existují tři různé typy chybové stránky: modulu Runtime chyba žlutý obrazovky z smrti (YSOD); Podrobnosti o výjimce YSOD; a vlastní chybové stránky. V [předchozí kurzu](displaying-a-custom-error-page-vb.md) jsme nakonfigurovali aplikaci, aby používala vlastní chybovou stránku pro vzdálené uživatele a YSOD podrobnosti výjimky pro uživatele, kteří navštěvují místně.

Použití lidských friendly vlastní chybovou stránku, který odpovídá vzhledu a chování webu je upřednostňovaný výchozí YSOD chyby za běhu, ale zobrazení vlastní chybovou stránku je jenom jednou ze součástí komplexní chyby zpracování řešení. Když dojde k chybě v aplikaci v produkčním prostředí, je důležité, aby vývojáři jsou upozorněni na chybu tak, aby můžou unearth příčinu chyby a vyřešit ji. Kromě toho mají být protokolovány podrobnosti k chybě, chyba mohli zkontrolován a diagnostice později v čase.

Tento kurz ukazuje, jak získat přístup k podrobnosti k neošetřené výjimce tak, aby lze protokolovat a vývojář oznámení. Dva kurzy následující tato prozkoumat chyby protokolování knihovny, které po bit konfigurace, budou automaticky oznámit vývojáři chyby za běhu a jejich podrobnosti protokolu.

> [!NOTE]
> Informace v tomto kurzu je velmi užitečné, pokud potřebujete ke zpracování neošetřených výjimek nějakým způsobem jedinečné nebo vlastní. V případech, kdy potřebujete jenom zaprotokolování výjimky a upozorňovat vývojáře pomocí knihovny k chybě protokolování je tento způsob si. Následující dva kurzy poskytovat přehled o dvě takové knihovny.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Při provádění kódu`Error`událost se vyvolá,

Události poskytují objekt mechanismus pro signalizace něčeho zajímavého došlo a jiným objektem pro spouštění kódu v odpovědi. Jako vývojář ASP.NET jste zvyklí chápat z hlediska události. Pokud budete chtít spouštět nějaký kód při návštěvníka klepne na příslušné tlačítko, můžete vytvořit obslužnou rutinu události pro toto tlačítko `Click` událostí a umístí kódu existuje. Vzhledem k tomu, že modul runtime ASP.NET vyvolává jeho [ `Error` událostí](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.error.aspx) vždy, když dojde k neošetřené výjimce vyplývá, že by se dostala kód pro protokolování podrobnosti k chybě v obslužné rutině události. Jak můžete vytvořit obslužnou rutinu události pro, ale `Error` události?

`Error` Událostí je jednou z mnoha události v [ `HttpApplication` – třída](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx) v některých fázích v kanálu HTTP, jsou vyvolány po dobu platnosti požadavku. Například `HttpApplication` třídy [ `BeginRequest` událostí](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.beginrequest.aspx) se vyvolá na začátku každého požadavku; jeho [ `AuthenticateRequest` událostí](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx) se vyvolá, když modul zabezpečení nalezen se žadatel. Tyto `HttpApplication` události poskytnout vývojář stránky znamená provést vlastní logiky v různých okamžicích v době životnosti žádost.

Obslužné rutiny události pro `HttpApplication` události mohou být umístěny v speciální soubor s názvem `Global.asax`. K vytvoření tohoto souboru ve vašem webu, přidejte novou položku do kořenového adresáře webu pomocí šablona globální třídy aplikace s názvem `Global.asax`.

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**Obrázek 1**: Přidejte `Global.asax` k vaší webové aplikaci  
 ([Kliknutím zobrazit obrázek v plné velikosti](processing-unhandled-exceptions-vb/_static/image3.png))

Obsahu a struktuře `Global.asax` soubor vytvořený pomocí sady Visual Studio liší mírně podle toho, jestli používáte projektu webové aplikace (WAP) nebo webové stránky projektu (WSP). S WAP `Global.asax` je implementovaný jako dva samostatné soubory - `Global.asax` a `Global.asax.vb`. `Global.asax` Soubor obsahuje nic, ale `@Application` direktiva, která odkazuje `.vb` souboru; událost obslužné rutiny, které vás zajímají jsou definovány v `Global.asax.vb` souboru. Pro WSPs, je vytvořen pouze jeden soubor, `Global.asax`, a obslužné rutiny událostí jsou definovány v `<script runat="server">` bloku.

`Global.asax` Soubor vytvořených za WAP šablona globální třídy aplikace Visual Studio obsahuje obslužné rutiny událostí s názvem `Application_BeginRequest`, `Application_AuthenticateRequest`, a `Application_Error`, které jsou obslužné rutiny události pro `HttpApplication` události `BeginRequest`, `AuthenticateRequest`, a `Error`, v uvedeném pořadí. Existují také obslužné rutiny událostí s názvem `Application_Start`, `Session_Start`, `Application_End`, a `Session_End`, které jsou obslužné rutiny událostí, které budou spuštěny při webové aplikace spustí, když spustí novou relaci, při ukončení aplikace, a při ukončení relace v uvedeném pořadí. `Global.asax` Soubor vytvořený v WSP Visual Studio obsahuje jenom `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, a `Session_End` obslužné rutiny událostí.

> [!NOTE]
> Při nasazování aplikace ASP.NET, je nutné zkopírovat `Global.asax` souboru do produkčního prostředí. `Global.asax.vb` Soubor, který je vytvořen v WAP, není potřeba kopírovat do produkčního prostředí, protože tento kód se zkompiluje do sestavení projektu.


Obslužné rutiny událostí vytvořené šablona globální třídy aplikace Visual Studio nejsou vyčerpávající. Můžete přidat obslužné rutiny události pro libovolný `HttpApplication` událostí pojmenováním obslužné rutiny události `Application_EventName`. Můžete například přidat následující kód, který `Global.asax` soubor k vytvoření obslužné rutiny události pro [ `AuthorizeRequest` událostí](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

Podobně můžete odebrat všechny obslužné rutiny vytvořených šablonou globální třídy aplikace, které nejsou potřebné. V tomto kurzu jsme vyžadují jenom obslužné rutiny události pro `Error` událostí; chování volné k odebrání dalších obslužné rutiny událostí z `Global.asax` souboru.

> [!NOTE]
> *Vytváření modulů HTTP v* nabízejí jiný způsob, jak definovat obslužné rutiny události pro `HttpApplication` události. Vytváření modulů HTTP jsou vytvořené jako soubor třídy, který můžete umístit přímo v projektu webové aplikace nebo oddělené do samostatné třídy knihovny. Protože je možné oddělit do knihovny tříd, vytváření modulů HTTP v nabízejí větší flexibilitu a opakovaně použitelné model pro vytváření `HttpApplication` obslužné rutiny událostí. Zatímco `Global.asax` soubor je konkrétní k webové aplikaci, kde se nachází, mohou být zkompilovány modulů HTTP do sestavení, které okamžiku přidání modulu protokolu HTTP k webu je jednoduché, vyřazením sestavení `Bin` složky a registraci V modulu `Web.config`. V tomto kurzu není podívejte se na vytváření a používání modulů HTTP, ale knihovny dvě protokolování chyb použít v následujících kurzech dva jsou implementované jako vytváření modulů HTTP. Další informace o výhodách vytváření modulů HTTP v najdete v části [pomocí modulů HTTP a obslužné rutiny pro vytvoření modulární komponenty ASP.NET](https://msdn.microsoft.com/en-us/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Načítání informací o neošetřené výjimky

V tomto okamžiku máme soubor Global.asax se `Application_Error` obslužné rutiny události. Když provede této obslužné rutiny události je potřeba upozornit vývojář chyby a protokolu její podrobnosti. K těmto úkolům musíme nejprve zjistit podrobnosti o výjimce, která byla vygenerována. Použít objekt serveru [ `GetLastError` metoda](https://msdn.microsoft.com/en-us/library/system.web.httpserverutility.getlasterror.aspx) se načíst podrobnosti o neošetřené výjimky, která způsobila, že `Error` na aktivaci události.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

`GetLastError` Metoda vrátí objekt typu `Exception`, což je základní typ pro všechny výjimky v rozhraní .NET Framework. Nicméně ve výše uvedeném kódu I mě přetypování vrácený objekt výjimky `GetLastError` do `HttpException` objektu. Pokud `Error` se vyvolání události, protože došlo k výjimce během zpracování prostředek ASP.NET pak zalomen výjimka, ke které došlo `HttpException`. Chcete-li získat skutečný výjimku, která vysráží použití událost chyby `InnerException` vlastnost. Pokud `Error` událost se vyvolá kvůli výjimce založené na protokolu HTTP, třeba požadavek na stránce neexistující `HttpException` je vyvolána, ale nemá vnitřní výjimku.

Následující kód používá `GetLastErrormessage` k načtení informací o výjimku, která aktivuje `Error` událostí, ukládání `HttpException` do proměnné s názvem `lastErrorWrapper`. Poté uloží typ, zpráv a trasování zásobníku výjimky původní do tří proměnných řetězce zjišťujeme, pokud `lastErrorWrapper` je skutečný výjimku, která aktivuje `Error` událostí (v případě výjimky založené na protokolu HTTP), nebo pokud se jenom Obálka pro výjimku, která byla vydána při zpracování požadavku.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

V tuto chvíli máte všechny informace, které budete muset psát kód, který bude protokolovat podrobnosti výjimky do databázové tabulky. Můžete vytvořit tabulku databáze s sloupce pro každou podrobnosti o chybě zájmu – typ, zprávu, trasování zásobníku a tak dále – spolu s další užitečné požadované informace, například adresu URL k požadované stránce a jméno aktuálně přihlášeného uživatele. V `Application_Error` obslužné rutiny události by pak připojení k databázi a vložení záznamu do tabulky. Podobně můžete přidat kód pro výstrahy vývojář chyby prostřednictvím e-mailu.

V knihovnách protokolování chyb v následujících dvou kurzy poskytují takové funkce předinstalované, proto není nutné vytvářet tento protokolování chyb a oznámení sami. Však k objasnění, která `Error` událost vyvolána a že `Application_Error` obslužné rutiny události lze protokolu podrobnosti o chybě a upozornění vývojář, Pojďme přidat kód, který upozorní vývojář, když dojde k chybě.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Vývojář upozornění, když dojde k neošetřené výjimce

Když dojde k neošetřené výjimce v provozním prostředí, je důležité výstrahy vývojový tým, aby můžou posoudit chyba a určit, jaké akce je třeba přijmout. Například pokud dojde k chybě při připojování k databázi, pak budete muset dvojité Zkontrolujte připojovací řetězec a, možná, otevřete lístek podpory se vaše webového hostingu společnosti. Pokud k výjimce došlo kvůli chybě programování, pravděpodobně nutné další kód nebo logiku ověření pro přidání do vyhnout tak v budoucnu.

Třídy rozhraní .NET Framework v [ `System.Net.Mail` obor názvů](https://msdn.microsoft.com/en-us/library/system.net.mail.aspx) můžete snadno odeslat e-mail. [ `MailMessage` Třída](https://msdn.microsoft.com/en-us/library/system.net.mail.mailmessage.aspx) představuje e-mailovou zprávu a má vlastnosti, například `To`, `From`, `Subject`, `Body`, a `Attachments`. `SmtpClass` Se používá k odeslání `MailMessage` objektu pomocí zadaného serveru SMTP; lze zadat prostřednictvím kódu programu nebo deklarativně v nastavení serveru SMTP [ `<system.net>` element](https://msdn.microsoft.com/en-us/library/6484zdc1.aspx) v `Web.config file`. Další informace o odesílání e-mailové zprávy v aplikaci ASP.NET najdete na Moje článku [odesílání e-mailu v ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)a [nejčastější dotazy týkající se System.Net.Mail](http://systemnetmail.com/).

> [!NOTE]
> `<system.net>` Element obsahuje nastavení serveru SMTP, který se používá `SmtpClient` třídy při odesílání e-mailu. Vaše webového hostingu společnosti pravděpodobně má server SMTP, který můžete použít k odesílání e-mailu z vaší aplikace. Informace o nastavení serveru SMTP, který byste měli použít ve webové aplikaci naleznete v části Podpora webového hostitele.


Přidejte následující kód, který `Application_Error` obslužné rutiny události pro odeslání vývojář e-mailu, když dojde k chybě:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

Při výše uvedeném kódu je velmi náročná, vytvoří hromadné ho HTML, který se zobrazí v e-mailu posílá vývojář. Spustí kód odkazem `HttpException` vrácený `GetLastError` – metoda (`lastErrorWrapper`). Skutečné výjimku, která byla vyvolána žádosti o se načítají prostřednictvím `lastErrorWrapper.InnerException` a přiřazený k proměnné `lastError`. Typ, zprávu a zásobník trasování informace získává z `lastError` a uložené v proměnné tři řetězce.

Další, `MailMessage` objekt s názvem `mm` je vytvořena. Text e-mailu je ve formátu HTML a zobrazí adresu URL k požadované stránce, názvu aktuálně přihlášeného uživatele a informace o výjimce (typ, zpráv a trasování zásobníku). Jedním z nástrojů věcí o `HttpException` třída je, že můžete vygenerovat HTML použít k vytvoření výjimka podrobnosti žlutý obrazovky z smrti (YSOD) voláním [GetHtmlErrorMessage metoda](https://msdn.microsoft.com/en-us/library/system.web.httpexception.gethtmlerrormessage.aspx). Tato metoda zde slouží k načtení kód YSOD podrobnosti výjimky a přidejte ji do e-mailu jako přílohu. Jedno slovo upozornění: Pokud výjimka, která aktivuje `Error` událostí došlo k výjimce založené na protokolu HTTP (třeba požadavek na stránce neexistující) potom `GetHtmlErrorMessage` metoda vrátí `null`.

Posledním krokem je odeslat `MailMessage`. To se provádí tak, že vytvoříte novou `SmtpClient` metoda a volání jeho `Send` metoda.

> [!NOTE]
> Před použitím tento kód ve vaší webové aplikaci budete chtít změnit hodnoty `ToAddress` a `FromAddress` konstanty z support@example.com libovolnou e-mailovou adresu by měly být odeslány na e-mailové oznámení chyby a pocházejí z. Také budete muset zadat nastavení serveru SMTP v `<system.net>` kapitoly `Web.config`. Poraďte se se svého poskytovatele hostitele webové nastavení serveru SMTP používat.


Tento kód na místě kdykoliv dojde k chybě vývojáře je odeslán shrnuje chybu, která obsahuje YSOD e-mailu. V předchozím kurzu jsme ukázán běhová chyba návštěvou Genre.aspx a předávání v neplatný `ID` hodnotu pomocí řetězce dotazu, jako je třeba `Genre.aspx?ID=foo`. Na stránce s `Global.asax` soubor na místě vytváří stejné prostředí pro uživatele, jako v předchozím kurzu - ve vývojovém prostředí budete se bude opakovat výjimka podrobnosti žlutý obrazovky z smrti, zatímco v produkčním prostředí budete najdete v části vlastní chybovou stránku. Kromě tohoto chování existující vývojář se odesílají e-mailu.

**Obrázek 2** zobrazuje e-mailu přijal při návštěvě `Genre.aspx?ID=foo`. Text e-mailu shrnuje informace o výjimce, při `YSOD.htm` přílohy zobrazuje obsah, který se zobrazí v YSOD podrobnosti výjimky (najdete v části **obrázek 3**).

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**Obrázek 2**: vývojář je odeslána e-mailové oznámení vždy, když dojde k neošetřené výjimce  
 ([Kliknutím zobrazit obrázek v plné velikosti](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**Obrázek 3**: obsahuje podrobnosti o výjimce YSOD jako přílohu e-mailové oznámení  
 ([Kliknutím zobrazit obrázek v plné velikosti](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Co se chystáte pomocí vlastní chybovou stránku?

Tento kurz vám ukázal, jak používat `Global.asax` a `Application_Error` obslužné rutiny události ke spouštění kódu, když dojde k neošetřené výjimce. Konkrétně jsme použili této obslužné rutiny události pro upozornění vývojář chybu; jsme může ji rozšířit, aby také podrobnosti o chybě protokolu v databázi. Přítomnost `Application_Error` obslužné rutiny události nemá vliv na prostředí koncového uživatele. Stále uvidí nakonfigurované chybové stránky, je-li jej YSOD podrobnosti o chybě, YSOD chyba Runtime nebo vlastní chybovou stránku.

Je přirozené zajímat, jestli `Global.asax` souboru a `Application_Error` událostí je nezbytné, pokud používáte vlastní chybovou stránku. Když dojde k chybě uživatele se zobrazí vlastní chybovou stránku, proč nelze jsme blokovat kód oznámit vývojáři a zaznamenat do protokolu podrobnosti o chybě do třídy modelu code-behind vlastní chybové stránky? Při kódu určitě můžete přidat vlastní chybovou stránku kódu třídy nemáte přístup k podrobnosti o výjimku, která aktivuje `Error` událost v případě, že pomocí techniky jsme prozkoumali v předchozím kurzu. Volání `GetLastError` vrátí metoda z vlastní chybovou stránku `Nothing`.

Z důvodu pro toto chování je, protože se vlastní chybová stránka je dosaženo pomocí přesměrování. Když k neošetřené výjimce dosáhne modulem runtime ASP.NET modul ASP.NET vyvolá jeho `Error` událostí (které provádí `Application_Error` obslužné rutiny události) a potom *přesměruje* uživateli vlastní chybovou stránku vydáním `Response.Redirect(customErrorPageUrl)`. `Response.Redirect` Metoda odešle odpověď klientovi s kódem stavu HTTP 302, instruující prohlížeče požádat o novou adresu URL, a to vlastní chybovou stránku. V prohlížeči pak automaticky požadavků tato nová stránka. Můžete zjistit, že vlastní chybovou stránku požadoval samostatně ze stránky kde chyba vytvořena, protože panelu Adresa v prohlížeči se změní na adresu URL vlastní chybové stránky (viz **obrázek 4**).

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**Obrázek 4**: když dojde k chybě v prohlížeči se přesměruje na adresu URL vlastní chybové stránky  
 ([Kliknutím zobrazit obrázek v plné velikosti](processing-unhandled-exceptions-vb/_static/image12.png))

Net efekt je, že žádosti, kde došlo k neošetřené výjimce skončí, když server odpoví přesměrování HTTP 302. Další požadavek na vlastní chybovou stránku je zbrusu novou žádost o; Pomocí tohoto bodu ASP.NET modul vyřadil informace o chybě a kromě toho se žádný způsob, jak přidružit novou žádost o pro vlastní chybovou stránku neošetřené výjimce v předchozí požadavek. To je důvod, proč `GetLastError` vrátí `null` při volání z vlastní chybovou stránku.

Je však možné, že vlastní chybovou stránku provést při stejné žádosti, která způsobila chybu. [ `Server.Transfer(url)` ](https://msdn.microsoft.com/en-us/library/system.web.httpserverutility.transfer.aspx) Metoda přenáší provádění na zadanou adresu URL a procesy ve stejném požadavku. Kód může přesunout v `Application_Error` obslužné rutiny události pro třídu modelu code-behind vlastní chybovou stránku, nahraďte ho v `Global.asax` následujícím kódem:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

Teď, když dojde k neošetřené výjimce `Application_Error` obslužné rutiny události přenosy ovládacího prvku na odpovídající vlastní chybovou stránku podle stavový kód HTTP. Vzhledem k tomu, že bylo přeneseno řízení, vlastní chybovou stránku má přístup k informacím o neošetřené výjimky prostřednictvím `Server.GetLastError` a může vývojář chyby, upozornění a protokolu její podrobnosti. `Server.Transfer` Volání zastaví modul ASP.NET z přesměrovat uživatele na vlastní chybovou stránku. Místo toho se vlastní chybová stránka obsahu se vrátí jako odpověď na stránku, který vytvořil chybu.

## <a name="summary"></a>Souhrn

Když dojde k neošetřené výjimce ve webové aplikaci ASP.NET modulem runtime ASP.NET vyvolá `Error` událostí a zobrazí se nakonfigurované chybová stránka. Upozorníme na vývojáře v chybě, podrobnosti protokolu nebo zpracovat nějak jiné vytvořením obslužné rutiny události pro události chyby. Existují dva způsoby vytvoření obslužné rutiny události pro `HttpApplication` události, jako `Error`: v `Global.asax` souboru nebo z modulu HTTP. Tento kurz vám ukázal, jak vytvořit `Error` obslužné rutiny událostí v `Global.asax` soubor, který prostřednictvím e-mailové zprávy upozorní vývojáři k chybě.

Vytvoření `Error` je užitečné, pokud potřebujete ke zpracování neošetřených výjimek nějakým způsobem jedinečné nebo vlastní obslužnou rutinu události. Ale vytvoření vlastního `Error` obslužné rutiny události k zaprotokolování výjimky nebo oznámit vývojář není využívat s maximální efektivitou času jako již existují knihovny protokolování volné a snadno použitelný chyby, které může být instalační program v řádu minut. Následující dva kurzy zkontrolujte dvě takové knihovny.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Vytváření modulů HTTP v ASP.NET a Přehled obslužných rutin HTTP](https://support.microsoft.com/kb/307985)
- [Řádně neodpovídá na požadavky neošetřených výjimek - zpracování neošetřených výjimek](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication`Třída a objekt aplikace ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Obslužné rutiny HTTP a vytváření modulů HTTP v technologii ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Odesílání e-mailu v technologii ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Pochopení `Global.asax` souboru](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [K vytvoření komponentů modulární ASP.NET pomocí modulů HTTP a obslužné rutiny](https://msdn.microsoft.com/en-us/library/aa479332.aspx)
- [Práce s ASP.NET `Global.asax` souboru](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Práce s `HttpApplication` instancí](https://msdn.microsoft.com/en-us/library/a0xez8f2.aspx)

>[!div class="step-by-step"]
[Předchozí](displaying-a-custom-error-page-vb.md)
[další](logging-error-details-with-asp-net-health-monitoring-vb.md)
