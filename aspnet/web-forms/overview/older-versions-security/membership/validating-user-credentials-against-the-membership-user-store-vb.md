---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
title: "Probíhá ověřování pověření uživatele proti úložišti uživatele členství (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu vyzkoušíme postup ověření přihlašovacích údajů uživatele proti úložišti uživatele členství pomocí programový znamená a ovládací prvek pro přihlášení..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 17772912-b47b-4557-9ce9-80f22df642f7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
msc.type: authoredcontent
ms.openlocfilehash: f57bc8c32757c1ea25bf6bbb34539570e4c09aad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="validating-user-credentials-against-the-membership-user-store-vb"></a>Probíhá ověřování pověření uživatele proti úložišti uživatele členství (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_VB.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_vb.pdf)

> V tomto kurzu vyzkoušíme postup ověření přihlašovacích údajů uživatele proti úložišti uživatele členství pomocí programový znamená a ovládací prvek pro přihlášení. Podíváme se také na tom, jak přizpůsobit vzhled a chování ovládacího prvku přihlášení.


## <a name="introduction"></a>Úvod

V <a id="Tutorial05"> </a> [předchozí kurzu](creating-user-accounts-vb.md) jsme se podívali na tom, jak vytvořit nový uživatelský účet v rámci členství. Jsme nejprve hledá v prostřednictvím kódu programu vytváření uživatelských účtů pomocí `Membership` třídy `CreateUser` metoda a potom zkontrolován pomocí ovládacího prvku webového CreateUserWizard. Na přihlašovací stránku však ověří aktuálně zadané přihlašovací údaje s pevně zakódovaným seznamu párů uživatelské jméno a heslo. Je potřeba aktualizovat logiku přihlašovací stránku tak, aby se ověřuje pověření proti úložiště framework členství uživatele.

Mnohem jako s vytváření uživatelských účtů, můžete ověřit pověření prostřednictvím kódu programu nebo deklarativně. Rozhraní API členství zahrnuje metodu pro ověřování prostřednictvím kódu programu přihlašovacích údajů uživatele proti úložiště uživatele. A ASP.NET se dodává s ovládacím prvkem webové přihlášení, která vykreslí uživatelské rozhraní s textová pole pro uživatelské jméno a heslo a tlačítko přihlásit.

V tomto kurzu vyzkoušíme postup ověření přihlašovacích údajů uživatele proti úložišti uživatele členství pomocí programový znamená a ovládací prvek pro přihlášení. Podíváme se také na tom, jak přizpůsobit vzhled a chování ovládacího prvku přihlášení. Můžeme začít!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Krok 1: Probíhá ověřování pověření pro úložiště členství uživatele

Pro webové servery, které používají ověřování pomocí formulářů přihlášení uživatele na web návštěvou přihlašovací stránku a zadat své přihlašovací údaje. Tyto přihlašovací údaje se pak porovnávají úložiště uživatele. Pokud jsou platné, uživateli je udělen lístek ověřování formulářů, což je token zabezpečení, který označuje identity a jejich pravost návštěvníka.

