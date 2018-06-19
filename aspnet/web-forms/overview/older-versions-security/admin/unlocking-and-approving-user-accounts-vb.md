---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-vb
title: Odemknutí a schválení uživatelské účty (VB) | Microsoft Docs
author: rick-anderson
description: Tento kurz ukazuje, jak vytvořit webovou stránku pro správce ke správě uživatelů uzamčen a stavy, které jsou schváleny. Také jsme uvidí schválení o nové uživatele...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 041854a5-ea8c-4de0-82f1-121ba6cb2893
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 977ed9d1e68e635eadd7aa8339670f5a5d209a7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891851"
---
<a name="unlocking-and-approving-user-accounts-vb"></a>Odemykání a schvalujícím uživatelské účty (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.14.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_vb.pdf)

> Tento kurz ukazuje, jak vytvořit webovou stránku pro správce ke správě uživatelů uzamčen a stavy, které jsou schváleny. Také jsme zobrazí schválení nových uživatelů až po ověření e-mailové adresy.


## <a name="introduction"></a>Úvod

Společně s uživatelské jméno, heslo a e-mailu, každý uživatelský účet má dvě pole stavu, které určují, jestli uživatel může přihlásit k webu: uzamčen a schváleny. Uživatel je automaticky uzamčen-li obsahují neplatné přihlašovací údaje zadaného počtu opakování během zadaného počtu minut (výchozí nastavení uzamčení uživatele po 5 neplatných pokusů o přihlášení během 10 minut). Schválené stav je užitečný ve scénářích, kde musí transpire některá z akcí, než bude moci přihlásit k webu nového uživatele. Uživatel může například muset nejdřív ověřit e-mailové adresy nebo schválení správcem, teprve pak ji bude moci přihlásit.

Protože uzamčeném mimo nebo neověřený uživatel se nemůže přihlásit, protože je jen přirozený k zajímat, jak můžete tyto stavy resetovat. ASP.NET neobsahuje žádné předdefinované funkce nebo uzamčen ovládacích prvků pro správu uživatelů a částečně schválení stavy, protože tato rozhodnutí třeba zpracovat na základě jednotlivé lokality. Některé servery může automaticky schválit všechny nové uživatelské účty (výchozí nastavení). Ostatní uživatelé mají správce schválit nové účty nebo není schválit uživatelé navštíví odkaz Odeslat e-mailovou adresu zadali při jejich registraci. Podobně některé weby může zablokovat uživatele, dokud správce vynuluje jejich stav, zatímco ostatní lokality odeslat e-mail neuzamčení uživatele s adresou URL, které může navštěvovat odemknout svůj účet.

Tento kurz ukazuje, jak vytvořit webovou stránku pro správce ke správě uživatelů uzamčen a stavy, které jsou schváleny. Také jsme zobrazí schválení nových uživatelů až po ověření e-mailové adresy.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Krok 1: Správa uživatelů uzamčen a stavy, které jsou schváleny

V <a id="Tutorial12"> </a> [ *sestavení rozhraní na vyberte jeden uživatelský účet z mnoha* ](building-an-interface-to-select-one-user-account-from-many-vb.md) kurzu jsme sestavený stránku, která uvedena každého uživatelského účtu ve stránkové, filtrovat GridView. Každé uživatelské jméno a e-mailu, jejich stavy, které jsou schválené a neuzamčení, jestli jsou aktuálně online a informací o tom uživatele jsou uvedeny mřížky. Ke správě uživatelů schválena a uzamčen stavy, jsme se ověřte mřížce upravovat. Chcete-li změnit schválené stav uživatele, by správce nejdříve vyhledejte uživatelský účet a pak upravte odpovídající řádek, rutina GridView, zaškrtnutí políčka schválené. Alternativně jsme může spravovat stavy schválené a neuzamčení prostřednictvím samostatné stránky ASP.NET.

