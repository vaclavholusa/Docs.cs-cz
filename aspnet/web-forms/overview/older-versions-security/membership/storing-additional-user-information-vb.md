---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
title: Ukládání dalších informací o uživatelích (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu jsme tuto otázku odpovědět sestavením aplikace vyloženě návštěv. Přitom se podíváme na různé možnosti pro modeli...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: ee4b924e-8002-4dc3-819f-695fca1ff867
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 33e686cc3b977c6c740dfaf1057e1e399d5a298b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755308"
---
<a name="storing-additional-user-information-vb"></a>Ukládání dalších informací o uživatelích (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_VB.zip) nebo [stahovat PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_vb.pdf)

> V tomto kurzu jsme tuto otázku odpovědět sestavením aplikace vyloženě návštěv. Současně jsme se podívá na různé možnosti pro modelování informace o uživateli v databázi a poté zjistit, jak tato data přidružit uživatelské účty vytvořené v rámci rozhraní členství.


## <a name="introduction"></a>Úvod

ASP. . NET framework členství nabízí flexibilní rozhraní pro správu uživatelů. Toto rozhraní API členství zahrnuje metody ověřují se přihlašovací údaje, načítání informací o aktuálně přihlášeného uživatele, vytvoření nového uživatelského účtu a odstranění uživatelského účtu, mimo jiné. Každý uživatelský účet v rámci členství obsahuje pouze vlastnosti, které jsou nutné pro ověření pověření a provádění úlohy související s účtem základní uživatele. To svědčí metody a vlastnosti [ `MembershipUser` třída](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), který modeluje uživatelský účet v rámci členství. Tato třída obsahuje vlastnosti, jako je [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), a [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), a metody, jako jsou [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) a [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Aplikace často, třeba k ukládání dalších informací o uživatelích nejsou zahrnuty v rámci členství. Online prodejce může být například nutné chcete, aby každý uživatel uložit své přenosů a fakturační adresu, platební údaje, předvolby doručování a kontaktní telefonní číslo. Kromě toho jednotlivé objednávky v systému je přidružený určitého uživatelského účtu.

`MembershipUser` Třídy nezahrnuje vlastnosti, jako je `PhoneNumber` nebo `DeliveryPreferences` nebo `PastOrders`. Jak jsme sledovat uživatelské informace potřebné pro aplikace a jeho integrovat členství v rámci? V tomto kurzu jsme tuto otázku odpovědět sestavením aplikace vyloženě návštěv. Současně jsme se podívá na různé možnosti pro modelování informace o uživateli v databázi a poté zjistit, jak tato data přidružit uživatelské účty vytvořené v rámci rozhraní členství. Pusťme se do práce!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Krok 1: Vytvoření aplikace návštěv datového modelu

Existuje řada různých technik, které mohou být použity k zaznamenání informací o uživateli v databázi a přidružit ho k uživatelské účty vytvořené v rámci rozhraní členství. Aby bylo možné tyto postupy ukazují, budeme muset rozšířit kurz webové aplikace tak, že zachytí nějaký druh uživatelská data. (V současné době aplikace datový model obsahuje pouze aplikaci služby tabulky vyžadované `SqlMembershipProvider`.)

Vytvoříme aplikaci velmi jednoduché návštěv, ve kterém můžete ověřeného uživatele napište komentář. Kromě ukládání návštěv komentáře, můžeme povolit každému uživateli ukládat své domácí města, domovskou stránku a podpisu. Pokud je zadán, uživatele domácí města, domovskou stránku a podpis se zobrazí v každá zpráva, že není dostupný v knize návštěv.

### <a name="adding-theguestbookcommentstable"></a>Přidávání`GuestbookComments`tabulky

Aby bylo možné zachytit návštěv komentáře, musíme vytvořit tabulku databáze s názvem `GuestbookComments` , který má sloupce jako `CommentId`, `Subject`, `Body`, a `CommentDate`. Je také potřeba mít každý záznam `GuestbookComments` uživatele, který zanechal komentář odkaz na tabulku.

Chcete-li přidat tuto tabulku do databáze, přejít na Průzkumník databáze v sadě Visual Studio a k podrobnostem `SecurityTutorials` databáze. Klikněte pravým tlačítkem na složku tabulky a zvolte Přidat novou tabulku. Tím se zobrazí rozhraní, která umožňuje definovat sloupců nové tabulky.


[![Přidá novou tabulku SecurityTutorials databáze](storing-additional-user-information-vb/_static/image2.png)](storing-additional-user-information-vb/_static/image1.png)

**Obrázek 1**: Přidat novou tabulku `SecurityTutorials` databáze ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image3.png))


Dále definujte `GuestbookComments`na sloupce. Začněte přidáním sloupec s názvem `CommentId` typu `uniqueidentifier`. V tomto sloupci se jednoznačně identifikovat každou komentář v knize návštěv, tak zakažte `NULL` s a označte ji jako primární klíč v tabulce. Místo zadání hodnoty pro `CommentId` pole v každém `INSERT`, jsme můžete určit, že nový `uniqueidentifier` hodnoty by měly být automaticky generovány pro toto pole na `INSERT` nastavením výchozí hodnotu sloupce na `NEWID()`. Po přidání tohoto prvního pole, jeho označení jako primární klíč a nastavení jeho výchozí hodnotu, vaše obrazovka by měla vypadat podobně jako obrazovky je vidět na obrázku 2.


[![Přidejte primární sloupec s názvem CommentId](storing-additional-user-information-vb/_static/image5.png)](storing-additional-user-information-vb/_static/image4.png)

**Obrázek 2**: přidejte primární sloupec s názvem `CommentId` ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image6.png))


Dále přidejte sloupec s názvem `Subject` typu `nvarchar(50)` a sloupec s názvem `Body` typu `nvarchar(MAX)`, zákaz `NULL` s v obou sloupců. Pod přidat sloupec s názvem `CommentDate` typu `datetime`. Zakázat `NULL` s a nastavte `CommentDate` výchozí hodnotu ve sloupci `getdate()`.

Už jen zbývá přidat sloupec, který přidruží účet uživatele každý návštěv komentář. Jednou z možností je přidat sloupec s názvem `UserName` typu `nvarchar(256)`. Toto je vhodnou volbou při používání poskytovatele členství jiné než `SqlMembershipProvider`. Ale při použití `SqlMembershipProvider`, jak se v této řadě kurzů `UserName` sloupec v `aspnet_Users` tabulky nemusí být jedinečný. `aspnet_Users` Primárního klíče tabulky je `UserId` a je typu `uniqueidentifier`. Proto `GuestbookComments` tabulce musí sloupec s názvem `UserId` typu `uniqueidentifier` (Probíhá zakazování `NULL` hodnoty). Pokračujte a přidat tento sloupec.

> [!NOTE]
> Jak jsme probírali v [ *vytvoření schématu členství v SQL serveru* ](creating-the-membership-schema-in-sql-server-vb.md) výukový program, členství v rámci je navržená k umožnění více webových aplikací s různým uživatelským účtům sdílet stejné úložiště uživatelů. Dělá to tak, že dělení uživatelské účty do různých aplikací. A při každé uživatelské jméno se musí být jedinečný v rámci aplikace, lze použít stejné uživatelské jméno v různých aplikací pomocí stejné úložiště uživatele. Je složeného `UNIQUE` omezením v atributu `aspnet_Users` tabulky na `UserName` a `ApplicationId` pole, nikoli však na jenom `UserName` pole. V důsledku toho je možné, aspnet\_tabulky uživatelé mít dva (nebo více) záznamů se stejným `UserName` hodnotu. Existuje, ale `UNIQUE` omezení `aspnet_Users` tabulky `UserId` pole (protože představuje primární klíč). A `UNIQUE` omezení je důležité, protože bez něho jsme nejde vytvořit omezení cizího klíče mezi `GuestbookComments` a `aspnet_Users` tabulky.


Po přidání `UserId` sloupce, uložte kliknutím na ikonu Uložit na panelu nástrojů v tabulce. Pojmenujte novou tabulku `GuestbookComments`.

Máme jeden problém poslední věnovat se `GuestbookComments` tabulky: potřebujeme vytvořit [omezení cizího klíče](https://msdn.microsoft.com/library/ms175464.aspx) mezi `GuestbookComments.UserId` sloupce a `aspnet_Users.UserId` sloupce. K dosažení tohoto cíle, klikněte na ikonu vztah v panelu nástrojů můžete spustit dialogové okno vztahy cizího klíče. (Alternativně můžete spustit toto dialogové okno tak, že přejdete do nabídky Návrháře tabulky a zvolíte relace.)

Klikněte na tlačítko Přidat v levém dolním rohu dialogu vztahy cizího klíče. Tím se přidá nová omezení cizího klíče, i když ještě nutné definovat tabulek, které se účastní v relaci.


[![Ke správě omezení cizího klíče tabulky pomocí dialogového okna cizího klíče](storing-additional-user-information-vb/_static/image8.png)](storing-additional-user-information-vb/_static/image7.png)

**Obrázek 3**: cizí klíč relace dialogu umožňuje spravovat omezení cizího klíče tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image9.png))


