---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: Ověřování přihlašovacích údajů uživatele, kteří členství uživatele Store (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu prozkoumáme ověření přihlašovacích údajů uživatele, kteří úložiště uživatele členství pomocí programové prostředky a ovládací prvek Login...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: e8c46d09a7ebab19204f7c439ec4333e0c36b73e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828952"
---
<a name="validating-user-credentials-against-the-membership-user-store-c"></a>Ověřování přihlašovacích údajů uživatele, kteří členství uživatele Store (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> V tomto kurzu prozkoumáme ověření přihlašovacích údajů uživatele, kteří úložiště uživatele členství pomocí programové prostředky a ovládacího prvku pro přihlášení. Podíváme se také na tom, jak přizpůsobit vzhled a chování ovládací prvek login.


## <a name="introduction"></a>Úvod

V <a id="Tutorial05"> </a> [předchozím kurzu](creating-user-accounts-cs.md) jsme se podívali na tom, jak vytvořit nový uživatelský účet v rámci členství. Zvažovali jsme i první programové vytvoření uživatelských účtů prostřednictvím `Membership` třídy `CreateUser` metoda a potom prozkoumat pomocí ovládacího prvku CreateUserWizard Web. Na přihlašovací stránku ověří však aktuálně zadané přihlašovací údaje pevně zakódované seznam dvojic uživatelské jméno a heslo. Potřebujeme aktualizovat přihlašovací stránku logiky tak, aby ověřuje pověření proti úložišti framework členství uživatele.

Podobně jako s vytváření uživatelských účtů, přihlašovacích údajů může být ověřen prostřednictvím kódu programu nebo deklarativně. Toto rozhraní API členství zahrnuje metodu pro ověřování prostřednictvím kódu programu přihlašovacích údajů uživatele, kteří úložiště uživatele. A ASP.NET se dodává s ovládacím prvkem webového přihlášení, která vykresluje uživatelské rozhraní s textová pole pro uživatelské jméno a heslo a tlačítko pro přihlášení.

V tomto kurzu prozkoumáme ověření přihlašovacích údajů uživatele, kteří úložiště uživatele členství pomocí programové prostředky a ovládacího prvku pro přihlášení. Podíváme se také na tom, jak přizpůsobit vzhled a chování ovládací prvek login. Pusťme se do práce!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Krok 1: Ověření přihlašovacích údajů, kteří Store členství uživatele

U webů používajících ověřování pomocí formulářů uživatel přihlásí na web tak, že navštívili přihlašovací stránku a zadání přihlašovacích údajů. Tyto přihlašovací údaje se pak porovnávají úložiště uživatele. Pokud jsou platné, uživateli je udělen lístek ověřování formulářů, což je token zabezpečení, která určuje identitu a pravosti návštěvníka.

Chcete-li ověřit uživatele vůči framework členství, použijte `Membership` třídy [ `ValidateUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx). `ValidateUser` Metoda přebírá dva vstupní parametry - *`username`* a *`password`* – a vrátí hodnotu typu Boolean označující, zda byly platné přihlašovací údaje. Jak je `CreateUser` metoda v předchozím kurzu jsme se zaměřili `ValidateUser` metoda deleguje skutečné ověřování do nakonfigurovaného zprostředkovatele členství.

`SqlMembershipProvider` Ověří zadané přihlašovací údaje získat heslo pro zadaného uživatele prostřednictvím `aspnet_Membership_GetPasswordWithFormat` uložené procedury. Vzpomeňte si, že `SqlMembershipProvider` ukládá hesla uživatelů pomocí jedné ze tří formátů: Vymazat, šifrovaných nebo mají hodnotu hash. `aspnet_Membership_GetPasswordWithFormat` Uloženou proceduru vrátí heslo v jeho nezpracovaném formátu. Šifrované nebo hodnoty hash hesel `SqlMembershipProvider` transformuje *`password`* hodnoty předané `ValidateUser` metodu na jeho ekvivalent šifrované nebo mají hodnotu hash stavu a porovná ho s co vrátil databáze. Pokud heslo uložené v databázi odpovídá formátovaný heslo zadané uživatelem, jsou pověření platná.

Umožňuje aktualizovat naši stránku pro přihlášení (~ /`Login.aspx`) tak, aby ověří zadané přihlašovací údaje proti úložišti framework uživatele členství. Jsme vytvořili tento přihlašovací stránku zpátky <a id="Tutorial02"> </a> [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-cs.md) kurz vytváření rozhraní s dvě textová pole pro uživatelské jméno a heslo, Pamatovat si mě zaškrtávacího políčka a tlačítka pro přihlášení (viz obrázek 1). Kód ověří zadané přihlašovací údaje pevně zakódované seznam dvojic uživatelské jméno a heslo (Scott a hesla, Jisun a hesla a Sam a hesla). V <a id="Tutorial03"> </a> [ *konfigurace ověřování formulářů a témata pokročilé* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) kurzu jsme aktualizovali kód na přihlašovací stránku k ukládání dalších informací ve formulářích lístek ověřování `UserData` vlastnost.


[![Přihlašovací stránky rozhraní zahrnuje dvě textová pole, třídy CheckBoxList a tlačítko](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**Obrázek 1**: tlačítko, na přihlašovací stránku rozhraní zahrnuje dvě textová pole a třídy CheckBoxList ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))


Na přihlašovací stránku uživatelského rozhraní může zůstat beze změny, ale potřebujeme k nahrazení tlačítka pro přihlášení `Click` obslužné rutiny události pomocí kódu, který ověřuje uživatele vůči úložiště uživatelských rozhraní framework členství. Aktualizujte obslužnou rutinu události tak, aby jeho kód, zobrazí se takto:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

Tento kód je výjimečně jednoduché. Začneme voláním `Membership.ValidateUser` metodu zadané uživatelské jméno a heslo. Pokud tato metoda vrátí hodnotu true, pak uživatel je přihlášen k webu prostřednictvím `FormsAuthentication` RedirectFromLoginPage metodu třídy. (Jak jsme probírali v <a id="Tutorial2"> </a> [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-cs.md) kurzu `FormsAuthentication.RedirectFromLoginPage` vytváří formy ověřovací lístek a potom přesměruje uživatele na příslušnou stránku.) Pokud přihlašovací údaje jsou neplatné, ale `InvalidCredentialsMessage` zobrazí popisek informující uživatele, že jeho uživatelské jméno nebo heslo bylo nesprávné.

To je všechno je to!

Pokud chcete otestovat, na přihlašovací stránku funguje podle očekávání, pokus o přihlášení s jedním z uživatelských účtů, které jste vytvořili v předchozím kurzu. Nebo, pokud jste ještě nevytvořili účet, pokračujte a vytvořte si ho z `~/Membership/CreatingUserAccounts.aspx` stránky.

> [!NOTE]
> Když uživatel zadá své přihlašovací údaje a přihlašovací stránky formulář odešle, přihlašovací údaje, včetně jeho heslo přenášejí přes Internet na webový server v *prostý text*. To znamená, že všechny kyberzločinci pro analýzu sítě síťový provoz můžete zobrazit uživatelské jméno a heslo. Chcete-li tomu zabránit, je nezbytné k šifrování síťového provozu s využitím [vrstvy SSL (Secure Socket)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Tím se zajistí, že jsou zašifrované přihlašovací údaje (stejně jako značka jazyka HTML celé stránky) od okamžiku, kdy nechají prohlížeč, dokud nedorazí k příjemci webový server.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Jak Framework členství zpracovává neplatných pokusů o přihlášení

Návštěvník dosáhne přihlašovací stránku a odešle jejich přihlašovacích údajů, jejich prohlížeč odešle požadavek HTTP na přihlašovací stránku. Pokud přihlašovací údaje jsou platné, odpověď HTTP, která zahrnuje ověřovací lístek do souboru cookie. Proto může kyberzločinci pokus o přerušení web vytvořit program, který vyčerpávajícím způsobem odesílá požadavky HTTP na přihlašovací stránku s platné uživatelské jméno a odhad toho, heslo. Pokud je heslo odhad správný, přihlašovací stránku vrátí ověřovacího lístku souboru cookie, v tomto okamžiku program ví, že má stumbled po pár platné uživatelské jméno/heslo. Prostřednictvím útoků hrubou silou může být takový program schopen klopýtnout na heslo uživatele, zejména v případě, že je slabé heslo.

Aby se zabránilo takovým útokům hrubou silou, členství v rámci zamezí uživateli pokud existuje mnoho neúspěšných pokusů o přihlášení v časovém období. Přesné parametrů se dají konfigurovat prostřednictvím následující nastavení konfigurace dvou zprostředkovatele členství:

- `maxInvalidPasswordAttempts` -Určuje, kolik neplatné heslo pro uživatele v časovém intervalu před zablokováním účtu jsou povoleny pokusy. Výchozí hodnota je 5.
- `passwordAttemptWindow` -Určuje časové období, během několika minut, během kterých zadaný počet neplatných pokusů o přihlášení způsobí, že účet, který chcete uzamknout. Výchozí hodnota je 10.

Pokud uživatel je uzamčen, které se nemůžou přihlásit, dokud správce odemkne svůj účet. Když je uživatel uzamčen, `ValidateUser` bude metoda *vždy* vrátit `false`i v případě platné pověření. Když toto chování snižuje pravděpodobnost, že se hacker se rozdělit na svůj web prostřednictvím metod útoku hrubou silou, ho můžete skončit uzamčení platný uživatel, který se jednoduše zapomene své heslo nebo neúmyslně má Caps Lock na nebo s nevhodným datem psaní.

Bohužel není žádný integrovaný nástroj pro odemknutí uživatelského účtu. Aby bylo možné odemknout účet, můžete upravit databázi přímo – změnit `IsLockedOut` pole `aspnet_Membership` tabulky pro k odpovídajícímu uživatelskému účtu – nebo vytvořit webové rozhraní, která obsahuje seznam uzamčených účtů s možnostmi pro odemčení. Prozkoumáme vytváření rozhraní pro správu k provedení běžných úkolů souvisejícím s účtem a role uživatele v budoucích kurzech.

> [!NOTE]
> Jednou z nevýhod `ValidateUser` metody je, že když zadané přihlašovací údaje jsou neplatné, neposkytuje žádné vysvětlení důvod, proč. Přihlašovací údaje může být neplatný, protože neexistuje žádná odpovídající dvojice uživatelského jména a hesla v úložišti uživatele, nebo protože uživatel dosud nebyly schváleny, nebo protože uživatele byl uzamčen. V kroku 4 uvidíme, jak zobrazit podrobné zprávy uživateli při jejich pokus o přihlášení selže.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Krok 2: Shromažďování přihlašovacích údajů prostřednictvím webového ovládacího prvku pro přihlášení

[Přihlášení webový ovládací prvek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) vykreslí velmi podobně jako jsme vytvořili v výchozí uživatelské rozhraní <a id="SKM5"> </a> [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-cs.md) kurzu. Pomocí ovládacího prvku pro přihlášení uloží, nám práci s k vytvoření rozhraní pro zadání přihlašovacích údajů s návštěvníka. Kromě toho ovládacího prvku pro přihlášení se automaticky přihlásí uživatele (za předpokladu, že zadané přihlašovací údaje jsou platné), a tím ukládání nám od nutnosti psát jakýkoli kód.

Aktualizujme `Login.aspx`, nahrazení ručně vytvořené rozhraní a kódu s ovládacím prvkem přihlášení. Začněte tím, že odebrání existujících značek a kódu v `Login.aspx`. Můžete odstranit přímý nebo jednoduše vyzkoušet komentář. Okomentujte deklarativní, uzavřete ji `<%--` a `--%>` oddělovače. Tyto oddělovače lze zadat ručně, nebo jak je vidět na obrázku 2, můžete vybrat text poznámky, a potom klikněte na komentář na vybrané řádky ikonu na panelu nástrojů. Podobně můžete Odkomentujte ikonu vybrané řádky Zakomentovat vybrané kód ve třídě použití modelu code-behind.


[![Odkomentujte existující kód a zdrojový kód v Login.aspx](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**Obrázek 2**: komentář navýšení kapacity existující deklarativní a zdrojový kód v `Login.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))


> [!NOTE]
> Odkomentujte ikonu vybrané řádky není k dispozici, při zobrazení deklarativní v sadě Visual Studio 2005. Pokud nepoužíváte Visual Studio 2008 je potřeba ručně přidat `<%--` a `--%>` oddělovače.


V dalším kroku přetáhněte ovládací prvek Login z panelu nástrojů na stránku a nastavit jeho `ID` vlastnost `myLogin`. V tomto okamžiku vaše obrazovka by měla vypadat podobně jako na obrázku 3. Všimněte si, že ovládací prvek Login výchozí rozhraní obsahuje ovládací prvky textové pole pro uživatelské jméno a heslo, zapamatovat příště zaškrtávacího políčka a tlačítka v protokolu. Existují také `RequiredFieldValidator` ovládací prvky pro dvě textová pole.


[![Přidat na stránku ovládací prvek Login](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**Obrázek 3**: Přidání ovládacího prvku přihlášení na stránce ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))


A máme Hotovo! Po kliknutí na tlačítko Přihlásit ovládací prvek Login zpětného odeslání dojde a bude volat ovládacího prvku pro přihlášení `Membership.ValidateUser` metodu zadané uživatelské jméno a heslo. Pokud přihlašovací údaje jsou neplatné, ovládací prvek Login zobrazí zpráva, například. Pokud však přihlašovací údaje jsou platné, ovládací prvek Login vytvoří lístek ověřování formulářů a přesměruje uživatele na příslušnou stránku.

Ovládací prvek Login používá k určení na příslušnou stránku a přesměruje uživatele na po úspěšném přihlášení čtyř faktorů:

- Určuje, zda je ovládací prvek přihlášení na přihlašovací stránce podle definice `loginUrl` nastavení v konfiguraci ověřování formulářů; toto nastavení výchozí hodnota je `Login.aspx`
- Přítomnost `ReturnUrl` parametr řetězce dotazu
- Hodnota ovládací prvek Login [ `DestinationUrl` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- `defaultUrl` Hodnota zadaná ve formulářích nastavení konfigurace ověřování; výchozí hodnota tohoto nastavení je `Default.aspx`

Jak znázorňuje obrázek 4 ovládacího prvku pro přihlášení používá tyto čtyři parametry můžete přejít na jeho odpovídající stránku rozhodnutí.


[![Přidat na stránku ovládací prvek Login](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**Obrázek 4**: Přidání ovládacího prvku přihlášení na stránce ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))


Využijte k otestování ovládací prvek Login následujícím webu prostřednictvím prohlížeče a přihlaste se jako stávajícího uživatele v rámci členství.

Ovládací prvek Login vykreslené rozhraní je vysoce konfigurovatelné. Existuje mnoho vlastností, které ovlivňují její vzhled; a co víc ovládací prvek Login lze převést na šablonu pro přesnou kontrolu nad rozložením prvky uživatelského rozhraní. Zbývající část tohoto kroku zkoumá, jak upravit vzhled a rozložení.

### <a name="customizing-the-login-controls-appearance"></a>Přizpůsobení vzhledu ovládací prvek Login

Nastavení vlastností výchozí ovládací prvek Login vykreslení uživatelské rozhraní s názvem (přihlásit), ovládacích prvků TextBox a popisek pro zadání uživatelského jména a hesla, zapamatovat další čas zaškrtávacího políčka a tlačítka přihlásit. Vzhled tyto prvky jsou všechny konfigurovat přes ovládací prvek Login mnoho vlastností. Kromě toho je možné přidat další prvky uživatelského rozhraní – například odkaz na stránku, kterou chcete vytvořit nový uživatelský účet - nastavením vlastností nebo dvě.

Věnujte chvíli gussy vzhledu naše ovládací prvek Login. Vzhledem k tomu, `Login.aspx` stránka již obsahuje textu v horní části stránky, která uvádí, že přihlášení, ovládací prvek Login je nadbytečný. Proto smažte [ `TitleText` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) hodnotu, aby bylo možné odebrat ovládací prvek Login název.

Uživatelské jméno: a heslo: popisky nalevo od dvou ovládacích prvků textového pole je možné přizpůsobit pomocí [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) a [ `PasswordLabelText` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx)v uvedeném pořadí. Umožňuje změnit uživatelské jméno: popisek číst uživatelské jméno:. Styly popisek a textové pole se dají konfigurovat prostřednictvím [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) a [ `TextBoxStyle` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx)v uvedeném pořadí.

Zapamatovat uživatele další čas políčko Text vlastnost lze nastavit pomocí ovládací prvek Login [ `RememberMeText property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx), a jeho výchozí zaškrtnutí stavu se dají konfigurovat prostřednictvím [ `RememberMeSet property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (který Výchozí hodnota je False). Pokračujte a nastavit `RememberMeSet` vlastnost na hodnotu True, tak, aby zapamatovat zaškrtávací políčko vedle čas se ve výchozím nastavení zaškrtnuto.

Ovládací prvek Login nabízí dvě vlastnosti pro přizpůsobení rozložení ovládacích prvků uživatelského rozhraní. [ `TextLayout property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) Označuje, zda uživatelské jméno: a heslo: popisky se zobrazí vlevo jejich odpovídajících textových polí (výchozí) nebo nad nimi. [ `Orientation property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) Označuje, zda vstupy uživatelské jméno a heslo jsou umístěna ve svislém směru (jeden nad sebou) nebo vodorovně. Teď předvedu ponechte tyto dvě vlastnosti výchozí nastavení, ale neváhejte se zkuste nastavit tyto dvě vlastnosti na jiné než výchozí hodnoty zobrazíte výsledný efekt.

> [!NOTE]
> V další části Konfigurace rozložení ovládacích prvků přihlášení, se podíváme na použití šablony k definování přesné rozložení prvků rozložení ovládacího prvku uživatelského rozhraní.


Zabalit nastavení vlastností ovládacího prvku přihlášení tak, že nastavíte [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) a [ `CreateUserUrl` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) do Not jste se nezaregistrovali? Vytvoření účtu! a `~/Membership/CreatingUserAccounts.aspx`v uvedeném pořadí. Tento postup přidá ovládací prvek Login rozhraní přejdete na stránku jsme vytvořili v hypertextový odkaz <a id="SKM6"> </a> [předchozím kurzu](creating-user-accounts-cs.md). Ovládací prvek Login [ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) a [ `HelpPageUrl` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) a [ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) a [ `PasswordRecoveryUrl` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) pracovat stejným způsobem, vykreslování odkazů na stránku nápovědy a stránky pro obnovení hesla.

Po provedení těchto změn vlastnosti, by vypadat podobně jako, který je znázorněno na obrázku 5 deklarativní a vzhled ovládacího prvku přihlášení.


[![Ovládací prvek Login vlastnosti hodnoty určují vzhled](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**Obrázek 5**: ovládací prvek Login vlastnosti hodnoty určují jeho vzhled ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Konfigurace rozložení ovládací prvek Login

Ovládací prvek webového přihlášení výchozí uživatelské rozhraní rozložen rozhraní v HTML `<table>`. Ale co když budeme potřebovat jemnější kontrolu nad vykresleného výstupu? Možná chcete nahradit `<table>` s řadou `<div>` značky. Nebo co když naše aplikace vyžaduje další přihlašovací údaje pro ověřování? Mnoho finanční webů, například vyžadovat, aby uživatelé zadat pouze uživatelské jméno a heslo, ale také osobní identifikační číslo (PIN) nebo identifikovatelné informace. Všechno, co může být důvody, je možné převést na šablonu, ze kterého můžete explicitně definujeme deklarativní rozhraní ovládacího prvku pro přihlášení.

Budeme muset udělat dvě věci za účelem aktualizace ovládacího prvku pro přihlášení ke shromažďování dalších přihlašovacích údajů:

1. Aktualizujte ovládací prvek Login rozhraní zahrnout webových ovládacích prvcích shromažďovat další přihlašovací údaje.
2. Logika ověřování vnitřní ovládací prvek Login přepište tak, aby v případě, že platnost uživatelského jména a hesla a jejich další přihlašovací údaje jsou platné, příliš pouze ověření uživatele.

Pokud chcete provést první úlohu, potřebujeme převést šablonu ovládacího prvku pro přihlášení a přidání příslušných ovládacích prvků webových. Jako druhý úkol, může být potlačeno logika ověřování přihlášení ovládacího prvku vytvořením obslužnou rutinu události pro ovládací prvek [ `Authenticate` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx).

Umožňuje aktualizovat ovládacího prvku pro přihlášení tak, aby vyzve uživatele pro jejich uživatelské jméno, heslo a e-mailovou adresu a pouze pokud e-mailovou adresu zadali, odpovídá e-mailová adresa u souboru ověřuje uživatele. Nejdřív potřebujeme převést ovládací prvek Login rozhraní do šablony. Z inteligentních značek ovládací prvek Login zvolte Převést na šablonu.


[![Převést na šablonu ovládacího prvku pro přihlášení](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**Obrázek 6**: převést na šablonu ovládacího prvku pro přihlášení ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))


> [!NOTE]
> Pokud chcete vrátit ovládací prvek Login předem template verze, klikněte na odkaz na resetování z ovládacího prvku inteligentních značek.


Převod ovládacího prvku pro přihlášení do šablony přidá `LayoutTemplate` do ovládacího prvku deklarativní s prvky jazyka HTML a ovládacích prvků webového definování uživatelského rozhraní. Jak je vidět na obrázku 7, převod ovládacího prvku do šablony odebere počet vlastností v okně vlastnosti jako `TitleText`, `CreateUserUrl`a tak dále, protože hodnoty těchto vlastností jsou ignorovány při použití šablony.


[![Méně vlastností jsou že k dispozici při přihlášení řídicí je převést na šablonu](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**Obrázek 7**: méně vlastnosti jsou k dispozici při přihlášení řídicí je převést na šablonu ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))


Značka jazyka HTML v `LayoutTemplate` může podle potřeby upravit. Podobně můžete do šablony přidat všechny nové webové ovládací prvky. Ale je důležité tento ovládací prvek Login základní webové ovládací prvky zůstat v šabloně a zachovat jejich přiřazené `ID` hodnoty. Konkrétně se neodebere ani přejmenovat `UserName` nebo `Password` textová pole, `RememberMe` zaškrtávací políčko, `LoginButton` tlačítko, `FailureText` popisek, nebo `RequiredFieldValidator` ovládacích prvků.

Ke shromažďování návštěvníka e-mailovou adresu, potřebujeme do šablony přidat textové pole. Přidejte následující kód mezi řádky tabulky (`<tr>`), který obsahuje `Password` textového pole a řádku tabulky obsahující zapamatovat další čas zaškrtávací políčko:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

Po přidání `Email` textového pole na stránce prostřednictvím prohlížeče. Jak ukazuje obrázek 8, ovládací prvek Login uživatelského rozhraní nyní zahrnuje třetí textové pole.


[![Ovládací prvek Login teď obsahuje textové pole pro e-mailovou adresu uživatele](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**Obrázek 8**: ovládací prvek Login teď obsahuje textové pole pro e-mailovou adresu uživatele ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))


V tomto okamžiku se pořád používá ovládacího prvku pro přihlášení `Membership.ValidateUser` metodu za účelem ověření zadané přihlašovací údaje. Odpovídajícím způsobem, hodnotu zadat do `Email` textového pole nemá žádný vliv na tom, jestli uživatel může přihlásit. V kroku 3 se podíváme na tom, jak přepsat logiku ověřování přihlášení ovládacího prvku tak, že přihlašovací údaje jsou pouze považovány za platné uživatelské jméno a heslo jsou platné a zadané e-mailová adresa odpovídá e-mailovou adresu v souboru.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Krok 3: Úprava logiky ověřování ovládací prvek Login

Když návštěvník dodává jí přihlašovací údaje a klikne na tlačítko, které vyplývá tlačítko přihlásit, zpětné volání a přihlaste se ovládací prvek proběhnou její ověřovací pracovní postup. Pracovní postup se spustí vyčkat [ `LoggingIn` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Všechny obslužné rutiny událostí, který je přidružený k této události může dojít ke zrušení protokolu v operaci tak, že nastavíte `e.Cancel` vlastnost `true`.

Pokud není zrušena protokolu v operaci, pracovním postupu vyvoláním [ `Authenticate` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Pokud je obslužná rutina události `Authenticate` události a pak zodpovídá za určení, jestli zadané přihlašovací údaje jsou platné, nebo ne. Pokud není zadána žádná obslužná rutina události, pomocí ovládacího prvku pro přihlášení `Membership.ValidateUser` metodou ke zjištění platnosti přihlašovacích údajů.

Pokud jsou platné zadané přihlašovací údaje, pak se vytvoří lístek ověřování pomocí formulářů, [ `LoggedIn` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) se vyvolá, a uživatel je přesměrován na příslušnou stránku. Pokud však přihlašovací údaje jsou považovány za neplatné, pak bude [ `LoginError` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) se vyvolá, a zobrazí zpráva informující uživatele, že je jejich přihlašovací údaje neplatné. Ve výchozím nastavení, při selhání přihlášení ovládací prvek jednoduše nastaví jeho `FailureText` popisek vlastnosti textu ovládacího prvku na chybovou zprávu (pokus o přihlášení nebylo úspěšné. Zkuste to prosím znovu). Ale pokud ovládací prvek Login [ `FailureAction` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) je nastavena na `RedirectToLoginPage`, pak přihlašovací problémy ovládacího prvku `Response.Redirect` na přihlašovací stránku přidáním parametru řetězce dotazu `loginfailure=1` (což způsobí, že přihlášení ovládací prvek pro zobrazení zprávy selhání).

Obrázek 9 nabízí vývojový diagram ověřování pracovního postupu.


[![Ovládací prvek Login ověřovací pracovní postup](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**Obrázek 9**: ovládací prvek Login ověřovací pracovní postup ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))


> [!NOTE]
> Pokud vás zajímá je, když použijete `FailureAction`společnosti `RedirectToLogin` stránce možnost, vezměte v úvahu následující scénář. Právě teď naše `Site.master` stránky předlohy aktuálně obsahuje text Hello, stranger zobrazí v levém sloupci, když uživatel anonymního uživatele, ale Představte si, že jsme chtěli tento text nahraďte ovládací prvek Login. To by umožnilo anonymního uživatele k přihlášení z jakékoli stránky na webu, namísto nutnosti jejich nutnost navštivte přímo stránku pro přihlášení. Nicméně pokud uživatel se nemohl přihlásit pomocí ovládacího prvku pro přihlášení pro vykreslení stránky předlohy, může mít smysl pro přesměrování na stránku pro přihlášení (`Login.aspx`), protože tuto stránku pravděpodobně obsahuje další pokyny, odkazy a další pomoc – jako jsou odkazy a vytvoří nový účet nebo načtení získání ztraceného hesla –, který se nepřidala do stránky předlohy.


### <a name="creating-theauthenticateevent-handler"></a>Vytváří`Authenticate`obslužné rutiny události

Aby bylo možné připojit v naší vlastní ověřovací logiku, musíme vytvořit obslužnou rutinu události pro ovládací prvek Login `Authenticate` událostí. Vytvoření obslužné rutiny události pro `Authenticate` událostí bude generovat následující definice obslužné rutiny události:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

Jak je vidět, `Authenticate` obslužná rutina události je předán objekt typu [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) jako svůj druhý vstupní parametr. `AuthenticateEventArgs` Třída obsahuje vlastnost typu Boolean s názvem `Authenticated` , který se používá k určení, jestli zadané přihlašovací údaje jsou platné. Náš úkol, je napsat kód, který určuje, jestli zadané přihlašovací údaje jsou platné, nebo Ne a nastavit `e.Authenticate` vlastnost odpovídajícím způsobem.

### <a name="determining-and-validating-the-supplied-credentials"></a>Určení a ověření zadané přihlašovací údaje

Použijte ovládací prvek Login [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) a [ `Password` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) určit uživatelské jméno a heslo pověření zadané uživatelem. Aby bylo možné určit, že hodnoty zadané do jakékoli další ovládací prvky webového (například `Email` textového pole jsme přidali v předchozím kroku), použijte *`LoginControlID`* `.FindControl`("*`controlID`*") k získání programový odkaz na ovládací prvek webového v šabloně jehož `ID` rovná vlastnost *`controlID`*. Například chcete získat odkaz na `Email` textové pole, použijte následující kód:

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

Chcete-li ověřit přihlašovací údaje uživatele budeme muset udělat dvě věci:

1. Ujistěte se, že jsou platné zadané uživatelské jméno a heslo
2. Zajistěte, aby odpovídal e-mailovou adresu zadali e-mailovou adresu v souboru pro uživatele při pokusu o přihlášení

K provedení první kontrola můžeme jednoduše použít `Membership.ValidateUser` metody, jako jsme viděli v kroku 1. Pro druhý kontrolu musíme určit e-mailovou adresu uživatele tak, že jsme ho můžete porovnat e-mailovou adresu, zadaný do textového pole ovládacího prvku. Chcete-li získat informace o konkrétním uživateli, použijte `Membership` třídy [ `GetUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx).

`GetUser` Metoda má několik přetížení. Pokud použijete bez předávání parametrů, vrátí informace o aktuálně přihlášeného uživatele. Chcete-li získat informace o konkrétním uživateli, zavolejte `GetUser` předanou ve své uživatelské jméno. V obou případech `GetUser` vrátí [ `MembershipUser` objekt](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), který má vlastnosti, jako je `UserName`, `Email`, `IsApproved`, `IsOnline`, a tak dále.

Následující kód implementuje tyto dvě kontroly. Pokud obě úspěšně prošel zpracováním, pak `e.Authenticate` je nastavena na `true`, v opačném případě je přiřazena `false`.

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

S tímto kódem na místě pokuste se přihlásit jako platného uživatele zadání správné uživatelské jméno, heslo a e-mailovou adresu. Zkuste to znovu, ale tentokrát použijte záměrně nesprávné e-mailovou adresu (viz obrázek 10). A konečně vyzkoušejte si to třetí čas pomocí uživatelského jména neexistuje. V prvním případě abyste by měla být úspěšně přihlášení k webu, ale v posledních dvou případech byste měli vidět ovládací prvek Login zpráva neplatné přihlašovací údaje.


[![Tito přihlásit při zadávání nesprávné e-mailovou adresu](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**Obrázek 10**: Tito nelze protokolu v při zadání nesprávné e-mailovou adresu ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))


> [!NOTE]
> Jak je popsáno v části Jak the členství Framework zpracovává neplatné pokusy o přihlášení v kroku 1, když `Membership.ValidateUser` metoda je volána a předány neplatné přihlašovací údaje, který uchovává informace o Neplatný pokus o přihlášení a zamezí uživateli, pokud překročí určitou Neplatné pokusy o zadání v rámci zadaného časového okna prahovou hodnotu. Od naší vlastní ověřovací logiky volání `ValidateUser` metoda nesprávné heslo pro platné uživatelské jméno se zvýší čítač pokusů o neplatné přihlášení, ale tento čítač se zvyšuje v případě, kdy uživatelské jméno a heslo jsou platné, ale e-mailová adresa není správná. Je pravděpodobné, toto chování je vhodný, protože není pravděpodobné, že se hacker znát uživatelské jméno a heslo, ale mají použití technik útoku hrubou silou k určení e-mailovou adresu uživatele.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Krok 4: Zlepšení zpráva neplatné přihlašovací údaje ovládací prvek Login

Když se uživatel pokusí přihlásit s neplatnými přihlašovacími údaji, ovládací prvek Login zobrazí se zpráva s vysvětlením, že pokus o přihlášení nebylo úspěšné. Konkrétně se ovládací prvek zobrazí zprávu určenou podle jeho [ `FailureText` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), který má výchozí hodnotu pokus o přihlášení nebyl úspěšný. Zkuste to prosím znovu.

Připomínáme, že existuje mnoho důvodů, proč může být neplatný přihlašovacích údajů uživatele:

- Uživatelské jméno neexistuje.
- Uživatelské jméno existuje, ale heslo je neplatné
- Uživatelské jméno a heslo jsou platné, ale uživatel ještě není schválené
- Uživatelské jméno a heslo jsou platné, ale uživatel je uzamčen, out (pravděpodobně vzhledem k tomu, že byl překročen počet neplatných pokusů o přihlášení v rámci zadaného časového rámce)

A mohou nastat z jiných důvodů při použití vlastní ověřovací logiku. Například s kódem jsme napsali v kroku 3, uživatelské jméno a heslo může být platná, ale e-mailová adresa může být nesprávný.

Bez ohledu na to, proč jsou neplatné přihlašovací údaje zobrazí ovládacího prvku pro přihlášení ke stejné chybové zprávě. Tato nedostatečná zpětná vazba může být matoucí pro uživatele, jehož účet ještě nepovolila nebo který byl uzamčen. Stačí nepatrné práce jsme ale mít ovládací prvek Login zobrazí další odpovídající zprávu.

Vždy, když se uživatel pokusí přihlásit s neplatnými přihlašovacími údaji ovládací prvek Login vyvolá jeho `LoginError` událostí. Pokračujte a vytvořte obslužnou rutinu události pro tuto událost a přidejte následující kód:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

Výše uvedený kód spustí nastavením ovládací prvek Login `FailureText` vlastnost na výchozí hodnotu (pokus o přihlášení nebylo úspěšné. Zkuste to prosím znovu). Pak zkontroluje, pokud zadané uživatelské jméno se mapuje na existující uživatelský účet. Pokud tedy consults výsledný `MembershipUser` objektu `IsLockedOut` a `IsApproved` vlastnosti, které chcete určit, jestli účet byl uzamčen nebo dosud nebyly schváleny. V obou případech `FailureText` vlastností se aktualizuje na odpovídající hodnotu.

K otestování tohoto kódu, záměrně pokus o přihlášení jako stávajícího uživatele, ale pomocí nesprávného hesla. Proveďte tuto pětkrát po sobě v časovém rámci 10 minut a účet uzamčen. Jak ukazuje obrázek 11, následné přihlašovací pokusy bude vždy selhání (i s správné heslo), ale teď bude zobrazovat více popisné byl váš účet uzamčen kvůli moc velký počet neplatných pokusů o přihlášení. Obraťte se prosím na správce, aby zprávy odemknout účet.


[![Tito provést moc velký počet neplatných pokusů o přihlášení a byl uzamčen](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**Obrázek 11**: Tito provádí příliš mnoho neplatný pokusů o přihlášení a má byl uzamčen Out ([kliknutím ji zobrazíte obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))


## <a name="summary"></a>Souhrn

Předchozí tento kurz, zadané přihlašovací údaje pevně zakódované seznam dvojic uživatelského jména a hesla ověřit naše přihlašovací stránku. V tomto kurzu jsme aktualizovali na stránce pro ověření pověření členství rámec. V kroku 1 jsme se podívali na použití `Membership.ValidateUser` metoda prostřednictvím kódu programu. V kroku 2 bylo nahrazeno naše ručně vytvořené uživatelské rozhraní a kódu ovládacího prvku pro přihlášení.

Ovládací prvek Login vykreslí přihlášení standardu uživatelské rozhraní a automaticky ověřuje pověření uživatele rámec členství. Kromě toho v případě platné přihlašovací údaje, ovládací prvek Login přihlásí uživatele prostřednictvím ověřování pomocí formulářů. Stručně řečeno plně funkční přihlašovací prostředí uživatelů je k dispozici jednoduše přetažením ovládacího prvku pro přihlášení na stránky, bez dalších deklarativní nebo kód. Navíc je vysoce přizpůsobitelné, umožňuje bez problémů stupeň kontroly nad obě vygenerované uživatelské rozhraní a ověřovací logiku ovládacího prvku pro přihlášení.

V tomto okamžiku můžete návštěvníků našeho webu vytvořte nový uživatelský účet a protokol do lokality, ale musíme ještě podívejte se na omezení přístupu k stránkám na základě ověřeného uživatele. Každý uživatel ověřený nebo anonymní, v současné době může zobrazení všech stránek na našem webu. Spolu se řízení přístupu na stránky náš web na základě uživatele podle uživatelů, můžeme mít některé stránky, jehož funkce závisí na uživatele. V dalším kurzu bude vypadat na tom, jak omezit přístup a na stránce funkce na základě přihlášeného uživatele.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Zobrazování vlastních zpráv, které uzamčen a neschválený uživatelů](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Zkoumání ASP.NET 2.0 pro členství, role a profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Postupy: Vytvoření přihlašovací stránky technologie ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Technická dokumentace ovládací prvek Login](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Pomocí přihlašovacích ovládacích prvků](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, Autor více ASP/ASP.NET knih a zakladatelem 4GuysFromRolla.com, má pracovali Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy  *[Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott může být dostupný na adrese [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Teresy Murphy a Michael Olivero. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Předchozí](creating-user-accounts-cs.md)
> [další](user-based-authorization-cs.md)
