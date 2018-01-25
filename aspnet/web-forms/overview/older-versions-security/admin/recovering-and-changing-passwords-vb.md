---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: "Obnovení a změna hesel (VB) | Microsoft Docs"
author: rick-anderson
description: "Technologie ASP.NET obsahuje dva ovládací prvky webového asistence s obnovením a změny hesla. Ovládací prvek PasswordRecovery umožňuje návštěvníka k obnovení jeho ztraceny pa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: b78469858483a9501a0f73d1c894e29ae0a99122
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="recovering-and-changing-passwords-vb"></a>Obnovení a změna hesel (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> Technologie ASP.NET obsahuje dva ovládací prvky webového asistence s obnovením a změny hesla. Ovládací prvek PasswordRecovery umožňuje návštěvníka k obnovení jeho hesla. ztraceny. Změna hesla byla řízení umožňuje uživatelům aktualizovat heslo. Jako další ovládací prvky související s přihlášením webové jsme viděli v rámci tohoto kurzu řady PasswordRecovery a změna hesla byla řídí pracují s rozhraní členství na pozadí a resetovat nebo změnit hesla uživatelů.


## <a name="introduction"></a>Úvod

Mezi weby pro moje bank, nástroj společnosti, phone společnosti, e-mailové účty a přizpůsobené webové portály, většina lidí, je nutné desítek různá hesla pamatovat. S pověřením mnoho pamatovat dní je běžné pro uživatele, kteří mají zapomenou své heslo. Pro tento účet, weby, které nabízejí uživatelské účty muset měl mít uživatel k obnovení jeho hesla. Tento proces obvykle zahrnuje generování nové, náhodné heslo a e-mailem žádají e-mailovou adresu uživatele v souboru. Většina uživatelů po přijetí své nové heslo vrátit do lokality a změnit si heslo z náhodně generované jeden více snadno zapamatovatelný klíčem.

Technologie ASP.NET obsahuje dva ovládací prvky webového asistence s obnovením a změny hesla. Ovládací prvek PasswordRecovery umožňuje návštěvníka k obnovení jeho hesla. ztraceny. Změna hesla byla řízení umožňuje uživatelům aktualizovat heslo. Jako další ovládací prvky související s přihlášením webové jsme viděli v rámci tohoto kurzu řady PasswordRecovery a změna hesla byla řídí pracují s rozhraní členství na pozadí a resetovat nebo změnit hesla uživatelů.

V tomto kurzu vyzkoušíme pomocí těchto dvou ovládacích prvků. Také jsme uvidí jak programově změnit a resetovat heslo uživatele pomocí `MembershipUser` třídy `ChangePassword` a `ResetPassword` metody.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Krok 1: Pomoc uživatelům obnovení ztraceného hesla

Všechny weby, které podporují uživatelské účty nutné uživatelům poskytnout stejný mechanismus pro obnovení jejich zapomenutým heslům. Dobrá zpráva je, že implementace takové funkce technologie ASP.NET není uloženy díky ovládací prvek PasswordRecovery webu. Ovládací prvek PasswordRecovery vykreslí rozhraní, které zobrazí výzvu pro své uživatelské jméno a v případě potřeby odpovědí na své bezpečnostní otázku. Ho e-maily pak uživatel své heslo.

> [!NOTE]
> Protože e-mailové zprávy jsou přenášena ve formátu prostého textu existují zabezpečení rizika spojená s odesíláním heslo uživatele e-mailem.


Ovládací prvek PasswordRecovery se skládá ze tří zobrazení:

- **Uživatelské jméno** -vyzve návštěvníka pro své uživatelské jméno. Toto je počáteční zobrazení.
- **Otázka**-zobrazí uživatelské jméno a bezpečnostní otázku uživatele textu, společně s textové pole pro uživatele k zadání odpovědi na své bezpečnostní otázku.
- **Úspěch**-zobrazí zprávu informující uživatele, který heslo má byla e-mailem.

Zobrazí se zobrazení a akce prováděné ovládacího prvku PasswordRecovery závisí na následujících nastavení konfigurace členství:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Rozhraní framework členství `RequiresQuestionAndAnswer` nastavení určuje, jestli uživatelé musí zadat bezpečnostní otázku a odpověď při registraci k účtu. Jak již bylo zmíněno <a id="_msoanchor_1"> </a> [ *vytváření uživatelských účtů* ](../membership/creating-user-accounts-vb.md) kurz, pokud `RequiresQuestionAndAnswer` má hodnotu True (výchozí), pak CreateUserWizard rozhraní obsahuje textové pole ovládací prvky pro nového uživatele bezpečnostní otázku a odpověď; Pokud `RequiresQuestionAndAnswer` hodnotu False, nebudou tyto údaje. Podobně pokud `RequiresQuestionAndAnswer` je True a potom zobrazí ovládací prvek PasswordRecovery, otázka zobrazit, co uživatel zadá své uživatelské jméno, heslo je obnovena pouze v případě, že uživatel zadá správný bezpečnostní otázce. Pokud `RequiresQuestionAndAnswer` je False, ale ovládací prvek PasswordRecovery přesune přímo ze zobrazení uživatelské jméno k zobrazení úspěch.

Poté, co uživatel zadal své uživatelské jméno - nebo jeho uživatelské jméno a zabezpečení odpovědí, pokud `RequiresQuestionAndAnswer` má hodnotu True - PasswordRecovery e-maily uživatele heslo. Pokud `EnablePasswordRetrieval` je možnost nastavena na hodnotu True, pak uživatel e-mailu jejich aktuální heslo. Pokud je nastaveno na hodnotu False a `EnablePasswordReset` nastaven na hodnotu True, pak ovládacího prvku PasswordRecovery vygeneruje nové, náhodné heslo pro uživatele a e-maily tohoto nového hesla k nim. Pokud oba `EnablePasswordRetrieval` a `EnablePasswordReset` hodnotu False, ovládacího prvku PasswordRecovery vyvolá výjimku.

> [!NOTE]
> Odvolat, který `SqlMembershipProvider` ukládá hesla uživatelů ke službám v jednom ze tří formátů: zaškrtnutí, Hashed (výchozí) nebo zašifrovaná. Používáno úložiště závisí na nastavení konfigurace členství. ukázkové aplikace používá formát hesla Hashed. Při použití formátu heslo Hashed `EnablePasswordRetrieval` možnost musí být nastaveno na hodnotu False, protože systém nemůže určit vlastní heslo uživatele z hodnoty hash verze uložená v databázi.


Obrázek 1 ukazuje, jak je konfigurace členství vliv PasswordRecovery rozhraní a chování.


[![RequiresQuestionAndAnswer, EnablePasswordRetrieval a EnablePasswordReset ovlivnit vzhled a chování PasswordRecovery ovládacího prvku](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**Obrázek 1**: `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, a `EnablePasswordReset` ovlivnit vzhled a chování ovládacího prvku PasswordRecovery ([Kliknutím zobrazit obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image3.png))


> [!NOTE]
> V <a id="_msoanchor_2"> </a> [ *vytváření schématu členství v systému SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) kurzu jsme nakonfigurovali zprostředkovatel členství nastavením `RequiresQuestionAndAnswer` na hodnotu True, `EnablePasswordRetrieval` na Hodnotu false, a `EnablePasswordReset` na hodnotu True.


### <a name="using-the-passwordrecovery-control"></a>Použití ovládacího prvku PasswordRecovery

Podívejme se na stránku ASP.NET pomocí ovládacího prvku PasswordRecovery. Otevřete `RecoverPassword.aspx` a přetáhněte ji a vyřadit ovládacího prvku PasswordRecovery z panelu nástrojů na návrháře; nastavit jeho `ID` k `RecoverPwd`. Jako přihlašovací údaje a CreateUserWizard webových ovládacích prvků vykreslit zobrazení ovládacích prvků PasswordRecovery bohaté složené rozhraní, které obsahuje popisky, textových polí, tlačítek a ovládací prvky pro ověřování. Můžete přizpůsobit vzhled zobrazení prostřednictvím vlastnosti stylu ovládacího prvku nebo převedením zobrazení na šablony. Nechat to jako cvičení pro zúčastněným čtečku.

Pokud uživatel navštíví Tato stránka se bude zadejte svoje uživatelské jméno a klikněte na tlačítko pro odeslání. Protože jsme nastavili `RequiresQuestionAndAnswer` vlastnost na hodnotu True v nastavení konfigurace naše členství, PasswordRecovery řízení se pak zobrazit otázku. Poté, co uživatel zadá svůj odpovědí správné zabezpečení a na odeslání, bude prvek PasswordRecovery aktualizovat jeden se náhodně vygenerované heslo uživatele a e-mailu toto heslo e-mailovou adresu na soubor. Všechny tyto bylo možné bez nám nutnosti napsat jediný řádek kódu!

Před zahájením testovacího této stránce, je poslední část konfigurace do mají tendenci: je potřeba zadat nastavení doručení e-mailu v `Web.config`. Ovládací prvek PasswordRecovery závisí na těchto nastavení pro odesílání e-mailu.

Konfigurace doručování e-mailu se specifikuje prostřednictvím [ `<system.net>` element](https://msdn.microsoft.com/library/6484zdc1.aspx)na [ `<mailSettings>` element](https://msdn.microsoft.com/library/w355a94k.aspx). Použití [ `<smtp>` element](https://msdn.microsoft.com/library/ms164240.aspx) udávajících metodu doručení a ve výchozím nastavení z adresy. Následující kód konfiguruje nastavení e-mailu, síti SMTP serveru s názvem `smtp.example.com` na port 25 a s uživatelským jménem a heslem přihlašovací údaje uživatelského jména a hesla.

> [!NOTE]
> `<system.net>`je podřízený prvek kořenového `<configuration>` elementu a na stejné úrovni jako `<system.web>`. Proto nevkládejte `<system.net>` v rámci `<system.web>` element; namísto toho uložte na stejné úrovni.


[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

Kromě používání SMTP server v síti, můžete alternativně zadat výstupního adresáře, kde má e-mailové zprávy zasílat uloží.

Po konfiguraci nastavení SMTP, přejděte `RecoverPassword.aspx` stránku prostřednictvím prohlížeče. Zkuste je napřed zadáním uživatelského jména, která neexistuje v úložišti uživatele. Jak je vidět na obrázku 2, ovládacího prvku PasswordRecovery zobrazí zprávu s upozorněním, že informace o uživateli nelze získat přístup. Text zprávy lze přizpůsobit prostřednictvím ovládacího prvku [ `UserNameFailureText` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Pokud je zadáno neplatné uživatelské jméno se zobrazí chybová zpráva](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**Obrázek 2**: chybová zpráva se zobrazí, pokud je zadáno neplatné uživatelské jméno ([Kliknutím zobrazit obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image6.png))


Nyní zadejte uživatelské jméno. Použití uživatelské jméno účtu v systému s e-mailovou adresu, můžete přístup a jejichž zabezpečení zodpovědět vám vědět. Po zadání uživatelského jména a kliknutí na tlačítko Odeslat, zobrazí ovládací prvek PasswordRecovery jeho zobrazení otázku. Jako s zobrazení uživatelské jméno, pokud zadáte nesprávné odpovězte PasswordRecovery ovládací prvek zobrazí chybové zprávy (viz obrázek 3). Použití [ `QuestionFailureText` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) přizpůsobit tato chybová zpráva.


[![Pokud uživatel zadá neplatná odpověď zabezpečení, zobrazí se chybová zpráva](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**Obrázek 3**: chybová zpráva se zobrazí, pokud uživatel zadá neplatná odpověď zabezpečení ([Kliknutím zobrazit obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image9.png))


Nakonec zadejte správné zabezpečení odpovědí a klikněte na tlačítko Odeslat. Na pozadí ovládacího prvku PasswordRecovery generuje náhodné heslo, přiřadí k uživatelskému účtu, odešle e-mail informující uživatele své nové heslo (viz obrázek 4) a potom zobrazí zobrazení informací o úspěchu.


[![Uživatel je odeslán E-mail s His nové heslo](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**Obrázek 4**: uživatele je odeslán E-mail s His nové heslo ([Kliknutím zobrazit obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image12.png))


### <a name="customizing-the-email"></a>Přizpůsobení e-mailu

Výchozí e-mailu pomocí ovládacího prvku PasswordRecovery je spíš matné (viz obrázek 4). Je zpráva odeslána z účet zadaný v `<smtp>` elementu `from` atribut s předmětem heslo a text ve formátu prostého textu:

Vraťte se k webu a přihlaste se pomocí následujících informací.

Uživatelské jméno: *uživatelské jméno*

heslo: *heslo*

Tuto zprávu lze přizpůsobit prostřednictvím kódu programu prostřednictvím obslužné rutiny události pro ovládací prvek PasswordRecovery [ `SendingMail` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), nebo deklarativně pomocí [ `MailDefinition` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Podíváme se na obě tyto možnosti.

`SendingMail` Událost je aktivována například právo předtím, než se odesílají e-mailové zprávy a je naše poslední možnost, aby prostřednictvím kódu programu upravit e-mailové zprávy. Pokud tato událost je aktivována, obslužné rutiny události je předán objekt typu [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), jejichž `Message` vlastnost obsahuje odkaz na e-mailu odeslány.

Vytvoření obslužné rutiny událostí `SendingMail` událostí a přidejte následující kód, který přidá prostřednictvím kódu programu `webmaster@example.com` do seznamu kopie.

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

E-mailové zprávě můžete nakonfigurovat taky přes deklarativní způsobem. PasswordRecovery `MailDefinition` vlastnost je objekt typu [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). `MailDefinition` Třída nabízí hostitel vlastnosti související s e-mailu, včetně `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`a další. Pro začátek se nastavit [ `Subject` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) něco srozumitelnější než ten, který je ve výchozím nastavení (heslo), používá, jako je vaše heslo bylo resetováno...

Chcete-li přizpůsobit text e-mailovou zprávu, kterou je potřeba vytvořit soubor samostatné e-mailové šablony, která obsahuje obsah textu. Začněte vytvořením novou složku na webu s názvem `EmailTemplates`. Dál přidejte nový textový soubor do této složky s názvem `PasswordRecovery.txt` a přidejte následující obsah:

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

Všimněte si použití zástupné symboly `<%UserName%>` a `<%Password%>`. Ovládací prvek PasswordRecovery automaticky uživatelské jméno a heslo obnovené před odesláním e-mailu uživatele nahradí tyto dva zástupné znaky.

A konečně, přejděte `MailDefinition`na [ `BodyFileName` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) k e-mailovou šablonu, kterou jsme právě vytvořili (`~/EmailTemplates/PasswordRecovery.txt`).

Po provedení těchto změn pokroku `RecoverPassword.aspx` stránky a zadejte vaše uživatelské jméno a zabezpečení odpovědí. Obdržíte e-mailu, který vypadá jako jeden z obrázku 5, by měl. Všimněte si, že `webmaster@example.com` bylo by kopie a zda byly aktualizovány předmět a text.


[![Byly aktualizovány předmět, text a seznamu kopie](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**Obrázek 5**: předmětu, textu a byly aktualizovány kopie seznamu ([Kliknutím zobrazit obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image15.png))


Odeslat e-mail ve formátu HTML nastavit [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) na hodnotu True (výchozí hodnota je False) a aktualizace šablony e-mailu, zahrnout HTML.

`MailDefinition` Vlastnost není pro třídu PasswordRecovery jedinečné. Jak jsme uvidí v kroku 2, změna hesla byla ovládací prvek také nabízí `MailDefinition` vlastnost. Kromě toho zahrnuje ovládacího prvku CreateUserWizard taková vlastnost lze nastavit automaticky odeslat zprávu Uvítacího e-mailu pro nové uživatele.

> [!NOTE]
> Aktuálně neexistují žádné odkazy v levém navigačním panelu pro dosažení `RecoverPassword.aspx` stránky. Uživatel by zájmu pouze v návštěvě této stránky, pokud uživatel se nepodařilo úspěšně Přihlaste se k webu. Proto aktualizovat `Login.aspx` stránky zahrnout odkaz `RecoverPassword.aspx` stránky.


### <a name="programmatically-resetting-a-users-password"></a>Prostřednictvím kódu programu resetuje heslo uživatele

Při resetování hesla uživatele PasswordRecovery řízení volání `MembershipUser` objektu [ `ResetPassword` metoda](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Tato metoda má dva přetížení:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)**-Resetuje heslo uživatele. Toto přetížení použijte, pokud `RequiresQuestionAndAnswer` je False.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)**-Obnoví jenom Pokud heslo uživatele zadaných *securityAnswer* je správný. Toto přetížení použijte, pokud `RequiresQuestionAndAnswer` má hodnotu True.

Obě přetížení vrátí nové, náhodně vygenerované heslo.

S jinými metodami v rozhraní framework členství, jako `ResetPassword` metoda delegátů k nakonfigurovaného zprostředkovatele. `SqlMembershipProvider` Vyvolá `aspnet_Membership_ResetPassword` předávání uložené procedury, uživatelské jméno, nové heslo a odpověď zadané heslo, mezi další pole. Uložená procedura zajišťuje, že odpověď hesla odpovídá a pak aktualizuje heslo uživatele.

Několik poznámky k implementaci nižší úrovně:

- Neuzamčení uživatele nelze resetovat svoje heslo. Však může neschválených uživatele. Jsme se zabývat uzamčené a schválení stavů v podrobněji <a id="_msoanchor_3"> </a> [ *Unlocking a schválení uživatelem* ](unlocking-and-approving-user-accounts-vb.md) kurzu účty.
- Pokud odpověď pro heslo je nesprávné, se zvýší počet pokusů o odpověď hesla se nezdařila uživatele. Dojde-li zadaný počet pokusů o zabezpečení neplatná odpověď v rámci určeného časového období, uživatel je uzamčen.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Word na tom, jak náhodných hesla jsou generovány.

Náhodně generovaný hesla v e-mailové zprávy v útvarů 4 a 5 jsou vytvořené pomocí třídy členství [ `GeneratePassword` metoda](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Tato metoda přijímá dva vstupní datového celé číslo - *délka* a *numberOfNonAlphanumericCharacters* - a vrátí řetězec alespoň *délka* znaků dlouhé a obsahují v nejméně *numberOfNonAlphanumericCharacters* počet jiných než alfanumerických znaků. Když tato metoda je volána z v rámci třídy členství nebo ovládací prvky webového související s přihlášením, hodnoty pro tyto dva parametry jsou určeny konfigurace členství `MinRequiredPasswordLength` a `MinRequiredNonalphanumericCharacters` vlastnosti, které se nastaví na 7 a 1, v uvedeném pořadí.

`GeneratePassword` Metoda používá k zajištění, že je v jaké náhodných znaků nejsou vybrány žádné odchylka kryptograficky silnou generátoru náhodných čísel. Kromě toho `GeneratePassword` je `Public`, což znamená, můžete ho přímo z vaší aplikace ASP.NET Pokud je potřeba generovat řetězců náhodných nebo hesla.

> [!NOTE]
> `SqlMembershipProvider` Třída vždy vygeneruje náhodné heslo aspoň 14 znaků, takže když `MinRequiredPasswordLength` je menší než 14 a její hodnota je ignorována.


## <a name="step-2-changing-passwords"></a>Krok 2: Změna hesla

Hesla náhodně generované obtížně mějte na paměti. Vezměte v úvahu heslo ukazuje obrázek 4: `WWGUZv(f2yM:Bd`. Akci, která potvrzení paměti! Needless znamená po odeslání náhodně vygenerované heslo tohoto uživatele se budete chtít změnit heslo zapamatovatelný.

Použití ovládacího prvku Změna hesla byla k vytvoření rozhraní pro uživatele Chcete-li změnit své heslo. Mnohem jako prvek PasswordRecovery ovládacího prvku Změna hesla byla se skládá ze dvou zobrazení: Změna hesla a úspěch. Zobrazení změnit heslo vyzve uživatele k jejich staré a nové heslo. Při zadávání správné staré heslo a nové heslo, které splňuje minimální délku a požadavky na jiných než alfanumerických znaků, ovládacího prvku Změna hesla byla aktualizace hesla a zobrazení úspěch.

> [!NOTE]
> Změna hesla byla řízení změní heslo uživatele vyvoláním `MembershipUser` objektu [ `ChangePassword` metoda](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). Změna hesla byla metoda přijímá dva `String` vstupní parametry - *Původní_heslo* a *nové_heslo*- a aktualizuje uživatelský účet s *nové_heslo*, za předpokladu, že zadaných *Původní_heslo* je správný.


Otevřete `ChangePassword.aspx` stránky a přidání ovládacího prvku Změna hesla byla na stránku pojmenování ho `ChangePwd`. V tomto okamžiku by měl zobrazení návrhu zobrazení změnit heslo zobrazení (viz obrázek 6). Jako pomocí ovládacího prvku PasswordRecovery můžete přepínat mezi zobrazeními pomocí inteligentních značek ovládacího prvku. Kromě toho tato zobrazení objevuje se přizpůsobitelné prostřednictvím vlastnosti různé stylu nebo převedením na šablonu.


[![Přidání ovládacího prvku Změna hesla byla na stránku](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**Obrázek 6**: Přidání ovládacího prvku Změna hesla byla na stránku ([Kliknutím zobrazit obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image18.png))


Změna hesla byla řízení můžete aktualizovat heslo aktuálně přihlášeného uživatele *nebo* hesla jiný, zadaného uživatele. Jak je vidět na obrázku 6, výchozí zobrazení změnit heslo vykreslí jenom tři ovládacích prvcích TextBox: jeden pro staré heslo a dva jsou pro nové heslo. Toto rozhraní výchozí se používá k aktualizaci hesla aktuálně přihlášeného uživatele.

Použití ovládacího prvku Změna hesla byla aktualizace hesla jiného uživatele, nastavte ovládacího prvku [ `DisplayUserName` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) na hodnotu True. Díky tomu přidá čtvrtý textové pole na stránku, dotaz na uživatelské jméno uživatele, jehož heslo chcete změnit.

Nastavení `DisplayUserName` na True je užitečné, pokud chcete, aby mohl zaznamenané si uživatel změnit svoje heslo bez nutnosti k přihlášení. Osobně je pravděpodobné, že není nic se nutnosti uživateli přihlášení před povolením jí chcete-li změnit své heslo. Proto nechte `DisplayUserName` nastavena na hodnotu False (výchozí). Při vytvoření toto rozhodnutí, ale jsme v podstatě se blokování anonymní uživatelé z dosažení této stránky. Aktualizovat autorizačních pravidel adres URL lokality tak, aby anonymní uživatelé z navštívíte Odepřít `ChangePassword.aspx`. Pokud bude nutné aktualizovat vaše paměti v syntaxi adresy URL autorizační pravidlo, přečtěte si téma zpět <a id="_msoanchor_4"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-vb.md) kurzu.

> [!NOTE]
> To nemusí připadat, který `DisplayUserName` vlastnost je užitečná pro umožňují tak správcům změnu hesla jiných uživatelů. Ale i v případě `DisplayUserName` nastaven na hodnotu True, musí být známé a zadali správné staré heslo. Se věnuje techniky pro umožňují tak správcům změnit hesla uživatelů ke službám v kroku 3.


Přejděte `ChangePassword.aspx` stránky prostřednictvím prohlížeče a změňte si heslo. Všimněte si, že pokud zadáte nové heslo, které nesplňuje délku hesla a požadavky na jiných než alfanumerických znaků zadaný v konfiguraci členství, zobrazí se chybová zpráva (viz obrázek 7).


[![Přidání ovládacího prvku Změna hesla byla na stránku](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**Obrázek 7**: Přidání ovládacího prvku Změna hesla byla na stránku ([Kliknutím zobrazit obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image21.png))


Po zadání správné staré heslo a platný nové heslo, přihlášeného na uživatele změnit heslo a zobrazí úspěch.

### <a name="sending-a-confirmation-email"></a>Odesílání e-mailu s potvrzením

Ve výchozím nastavení Změna hesla byla řízení neodešle e-mailovou zprávu uživatele, jejichž hesla byla právě aktualizována. Pokud chcete odeslat e-mail, jednoduše nakonfigurovat ovládacího prvku `MailDefinition` vlastnost. Umožňuje nakonfigurovat Změna hesla byla ovládacího prvku tak, aby uživatel je odeslán e-mail ve formátu HTML, který obsahuje své nové heslo.

Začněte vytvořením nového souboru v `EmailTemplates` složku s názvem `ChangePassword.htm`. Přidejte následující kód:

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

Dále nastavte Změna hesla byla ovládacího prvku `MailDefinition` vlastnosti `BodyFileName`, `IsBodyHtml`, a `Subject` vlastnosti, které chcete ~ / EmailTemplates/ChangePassword.htm, True a heslo změnil!, v uvedeném pořadí.

Po provedení těchto změn, otevírat stránku a změňte si heslo znovu. Tentokrát ovládací prvek Změna hesla byla odešle přizpůsobené, ve formátu HTML e-mailu na e-mailovou adresu uživatele v souboru (viz obrázek 8).


[![E-mailová zpráva s informací, že jejich heslo uživatele byl změněn](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**Obrázek 8**: e-mailová zpráva s informací, že jejich heslo uživatele změnil ([Kliknutím zobrazit obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Krok 3: Povolení správcům změnit hesla uživatelů

Běžné funkce v aplikacích, které podporují uživatelské účty je schopnost správce o změnu hesla jiných uživatelů. Někdy se tato funkce vyžaduje, protože systém nemá schopnost změny hesla pro uživatele. V takovém případě by jedině uživatel obnovit zapomenuté heslo správce je přiřadit nové heslo. S ovládacími prvky PasswordRecovery a změna hesla byla však administrativní uživatelé nemusí zaneprázdněných sami s změna hesel uživatelů, uživatelé mohou díky této sami.

Ale co když váš klient insists, že administrativní uživatelé by měli být moci změnit hesla jiných uživatelů? Bohužel přidání této funkce lze bit práce. Chcete-li změnit heslo uživatele, musí být poskytnuty staré a nové heslo `MembershipUser` objektu `ChangePassword` metoda, ale správce by neměl mít znát heslo uživatele, aby ji upravit.

Jeden alternativní řešení, je nejprve obnovit heslo uživatele a pak ji změňte nové heslo pomocí kódu podobně jako tento:

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

Tento kód začíná načítání informací o *uživatelské jméno*, což je uživatele, jehož heslo správce chce změnit. Dále `ResetPassword` metoda je volána, který přiřazuje a nové, náhodné heslo uživatele. Toto náhodně vygenerované heslo je vrácená metodou a uložené v proměnné `resetPwd`. Teď, když budeme vědět, za který uživatelské heslo, můžeme ho změnit prostřednictvím volání `ChangePassword`.

Problém je, že tento kód funguje jenom v případě konfigurace systému členství je nastaven tak, aby `RequiresQuestionAndAnswer` je False. Pokud `RequiresQuestionAndAnswer` má hodnotu True, jako je v naší aplikaci, pak se `ResetPassword` metoda musí být předán bezpečnostní otázce, jinak vyvolá výjimku.

Pokud rozhraní členství je nakonfigurován tak, aby vyžadovala bezpečnostní otázku a odpověď, a ještě vašeho klienta insists, aby správci mohli změnit hesla uživatelů ke službám, máte tři možnosti:

- Throw rukou v vzduchem a váš klient říct, že toto je pouze jeden z věcí, které nelze provést.
- Nastavit `RequiresQuestionAndAnswer` má hodnotu False. Výsledkem je méně zabezpečené aplikace. Představte si, že nefarious uživatel získal přístup k e-mailovou schránku jiného uživatele. Možná ohroženými uživatelskými opustil svého stolu přejít na oběd a nebyla uzamčení pracovní stanici, nebo možná získat přístup k e-mailu z veřejného terminálu a nebylo odhlásit. V obou případech můžete navštívit nefarious uživatele `RecoverPassword.aspx` stránky a zadejte uživatelské jméno. Systém se pak e-mailu obnovené heslo bez výzvy pro bezpečnostní otázce.
- Nepoužívat abstraktní vrstvu vytvořené framework členství a pracovat přímo s databázi systému SQL Server. Schéma členství zahrnuje uložené procedury s názvem `aspnet_Membership_SetPassword` , nastaví heslo uživatele a aby bylo možné úkolu nevyžaduje bezpečnostní otázce ani staré heslo.

Žádnou z těchto možností jsou zvláště přitažlivé, ale který je, jak se někdy přejde vývojáře.

I se dopředu a implementovat třetí přístup psaní kódu, který obchází `Membership` a `MembershipUser` třídy a pracuje přímo na `SecurityTutorials` databáze.

> [!NOTE]
> Pracovat přímo s databází, je shattered zapouzdření poskytované rozhraní členství. Toto rozhodnutí sváže nám `SqlMembershipProvider`, provádění kódu méně přenositelností. Kromě toho tento kód nemusí fungovat podle očekávání v budoucích verzích technologie ASP.NET, pokud se změní členství ve schématu. Tento přístup je alternativní řešení, a jako většinu řešení není příkladem osvědčené postupy.


Kód obsahuje některé body bits a je velmi náročná. Proto nechcete zbytečných souborů v tomto kurzu se hloubkové posouzení ho. Pokud vás zajímá v dalších informací, stáhněte si kód pro tento kurz a najdete `~/Administration/ManageUsers.aspx` stránky. Tuto stránku, kterou jsme vytvořili v <a id="_msoanchor_5"> </a> [předchozí kurzu](building-an-interface-to-select-one-user-account-from-many-vb.md), obsahuje seznam jednotlivých uživatelů. Aktualizovali GridView zahrnout odkaz `UserInformation.aspx` stránky, předávání uživatelské jméno vybraného uživatele prostřednictvím řetězec dotazu. `UserInformation.aspx` Stránky zobrazí informace o vybraný uživatel a textová pole pro změnu hesla (viz obrázek 9).

Po zadání nové heslo, potvrzení do textového pole druhý a kliknutím na tlačítko Aktualizovat uživatele, zpětné volání vyplývá a `aspnet_Membership_SetPassword` vyvolat uloženou proceduru aktualizace heslo uživatele. Doporučte I ty čtečky zájem o tuto funkci seznámení s kód a zkuste to rozšíření funkcí, které chcete zahrnout, odesílání e-mailu pro uživatele, jehož heslo bylo změněno.


[![Správce může změnit heslo uživatele](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**Obrázek 9**: Správce může změnit heslo uživatele ([Kliknutím zobrazit obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image27.png))


> [!NOTE]
> `UserInformation.aspx` Stránce aktuálně funguje pouze, pokud rozhraní členství je nakonfigurovaná pro ukládání hesel ve formátu Clear nebo Hashed. Chybí však kód k šifrování nové heslo, i když se zobrazí pozvánka k přidat tuto funkci. Způsob I doporučujeme přidání nezbytného kódu je použití decompiler jako [Reflector](http://www.aisto.com/roeder/dotnet/) pro zjištění zdrojového kódu pro metody v rozhraní .NET Framework; spustit tak, že prověří `SqlMembershipProvider` třídy `ChangePassword` metoda. Toto je technika, který byl použit pro zápis kódu pro vytvoření hodnoty hash hesla.


## <a name="summary"></a>Souhrn

Technologie ASP.NET nabízí dvou ovládacích prvků, aby pomohla uživatelům spravovat své heslo. Ovládací prvek PasswordRecovery je užitečné pro ty, kteří zapomenou své heslo. V závislosti na konfiguraci framework členství uživatel je buď e-mailem hesla pro existující nebo nové, náhodně vygenerované heslo. Změna hesla byla ovládací prvek umožňuje uživatelům aktualizovat své heslo.

Jako přihlašovací údaje a CreateUserWizard ovládacích prvků vykreslení ovládacích prvků PasswordRecovery a změna hesla byla bohaté uživatelské rozhraní bez nutnosti psaní ikonu deklarativní nebo řádek kódu. Pokud výchozí uživatelské rozhraní nevyhovuje vašim potřebám, můžete přizpůsobit přes různé vlastnosti stylu. Ovládací prvky rozhraní Alternativně může být převedeny na šablony pro i podrobnějšího ovládání. Na pozadí tyto ovládací prvky použít rozhraní API členství vyvolání `MembershipUser` objektu `ResetPassword` a `ChangePassword` metody.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Změna hesla byla řízení – elementy QuickStart](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Ovládací prvek PasswordRecovery – elementy QuickStart](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Odesílání e-mailu v technologii ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail`Nejčastější dotazy](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu zahrnují Michael Emmings a Suchi Banerjee. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](building-an-interface-to-select-one-user-account-from-many-vb.md)
[další](unlocking-and-approving-user-accounts-vb.md)