Klikněte na ikonu tří teček v řádku "Tabulky a sloupce specifikace" na pravé straně. Tím se spustí dialogovém okně tabulky a sloupce, ze kterého lze zadat primární klíč tabulky a sloupce a sloupce cizího klíče z `GuestbookComments` tabulky. Zejména `aspnet_Users` a `UserId` jako primární klíč tabulky a sloupce, a `UserId` z `GuestbookComments` tabulce jako sloupec cizího klíče (viz obrázek 4). Po definování primární a cizí klíče tabulky a sloupce, klikněte na tlačítko OK se vraťte do dialogového okna vztahy cizího klíče.


[![Vytvoření cizího klíče omezení mezi aspnet_Users a GuesbookComments tabulek](storing-additional-user-information-vb/_static/image11.png)](storing-additional-user-information-vb/_static/image10.png)

**Obrázek 4**: zavést cizího klíče omezení `aspnet_Users` a `GuesbookComments` tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image12.png))


V tomto okamžiku se vytvořilo omezení cizího klíče. Přítomnost tohoto omezení zajišťuje [relační integrity](http://en.wikipedia.org/wiki/Referential_integrity) mezi dvěma tabulkami ve zaručující, že nikdy bude návštěv položka odkazuje na neexistující uživatelský účet. Ve výchozím omezení cizího klíče zakážete nadřazený záznam odstranit, pokud existují odpovídající podřízené záznamy. To znamená pokud uživatel provede jednu nebo více poznámek návštěv a pak jsme pokus o odstranění tohoto uživatelského účtu, odstranění se nezdaří, pokud jsou jako první smazány jeho návštěv komentáře.

Automaticky odstranit související podřízené záznamy při odstranění záznamu nadřazené lze nastavit omezení cizího klíče. Omezení cizího klíče jsme jinými slovy, můžete nastavit tak, aby uživatele návštěv položky se automaticky odstraní při odstranění svůj uživatelský účet. K tomu, rozbalte v části "INSERT a UPDATE specifikace" a "Odstranit pravidlo" vlastnost nastavit na sebe.


[![Konfigurace omezení cizího klíče k kaskádové odstranění](storing-additional-user-information-vb/_static/image14.png)](storing-additional-user-information-vb/_static/image13.png)

**Obrázek 5**: Nakonfigurujte omezení pro cizí klíč k kaskádová ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image15.png))


Uložit omezení cizího klíče, klikněte na tlačítko Zavřít ukončíte mimo vztahy cizího klíče. Pak klikněte na ikonu Uložit na panelu nástrojů uložte tabulku a tento vztah.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Ukládání Domovská, domovskou stránku a podpis uživatele

`GuestbookComments` Tabulka ukazuje, jak ukládat informace, které sdílí vztah jeden mnoho s uživatelskými účty. Protože každý uživatelský účet může mít libovolný počet komentářů, tento vztah je modelovaná vytvořením tabulky pro uložení sadu komentáře, která zahrnuje sloupce, odkazuje zpět všechny komentáře s určitým uživatelem. Při použití `SqlMembershipProvider`, tento odkaz se nejlépe naváže vytvořením sloupec s názvem `UserId` typu `uniqueidentifier` a omezení cizího klíče mezi tento sloupec a `aspnet_Users.UserId`.

Nyní potřebujeme přidružit každý uživatelský účet pro ukládání domácí města, domovskou stránku a podpis, který se zobrazí v jeho návštěv komentáře uživatele tři sloupce. Existuje řada způsobů, jak to provést:

- <strong>Přidat nové sloupce</strong><strong>`aspnet_Users`</strong><strong>nebo</strong><strong>`aspnet_Membership`</strong><strong>tabulky.</strong> Můžu by doporučujeme tento přístup, protože upravuje schéma používané `SqlMembershipProvider`. Toto rozhodnutí může vrátit haunt můžete snížit cestách. Například v co když, budoucí verze technologie ASP.NET používá jiný `SqlMembershipProvider` schématu. Microsoft může obsahovat nástroj pro migraci ASP.NET 2.0 `SqlMembershipProvider` dat na nové schéma, ale pokud jste upravili technologii ASP.NET 2.0 `SqlMembershipProvider` schématu, takový převod nemusí být možné.

- **Použití prostředí ASP. Profil rozhraní NET definuje vlastnost profilu pro domácí města, domovskou stránku a podpis.** Technologie ASP.NET obsahuje profil architektura, která je navržená k ukládání další uživatelská data. Jako jsou členství v rámci profilu framework propojitelnosti podle modelu poskytovatele. Rozhraní .NET Framework se dodává se `SqlProfileProvider` , uloží profilová data v databázi serveru SQL Server. Ve skutečnosti naše databáze již obsahuje tabulky používané `SqlProfileProvider` (`aspnet_Profile`), jak byl přidán, když jsme přidali, aplikační služby zpět [ *vytvoření schématu členství v SQL serveru* ](creating-the-membership-schema-in-sql-server-vb.md)kurzu.   
  Hlavní výhodou služby rozhraní framework profilu je, že umožňuje vývojářům definovat vlastnosti profilu v `Web.config` – musí být napsaný pro serializaci dat profilu do a z základnímu úložišti dat. žádný kód. Stručně řečeno je neuvěřitelně jednoduché definují sadu vlastností profilu a pracovat s nimi v kódu. Ale profil systému opustí mnoho dalších požadovaných, pokud jde o do správy verzí, takže pokud máte aplikaci, kde očekáváte, že nové vlastnosti specifické pro uživatele přidat na pozdější dobu, nebo existující aplikace do odebrat nebo změnit, a pak nemusí být v rámci profilu  nejlepší možností. Kromě toho `SqlProfileProvider` ukládá vlastnosti profilu, které vysoce Nenormalizovaná způsobem, takže další možné ke spouštění dotazů přímo na data profilu (třeba na, kolik uživatelů domácí města v New Yorku).   
  Další informace o rozhraní profilu najdete v části "Další čtení" na konci tohoto kurzu.

- <strong>Tyto tři sloupce přidat do nové tabulky v databázi a vytvořit vztah 1: 1 mezi tuto tabulku a</strong><strong>`aspnet_Users`</strong><strong>.</strong> Tento přístup vyžaduje trochu více práce než s použitím rozhraní framework profilu, ale nabízí maximální flexibilitu v tom, jak jsou modelovány vlastnosti další uživatele v databázi. Tato možnost, které používáme v tomto kurzu se.

Vytvoříme nové tabulky nazvané `UserProfiles` uložte domácí města, domovskou stránku a podpis pro každého uživatele. Klikněte pravým tlačítkem na složku tabulky v okně Průzkumník databáze a zvolte možnost vytvořit novou tabulku. Pojmenujte první sloupec `UserId` a nastavte její typ `uniqueidentifier`. Zakázat `NULL` hodnoty a označit jako primární klíč sloupec. V dalším kroku přidejte sloupce s názvem: `HomeTown` typu `nvarchar(50)`; `HomepageUrl` typu `nvarchar(100)`; a podpis typu `nvarchar(500)`. Každá z těchto tří sloupců může přijmout `NULL` hodnotu.


[![Vytvoření tabulky UserProfiles](storing-additional-user-information-vb/_static/image17.png)](storing-additional-user-information-vb/_static/image16.png)

**Obrázek 6**: vytvořit `UserProfiles` tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image18.png))


Uložte tabulku a pojmenujte ho `UserProfiles`. A konečně navázat omezení cizího klíče mezi `UserProfiles` tabulky `UserId` pole a `aspnet_Users.UserId` pole. Jako jsme to udělali s omezení cizího klíče mezi `GuestbookComments` a `aspnet_Users` tabulkám, toto omezení kaskádovitě přenést na odstranění. Protože `UserId` pole v `UserProfiles` je primární klíče, tím se zajistí, že bude existovat více než jeden záznam v `UserProfiles` tabulky pro každý uživatelský účet. Tento typ relace se označuje jako 1: 1.

Po vytvoření datového modelu, jsme ho můžete používat. V krocích 2 a 3 se podíváme na jak zobrazit a upravit domovského města, domovskou stránku a podpis informace aktuálně přihlášeného uživatele. V kroku 4 vytvoříme rozhraní pro ověření uživatelé posílat nové komentáře návštěv a zobrazení ty stávající.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Krok 2: Zobrazení Domovská, domovskou stránku a podpis uživatele