V tomto kurzu budeme používat dvě stránky ASP.NET: `ManageUsers.aspx` a `UserInformation.aspx`. Na nápad tady je, že `ManageUsers.aspx` seznamů uživatelské účty v systému, zatímco `UserInformation.aspx` umožňuje správcům spravovat schválené a neuzamčení stavy pro konkrétního uživatele. Naše první of firma pořadí je k posílení GridView v `ManageUsers.aspx` zahrnout HyperLinkField, která vykreslí jako sloupec odkazů. Chceme, přejděte na každý odkaz `UserInformation.aspx?user=UserName`, kde *uživatelské jméno* je jméno uživatele, který chcete upravit.

> [!NOTE]
> Pokud jste stáhli kód pro <a id="Tutorial13"> </a> [ *obnovení a změna hesla* ](recovering-and-changing-passwords-vb.md) kurzu jste si všimli, `ManageUsers.aspx` stránka již obsahuje sadu " Správa"odkazy a `UserInformation.aspx` stránka poskytuje rozhraní pro změnu vybraného uživatele nové heslo. Rozhodli nechcete replikovat, které tuto funkci v kódu přidružené v tomto kurzu, protože to šlo pomocí obcházení rozhraní API členství a pracuje přímo s databází systému SQL Server, chcete-li změnit heslo uživatele. V tomto kurzu spustí od začátku pomocí `UserInformation.aspx` stránky.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Přidání "Manage" odkazy na`UserAccounts`GridView

Otevřete `ManageUsers.aspx` stránky a přidejte HyperLinkField k `UserAccounts` GridView. Nastavte HyperLinkField `Text` vlastnost "Manage" a jeho `DataNavigateUrlFields` a `DataNavigateUrlFormatString` vlastnosti, které chcete `UserName` a "UserInformation.aspx?user={0}", v uvedeném pořadí. Tato nastavení konfigurovat HyperLinkField tak, aby všechny hypertextové odkazy zobrazit text "Manage", ale každý odkaz předá do příslušné *uživatelské jméno* hodnotu na řetězec dotazu.

Po přidání HyperLinkField do GridView, chvíli trvat, chcete-li zobrazit `ManageUsers.aspx` stránku prostřednictvím prohlížeče. Jak ukazuje obrázek 1, každý řádek GridView nyní zahrnuje odkaz "Manage". Na odkaz "Manage" Bruce odkazuje na `UserInformation.aspx?user=Bruce`, zatímco na odkaz "Manage" Dave odkazuje na `UserInformation.aspx?user=Dave`.


[![Přidá HyperLinkField](unlocking-and-approving-user-accounts-vb/_static/image2.png)](unlocking-and-approving-user-accounts-vb/_static/image1.png)

**Obrázek 1**: HyperLinkField přidá odkaz "Manage" pro každý uživatelský účet ([Kliknutím zobrazit obrázek v plné velikosti](unlocking-and-approving-user-accounts-vb/_static/image3.png))


