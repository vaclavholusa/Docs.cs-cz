---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: Obnovení a změna hesel (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Technologie ASP.NET obsahuje dva webové ovládací prvky pro pomoc s obnovením a změna hesel. Ovládací prvek PasswordRecovery umožňuje návštěvník k obnovení jeho ztráty pa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: 68b70708904a46e31639331bbfd0dd31240ea421
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393993"
---
<a name="recovering-and-changing-passwords-vb"></a>Obnovení a změna hesel (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> Technologie ASP.NET obsahuje dva webové ovládací prvky pro pomoc s obnovením a změna hesel. Ovládací prvek PasswordRecovery umožňuje návštěvník k obnovení jeho hesla ztraceny. Ovládací prvek ChangePassword umožňuje uživatelům aktualizovat své heslo. Stejně jako ostatní ovládací prvky související s přihlášením webové jsme viděli v celé této sérii, PasswordRecovery a ovládací prvky pro metodu ChangePassword pracovat s framework členství na pozadí, resetování nebo změna hesla uživatelů.


## <a name="introduction"></a>Úvod

Mezi weby pro moje bank, nástroj společnosti, telefon společnosti, e-mailové účty a přizpůsobené webové portály, jako je většina lidí, mám desítky odlišná hesla nezapomeňte. S tolika pověření do těchto dnů učit nazpaměť není, uživatelé zapomenou své heslo. Na účtu k tomu, weby, které nabízejí uživatelské účty musí obsahovat způsob pro uživatele k obnovení jeho hesla. Tento proces obvykle zahrnuje vygenerovat nový, náhodný heslo a e-mailem na e-mailovou adresu uživatele v souboru. Po obdržení nového hesla většina uživatelů vraťte se na web a změnit si heslo z náhodně generované ten, který má více snadno zapamatovatelné jeden.

Technologie ASP.NET obsahuje dva webové ovládací prvky pro pomoc s obnovením a změna hesel. Ovládací prvek PasswordRecovery umožňuje návštěvník k obnovení jeho hesla ztraceny. Ovládací prvek ChangePassword umožňuje uživatelům aktualizovat své heslo. Stejně jako ostatní ovládací prvky související s přihlášením webové jsme viděli v celé této sérii, PasswordRecovery a ovládací prvky pro metodu ChangePassword pracovat s framework členství na pozadí, resetování nebo změna hesla uživatelů.

V tomto kurzu prozkoumáme pomocí těchto dvou ovládacích prvků. Budeme se také zjistit, jak programově změnit a resetovat heslo uživatele prostřednictvím `MembershipUser` třídy `ChangePassword` a `ResetPassword` metody.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Krok 1: Pomáhá uživatelům obnovení ztraceného hesla

Všechny weby, které podporují uživatelské účty se třeba uživatelům poskytnout určitý mechanismus pro obnovení zapomenutého hesla. Dobrou zprávou je, že implementace těchto funkcí v technologii ASP.NET podrobným díky PasswordRecovery ovládací prvek je. Ovládací prvek PasswordRecovery vykreslí rozhraní, které se zobrazí výzva pro své uživatelské jméno a v případě potřeby odpovědí na své bezpečnostní otázku. To potom e-mailem uživateli své heslo.

> [!NOTE]
> Protože e-mailové zprávy při přenosu ve formátu prostého textu se bezpečnostní rizika související s odesláním heslo uživatele e-mailem.


Ovládací prvek PasswordRecovery se skládá ze tří zobrazení:

- **Uživatelské jméno** -vyzve návštěvníka pro své uživatelské jméno. Toto je počáteční zobrazení.
- **Dotaz**-zobrazí jako text spolu s textové pole pro uživatele k zadání odpovědi na své bezpečnostní otázky uživatelského jména a bezpečnostní otázku uživatele.
- **Úspěch**-zobrazí zprávu informující uživatele, který byl poslat e-mail své heslo.

Zobrazí zobrazení a akce prováděné PasswordRecovery ovládací prvek závisí na následující nastavení konfigurace členství:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Členství v rámci `RequiresQuestionAndAnswer` nastavení určuje, jestli uživatelé musí zadat zabezpečovací otázka a odpověď při registraci k účtu. Jak jsme probírali v <a id="_msoanchor_1"> </a> [ *vytváření uživatelských účtů* ](../membership/creating-user-accounts-vb.md) výukový program, pokud `RequiresQuestionAndAnswer` má hodnotu True (výchozí), pak CreateUserWizard rozhraní obsahuje textové pole ovládací prvky pro nového uživatele bezpečnostní otázky a odpovědi. Pokud `RequiresQuestionAndAnswer` má hodnotu False, neshromažďuje žádné takové informace. Podobně pokud `RequiresQuestionAndAnswer` je hodnota True, pak zobrazí ovládací prvek PasswordRecovery zobrazení na otázku, co uživatel zadá své uživatelské jméno, heslo je obnovena pouze v případě, že uživatel zadá správný bezpečnostní otázce. Pokud `RequiresQuestionAndAnswer` má hodnotu False, ale PasswordRecovery ovládacího prvku přesune přímo ze zobrazení uživatelského jména k zobrazení informací o úspěchu.

Poté, co uživatel zadal své uživatelské jméno - nebo jeho uživatelské jméno a zabezpečení odpovědí, pokud `RequiresQuestionAndAnswer` má hodnotu True – PasswordRecovery e-mailem uživateli své heslo. Pokud `EnablePasswordRetrieval` možnost nastavená na hodnotu True, pak uživatel e-mailem je jejich aktuální heslo. Pokud je nastavena na hodnotu False a `EnablePasswordReset` je nastavena na hodnotu True, pak prvek PasswordRecovery vygeneruje nový, náhodné heslo pro uživatele a e-mailem tohoto nového hesla k nim. Pokud mají oba `EnablePasswordRetrieval` a `EnablePasswordReset` jsou False, prvek PasswordRecovery vyvolá výjimku.

> [!NOTE]
> Vzpomeňte si, že `SqlMembershipProvider` ukládá hesla uživatelů v jednom ze tří formátů: Vymazat, Hashed (výchozí) nebo šifrované. Mechanismus úložiště, který se používá, závisí na nastavení konfigurace členství. ukázkové aplikace používá formát hesla Hashed. Při použití Hashed formát hesla `EnablePasswordRetrieval` možnost musí být nastavena na hodnotu False, protože systém nemůže určit skutečnou heslo uživatele z hodnoty hash verze uložené v databázi.


Obrázek 1 ukazuje, jak PasswordRecovery rozhraní a chování je ovlivněno konfigurace členství.


[![RequiresQuestionAndAnswer, EnablePasswordRetrieval a EnablePasswordReset ovlivnit vzhled a chování PasswordRecovery ovládacího prvku](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**Obrázek 1**: `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, a `EnablePasswordReset` ovlivnit vzhled a chování ovládacího prvku PasswordRecovery ([kliknutím ji zobrazíte obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image3.png))


> [!NOTE]
> V <a id="_msoanchor_2"> </a> [ *vytvoření schématu členství v SQL serveru* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) kurzu jsme nakonfigurovali tak, že nastavíte zprostředkovatele členství `RequiresQuestionAndAnswer` na hodnotu True, `EnablePasswordRetrieval` do False, a `EnablePasswordReset` na hodnotu True.


### <a name="using-the-passwordrecovery-control"></a>Použití ovládacího prvku PasswordRecovery

Podívejme se na použití ovládacího prvku PasswordRecovery na stránce ASP.NET. Otevřít `RecoverPassword.aspx` a přetáhněte a umístěte PasswordRecovery ovládacího prvku z panelu nástrojů na Návrhář; nastavit jeho `ID` k `RecoverPwd`. Stejně jako přihlašovací jméno a CreateUserWizard webové ovládací prvky zobrazení ovládacích prvků PasswordRecovery vykreslení také bohaté rozhraní složených příkazů, který obsahuje popisky, textová pole, tlačítka a validačních ovládacích prvků. Můžete přizpůsobit vzhled zobrazení prostřednictvím vlastnosti stylu ovládacího prvku nebo převedením zobrazení do šablon. Můžu ponechte toto cvičení pro dotčené čtečku.

Když uživatel navštíví tuto stránku Jana se zadají své uživatelské jméno a klikněte na tlačítko Odeslat. Protože jsme nastavili `RequiresQuestionAndAnswer` vlastnost na hodnotu True v našich členství nastavení konfigurace PasswordRecovery řídit se pak zobrazí zobrazení otázky. Poté, co uživatel zadá své správné zabezpečovací odpověď a klikne na tlačítko Odeslat, ovládací prvek PasswordRecovery se aktualizovat na některou náhodně generované heslo uživatele a pošle e-mail toto heslo e-mailovou adresu v souboru. To vše bez nám museli psát jediný řádek kódu bylo možné!

Před testováním této stránce je poslední část konfigurace mají tendenci: potřebujeme k určení nastavení doručení e-mailu v `Web.config`. Ovládací prvek PasswordRecovery závisí na nastavení pro odesílání e-mailu.

Konfigurace doručování e-mailu se specifikuje prostřednictvím [ `<system.net>` element](https://msdn.microsoft.com/library/6484zdc1.aspx)společnosti [ `<mailSettings>` element](https://msdn.microsoft.com/library/w355a94k.aspx). Použití [ `<smtp>` element](https://msdn.microsoft.com/library/ms164240.aspx) udávajících metodu doručení a ve výchozím nastavení z adresy. Následující kód konfiguruje nastavení e-mailu, síti SMTP serveru s názvem `smtp.example.com` přes port 25 a pomocí uživatelského jména a hesla pověření uživatelského jména a hesla.

> [!NOTE]
> `<system.net>` je podřízený element kořenového `<configuration>` elementu a na stejné úrovni `<system.web>`. Proto neumisťujte `<system.net>` element v rámci `<system.web>` element; místo toho umístit na stejné úrovni.


[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

Kromě použití SMTP server v síti, můžete alternativně zadat výstupní adresář, ve kterém by měl uloží e-mailové zprávy k odeslání.

Po konfiguraci nastavení SMTP, přejděte na web `RecoverPassword.aspx` stránky prostřednictvím prohlížeče. Nejprve zkuste zadat uživatelské jméno, která neexistuje v úložišti uživatele. Jak znázorňuje obrázek 2 PasswordRecovery ovládací prvek zobrazí zprávu s oznámením, že informace o uživateli není přístupný. Text zprávy je možné přizpůsobit pomocí ovládacího prvku [ `UserNameFailureText` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Pokud je zadané neplatné uživatelské jméno, zobrazí se chybová zpráva](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**Obrázek 2**: Pokud je zadané neplatné uživatelské jméno, zobrazí se chybová zpráva ([kliknutím ji zobrazíte obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image6.png))


Teď zadejte uživatelské jméno. Použijte uživatelské jméno účtu v systému s e-mailovou adresu, můžete přistupovat a jehož zabezpečení odpovědí můžete vědět. Po zadání uživatelského jména a kliknutí na tlačítko Odeslat, PasswordRecovery ovládací prvek zobrazí jeho zobrazení otázky. Jako uživatelské jméno je zobrazení, pokud zadáte nesprávné odpovědět zobrazí ovládací prvek PasswordRecovery chybové zprávy (viz obrázek 3). Použití [ `QuestionFailureText` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) přizpůsobení tato chybová zpráva.


[![Pokud uživatel zadá neplatný bezpečnostní otázce, zobrazí se chybová zpráva](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**Obrázek 3**: Pokud uživatel zadá neplatný bezpečnostní otázce, zobrazí se chybová zpráva ([kliknutím ji zobrazíte obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image9.png))


Nakonec zadejte správný bezpečnostní otázce a klikněte na Odeslat. Na pozadí ovládacího prvku PasswordRecovery generuje náhodné heslo, přiřadí uživatelský účet, odešle e-mail informující uživatele nového hesla (viz obrázek 4) a potom zobrazí zobrazení informací o úspěchu.


[![Uživateli se odešle E-mail s jeho nové heslo](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**Obrázek 4**: uživatele se odešle E-mail s jeho nové heslo ([kliknutím ji zobrazíte obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image12.png))


### <a name="customizing-the-email"></a>Přizpůsobení e-mailu

Výchozí e-mailu PasswordRecovery ovládacím prvkem je spíše mdle (viz obrázek 4). Je zpráva odeslána z účtu zadanému u `<smtp>` elementu `from` atribut s předmětem heslo a text ve formátu prostého textu:

Vraťte se na web a přihlaste se pomocí následujících informací.

Uživatelské jméno: *uživatelské jméno*

heslo: *heslo*

Tato zpráva je možné přizpůsobit prostřednictvím kódu programu pomocí obslužné rutiny události pro ovládací prvek PasswordRecovery [ `SendingMail` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), nebo deklarativně až [ `MailDefinition` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Podívejme se na obě tyto možnosti.

`SendingMail` Událost se aktivuje těsně před e-mailové zprávy je odeslán a je naše poslední příležitost dobře se prostřednictvím kódu programu nastavit e-mailové zprávy. Když se tato událost je aktivována, obslužná rutina události je předán objekt typu [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), jehož `Message` vlastnost obsahuje odkaz na e-mailu, o které se mají odeslat.

Vytvořte obslužnou rutinu události pro `SendingMail` událostí a přidejte následující kód, který přidává prostřednictvím kódu programu `webmaster@example.com` do seznamu Popisků.

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

E-mailové zprávě je možné nakonfigurovat také pomocí deklarativní způsobem. PasswordRecovery `MailDefinition` vlastnost je objekt typu [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). `MailDefinition` Třída nabízí celou řadu vlastností související s e-mailu, včetně `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`a další. Pokud začínáte, nastavte [ `Subject` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) na něco popisného více než jeden používá ve výchozím nastavení (heslo), jako je vaše heslo bylo resetováno...

Přizpůsobit text e-mailové zprávy, které je třeba vytvořit soubor oddělené e-mailové šablony, který obsahuje obsah textu. Začněte tím, že vytvoříte novou složku na webu s názvem `EmailTemplates`. V dalším kroku přidejte do nového textového souboru do této složky s názvem `PasswordRecovery.txt` a přidejte následující obsah:

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

Všimněte si použití zástupných symbolů `<%UserName%>` a `<%Password%>`. Ovládací prvek PasswordRecovery automaticky nahradí tyto dva zástupné symboly pomocí uživatelského jména a hesla obnovené před odesláním e-mailu uživatele.

A konečně, přejděte `MailDefinition`společnosti [ `BodyFileName` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) do e-mailové šablony jsme právě vytvořili (`~/EmailTemplates/PasswordRecovery.txt`).

Po provedení těchto změn revidovat `RecoverPassword.aspx` stránku a zadejte uživatelské jméno a zabezpečení odpovědí. Obdržíte by měl e-mailu, která vypadá jako na obrázku 5. Všimněte si, že `webmaster@example.com` byla kopie by a že byly aktualizovány předmět a text.


[![Byly aktualizovány předmět, text a seznam Popisků](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**Obrázek 5**: The předmět, text a kopie byl aktualizován seznam ([kliknutím ji zobrazíte obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image15.png))


Chcete-li odeslat e-mailu ve formátu HTML nastavte [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) na hodnotu True (výchozí hodnota je False) a aktualizaci e-mailové šablony, které chcete zahrnout HTML.

`MailDefinition` Vlastnost není pro třídu PasswordRecovery jedinečné. Jak vidíte v kroku 2, ovládací prvek ChangePassword také nabízí `MailDefinition` vlastnost. Kromě toho zahrnuje ovládacím prvku CreateUserWizard těchto vlastností, můžete nakonfigurovat automaticky odeslat zprávu Uvítacího e-mailu novým uživatelům.

> [!NOTE]
> Aktuálně neexistují žádné odkazy v levém navigačním panelu pro dosažení `RecoverPassword.aspx` stránky. Uživatel by pouze zajímat navštívit tuto stránku, pokud byla schopna úspěšně Přihlaste se k webu. Proto se aktualizace `Login.aspx` zahrnout odkaz na stránku `RecoverPassword.aspx` stránky.


### <a name="programmatically-resetting-a-users-password"></a>Resetuje heslo uživatele se prostřednictvím kódu programu

Při resetování hesla uživatele PasswordRecovery řídit volání `MembershipUser` objektu [ `ResetPassword` metoda](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Tato metoda má dvě přetížení:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** – může resetovat hesla uživatele. Toto přetížení použijte, pokud `RequiresQuestionAndAnswer` je False.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** -Obnoví pouze tehdy, pokud uživatelské heslo zadané *securityAnswer* je správná. Toto přetížení použijte, pokud `RequiresQuestionAndAnswer` má hodnotu True.

Obě přetížení vrátí nové, náhodně vygenerované heslo.

Jako s jinými metodami v rámci členství `ResetPassword` metoda delegáty pro konfigurovaný poskytovatel. `SqlMembershipProvider` Vyvolá `aspnet_Membership_ResetPassword` předáním hodnoty uložené procedury, uživatelské jméno, nové heslo a odpovědí zadané heslo, mimo jiné pole. Uložená procedura zajistí, že odpověď hesla odpovídá a pak aktualizuje heslo uživatele.

Několik poznámky k implementaci nižší úrovně:

- Neuzamčení uživatele nelze resetovat své heslo. Nicméně může neschválených uživatele. Jsme se popisují uzamčeném navýšení kapacity a schválení stavy podrobněji <a id="_msoanchor_3"> </a> [ *Unlocking a schvalování uživatelských* ](unlocking-and-approving-user-accounts-vb.md) kurzu účty.
- Pokud odpověď hesla je nesprávná, se zvýší počet pokusů o odpověď hesla uživatele. Pokud dojde k zadaný počet pokusů o neplatné zabezpečení odpověď v rámci zadaného časového období, uživatel je uzamčen.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Aplikace Word v jak the náhodná hesla jsou generovány.

Náhodně generované heslo uvedené v e-mailové zprávy s obrázky 4 a 5 jsou vytvořeny pomocí třídy členství [ `GeneratePassword` metoda](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Tato metoda přijímá dva celé číslo vstupní odřádkování - *délka* a *numberOfNonAlphanumericCharacters* – a vrátí řetězec alespoň *délka* znaků dlouhé v nejméně *numberOfNonAlphanumericCharacters* počet jiných než alfanumerických znaků. Když tato metoda je volána z v rámci třídy členství nebo související s přihlášením webové ovládací prvky, jsou určeny hodnoty pro tyto dva parametry konfigurace členství `MinRequiredPasswordLength` a `MinRequiredNonalphanumericCharacters` vlastnosti, které jsme si nastavili 7 a 1, v uvedeném pořadí.

`GeneratePassword` Metoda používá k zajištění, že neexistuje žádná posun v jaké náhodných znaků jsou vybrány kryptograficky silnou generátor náhodných čísel. Kromě toho `GeneratePassword` je `Public`, což znamená, že můžete přímo z vaší aplikace ASP.NET potřebujete ke generování náhodného řetězce nebo hesla.

> [!NOTE]
> `SqlMembershipProvider` Vždy vytvoří náhodné heslo dlouhé aspoň 14 znaků, takže když `MinRequiredPasswordLength` je menší než 14 a její hodnota je ignorována.


## <a name="step-2-changing-passwords"></a>Krok 2: Změna hesel

Náhodně vygenerované heslo je obtížné mějte na paměti. Vezměte v úvahu heslo je znázorněno na obrázku 4: `WWGUZv(f2yM:Bd`. Vyzkoušejte potvrzení, která do paměti. Needless znamená po odeslání náhodně generované heslo tohoto uživatele si budete chtít změnit heslo zapamatovatelný.

Ovládací prvek ChangePassword použijte k vytvoření rozhraní pro uživatele, chcete-li změnit své heslo. Podobně jako ovládací prvek PasswordRecovery, ovládací prvek ChangePassword se skládá ze dvou zobrazení: Změna hesla a úspěšné dokončení. Změna hesla zobrazení vyzve uživatele k jejich staré a nové heslo. Po zadání správné staré heslo a nové heslo, které splňuje minimální délku a požadavky na jiné než alfanumerické znaky, ovládací prvek ChangePassword aktualizuje heslo uživatele a zobrazí zobrazení informací o úspěchu.

> [!NOTE]
> Ovládací prvek ChangePassword změní heslo uživatele vyvoláním `MembershipUser` objektu [ `ChangePassword` metoda](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). Metodu ChangePassword přijímá dva `String` vstupní parametry - *oldPassword* a *newPassword*– a aktualizuje uživatelský účet s *newPassword*, za předpokladu, že zadané *oldPassword* je správná.


Otevřít `ChangePassword.aspx` stránky a ovládací prvek ChangePassword přidat na stránku jeho pojmenování `ChangePwd`. V tomto okamžiku by měl zobrazení návrhu zobrazení změnit heslo zobrazení (viz obrázek 6). Stejně jako s ovládacím prvkem PasswordRecovery, můžete přepínat mezi zobrazeními přes inteligentní značky ovládacího prvku. Kromě toho vystoupení na tato zobrazení lze přizpůsobit prostřednictvím vlastnosti různé stylu nebo jejich převedením na šablonu.


[![Přidejte ovládací prvek ChangePassword na stránku](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**Obrázek 6**: přidejte ovládací prvek ChangePassword na stránku ([kliknutím ji zobrazíte obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image18.png))


Ovládací prvek ChangePassword lze aktualizovat heslo aktuálně přihlášeného uživatele *nebo* heslo jiné, zadaného uživatele. Jak je vidět na obrázku 6, výchozí heslo změnit zobrazení vykreslí pouhých tří ovládacích prvcích TextBox: jeden pro staré heslo a dvě pro nové heslo. Tento výchozí rozhraní se používá k aktualizaci hesla aktuálně přihlášeného uživatele.

Pokud chcete použít ovládací prvek ChangePassword aktualizovat heslo pro jiného uživatele, nastavte ovládacího prvku [ `DisplayUserName` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) na hodnotu True. Tím se přidají čtvrté textového pole na stránku, která vás vyzve k zadání uživatelského jména uživatele, jehož heslo chcete-li změnit.

Nastavení `DisplayUserName` na hodnotu True, je užitečné, pokud chcete, aby si přihlášeného uživatele bez nutnosti přihlášení změnit své heslo. Osobně kterou myslím, že není nic špatného by po uživateli přihlášení předtím, než jí změnit své heslo. Proto nechte `DisplayUserName` nastavena na hodnotu False (výchozí). V rozhodování, ale jsme v podstatě se blokování anonymním uživatelům dosažení této stránky. Aktualizovat autorizačních pravidel adres URL webu tak, aby zakázat anonymní uživatele z webu `ChangePassword.aspx`. Pokud je potřeba aktualizovat vaše paměti v syntaxi adresy URL autorizační pravidlo, vraťte se do <a id="_msoanchor_4"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-vb.md) kurzu.

> [!NOTE]
> To může zdát, `DisplayUserName` vlastnost je užitečná pro povolení správce o změnu hesla jiných uživatelů. Nicméně i v případě `DisplayUserName` nastavena na hodnotu True, musí být známé a zadali správné staré heslo. Věnuje se techniky umožňující správcům, aby se změna hesla uživatelů v kroku 3.


Přejděte `ChangePassword.aspx` stránce prostřednictvím prohlížeče a změňte si heslo. Všimněte si, že pokud zadáte nové heslo, které nesplní délku hesla a požadavky na jiný než alfanumerický znak zadaného v konfiguraci členství, zobrazí se chybová zpráva (viz obrázek 7).


[![Přidejte ovládací prvek ChangePassword na stránku](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**Obrázek 7**: přidejte ovládací prvek ChangePassword na stránku ([kliknutím ji zobrazíte obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image21.png))


Při zadávání správné staré heslo a platný nové heslo, přihlášeného uživatele změnit heslo a zobrazí zobrazení informací o úspěchu.

### <a name="sending-a-confirmation-email"></a>Odeslání potvrzovací E-mail

Ve výchozím nastavení ovládací prvek ChangePassword e-mailovou zprávu pro uživatele, jehož heslo bylo aktualizováno pouze neodešle. Pokud chcete odeslat e-mailu, jednoduše nakonfigurovat ovládacího prvku `MailDefinition` vlastnost. Umožňuje nakonfigurovat ovládací prvek ChangePassword tak, aby uživatel se odešle e-mail ve formátu HTML, který obsahuje nového hesla.

Začněte vytvořením nového souboru v `EmailTemplates` složku s názvem `ChangePassword.htm`. Přidejte následující kód:

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

Dále nastavte ovládací prvek ChangePassword `MailDefinition` vlastnosti `BodyFileName`, `IsBodyHtml`, a `Subject` vlastností ~ / změnil EmailTemplates/ChangePassword.htm, True a hesla! v uvedeném pořadí.

Po provedení těchto změn, otevírat stránku a změňte si heslo znovu. Tentokrát, ovládací prvek ChangePassword odešle e-mail přizpůsobené, ve formátu HTML e-mailovou adresu uživatele v souboru (viz obrázek 8).


[![Zpráva e-mailu informuje uživatele, že jejich heslo změněno](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**Obrázek 8**: e-mailová zpráva informuje uživatele, že jejich heslo změněno ([kliknutím ji zobrazíte obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Krok 3: Povolení správcům měnit hesla uživatelů

Běžné funkce v aplikacích, které podporují uživatelských účtů je možnost pro správce o změnu hesla jiných uživatelů. Někdy je to nezbytně nutné, protože systém nemá schopnost uživatelům změnit si vlastní hesla. V takovém případě by jedině uživatel může obnovit zapomenuté heslo správce pro jejich přiřazení nové heslo. S ovládacími prvky PasswordRecovery a metodu ChangePassword však administrativní uživatelé nemusí zaneprázdněný sami změna hesel uživatelů, jako jsou dokáže udělat tento sami uživatelé.

Ale co když klient insists administrativní uživatelé měli změnit hesla jiných uživatelů? Bohužel přidání této funkce může být trochu práce. Chcete-li změnit heslo uživatele, musí být poskytnuty staré a nové heslo `MembershipUser` objektu `ChangePassword` metody, ale správce by neměl mít vědět heslo uživatele, aby bylo možné ho upravit.

Jeden alternativní řešení je nejprve resetovat heslo uživatele a změňte ho na nové heslo pomocí kódu, jako je následující:

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

Tento kód spustí načtením informací o *uživatelské jméno*, což je uživatele, jehož heslo správce chce změnit. Dále `ResetPassword` je vyvolána metoda, která přiřadí a nový, náhodný heslo uživatele. Toto náhodně vygenerované heslo je vrácen touto metodou a uložen v proměnné `resetPwd`. Teď, když víme heslo uživatele, můžeme ho změnit prostřednictvím volání `ChangePassword`.

Problém je, že tento kód funguje jenom v případě konfigurace systému členství je nastavena tak, aby `RequiresQuestionAndAnswer` je False. Pokud `RequiresQuestionAndAnswer` je hodnota True, jako je s naší aplikací, pak bude `ResetPassword` metoda musí být předán zabezpečovací odpověď, v opačném případě vyvolá výjimku.

Pokud členství v rámci nakonfigurován tak, aby zabezpečovací otázka a odpověď, a ještě vašeho klienta insists, že správci mohli změnit hesla uživatelů, máte tři možnosti:

- Vyvolat možnost využívat ve vzduchu a že váš klient se, že se jedná jen jednou z věcí, které nelze provést.
- Nastavte `RequiresQuestionAndAnswer` na hodnotu False. Výsledkem je méně zabezpečené aplikace. Představte si, že neslouží pro nekalé účely uživatel získal přístup k e-mailové schránky jiných uživatelů. Možná ohrožené uživatele opustil svého stolu přejdete na oběd a neměli zamknout pracovní stanici nebo možná získat přístup k e-mailu z veřejného terminálu a neměli Odhlásit se. V obou případech se můžou navštívit neslouží pro nekalé účely uživatele `RecoverPassword.aspx` stránku a zadejte uživatelské jméno uživatele. Systém se e-mail obnovené heslo bez výzvy pro zabezpečovací odpověď.
- Obejít abstraktní vrstvu vytvořil členství v rámci a práci přímo s databází SQL serveru. Schématu členství zahrnuje uloženou proceduru s názvem `aspnet_Membership_SetPassword` , který nastaví heslo uživatele a zabezpečení odpovědí nebo staré heslo není vyžadován k provedení svých úkolů.

Žádnou z těchto možností jsou zvláště atraktivní, ale to je jak přejde někdy život vývojáře.

Můžu jsem a implementovat třetí postup psaní kódu, který obchází `Membership` a `MembershipUser` třídy a používá přímo `SecurityTutorials` databáze.

> [!NOTE]
> Při práci přímo s databází, je shattered zapouzdření poskytovaného rámcem členství. Toto rozhodnutí propojuje nám `SqlMembershipProvider`, provádění našeho kódu méně portable. Kromě toho tento kód nemusí fungovat podle očekávání v budoucích verzí prostředí ASP.NET, pokud se změní členství ve schématu. Tento přístup je alternativní řešení, a jako většina řešení není příkladem osvědčené postupy.


Kód obsahuje některé body bity a je poměrně dlouhé. Proto se nechci se nyní dál sbližuje tyto v tomto kurzu se podrobné zkoumání. Pokud vás více zajímá, stáhněte si kód pro tento kurz a navštivte `~/Administration/ManageUsers.aspx` stránky. Tuto stránku, kterou jsme vytvořili v <a id="_msoanchor_5"> </a> [předchozím kurzu](building-an-interface-to-select-one-user-account-from-many-vb.md), jsou uvedeny jednotlivé uživatele. Aktualizovali jsme GridView zahrnout odkaz `UserInformation.aspx` stránku předáním uživatelské jméno vybraného uživatele prostřednictvím řetězec dotazu. `UserInformation.aspx` Stránky zobrazí informace o vybraného uživatele a textová pole pro změnu hesla (viz obrázek 9).

Po zadání nové heslo, potvrzení do druhého textového pole a kliknutím na tlačítko Aktualizovat uživatele, zpětné volání vyplývá a `aspnet_Membership_SetPassword` uložené procedury je vyvolána, aktualizuje se heslo uživatele. Neváhejte těchto čtenáři zájem o tuto funkci se blíže seznámíte s kódem a zkuste to rozšíření funkce pro zasílání e-mailu pro uživatele, jejichž heslo se změnilo.


[![Správce může změnit heslo uživatele](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**Obrázek 9**: Správce může změnit heslo uživatele ([kliknutím ji zobrazíte obrázek v plné velikosti](recovering-and-changing-passwords-vb/_static/image27.png))


> [!NOTE]
> `UserInformation.aspx` Stránce aktuálně funguje pouze, pokud rozhraní členství je nakonfigurovaná pro ukládání hesel ve formátu vymazat nebo Hashed. I když budete vyzváni k přidání této funkce neobsahuje kód k šifrování nové heslo. Způsob, jak můžu jenom doporučit přidání nezbytného kódu je použití decompiler jako [Reflector](http://www.aisto.com/roeder/dotnet/) k Zkontrolujte zdrojový kód pro metody v rozhraní .NET Framework; začít tím, že kontroluje `SqlMembershipProvider` třídy `ChangePassword` metoda. Toto je postup, který jsem používal napsat kód pro vytvoření hodnoty hash hesla.


## <a name="summary"></a>Souhrn

Technologie ASP.NET nabízí dva ovládací prvky k poskytování pomoci uživatelům spravovat své heslo. Ovládací prvek PasswordRecovery je užitečný pro ty, kteří jste zapomněli své heslo. V závislosti na konfiguraci členství v rámci uživatel je buď e-mailem hesla existující nebo nové, náhodně vygenerované heslo. Ovládací prvek ChangePassword umožňuje uživateli aktualizovat své heslo.

Jako ovládací prvky pro přihlášení a CreateUserWizard vykreslení ovládacích prvků PasswordRecovery a metodu ChangePassword bohatého uživatelského rozhraní bez nutnosti psát ikonu z deklarativní nebo řádku kódu. Pokud uživatelské rozhraní výchozí podobě nevyhovuje vašim potřebám, můžete přizpůsobit pomocí různých vlastnosti stylu. Můžete také ovládací prvky rozhraní se může převést do šablon pro i na jemnější stupeň kontroly. Na pozadí použijte tyto ovládací prvky rozhraní API pro členství, volání `MembershipUser` objektu `ResetPassword` a `ChangePassword` metody.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Ovládací prvek ChangePassword šablon rychlý start](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Ovládací prvek PasswordRecovery šablon rychlý start](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Odeslání e-mailu v ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` Nejčastější dotazy](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, Autor více ASP/ASP.NET knih a zakladatelem 4GuysFromRolla.com, má pracovali Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy  *[Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott může být dostupný na adrese [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu zahrnují Michael Emmings a Suchi Banerjee. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](building-an-interface-to-select-one-user-account-from-many-vb.md)
> [další](unlocking-and-approving-user-accounts-vb.md)