Existuje řada různých způsobů, jak povolit aktuálně přihlášenému uživateli zobrazit a upravit jeho domovského města, domovskou stránku a podpis informace. Můžeme ručně vytvořit uživatelské rozhraní pomocí textového pole a ovládací prvky popisku nebo jsme použít jeden z dat webové ovládací prvky, jako je například ovládací prvek DetailsView. K provedení databáze `SELECT` a `UPDATE` příkazy nám napsat ADO.NET kód ve třídě použití modelu code-behind naši stránku, nebo můžete také využívat deklarativní přístup s ovládacím prvkem SqlDataSource. V ideálním případě by naší aplikace obsahoval vrstvené architekturu, která jsme může buď vyvolat prostřednictvím kódu programu z třídy modelu code-behind na stránce nebo deklarativně pomocí ovládacího prvku ObjectDataSource.

Protože v této sérii kurzů se zaměřuje na ověřování pomocí formulářů, autorizace, uživatelských účtů a rolí, nesmí být vyčerpávající diskusi o tyto možnosti přístupu k datům různých nebo proč vrstvené architektury je upřednostňované nad provádění příkazů SQL přímo na stránce ASP.NET. Teď předvedu provede pomocí prvku DetailsView a SqlDataSource – možnost nejrychlejší a nejjednodušší – ale uvedenou koncepci lze použít jistě alternativní webové ovládací prvky a datová logikou přístupu. Další informace o práci s daty v ASP.NET, najdete v mé *[pracovat s daty v ASP.NET 2.0](../../data-access/index.md)* série kurzů.

Otevřít `AdditionalUserInfo.aspx` stránku `Membership` složky a přidat na stránku, nastavením jeho vlastnosti ID na ovládacím prvku DetailsView `UserProfile` a vymazání jeho `Width` a `Height` vlastnosti. Rozbalte ovládacím prvku DetailsView inteligentních značek a zvolte a vytvořte jeho vazbu nový ovládací prvek zdroje dat. Tím spustíte Průvodce konfigurací zdroje dat (viz obrázek 7). Prvním krokem žádostí o zadání typu zdrojového data. Protože jsme se chystáte připojit přímo `SecurityTutorials` databáze, zvolte ikonu databáze zadání `ID` jako `UserProfileDataSource`.


[![Přidat nový ovládací prvek SqlDataSource s názvem UserProfileDataSource](storing-additional-user-information-vb/_static/image20.png)](storing-additional-user-information-vb/_static/image19.png)