Bude vytvoření uživatelského rozhraní a kód `UserInformation.aspx` stránka ve chvíli, ale prvního Pojďme talk o programové změny uživatel uzamčen a stavy, které jsou schváleny. [ `MembershipUser` Třída](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) má [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) a [ `IsApproved` vlastnosti](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). `IsLockedOut` Vlastnost je jen pro čtení. Neexistuje žádný mechanismus na prostřednictvím kódu programu uzamčení uživatele. Chcete-li odemčení uživatele, použijte `MembershipUser` třídy [ `UnlockUser` metoda](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx). `IsApproved` Vlastnost je možné číst a zapisovat. Pokud chcete uložit změny na tuto vlastnost, musíme volání `Membership` třídy [ `UpdateUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)a předejte upravenou `MembershipUser` objektu.

Protože `IsApproved` vlastnost je možné číst a zapisovat, ovládací prvek zaškrtávací políčko je pravděpodobně nejlepší element uživatelského rozhraní pro konfiguraci této vlastnosti. Však nebude fungovat zaškrtávací políčko `IsLockedOut` vlastnost vzhledem k tomu, že správce nemůže zablokovat uživatele, uživatel může odemknout jenom uživatele. Vhodné uživatelské rozhraní pro `IsLockedOut` vlastnost je tlačítko, která po kliknutí na odemkne uživatelský účet. Toto tlačítko by měly povoleny, pouze pokud je uživatel uzamčen.

### <a name="creating-theuserinformationaspxpage"></a>Vytváření`UserInformation.aspx`stránky

Snažíme se teď připravena k implementaci uživatelského rozhraní v `UserInformation.aspx`. Otevřete tuto stránku a přidejte následující ovládací prvky webového:

- Hypertextový odkaz řídit, která po kliknutí na vrátí správcům `ManageUsers.aspx` stránky.
- Ovládací prvek popisek webu pro zobrazení název vybraného uživatele. Nastavit tento popisek `ID` k `UserNameLabel` a vymažte jeho vlastnost Text.
- Ovládací prvek zaškrtávací políčko s názvem `IsApproved`. Nastavte její `AutoPostBack` vlastnost `True`.
- Ovládací prvek popisek pro zobrazení uživatel je uzamčen poslední datum. Název tento popisek `LastLockedOutDateLabel` a vymažte její `Text` vlastnost.
- Tlačítko pro odemknutí uživatele. Toto tlačítko název `UnlockUserButton` a nastavit jeho `Text` vlastnost "Odemknout uživatele".
- Ovládací prvek popisek pro zobrazení stavových zpráv, například "byla aktualizována schválené stavu uživatele." Název tohoto ovládacího prvku `StatusMessage`, zrušte na jeho `Text` vlastnost a nastavte její `CssClass` vlastnost `Important`. ( `Important` Třída CSS, která je definována v `Styles.css` souboru šablony stylů; zobrazí odpovídající text ve velkých, red písma.)

Po přidání těchto ovládacích prvků, zobrazení návrhu v sadě Visual Studio by měl vypadat podobně jako na obrázku 2 snímku obrazovky.


[![Vytvoření uživatelského rozhraní pro UserInformation.aspx](unlocking-and-approving-user-accounts-vb/_static/image5.png)](unlocking-and-approving-user-accounts-vb/_static/image4.png)

**Obrázek 2**: vytváření uživatelského rozhraní pro `UserInformation.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](unlocking-and-approving-user-accounts-vb/_static/image6.png))


S uživatelským rozhraním dokončení naše dalším úkolem je nastaven `IsApproved` zaškrtávací políčko a jiných ovládacích prvků na základě vybraného uživatele informací. Vytvoření obslužné rutiny události pro danou stránku `Load` událostí a přidejte následující kód:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample1.vb)]

Výše uvedený kód spustí tím zajistí, že se jedná o první návštěvě stránky a následný postback. Pak přečte uživatelské jméno předána `user` pole řetězce dotazu a načte informace o účtu uživatele prostřednictvím `Membership.GetUser(username)` metoda. Pokud žádné uživatelské jméno byla zadána prostřednictvím řetězec dotazu, nebo pokud se zadaný uživatel nebyl nalezen, správce budou odeslána zpět do `ManageUsers.aspx` stránky.

`MembershipUser` Objektu `UserName` se následně zobrazí hodnota `UserNameLabel` a `IsApproved` je zaškrtnuté políčko na základě `IsApproved` hodnotu vlastnosti.

`MembershipUser` Objektu [ `LastLockoutDate` vlastnost](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) vrátí `DateTime` hodnotu, která určuje, kdy byl naposledy uživatel uzamčen. Pokud uživatel nikdy byl uzamčen, hodnota vrácená závisí na zprostředkovateli členství. Když je vytvořen nový účet, `SqlMembershipProvider` nastaví `aspnet_Membership` tabulky `LastLockoutDate` do `1754-01-01 12:00:00 AM`. Výše uvedený kód zobrazí prázdný řetězec v `LastLockoutDateLabel` Pokud `LastLockoutDate` vlastnost dojde před rok 2000; v opačném část data `LastLockoutDate` vlastnost je zobrazena v popisku. `UnlockUserButton`Na `Enabled` je vlastnost nastavena na uživatele, dojde k uzamčení stavu, což znamená, že toto tlačítko bude pouze povoleno, jestliže je uzamčen.