K ověření uživatele vůči rozhraní členství, použijte `Membership` třídy [ `ValidateUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx). `ValidateUser` Metoda přebírá dva vstupní parametry - `username` a `password` - a vrátí logickou hodnotu udávající, zda byly přihlašovací údaje platná. Upozorňujeme `CreateUser` metoda jsme se zaměřili na v tomto kurzu předchozí `ValidateUser` metoda deleguje samotné ověření do nakonfigurovaného zprostředkovatele členství.

`SqlMembershipProvider` Ověřuje zadaná pověření tak, že získání hesla zadaného uživatele prostřednictvím `aspnet_Membership_GetPasswordWithFormat` uložené procedury. Odvolat, který `SqlMembershipProvider` ukládá hesla uživatelů ke službám pomocí jedné ze tří formátů: clear, šifrovaná, nebo s použitím algoritmu hash. `aspnet_Membership_GetPasswordWithFormat` Uložené procedury vrátí heslo v jeho nezpracovaném formátu. Pro šifrovaná, nebo hodnotu hash hesla `SqlMembershipProvider` transformuje `password` hodnoty předané `ValidateUser` metoda do ekvivalentu šifrovat nebo stavu s použitím algoritmu hash a porovná je s co byla vrácena z databáze. Pokud heslo, které jsou uloženy v databázi odpovídá formátovaný heslo zadané uživatelem, jsou pověření platná.

Umožňuje aktualizovat naše přihlašovací stránku (~ /`Login.aspx`) tak, aby ověřuje zadaná pověření proti úložiště framework uživatele členství. Jsme vytvořili této přihlašovací stránce zpět v <a id="Tutorial02"> </a> [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-vb.md) kurzu, vytváření rozhraní se dvě textová pole pro uživatelské jméno a heslo, Zapamatovat uživatele zaškrtávací políčko a tlačítko pro přihlášení (viz obrázek 1). Kód ověřuje zadaná pověření proti pevně seznamu párů uživatelské jméno a heslo (Scott a hesla, Jisun a hesla a Sam a hesla). V <a id="Tutorial03"> </a> [ *konfiguraci ověřování formulářů a rozšířené témata* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) kurzu aktualizovali jsme přihlašovací stránky kód pro uložení dalších informací ve formulářích lístek ověřování `UserData` vlastnost.


[![Rozhraní přihlašovací stránku obsahuje dvě textová pole, třídy CheckBoxList a tlačítko](validating-user-credentials-against-the-membership-user-store-vb/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image1.png)

**Obrázek 1**: přihlašovací stránky rozhraní zahrnuje dvě textová pole, třídy CheckBoxList a tlačítko ([Kliknutím zobrazit obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-vb/_static/image3.png))


Uživatelské rozhraní přihlašovací stránky může zůstat beze změny, ale je potřeba nahradit přihlašovací tlačítko `Click` obslužné rutiny události s kódem, který ověřuje uživatele před úložiště framework uživatele členství. Aktualizujte obslužné rutiny události tak, aby jeho kód vypadat takto:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample1.vb)]

Tento kód je pozoruhodně jednoduché. Začneme voláním `Membership.ValidateUser` metody předávání v zadané uživatelské jméno a heslo. Pokud tato metoda vrátí hodnotu True, pak uživatel je přihlášen k webu prostřednictvím `FormsAuthentication` metodu RedirectFromLoginPage třídy. (Jak již bylo zmíněno <a id="Tutorial02"> </a> [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-vb.md) kurzu `FormsAuthentication.RedirectFromLoginPage` vytvoří lístek pro ověřování formuláře a pak přesměruje uživatele na příslušnou stránku.) Pokud přihlašovací údaje jsou neplatné, ale `InvalidCredentialsMessage` zobrazí popisek informující uživatele, jejich uživatelské jméno nebo heslo je nesprávné.

To je všechno je k němu!

Pokud chcete otestovat, že přihlašovací stránky funguje podle očekávání, pokuste se přihlásit s jedním z uživatelské účty, které jste vytvořili v předchozím kurzu. Případně, pokud jste ještě nevytvořili účet, pokračujte a vytvořit z `~/Membership/CreatingUserAccounts.aspx` stránky.

> [!NOTE]
> Když uživatel zadá své přihlašovací údaje a přihlašovací stránky formulář odešle, přihlašovací údaje, včetně jeho heslo přenášejí přes Internet k webovému serveru v *prostý text*. To znamená, že všechny poskytuje sledování toku dat síťový provoz můžete zobrazit uživatelské jméno a heslo. Chcete-li tomu zabránit, je nezbytné k šifrování síťového provozu pomocí [vrstvy SSL (Secure Socket)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Tím bude zajištěno, že přihlašovací údaje (stejně jako značka jazyka HTML celou stránku) jsou šifrována od okamžiku, kdy opustí prohlížeče, dokud nedorazí k příjemci webový server.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Jak Framework členství zpracovává neplatných pokusů o přihlášení

Návštěvník dosáhne přihlašovací stránku a odešle přihlašovacích údajů, jejich prohlížeč odešle požadavek HTTP na přihlašovací stránku. Pokud jsou pověření platná, odpověď HTTP, která zahrnuje lístek ověřování v souboru cookie. Proto se hacker pokusu rozdělit na váš web vytvořit program, který podrobně odešle požadavky HTTP na přihlašovací stránku s platné uživatelské jméno a odhad na heslo. Pokud odhad heslo je správné, vrátí stránku pro přihlášení ověřovacího lístku souboru cookie, okamžiku program ví, že má stumbled po pár platné uživatelské jméno a heslo. Útok hrubou silou může být takového programu moct klopýtnout při heslo uživatele, zejména v případě, že je slabé heslo.

Pokud chcete zabránit takové útoky hrubou silou, členství v rámci zamezí uživatele Pokud je počet pokusů o neúspěšných přihlášení v časovém období. Přesné parametry se dají konfigurovat prostřednictvím následující nastavení konfigurace dvou zprostředkovatele členství:

- `maxInvalidPasswordAttempts`-Určuje, kolik neplatné heslo pokusy jsou povoleny pro uživatele během časového období před zablokováním účtu. Výchozí hodnota je 5.
- `passwordAttemptWindow`-Určuje časové období v minutách, během které způsobí zadaný počet neplatných pokusů o přihlášení, účet bude uzamčen. Výchozí hodnota je 10.

Pokud uživatel byl uzamčen, nelze se přihlásit, dokud správce odemkne svůj účet. Pokud je uživatel uzamčen, `ValidateUser` metoda bude *vždy* vrátit `False`i v případě, že jsou zadané platné přihlašovací údaje. Když toto chování snižuje pravděpodobnost, že se hacker se rozdělit na váš web prostřednictvím metody hrubou silou, se můžou skončit uzamykání platný uživatel, který jednoduše zapomněl svoje heslo nebo omylem má klávesy Caps Lock na či má chybný zadáním den.

Bohužel neexistuje žádné předdefinované nástroj pro odemknutí uživatelský účet. Chcete-li odemknout účet, můžete upravit databázi přímo - změnit `IsLockedOut` pole `aspnet_Membership` tabulky pro k odpovídajícímu uživatelskému účtu - nebo vytvořte webové rozhraní, které uvádí uzamčení účtů s možnostmi pro odemčení. Vyzkoušíme vytváření rozhraní pro správu vykonávání běžných úloh souvisejícím s účtem a role uživatele v budoucnu kurzu.

> [!NOTE]
> Jedinou nevýhodou `ValidateUser` metoda je, že když zadané přihlašovací údaje jsou neplatné, neposkytuje žádné vysvětlení, proč. Přihlašovací údaje zřejmě není platná, protože v úložišti uživatele neexistuje žádné odpovídající uživatelské jméno a heslo pár nebo protože uživatel ještě nebyla schválena, nebo protože uživatel byl uzamčen. V kroku 4 jsme se zobrazí zobrazení více podrobná zpráva uživateli při jeho pokusu o přihlášení selže.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Krok 2: Shromažďování přihlašovacích údajů prostřednictvím řízení webové přihlášení

[Ovládací prvek webu přihlášení](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) vykreslí velmi podobné jsme vytvořili zpět ve výchozím nastavení uživatelské rozhraní <a id="Tutorial02"> </a> [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-vb.md) kurzu. Použití ovládacího prvku přihlášení nám šetří práci museli vytvářet rozhraní pro zadání přihlašovacích údajů návštěvníka. Kromě toho ovládací prvek pro přihlášení automaticky přihlásí uživatel (za předpokladu, že odeslaná přihlašovací údaje jsou platné), a tím ukládání nám z nutnosti psaní jakéhokoli kódu.

Umožňuje aktualizovat `Login.aspx`, nahraďte ručně vytvořené rozhraní a kódu s ovládacím prvkem přihlášení. Začněte tím, že odebrání stávající kód a kódu v `Login.aspx`. Může odstranit přímý nebo ho jednoduše komentář. Na komentář deklarativní, uzavřete ji s `<%--` a `--%>` oddělovače. Tyto oddělovače lze zadat ručně, nebo, jak je vidět na obrázku 2, můžete si vybrat text poznámky, a potom klikněte na Odkomentujte vybrané řádky ikonu na panelu nástrojů. Podobně můžete Odkomentujte ikonu vybrané řádky komentář na vybraný úsek kódu ve třídě kódu.


[![Komentář existující deklarativní a zdrojový kód v Login.aspx](validating-user-credentials-against-the-membership-user-store-vb/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image4.png)

**Obrázek 2**: komentář se existující deklarativní značky a zdrojový kód v Login.aspx ([Kliknutím zobrazit obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-vb/_static/image6.png))


> [!NOTE]
> Odkomentujte ikonu vybrané řádky není k dispozici, při zobrazení deklarativní v sadě Visual Studio 2005. Pokud nepoužíváte Visual Studio 2008 je potřeba ručně přidat `<%--` a `--%>` oddělovače.


Přetáhněte ovládací prvek přihlášení z panelu nástrojů na stránku a nastavit jeho `ID` vlastnost `myLogin`. Obrazovky v tomto okamžiku by měla vypadat podobně jako na obrázku 3. Všimněte si, že přihlášení ovládacího prvku výchozí rozhraní obsahuje ovládací prvky textové pole pro uživatelské jméno a heslo, Zapamatovat uživatele při příštím zaškrtávací políčko a tlačítko v protokolu. Existují také `RequiredFieldValidator` ovládací prvky pro dvou textových polí.


[![Přidání ovládacího prvku přihlášení na stránce](validating-user-credentials-against-the-membership-user-store-vb/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image7.png)

**Obrázek 3**: Přidání ovládacího prvku přihlášení na stránce ([Kliknutím zobrazit obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-vb/_static/image9.png))


A máme Hotovo! Při kliknutí na tlačítko přihlásit přihlášení ovládacího prvku, dojde ke zpětné volání a bude volat ovládací prvek pro přihlášení `Membership.ValidateUser` metody předávání v zadané uživatelské jméno a heslo. Pokud přihlašovací údaje jsou neplatné, zobrazí se zpráva například ovládací prvek pro přihlášení. Pokud však jsou platná pověření, ovládací prvek pro přihlášení vytvoří lístek pro ověřování formuláře a přesměruje uživatele na příslušnou stránku.

Ovládací prvek pro přihlášení pomocí čtyř faktorů zjistí na příslušnou stránku a přesměruje uživatele na po úspěšném přihlášení:

- Zda je ovládací prvek pro přihlášení na přihlašovací stránce podle definice `loginUrl` nastavení v konfiguraci ověřování formulářů; toto nastavení výchozí hodnota je`Login.aspx`
- Přítomnost `ReturnUrl` parametr řetězce dotazu
- Hodnota prvku přihlášení [ `DestinationUrl` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- `defaultUrl` Zadána hodnota ve formulářích nastavení konfigurace ověřování; toto nastavení výchozí hodnota je Default.aspx

Jak znázorňuje obrázek 4 ovládací prvek pro přihlášení používá tyto čtyři parametry přijaty ve své rozhodnutí příslušnou stránku.


[![Přidání ovládacího prvku přihlášení na stránce](validating-user-credentials-against-the-membership-user-store-vb/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image10.png)

**Obrázek 4**: Přidání ovládacího prvku přihlášení na stránce ([Kliknutím zobrazit obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-vb/_static/image12.png))


K otestování ovládací prvek pro přihlášení návštěvou webu přes prohlížeč a přihlaste se jako stávajícího uživatele v rámci členství chvíli trvat.

Ovládací prvek přihlášení vykreslené rozhraní je vysoce konfigurovatelné. Existuje několik vlastností, které ovlivňují její vzhled; Navíc ovládací prvek pro přihlášení můžete převést na šablonu pro přesnou kontrolu nad rozložení prvky uživatelského rozhraní. Zbývající část tohoto kroku prověří tom, jak přizpůsobit vzhled a rozložení.

### <a name="customizing-the-login-controls-appearance"></a>Přizpůsobení vzhledu ovládacího prvku přihlášení

Výchozí nastavení vlastností ovládacího prvku přihlášení vykreslení uživatelského rozhraní s názvem (přihlásit), textové pole a popisek – ovládací prvky pro uživatelské jméno a heslo vstupy, Zapamatovat uživatele další čas zaškrtávací políčko a tlačítko přihlásit. Objevuje z těchto elementů se všechny konfigurovatelné prostřednictvím mnoho vlastností ovládacího prvku přihlášení. Kromě toho mohou být přidány další prvky uživatelského rozhraní – například odkaz na stránku, kterou chcete vytvořit nový uživatelský účet - nastavením vlastnosti nebo dvě.

Věnujte čas na gussy až vzhled ovládacího prvku naše přihlášení. Vzhledem k tomu `Login.aspx` stránku už má textu v horní části stránky, která uvádí přihlášení, a název prvku přihlášení je nadbytečné. Proto Vyčistit [ `TitleText` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) hodnotu, aby bylo možné odebrat nadpis prvek přihlášení.

Uživatelské jméno: a heslo: popisky nalevo od dvou ovládacích prvků textového pole lze přizpůsobit pomocí [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) a [ `PasswordLabelText` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), v uvedeném pořadí. Změňte uživatelské jméno: štítek, který chcete číst uživatelské jméno:. Styly popisek a textové pole se dají konfigurovat prostřednictvím [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) a [ `TextBoxStyle` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx), v uvedeném pořadí.

Zapamatovat uživatele další čas políčko Text – vlastnost lze nastavit pomocí prvku přihlášení [ `RememberMeText` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx), a jeho výchozím nastavení zaškrtnuto, stav se dá konfigurovat pomocí [ `RememberMeSet` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx)(které výchozí hodnota je False). Pokračovat a nastavit `RememberMeSet` vlastnost na hodnotu True, aby Zapamatovat uživatele zaškrtávací políčko vedle času bude ve výchozím nastavení zaškrtnuto.

Ovládací prvek pro přihlášení nabízí dvě vlastnosti pro úpravu rozložení jeho ovládacích prvků uživatelského rozhraní. [ `TextLayout` Vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) označuje, zda uživatelské jméno: a heslo: popisky se zobrazí vlevo jejich odpovídající textových polí (výchozí), nebo nad nimi. [ `Orientation` Vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) označuje, zda vstupy uživatelské jméno a heslo jsou umístěna ve svislém směru (jeden nad sebou) nebo vodorovně. Přechod do ponechat tyto dvě vlastnosti nastavit na výchozí hodnoty, ale I doporučujeme zkuste nastavit tyto dvě vlastnosti na jiné než výchozí hodnoty zobrazíte výsledný efekt.

> [!NOTE]
> V další části Konfigurace rozložení ovládacího prvku přihlášení, podíváme se na definování přesné rozložení rozložení ovládacích prvků uživatelského rozhraní pomocí šablony.


Zabalení nastavení vlastností ovládacího prvku přihlášení nastavením [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) a [ `CreateUserUrl` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) k není ještě zaregistrována? Vytvoření účtu! a `~/Membership/CreatingUserAccounts.aspx`, v uvedeném pořadí. Tím se přidá přihlášení ovládacího prvku rozhraní přejdete na stránku jsme vytvořili v hypertextový odkaz <a id="Tutorial05"> </a> [předchozí kurzu](creating-user-accounts-vb.md). Ovládací prvek přihlášení [ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) a [ `HelpPageUrl` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) a [ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) a [ `PasswordRecoveryUrl` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) pracovní stejným způsobem, vykreslování odkazů na stránky nápovědy a na stránce obnovení hesla.

Po provedení těchto změn vlastnosti, by měla vypadat podobná vidět na obrázku 5 deklarativní a vzhled ovládacího prvku přihlášení.


[![Hodnoty vlastností ovládacího prvku přihlášení, určují její vzhled](validating-user-credentials-against-the-membership-user-store-vb/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image13.png)

**Obrázek 5**: přihlášení vlastností ovládacího prvku' hodnoty stanovují její vzhled ([Kliknutím zobrazit obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-vb/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Konfigurace rozložení ovládacích prvků přihlášení

Ovládací prvek webu přihlášení výchozí uživatelské rozhraní rozložen rozhraní v kódu HTML `<table>`. Ale co když je třeba lepší kontrolu nad Vykreslený výstup? Možná chcete nahradit `<table>` s řadou `<div>` značky. Nebo co v případě naší aplikace vyžaduje další pověření pro ověřování? Mnohé weby finančních, například vyžadovat, aby zadejte jenom uživatelské jméno a heslo, ale také osobní identifikační číslo (PIN) nebo jiné identifikační údaje uživatelé. Ať může být důvodům, je možné převést na šablonu, ze kterého můžete explicitně definujeme rozhraní deklarativní ovládací prvek pro přihlášení.

Budeme muset udělat dvě věci, aby bylo možné aktualizovat ovládací prvek pro přihlášení ke shromažďování další přihlašovací údaje:

1. Aktualizujte rozhraní ovládacího prvku přihlášení zahrnout webových ovládacích prvků shromažďovat další přihlašovací údaje.
2. Přepsání logiku interní ověřování přihlášení ovládacího prvku tak, aby pokud uživatelského jména a hesla je platná a jejich další přihlašovací údaje jsou platné, příliš pouze ověření uživatele.

K provedení první úlohy, je potřeba převést ovládací prvek pro přihlášení do šablony a přidat potřebné ovládací prvky webového. Jako v druhé úloze, může být potlačeno logiku ověřování přihlášení ovládacího prvku tak, že vytvoříte obslužné rutiny události pro ovládací prvek [ `Authenticate` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx).

Umožňuje aktualizovat ovládací prvek pro přihlášení, tak, aby vyzve uživatele pro jejich uživatelské jméno, heslo a e-mailovou adresu a pouze ověřuje uživatele, pokud odpovídá e-mailová adresa zadaná e-mailové adresy na soubor. Nejprve musíme rozhraní ovládacího prvku přihlášení převést na šablonu. Z ovládacího prvku přihlášení inteligentních značek zvolte Převést na šablonu možnost.


[![Převést ovládací prvek pro přihlášení do šablony](validating-user-credentials-against-the-membership-user-store-vb/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image16.png)

**Obrázek 6**: převést ovládací prvek pro přihlášení do šablony ([Kliknutím zobrazit obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-vb/_static/image18.png))


> [!NOTE]
> Zrušit ovládací prvek pro přihlášení k jeho předem template verze, a klikněte na odkaz resetování ze inteligentní značky ovládacího prvku.


Převádění ovládací prvek pro přihlášení do šablony přidá `LayoutTemplate` do ovládacího prvku deklarativní s elementů HTML a ovládací prvky webového definování uživatelského rozhraní. Jak je vidět na obrázku 7, převod ovládacího prvku do šablony odebere počet vlastností v okně Vlastnosti, jako `TitleText`, `CreateUserUrl`, a tak dále, protože hodnoty těchto vlastností jsou ignorovány, při použití šablony.


[![Méně vlastností jsou že k dispozici při přihlášení řídicí je převést na šablonu](validating-user-credentials-against-the-membership-user-store-vb/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image19.png)

**Obrázek 7**: méně vlastnosti jsou k dispozici při přihlášení řídicí je převést na šablonu ([Kliknutím zobrazit obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-vb/_static/image21.png))


Značka jazyka HTML v `LayoutTemplate` může podle potřeby upravit. Podobně Nebojte se přidat všechny nové prvky, webové šablony. Je však důležité ovládací prvky webového jádra daného ovládacího prvku přihlášení zůstat v šabloně a zachovat jejich přiřazené `ID` hodnoty. Konkrétně neodebere ani přejmenovat `UserName` nebo `Password` textová pole, `RememberMe` zaškrtávací políčko, `LoginButton` tlačítko `FailureText` štítek, nebo `RequiredFieldValidator` ovládací prvky.

Shromažďovat návštěvníka e-mailovou adresu, je potřeba přidat textové pole v šabloně. Přidejte následující kód mezi řádku tabulky (`<tr>`), který obsahuje `Password` textové pole a řádku tabulky, který obsahuje Zapamatovat uživatele vedle čas zaškrtávací políčko:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample2.aspx)]

Po přidání `Email` textovému poli, najdete na stránce prostřednictvím prohlížeče. Jak ukazuje obrázek 8, uživatelské rozhraní ovládacího prvku přihlášení nyní zahrnuje třetí textové pole.


[![Ovládací prvek pro přihlášení nyní zahrnuje textové pole pro e-mailovou adresu uživatele](validating-user-credentials-against-the-membership-user-store-vb/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image22.png)

**Obrázek 8**: ovládací prvek pro přihlášení nyní zahrnuje textové pole pro e-mailovou adresu uživatele ([Kliknutím zobrazit obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-vb/_static/image24.png))


V tomto okamžiku je stále pomocí ovládacího prvku přihlášení `Membership.ValidateUser` metodu za účelem ověření zadané přihlašovací údaje. Odpovídajícím způsobem, hodnotu zadat do `Email` TextBox nemá žádný vliv na tom, jestli uživatel může přihlásit. V kroku 3 Podíváme se na postup přepsání logiku ověřování přihlášení ovládacího prvku tak, aby přihlašovací údaje jsou pouze považovány za platné Pokud uživatelské jméno a heslo jsou platné a zadané e-mailová adresa odpovídá e-mailovou adresu na soubor.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Krok 3: Úprava logiku ověřování přihlášení ovládacího prvku

Když návštěvník dodává jí přihlašovací údaje a klikne na tlačítko, které vyplývá tlačítko přihlásit, zpětné volání a přihlášení řídit proběhnou jeho ověřovací pracovní postup. Pracovní postup spustí zobrazením [ `LoggingIn` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Všechny obslužné rutiny přidružené k této události může zrušit protokolu v operaci nastavením `e.Cancel` vlastnost `True`.

Pokud není v protokolu v operaci zrušit, pracovní postup hodnoty zobrazením [ `Authenticate` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Pokud je obslužné rutiny události pro `Authenticate` událostí a pak je zodpovědný za určení toho, jestli zadané přihlašovací údaje jsou platné, nebo ne. Pokud je zadána žádná obslužná rutina události, používá ovládací prvek pro přihlášení `Membership.ValidateUser` metoda k určení platnost přihlašovacích údajů.

Pokud zadané přihlašovací údaje jsou platné, pak se vytvoří lístek pro ověřování pomocí formulářů, [ `LoggedIn` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) se vyvolá, a uživatel se přesměruje na příslušnou stránku. Pokud však přihlašovací údaje jsou považovány za neplatné, pak se [ `LoginError` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) se vyvolá, a se zobrazí zpráva informující uživatele, že jejich přihlašovacích údajů byla neplatná. Ve výchozím nastavení, při selhání přihlášení řízení jednoduše nastaví jeho `FailureText` Label Text ovládacího prvku na selhání zprávy (pokus o přihlášení nebylo úspěšné. Zkuste to prosím znovu). Ale pokud přihlášení ovládacího prvku [ `FailureAction` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) je nastaven na `RedirectToLoginPage`, pak problémy řízení přihlášení `Response.Redirect` na přihlašovací stránku připojením parametru řetězce dotazu `loginfailure=1` (což způsobí, že přihlášení ovládací prvek zobrazí zpráva o neúspěšném zpracování).

Obrázek 9 nabízí vývojový diagram pracovního postupu ověřování.


[![Pracovní postup ověření přihlášení ovládacího prvku](validating-user-credentials-against-the-membership-user-store-vb/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image25.png)

**Obrázek 9**: pracovní postup ověření přihlášení řízení ([Kliknutím zobrazit obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-vb/_static/image27.png))


> [!NOTE]
> Pokud jste zajímá, kdy by mohl použít `FailureAction`na `RedirectToLogin` stránky možnost, vezměte v úvahu následující scénář. Nyní naše `Site.master` stránky předlohy aktuálně má text, Hello, stranger zobrazovaný v levém sloupci při návštěvě anonymního uživatele, ale Představte si, že jsme chtěli nahraďte tento text s ovládacím prvkem přihlášení. To by umožnilo anonymního uživatele k přihlášení z libovolné stránky na webu, tedy nutné přímo navštívit stránku přihlášení. Ale pokud uživatele se nepodařilo přihlásit pomocí ovládacího prvku přihlášení pro vykreslení stránky předlohy, může být vhodné k jejich přesměrování na přihlašovací stránku (`Login.aspx`), protože této stránce pravděpodobně obsahuje další pokyny, odkazy a další Nápověda - například odkazy na vytvoření nový účet nebo načtení ztraceného hesla -, které nebyly přidané k hlavní stránce.


### <a name="creating-theauthenticateevent-handler"></a>Vytváření`Authenticate`obslužné rutiny události

Aby bylo možné zařadit logiku vlastního ověřování, je potřeba vytvořit obslužnou rutinu události pro ovládací prvek přihlášení `Authenticate` událostí. Vytvoření obslužné rutiny události pro `Authenticate` událostí vygeneruje následující definice obslužná rutina události:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample3.vb)]

Jak je vidět `Authenticate` obslužné rutiny události se předá objekt typu [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) jako svůj druhý vstupní parametr. `AuthenticateEventArgs` Třída obsahuje vlastnost typu Boolean s názvem `Authenticated` používané k určení, zda jsou zadaná pověření platná. Naše úkolů, pak je k zápisu sem kód, který určí, zda jsou zadaná pověření platná nebo není a nastavit `e.Authenticate` vlastnost odpovídajícím způsobem.

### <a name="determining-and-validating-the-supplied-credentials"></a>Určení a ověřování zadané přihlašovací údaje

Použití ovládacího prvku přihlášení [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) a [ `Password` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) určit uživatelské jméno a heslo přihlašovací údaje zadané uživatelem. Aby bylo možné zjistit hodnot zadaných do jakékoli další ovládací prvky webového (například `Email` TextBox jsme přidali v předchozím kroku), použijte `LoginControlID.FindControl`("*`controlID`*") získat programový odkaz na webu řízení v šabloně, jehož `ID` vlastnost rovná  *`controlID`* . Chcete-li třeba `Email` textovému poli, použijte následující kód:

`Dim EmailTextBox As TextBox = CType(myLogin.FindControl("Email"), TextBox)`

Aby bylo možné ověření přihlašovacích údajů uživatele je potřeba udělat dvě věci:

1. Ujistěte se, že zadané uživatelské jméno a heslo jsou platné
2. Zajistěte, aby odpovídal e-mailovou adresu zadali e-mailovou adresu na souboru pro uživatele došlo k pokusu o přihlášení

K provedení první kontrola můžeme jednoduše použít `Membership.ValidateUser` metoda jako jsme viděli v kroku 1. Pro druhý kontrolu musíme určit e-mailovou adresu uživatele, aby jsme můžete porovnat s e-mailová adresa zadaná do ovládacího prvku textového pole. Chcete-li získat informace o konkrétní uživatele, použijte `Membership` třídy [ `GetUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx).