**Obrázek 7**: přidejte nový ovládací prvek SqlDataSource název `UserProfileDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image21.png))


Na další obrazovce zobrazí výzvu pro databáze, kterou chcete použít. Už jsme definovali připojovacího řetězce v `Web.config` pro `SecurityTutorials` databáze. Tento název připojovacího řetězce – `SecurityTutorialsConnectionString` – by měla být v rozevíracím seznamu. Vyberte tuto možnost a klikněte na tlačítko Další.


[![Z rozevíracího seznamu zvolte SecurityTutorialsConnectionString](storing-additional-user-information-vb/_static/image23.png)](storing-additional-user-information-vb/_static/image22.png)

**Obrázek 8**: Zvolte `SecurityTutorialsConnectionString` z rozevíracího seznamu ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image24.png))


Na následující obrazovce se zobrazí výzva k určení tabulky a sloupce do dotazu. Zvolte `UserProfiles` tabulky z rozevíracího seznamu a zkontrolovat všechny sloupce.


[![Převést zpět všechny sloupce z tabulky UserProfiles](storing-additional-user-information-vb/_static/image26.png)](storing-additional-user-information-vb/_static/image25.png)

**Obrázek 9**: přeneste zpět všechny sloupce `UserProfiles` tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image27.png))


Aktuální dotaz vrátí obrázek 9 *všechny* záznamů v `UserProfiles`, ale nás zajímá jenom v záznamu aktuálně přihlášeného uživatele. Chcete-li přidat `WHERE` klauzule, klikněte na tlačítko `WHERE` tlačítko Přidat zobrazíte `WHERE` klauzule dialogové okno (viz obrázek 10). Tady můžete vybrat sloupec, který se filtrovat, operátor a zdroj daného parametru filtru. Vyberte `UserId` jako sloupce a "=" jako operátor.

Bohužel neexistuje žádný zdroj integrované parametr vrátit aktuálně přihlášeného uživatele `UserId` hodnotu. Budeme muset zkopírovat tuto hodnotu prostřednictvím kódu programu. Proto nastavte zdroj rozevíracího seznamu na "Žádný" klikněte na tlačítko Přidat parametr přidat a klikněte na tlačítko OK.


[![Přidání parametru filtr na sloupec UserId](storing-additional-user-information-vb/_static/image29.png)](storing-additional-user-information-vb/_static/image28.png)

**Obrázek 10**: Přidání parametru filtru na `UserId` sloupec ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image30.png))


Po kliknutí na tlačítko OK se vrátíte na obrazovku, je znázorněno na obrázku 9. Tentokrát ale bude příkaz jazyka SQL v dolní části obrazovky by měl obsahovat `WHERE` klauzuli. Klikněte na tlačítko Další přejděte obrazovku "Testovat dotaz". Tady můžete spustit dotaz a zobrazit výsledky. Kliknutím na Dokončit dokončíte průvodce.

Po dokončení Průvodce konfigurací zdroje dat, vytvoří Visual Studio SqlDataSource ovládacího prvku v závislosti na nastaveních určených v průvodci. Kromě toho ručně přidá BoundFields na ovládacím prvku DetailsView. pro každý sloupec vrácené ovládacím prvkem SqlDataSource `SelectCommand`. Není nutné zobrazíte `UserId` pole v ovládacím prvku DetailsView, protože uživatel není potřeba znát tuto hodnotu. Můžete odebrat toto pole přímo v ovládacím prvku DetailsView deklarativní nebo kliknutím na tlačítko "Upravit pole" propojení z jeho inteligentních značek.

Na stránce deklarativní v tuto chvíli by měl vypadat nějak takto:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample1.aspx)]

Budeme potřebovat programově nastavit ovládacím prvkem SqlDataSource `UserId` parametr pro aktuálně přihlášeného uživatele `UserId` předtím, než je vybraná data. Toho můžete docílit tak, že vytvoříte obslužnou rutinu události pro ovládacím prvkem SqlDataSource `Selecting` událostí a přidáním následujícího kódu existuje:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample2.vb)]

Výše uvedený kód spustí získat odkaz na aktuálně přihlášeného uživatele voláním `Membership` třídy `GetUser` metody. Tím se vrátí `MembershipUser` objekt, jehož `ProviderUserKey` obsahuje vlastnost `UserId`. `UserId` Hodnota je poté přiřazen ve třídě SqlDataSource `@UserId` parametru.

> [!NOTE]
> `Membership.GetUser()` Metoda vrátí informace o aktuálně přihlášeného uživatele. Pokud anonymního uživatele je na stránce, vrátí hodnotu `Nothing`. V takovém případě se to bude mít `NullReferenceException` na následující řádek kódu při pokusu o čtení `ProviderUserKey` vlastnost. Samozřejmě, nemáme se starat o `Membership.GetUser()` vrácení ustanovení `AdditionalUserInfo.aspx` stránce, protože jsme nakonfigurovali autorizace adres URL v předchozím kurzu tak, aby jenom ověření uživatelé můžou přistupovat k prostředkům ASP.NET v této složce. Pokud potřebujete přístup k informacím o aktuálně přihlášeného uživatele na stránce, kde je povolen anonymní přístup, ujistěte se, že zkontroluje, jestli `MembershipUser` objekt vrácený z `GetUser()` metoda není nic před odkazování na její vlastnosti.


Pokud navštívíte `AdditionalUserInfo.aspx` stránky prostřednictvím prohlížeče se zobrazí prázdnou stránku, protože musíme ještě přidat všechny řádky `UserProfiles` tabulky. V kroku 6 se podíváme na tom, jak přizpůsobit ovládacím prvku CreateUserWizard automaticky přidáte nový řádek `UserProfiles` tabulky, když se vytvoří nový uživatelský účet. Prozatím se však budeme muset ručně vytvořit záznam v tabulce.

Přejděte do Průzkumníku databází v sadě Visual Studio a rozbalte složku tabulky. Klikněte pravým tlačítkem na `aspnet_Users` tabulky a zvolte "Zobrazit Data tabulky" Pokud chcete zobrazit záznamy v tabulce; stejnou věc udělat `UserProfiles` tabulky. Obrázek 11 zobrazí tyto výsledky při svisle vedle sebe. V databázi nejsou aktuálně `aspnet_Users` záznamy pro Bruce Fred a Tito, ale žádné záznamy v `UserProfiles` tabulky.


[![Zobrazí se obsah aspnet_Users a UserProfiles tabulek](storing-additional-user-information-vb/_static/image32.png)](storing-additional-user-information-vb/_static/image31.png)

**Obrázek 11**: obsah `aspnet_Users` a `UserProfiles` tabulky se zobrazí ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image33.png))


Přidání nového záznamu `UserProfiles` tabulky ručním zadáním hodnoty pro `HomeTown`, `HomepageUrl`, a `Signature` pole. Nejjednodušší způsob, jak získat platný `UserId` hodnotu v novém `UserProfiles` záznamu je výběr `UserId` pole z určitého uživatelského účtu v `aspnet_Users` tabulky a zkopírujte a vložte ho do `UserId` pole `UserProfiles`. Obrázek 12 se zobrazí `UserProfiles` tabulky po přidání nového záznamu pro Bruce.


[![Záznam byl přidán do UserProfiles pro Bruce](storing-additional-user-information-vb/_static/image35.png)](storing-additional-user-information-vb/_static/image34.png)

**Obrázek 12**: záznam A přidal do `UserProfiles` pro Bruce ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image36.png))


Vraťte se `AdditionalUserInfo.aspx page`, přihlášen jako Bruce. Jak ukazuje obrázek 13, se zobrazují Bruce jeho nastavení.


[![Aktuálně návštěvě uživatele se zobrazí jeho nastavení](storing-additional-user-information-vb/_static/image38.png)](storing-additional-user-information-vb/_static/image37.png)

**Obrázek 13**: aktuálně návštěvě uživatele se zobrazí jeho nastavení ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image39.png))


> [!NOTE]
> Přejít dopředu a ručně přidávat záznamy `UserProfiles` tabulky pro každého uživatele se členstvím. V kroku 6 se podíváme na tom, jak přizpůsobit ovládacím prvku CreateUserWizard automaticky přidáte nový řádek `UserProfiles` tabulky, když se vytvoří nový uživatelský účet.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Krok 3: Které uživateli umožňují upravit jeho Domovská stránka městě, domovskou stránku a podpis

V tomto okamžiku můžete zobrazit aktuálně přihlášeného uživatele jejich domovské města, domovskou stránku a podpis nastavení, ale ještě je nemohou upravovat. Umožňuje aktualizovat ovládacím prvku DetailsView tak, aby data lze upravovat.

První věc musíme udělat, je přidat `UpdateCommand` pro ovládacím prvkem SqlDataSource zadání `UPDATE` příkazu ke spuštění a jeho odpovídající parametry. Vyberte ve třídě SqlDataSource a v okně Vlastnosti klikněte na symbol tří teček vedle vlastnosti UpdateQuery zobrazíte dialogové okno Editor příkazů a parametrů. Zadejte následující `UPDATE` příkaz do textového pole:

[!code-sql[Main](storing-additional-user-information-vb/samples/sample3.sql)]

Klikněte na tlačítko "Aktualizovat parametry", který vytvoří parametr v ovládacím prvkem SqlDataSource `UpdateParameters` kolekce pro každý z parametrů v `UPDATE` příkazu. Ponechte na žádný zdroj pro všechny sady parametrů a klikněte na tlačítko OK se vyplňte dialogové okno.


[![Zadejte vlastnost UpdateCommand ve třídě SqlDataSource a UpdateParameters](storing-additional-user-information-vb/_static/image41.png)](storing-additional-user-information-vb/_static/image40.png)

**Obrázek 14**: zadejte ve třídě SqlDataSource `UpdateCommand` a `UpdateParameters` ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image42.png))


Z důvodu dodatky jsme provedli SqlDataSource ovládacího prvku DetailsView ovládací prvek nyní podporuje úpravy. V ovládacím prvku DetailsView inteligentních značek zaškrtněte políčko "Povolit úpravy". Tento postup přidá ovládacího prvku CommandField `Fields` kolekce s jeho `ShowEditButton` nastavenou na hodnotu True. Tím zkopírujete tlačítko pro úpravy v ovládacím prvku DetailsView je zobrazen v režimu jen pro čtení a aktualizace a tlačítka Storno při zobrazení v režimu úprav. Místo by uživatel musel kliknout na upravit, ale můžeme nechat vykreslování prvku DetailsView. ve stavu "vždy upravitelné" tak, že nastavíte ovládacím prvku DetailsView [ `DefaultMode` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) k `Edit`.

S těmito změnami prvku DetailsView deklarativní by měl vypadat nějak takto:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample4.aspx)]

Poznámka: Přidání CommandField a `DefaultMode` vlastnost.

Pokračujte a otestovat tuto stránku prostřednictvím prohlížeče. Při návštěvě jako uživatel, který nemá odpovídající záznam v `UserProfiles`, nastavení uživatele se zobrazí v upravitelné rozhraní.


[![Vykreslí upravitelné rozhraní ovládacím prvku DetailsView.](storing-additional-user-information-vb/_static/image44.png)](storing-additional-user-information-vb/_static/image43.png)

**Obrázek 15**: ovládacím prvku DetailsView. vykreslí upravitelné rozhraní ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image45.png))


Zkuste změna hodnoty a kliknutím na tlačítko Aktualizovat. Zdá se, jak je, že se nic nestalo. Je zpětné volání a hodnoty se uloží do databáze, ale neexistuje žádný vizuální zpětnou vazbu, ke které došlo uložit.

Chcete-li to napravit, vraťte se do sady Visual Studio a přidejte ovládací prvek popisek nad ovládacím prvku DetailsView. Nastavte jeho `ID` k `SettingsUpdatedMessage`, jeho `Text` vlastnost "vaše nastavení bylo aktualizováno," a jeho `Visible` a `EnableViewState` vlastností `False`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample5.aspx)]

Potřebujeme zobrazíte `SettingsUpdatedMessage` popisek pokaždé, když se aktualizuje ovládacím prvku DetailsView. K tomu vytvořit obslužnou rutinu události pro ovládacím prvku DetailsView `ItemUpdated` událostí a přidejte následující kód:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample6.vb)]

Vraťte se `AdditionalUserInfo.aspx` stránce prostřednictvím prohlížeče a aktualizovat data. Tentokrát, zobrazí se užitečné stavovou zprávu.


[![Krátký zpráva se zobrazí při nastavení jsou aktualizovány](storing-additional-user-information-vb/_static/image47.png)](storing-additional-user-information-vb/_static/image46.png)

**Obrázek 16**: při aktualizaci nastavení, zobrazí se zpráva A krátký ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image48.png))


> [!NOTE]
> Ovládacím prvku DetailsView uživatele úpravy ponechá rozhraní být požadovaného mnoho dalších. Používá standardní velikosti textová pole, ale podpis pole by měl pravděpodobně být ve víceřádkovém textovém poli. RegularExpressionValidator by měla sloužit k zajištění, že adresa URL domovské stránky, je-li zadán, začne řetězcem "http://" nebo "https://". Kromě toho od ovládacím prvku DetailsView. ovládací prvek má jeho `DefaultMode` nastavenou na `Edit`, tlačítko Storno nedělá. Ji by měl buď odebrat nebo po kliknutí na přesměruje uživatele na jinou stránku (například `~/Default.aspx`). Tato vylepšení opuštění jako cvičení pro čtečku.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Přidání odkazu`AdditionalUserInfo.aspx`stránky na stránce předlohy

V současné době webu neposkytuje žádné odkazy na `AdditionalUserInfo.aspx` stránky. Jediný způsob, jak k němu přistoupit je zadejte adresu URL stránky přímo do panelu Adresa prohlížeče. Přidáme odkaz na tuto stránku `Site.master` stránky předlohy.

Připomínáme, že hlavní stránka obsahuje ovládacího prvku LoginView Web v jeho `LoginContent` ContentPlaceHolder zobrazující různých značek pro návštěvníky ověřený a anonymní. Aktualizovat ovládacího prvku LoginView `LoggedInTemplate` zahrnout odkaz `AdditionalUserInfo.aspx` stránky. Po provedení těchto změn LoginView deklarativní ovládacího prvku by měl vypadat nějak takto:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample7.aspx)]

Poznámka: přidání `lnkUpdateSettings` ovládacího prvku hypertextový odkaz `LoggedInTemplate`. S tímto odkazem na místě ověřeným uživatelům rychle přejít na stránku lze zobrazit a upravit domovského města, domovskou stránku a podpis nastavení.

## <a name="step-4-adding-new-guestbook-comments"></a>Krok 4: Přidání nových komentářů návštěv

`Guestbook.aspx` Je stránka, kde můžete ověřeným uživatelům zobrazit návštěv a napište komentář. Začněme s vytvářením rozhraní pro přidání nového komentáře návštěv.

Otevřít `Guestbook.aspx` stránce v sadě Visual Studio a vytvořit uživatelské rozhraní, který se skládá ze dvou ovládacích prvků textového pole, jeden pro nový komentář předmětu a jeden pro jeho tělo. Nastavení prvního textového pole ovládacího prvku `ID` vlastnost `Subject` a jeho `Columns` vlastnost 40; nastavit druhé `ID` k `Body`, jeho `TextMode` k `MultiLine`a jeho `Width` a `Rows` Vlastnosti "95 %", 8, v uvedeném pořadí. K dokončení uživatelského rozhraní, přidejte ovládací prvek tlačítko Web s názvem `PostCommentButton` a nastavte jeho `Text` vlastnost "Napsat svůj komentář".

Protože každý návštěv komentář vyžaduje předmět a text, přidáte RequiredFieldValidator pro každou z textových polí. Nastavte `ValidationGroup` vlastnosti těchto ovládacích prvků na "EnterComment"; nastavte `PostCommentButton` ovládacího prvku `ValidationGroup` vlastnost "EnterComment". Další informace o ASP. Ovládací prvky ověřování NET společnosti, prohlédněte si [ověřování formuláře v technologii ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [rozbor validačních ovládacích prvků v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)a [kurzu ovládací prvky serveru ověřování](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) na [W3Schools](http://www.w3schools.com/).

Po vytvoření uživatelského rozhraní na stránce deklarativní by měl vypadat přibližně takto:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample8.aspx)]

Dokončení uživatelského rozhraní, naše dalším krokem je vložení nového záznamu do `GuestbookComments` tabulky, když `PostCommentButton` dojde ke kliknutí na. To lze provést různými způsoby: jsme napsali kód, ADO.NET na tlačítku `Click` obslužné rutiny události; jsme můžete přidat ovládací prvek SqlDataSource na stránku, konfigurovat jeho `InsertCommand`a poté zavolejte jeho `Insert` metody z `Click` událostí Obslužná rutina; nebo může vytváříme střední vrstvy, která je zodpovědná za vkládání nových komentářů návštěv a vyvolání této funkce `Click` obslužné rutiny události. Protože jsme se podívali na použití SqlDataSource v kroku 3, použijeme kódu ADO.NET tady.

> [!NOTE]
> Třídy rozhraní ADO.NET pro programově přístup k datům z databáze Microsoft SQL serveru se nacházejí v `System.Data.SqlClient` oboru názvů. Budete muset tento obor názvů naimportujte třída použití modelu code-behind na stránce (například `Imports System.Data.SqlClient`).


Vytvořte obslužnou rutinu události pro `PostCommentButton`společnosti `Click` událostí a přidejte následující kód:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample9.vb)]

`Click` Spustí obslužnou rutinu události tak, že zkontrolujete, že uživatelský data nejsou platná. Pokud není, obslužná rutina události ukončí před vložení záznamu. Za předpokladu, že platnost zadaných dat aktuálně přihlášeného uživatele `UserId` hodnota je načtena a uložena v `currentUserId` místní proměnné. Tato hodnota je potřeba, proto jsme musí zadat `UserId` hodnotu, pokud vložení záznamu do `GuestbookComments`.

Pod připojovacího řetězce pro `SecurityTutorials` databáze je načten z `Web.config` a `INSERT` zadaný příkaz jazyka SQL. A `SqlConnection` objekt je pak vytvořit a otevřít. Další, `SqlCommand` objekt je vytvořen a hodnoty pro parametry používané `INSERT` dotazu jsou přiřazeny. `INSERT` Poté je proveden příkaz a připojení ukončeno. Na konci obslužná rutina události `Subject` a `Body` textová pole `Text` vlastnosti jsou odstraněné tak, aby uživatele hodnoty se zachová pro zpětné volání.

Pokračujte a otestovat na této stránce v prohlížeči. Vzhledem k tomu, že se tato stránka `Membership` složky není přístupná pro anonymní návštěvníky. Proto je potřeba nejdřív přihlásit (Pokud nemáte). Zadejte hodnotu do `Subject` a `Body` textová pole a klikněte na tlačítko `PostCommentButton` tlačítko. To způsobí, že nový záznam přidat do `GuestbookComments`. Na zpětné volání předmět a text, který jste zadali vymažou z textových polí.

Po kliknutí `PostCommentButton` tlačítko zde není žádný vizuální zpětnou vazbu, který byl komentář přidán do návštěv. Stále potřebujeme aktualizovat tuto stránku a zobrazovat existující návštěv komentáře, které budeme provádět v kroku 5. Jakmile jsme to udělat, jen přidat komentář se zobrazí v seznamu komentářů, zajištění adekvátní vizuální zpětnou vazbu. Teď, potvrďte, že byla uložena komentář návštěv prozkoumáním obsah `GuestbookComments` tabulky.

Obrázek 17 zobrazí obsah `GuestbookComments` tabulky po zbývá dvě komentáře.


[![Zobrazí se návštěv komentáře v tabulce GuestbookComments](storing-additional-user-information-vb/_static/image50.png)](storing-additional-user-information-vb/_static/image49.png)

**Obrázek 17**: Zobrazí se návštěv komentáře v `GuestbookComments` tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image51.png))


> [!NOTE]
> Pokud se uživatel pokusí vložit návštěv komentář, který obsahuje potenciálně nebezpečné – například kód HTML – ASP.NET vyvolá `HttpRequestValidationException`. Další informace o této výjimky, proč je vyvolána, a jak povolit uživatelům odesílat potenciálně nebezpečné hodnoty, najdete [dokument White Paper žádost o ověření](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>Krok 5: Výpis existující komentáře návštěv

Kromě zanechání komentáře, může uživatel navštívit `Guestbook.aspx` stránka také měl zobrazit existující komentáře návštěv. Chcete-li to provést, přidejte ovládací prvek ListView s názvem `CommentList` do dolní části stránky.

> [!NOTE]
> Ovládací prvek ListView je nová technologie ASP.NET, verze 3.5. Je určená k zobrazení seznamu položek ve vysoce přizpůsobitelné a flexibilní rozložení, ale stále nabízet integrované úprava, vkládání, odstraňování, stránkování a řazení funkce, jako jsou prvku GridView. Pokud používáte technologii ASP.NET 2.0, musíte místo toho pomocí ovládacího prvku DataList nebo Repeater. Další informace o použití ListView, naleznete v tématu [Scott Guthrie](https://weblogs.asp.net/scottgu/)na blogu, [asp: ListView ovládací prvek](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)a my článku [zobrazení dat pomocí ovládacího prvku ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Otevřít inteligentní značky prvku ListView a z rozevíracího seznamu zvolit zdroj dat vazbu ovládacího prvku na nový zdroj dat. Jak jsme viděli v kroku 2, tím spustíte Průvodce konfigurací zdroje dat. Vyberte ikonu databáze, název výsledný SqlDataSource `CommentsDataSource`a klikněte na tlačítko OK. V dalším kroku vyberte `SecurityTutorialsConnectionString` připojovací řetězec z rozevíracího seznamu a klikněte na tlačítko Další.

V tomto okamžiku v kroku 2 jsme zadali data dotazu podle výběru `UserProfiles` tabulky z rozevíracího seznamu a vyberte sloupce, které chcete vrátit (vrátit zpět k obrázek 9). Tentokrát, ale chceme vytvořit příkaz SQL, který si vyžádá zpět nejen záznamy ze `GuestbookComments`, ale také komentátor domácí města, domovskou stránku, podpisu a uživatelské jméno. Proto vyberte přepínač "Zadejte vlastní příkaz SQL nebo uloženou proceduru" a klikněte na tlačítko Další.

Tím se otevře na obrazovce "Definovat vlastní příkazy nebo uložené procedury". Kliknutím na tlačítko Tvůrce dotazů graficky sestavení dotazu. Tvůrce dotazů spustí s výzvou k určení, které chceme dotaz z tabulky. Vyberte `GuestbookComments`, `UserProfiles`, a `aspnet_Users` tabulky a klikněte na tlačítko OK. Tím přidáte všechny tři tabulky na návrhovou plochu. Protože existují omezení cizího klíče mezi `GuestbookComments`, `UserProfiles`, a `aspnet_Users` tabulky, Tvůrce dotazů automaticky `JOIN` s těchto tabulek.

Už jen zbývá určit sloupce, které chcete vrátit. Z `GuestbookComments` tabulce vyberte `Subject`, `Body`, a `CommentDate` sloupce; vrátit `HomeTown`, `HomepageUrl`, a `Signature` sloupce z `UserProfiles` tabulky; a vrátit `UserName` z `aspnet_Users`. Přidejte také "`ORDER BY CommentDate DESC`" na konci `SELECT` dotaz tak, aby nejnovější příspěvky jsou vrácena jako první. Po provedení tento výběr, vypadat podobně jako na snímek v 18 obrázek obrazovky rozhraní Tvůrce dotazů.


[![Constructed dotaz spojí GuestbookComments, UserProfiles a aspnet_Users tabulky](storing-additional-user-information-vb/_static/image53.png)](storing-additional-user-information-vb/_static/image52.png)

**Obrázek 18**: dotaz vytvořený `JOIN` s `GuestbookComments`, `UserProfiles`, a `aspnet_Users` tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image54.png))


Kliknutím na OK zavřete okno editoru dotazů a návrat na obrazovku "Definovat vlastní příkazy nebo uložené procedury". Klikněte na tlačítko vedle záloh na obrazovku "Testovat dotaz", kde můžete zobrazit výsledky dotazu po kliknutí na tlačítko Testovat dotaz. Jakmile budete připraveni, klikněte na tlačítko Dokončit dokončete průvodce pro zdroj dat nakonfigurovat.

Když jsme dokončili Průvodce konfigurace zdroje dat v kroku 2, přiřazeném ovládacím prvku DetailsView `Fields` kolekce bylo aktualizováno, aby zahrnovalo Vlastnost BoundField pro každý sloupec vrácený `SelectCommand`. ListView, ale zůstávají beze změn; stále potřebujeme definovat jeho rozložení. Rozložení ListView lze sestavit ručně prostřednictvím jeho deklarativním označení nebo z možnosti "ListView konfigurace" v jeho inteligentních značek. Můžu obvykle raději ručně definování značky, ale pomocí libovolné metody je nejvíce přirozené pro vás.

Můžu skončila pomocí následujících `LayoutTemplate`, `ItemTemplate`, a `ItemSeparatorTemplate` pro ovládací prvek ListView:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample10.aspx)]

`LayoutTemplate` Definuje kód, protože ho vygeneroval ovládacího prvku, zatímco `ItemTemplate` vykreslí každé položky vrácené ovládacím prvkem SqlDataSource. `ItemTemplate`Výsledný kód je umístěn v `LayoutTemplate`společnosti `itemPlaceholder` ovládacího prvku. Kromě `itemPlaceholder`, `LayoutTemplate` obsahuje ovládací prvek DataPager, který omezuje ListView zobrazuje jenom 10 návštěv komentáře na jedné stránce (výchozí) a vykreslí je stránkovací rozhraní.

Moje `ItemTemplate` zobrazí subjekt každý návštěv komentář `<h4>` element s text nacházející se pod předmět. Mějte na paměti, že syntaxe pro zobrazení textu trvá dat vrácených `Eval("Body")` příkaz vázání dat, převede ho na řetězec a nahradí konců řádků s `<br />` elementu. Chcete-li zobrazit konce řádků zadaný při odesílání komentáře, protože prázdné znaky je ignorován v HTML je potřeba tento převod. Podpis uživatele se zobrazí pod text kurzívou, za nímž následuje uživatele domácí města, odkaz na jeho domovské stránky, datum a čas, který byl proveden komentář a uživatelské jméno osoby, který zanechal komentář.

Pokud chcete zobrazit stránku prostřednictvím prohlížeče chvíli trvat. Měli byste vidět poznámky, které jste přidali do návštěv v kroku 5, tady zobrazí.


[![Guestbook.aspx pak zobrazí návštěv komentáře](storing-additional-user-information-vb/_static/image56.png)](storing-additional-user-information-vb/_static/image55.png)

**Obrázek 19**: `Guestbook.aspx` se teď zobrazují poznámky návštěv ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image57.png))


Zkuste přidat nový komentář návštěv. Po kliknutí `PostCommentButton` tlačítko stránce odešle zpět a přidá komentář do databáze, ale ovládací prvek ListView se aktualizuje a zobrazí nový komentář. To je možné vyřešit buď:

- Aktualizuje `PostCommentButton` tlačítka `Click` obslužná rutina události tak, že vyvolá ovládacího prvku ListView `DataBind()` metoda po vložení nového komentáře do databáze, nebo
- Nastavení ovládacího prvku ListView `EnableViewState` vlastnost `False`. Tento přístup funguje, protože tím, že zakážete stav zobrazení ovládacího prvku, třeba rebind na podkladová data při každém postbacku.

Ke stažení z tohoto kurzu na webu kurz ukazuje obě tyto metody. Ovládací prvek ListView `EnableViewState` vlastnost `False` a je k dispozici v kódu potřeby prostřednictvím kódu programu znovu připojit data, která mají ListView `Click` obslužná rutina události, ale je zakomentovaný.

> [!NOTE]
> Aktuálně `AdditionalUserInfo.aspx` stránky umožňuje uživateli zobrazit a upravit domovského města, domovskou stránku a podpis nastavení. Může být dobré si aktualizovat `AdditionalUserInfo.aspx` zobrazíte přihlášeného uživatele návštěv komentáře. To znamená, kromě přezkoumání a úprava její informace, může uživatel navštívit `AdditionalUserInfo.aspx` stránku, abyste viděli, jaké návštěv komentáře, které se provádí v minulosti. Můžu ponechte toto cvičení pro dotčené čtečku.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Krok 6: Přizpůsobení ovládacího prvku CreateUserWizard zahrnout rozhraní pro Domovská, domovskou stránku a podpis

`SELECT` Dotazu použitého `Guestbook.aspx` stránce používá `INNER JOIN` kombinování souvisejících záznamů mimo `GuestbookComments`, `UserProfiles`, a `aspnet_Users` tabulky. Pokud uživatel, který nemá žádný záznam v `UserProfiles` bude návštěv připomínky, komentáře se nezobrazí ListView, protože `INNER JOIN` vrátí jenom `GuestbookComments` záznamů, pokud jsou k dispozici odpovídající záznamy v `UserProfiles` a `aspnet_Users`. A jak jsme viděli v kroku 3, pokud uživatel nemá záznamu `UserProfiles` si nemůžete zobrazit nebo upravit jeho nastavení v `AdditionalUserInfo.aspx` stránky.

Needless znamená kvůli náš návrh rozhodnutí je důležité, aby měly všechny uživatelské účty v systému členství odpovídající záznam v `UserProfiles` tabulky. Rádi bychom je pro odpovídající záznam přidat do `UserProfiles` vždy, když se vytvoří nový členský účet uživatele prostřednictvím CreateUserWizard.

Jak je popsáno v [ *vytváření uživatelských účtů* ](creating-user-accounts-vb.md) kurz, po vytvoření nového uživatelského účtu členství v ovládacím prvku CreateUserWizard vyvolá jeho [ `CreatedUser` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Abychom mohli vytvořit obslužnou rutinu události pro tuto událost, získejte ID uživatele pro uživatele nově vytvořené a vložte záznam do `UserProfiles` tabulky s použitím výchozích hodnot pro `HomeTown`, `HomepageUrl`, a `Signature` sloupce. A co víc je možné zobrazit výzvu uživateli pro tyto hodnoty úpravou ovládacího prvku CreateUserWizard rozhraní zahrnout další textová pole.

Nejprve se podíváme na to, jak přidat nový řádek `UserProfiles` tabulku v `CreatedUser` obslužné rutiny události s výchozími hodnotami. Pod uvidíme, jak přizpůsobit aplikaci prvku CreateUserWizard uživatelské rozhraní pro zahrnutí polí další formuláře ke shromažďování domácí města, domovskou stránku a podpis nového uživatele.

### <a name="adding-a-default-row-touserprofiles"></a>Přidání výchozí řádek`UserProfiles`

V [ *vytváření uživatelských účtů* ](creating-user-accounts-vb.md) kurzu jsme přidali ovládacím prvku CreateUserWizard k `CreatingUserAccounts.aspx` stránku `Membership` složky. Abyste měli CreateUserWizard ovládacího prvku, přidejte záznam do `UserProfiles` tabulky při vytváření uživatelských účtů, musíme aktualizovat funkce ovládacího prvku CreateUserWizard. Místo provedení těchto změn na `CreatingUserAccounts.aspx` stránce, můžeme místo toho přidejte nového ovládacího prvku CreateUserWizard k `EnhancedCreateUserWizard.aspx` stránce a provést změny pro účely tohoto kurzu existuje.

Otevřít `EnhancedCreateUserWizard.aspx` stránce v sadě Visual Studio a přetáhněte z panelu nástrojů na stránku ovládacím prvku CreateUserWizard. Nastavení ovládacího prvku CreateUserWizard `ID` vlastnost `NewUserWizard`. Jak jsme probírali v [ *vytváření uživatelských účtů* ](creating-user-accounts-vb.md) kurz, CreateUserWizard výchozí uživatelské rozhraní výzvy návštěvníka potřebné informace. Jakmile tyto informace není zadaný, ovládací prvek interně vytvoří nový uživatelský účet v rámci členství bez nám museli psát jediný řádek kódu.

Počet událostí ovládacím prvku CreateUserWizard vyvolá během jejího pracovního postupu. Poté, co návštěvník poskytuje informace o požadavku a formulář odešle, ovládacím prvku CreateUserWizard zpočátku aktivuje její [ `CreatingUser` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Pokud dojde k potížím během procesu vytvoření [ `CreateUserError` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) se aktivuje; ale, pokud je uživatel vytvořen úspěšně, pak bude [ `CreatedUser` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) je vyvolána. V [ *vytváření uživatelských účtů* ](creating-user-accounts-vb.md) kurzu jsme vytvořili obslužnou rutinu události pro `CreatingUser` události a ujistěte se, že zadané uživatelské jméno neobsahovala počáteční nebo koncové mezery a který uživatelské jméno neobjevil kdekoli v hesle.

Chcete-li přidat řádek v `UserProfiles` tabulky pro nově vytvořené uživatelem, potřebujeme vytvořit obslužnou rutinu události pro `CreatedUser` událostí. Časem `CreatedUser` událost se aktivuje, uživatelský účet už byl vytvořen v rámci členství, umožňuje nám k načtení hodnoty ID uživatele účtu.

Vytvořte obslužnou rutinu události pro `NewUserWizard`společnosti `CreatedUser` událostí a přidejte následující kód:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample11.vb)]

Výše uvedené kód bytostí načtením UserId právě přidaný uživatelský účet. To lze provést pomocí `Membership.GetUser(username)` metoda vrací informace o konkrétní uživatele a poté použijete `ProviderUserKey` vlastnost pro načtení jejich ID uživatele. Uživatelské jméno zadané uživatelem v ovládacím prvku CreateUserWizard je k dispozici prostřednictvím jeho [ `UserName` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

V dalším kroku se načte připojovací řetězec z `Web.config` a `INSERT` zadaný příkaz. ADO.NET objekty jsou vytvořeny a příkaz provést. Kód přiřadí [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) instance na `@HomeTown`, `@HomepageUrl`, a `@Signature` parametry, které má za následek vložení databáze `NULL` hodnoty `HomeTown`, `HomepageUrl`, a `Signature` pole.

Přejděte `EnhancedCreateUserWizard.aspx` stránce prostřednictvím prohlížeče a vytvořit nový uživatelský účet. Až to uděláte, vraťte se do sady Visual Studio a zkontrolovat obsah `aspnet_Users` a `UserProfiles` tabulky (jak jsme to udělali v obrázek 12). Měli byste vidět nový uživatelský účet v `aspnet_Users` a odpovídající `UserProfiles` řádek (s `NULL` hodnoty `HomeTown`, `HomepageUrl`, a `Signature`).


[![Byly přidány nového uživatelského účtu a UserProfiles záznamu](storing-additional-user-information-vb/_static/image59.png)](storing-additional-user-information-vb/_static/image58.png)

**Obrázek 20**: A nový uživatelský účet a `UserProfiles` přidali záznam ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image60.png))


Poté, co má návštěvníka zadaná jeho nové informace o účtu a kliknutí na tlačítko "Create User", je vytvořen uživatelský účet a řádek přidán do `UserProfiles` tabulky. Pak zobrazí CreateUserWizard jeho `CompleteWizardStep`, který se zobrazí zpráva o úspěchu a tlačítko pro pokračování. Kliknutím na tlačítko Pokračovat vyvolá zpětné volání, ale nebyla provedena žádná akce, byste museli opustit uživatel zablokován na `EnhancedCreateUserWizard.aspx` stránky.

Lze zadat adresu URL pro uživatele poslat na po kliknutí na tlačítko Pokračovat pomocí ovládacího prvku CreateUserWizard na [ `ContinueDestinationPageUrl` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Nastavte `ContinueDestinationPageUrl` vlastnost "~ / Membership/AdditionalUserInfo.aspx". Tato akce trvá nového uživatele, aby `AdditionalUserInfo.aspx`, kde můžete zobrazit a aktualizovat svoje nastavení.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Přizpůsobení rozhraní CreateUserWizard výzvu k Domovská, domovskou stránku a podpis nového uživatele

Pro scénáře vytvoření jednoduchého účtu kde potřebujete shromažďovat pouze základní informace o uživatelském účtu jako uživatelské jméno, heslo a e-mailu stačí výchozí rozhraní prvku CreateUserWizard. Ale co když jsme chtěli výzvu musí návštěvníka zadejte svůj domácí města, domovskou stránku a podpis při vytváření svého účtu? Je možné přizpůsobit prvku CreateUserWizard rozhraní ke sběru dalších informací při registraci, a tyto informace slouží v `CreatedUser` obslužnou rutinu události pro vkládání dalších záznamů do databáze.

Ovládacím prvku CreateUserWizard rozšiřuje ovládací prvek ASP.NET průvodce, který je ovládací prvek, který umožňuje vývojářům definovat řadu seřazený `WizardSteps`. Průvodce ovládací prvek vykreslí aktivního kroku a poskytuje rozhraní navigace, které umožňuje přesunout tyto kroky musí návštěvníka. Ovládací prvek Průvodce je ideální pro rozdělení dlouhé úkolů v několika krocích. Další informace o ovládacím prvku průvodce najdete v tématu [vytvoření uživatelského rozhraní pro krok za krokem s ovládacím prvkem ASP.NET 2.0 průvodce](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Výchozí značky prvku CreateUserWizard definuje dva `WizardSteps`: `CreateUserWizardStep` a `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample12.aspx)]