Za chvíli k testování `UserInformation.aspx` stránku prostřednictvím prohlížeče. Samozřejmě, bude muset začínají `ManageUsers.aspx` a vyberte uživatelský účet pro správu. Při přicházejících u `UserInformation.aspx`, Všimněte si, že `IsApproved` zaškrtávací políčko je zaškrtnuto, pouze je-li uživatel schválen. Pokud uživatel někdy byl uzamčen, zobrazí se jejich poslední uzamčen datum. Tlačítko Odemknout uživatele je povoleno pouze v případě, že uživatel je aktuálně uzamčen. Zaškrtnutí `IsApproved` zaškrtávací políčko nebo klepnutím na tlačítko Odemknout uživatele způsobí, že zpětné volání, ale žádné úpravy probíhají uživatelskému účtu, protože jsme ještě pro vytváření obslužných rutin událostí pro tyto události.

Vraťte se na Visual Studio a vytváření obslužných rutin událostí pro `IsApproved` zaškrtávací políčko je `CheckedChanged` událostí a `UnlockUser` tlačítka `Click` událostí. V `CheckedChanged` obslužné rutiny události, nastavit uživatele `IsApproved` vlastnost, která má `Checked` vlastnost políčko a potom uložte změny prostřednictvím volání `Membership.UpdateUser`. V `Click` obslužné rutiny události, jednoduše volání `MembershipUser` objektu `UnlockUser` metoda. V obou obslužné rutiny událostí zobrazí zprávu vhodný v `StatusMessage` popisek.

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample2.vb)]

### <a name="testing-theuserinformationaspxpage"></a>Testování`UserInformation.aspx`stránky

Pomocí těchto obslužné rutiny událostí v místě otevírat stránku a neschválených uživatele. Jak je vidět na obrázku 3, byste měli vidět brief, zpráva na stránce, která znamená, že uživatele `IsApproved` vlastnost byla úspěšně změněna.


[![Jan byl Neschváleno](unlocking-and-approving-user-accounts-vb/_static/image8.png)](unlocking-and-approving-user-accounts-vb/_static/image7.png)

**Obrázek 3**: Jan byl Neschváleno ([Kliknutím zobrazit obrázek v plné velikosti](unlocking-and-approving-user-accounts-vb/_static/image9.png))


V dalším kroku odhlášení a zkuste se přihlásit jako uživatel, jehož účet byl právě neschválených. Protože uživatel není schválený, nemůže přihlásit. Ve výchozím nastavení zobrazí ovládací prvek pro přihlášení stejnou zprávu, pokud uživatel se nemůže přihlásit bez ohledu na důvod. Ale v <a id="Tutorial6"> </a> [ *ověřování pověření proti členství uživatele úložiště uživatele* ](../membership/validating-user-credentials-against-the-membership-user-store-vb.md) kurzu jsme se podívali na rozšíření ovládací prvek pro přihlášení k zobrazení vhodnější zprávy. Jak ukazuje obrázek 4, Jan se zobrazí zprávu, která vysvětluje, že mu nelze přihlášení vzhledem k tomu, že účet ještě není schválený.


[![Jan nelze protože His účet pro přihlášení je Neschváleno](unlocking-and-approving-user-accounts-vb/_static/image11.png)](unlocking-and-approving-user-accounts-vb/_static/image10.png)

**Obrázek 4**: Jan nelze protože His účet pro přihlášení je Neschváleno ([Kliknutím zobrazit obrázek v plné velikosti](unlocking-and-approving-user-accounts-vb/_static/image12.png))


