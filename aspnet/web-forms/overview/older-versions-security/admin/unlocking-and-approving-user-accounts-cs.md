---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Odemykání a schvalování uživatelských účtů (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Tento kurz ukazuje, jak vytvořit webovou stránku pro správce ke správě uzamčen a schválení stavy uživatelů. Také jsme uvidí schválení nového uživatele o...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a8373f62833c3a76d2e7f96193e5ecbe2d9c593
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753460"
---
<a name="unlocking-and-approving-user-accounts-c"></a>Odemykání a schvalování uživatelských účtů (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> Tento kurz ukazuje, jak vytvořit webovou stránku pro správce ke správě uzamčen a schválení stavy uživatelů. Také jsme uvidí schválení nových uživatelů až po ověření e-mailová adresa.


## <a name="introduction"></a>Úvod

Společně se uživatelské jméno, heslo a e-mailu, každý uživatelský účet má dvě pole Stav, určující, zda uživatel může přihlásit k webu: uzamčen a schválena. Uživatel je automaticky uzamčen-li poskytují neplatné přihlašovací údaje do zadaného počtu opakování v zadaném počtu minut (výchozí nastavení uzamčení uživatele po 5 neplatných pokusů o přihlášení během 10 minut). Schválené stav je užitečné v situacích, kde musí probíhají některé akce, bude moci přihlásit k webu nového uživatele. Uživatel například může být nutné nejprve ověření e-mailová adresa nebo schválení správcem, teprve pak ji bude moci přihlásit.

Protože uzamčeném mimo nebo neověřený uživatel nemůže přihlásit, je pouze přirozeného k zajímat, jak můžete tyto stavy resetování. ASP.NET neobsahuje žádné předdefinované funkce nebo uzamčen webové ovládací prvky pro správu uživatelů a v části schválené stavy, protože tato rozhodnutí musí zpracovat na základě serveru lokality. Některé servery může automaticky schválit všechny nové uživatelské účty (výchozí chování). Ostatní uživatelé měli správce schválit nové účty nebo není schválit uživatelé navštíví odkaz odesílat e-mailovou adresu, když se zaregistrovali k dispozici. Podobně některé weby může uzamčení uživatelů, dokud správce resetuje jejich stav, zatímco jiné weby odeslat e-mailu neuzamčení uživateli pomocí adresy URL, které může navštěvovat odemknout svůj účet.

Tento kurz ukazuje, jak vytvořit webovou stránku pro správce ke správě uzamčen a schválení stavy uživatelů. Také jsme uvidí schválení nových uživatelů až po ověření e-mailová adresa.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Krok 1: Správa uživatelů uzamčen a schválení stavy

V <a id="Tutorial12"> </a> [ *vytvoření rozhraní pro výběr jednoho uživatelského účtu z mnoha* ](building-an-interface-to-select-one-user-account-from-many-cs.md) kurzu jsme vytvořit stránku, která uvedené všechny uživatelské účty ve stránkovaném, filtrování ovládacího prvku GridView. Mřížka obsahuje každé uživatelské jméno a e-mailu, jejich schválené a neuzamčení stavy, určuje, zda jsou aktuálně online a informací o tom, že uživatel. Ke správě uživatelů schválena a stavy uzamčen, může Usnadňujeme tuto mřížku upravovat. Chcete-li změnit stav uživatele, schválené, by správce nejdřív vyhledejte uživatelský účet a pak upravte odpovídající řádek prvku GridView, kontrola nebo zrušení zaškrtnutí políčka schválené. Alternativně může spravujeme schválené a neuzamčení stavy prostřednictvím samostatné stránky technologie ASP.NET.

Pro účely tohoto kurzu použijeme dvě stránky ASP.NET: `ManageUsers.aspx` a `UserInformation.aspx`. Zde spočívá v tom, `ManageUsers.aspx` seznam uživatelských účtů v systému, zatímco `UserInformation.aspx` umožňuje správcům spravovat schválené a neuzamčení stavy pro konkrétního uživatele. Naše první je k posílení GridView v `ManageUsers.aspx` zahrnout HyperLinkField, která vykresluje jako sloupec odkazů. Chceme, aby každý odkaz tak, aby odkazoval na `UserInformation.aspx?user=UserName`, kde *uživatelské jméno* je jméno uživatele, kterého chcete upravit.

> [!NOTE]
> Pokud jste stáhli kód <a id="Tutorial13"> </a> [ *obnovení a změna hesel* ](recovering-and-changing-passwords-cs.md) kurz, mohli jste si všimnout, který `ManageUsers.aspx` stránka již obsahuje sadu " Správa"odkazů a `UserInformation.aspx` stránka poskytuje rozhraní pro změnu hesla vybraného uživatele. Můžu se rozhodli replikace, které tuto funkci v kódu přidružená v tomto kurzu, protože to šlo obcházení členství rozhraní API a provoz přímo s databází SQL serveru ke změně hesla uživatele. Tento kurz pracuje úplně od začátku s `UserInformation.aspx` stránky.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Přidání "Manage" odkazy na`UserAccounts`GridView

Otevřít `ManageUsers.aspx` stránky a přidat HyperLinkField k `UserAccounts` ovládacího prvku GridView. Nastavte HyperLinkField `Text` vlastnost "Manage" a jeho `DataNavigateUrlFields` a `DataNavigateUrlFormatString` vlastností `UserName` a "UserInformation.aspx?user={0}" v uvedeném pořadí. Tato nastavení konfigurují HyperLinkField tak, aby všechny hypertextové odkazy zobrazí text "Manage", ale každý odkaz předává do příslušné *uživatelské jméno* hodnotu na řetězec dotazu.

Po přidání HyperLinkField do prvku GridView, věnujte chvíli zobrazíte `ManageUsers.aspx` stránky prostřednictvím prohlížeče. Obrázek 1 ukazuje, každý řádek prvku GridView teď obsahuje odkaz "Manage". Odkazuje na odkaz "Manage" Bruce `UserInformation.aspx?user=Bruce`, že odkaz "Manage" Dave odkazuje na `UserInformation.aspx?user=Dave`.


[![Přidá HyperLinkField](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Obrázek 1**: HyperLinkField přidá odkaz "Manage" pro každý uživatelský účet ([kliknutím ji zobrazíte obrázek v plné velikosti](unlocking-and-approving-user-accounts-cs/_static/image3.png))


Budeme vytvářet uživatelské rozhraní a kód pro `UserInformation.aspx` stránce v okamžiku, ale první Pojďme build2014 o tom, jak programově změnit uživatel uzamčen a schválení stavy. [ `MembershipUser` Třídy](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) má [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) a [ `IsApproved` vlastnosti](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). `IsLockedOut` Vlastnost je jen pro čtení. Neexistuje žádný mechanismus k programově uzamčení uživatele. Chcete-li odemčení uživatele, použijte `MembershipUser` třídy [ `UnlockUser` metoda](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx). `IsApproved` Vlastnost je čtení a zápisu. K ukládání veškerých změn této vlastnosti, musíme volání `Membership` třídy [ `UpdateUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)a předejte upravené `MembershipUser` objektu.

Vzhledem k tomu, `IsApproved` vlastnost slouží ke čtení a zápis, ovládací prvek CheckBox je pravděpodobně nejlepší prvku uživatelského rozhraní pro konfiguraci této vlastnosti. Ale nebude fungovat zaškrtávací políčko `IsLockedOut` vlastnost vzhledem k tomu, že si uživatel nemůže uzamknout správce, které může odemknout jenom uživatel. Vhodné uživatelské prostředí pro `IsLockedOut` vlastnost je tlačítko, po kliknutí na odemkne uživatelský účet. Toto tlačítko musí být povolené, pouze pokud je uživatel uzamčen.

### <a name="creating-theuserinformationaspxpage"></a>Vytváří`UserInformation.aspx`stránky

Teď máme připraven k implementaci uživatelského rozhraní v `UserInformation.aspx`. Otevřete tuto stránku a přidejte následující webové ovládací prvky:

- Hypertextový odkaz ovládací prvek, který, při kliknutí vrátí správce, aby `ManageUsers.aspx` stránky.
- Popisek webový ovládací prvek pro zobrazení vybrané uživatelské jméno. Nastavit tento popisek `ID` k `UserNameLabel` a vymažte její `Text` vlastnost.
- Ovládací prvek zaškrtávací políčko s názvem `IsApproved`. Nastavte jeho `AutoPostBack` vlastnost `true`.
- Ovládací prvek popisek a pro zobrazování uživatel je uzamčen datum. Pojmenujte tento popisek `LastLockedOutDateLabel` a vymažte její `Text` vlastnost.
- Tlačítko pro odemknutí uživatele. Pojmenujte toto tlačítko `UnlockUserButton` a nastavte jeho `Text` vlastnost "Odemčení uživatele".
- Ovládací prvek popisku pro zobrazení stavových zpráv, jako například "schválený stav uživatele byl aktualizován." Tento ovládací prvek pojmenujte `StatusMessage`, zrušte zaškrtnutí políčka si jeho `Text` vlastnost a nastavte jeho `CssClass` vlastnost `Important`. ( `Important` Třídu šablony stylů CSS je definována v `Styles.css` soubor šablony stylů; zobrazuje odpovídající text velký červený písmem.)

Po přidání těchto ovládacích prvků, by měl vypadat podobně jako na obrázku 2 snímek obrazovky zobrazení návrhu v sadě Visual Studio.


[![Vytvoření uživatelského rozhraní pro UserInformation.aspx](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Obrázek 2**: vytvoření uživatelského rozhraní pro `UserInformation.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](unlocking-and-approving-user-accounts-cs/_static/image6.png))


Dokončení uživatelského rozhraní, naše dalším úkolem je nastavena `IsApproved` zaškrtávací políčko a další ovládací prvky založené na vybrané uživatelské informace. Vytvořte obslužnou rutinu události pro danou stránku `Load` událostí a přidejte následující kód:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

Výše uvedený kód začne tím, že zajišťuje, že se jedná o první návštěvě stránky a následný postback. Pak přečte uživatelské jméno předané prostřednictvím `user` pole řetězce dotazu a načte informace o účtu uživatele prostřednictvím `Membership.GetUser(username)` metody. Pokud žádné uživatelské jméno není zadaná pomocí řetězec dotazu, nebo pokud se zadaný uživatel se nenašel, správce budou odeslána zpět do `ManageUsers.aspx` stránky.

`MembershipUser` Objektu `UserName` se následně zobrazí hodnota `UserNameLabel` a `IsApproved` je zaškrtnuté políčko na základě `IsApproved` hodnotu vlastnosti.

`MembershipUser` Objektu [ `LastLockoutDate` vlastnost](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) vrátí `DateTime` hodnotu, která udává, kdy byl naposledy uživatel uzamčen. Pokud uživatel nikdy byl uzamčen, vrácená hodnota závisí na zprostředkovatele členství. Když je vytvořen nový účet, `SqlMembershipProvider` nastaví `aspnet_Membership` tabulky `LastLockoutDate` pole `1754-01-01 12:00:00 AM`. Výše uvedený kód zobrazí prázdný řetězec v `LastLockoutDateLabel` Pokud `LastLockoutDate` vlastnost předchází roku 2000; v opačném případě část data `LastLockoutDate` vlastnost je zobrazený v popisku. `UnlockUserButton'` s `Enabled` vlastnost je nastavena podle uživatele uzamčen stavu, což znamená, že toto tlačítko bude povolit, pouze pokud je uživatel uzamčen.

Za chvíli testování `UserInformation.aspx` stránky prostřednictvím prohlížeče. Samozřejmě, musíte spustit na `ManageUsers.aspx` a vyberte uživatelský účet pro správu. Při dosažení `UserInformation.aspx`, Všimněte si, že `IsApproved` zaškrtávací políčko je zaškrtnuto, pouze je-li uživatel schválen. Pokud někdy byl uzamčen uživatel, zobrazí se jejich poslední datum uzamčení. Tlačítka pro odemknutí uživatele je povoleno pouze v případě, že uživatel je aktuálně uzamčen. Zaškrtnutím nebo zrušením zaškrtnutí `IsApproved` zaškrtávací políčko nebo kliknutím na tlačítko odemčení uživatele vyvolá zpětné volání, ale žádné změny provedené uživatelský účet vzhledem k tomu, že jsme dosud k vytvoření obslužné rutiny události pro tyto události.

Vraťte se do sady Visual Studio a vytvořte obslužné rutiny událostí pro `IsApproved` zaškrtávací políčko pro `CheckedChanged` událostí a `UnlockUser` tlačítka `Click` událostí. V `CheckedChanged` obslužná rutina události, nastavit uživatele `IsApproved` vlastnost `Checked` vlastnost na zaškrtávací políčko a potom uložte změny prostřednictvím volání `Membership.UpdateUser`. V `Click` obslužná rutina události, jednoduše volání `MembershipUser` objektu `UnlockUser` metody. V obou obslužné rutiny událostí, zobrazení vhodný zprávy v `StatusMessage` popisek.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Testování`UserInformation.aspx`stránky

Pomocí těchto obslužných rutin událostí v místě, otevírat stránku a neschválených uživatele. Obrázek 3 ukazuje, měli byste vidět stručný zpráva na stránce, což indikuje, že uživatele `IsApproved` úspěšně změněna vlastnost.


[![Chris byl Neschváleno](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Obrázek 3**: Chris byl Neschváleno ([kliknutím ji zobrazíte obrázek v plné velikosti](unlocking-and-approving-user-accounts-cs/_static/image9.png))


V dalším kroku odhlášení a zkuste se přihlásit jako uživatel, jehož účet byl právě neschválených. Vzhledem k tomu, že uživatel není schválený, nemůžou přihlásit. Ve výchozím nastavení ovládací prvek Login zobrazí stejnou zprávu, pokud uživatel nemůže přihlásit, bez ohledu na důvod. Ale <a id="Tutorial6"> </a> [ *ověření uživatele přihlašovací údaje proti the členství uživatele Store* ](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) kurzu jsme se podívali na rozšíření ovládacího prvku pro přihlášení k zobrazení více odpovídající zprávu. Jak je vidět na obrázku 4, Chris se zobrazí se zpráva s vysvětlením, že mu nemůžou přihlásit, protože jeho účet ještě není schválený.


[![Chris nelze protože jeho účet pro přihlášení je Neschváleno](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Obrázek 4**: Chris nelze protože jeho účet pro přihlášení je Neschváleno ([kliknutím ji zobrazíte obrázek v plné velikosti](unlocking-and-approving-user-accounts-cs/_static/image12.png))


Otestovat funkci neuzamčení, pokuste se přihlásit jako uživatel s schválené, ale použít nesprávné heslo. Opakujte tento postup potřebný počet vícekrát, účet uživatele byl uzamčen. Ovládací prvek Login byl také aktualizuje a zobrazí vlastní zprávu při pokusu o přihlášení z neuzamčení účtu. Víte, že účet byl uzamčen po spuštění zobrazuje na přihlašovací stránku následující zpráva: "váš účet byl uzamčen kvůli moc velký počet neplatných pokusů o přihlášení. Obraťte se prosím na správce, aby váš účet byl odemčen."

Vraťte se `ManageUsers.aspx` stránky a klikněte na odkaz Správa neuzamčení uživatele. Jak je vidět na obrázku 5, měli byste vidět hodnotu `LastLockedOutDateLabel` tlačítka odemčení uživatele by měla být povolená. Klikněte na tlačítko odemčení uživatele k odemknutí uživatelského účtu. Jakmile jste odemkli uživatele, budou moct znovu přihlásit.


[![Dave byl uzamčen přístup do systému](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Obrázek 5**: Dave má byl uzamčen navýšení kapacity systému ([kliknutím ji zobrazíte obrázek v plné velikosti](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>Krok 2: Zadání nového uživatele schváleno stav

Schválené stav je užitečné v situacích, kde má určitou akci, která se má provést předtím, než se nový uživatel moci přihlásit a získat přístup k funkcím specifické pro uživatele webu. Například možná používáte privátní web, kde jsou všechny stránky, s výjimkou na stránkách přihlášení a registraci přístupné pouze ověřeným uživatelům. Ale co se stane, když cizím dosáhne svůj web, najde na přihlašovací stránku a vytvoří účet? K tomu nedocházelo můžete přesunout na přihlašovací stránce `Administration` složky a vyžadovat, že správce ručně vytvořit jednotlivé účty. Alternativně může povolit všem uživatelům k registraci, ale zakázat přístup k webům, dokud správce schválí uživatelský účet.

Ve výchozím nastavení schválí ovládacím prvku CreateUserWizard nových účtů. Můžete nakonfigurovat toto chování pomocí ovládacího prvku [ `DisableCreatedUser` vlastnost](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Tuto vlastnost nastavte na `true` není schválení nových uživatelských účtů.

> [!NOTE]
> Ve výchozím nastavení protokoluje prvku CreateUserWizard automaticky na nový uživatelský účet. Toto chování závisí ovládací prvek [ `LoginCreatedUser` vlastnost](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Protože neschválených uživatelé se nemůžou přihlásit k webu, když `DisableCreatedUser` je `true` nový uživatelský účet není přihlášen na web, bez ohledu na hodnotu `LoginCreatedUser` vlastnost.


Při vytváření nových uživatelských účtů prostřednictvím prostřednictvím kódu programu `Membership.CreateUser` metodu pro vytvoření neschválených uživatelský účet pomocí jednoho z přetížení, které přijímají nového uživatele `IsApproved` hodnota vlastnosti jako vstupní parametr.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Krok 3: Schválení uživatelé pomocí ověření e-mailová adresa

Počet webů, které podporují uživatelské účty nejsou schválit noví uživatelé ověřují e-mailovou adresu, které jsou zadané při registraci. Tento proces ověření se běžně používá k zamezit robotů, spammery a dalších ne'er-do-wells vyžaduje jedinečný, ověřený e-mailovou adresu a přidá další krok v procesu registrace. V tomto modelu nový uživatel zaregistruje do jejich odesláním e-mailovou zprávu, která obsahuje odkaz na ověřovací stránku. Návštěvou odkaz má uživatel prověřené dostali e-mail, a proto, že e-mailová adresa zadaná je platný. Na stránce ověřování zodpovídá za schválení uživatele. Může k tomu dojít automaticky, a tím schválení všichni uživatelé, kteří dosáhne této stránky, nebo až poté, co uživatel zadá některé další informace, jako například [test CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Pro tento pracovní postup, musíme nejprve aktualizovat na stránku pro vytvoření účtu tak, aby noví uživatelé si neschválených. Otevřít `EnhancedCreateUserWizard.aspx` stránku `Membership` složky a sady ovládacím prvku CreateUserWizard `DisableCreatedUser` vlastnost `true`.

V dalším kroku budeme muset nakonfigurovat ovládacím prvku CreateUserWizard k odesílání e-mailu novému uživateli s pokyny k ověření svého účtu. Konkrétně jsme bude obsahovat odkaz v e-mailu na `Verification.aspx` stránky (která jsme dosud k vytvoření) a předejte nový uživatel `UserId` prostřednictvím řetězec dotazu. `Verification.aspx` Stránky se vyhledávání identifikátoru objektu pomocí zadaného uživatele a označte je schválený.

### <a name="sending-a-verification-email-to-new-users"></a>Odesílání ověřovací E-mail pro nové uživatele

K odesílání e-mailu z ovládacího prvku CreateUserWizard, konfigurovat jeho `MailDefinition` vlastnost odpovídajícím způsobem. Jak je popsáno v <a id="Tutorial13"> </a> [předchozí kurz o službě](recovering-and-changing-passwords-cs.md), zahrnují ovládací prvky ChangePassword a PasswordRecovery [ `MailDefinition` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) to funguje stejným způsobem jako CreateUserWizard ovládacího prvku.

> [!NOTE]
> Použít `MailDefinition` vlastnost musíte zadat e-mailu doručování možnosti `Web.config`. Další informace najdete v [odesílání e-mailu v ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Začněte tím, že vytvoříte novou šablonu e-mailu s názvem `CreateUserWizard.txt` v `EmailTemplates` složky. Pro šablonu používáte následující text:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Nastavte `MailDefinition'` s `BodyFileName` vlastnost "~ / EmailTemplates/CreateUserWizard.txt" a jeho `Subject` vlastnost "Vítejte Můj web! Aktivujte svůj účet."

Všimněte si, že `CreateUserWizard.txt` obsahuje e-mailovou šablonu `<%VerificationUrl%>` zástupný symbol. Tady adresu URL `Verification.aspx` stránky budou umístěny. CreateUserWizard automaticky nahradí `<%UserName%>` a `<%Password%>` zástupné symboly pomocí nového účtu uživatelské jméno a heslo, ale neexistuje žádný předdefinovaný `<%VerificationUrl%>` zástupný symbol. Musíme ručně nahraďte adresu URL odpovídající ověření.

K tomu, vytvořit obslužnou rutinu události pro CreateUserWizard [ `SendingMail` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) a přidejte následující kód:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

`SendingMail` Událost se aktivuje po `CreatedUser` událostí, což znamená, že podle času výše obslužná rutina události spustí nový uživatel již byl vytvořen účet. Jsme dostanete nový uživatel `UserId` pomocí volání `Membership.GetUser` metodu `UserName` vstoupila v ovládacím prvku CreateUserWizard. Dále je vytvořen ověření adresy URL. Příkaz `Request.Url.GetLeftPart(UriPartial.Authority)` vrátí `http://yourserver.com` část adresy URL; `Request.ApplicationPath` vrátí cestu, kde je integrován aplikace. Ověřovací adresa URL je pak definován jako `Verification.aspx?ID=userId`. Tyto dva řetězce jsou poté spojeny tvoří úplnou adresu URL. A konečně, e-mailu zprávu (`e.Message.Body`) má všechny výskyty `<%VerificationUrl%>` nahrazena úplnou adresu URL.

Výsledkem je, že noví uživatelé si neschválených, což znamená, že nemůže přihlásit na web. Kromě toho automaticky odešlou se e-mailu s odkazem na adresu URL ověřování (viz obrázek 6).


[![Nový uživatel dostane E-mail s odkazem na adresu URL pro ověření](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Obrázek 6**: nový uživatel dostane E-mail s odkazem na adresu URL pro ověření ([kliknutím ji zobrazíte obrázek v plné velikosti](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> Krok CreateUserWizard výchozí prvku CreateUserWizard zobrazí zprávu informující uživatele jejich účet se vytvořil a zobrazí tlačítko Pokračovat. Po kliknutí na tento uživatel přejde na adresu URL zadanou v ovládacího prvku `ContinueDestinationPageUrl` vlastnost. CreateUserWizard v `EnhancedCreateUserWizard.aspx` konfigurované k odesílání nových uživatelů `~/Membership/AdditionalUserInfo.aspx`, která vyzve uživatele k jejich rodiště, adresa URL domovské stránky a podpis. Protože tyto informace lze přidat pouze v případě přihlášených uživatelů, má smysl aktualizovat tuto vlastnost na odeslání uživatele zpět na domovskou stránku webu (`~/Default.aspx`). Kromě toho `EnhancedCreateUserWizard.aspx` stránky nebo kroku CreateUserWizard by měl rozšířit uživatel informován, že byl odeslán ověřovací e-mail a jejich účtu nebude neaktivuje, dokud, postupujte podle pokynů v tomto e-mailu. Tyto úpravy opuštění jako cvičení pro čtečku.


### <a name="creating-the-verification-page"></a>Vytvoření stránky ověření

Naše posledním úkolem je vytvořit `Verification.aspx` stránky. Přidat tuto stránku do kořenové složky, přidružíte ho `Site.master` stránky předlohy. Jak jsme provedli s největším počtem předchozí stránky obsahu přidat k webu, odebrat ovládací prvek obsahu, který odkazuje `LoginContent` ContentPlaceHolder tak, aby stránka obsahu používá stránku předlohy výchozí obsahu.

Přidání ovládacího prvku popisku Web `Verification.aspx` nastavte jeho `ID` k `StatusMessage` a vymažte její vlastnost textu. Dále vytvořte `Page_Load` obslužné rutiny události a přidejte následující kód:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

Hromadné výše uvedený kód ověří, zda `UserId` prostřednictvím zadaný řetězec dotazu existuje, že se jedná o platný `Guid` hodnota a zda odkazuje na existující uživatelský účet. Pokud všechny tyto kontroly úspěšně prošel zpracováním, uživatelský účet je schválen; v opačném případě se zobrazí vhodný stavová zpráva.

Obrázek 7 znázorňuje `Verification.aspx` stránce, když uživatel prostřednictvím prohlížeče.


[![Nový uživatelský účet se teď schvalují](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Obrázek 7**: nový uživatelský účet se teď schvalují ([kliknutím ji zobrazíte obrázek v plné velikosti](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>Souhrn

Všechny uživatelské účty členství mít dva stavy, které určují, jestli uživatel může přihlásit k webu: `IsLockedOut` a `IsApproved`. Obě tyto vlastnosti musí být `true` pro uživatele k přihlášení.

Uživatel je uzamčen stav se používá jako bezpečnostní opatření k snížit pravděpodobnost, že se hacker rozdělení do lokality prostřednictvím metod útoku hrubou silou. Konkrétně uživatel je uzamčen-li počet neplatných pokusů o přihlášení v rámci určité období. Tyto hranice se dají konfigurovat pomocí nastavení zprostředkovatele členství v `Web.config`.

Schválené stavu se běžně používá jako způsob zakázat noví uživatelé přihlásit, dokud se některá z akcí vypršel. Třeba web vyžaduje, že nové účty nejdříve schválit správce nebo jako jsme viděli v kroku 3 pomocí ověření e-mailová adresa.

Všechno nejlepší programování!

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, Autor více ASP/ASP.NET knih a zakladatelem 4GuysFromRolla.com, má pracovali Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy  *[Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott může být dostupný na adrese [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k...

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](recovering-and-changing-passwords-cs.md)
> [další](building-an-interface-to-select-one-user-account-from-many-vb.md)