První `WizardStep`, `CreateUserWizardStep`, vykreslí rozhraní, které vyzve k zadání uživatelského jména, hesla, e-mailu a tak dále. Po návštěvníka poskytne tyto údaje a klikne na tlačítko "Create User", které se zobrazí `CompleteWizardStep`, která zobrazuje zprávy o úspěchu a tlačítko Pokračovat.

Přizpůsobení ovládacího prvku CreateUserWizard rozhraní pro zahrnutí polí další formuláře, můžeme:

- <strong>Vytvořit jednu nebo více nových</strong><strong>`WizardStep`</strong><strong>s tak, aby obsahovala další prvky uživatelského rozhraní</strong>. Chcete-li přidat nový `WizardStep` chcete CreateUserWizard, klikněte na tlačítko "Přidat nebo odebrat `WizardStep` s" odkaz z jeho inteligentních značek ke spuštění `WizardStep` Editor kolekce. Odtud můžete přidat, odebrat nebo změnit pořadí kroků v průvodci. Jedná se o postup, který budeme používat pro účely tohoto kurzu.

- <strong>Převést</strong><strong>`CreateUserWizardStep`</strong><strong>do upravitelné</strong><strong>`WizardStep`</strong><strong>.</strong> Tím se nahradí `CreateUserWizardStep` s ekvivalentní `WizardStep` jehož značky definují uživatelské rozhraní, která odpovídá `CreateUserWizardStep`"s. Převedením `CreateUserWizardStep` do `WizardStep` jsme přemístění ovládací prvky nebo přidat další prvky uživatelského rozhraní k tomuto kroku. Chcete-li převést `CreateUserWizardStep` nebo `CompleteWizardStep` do upravovat `WizardStep`, klikněte "uživatele vytvořit vlastní krok" nebo "Upravit krok dokončení" propojit inteligentní značky ovládacího prvku.

