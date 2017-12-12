---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: "Ukládání informací o další uživatele (C#) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu jsme tuto otázku odpovědět podle budovy vyloženě jen základní návštěv aplikace. Při tom se podíváme na různé možnosti pro modeli..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d3c622dc352c804ddfea072bf3d52496c719ea6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="storing-additional-user-information-c"></a>Ukládání informací o další uživatele (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> V tomto kurzu jsme tuto otázku odpovědět podle budovy vyloženě jen základní návštěv aplikace. Při tom jsme bude podívejte se na různé možnosti pro modelování informace o uživateli v databázi a poté zjistit, jak tato data přidružit uživatelské účty, vytvořené pomocí rozhraní členství.


## <a name="introduction"></a>Úvod

SOUBOR ASP. Na NET framework členství nabízí flexibilní rozhraní pro správu uživatelů. Rozhraní API členství zahrnuje metody pro ověřování přihlašovacích údajů, načítání informací o aktuálně přihlášeného uživatele, vytvořit nový uživatelský účet a odstranění uživatelského účtu, mimo jiné. Každý uživatelský účet v rámci členství obsahuje pouze vlastnosti, které jsou potřebné pro ověření pověření a provádění úloh souvisejících s účtem nezbytné uživatele. To svědčí metody a vlastnosti [ `MembershipUser` třída](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.aspx), který modelů uživatelský účet v rámci členství. Tato třída obsahuje vlastnosti, například [ `UserName` ](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.email.aspx), a [ `IsLockedOut` ](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.islockedout.aspx), a metody, jako jsou [ `GetPassword` ](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.getpassword.aspx) a [ `UnlockUser` ](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.unlockuser.aspx).

Často aplikace je nutné uložit informace o dalších uživateli, které nejsou zahrnuty v rámci členství. Online prodejce může například potřebovat umožníte každý uživatel uložit svůj přenosů a fakturační adresu, platební údaje, předvolby doručení a obraťte se na telefonní číslo. Kromě toho každý pořadí v systému je přidružen určitého uživatelského účtu.

`MembershipUser` Třída nezahrnuje vlastnosti, například `PhoneNumber` nebo `DeliveryPreferences` nebo `PastOrders`. Jak tedy jsme sledovat informace o uživateli, které jsou potřebné pro aplikaci a mějte ho integrovat členství framework? V tomto kurzu jsme tuto otázku odpovědět podle budovy vyloženě jen základní návštěv aplikace. Při tom jsme bude podívejte se na různé možnosti pro modelování informace o uživateli v databázi a poté zjistit, jak tato data přidružit uživatelské účty, vytvořené pomocí rozhraní členství. Můžeme začít!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Krok 1: Vytvoření aplikace návštěv datový Model

Existuje mnoho různých technik, které mohou být použity k zaznamenání informací o uživateli v databázi a přidružte ji k uživatelské účty, vytvořené pomocí rozhraní členství. Chcete-li ilustrují tyto postupy, je potřeba posílení kurz webové aplikace tak, že zachytí nějaká data související s uživatelem. (V současné době datový model aplikace obsahuje pouze aplikaci služby tabulky vyžaduje `SqlMembershipProvider`.)

Umožňuje vytvořit aplikaci velmi jednoduché návštěv kde ověřeného uživatele můžete nechat komentář. Kromě ukládání návštěv komentáře, můžeme umožnit každému uživateli ukládat svůj domácí města, domovskou stránku a podpis. Pokud je zadán, domácí městě uživatele, podpisu a domovské stránky se zobrazí na každou zprávu, kterou si opustil v knize návštěv.

### <a name="adding-theguestbookcommentstable"></a>Přidávání`GuestbookComments`tabulky

Aby bylo možné zachytit návštěv komentáře, je potřeba vytvořit tabulku databáze s názvem `GuestbookComments` s sloupce jako `CommentId`, `Subject`, `Body`, a `CommentDate`. Také je potřeba mít každý záznam v `GuestbookComments` uživatele, který zbývajících komentář odkaz na tabulku.

Pokud chcete tuto tabulku do databáze hotový, přejděte do Průzkumníka databáze v sadě Visual Studio a k podrobnostem `SecurityTutorials` databáze. Klikněte pravým tlačítkem na složku tabulky a zvolte Přidat novou tabulku. Po výběru této možnosti rozhraní, které umožňuje definovat sloupce pro novou tabulku.


[![Přidat novou tabulku do databáze SecurityTutorials](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**Obrázek 1**: přidejte do nové tabulky `SecurityTutorials` databáze ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image3.png))


V dalším kroku definovat `GuestbookComments`na sloupce. Začněte přidáním sloupec s názvem `CommentId` typu `uniqueidentifier`. V tomto sloupci se jednoznačně identifikovat každou komentář v knize návštěv, takže zakáže `NULL` s a označte ji jako primární klíč tabulky. Místo zadání hodnoty pro `CommentId` pole na všech `INSERT`, jsme můžete určit, že nový `uniqueidentifier` hodnotu by měl být automaticky vygenerován pro toto pole na `INSERT` nastavením sloupce výchozí hodnotu na `NEWID()`. Po přidání toto první pole označení jako primární klíč a nastavení jeho výchozí hodnotu obrazovky by měl vypadat na uvedené na obrázku 2 snímku obrazovky.


[![Přidejte primární sloupec s názvem CommentId](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**Obrázek 2**: přidejte primární sloupec s názvem `CommentId` ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image6.png))


Dál přidejte sloupec s názvem `Subject` typu `nvarchar(50)` a sloupec s názvem `Body` typu `nvarchar(MAX)`, zákaz `NULL` s v obou sloupcích. Následující, přidat sloupec s názvem `CommentDate` typu `datetime`. Zakáže `NULL` s a sadu `CommentDate` výchozí hodnota sloupce do `getdate()`.

Zbývá přidat sloupec, který přidruží účet uživatele každý návštěv komentář. Jednou z možností je přidat sloupec s názvem `UserName` typu `nvarchar(256)`. To je vhodné použít v případě pomocí zprostředkovatele členství jiné než `SqlMembershipProvider`. Ale při použití `SqlMembershipProvider`, protože jsme se nachází v kurzu této série, `UserName` sloupec v `aspnet_Users` tabulky není musí být jedinečný. `aspnet_Users` Je primární klíč tabulky `UserId` a je typu `uniqueidentifier`. Proto `GuestbookComments` tabulku je nutné sloupec s názvem `UserId` typu `uniqueidentifier` (zakazuje `NULL` hodnoty). Pokračovat a přidat tento sloupec.

> [!NOTE]
> Jak již bylo zmíněno [ *vytváření schématu členství v systému SQL Server* ](creating-the-membership-schema-in-sql-server-cs.md) kurz, členství v rámci vytvořené za účelem povolení několika webových aplikací s různými uživatelskými účty sdílet stejné úložiště uživatelů. Dělá to pomocí dělení uživatelské účty do různých aplikací. A při každé uživatelské jméno je musí být jedinečný v rámci aplikace, může se použít stejné uživatelské jméno v různých aplikacích pomocí stejné úložiště uživatele. Je složené `UNIQUE` omezením `aspnet_Users` tabulky na `UserName` a `ApplicationId` pole, ale není jednou na právě na `UserName` pole. V důsledku toho je možné aspnet\_uživatelé tabulka má dva (nebo více) záznamů se stejným `UserName` hodnotu. Existuje, ale `UNIQUE` omezení `aspnet_Users` tabulky `UserId` pole (protože je primární klíč). A `UNIQUE` omezení je důležité, protože bez jsme nejde vytvořit omezení cizího klíče mezi `GuestbookComments` a `aspnet_Users` tabulky.


Po přidání `UserId` sloupce, uložte kliknutím na ikonu Uložit na panelu nástrojů v tabulce. Pojmenujte novou tabulku `GuestbookComments`.