`GetUser` Metoda má několik přetížení. Pokud se použije bez předávání žádné parametry, vrátí informace o aktuálně přihlášeného uživatele. Chcete-li získat informace o konkrétní uživatel, volejte `GetUser` předávání v své uživatelské jméno. V obou případech `GetUser` vrátí [ `MembershipUser` objekt](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), který má vlastnosti, například `UserName`, `Email`, `IsApproved`, `IsOnline`a tak dále.

Následující kód implementuje tyto dvě kontroly. Pokud obě podmínky, pak `e.Authenticate` je nastaven na `True`, v opačném případě je přiřazen `False`.

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample4.vb)]

S tímto kódem na místě pokuste se přihlásit jako platného uživatele, zadání správné uživatelské jméno, heslo a e-mailovou adresu. Zkuste to znovu, ale tentokrát záměrně použít nesprávný e-mailovou adresu (viz obrázek 10). Nakonec vyzkoušejte třetí čas pomocí uživatelského jména neexistující. V prvním případě je by měla být úspěšně přihlášen k lokalitě, ale v posledních dvou případech se měla zobrazit zpráva prvek přihlášení neplatné přihlašovací údaje.


[![Tito nemůže přihlásit při zadávání nesprávné e-mailová adresa](validating-user-credentials-against-the-membership-user-store-vb/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image28.png)

**Obrázek 10**: Tito nelze protokolu v při zadávání nesprávné e-mailovou adresu ([Kliknutím zobrazit obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-vb/_static/image30.png))


> [!NOTE]
> Jak je popsáno v části Jak členství Framework zpracovává neplatný pokusů o přihlášení v kroku 1, pokud `Membership.ValidateUser` metoda je volána a předán neplatné přihlašovací údaje, uchovává informace o pokus o neplatné přihlášení a zamezí uživatele, pokud se překročí určitou Prahová hodnota neplatných pokusů o zadání v rámci určeného časového období. Od našich volání logiku vlastního ověřování `ValidateUser` metoda nesprávné heslo pro platné uživatelské jméno se zvýší čítač pokusů o neplatné přihlášení, ale hodnota tohoto čítače se zvýší v případě, kde uživatelské jméno a heslo jsou platné, ale e-mailová adresa je nesprávný. Je možné se, toto chování je vhodná, protože nepravděpodobné, že se hacker znát uživatelské jméno a heslo, ale muset použít techniky hrubou silou k určení e-mailovou adresu uživatele.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Krok 4: Zlepšení zpráv neplatné přihlašovací údaje přihlášení ovládacího prvku

Když se uživatel pokusí přihlásit s neplatnými přihlašovacími údaji, ovládací prvek pro přihlášení zobrazí zprávu, která vysvětluje, že pokus o přihlášení nebylo úspěšné. Konkrétně tento ovládací prvek zobrazí zprávu určenou jeho [ `FailureText` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), který má výchozí hodnotu pokus o přihlášení nebylo úspěšné. Zkuste to prosím znovu.

Odvolat, že existuje mnoho důvodů, proč přihlašovacích údajů uživatele je neplatný:

- Uživatelské jméno neexistuje.
- Uživatelské jméno existuje, ale heslo je neplatné
- Uživatelské jméno a heslo jsou platné, ale uživatel není ještě schváleny
- Uživatelské jméno a heslo jsou platné, ale uživatel je uzamčen (pravděpodobně vzhledem k tomu, že byl překročen počet neplatných pokusů o přihlášení v rámci zadaného časového rámce) na více systémů

A je možné z jiných důvodů při použití vlastní ověřovací logiku. Například kód jsme napsali v kroku 3, uživatelské jméno a heslo může být platný, ale e-mailová adresa může být nesprávný.

Bez ohledu na to, proč jsou neplatné přihlašovací údaje zobrazí ovládací prvek pro přihlášení ke stejné chybové zprávě. Může to být matoucí pro uživatele, jehož účet ještě nebyl schválen nebo který byl uzamčen kvůli chybějící zpětnou vazbu. S trocha pracovní jsme ale mít přihlášení řízení zobrazí zprávu vhodnější.

Vždy, když se uživatel pokusí přihlásit s neplatnými přihlašovacími údaji vyvolá ovládací prvek pro přihlášení jeho `LoginError` událostí. Pokračujte a vytvoření obslužné rutiny události pro tuto událost a přidejte následující kód:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample5.vb)]