- **Použijte kombinaci výše uvedených dvou možností.**

Jeden důležité brát v úvahu je, že ovládacím prvku CreateUserWizard spustí jeho procesu vytvoření účtu uživatele při kliknutí na tlačítko "Create User" v rámci jeho `CreateUserWizardStep`. Nebude vadit, když existují další `WizardStep` s po `CreateUserWizardStep` nebo ne.

Při přidávání vlastní `WizardStep` do ovládacího prvku CreateUserWizard ke shromažďování vstupu pro další uživatele, vlastní `WizardStep` lze umístit před nebo po `CreateUserWizardStep`. Pokud byla zaslána před `CreateUserWizardStep` potom jsou shromažďovány další uživatelský vstup z vlastního `WizardStep` je k dispozici pro `CreatedUser` obslužné rutiny události. Ale pokud vlastní `WizardStep` , přichází po `CreateUserWizardStep` potom podle času vlastní `WizardStep` se zobrazí nový uživatelský účet má již vytvořené a `CreatedUser` již byla aktivována událost.

Obrázek 21 ukazuje pracovní postup při přidaného `WizardStep` předchází `CreateUserWizardStep`. Protože byla shromážděna dalších informací o uživatelích čas `CreatedUser` událost je aktivována, všechny musíme udělat, je aktualizace `CreatedUser` obslužná rutina události načtení tyto vstupy a pro použití `INSERT` hodnoty parametru příkazu (spíše než `DBNull.Value`).


