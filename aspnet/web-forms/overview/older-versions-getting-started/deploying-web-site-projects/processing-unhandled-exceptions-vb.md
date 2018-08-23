---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: Zpracování neošetřených výjimek (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Pokud dojde k chybě za běhu na webovou aplikaci v produkčním prostředí je důležité informovat vývojáře a protokolovat chyby tak, aby může být zjištěna v la...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 29ea7f376f61c242ab93cfb71e1a7b435c575482
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752041"
---
<a name="processing-unhandled-exceptions-vb"></a>Zpracování neošetřených výjimek (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb/samples) ([stažení](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Pokud dojde k chybě za běhu na webovou aplikaci v produkčním prostředí je důležité informovat vývojáře a protokolovat chyby tak, aby může být zjištěna později v čase. Tento kurz obsahuje přehled toho, jak ASP.NET zpracovává chyby za běhu a zkoumá jeden způsob, jak mají vlastní kód spustit pokaždé, když se bubliny neošetřené výjimce až modul runtime ASP.NET.


## <a name="introduction"></a>Úvod

Když dojde k neošetřené výjimce v aplikaci ASP.NET, bubliny až modul runtime ASP.NET, která vyvolává `Error` událostí a zobrazí se příslušná chybová stránka. Existují tři různé typy chybové stránky: modulu Runtime chybě žlutý obrazovky z smrti (YSOD); Podrobnosti o výjimce YSOD; a vlastní chybové stránky. V [předchozím kurzu](displaying-a-custom-error-page-vb.md) jsme nakonfigurovali aplikaci, aby používala vlastní chybové stránky pro vzdálené uživatele a YSOD podrobnosti o výjimce pro návštěvníků místně.

Pomocí vhodných lidských vlastní chybové stránky, která odpovídá vzhledu a chování webu je upřednostňována před výchozí YSOD chyba modulu Runtime, ale zobrazení vlastní chybové stránky je pouze jednu část komplexní řešení pro zpracování chyb. Při výskytu chyby v aplikaci v produkčním prostředí, je důležité, že vývojáři se zobrazí oznámení o chybě tak, aby mohli unearth příčina výjimky a řešit. Podrobnosti o chybě kromě toho mají být protokolovány tak, aby chyby můžete prozkoumat a diagnostikovat později v čase.

Tento kurz ukazuje, jak získat přístup k podrobnosti k neošetřené výjimce, takže můžete protokolovat a vývojář oznámení. Dva kurzy tohohle po prozkoumání Chyba protokolování knihovny, které, po hodně konfigurace, budou automaticky informovat vývojáře chyby za běhu a protokolovat jejich podrobnosti.

> [!NOTE]
> Informace vyšetřovány v tomto kurzu jsou nejužitečnější tehdy, pokud budete potřebovat ke zpracování neošetřených výjimek způsobem jedinečný nebo vlastní. V případech, kdy je potřeba jenom zaprotokolování výjimky a upozornění vývojář pomocí knihovny protokolování chyb je způsob, jak přejít. Následující dva kurzy poskytovat přehled o dvou tyto knihovny.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Při provádění kódu`Error`událost je aktivována

Události poskytují objekt mechanismus pro signalizaci, že něco zajímavého došlo a pro jiný objekt ke spouštění kódu vytvořeného v odpovědi. Jako vývojář ASP.NET jsou zvyklí na přemýšlejí o události. Pokud budete chtít spouštět nějaký kód při návštěvníka klikne na určité tlačítko, vytvořit obslužnou rutinu události pro toto tlačítko `Click` událostí a ukládejte kód existuje. Vzhledem k tomu, že modul runtime ASP.NET vyvolá jeho [ `Error` události](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) pokaždé, když dojde k neošetřené výjimce vyplývá, že kód pro protokolování podrobností o chybách společnosti přejde v obslužné rutině události. Jak se vytváří obslužná rutina události, ale `Error` události?

`Error` Událostí je jednou z mnoha událostí v [ `HttpApplication` třídy](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) na určité fáze kanálu HTTP, která jsou vyvolány během životního cyklu požadavku. Například `HttpApplication` třídy [ `BeginRequest` události](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) je vyvolána na začátku každého požadavku; jeho [ `AuthenticateRequest` události](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) se vyvolá, když modul zabezpečení zjistila žadatel. Tyto `HttpApplication` události poskytují vývojář stránky znamená kvadrantu vlastní logiku v různých fázích životního cyklu požadavku.

Obslužné rutiny událostí pro `HttpApplication` události je možné použít ve zvláštním souboru s názvem `Global.asax`. Chcete-li vytvořit tento soubor na vašem webu, přidat novou položku do kořenového adresáře vašeho webu pomocí šablony globální třída aplikace s názvem `Global.asax`.

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**Obrázek 1**: Přidat `Global.asax` do vaší webové aplikace  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](processing-unhandled-exceptions-vb/_static/image3.png))

Obsah a struktura `Global.asax` soubor vytvořený pomocí sady Visual Studio se liší mírně podle toho, jestli používáte projektů webových aplikací (WAP) nebo projektu webové stránky (soubor WSP). S protokolem WAP `Global.asax` je implementovaný jako dva samostatné soubory – `Global.asax` a `Global.asax.vb`. `Global.asax` Soubor obsahuje nic ale `@Application` direktiva, která odkazuje `.vb` událost obslužné rutiny, které vás zajímají jsou definovány v; soubor `Global.asax.vb` souboru. Pro WSPs, bude vytvořena pouze jeden soubor, `Global.asax`, a obslužné rutiny událostí, které jsou definovány v `<script runat="server">` bloku.

`Global.asax` Soubor vytvořený v protokolem WAP šablona globální třídy aplikace Visual Studio obsahuje obslužné rutiny události s názvem `Application_BeginRequest`, `Application_AuthenticateRequest`, a `Application_Error`, které jsou obslužné rutiny událostí pro `HttpApplication` události `BeginRequest`, `AuthenticateRequest`, a `Error`v uvedeném pořadí. Existují také obslužné rutiny události s názvem `Application_Start`, `Session_Start`, `Application_End`, a `Session_End`, které jsou obslužné rutiny událostí, které se aktivují, když webové aplikace spustí, když spustí novou relaci, při ukončení aplikace a ukončení relace v uvedeném pořadí. `Global.asax` Soubor vytvořený v WSP pomocí sady Visual Studio obsahuje pouze `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, a `Session_End` obslužných rutin událostí.

> [!NOTE]
> Při nasazení aplikace ASP.NET je nutné zkopírovat `Global.asax` souboru do produkčního prostředí. `Global.asax.vb` Soubor, který je vytvořen v WAP, není potřeba zkopírovat do produkčního prostředí, protože tento kód je zkompilován sestavení projektu.


Obslužné rutiny událostí, vytvoří šablona globální třídy aplikace Visual Studio nejsou vyčerpávající. Můžete přidat obslužnou rutinu události pro všechny `HttpApplication` události pojmenováním obslužná rutina události `Application_EventName`. Můžete například přidat následující kód, který `Global.asax` souboru se má vytvořit obslužnou rutinu události pro [ `AuthorizeRequest` události](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

Podobně můžete odebrat všechny obslužné rutiny událostí vytvořených šablonou globální třída aplikace, které nejsou potřebné. Pro účely tohoto kurzu požadujeme pouze, aby obslužná rutina události `Error` události; teď můžete odebrat ostatní obslužné rutiny událostí z `Global.asax` souboru.

> [!NOTE]
> *Z modulů HTTP* nabízejí jiný způsob, jak definovat obslužné rutiny událostí pro `HttpApplication` události. Z modulů HTTP jsou vytvořeny jako soubor třídy, který můžete umístit přímo v rámci projektu webové aplikace nebo rozdělen do samostatné knihovně tříd. Protože se může být rozdělen do knihovny tříd, modulů HTTP nabízí flexibilní a opakovaně použitelné model pro vytváření `HttpApplication` obslužných rutin událostí. Vzhledem k tomu `Global.asax` soubor je konkrétní do webové aplikace, kde se nachází, mohou být zkompilovány z modulů HTTP do sestavení, v tomto okamžiku přidání modulu protokolu HTTP na web je snadné – stačí vyřazením sestavení `Bin` složky a registrace V modulu `Web.config`. V tomto kurzu není podívejte se na vytváření a používání modulů HTTP, ale dvě protokolování chyb knihovny použité v následujících kurzech dva jsou implementovány jako moduly protokolu HTTP. Další informace o výhodách z modulů HTTP najdete [pomocí protokolu HTTP moduly a obslužné rutiny pro vytvoření modulární komponenty technologie ASP.NET](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Načítání informací o neošetřené výjimky

V tuto chvíli máme soubor Global.asax s `Application_Error` obslužné rutiny události. Spustí tuto obslužnou rutinu události potřebujeme upozornit vývojáře chyby a protokolovat její podrobnosti. K provedení následujících úkolů, musíme nejprve určit podrobnosti o výjimce, která byla vygenerována. Použití objektu serveru [ `GetLastError` metoda](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) se načíst podrobnosti o neošetřené výjimky, která způsobila `Error` události, která se aktivuje.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

`GetLastError` Metoda vrátí objekt typu `Exception`, což je základní typ pro všechny výjimky v rozhraní .NET Framework. Nicméně ve výše uvedeném kódu I jsem přetypování objekt výjimky vrácené `GetLastError` do `HttpException` objektu. Pokud `Error` Probíhá vyvolání události, protože došlo k výjimce během zpracování prostředků technologie ASP.NET a zabalení výjimky, která byla vyvolána v rámci `HttpException`. Chcete-li získat skutečný výjimku, která se srážejí použijte událost chyby `InnerException` vlastnost. Pokud `Error` vyvolání události z důvodu výjimky založené na protokolu HTTP, třeba požadavek na neexistující stránku `HttpException` je vyvolána výjimka, ale nemá vnitřní výjimku.

Následující kód používá `GetLastErrormessage` k načtení informací o výjimce, která aktivuje `Error` události, ukládání `HttpException` do proměnné s názvem `lastErrorWrapper`. Pak ukládá typu, zprávy a trasování zásobníku původní výjimky do tří proměnných řetězce, kontroluje se, pokud `lastErrorWrapper` skutečné výjimka, která aktivuje `Error` událostí (v případě výjimky založené na protokolu HTTP) nebo pokud je pouze Obálka pro výjimku, která byla vyvolána při zpracování požadavku.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

V tomto okamžiku máte všechny informace, je nutné napsat kód, který do databázové tabulky se budou protokolovat podrobnosti této výjimky. Můžete vytvořit databázovou tabulku se sloupci pro všechny podrobnosti o chybě, které vás zajímají – typ, zprávy, trasování zásobníku a tak dále - společně s další užitečné požadované informace, například adresu URL požadované stránky a jméno aktuálně přihlášeného uživatele. V `Application_Error` obslužné rutiny události by potom připojte k databázi a vložení záznamu do tabulky. Podobně můžete přidat kód pro vývojáře o chybě e-mailem upozorní.

Knihovny protokolování chyba prozkoumat v následujících dvou kurzech poskytují takové funkce hned po spuštění, takže není nutné vytvářet tento protokolování chyb a oznámení sami. Ale pro ilustraci, který `Error` vyvolávání událostí a že `Application_Error` obslužná rutina události je možné protokolovat podrobnosti o chybě a upozornění vývojář, přidejte kód, který upozorní vývojáře, když dojde k chybě.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Vývojář upozornění, když dojde k neošetřené výjimce

Když dojde k neošetřené výjimce v produkčním prostředí je potřeba upozornit vývojový tým tak, aby můžete vyhodnotit chyby a určete, jaké akce je třeba provést. Například pokud dojde k chybě při připojování k databázi, musíte do dvojitých Zkontrolujte připojovací řetězec a možná, otevřete lístek podpory s webového hostování společnosti. Pokud k výjimce došlo z důvodu programovací chyba, další kód nebo logiku ověřování možná muset přidat k zabránění takové chyby v budoucnu.

Třídy, v rozhraní .NET Framework [ `System.Net.Mail` obor názvů](https://msdn.microsoft.com/library/system.net.mail.aspx) usnadňují odesílání e-mailu. [ `MailMessage` Třídy](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) představuje e-mailovou zprávu a má vlastnosti, jako je `To`, `From`, `Subject`, `Body`, a `Attachments`. `SmtpClass` Se používá k odesílání `MailMessage` pomocí zadaný server SMTP; nastavení serveru SMTP se dá nastavit prostřednictvím kódu programu nebo deklarativně v [ `<system.net>` element](https://msdn.microsoft.com/library/6484zdc1.aspx) v `Web.config file`. Další informace o odesílání e-mailové zprávy v aplikaci ASP.NET najdete v mé článku [odesílání e-mailu v ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)a [nejčastější dotazy týkající se System.Net.Mail](http://systemnetmail.com/).

> [!NOTE]
> `<system.net>` Element obsahuje nastavení serveru SMTP, který se používá `SmtpClient` třídy při odesílání e-mailu. Váš web pravděpodobně firma poskytující hosting má server SMTP, který můžete použít k odesílání e-mailu z vaší aplikace. Informace o nastavení serveru SMTP, kterou byste měli použít ve webové aplikaci naleznete v části Podpora webového hostitele.


Přidejte následující kód, který `Application_Error` obslužnou rutinu události pro odeslání vývojář e-mailu, když dojde k chybě:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

Ve výše uvedeném kódu je poměrně dlouhé, hromadné ho vytvoří HTML, který se zobrazí v e-mailu pro vývojáře. Kód spustí odkazováním `HttpException` vrácených `GetLastError` – metoda (`lastErrorWrapper`). Skutečné výjimky, ke které došlo v požadavku se načítají prostřednictvím `lastErrorWrapper.InnerException` a je přiřazená k proměnné `lastError`. Typ, zprávy a zásobníků informace o trasování je načten z `lastError` a uložení do proměnných tři řetězce.

Další, `MailMessage` objekt s názvem `mm` se vytvoří. E-mailu je ve formátu HTML a zobrazí adresu URL požadované stránky, název aktuálně přihlášeného uživatele a informace o výjimce (typ, zprávy a trasování zásobníku). Jednu z výhod `HttpException` třída je, že můžete vygenerovat HTML použité k vytvoření výjimky podrobnosti žlutý obrazovky z smrti (YSOD) voláním [GetHtmlErrorMessage metoda](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Tato metoda se tady používá k načtení značek YSOD podrobnosti o výjimce a přidejte ho do e-mailu jako přílohu. Varování o jedno slovo: Pokud výjimka, která aktivuje `Error` událost byla výjimka založené na protokolu HTTP (třeba požadavek na neexistující stránku) pak bude `GetHtmlErrorMessage` vrátí metoda `null`.

Posledním krokem je odeslat `MailMessage`. To se provádí tak, že vytvoříte nový `SmtpClient` metoda a volání jeho `Send` metoda.

> [!NOTE]
> Před použitím tohoto kódu ve webové aplikaci budete chtít změnit hodnoty `ToAddress` a `FromAddress` konstanty support@example.com e-mailu adresu e-mailové oznámení chyb by měly být odeslány na a pocházejí z. Také budete muset zadat nastavení serveru SMTP v `<system.net>` tématu `Web.config`. Vyhledejte poskytovatele webového hostitele k určení nastavení serveru SMTP použít.


S tímto kódem na místě kdykoliv dojde k chybě vývojáře přijde e-mailové zprávě, která shrnuje chybu a také YSOD. V předchozím kurzu jsme prokázali Chyba za běhu navštívit Genre.aspx a předáním neplatný `ID` hodnotu pomocí řetězce dotazu, jako je třeba `Genre.aspx?ID=foo`. Na stránce s `Global.asax` soubor na místě produkuje stejné prostředí pro uživatele v předchozím kurzu – ve vývojovém prostředí budete i další zobrazíte výjimky podrobnosti žlutý obrazovka smrti, zatímco v produkčním prostředí budete Zobrazit vlastní chybovou stránku. Kromě tohoto chování existující vývojář se odešle e-mailu.

**Obrázek 2** ukazuje přijetí při návštěvě e-mailu `Genre.aspx?ID=foo`. E-mailu shrnuje informace o výjimce, zatímco `YSOD.htm` přílohy zobrazí obsah, který se zobrazí v YSOD podrobnosti o výjimce (naleznete v tématu **obrázek 3**).

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**Obrázek 2**: vývojáře přijde e-mailové oznámení pokaždé, když dojde k neošetřené výjimce  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**Obrázek 3**: obsahuje podrobnosti o výjimce YSOD jako příloha e-mailové oznámení  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>A co pomocí vlastní chybovou stránku?

Tento kurz vám ukázal, jak používat `Global.asax` a `Application_Error` obslužné rutiny události k provádění kódu, když dojde k neošetřené výjimce. Konkrétně jsme použili této obslužné rutiny události upozornění pro vývojáře chyby; Společnost Microsoft může ji rozšířit, aby přihlásit také podrobnosti o chybě databáze. Přítomnost `Application_Error` obslužná rutina události nemá vliv na prostředí koncového uživatele. Stále vidí nakonfigurované chybovou stránku, už to YSOD podrobnosti o chybě, YSOD chyba modulu Runtime nebo vlastní chybovou stránku.

Je přirozené zajímat, jestli `Global.asax` souboru a `Application_Error` událostí je nezbytné, pokud používáte vlastní chybové stránky. Když došlo k chybě uživatele se zobrazí vlastní chybovou stránku tak proč nelze klademe kódu, kterého chcete upozornit vývojáře a protokolovat podrobnosti o chybě do třídy modelu code-behind vlastní chybové stránky? Zatímco určitě můžete přidat kód do třídy modelu code-behind vlastní chybovou stránku nemáte přístup k podrobnosti o výjimce, která aktivuje `Error` událost v případě technikou Prozkoumali jsme v předchozím kurzu. Volání `GetLastError` vrátí metoda z vlastní chybovou stránku `Nothing`.

Důvod pro toto chování je vzhledem k tomu, že se vlastní chybová stránka je kontaktovat prostřednictvím přesměrování. Vyvolá neošetřenou výjimku dosáhne-li modul runtime ASP.NET modul ASP.NET jeho `Error` událostí (která spustí `Application_Error` obslužná rutina události) a potom *přesměruje* uživateli vlastní chybovou stránku vydáním `Response.Redirect(customErrorPageUrl)`. `Response.Redirect` Metoda odešle odpověď klientovi stavovým kódem HTTP 302, že prohlížeč požádat o novou adresu URL, a to vlastní chybovou stránku. Prohlížeč pak automaticky požaduje tuto novou stránku. Poznáte, že vlastní chybové stránky byl požadován odděleně od stránce původu chybu, protože adresního řádku prohlížeče se změní na adresu URL vlastní chybové stránky (viz **obrázek 4**).

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**Obrázek 4**: když došlo k chybě v prohlížeči přesměrován na adresu URL vlastní chybové stránky  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](processing-unhandled-exceptions-vb/_static/image12.png))

Výsledkem je, že žádost, ve kterém došlo k neošetřené výjimce končí, když server odpoví přesměrování HTTP 302. Další požadavek na vlastní chybovou stránku je zbrusu novou žádost o; už v tomto okamžiku ASP.NET modul vyřadil informace o této chybě a navíc nemá žádný způsob, jak přidružit nový požadavek na vlastní chybovou stránku neošetřené výjimky v předchozí žádosti. To je důvod, proč `GetLastError` vrátí `null` při volání z vlastní chybovou stránku.

Je však možné mít vlastní chybovou stránku provedených během stejný požadavek, který způsobil chybu. [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) Metoda přenese vykonávání na zadanou adresu URL a z procesů v rámci stejné žádosti. Můžete přesunout kód `Application_Error` obslužné rutiny události k použití modelu code-behind třídy vlastní chybovou stránku, nahraďte ji v `Global.asax` následujícím kódem:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

Teď, když dojde k neošetřené výjimce `Application_Error` obslužná rutina události přenese ovládací prvek na odpovídající vlastní chybovou stránku, na základě kódu stavu protokolu HTTP. Vzhledem k tomu, že byl ovládací prvek převeden, vlastní chybovou stránku má přístup k informacím o neošetřené výjimky prostřednictvím `Server.GetLastError` a může informovat vývojáře o chybě a protokolovat její podrobnosti. `Server.Transfer` Volání přestane modul ASP.NET z přesměruje uživatele na vlastní chybovou stránku. Místo toho vlastní chybovou stránku obsahu je vrácen jako odpověď na stránku, který vytvořil chybu.

## <a name="summary"></a>Souhrn

Když dojde k neošetřené výjimce ve webové aplikaci ASP.NET, modul runtime ASP.NET vyvolá `Error` událostí a zobrazí se nakonfigurované chybová stránka. Pro vývojáře v chybě, můžete být informováni protokolovat podrobnosti nebo zpracovat nějak další tak, že vytvoříte obslužnou rutinu události pro událost chyby. Existují dva způsoby, jak vytvořit obslužnou rutinu události pro `HttpApplication` události, jako jsou `Error`: v `Global.asax` souboru nebo z modulu HTTP. Tento kurz vám ukázal, jak vytvořit `Error` obslužné rutině událostí ve `Global.asax` soubor, který upozorní vývojáře chybu prostřednictvím e-mailovou zprávu.

Vytvoření `Error` obslužná rutina události je užitečné, pokud budete potřebovat ke zpracování neošetřených výjimek způsobem jedinečný nebo vlastní. Však vytvářet své vlastní `Error` obslužné rutiny události k zaprotokolování výjimky nebo upozornit vývojáře není co nejefektivněji využít váš čas jako již existují knihovny pro protokolování zdarma a snadno se používá chyba může být také nastaven v řádu minut. Následující dva kurzy zkontrolujte dvě tyto knihovny.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Přehled obslužných rutin HTTP a ASP.NET z modulů HTTP](https://support.microsoft.com/kb/307985)
- [Řádně zpracování neošetřených výjimek – zpracování neošetřených výjimek](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` Třídy a objekt aplikace ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Obslužné rutiny HTTP a moduly protokolu HTTP v ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Odeslání e-mailu v ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Principy `Global.asax` souboru](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Použití modulů HTTP a obslužné rutiny k vytvoření komponentů modulární ASP.NET](https://msdn.microsoft.com/library/aa479332.aspx)
- [Práce s ASP.NET `Global.asax` souboru](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Práce s `HttpApplication` instancí](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Předchozí](displaying-a-custom-error-page-vb.md)
> [další](logging-error-details-with-asp-net-health-monitoring-vb.md)