Výše uvedený kód spustí nastavením prvek přihlášení `FailureText` vlastnost na výchozí hodnotu (pokus o přihlášení nebylo úspěšné. Zkuste to prosím znovu). Pak zkontroluje, pokud uživatelské jméno mapuje existující uživatelský účet. Pokud ano, zajímají výsledná `MembershipUser` objektu `IsLockedOut` a `IsApproved` vlastnosti, které chcete zjistit, zda účet byl uzamčen či ještě nebyla schválena. V obou případech `FailureText` vlastnost se aktualizuje na odpovídající hodnotu.

K otestování tohoto kódu, záměrně pokuste se přihlásit jako stávajícího uživatele, ale použijte nesprávné heslo. Proveďte tuto pětkrát v řádku v časovém rámci 10 minut a účet uzamčen. Jak ukazuje obrázek 11, přihlášení následné pokusy o bude vždy selhat (i s správné heslo), ale bude nyní zobrazit více popisný byl váš účet uzamčen kvůli moc velký počet neplatných pokusů o přihlášení. Obraťte se na správce, aby zprávu odemknout účet.


[![Tito provést moc velký počet neplatných pokusů o přihlášení a byl uzamčen](validating-user-credentials-against-the-membership-user-store-vb/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image31.png)

**Obrázek 11**: Tito provést příliš mnoho neplatný pokusů o přihlášení a má byl uzamčen Out ([Kliknutím zobrazit obrázek v plné velikosti](validating-user-credentials-against-the-membership-user-store-vb/_static/image33.png))


## <a name="summary"></a>Souhrn

Předchozí tohoto kurzu, zadané přihlašovací údaje s pevně zakódovaným seznamu párů uživatelského jména a hesla ověřit naše přihlašovací stránku. V tomto kurzu budeme aktualizovat stránku k ověření pověření proti rozhraní členství. V kroku 1 jsme se podívali na pomocí `Membership.ValidateUser` metoda prostřednictvím kódu programu. V kroku 2 bylo nahrazeno naše ručně vytvořené uživatelské rozhraní a kódu ovládací prvek pro přihlášení.

Ovládací prvek pro přihlášení vykreslí přihlášení standardní uživatelské rozhraní a automaticky ověřuje pověření uživatele proti rozhraní členství. Kromě toho v případě platné přihlašovací údaje, ovládací prvek pro přihlášení se přihlásí uživatele prostřednictvím ověřování pomocí formulářů. Plně funkční přihlašování uživatele je stručně řečeno, k dispozici jednoduše přetažením ovládací prvek pro přihlášení na stránce, žádné velmi deklarativní nebo kódu, které jsou nezbytné. Co je další, je vysoce přizpůsobitelné, aby vám umožnil jemné stupeň kontroly nad obou vykreslené uživatelské rozhraní a ověřování logiku ovládací prvek pro přihlášení.

V tomto okamžiku můžete návštěvníkům náš web vytvořit nový uživatelský účet a protokol do lokality, ale musíme ještě podívejte se na omezení přístupu k stránky na základě ověření uživatele. Každý uživatel ověřený nebo anonymní, v současné době můžete zobrazení všech stránek na svém webovém serveru. Společně s řízení přístupu na stránky náš web na základě uživatele podle uživatelů, může máme určité stránky, jejichž funkce závisí na uživatele. V dalším kurzu vypadá na tom, jak omezit přístup a funkce v stránku na základě přihlášeného uživatele.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Zobrazení vlastních zpráv, které uzamčen a neschválený uživatelů](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Zkoumání ASP.NET 2.0 je členství, role a profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Postupy: Vytvoření přihlašovací stránky technologie ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Technická dokumentace přihlášení ovládací prvek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Použití ovládacích prvků přihlášení](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Teresy Murphy a Michael Olivero. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Předchozí](creating-user-accounts-vb.md)
[další](user-based-authorization-vb.md)