Pokud chcete otestovat funkci neuzamčení, pokuste se přihlásit jako uživatel s schválené, ale použijte nesprávné heslo. Opakujte tento postup nezbytné počet pokusů, dokud uživatelský účet byl uzamčen. Ovládací prvek pro přihlášení byla aktualizována taky zobrazit vlastní zprávu při pokusu o přihlášení z neuzamčení účtu. Víte, že účet byl uzamčen po spuštění zobrazuje na přihlašovací stránku následující zpráva: "váš účet byl uzamčen kvůli moc velký počet neplatných pokusů o přihlášení. Obraťte se na správce, aby váš účet se odemkne."

Vraťte se na `ManageUsers.aspx` a klikněte na odkaz Správa neuzamčení uživatele. Jak je vidět na obrázku 5, měli byste vidět hodnotu v `LastLockedOutDateLabel` tlačítko Odemknout uživatele by měla být povolená. Klikněte na tlačítko Odemknout uživatele pro odemčení uživatelského účtu. Jakmile máte odemčený uživatele, bude moci přihlásit znovu.


[![Byl uzamčen Dave mimo systém](unlocking-and-approving-user-accounts-vb/_static/image14.png)](unlocking-and-approving-user-accounts-vb/_static/image13.png)

**Obrázek 5**: Dave má byl uzamčen na systému ([Kliknutím zobrazit obrázek v plné velikosti](unlocking-and-approving-user-accounts-vb/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>Krok 2: Určení noví uživatelé schválení stavu

Schválené stav je užitečný ve scénářích, kde chcete některé akce má být provedena před nový uživatel moct přihlásit a získat přístup k funkcím uživatelská lokality. Například používáte privátní web, kde jsou všechny stránky, s výjimkou stránky přihlášení a registrace přístupné pouze pro ověřené uživatele. Ale co se stane, pokud váš web dosáhne cizí osoby vyhledá na přihlašovací stránku a vytvoří účet? Chcete-li tomu zabránit může přesunout na přihlašovací stránku k `Administration` složku a vyžadovat, že správce ručně vytvořit jednotlivé účty. Alternativně může kdokoli registrace, ale zakázat přístup k serveru, dokud správce schválí uživatelský účet.

Ve výchozím nastavení ovládacího prvku CreateUserWizard schválí nové účty. Můžete nakonfigurovat toto chování pomocí ovládacího prvku [ `DisableCreatedUser` vlastnost](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Tuto vlastnost nastavit na `True` není schválení nových uživatelských účtů.

> [!NOTE]
> Ve výchozím nastavení ovládacího prvku CreateUserWizard automaticky přihlásí nový uživatelský účet. Toto chování je závisí na ovládacího prvku [ `LoginCreatedUser` vlastnost](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Protože neschválených uživatelé nemůže přihlásit k webu, když `DisableCreatedUser` je `True` nový uživatelský účet není přihlášen na web, bez ohledu na hodnotu `LoginCreatedUser` vlastnost.


Při vytváření nových uživatelských účtů pomocí prostřednictvím kódu programu `Membership.CreateUser` metodu pro vytvoření neschválených uživatelský účet použijte jednu z přetížení, které přijímají nového uživatele `IsApproved` hodnotu vlastnosti jako vstupní parametr.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Krok 3: Schválení uživatelé kontrolou e-mailové adresy

Mnoho webů, které podporují uživatelské účty schvalovat nové uživatele, dokud se ověřit e-mailovou adresu, které budou zadané při registraci. Tento proces ověření se běžně používá zabránit robotů, odesílateli nevyžádané pošty a jiných ne'er-do-wells tak, jak vyžaduje jedinečný, ověřené e-mailovou adresu a přidá na další krok v procesu registrace. V tomto modelu zaregistruje nový uživatel se se odešlou e-mailovou zprávu, která obsahuje odkaz na stránku ověření. Když přejdete na odkaz uživatele případě budou přijatých e-mailu, a proto, že e-mailová adresa zadaná je platný. Stránku ověření je zodpovědná za schválení uživatele. To se může stát automaticky, a tím schválení každý uživatel, který dosáhne této stránky, nebo až poté, co uživatel poskytuje nějaké další informace, například [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Abychom vyhověli tento pracovní postup, je potřeba nejdřív aktualizovat stránku vytvoření účtu tak, aby noví uživatelé neschválených. Otevřete `EnhancedCreateUserWizard.aspx` stránku `Membership` složku a sady ovládacího prvku CreateUserWizard `DisableCreatedUser` vlastnost, která má `True`.

Dále je potřeba nakonfigurovat CreateUserWizard řízení odeslat e-mail pro nového uživatele s pokyny k ověření svého účtu. Konkrétně jsme bude obsahovat odkaz v e-mailu o `Verification.aspx` stránky (které jsme ještě k vytvoření) a předejte nového uživatele `UserId` prostřednictvím řetězec dotazu. `Verification.aspx` Stránku bude vyhledat zadaný uživatel a označit je schválena.

### <a name="sending-a-verification-email-to-new-users"></a>Odesílání e-mailu s ověření pro nové uživatele

Odeslat e-mail z ovládacího prvku CreateUserWizard, konfigurovat jeho `MailDefinition` vlastnost správně. Jak je popsáno v <a id="Tutorial13"> </a> [předchozí kurzu](recovering-and-changing-passwords-vb.md), změna hesla byla a PasswordRecovery ovládací prvky zahrnují [ `MailDefinition` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) to funguje stejným způsobem jako CreateUserWizard ovládacího prvku.

> [!NOTE]
> Použít `MailDefinition` vlastnost je třeba zadat doručení pošty možnosti v `Web.config`. Další informace najdete v části [odesílání e-mailu v ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Začněte vytvořením novou šablonu e-mailu s názvem `CreateUserWizard.txt` v `EmailTemplates` složky. Pro šablonu použijte následující text:

[!code-aspx[Main](unlocking-and-approving-user-accounts-vb/samples/sample3.aspx)]

Nastavte `MailDefinition`na `BodyFileName` vlastnost "~ / EmailTemplates/CreateUserWizard.txt" a jeho `Subject` vlastnost "Vítá vás Moje webu! Aktivujte svůj účet."

Všimněte si, že `CreateUserWizard.txt` obsahuje e-mailovou šablonu `<%VerificationUrl%>` zástupný symbol. To je, kdy adresu URL `Verification.aspx` stránky budou umístěny. CreateUserWizard automaticky nahrazuje `<%UserName%>` a `<%Password%>` zástupné symboly nový účet uživatelské jméno a heslo, ale neexistuje žádné předdefinované `<%VerificationUrl%>` zástupný symbol. Je potřeba ručně nahraďte adresu URL odpovídající ověření.

K tomu, vytvoření obslužné rutiny události pro CreateUserWizard [ `SendingMail` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) a přidejte následující kód:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample4.vb)]

`SendingMail` Událost aktivuje se po `CreatedUser` událostí, což znamená, že v době výše obslužné rutiny události provede nového uživatele účet již existuje. Jsme můžete získat přístup nového uživatele `UserId` hodnotu voláním `Membership.GetUser` předávání v případě metody `UserName` do ovládacího prvku CreateUserWizard. Dále je vytvořen ověření adresy URL. Příkaz `Request.Url.GetLeftPart(UriPartial.Authority)` vrátí `http://yourserver.com` v adrese URL; `Request.ApplicationPath` vrátí cestu, kde se zobrazuje aplikace. Ověřování adresy URL je pak definován jako `Verification.aspx?ID=userId`. Tyto dva řetězce jsou poté vytvoří spojením úplnou adresu URL. Nakonec tělo e-mailové zprávy (`e.Message.Body`) má všechny výskyty `<%VerificationUrl%>` nahradí úplnou adresu URL.

Net efekt je, že noví uživatelé neschválených, což znamená, že nemůžete přihlásit do lokality. Kromě toho se automaticky odešlou e-mail s odkazem na adresu URL ověření (viz obrázek 6).


[![Nový uživatel obdrží E-mail s odkazem na adresu URL, ověření](unlocking-and-approving-user-accounts-vb/_static/image17.png)](unlocking-and-approving-user-accounts-vb/_static/image16.png)

**Obrázek 6**: nový uživatel obdrží E-mail s odkazem na adresu URL ověření ([Kliknutím zobrazit obrázek v plné velikosti](unlocking-and-approving-user-accounts-vb/_static/image18.png))


> [!NOTE]
> Ovládací prvek CreateUserWizard výchozí CreateUserWizard krok zobrazí zprávu informující uživatele svůj účet byl vytvořen a zobrazí tlačítko Pokračovat. Po kliknutí na tento uživatel přejde na adresu URL zadanou v ovládacího prvku `ContinueDestinationPageUrl` vlastnost. CreateUserWizard v `EnhancedCreateUserWizard.aspx` je nakonfigurován pro odesílání nových uživatelů `~/Membership/AdditionalUserInfo.aspx`, který vyzve uživatele k jejich hometown, adresa URL domovské stránky a podpis. Protože tyto informace lze přidat pouze podle přihlášených uživatelů, má smysl k aktualizaci této vlastnosti uživatelům poslat zpět na domovskou stránku na webu (`~/Default.aspx`). Kromě toho `EnhancedCreateUserWizard.aspx` stránky nebo krok CreateUserWizard by měl být rozšířen informovat uživatele, že byl odeslán ověřovací e-mail a jejich účtu nebude aktivován, dokud se postupujte podle pokynů v této e-mailu. Tyto úpravy jako cvičení nechat pro čtečku.


### <a name="creating-the-verification-page"></a>Vytváření stránku ověření

Naše poslední je vytvořit `Verification.aspx` stránky. Přidejte tuto stránku do kořenové složky, jeho s přidružením `Site.master` stránky předlohy. Jak jsme krok s většinou předchozí stránky obsahu přidat na web, odeberte obsahu ovládací prvek, který odkazuje `LoginContent` ContentPlaceHolder tak, aby používala stránky obsahu stránky předlohy výchozí obsahu.

Přidání ovládacího prvku popisek webové `Verification.aspx` nastavte její `ID` k `StatusMessage` a vymažte jeho vlastnost text. Dále vytvořte `Page_Load` obslužné rutiny události a přidejte následující kód:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample5.vb)]

Hromadným ve výše uvedeném kódu ověří, zda ID uživatele prostřednictvím řetězec dotazu existuje, že se jedná o platný `Guid` hodnota a zda odkazuje na uživatelský účet. Pokud všechny z těchto kontrol úspěšně, uživatelský účet je schválen; jinak se zobrazí zpráva vhodný stav.

Na obrázku 7 vidíte `Verification.aspx` stránky po návštěvě prostřednictvím prohlížeče.


[![Nový uživatelský účet je nyní schválení](unlocking-and-approving-user-accounts-vb/_static/image20.png)](unlocking-and-approving-user-accounts-vb/_static/image19.png)

**Obrázek 7**: nový uživatelský účet je nyní schválení ([Kliknutím zobrazit obrázek v plné velikosti](unlocking-and-approving-user-accounts-vb/_static/image21.png))


## <a name="summary"></a>Souhrn

Všechny uživatelské účty členství mít dva stavy, které určují, jestli uživatel může přihlásit k webu: `IsLockedOut` a `IsApproved`. Obě tyto vlastnosti musí být `True` k přihlášení uživatele.

Uživatel je uzamčen stav se používá jako bezpečnostní opatření ke snížení pravděpodobnost, že se hacker rozdělení do lokality pomocí metody hrubou silou. Konkrétně Pokud je počet neplatných pokusů o přihlášení v rámci určité období je uzamčen uživatele. Tyto hranice jsou konfigurovat pomocí nastavení zprostředkovatele členství v `Web.config`.

Schválené stav se běžně používá jako prostředek se má noví uživatelé přihlásit, dokud některé akce vypršel. Možná web vyžaduje, aby nové účty nejprve schválení správce nebo jako jsme viděli v kroku 3 kontrolou e-mailové adresy.

Radostí programování!

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování...

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](recovering-and-changing-passwords-vb.md)