[![Pracovní postup CreateUserWizard, když Třída CreateUserWizardStep předchází další prvek WizardStep](storing-additional-user-information-vb/_static/image62.png)](storing-additional-user-information-vb/_static/image61.png)

**Obrázek 21**: The CreateUserWizard pracovního postupu při další `WizardStep` Precedes `CreateUserWizardStep` ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image63.png))


Pokud vlastní `WizardStep` nachází *po* `CreateUserWizardStep`, ale proces vytváření účtu uživatele dojde k předtím, než uživatel má využili příležitost dobře se zadejte svůj domácí města, domovská stránka nebo podpis. V takovém případě musí být vložena do databáze po vytvoření uživatelského účtu, jak ukazuje obrázek 22 tyto další informace.


[![Při další prvek WizardStep, přichází po třídu CreateUserWizardStep CreateUserWizard pracovního postupu](storing-additional-user-information-vb/_static/image65.png)](storing-additional-user-information-vb/_static/image64.png)

**Obrázek 22**: The CreateUserWizard pracovního postupu při další `WizardStep` dodává po `CreateUserWizardStep` ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image66.png))


Pracovní postup znázorňuje obrázek 22 čeká k vložení záznamu do `UserProfiles` tabulky až po dokončení kroku 2. Pokud návštěvníka zavře svém webovém prohlížeči po kroku 1, ale spojili jsme se bude mít stav, kdy byl uživatelský účet vytvořen, ale žádný záznam se přidal do `UserProfiles`. Jeden alternativním řešením je záznam obsahuje `NULL` nebo výchozí hodnoty, které jsou vloženy do `UserProfiles` v `CreatedUser` obslužné rutiny události (která se vyvolá po kroku 1) a aktualizujete tento záznam po dokončení kroku 2. To zajistí, že `UserProfiles` pro uživatelský účet se přidá záznam, i když uživatel ukončí uprostřed proces registrace prostřednictvím.