Máme jeden poslední problém se účastnit se `GuestbookComments` tabulky: je potřeba vytvořit [omezení cizího klíče](https://msdn.microsoft.com/en-us/library/ms175464.aspx) mezi `GuestbookComments.UserId` sloupce a `aspnet_Users.UserId` sloupec. Jak toho docílit, klikněte na ikonu vztah na panelu nástrojů spustit dialogové okno cizího klíče relace. (Alternativně můžete spustit tohoto dialogového okna přechodem do nabídky Návrháře tabulky a zvolením vztahy.)

Kliknutím na tlačítko Přidat v levém dolním rohu dialogu vztahy cizího klíče. Tím se přidá nové omezení cizího klíče, i když musíme definovat tabulky, které účast v relaci.


[![Pomocí dialogového okna relace cizích klíčů ke správě omezení cizího klíče tabulky](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**Obrázek 3**: cizí klíč relace dialogu použít ke správě omezení cizího klíče tabulky ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image9.png))


Potom klikněte na ikonu výpustky v řádku "Tabulky a sloupce specifikace" na pravé straně. Tím se spustí dialogové okno tabulky a sloupce, ze které můžeme určit primární klíč tabulky a sloupce a sloupec cizího klíče z `GuestbookComments` tabulky. Zejména `aspnet_Users` a `UserId` jako primární klíč tabulky a sloupce, a `UserId` z `GuestbookComments` tabulce jako sloupec cizího klíče (viz obrázek 4). Po definování primárního a cizího klíče tabulky a sloupce, klikněte na tlačítko OK se vrátíte do dialogového okna cizího klíče relace.


[![Vytvoření cizího klíče omezení mezi aspnet_Users a GuesbookComments tabulky](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**Obrázek 4**: vytvoření cizí klíč omezení mezi `aspnet_Users` a `GuesbookComments` tabulky ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image12.png))


V tomto okamžiku navázání omezení cizího klíče. Zajišťuje přítomnost toto omezení [relační integrity](http://en.wikipedia.org/wiki/Referential_integrity) mezi dvěma tabulkami podle zaručit, že nikdy bude návštěv položku, která odkazuje na neexistující uživatelský účet. Ve výchozím nastavení bude omezení cizího klíče zakáže nadřazeného záznamu odstranit, pokud existují odpovídající podřízené záznamy. To znamená pokud uživatel provede jeden nebo více návštěv komentáře, a potom jsme pokusu o odstranění uživatelského účtu, odstranit selže, pokud jsou jako první smazány jeho návštěv komentáře.

Automaticky odstranit přidružené podřízené záznamy při odstranění nadřazeného záznamu lze nastavit omezení cizího klíče. Omezení cizího klíče jsme jinými slovy, můžete nastavit tak, aby uživatele návštěv položky se automaticky odstraní při odstranění svůj uživatelský účet. K tomu, rozbalte v části "INSERT a UPDATE Specification" a nastavte vlastnost "Odstranit pravidlo" na Cascade.


[![Konfigurace omezení cizího klíče pro kaskádové odstranění](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**Obrázek 5**: Konfigurace omezení pro cizí klíč k odstraní Cascade ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image15.png))


Pokud chcete uložit omezení cizího klíče, klikněte na tlačítko Zavřít ukončete mimo vztahy cizího klíče. Pak klikněte na ikonu Uložit na panelu nástrojů pro uložení v tabulce a této relace.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Ukládání domovské města, domovskou stránku a podpis uživatele

`GuestbookComments` Tabulka ukazuje, jak ukládat informace, které sdílí vztah jeden mnoho s uživatelskými účty. Vzhledem k tomu, že každý uživatelský účet může mít libovolný počet přidružené komentáře, tento vztah je modelovaná vytvoření tabulky pro uložení nastavení komentáře, které obsahuje sloupec, odkazy Zpět každý komentář na konkrétního uživatele. Při použití `SqlMembershipProvider`, tento odkaz se nejlépe navazuje vytváření sloupec s názvem `UserId` typu `uniqueidentifier` a omezení cizího klíče mezi v tomto sloupci a `aspnet_Users.UserId`.

Nyní potřebujeme přidružit každý uživatelský účet k uložení uživatele domácí města, domovskou stránku a podpisu, který se zobrazí v jeho návštěv komentáře tři sloupce. Existují různé způsoby, jak dosáhnout počet:

- **Přidat nové sloupce ***`aspnet_Users`*** nebo ***`aspnet_Membership`*** tabulky.** Protože upravuje schéma používá I nebude doporučujeme tento přístup `SqlMembershipProvider`. Toto rozhodnutí může se vraťte k haunt jste dolů na cestách. Například v co v případě, budoucí verzi technologie ASP.NET používá jiné `SqlMembershipProvider` schématu. Microsoft může zahrnovat nástroj pro migraci technologii ASP.NET 2.0 `SqlMembershipProvider` dat na nové schéma, ale pokud jste upravili technologii ASP.NET 2.0 `SqlMembershipProvider` schématu, takový převod nemusí být možné.

- **Pomocí prostředí ASP. Na NET profil framework, definování vlastnosti profilu pro domácí města, domovskou stránku a podpis.** Technologie ASP.NET obsahuje profil rozhraní, které je určen k ukládání další uživatelská data. Jako rozhraní členství je rozhraní profil vytvořené na modelu poskytovatelů. Rozhraní .NET Framework se dodává s `SqlProfileProvider` sthat ukládá data profilu v databázi systému SQL Server. Ve skutečnosti naše databáze již obsahuje tabulky používané `SqlProfileProvider` (`aspnet_Profile`), jak byl přidán, když jsme přidali aplikačních služeb zpátky <a id="_msoanchor_2"> </a> [ *vytváření schématu členství v systému SQL Server* ](creating-the-membership-schema-in-sql-server-cs.md) kurzu.   
 Hlavní výhodou rozhraní profilu je, že umožňuje vývojářům k definování vlastností profilu v `Web.config` – žádný kód potřeba zapsat k serializaci dat profilu do a z příslušné datové úložiště. Stručně řečeno je velmi snadno definovat sadu vlastností profilu a pokud s nimi pracovat v kódu. Ale profil systému opustí mnoho pro potřeby, pokud jde o správu verzí, takže pokud máte aplikaci, kde byste měli nové vlastnosti specifické pro uživatele přidat později, nebo existující na odebrat nebo změnit, a potom rozhraní profil nemusí být  nejlepší možnost. Kromě toho `SqlProfileProvider` ukládá vlastnosti profilu vysoce nenormalizované způsobem, což další možné ke spouštění dotazů na data profilu (například počet uživatelů, kteří mají domácí městě v New Yorku) přímo.   
 Další informace o rozhraní profilu najdete v části "Další odečty" na konci tohoto kurzu.

- **Přidat tyto tři sloupce do nové tabulky v databázi a vytvořit relace mezi této tabulky a ***`aspnet_Users`***.** Tento postup zahrnuje trochu další práci, než s použitím profilu framework, ale nabízí nejvyšší flexibilitu v tom, jak jsou modelovány vlastnosti další uživatele v databázi. Tato možnost, kterou použijeme v tomto kurzu se.

Vytvoříme nové tabulky `UserProfiles` uložte domácí města, domovskou stránku a podpis pro každého uživatele. Klikněte pravým tlačítkem na složku tabulky v okně Průzkumníka databáze a vytvořit novou tabulku. Název první sloupec `UserId` a nastavte její typ `uniqueidentifier`. Zakáže `NULL` hodnoty a označte sloupec jako primární klíč. V dalším kroku přidat sloupce s názvem: `HomeTown` typu `nvarchar(50)`; `HomepageUrl` typu `nvarchar(100)`; a podpis typu `nvarchar(500)`. Každý z těchto tří sloupců může přijmout `NULL` hodnotu.


[![Vytvoření tabulky UserProfiles](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**Obrázek 6**: vytvoření `UserProfiles` tabulky ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image18.png))


Uložte tabulku a pojmenujte ji `UserProfiles`. Nakonec navázání omezení cizího klíče mezi `UserProfiles` tabulky `UserId` pole a `aspnet_Users.UserId` pole. Jako jsme to udělali s omezení cizího klíče mezi `GuestbookComments` a `aspnet_Users` tabulky, mít toto omezení kaskádové odstranění. Vzhledem k tomu `UserId` pole `UserProfiles` je primární klíče, zajistíte tak, že bude více než jeden záznam v `UserProfiles` tabulky pro každý uživatelský účet. Tento typ vztahu se označuje jako 1: 1.

Teď, když máme datový model, který vytvořili, jsme připravení ho použít. Kroky 2 a 3 se podíváme na jak aktuálně přihlášeného uživatele můžete zobrazit a upravit jejich domácí města, domovskou stránku a podpis informace. V kroku 4 vytvoříme rozhraní pro ověřené uživatele k odeslání nové komentáře knihy návštěv a zobrazení existující.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Krok 2: Zobrazení domovské města, domovskou stránku a podpis uživatele

Existuje mnoho různých způsobů, jak povolit aktuálně přihlášeného uživatele k zobrazení a úprava jeho domácí města, domovskou stránku a podpis informací. Ovládací prvky popisek nebo jsme může používat jeden z datových ovládacích prvků, jako je třeba Správa DetailsView a jsme ručně vytvořit uživatelské rozhraní s textového pole. K provedení databáze `SELECT` a `UPDATE` příkazy jsme ADO.NET napsat kód na naší stránce kódu třídy nebo alternativně využívat deklarativní přístup s SqlDataSource. Naše aplikace v ideálním případě by obsahovat vrstvené architekturu, která jsme může buď vyvolat prostřednictvím kódu programu z třídy kódu stránky nebo deklarativně prostřednictvím ovládacího prvku ObjectDataSource.

Vzhledem k tomu, že tato řada kurz se zaměřuje na ověřování pomocí formulářů, ověřování, uživatelské účty a rolí, nebude vyčerpávající diskusi o tyto možnosti přístupu různých datových nebo proč vrstvené architektura je upřednostňovaný přes přímo provádění příkazů SQL ze stránky ASP.NET. Přechod do provede pomocí DetailsView a SqlDataSource – možnost nejrychlejší a nejjednodušší – ale určitě Principy probírané lze použít pro alternativní webové ovládací prvky a data logiku přístup. Další informace o práci s daty v technologii ASP.NET, najdete v části Moje  *[práci s daty v technologii ASP.NET 2.0](../../data-access/index.md)*  kurz řady.

Otevřete `AdditionalUserInfo.aspx` stránky v `Membership` složky a přidejte na stránku nastavení ovládacího prvku DetailsView jeho `ID` vlastnost `UserProfile` a vymazání jeho `Width` a `Height` vlastnosti. Rozbalte DetailsView inteligentních značek a vyberte pro vytvoření vazby na ovládací prvek nové datové zdroje. Tím se spustí Průvodce konfigurací zdroje dat (viz obrázek 7). Prvním krokem žádostí o zadání typu zdrojového data. Vzhledem k tomu, že jsme se chystáte připojit přímo na `SecurityTutorials` databázi, zvolte ikonu databáze určení `ID` jako `UserProfileDataSource`.


[![Přidat nový ovládací prvek SqlDataSource s názvem UserProfileDataSource](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**Obrázek 7**: přidejte nový název ovládacího prvku SqlDataSource `UserProfileDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image21.png))


Na další obrazovce zobrazí výzvu pro databázi použít. Už jsme definovali připojovací řetězec v `Web.config` pro `SecurityTutorials` databáze. Tento název připojovacího řetězce – `SecurityTutorialsConnectionString` – musí být v rozevíracím seznamu. Vyberte tuto možnost a kliknutím na tlačítko Další.


[![V rozevíracím seznamu vyberte SecurityTutorialsConnectionString](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**Obrázek 8**: Zvolte `SecurityTutorialsConnectionString` z rozevíracího seznamu ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image24.png))


Na další obrazovce zobrazí nám k určení tabulky a sloupce, které chcete dotaz. Vyberte `UserProfiles` tabulky z rozevíracího seznamu a zkontrolujte všechny sloupce.


[![Přepněte zpět všechny sloupce z tabulky UserProfiles](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**Obrázek 9**: Přepněte zpět všechny sloupce z `UserProfiles` tabulky ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image27.png))


Aktuální dotaz vrátí obrázek 9 *všechny* záznamy v `UserProfiles`, ale nemůžeme zajímá pouze v záznamu aktuálně přihlášeného uživatele. Přidání `WHERE` klauzule, klikněte na tlačítko `WHERE` tlačítko Přidat zobrazíte `WHERE` klauzule dialogu (viz obrázek 10). Tady můžete vybrat sloupec pro filtrování, operátor a zdroj daného parametru filtru. Vyberte `UserId` jako sloupce a "=" jako operátor.

Bohužel neexistuje předdefinované parametr zdroj vrátit aktuálně přihlášeného uživatele `UserId` hodnotu. Je potřeba získat tuto hodnotu prostřednictvím kódu programu. Proto nastavte rozevírací seznam zdrojů na "Žádný" klikněte na tlačítko Přidat parametr přidat a klikněte na tlačítko OK.


[![Přidání parametru filtr pro sloupec ID uživatele](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**Obrázek 10**: přidejte parametr filtru na `UserId` sloupce ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image30.png))


Po kliknutí na tlačítko OK se vrátíte na obrazovku vidět na obrázku 9. Tentokrát ale dotazu SQL v dolní části obrazovky by měla obsahovat `WHERE` klauzule. Přejděte obrazovku "Test dotaz", klikněte na tlačítko Další. Zde můžete spustit dotaz a prohlédněte výsledky. Kliknutím na tlačítko Dokončit ukončete průvodce.

Po dokončení Průvodce konfigurací zdroje dat, Visual Studio vytvoří SqlDataSource ovládacího prvku v závislosti na nastaveních určených v průvodci. Kromě toho ručně přidá BoundFields DetailsView pro každý sloupec vrácený SqlDataSource `SelectCommand`. Není nutné zobrazit `UserId` pole v ovládacím prvku DetailsView, protože uživatel nemusí vědět, tato hodnota. Můžete odebrat tato pole přímo z deklarativní ovládací prvek DetailsView nebo kliknutím na "Upravit pole" odkaz z jeho inteligentních značek.

V tomto okamžiku vaší stránky deklarativní by měl vypadat takto:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

Je potřeba nastavit programově prvku SqlDataSource `UserId` parametr pro aktuálně přihlášeného uživatele `UserId` předtím, než je vybraná data. Můžete to provést tak, že vytvoříte obslužné rutiny události pro ve třídě SqlDataSource `Selecting` událostí a přidáním následující kód existuje:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

Výše uvedený kód začíná získat odkaz na aktuálně přihlášeného uživatele voláním `Membership` třídy `GetUser` metoda. Tento příkaz vrátí `MembershipUser` objekt, jehož `ProviderUserKey` vlastnost obsahuje `UserId`. `UserId` Hodnota je pak přiřazena SqlDataSource `@UserId` parametr.

> [!NOTE]
> `Membership.GetUser()` Metoda vrátí informace o aktuálně přihlášeného uživatele. Pokud anonymního uživatele je na stránce, vrátí hodnotu `null`. V takovém případě to povede k `NullReferenceException` na následující řádek kódu při pokusu o čtení `ProviderUserKey` vlastnost. Samozřejmě nemáme si dělat starosti `Membership.GetUser()` vrácení `null` hodnotu `AdditionalUserInfo.aspx` stránky, protože jsme nakonfigurovali autorizace adres URL v předchozí kurzu tak, aby může používat pouze ověření uživatelé prostředky technologie ASP.NET v této složce. Pokud potřebujete přístup k informacím o aktuálně přihlášeného uživatele na stránce, kde je povolen anonymní přístup, ujistěte se, zkontroluje, jestli jinou hodnotu než`null MembershipUser` objekt je vrácen z `GetUser()` metody před odkazující na její vlastnosti.


Pokud navštívíte `AdditionalUserInfo.aspx` stránku prostřednictvím prohlížeče se zobrazí prázdné stránky, protože se musí ještě přidat všechny řádky, které `UserProfiles` tabulky. V kroku 6 se podíváme na tom, jak přizpůsobit a automaticky tak přidejte nový řádek do ovládacího prvku CreateUserWizard `UserProfiles` tabulky při vytvoření nového uživatelského účtu. Prozatím se však bude musíme ručně vytvořit záznam v tabulce.

Přejděte do Průzkumníka databáze v sadě Visual Studio a rozbalte složku tabulky. Klikněte pravým tlačítkem na `aspnet_Users` tabulky a vybrat "Zobrazit Data tabulky" zobrazíte záznamy v tabulce; stejnou věc udělat `UserProfiles` tabulky. Obrázek 11 zobrazuje tyto výsledky při rozložen formou dlaždic svisle. V databázi nejsou aktuálně `aspnet_Users` záznamy pro Bruce, František a Tito, ale žádné záznamy v `UserProfiles` tabulky.


[![Zobrazí se obsah aspnet_Users a UserProfiles tabulky](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**Obrázek 11**: obsah `aspnet_Users` a `UserProfiles` zobrazí tabulky ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image33.png))


Přidejte nový záznam pro `UserProfiles` ručním zadáním hodnoty v tabulce `HomeTown`, `HomepageUrl`, a `Signature` pole. Nejjednodušší způsob, jak získat platnou `UserId` hodnotu v novém `UserProfiles` záznamu je výběr `UserId` pole z určitého uživatelského účtu v `aspnet_Users` tabulky a zkopírujte a vložte ji do `UserId` pole `UserProfiles`. Obrázek 12 znázorňuje `UserProfiles` tabulky po byl přidán nový záznam pro Bruce.


[![Záznam byla přidána do UserProfiles pro Bruce](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**Obrázek 12**: záznam A byla přidána do `UserProfiles` pro Bruce ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image36.png))


Vraťte se na `AdditionalUserInfo.aspx` stránky, přihlášení jako Bruce. Jak ukazuje obrázek 13, zobrazí se na Bruce nastavení.


[![Uživatel aktuálně návštěvou je znázorněno His nastavení](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**Obrázek 13**: současné době návštěvou uživatele je znázorněno His nastavení ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image39.png))


> [!NOTE]
> Přejděte dopředu a ručně přidat záznamy v `UserProfiles` tabulky pro každého uživatele členství. V kroku 6 se podíváme na tom, jak přizpůsobit a automaticky tak přidejte nový řádek do ovládacího prvku CreateUserWizard `UserProfiles` tabulky při vytvoření nového uživatelského účtu.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Krok 3: Povolení uživatelům upravit jeho domovské města, domovskou stránku a podpis

V tomto okamžiku můžete zobrazit aktuálně přihlášeného uživatele, jejich domácí města, domovskou stránku a podpis nastavení, ale nemohou upravovat ještě je. Umožňuje aktualizovat ovládací prvek DetailsView tak, aby data lze upravovat.

První věc budeme muset udělat, je přidat `UpdateCommand` pro SqlDataSource, určení `UPDATE` jeho odpovídající parametry a příkaz k provedení. Vyberte SqlDataSource a v okně vlastností klikněte na tři tečky vedle UpdateQuery vlastnost, která má vyvolat dialogové okno Editor kolekce parametrů. Zadejte následující `UPDATE` příkaz do textového pole:

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

Potom klikněte na tlačítko "Aktualizovat parametry", která vytvoří parametr v ovládacím prvku SqlDataSource `UpdateParameters` pro každou z parametrů v kolekci `UPDATE` příkaz. Nechte zdrojem pro všechny sady parametrů na hodnotu None a klikněte na tlačítko OK dialogové okno vyplňte.


[![Zadejte vlastnost UpdateCommand a UpdateParameters SqlDataSource](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**Obrázek 14**: zadejte ve třídě SqlDataSource `UpdateCommand` a `UpdateParameters` ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image42.png))


Z důvodu budou přidány jsme provedli do ovládacího prvku SqlDataSource DetailsView řízení může nyní podporovat úpravy. Z prvku DetailsView inteligentních značek zaškrtnutím políčka "Povolit úpravy". Tento postup přidá CommandField do ovládacího prvku `Fields` kolekce s jeho `ShowEditButton` vlastností nastavenou na hodnotu True. To vykreslí tlačítko pro úpravy, když DetailsView se zobrazuje v režimu jen pro čtení a aktualizaci a tlačítka Storno při zobrazení v režimu úprav. Místo nutnosti uživateli klikněte na tlačítko Upravit, ale můžete máme vykreslení DetailsView ve stavu "vždy upravovat" nastavením ovládací prvek DetailsView [ `DefaultMode` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) k `Edit`.

Tyto změny prvek DetailsView deklarativní by měl vypadat takto:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

Poznámka: Přidání CommandField a `DefaultMode` vlastnost.

Pokračujte a testování tuto stránku prostřednictvím prohlížeče. Při návštěvě s uživatelem, který má odpovídající záznam v `UserProfiles`, nastavení uživatele se zobrazují v upravitelné rozhraní.


[![DetailsView vykreslí upravitelné rozhraní](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**Obrázek 15**: DetailsView vykreslí upravitelné rozhraní ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image45.png))


Zkuste změnit hodnoty a kliknutím na tlačítko Aktualizovat. Zdá se, jako kdyby se nic nestane. Je zpětné volání a hodnoty se uloží do databáze, ale neexistuje žádné visual zpětnou vazbu, která uložení došlo k chybě.

Chcete-li to opravit, vraťte k sadě Visual Studio a přidání ovládacího prvku popisek výše DetailsView. Nastavte její `ID` k `SettingsUpdatedMessage`, jeho `Text` vlastnost "vaše nastavení bylo aktualizováno," a jeho `Visible` a `EnableViewState` vlastnosti, které chcete `false`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

Potřebujeme zobrazíte `SettingsUpdatedMessage` popisku při každé aktualizaci DetailsView. K tomu, vytvoření obslužné rutiny události pro prvku DetailsView `ItemUpdated` událostí a přidejte následující kód:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

Vraťte se na `AdditionalUserInfo.aspx` stránky prostřednictvím prohlížeče a aktualizovat data. Tentokrát užitečné stavová zpráva se zobrazí.


[![Krátký zpráva se zobrazí při aktualizaci nastavení](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**Obrázek 16**: krátkou zprávu A se zobrazí, když jsou aktualizovány nastavení ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image48.png))


> [!NOTE]
> Ovládací prvek DetailsView je úpravy rozhraní nechá mnoho k být potřeby. Používá standardní velikosti textová pole, ale pole podpisu pravděpodobně by měla být Víceřádkový textové pole. RegularExpressionValidator slouží k zajištění, že adresu URL domovské stránky, je-li zadán, začne řetězcem "http://" nebo "https://". Kromě toho od DetailsView má ovládací prvek jeho `DefaultMode` vlastnost nastavena na hodnotu `Edit`, na tlačítko Storno nemá žádný. Ho měl buď odstranit, nebo při kliknutí na, přesměruje uživatele na jinou stránku (například `~/Default.aspx`). Tato vylepšení jako cvičení nechat pro čtečku.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Přidání odkazu`AdditionalUserInfo.aspx`stránky na hlavní stránce

V současné době webu neposkytuje žádné odkazy na `AdditionalUserInfo.aspx` stránky. Jediný způsob, jak bude je přímo do panelu Adresa v prohlížeči zadejte adresu URL stránky. Umožňuje přidat odkaz na tuto stránku `Site.master` stránky předlohy.

Odvolat, že stránka předlohy obsahuje LoginView webové ovládacího prvku v jeho `LoginContent` ContentPlaceHolder, která zobrazuje různých značek pro návštěvníky ověřený a anonymní. Aktualizovat ovládací prvek LoginView `LoggedInTemplate` zahrnout odkaz `AdditionalUserInfo.aspx` stránky. Po provedení těchto změn LoginView deklarativní značky ovládacího prvku by měl vypadat takto:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

Poznámka: přidání `lnkUpdateSettings` ovládacího prvku hypertextový odkaz `LoggedInTemplate`. S tímto odkazem na místě můžete rychle ověření uživatelé přejít na stránce lze zobrazit a upravit jejich domácí města, domovskou stránku a podpis nastavení.

## <a name="step-4-adding-new-guestbook-comments"></a>Krok 4: Přidání nové komentáře návštěv

`Guestbook.aspx` Je stránka, kde může ověřeným uživatelům zobrazit knihy návštěv a komentář. Začneme vytvořením rozhraní pro přidání nové návštěv komentáře.

Otevřete `Guestbook.aspx` stránka v sadě Visual Studio a následně vytvořit uživatelské rozhraní obsahující dvě TextBox – ovládací prvky, jeden pro nový komentář subjektu a jeden pro jeho obsahu. Nastavit první ovládacího prvku TextBox `ID` vlastnost `Subject` a jeho `Columns` vlastnost na 40; nastavte sekundu `ID` k `Body`, jeho `TextMode` k `MultiLine`a jeho `Width` a `Rows` Vlastnosti "95 %", 8, v uvedeném pořadí. K dokončení uživatelské rozhraní, přidání ovládacího prvku tlačítko Web s názvem `PostCommentButton` a nastavit jeho `Text` vlastnost na "Post vaše komentáře".

Vzhledem k tomu, že každý komentář návštěv vyžaduje předmět a text, přidáte RequiredFieldValidator pro každou z textových polí. Nastavte `ValidationGroup` vlastnost těchto ovládacích prvků, aby "EnterComment"; nastavte `PostCommentButton` ovládacího prvku `ValidationGroup` vlastnost "EnterComment". Další informace o ASP. Ovládací prvky na NET ověřování, podívejte se na [ověřování formuláře v technologii ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [Rozvěrače ověřovacích ovládacích prvků v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)a [ověření serveru ovládací prvky kurzu](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) na [W3Schools](http://www.w3schools.com/).

Po vytvoření uživatelského rozhraní vaší stránky deklarativní by měl vypadat přibližně takto:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

S uživatelským rozhraním dokončení naše dalším úkolem je vložit nový záznam do `GuestbookComments` tabulky, kdy `PostCommentButton` po kliknutí na. To lze provést v několika způsoby: jsme můžete napsat kód ADO.NET ve na tlačítko `Click` obslužné rutiny události; můžete přidat ovládací prvek SqlDataSource na stránku, konfigurovat jeho `InsertCommand`a pak zavolají jeho `Insert` metoda z `Click` událostí Obslužná rutina; nebo jsme může vytvářet střední vrstvy, která je zodpovědná za vkládání nové komentáře návštěv a volání této funkce `Click` obslužné rutiny události. Vzhledem k tomu, že jsme se podívali na používání SqlDataSource v kroku 3, můžeme tady použít kód ADO.NET.

> [!NOTE]
> Třídy ADO.NET pro prostřednictvím kódu programu přístup k datům z databáze Microsoft SQL Server jsou umístěné v `System.Data.SqlClient` oboru názvů. Budete muset importovat tento obor názvů do třídy modelu code-behind vaší stránky (tj, `using System.Data.SqlClient;`).


Vytvoření obslužné rutiny událostí `PostCommentButton`na `Click` událostí a přidejte následující kód:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

`Click` Obslužné rutiny události se spustí kontrola, zda uživatel zadal data jsou platná. Pokud není, obslužné rutiny události ukončen před vložení záznamu. Za předpokladu, že zadaná data jsou platná, je aktuálně přihlášený uživatel `UserId` hodnota je načíst a uložená v `currentUserId` místní proměnné. Tato hodnota je potřeba, protože jsme musíte zadat `UserId` hodnota při vložení záznamu do `GuestbookComments`.

Následující, řetězec připojení `SecurityTutorials` databáze se načítají z `Web.config` a `INSERT` je zadán příkaz SQL. A `SqlConnection` objekt je pak vytvoří a otevřít. Další, `SqlCommand` vytvoření objektu a hodnoty pro parametry použít ve `INSERT` dotazu jsou přiřazeny. `INSERT` Pak spustit příkaz a připojení ukončeno. Na konci obslužné rutiny události `Subject` a `Body` textových polí `Text` vlastnosti jsou vymazány tak, aby uživatele hodnoty nejsou zachová pro zpětné volání.

Pokračujte a otestování tuto stránku v prohlížeči. Vzhledem k tomu, že tato stránka je v `Membership` složky není přístupná pro anonymní návštěvníky. Proto musíte nejdřív přihlásit (Pokud máte ještě není). Zadejte hodnotu do `Subject` a `Body` textových polí a klikněte na `PostCommentButton` tlačítko. To způsobí, že nový záznam pro přidání do `GuestbookComments`. Na zpětné volání předmět a text, který jste zadali vymažou z textových polí.

Po kliknutí na tlačítko `PostCommentButton` tlačítko existuje je žádné vizuální zpětnou vazbu, přidání poznámky do knihy návštěv. Stále je potřeba aktualizovat této stránce zobrazit existující návštěv poznámky, které uděláme v kroku 5. Po jsme provést tuto akci, právě přidané komentář se zobrazí v seznamu komentáře, poskytnutí zpětné vazby odpovídající visual. Teď, potvrďte, že komentář návštěv byla uložena tak, že prověří obsah `GuestbookComments` tabulky.

Obrázek 17 zobrazí obsah `GuestbookComments` tabulky po byly ponechány dvě komentáře.


[![Uvidíte návštěv poznámky v tabulce GuestbookComments](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**Obrázek 17**: uvidíte návštěv poznámky v `GuestbookComments` tabulky ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image51.png))


> [!NOTE]
> Pokud se uživatel pokusí vložit návštěv komentář, který obsahuje potenciálně nebezpečné – například kód HTML – ASP.NET vyvolá výjimku `HttpRequestValidationException`. Další informace o výjimku, proč je vyvolána, a jak povolit uživatelům odesílat potenciálně nebezpečné hodnoty, najdete [dokument White Paper žádosti o ověření](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>Krok 5: Výpis existující návštěv komentáře

Kromě a komentáře, uživatele, kteří navštěvují `Guestbook.aspx` stránky také by měl být schopní zobrazit návštěv existující komentáře. K tomu, přidání ovládacího prvku ListView s názvem `CommentList` do dolní části stránky.

> [!NOTE]
> Ovládacího prvku ListView je nová technologii ASP.NET verze 3.5. Je určený k zobrazení seznamu položek ve vysoce přizpůsobitelné a flexibilní rozložení, ale stále nabízí integrovanou úpravy, vkládání, odstraňování, stránkování a řazení funkcí jako GridView. Pokud používáte technologii ASP.NET 2.0, musíte místo toho používat ovládací prvek DataList nebo opakovače. Další informace o používání ListView najdete v tématu [Scott Guthrie](https://weblogs.asp.net/scottgu/)na položku blogu, [asp: ListView řízení](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)a my článku [zobrazení dat pomocí ovládacího prvku ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Otevřete ListView inteligentních značek a z rozevíracího seznamu vyberte zdroj dat vytvořit vazbu ovládacího prvku na nový zdroj dat. Jak jsme viděli v kroku 2, tím spustíte Průvodce konfigurací zdroje dat. Vyberte ikonu pro databáze, název výsledné SqlDataSource `CommentsDataSource`a klikněte na tlačítko OK. Potom vyberte `SecurityTutorialsConnectionString` připojovací řetězec z rozevíracího seznamu a klikněte na tlačítko Další.

V tomto okamžiku v kroku 2 jsme určeného data, která mají dotazu výdej `UserProfiles` tabulky z rozevíracího seznamu a vybrat sloupce, které chcete vrátit (odkazuje zpět na obrázku 9). Tentokrát ale chceme vytvořit příkaz SQL, který vrátí zpět nejen záznamy ze `GuestbookComments`, ale také commenter domácí města, domovskou stránku, podpisu a uživatelské jméno. Proto vyberte přepínač "Zadejte vlastní příkaz SQL nebo uloženou proceduru" a klikněte na tlačítko Další.

Tím se otevře na obrazovce "Definovat vlastní příkazy nebo uložené procedury". Kliknutím na tlačítko Tvůrce dotazů graficky sestavení dotazu. Tvůrce dotazů se spustí na výzvy nám k určení, které chcete dotaz z tabulky. Vyberte `GuestbookComments`, `UserProfiles`, a `aspnet_Users` tabulky a klikněte na tlačítko OK. Všechny tři tabulky se přidá do návrhovou plochu. Vzhledem k tomu, že existují omezení cizích klíčů mezi `GuestbookComments`, `UserProfiles`, a `aspnet_Users` tabulek, Tvůrce dotazů automaticky `JOIN` s tyto tabulky.

Zbývá určit sloupce, které chcete vrátit. Z `GuestbookComments` tabulky vyberte `Subject`, `Body`, a `CommentDate` sloupce; vraťte se `HomeTown`, `HomepageUrl`, a `Signature` sloupce z `UserProfiles` tabulky; a vrátit `UserName` z `aspnet_Users`. Navíc přidat "`ORDER BY CommentDate DESC`" na konec `SELECT` dotaz tak, aby nejnovější příspěvky jsou vrácena jako první. Po provedení výběru, vaše Tvůrce dotazů rozhraní by měl vypadat snímku v 18 obrázek obrazovky.


[![Dotaz Constructed spojí GuestbookComments, UserProfiles a aspnet_Users tabulky](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**Obrázek 18**: dotaz sestavený `JOIN` s `GuestbookComments`, `UserProfiles`, a `aspnet_Users` tabulky ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image54.png))


Kliknutím na OK zavřete okno Tvůrce dotazů a vraťte se na obrazovce "Definovat vlastní příkazy nebo uložené procedury". Na tlačítko Další zálohy k obrazovce pro "Testovací dotaz", kde může zobrazit výsledky dotazu kliknutím na tlačítko Testovat dotaz. Jakmile budete připraveni, klikněte na tlačítko Dokončit dokončete průvodce Konfigurace zdroje dat.

Když jsme dokončit Průvodce konfigurace zdroje dat v kroku 2, související ovládací prvek DetailsView na `Fields` kolekce byla aktualizována zahrnout BoundField pro každý sloupec vrácený `SelectCommand`. ListView, ale zůstává beze změny; stále je potřeba definovat jeho rozložení. Rozložení ListView lze sestavit ručně pomocí jeho deklarativní nebo v jeho inteligentních značek pomocí volby "Konfigurace ListView". I obvykle preferuje ruční definování značky, ale pomocí libovolné metody je nejvíce přirozené pro vás.

I skončila pomocí následujících `LayoutTemplate`, `ItemTemplate`, a `ItemSeparatorTemplate` pro moje ListView – ovládací prvek:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

`LayoutTemplate` Definuje kód vysílaných ovládacího prvku, při `ItemTemplate` vykreslí jednotlivých položek vrácených ve třídě SqlDataSource. `ItemTemplate`Na výsledný kód je umístěn v `LayoutTemplate`na `itemPlaceholder` ovládacího prvku. Kromě `itemPlaceholder`, `LayoutTemplate` zahrnuje DataPager řízení, které omezuje ListView zobrazí právě 10 komentáře návštěv na stránce (výchozí) a vykreslí rozhraní stránkování.

Moje `ItemTemplate` zobrazí každý návštěv komentář předmět v `<h4>` element spolu s textem nacházející se pod předmět. Má data vrácená této syntaxe pro zobrazení textu `Eval("Body")` příkaz vazby dat, převede ji na řetězec a nahradí zalomení s `<br />` elementu. Tento převod je třeba zobrazit konce řádků zadaný při odesílání komentář, od prázdných znaků je ignorován v HTML. Podpis uživatele se zobrazí pod text kurzívou následuje uživatele domácí města, odkaz na jeho domovskou stránku, datum a čas vytvoření poznámky a uživatelské jméno osoby, která zbývajících komentář.

Chcete-li zobrazit stránku prostřednictvím prohlížeče chvíli trvat. Měli byste vidět komentáře, které jste přidali do návštěv v kroku 5 zobrazí tady.


[![Nyní Guestbook.aspx zobrazí návštěv komentáře](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**Obrázek 19**: `Guestbook.aspx` teď zobrazuje návštěv komentáře ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image57.png))


Zkuste přidat nový komentář knihy návštěv. Po kliknutí na tlačítko `PostCommentButton` tlačítko stránce provede zpětné a komentář se přidá do databáze, ale ovládacího prvku ListView neaktualizuje zobrazíte nový komentář. Tento problém může být vyřešený buď:

- Aktualizace `PostCommentButton` tlačítka `Click` obslužné rutiny události, které se vyvolá ovládacího prvku ListView `DataBind()` metoda po vložení nový komentář do databáze, nebo
- Nastavení ovládacího prvku ListView `EnableViewState` vlastnost `false`. Tento přístup funguje, protože zakázáním stav zobrazení ovládacího prvku musí rebind v základních datech na každé zpětné volání.

Ke stažení z tohoto kurzu na webu kurz ukazuje obě metody. Ovládacího prvku ListView `EnableViewState` vlastnost `false` a kód potřebný k prostřednictvím kódu programu rebind data, která mají ListView je k dispozici v `Click` obslužné rutiny události, ale je označeno jako komentář.

> [!NOTE]
> Aktuálně `AdditionalUserInfo.aspx` stránky umožňuje uživatelům zobrazit a upravit jejich domácí města, domovskou stránku a podpis nastavení. Může to být dobrý aktualizovat `AdditionalUserInfo.aspx` zobrazíte přihlášeného v komentářích návštěv uživatele. To znamená, kromě zkoumání a upravíte informace o ní, můžete navštívit uživatele `AdditionalUserInfo.aspx` na které uvidíte, jaké návštěv komentáře Jana se provádí v minulosti. Nechat to jako cvičení pro zúčastněným čtečku.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Krok 6: Přizpůsobení ovládacího prvku CreateUserWizard zahrnout rozhraní pro domovskou města, podpisu a domovské stránky

`SELECT` Dotazu používané `Guestbook.aspx` stránka používá `INNER JOIN` kombinování souvisejících záznamů mezi `GuestbookComments`, `UserProfiles`, a `aspnet_Users` tabulky. Pokud uživatel, který nemá žádný záznam v `UserProfiles` díky a návštěv komentář, komentář se nezobrazí v zobrazení ListView, protože `INNER JOIN` pouze vrátí `GuestbookComments` záznamů, pokud jsou k dispozici odpovídající záznamy v `UserProfiles` a `aspnet_Users`. A jak jsme viděli v kroku 3, pokud uživatel nemá záznam `UserProfiles` Jana nemůžete zobrazit nebo upravit jeho nastavení `AdditionalUserInfo.aspx` stránky.

Needless znamená kvůli naše návrhu rozhodnutí je důležité, aby všechny uživatelské účty v systému členství odpovídající záznam v `UserProfiles` tabulky. Co chceme je pro odpovídající záznam pro přidání do `UserProfiles` vždy, když se vytvoří nový členský účet uživatele prostřednictvím CreateUserWizard.

Jak je popsáno v [ *vytváření uživatelských účtů* ](creating-user-accounts-cs.md) kurzu po nový členský účet uživatele je vytvoření ovládacího prvku CreateUserWizard vyvolá jeho [ `CreatedUser` událostí](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Nemůžeme vytvořit obslužnou rutinu události pro tuto událost, získat ID uživatele pro uživatele právě vytvořené a pak vložení záznamu do `UserProfiles` tabulky s výchozími hodnotami u `HomeTown`, `HomepageUrl`, a `Signature` sloupce. Navíc je možné zobrazit výzvu uživateli pro tyto hodnoty přizpůsobením rozhraní ovládacího prvku CreateUserWizard zahrnout další textových polí.

Nejprve podíváme, jak přidat nový řádek, abyste `UserProfiles` tabulky v `CreatedUser` obslužné rutiny události s výchozími hodnotami. Následující, jsme se zobrazí postup přizpůsobení ovládacího prvku CreateUserWizard uživatelské rozhraní pro zahrnutí polí další formuláře ke shromáždění domácí města, domovskou stránku a podpis nového uživatele.

### <a name="adding-a-default-row-touserprofiles"></a>Přidávání na řádek výchozí pro`UserProfiles`

V [ *vytváření uživatelských účtů* ](creating-user-accounts-cs.md) kurzu jsme přidali CreateUserWizard ovládacího prvku k `CreatingUserAccounts.aspx` stránku `Membership` složky. Aby bylo možné používat CreateUserWizard ovládací prvek přidejte záznam do `UserProfiles` tabulky při vytváření uživatelských účtů, je potřeba aktualizovat funkce CreateUserWizard ovládacího prvku. Místo provedení těchto změn na `CreatingUserAccounts.aspx` budeme místo toho přidejte nový ovládací prvek CreateUserWizard do `EnhancedCreateUserWizard.aspx` stránky a provést změny v tomto kurzu existuje.

Otevřete `EnhancedCreateUserWizard.aspx` stránka v sadě Visual Studio a přetáhněte ovládací prvek CreateUserWizard z panelu nástrojů na stránce. Nastavení ovládacího prvku CreateUserWizard `ID` vlastnost `NewUserWizard`. Jak již bylo zmíněno <a id="_msoanchor_5"> </a> [ *vytváření uživatelských účtů* ](creating-user-accounts-cs.md) kurz, CreateUserWizard výchozí uživatelské rozhraní zobrazí výzvu návštěvníka potřebné informace. Jakmile dodal tyto informace ovládacího prvku interně vytvoří nový uživatelský účet v rámci členství bez nám nutnosti napsat jediný řádek kódu.

Ovládací prvek CreateUserWizard vyvolá určitý počet událostí během jejího pracovního postupu. Jakmile návštěvník poskytuje informace o požadavku a formulář odešle, ovládacího prvku CreateUserWizard původně aktivuje její [ `CreatingUser` událostí](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Pokud dojde k problému při procesu vytvoření [ `CreateUserError` událostí](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) je aktivována například; ale, pokud byl uživatel úspěšně vytvořen, pak se [ `CreatedUser` událostí](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) se vyvolá. V <a id="_msoanchor_6"> </a> [ *vytváření uživatelských účtů* ](creating-user-accounts-cs.md) kurzu jsme vytvořili obslužné rutiny události pro `CreatingUser` událost a ujistěte se, že se zadaným uživatelským jménem neobsahoval žádné úvodní nebo koncové mezery a že uživatelské jméno se neobjevil kdekoli v hesle.

Chcete-li přidat řádek `UserProfiles` tabulky pro uživatele právě vytvořili, je potřeba vytvořit obslužnou rutinu události pro `CreatedUser` událostí. V čase `CreatedUser` událost je aktivována, uživatelský účet již existuje v rámci členství, to nám umožňuje načíst hodnotu ID uživatele účtu.

Vytvoření obslužné rutiny událostí `NewUserWizard`na `CreatedUser` událostí a přidejte následující kód:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

Výše uvedený kód lidé načtením UserId právě přidané uživatelského účtu. Toho dosahuje pomocí `Membership.GetUser(username)` metoda vrátí informace o konkrétní uživatele a potom pomocí `ProviderUserKey` vlastnost načíst jejich ID uživatele. Uživatelské jméno zadané uživatelem v ovládacím prvku CreateUserWizard je k dispozici prostřednictvím jeho [ `UserName` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

V dalším kroku se načítají připojovací řetězec `Web.config` a `INSERT` zadán příkaz. Instalace nezbytné ADO.NET objektů a provedení příkazu. Přiřadí kód [ `DBNull` ](https://msdn.microsoft.com/en-us/library/system.dbnull.aspx) instance k `@HomeTown`, `@HomepageUrl`, a `@Signature` parametry, které má za následek vložení databáze `NULL` hodnoty `HomeTown`, `HomepageUrl`, a `Signature` pole.

Přejděte `EnhancedCreateUserWizard.aspx` stránky prostřednictvím prohlížeče a vytvořit nový uživatelský účet. Až to uděláte, vraťte se na Visual Studio a ověřit obsah `aspnet_Users` a `UserProfiles` tabulky (stejně, jako jsme udělali zpět v obrázek 12). Měli byste vidět nový uživatelský účet v `aspnet_Users` a odpovídající `UserProfiles` řádek (s `NULL` hodnoty pro `HomeTown`, `HomepageUrl`, a `Signature`).


[![Byly přidány nový uživatelský účet a UserProfiles záznam](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**Obrázek 20**: A nový uživatelský účet a `UserProfiles` byly přidány záznamu ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image60.png))


Po návštěvníka má zadaný jeho nové informace o účtu a kliknutí na tlačítko "Vytvořit uživatele", uživatelský účet je vytvořen a řádek přidán do `UserProfiles` tabulky. CreateUserWizard potom zobrazí jeho `CompleteWizardStep`, který se zobrazí zpráva o úspěšném provedení a tlačítko Pokračovat. Kliknutím na tlačítko Pokračovat způsobí, že zpětné volání, ale nebyla provedena žádná akce, a uživatel zablokované na `EnhancedCreateUserWizard.aspx` stránky.

Lze zadat adresu URL k odesílání uživatele do když po kliknutí na tlačítko Pokračovat prostřednictvím ovládacího prvku CreateUserWizard [ `ContinueDestinationPageUrl` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Nastavte `ContinueDestinationPageUrl` vlastnost "~ / Membership/AdditionalUserInfo.aspx". Tato akce trvá nového uživatele, který `AdditionalUserInfo.aspx`, kde můžete zobrazit a aktualizovat svoje nastavení.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Přizpůsobení CreateUserWizard rozhraní výzva k domovské města, domovskou stránku a podpis nového uživatele

Ovládací prvek CreateUserWizard výchozí rozhraní je dostačující pro scénáře vytvoření jednoduchého účtu, kdy potřebují shromažďují pouze základní informace o uživatelském účtu jako uživatelské jméno, heslo a e-mailu. Ale co když jsme chtěli výzvu návštěvníka zadejte svůj domácí města, domovskou stránku a podpis při vytváření svůj účet? Je možné přizpůsobit rozhraní ovládacího prvku CreateUserWizard ke sběru dalších informací při registraci, a tyto informace mohou být používány `CreatedUser` obslužné rutiny události vložení další záznamy do základní databáze.

Ovládací prvek CreateUserWizard rozšiřuje ovládací prvek ASP.NET průvodce, který je ovládací prvek, který umožňuje vývojářům definovat řadu seřazené `WizardSteps`. Ovládací prvek Průvodce vykreslí active krok a poskytuje navigační rozhraní, které umožňuje návštěvníka přesunout pomocí těchto kroků. Řízení Průvodce je ideální pro rozdělení dlouho úloh do několika krocích. Další informace o řízení průvodce najdete v tématu [vytváření podrobné uživatelské rozhraní pomocí ovládacího prvku ASP.NET 2.0 průvodce](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Ovládací prvek CreateUserWizard výchozí značky definují dva `WizardSteps`: `CreateUserWizardStep` a `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

První `WizardStep`, `CreateUserWizardStep`, vykreslí rozhraní, které zobrazí výzvu pro uživatelské jméno, heslo, e-mailu a tak dále. Po návštěvníka poskytne tyto údaje a klikne na tlačítko "Vytvořit uživatele", která se zobrazí `CompleteWizardStep`, která zobrazuje zprávy Úspěch a tlačítko Pokračovat.

Chcete-li přizpůsobit rozhraní CreateUserWizard ovládacího prvku pro zahrnutí polí další formuláře, můžeme:

- **Vytvořte jeden nebo více nových ***`WizardStep`*** s tak, aby obsahovala další prvky uživatelského rozhraní**. Chcete-li přidat nový `WizardStep` chcete CreateUserWizard, klikněte na tlačítko "Přidat nebo odebrat `WizardSteps`" odkaz z jeho inteligentních značek ke spuštění `WizardStep` Editor kolekce. Odtud můžete přidat, odebrat nebo změnit pořadí kroků v průvodci. Toto je přístupů, které budeme používat pro účely tohoto kurzu.

- **Převést ***`CreateUserWizardStep`*** do upravitelné ***`WizardStep`***.** Tím se nahradí `CreateUserWizardStep` s ekvivalentní `WizardStep` jejichž poznámky definuje uživatelské rozhraní, které odpovídá `CreateUserWizardStep`' s. Převedením `CreateUserWizardStep` do `WizardStep` jsme můžete změnit umístění ovládacích prvků nebo přidat další prvky uživatelského rozhraní pro tento krok. Převést `CreateUserWizardStep` nebo `CompleteWizardStep` do upravitelné `WizardStep`, klikněte "upravit vytvořit uživateli krok" nebo "Přizpůsobit kroku dokončení" odkaz z inteligentní značky ovládacího prvku.

- **Pomocí některé kombinace výše uvedených dvou možností.**

Je důležité mít na paměti, mějte na paměti, ovládacího prvku CreateUserWizard provede jeho procesu vytvoření účtu uživatele při kliknutí na tlačítko "Vytvořit uživatele" v nástroji jeho `CreateUserWizardStep`. Není důležité, pokud existují další `WizardStep` s po `CreateUserWizardStep` nebo ne.

Při přidání vlastního `WizardStep` do ovládacího prvku CreateUserWizard shromažďovat další uživatelský vstup, vlastní `WizardStep` může před nebo po `CreateUserWizardStep`. Pokud se nachází před `CreateUserWizardStep` pak další uživatelský vstup shromážděných z vlastní `WizardStep` je k dispozici pro `CreatedUser` obslužné rutiny události. Ale pokud vlastní `WizardStep` dodává po `CreateUserWizardStep` pak podle času vlastní `WizardStep` se zobrazí nový uživatelský účet již existuje a `CreatedUser` již byla aktivována událost.

21 obrázek ukazuje pracovní postup při přidaném `WizardStep` předchází `CreateUserWizardStep`. Vzhledem k tomu, že další uživatelské informace byly získány v čase `CreatedUser` se aktivuje událost, všechny se musí provést aktualizaci `CreatedUser` obslužné rutiny události načtení těchto vstupy a pro použití `INSERT` hodnoty parametrů pro příkaz (místo `DBNull.Value`).


[![Pokud Třída CreateUserWizardStep předchází dalších kroků průvodce CreateUserWizard pracovního postupu](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**Obrázek 21**: CreateUserWizard pracovního postupu při další `WizardStep` Precedes `CreateUserWizardStep` ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image63.png))


Pokud vlastní `WizardStep` je umístěn *po* `CreateUserWizardStep`, ale dojde k procesu vytvoření účtu uživatele předtím, než uživatel dostal příležitost k zadání svůj domácí Město, domovskou stránku nebo podpis. V takovém případě je potřeba vložit do databáze po vytvoření uživatelského účtu, jak ukazuje obrázek 22 tyto další informace.


[![Při dalších kroků průvodce dodává po třídu CreateUserWizardStep CreateUserWizard pracovního postupu](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**Obrázek 22**: CreateUserWizard pracovního postupu při další `WizardStep` dodává po `CreateUserWizardStep` ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image66.png))


Pracovní postup znázorňuje obrázek 22 čeká k vložení záznamu do `UserProfiles` tabulky až po dokončení kroku 2. Pokud návštěvníka zavře svůj prohlížeč po kroku 1, ale jsme bude bylo dosaženo stavu, kdy byl vytvořený uživatelský účet, ale žádný záznam byla přidána do `UserProfiles`. Jeden řešením je záznam obsahuje `NULL` nebo výchozí hodnoty, které jsou vloženy do `UserProfiles` v `CreatedUser` obslužné rutiny události (který aktivuje po kroku 1) a pak aktualizaci tento záznam po dokončení kroku 2. To zajistí, že `UserProfiles` záznam přidán pro uživatelský účet i v případě, že uživatel ukončí polovině proces registrace prostřednictvím.

Pro účely tohoto kurzu vytvoříme novou `WizardStep` k tomu dojde po `CreateUserWizardStep` ale předtím, než `CompleteWizardStep`. První get kroků průvodce v umístěte a nakonfigurovat a potom jsme budete Podíváme se na kód.

Inteligentní značky CreateUserWizard ovládacího prvku, vyberte "Přidat nebo odebrat `WizardStep` s", který spustí `WizardStep` dialogové okno Editor kolekcí. Přidejte nový `WizardStep`, nastavení jeho `ID` k `UserSettings`, jeho `Title` "Vaše nastavení" a jeho `StepType` k `Step`. Umístěte ho tak, aby po přechodu `CreateUserWizardStep` ("zaregistrovat pro váš nový účet") a před `CompleteWizardStep` ("úplná"), jak ukazuje obrázek 23.


[![Přidání nové kroků průvodce CreateUserWizard ovládacího prvku](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**Obrázek 23**: Přidejte nové `WizardStep` do ovládacího prvku CreateUserWizard ([Kliknutím zobrazit obrázek v plné velikosti](storing-additional-user-information-cs/_static/image69.png))


Kliknutím na OK zavřete `WizardStep` dialogové okno Editor kolekcí. Nové `WizardStep` svědčí deklarativní aktualizované CreateUserWizard ovládacího prvku:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

Všimněte si nové `<asp:WizardStep>` elementu. Je potřeba přidat uživatelské rozhraní ke shromažďování domácí města, domovskou stránku a podpis zde nového uživatele. Tento obsah můžete zadat v deklarativní syntaxi nebo prostřednictvím návrháře. Chcete-li použít Návrháře dotazů, vyberte krok "Vaše nastavení" z rozevíracího seznamu ve značce inteligentní zobrazíte krok v návrháři.

> [!NOTE]
> Výběr krok prostřednictvím inteligentní značky rozevíracího seznamu aktualizací prvku CreateUserWizard [ `ActiveStepIndex` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), která určuje index počáteční krok. Proto pokud upravit krok "Vaše nastavení" v Návrháři pomocí tohoto rozevíracího seznamu, je nutné ho nastavit zpět na "Přihlášení k si nový účet", aby tento krok se zobrazí, když uživatelé navštíví nejdřív `EnhancedCreateUserWizard.aspx` stránky.


Vytvoření uživatelského rozhraní v rámci "Vaše nastavení" krok, který obsahuje tři ovládací prvky textové pole s názvem `HomeTown`, `HomepageUrl`, a `Signature`. Po vytváření toto rozhraní, CreateUserWizard deklarativní by měl vypadat takto:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

Pokračujte a najdete na této stránce prostřednictvím prohlížeče a vytvořit nový uživatelský účet, zadání hodnoty domácí města, domovskou stránku a podpis. Po dokončení `CreateUserWizardStep` uživatelský účet je vytvořen v rámci členství a `CreatedUser` spustí obslužnou rutinu události, které přidá nový řádek na `UserProfiles`, ale s databází `NULL` hodnota `HomeTown`, `HomepageUrl`, a `Signature`. Hodnoty zadané pro podpisu domácí města, domovskou stránku a nikdy se používají. Net výsledkem je nový uživatelský účet s `UserProfiles` záznam, jehož `HomeTown`, `HomepageUrl`, a `Signature` pole ještě nutné zadat.

Potřebujeme ke spouštění kódu po kroku "Vaše nastavení", který přijímá domácí města, honepage a podpis hodnoty zadaného uživatelem a aktualizuje příslušné `UserProfiles` záznamu. Pokaždé, když uživatel přesune mezi kroků v Průvodci řídit, v průvodci [ `ActiveStepChanged` událostí](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) aktivuje. Můžeme vytvořit obslužnou rutinu události pro tuto událost a aktualizace `UserProfiles` tabulky po dokončení kroku "Vaše nastavení".

Přidání obslužné rutiny události pro CreateUserWizard `ActiveStepChanged` událostí a přidejte následující kód:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

Výše uvedený kód spustí na určení toho, jestli jsme právě dosáhli "Dokončit" krok. Vzhledem k tomu, že krok "Dokončit" nachází hned za krok "Vaše nastavení", pak když dosáhne návštěvníka "Úplná" krok, znamená, že se právě dokončila "Vaše nastavení" krok.

V takovém případě je potřeba programově odkazovat ovládacích prvcích TextBox v rámci `UserSettings WizardStep`. Toho dosahuje pomocí prvního `FindControl` metodu prostřednictvím kódu programu odkazující na `UserSettings WizardStep`a potom znovu tak, aby odkazovaly textová pole uvnitř `WizardStep`. Jakmile máte bylo odkazováno textová pole, je vše připraveno k provedení `UPDATE` příkaz. `UPDATE` Příkaz má stejný počet parametrů jako `INSERT` příkaz v `CreatedUser` obslužné rutiny události, ale zde použijeme domácí města, domovskou stránku a podpis hodnoty zadané uživatelem.

Pomocí této obslužné rutiny události na místě, přejděte `EnhancedCreateUserWizard.aspx` stránky prostřednictvím prohlížeče a vytvořit nový uživatelský účet zadání hodnoty domácí města, domovskou stránku a podpis. Po vytvoření nového účtu měli byste být přesměrovaní na `AdditionalUserInfo.aspx` stránky, kde právě zadali domácí města, domovskou stránku a podpis se zobrazují informace.

> [!NOTE]
> Náš web aktuálně má dvě stránky, ze kterých návštěvník můžete vytvořit nový účet: `CreatingUserAccounts.aspx` a `EnhancedCreateUserWizard.aspx`. Přejděte na webu sitemap a přihlašovací stránky `CreatingUserAccounts.aspx` stránky, ale `CreatingUserAccounts.aspx` stránky není požádat uživatele o domácí města, domovskou stránku a podpis informace a nepřidá odpovídající řádek, abyste `UserProfiles`. Proto buď aktualizovat `CreatingUserAccounts.aspx` stránky tak, aby tato funkce nabízí nebo aktualizovat stránku sitemap a přihlaste se k odkazování `EnhancedCreateUserWizard.aspx` místo `CreatingUserAccounts.aspx`. Pokud si zvolíte druhou možnost, nezapomeňte aktualizovat `Membership` složky `Web.config` tak, aby anonymní uživatelé přístup k souboru `EnhancedCreateUserWizard.aspx` stránky.


## <a name="summary"></a>Souhrn

V tomto kurzu jsme se podívali na techniky pro modelování data, která souvisí se uživatelské účty v rámci členství. Konkrétně jsme se podívali na modelování entit, které sdílejí vztah jeden mnoho se uživatelské účty, jakož i data, která sdílí relace. Kromě toho jsme viděli, jak to související informace může zobrazit, Vložit a aktualizuje, s příklady pomocí ovládacího prvku SqlDataSource a jiné pomocí ADO.NET kódu.

V tomto kurzu dokončení naše podívejte se na uživatelské účty. Od verze v dalším kurzu jsme zapnout naše pozornost role. Přes další několik kurzy se podíváme na rozhraní rolí, najdete v části Postup vytvoření nové role, jak přiřadit role pro uživatele, jak určit, jaké role uživatel patří do a jak pro použití ověřování na základě rolí.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Přístup k informacím a aktualizace dat v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Ovládací prvek ASP.NET 2.0 Průvodce](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Vytváření pomocí ovládacího prvku ASP.NET 2.0 průvodce krok za krokem uživatelské rozhraní](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Vytváření vlastní zdroj dat ovládacího prvku parametrů](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Přizpůsobení ovládacího prvku CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Ovládací prvek DetailsView – elementy QuickStart](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Zobrazení dat pomocí ovládacího prvku ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Rozvěrače ověření ovládacích prvků technologie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Vložení úpravy a odstraňování dat](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Ověření formuláře technologie ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Informace o shromažďování vlastního uživatelského registraci](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profily v technologii ASP.NET 2.0](http://www.odetocode.com/Articles/440.aspx)
- [Asp: ListView ovládací prvek](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Rychlý start profily uživatelů](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování...

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Předchozí](user-based-authorization-cs.md)
[další](creating-the-membership-schema-in-sql-server-vb.md)