Pro účely tohoto kurzu vytvoříme novou `WizardStep` , která nastane po `CreateUserWizardStep` ale předtím, než `CompleteWizardStep`. První get WizardStep v umístění a nakonfigurovaný a potom nám budete Podívejme se na kód.

V prvku CreateUserWizard inteligentní značky, vyberte "Přidat nebo odebrat `WizardStep` s", která se vyvolá `WizardStep` dialogové okno Editor kolekcí. Přidat nový `WizardStep`a nastavte jeho `ID` k `UserSettings`, jeho `Title` na "Nastavení" a jeho `StepType` k `Step`. Umístěte ho tak, že jde o po `CreateUserWizardStep` ("zaregistrujte vašeho nového účtu služby") a před `CompleteWizardStep` ("dokončených"), jak ukazuje obrázek 23.


[![Přidejte nový prvek WizardStep ovládacího prvku CreateUserWizard](storing-additional-user-information-vb/_static/image68.png)](storing-additional-user-information-vb/_static/image67.png)

**Obrázek 23**: Přidání nový `WizardStep` do ovládacího prvku CreateUserWizard ([kliknutím ji zobrazíte obrázek v plné velikosti](storing-additional-user-information-vb/_static/image69.png))


Kliknutím na OK zavřete `WizardStep` dialogové okno Editor kolekcí. Nové `WizardStep` svědčí prvku CreateUserWizard aktualizované deklarativní:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample13.aspx)]

Všimněte si, nové `<asp:WizardStep>` elementu. Potřebujeme přidat uživatelské rozhraní pro shromažďování domácí města, domovskou stránku a podpis zde nového uživatele. Tento obsah můžete zadat v deklarativní syntaxe nebo prostřednictvím návrháře. Použití návrháře, vyberte z rozevíracího seznamu v inteligentních značek k přejděte ke kroku v Návrháři krok "Nastavení".

> [!NOTE]
> Výběr krok prostřednictvím inteligentních značek rozevíracího seznamu aktualizuje prvku CreateUserWizard [ `ActiveStepIndex` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), která určuje index počáteční krok. Proto pokud tohoto rozevíracího seznamu můžete upravit krok "Nastavení" v návrháři, nezapomeňte nastavit zpět na "Přihlašování až pro svůj nový účet" tak, aby tento krok se zobrazí, když uživatelé navštíví nejprve `EnhancedCreateUserWizard.aspx` stránky.


Vytvoření uživatelského rozhraní v rámci kroku "Nastavení", který obsahuje tři ovládací prvky textového pole s názvem `HomeTown`, `HomepageUrl`, a `Signature`. Po vytváření toto rozhraní, CreateUserWizard deklarativní by měl vypadat nějak takto:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample14.aspx)]

Pokračujte a tuto stránku prostřednictvím prohlížeče a vytvořit nový uživatelský účet, zadávání hodnot pro domácí města, domovskou stránku a podpisu. Po dokončení `CreateUserWizardStep` uživatelský účet je vytvořen v rámci členství a `CreatedUser` spustí obslužnou rutinu události, které přidá nový řádek do `UserProfiles`, ale s databází `NULL` hodnota `HomeTown`, `HomepageUrl`, a `Signature`. Hodnoty zadané pro domácí města, domovskou stránku a podpis se nikdy nepoužívá. Net výsledkem je nový uživatelský účet se `UserProfiles` záznam, jehož `HomeTown`, `HomepageUrl`, a `Signature` pole ještě nutné zadat.

Potřebujeme ke spuštění kódu po kroku "Nastavení", který má domácí města, honepage a podpis hodnoty zadané uživatelem a aktualizuje odpovídající `UserProfiles` záznamu. Pokaždé, když uživatel přesune mezi kroky v Průvodci ovládacích prvků, průvodce [ `ActiveStepChanged` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) aktivována. Můžeme vytvořit obslužnou rutinu události pro tuto událost a aktualizace `UserProfiles` tabulky po dokončení kroku "Nastavení".

Přidat obslužnou rutinu události pro CreateUserWizard `ActiveStepChanged` událostí a přidejte následující kód:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample15.vb)]

Výše uvedený kód se spustí na zjištění, zda právě jsme dosáhli krok "Dokončit". Vzhledem k tomu, že v kroku "Dokončit" se nachází hned za krok "Nastavení", pak dosáhne návštěvníka "Dokončených" krok, to znamená, že uživatel právě dokončili krok "Nastavení".

V takovém případě budeme potřebovat programově odkazovat na ovládací prvky textového pole v rámci `UserSettings WizardStep`. To lze provést pomocí `FindControl` metoda programově odkazující `UserSettings WizardStep`a pak odkazovat textových polí v rámci `WizardStep`. Když neodkazujete textových polí jsme připraveni spustit `UPDATE` příkazu. `UPDATE` Příkaz má stejný počet parametrů jako `INSERT` příkaz v `CreatedUser` obslužná rutina události, ale zde použijeme domácí města, domovskou stránku a podpis hodnoty poskytnuté uživatelem.

Pomocí této obslužné rutiny události na místě, přejděte `EnhancedCreateUserWizard.aspx` stránce prostřednictvím prohlížeče a vytvořit nový uživatelský účet zadávání hodnot pro domácí města, domovskou stránku a podpisu. Po vytvoření nového účtu měli byste být přesměrovaní na `AdditionalUserInfo.aspx` stránku, kde právě zadali domácí města, domovskou stránku a podpis se zobrazují informace.

> [!NOTE]
> Náš web aktuálně obsahuje dvě stránky, ze kterých návštěvník můžete vytvořit nový účet: `CreatingUserAccounts.aspx` a `EnhancedCreateUserWizard.aspx`. Přejděte na přihlašovací stránce a mapy webu na webu `CreatingUserAccounts.aspx` stránky, ale `CreatingUserAccounts.aspx` stránky není vyzve uživatele k domácí města, domovskou stránku a podpis informace a nedojde k přidání odpovídající řádek `UserProfiles`. Proto, aktualizujte `CreatingUserAccounts.aspx` stránce tak, aby tato funkce nabízí nebo aktualizujte stránku mapy webu a přihlaste se k odkazování `EnhancedCreateUserWizard.aspx` místo `CreatingUserAccounts.aspx`. Pokud se rozhodnete druhou možnost, nezapomeňte aktualizovat `Membership` složky `Web.config` tak, aby umožňoval anonymním uživatelům přístup k souboru `EnhancedCreateUserWizard.aspx` stránky.


## <a name="summary"></a>Souhrn

V tomto kurzu jsme se podívali na techniky pro modelování dat, která souvisí s účty uživatelů v rámci členství. Konkrétně jsme se podívali na modelování entity, které sdílejí vztah jeden mnoho se uživatelské účty, jakož i data, která sdílí vztah 1: 1. Kromě toho jsme viděli, jak to související informace může zobrazit, Vložit a aktualizovány, některé příklady použití ovládacím prvkem SqlDataSource a ostatní pomocí kódu ADO.NET.

V tomto kurzu dokončíte naše pohled na uživatelské účty. Spouští se k dalšímu kurzu Změníme naši pozornost k rolím. Příštích několika kurzy se podíváme na rozhraní role zjistit, jak vytvořit nové role přiřazení rolí uživatelům, jak určit, ke kterým rolím uživatel patří do a jak se autorizace na základě rolí.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Přístup k a aktualizace dat v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Ovládací prvek ASP.NET 2.0 Průvodce](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Vytvoření pomocí ovládacího prvku ASP.NET 2.0 průvodce krok za krokem uživatelského rozhraní](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Vytváří se vlastní zdroj dat řídicí parametry](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Přizpůsobení ovládacího prvku CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Ovládací prvek DetailsView šablon rychlý start](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Zobrazení dat pomocí ovládacího prvku ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Rozbor validačních ovládacích prvků v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Úpravy vložení a odstranění dat](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Ověření formuláře v ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Shromažďování informací o registraci vlastního uživatele](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profily v technologii ASP.NET 2.0](http://www.odetocode.com/Articles/440.aspx)
- [Asp: ListView ovládacího prvku](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Rychlý start profily uživatelů](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, Autor více ASP/ASP.NET knih a zakladatelem 4GuysFromRolla.com, má pracovali Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy  *[Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott může být dostupný na adrese [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k...

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](user-based-authorization-vb.md)
