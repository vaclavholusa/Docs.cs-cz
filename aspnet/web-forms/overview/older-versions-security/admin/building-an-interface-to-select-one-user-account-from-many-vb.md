---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: Vytvoření rozhraní pro výběr jednoho uživatelského účtu z mnoha (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu vytvoříme uživatelského rozhraní pomocí mřížky stránkovaného, filtrování. Zejména našeho uživatelského rozhraní se skládají z řady z LinkButtons pro...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: ec257b09209cb1a377f1ae93b58db4469f438a46
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373188"
---
<a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>Vytvoření rozhraní pro výběr jednoho uživatelského účtu z mnoha (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> V tomto kurzu vytvoříme uživatelského rozhraní pomocí mřížky stránkovaného, filtrování. Zejména našeho uživatelského rozhraní bude skládat z řady LinkButtons pro filtrování výsledků podle počáteční písmeno uživatelské jméno a ovládacího prvku GridView, zobrazíte odpovídající uživatelé. Začneme obsahující všechny uživatelské účty v GridView. Pak v kroku 3, přidáme filtr LinkButtons. Krok 4 zjistí stránkování filtrované výsledky. Rozhraní vytvořený v kroky 2 až 4 se použije v dalších kurzech k provádění úloh správy pro konkrétní uživatelský účet.


## <a name="introduction"></a>Úvod

V <a id="_msoanchor_1"> </a> [ *přiřazení rolí uživatelům* ](../roles/assigning-roles-to-users-vb.md) kurzu jsme vytvořili základní rozhraní pro správce zajišťující vyberte uživatele a spravovat své role. Konkrétně rozhraní zobrazí správce se rozevírací seznam všech uživatelů. Toto rozhraní je vhodný, pokud existují, ale s deseti uživatelské účty, ale nepraktické pro weby se stovkami nebo tisícovkami účtů. Stránkovaná, filtrovatelné grid je vhodnější uživatelského rozhraní pro weby s velké uživatelské základny.

V tomto kurzu vytvoříme uživatelské rozhraní. Zejména našeho uživatelského rozhraní bude skládat z řady LinkButtons pro filtrování výsledků podle počáteční písmeno uživatelské jméno a ovládacího prvku GridView, zobrazíte odpovídající uživatelé. Začneme obsahující všechny uživatelské účty v GridView. Pak v kroku 3, přidáme filtr LinkButtons. Krok 4 zjistí stránkování filtrované výsledky. Rozhraní vytvořený v kroky 2 až 4 se použije v dalších kurzech k provádění úloh správy pro konkrétní uživatelský účet.

Pusťme se do práce!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1: Přidání nové stránky ASP.NET

V tomto kurzu a další dvě jsme se zkoumání různých funkcí souvisejících s správy a možnosti. Potřebujeme sérii stránek ASP.NET k implementaci témata prozkoumat v rámci těchto kurzů. Pojďme vytvořit tyto stránky a aktualizaci mapy webu.

Začněte tím, že vytvoříte novou složku v projektu s názvem `Administration`. V dalším kroku přidejte dvě nové stránky ASP.NET do složky, propojení s každou stránku `Site.master` stránky předlohy. Název stránky:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Přidat do kořenového adresáře webu také dvě stránky: `ChangePassword.aspx` a `RecoverPassword.aspx`.

Tyto čtyři stránky, v tomto okamžiku má dva ovládací prvky obsahu, jeden pro každou z prvků ContentPlaceHolder na hlavní stránce: `MainContent` a `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

Má být zobrazena na hlavní stránce výchozí značky `LoginContent` ContentPlaceHolder pro tyto stránky. Proto odebrat deklarativní `Content2` ovládacího prvku obsahu. Až to uděláte, na stránkách značek by měl obsahovat pouze jeden ovládací prvek obsahu.

Stránky technologie ASP.NET v `Administration` složky jsou určené výhradně pro administrativní uživatelé. Přidali jsme do systému v roli správce <a id="_msoanchor_2"> </a> [ *vytváření a Správa rolí* ](../roles/creating-and-managing-roles-vb.md) kurz; omezit přístup na tyto dvě stránky do této role. Chcete-li to provést, přidejte `Web.config` do souboru `Administration` složky a nakonfigurovat jeho `<authorization>` element připouštějí uživatelé v roli správce a zakázat všechny ostatní.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

Průzkumník řešení vašeho projektu v tomto okamžiku by měl vypadat podobně jako obrazovky je vidět na obrázku 1.


[![Čtyři nové stránky a v souboru Web.config se přidaly na web](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**Obrázek 1**: čtyři nové stránky a `Web.config` soubor byly přidány k webu ([kliknutím ji zobrazíte obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))


Nakonec aktualizujte mapy webu (`Web.sitemap`) zahrnout položku pro `ManageUsers.aspx` stránky. Přidejte následující kód XML po `<siteMapNode>` jsme přidali kurzy role.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

Pomocí mapy webu, aktualizovat přejděte na web prostřednictvím prohlížeče. Jak je vidět na obrázku 2, navigaci na levé straně teď obsahuje položky pro správu kurzy.


[![Mapa webu obsahuje uzel s názvem Správa uživatelů](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**Obrázek 2**: mapy webu obsahuje uzel s názvem Správa uživatelů ([kliknutím ji zobrazíte obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Krok 2: Výpis všech uživatelských účtů v GridView

Naším cílem end pro účely tohoto kurzu je vytvoření stránkované, filtrování mřížky, přes který může správce vybrat uživatelský účet ke správě. Začněme tím, že výpis *všechny* uživatelů v GridView. Po jejím dokončení přidáme filtrování a stránkování rozhraní a funkce.

Otevřít `ManageUsers.aspx` stránku `Administration` složky a přidejte prvku GridView, nastavení jeho `ID` k `UserAccounts` ve chvíli, napíšeme kód k vytvoření vazby sadu uživatelských účtů pomocí ovládacího prvku GridView `Membership` třídy `GetAllUsers` – metoda. Jak je popsáno v předchozích kurzech se `GetAllUsers` metoda vrátí hodnotu `MembershipUserCollection` objekt, což je kolekce z `MembershipUser` objekty. Každý `MembershipUser` v kolekci obsahuje vlastnosti, jako je `UserName`, `Email`, `IsApproved`, a tak dále.

Pokud chcete zobrazit informace o požadované uživatelském účtu v prvku GridView, nastavte prvku GridView `AutoGenerateColumns` vlastnost na hodnotu False a přidejte BoundFields pro `UserName`, `Email`, a `Comment` vlastnosti a CheckBoxFields pro `IsApproved`, `IsLockedOut`, a `IsOnline` vlastnosti. Tuto konfiguraci můžete použít prostřednictvím ovládacího prvku deklarativní nebo dialogového okna pole. Obrázek 3 ukazuje snímek obrazovky pole poté, co bylo zrušeno na zaškrtávací políčko automaticky generovat pole a BoundFields a CheckBoxFields jsme přidali a nakonfigurovali dialogové okno.


[![Přidejte tři BoundFields a tři CheckBoxFields do prvku GridView.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**Obrázek 3**: Přidejte tři BoundFields a tři CheckBoxFields do prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))


Po dokončení konfigurace vašeho ovládacího prvku GridView, ujistěte se, že jeho deklarativním označení vypadá přibližně takto:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

Dále musíme napsat kód, který váže uživatelské účty do prvku GridView. Vytvořit metodu s názvem `BindUserAccounts` k provedení této úlohy a pak volejte jej z `Page_Load` obslužné rutiny události při první návštěvě stránky.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

Za chvíli testovací stránka prostřednictvím prohlížeče. Obrázek 4 ukazuje, `UserAccounts` GridView obsahuje uživatelské jméno, e-mailovou adresu a další relevantní účtu informace pro všechny uživatele v systému.


[![Uživatelské účty vypisují v prvku GridView.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**Obrázek 4**: The uživatelské účty vypisují v prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Krok 3: Filtrování výsledků podle první písmeno uživatelského jména

Aktuálně `UserAccounts` GridView ukazuje *všechny* uživatelských účtů. Pro moduly websites se stovkami nebo tisícovkami uživatelských účtů je nutné tento uživatel moct rychle Zredukovat zobrazené účty. To lze provést přidáním filtrování LinkButtons na stránku. Přidejme 27 LinkButtons na stránku: jednu s názvem všechny spolu s jeden odkazem (LinkButton) pro každé písmeno abecedy. Návštěvník klikne všechny odkazem (LinkButton), prvku GridView zobrazí všechny uživatele. Pokud kliknou určitým písmenem, zobrazí se pouze uživatelé, jejichž uživatelské jméno začíná vybrané písmeno.

Naše první úloha je přidání 27 ovládací prvky odkazem (LinkButton). Jednou z možností je vytvořit 27 LinkButtons deklarativně, postupně po jednom. Flexibilnější přístupem je použití prvku opakovače s `ItemTemplate` , který vykreslí odkazem (LinkButton) a potom sváže Repeater jako možnosti filtrování `String` pole.

Začněte tím, že přidání ovládacím prvkem Repeater na stránku výše `UserAccounts` ovládacího prvku GridView. Nastavte Repeater `ID` vlastnost `FilteringUI` konfigurace šablon Repeateru tak, aby jeho `ItemTemplate` vykreslí odkazem (LinkButton) jehož `Text` a `CommandName` vlastnosti je vázána na aktuální prvek pole. Jak jsme viděli v <a id="_msoanchor_3"> </a> [ *přiřazení rolí uživatelům* ](../roles/assigning-roles-to-users-vb.md) výukový program, můžete to udělat pomocí `Container.DataItem` Syntaxe datové vazby. Použít Repeater `SeparatorTemplate` zobrazíte svislé čáry mezi každého odkazu.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

K naplnění této opakovače s požadovanými možnostmi filtrování, vytvořit metodu s názvem `BindFilteringUI`. Nezapomeňte volat tuto metodu z `Page_Load` obslužné rutiny události na prvním načtením stránky.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

Tato metoda určuje možnosti filtrování jako prvky `String` pole `filterOptions` pro každý prvek v poli Opakovači vykreslí prvek LinkButton s jeho `Text` a `CommandName` vlastnosti přiřazená hodnota pole element.

Obrázek 5 ukazuje, `ManageUsers.aspx` stránce při prohlížení prostřednictvím prohlížeče.


[![Opakovači uvádí 27 filtrování LinkButtons](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**Obrázek 5**: The Repeater uvádí 27 filtrování LinkButtons ([kliknutím ji zobrazíte obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))


> [!NOTE]
> Uživatelská jména, může začínat libovolný znak, včetně čísla a interpunkční znaménka. Chcete-li zobrazit tyto účty, bude správce muset použít možnost všechny odkazem (LinkButton). Alternativně můžete přidat odkazem (LinkButton) se vraťte všechny uživatelské účty, které začínat číslicí. Můžu ponechte toto cvičení pro čtečku.


Kliknutím na některý z filtrování LinkButtons vyvolá zpětné volání a vyvolá Repeateru `ItemCommand` události, ale není žádná změna v mřížce, protože jsme ještě na psaní jakéhokoli kódu k filtrování výsledků. `Membership` Obsahuje třídy [ `FindUsersByName` metoda](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) , která vrací tyto uživatelské účty, jejichž uživatelské jméno odpovídá zadanému vyhledávacímu vzoru. Můžeme použít tuto metodu pro načtení pouze tyto uživatelské účty, jejichž uživatelská jména začínají písmenem určený `CommandName` z filtrované odkazem (LinkButton), který bylo kliknuto.

Začněte tím, že aktualizace `ManageUser.aspx` třídy stránky kódu tak, že obsahují vlastnost s názvem `UsernameToMatch` tato vlastnost se uchovávají napříč postbacků řetězec filtru uživatelské jméno:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

`UsernameToMatch` Jeho hodnota je přiřazen do vlastnosti se ukládají `ViewState` kolekce pomocí klíče UsernameToMatch. Pokud je hodnota této vlastnosti pro čtení, zkontroluje, zda existuje hodnota v `ViewState` kolekce; v opačném případě ji vrátí výchozí hodnotu, prázdný řetězec. `UsernameToMatch` Vlastnost vykazuje běžným vzorem, a to uložením hodnotu tak, aby postbacků jsou trvalé změny vlastnosti stavu zobrazení. Další informace o tomto vzoru, najdete v článku [stav zobrazení ASP.NET Principy](https://msdn.microsoftn-us/library/ms972976.aspx).

Dále, aktualizujte `BindUserAccounts` metoda takže místo volání `Membership.GetAllUsers`, volá `Membership.FindUsersByName`a předejte hodnotu `UsernameToMatch` vlastnost připojená se zástupným znakem SQL %.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

Pouze uživatelé, jejichž uživatelské jméno začíná písmeno A zobrazit, nastavte `UsernameToMatch` vlastnost na objekt a následně zavolat `BindUserAccounts` by výsledkem volání `Membership.FindUsersByName("A%")`, která vrátí všechny uživatele, uživatelské jméno začíná A. Podobně vrátit *všechny* uživatele, přiřazovat prázdného řetězce `UsernameToMatch` vlastnost tak, aby `BindUserAccounts` metodou vyvolán `Membership.FindUsersByName("%")`, a tím vrátí všechny uživatelské účty.

Vytvořte obslužnou rutinu události pro Repeater `ItemCommand` událostí. Tato událost je vyvolána pokaždé, když se jeden z filtrů LinkButtons dojde ke kliknutí na; je jí předán na kliknutí na prvek LinkButton `CommandName` hodnotu prostřednictvím `RepeaterCommandEventArgs` objektu. Musíme odpovídající hodnotu přiřadit `UsernameToMatch` vlastnost a poté zavolejte `BindUserAccounts` metody. Pokud `CommandName` je vše, přiřaďte prázdného řetězce `UsernameToMatch` tak, aby se zobrazují všechny uživatelské účty. V opačném případě přiřadit `CommandName` hodnotu `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

S tímto kódem na místě otestujte si moct funkce filtrování. Při první návštěvě stránky, se zobrazují všechny uživatelské účty (vrátit zpět k obrázek 5). Kliknutí na prvek LinkButton A vyvolá zpětné volání a vyfiltruje výsledky, pouze uživatelské účty, které začínají na A zobrazení.


[![Pomocí filtrování LinkButtons můžete zobrazit tyto uživatele, jejichž uživatelské jméno začíná určitým písmenem](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**Obrázek 6**: použít k zobrazení těchto uživatelů jejichž uživatelské jméno začíná písmenem některých LinkButtons filtrování ([kliknutím ji zobrazíte obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Krok 4: Aktualizace pomocí stránkování prvku GridView.

GridView je zobrazen v obrázcích 5 a 6 obsahuje seznam všech záznamů vrácených z `FindUsersByName` metody. Pokud existují stovky nebo tisíce uživatelské účty to může vést k přetížení informace při zobrazení všech účtů (stejně jako v případě při kliknutí na všechny odkazem (LinkButton), nebo když zpočátku na stránce). Chcete-li k dispozici jsou uživatelské účty ve více spravovatelných blocích, nakonfigurujeme prvku GridView. Chcete-li zobrazit 10 uživatelských účtů najednou.

Ovládací prvek GridView nabízí dva typy stránkování:

- **Výchozí stránkování** – snadno se implementuje, ale neefektivní. Řečeno v kostce, s výchozí stránkování prvku GridView očekává *všechny* záznamů z jeho datového zdroje. Potom pouze zobrazí odpovídající stránku záznamy.
- **Vlastní stránkování** -vyžaduje více práce na implementaci, ale je mnohem efektivnější než výchozí stránkování, protože s vlastní stránkování dat zdroje vrátí pouze přesnou sadu záznamů pro zobrazení.

Při procházení tisíc záznamů, může být poměrně podstatné rozdíly ve výkonnosti mezi výchozí a vlastní stránkování. Vzhledem k tomu, že vytváříme může toto rozhraní předpokladu, že se stovkami nebo tisícovkami objektů uživatelské účty, použijeme vlastní stránkování.

> [!NOTE]
> Podrobnější informace o rozdílech mezi výchozí a vlastní stránkování, jakož i výzvy při implementaci vlastní stránkování, najdete v tématu [efektivně stránkování prostřednictvím velkých objemů dat](https://asp.net/learn/data-access/tutorial-25-vb.aspx). Analýzy výkonu rozdílu mezi výchozí a vlastní stránkování, naleznete v tématu [vlastní stránkování v ASP.NET s SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Chcete-li implementovat vlastní stránkování, potřebujeme některé mechanismus, podle kterého chcete získat přesné podmnožinu záznamů se zobrazuje v prvku GridView. Dobrou zprávou je, že `Membership` třídy `FindUsersByName` metoda má přetížení, která umožňuje určit index stránky a velikost stránky a vrátí pouze uživatelské účty, které spadají do tohoto rozsahu záznamů.

Konkrétně se toto přetížení má následující podpis: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

*PageIndex* parametr určuje stránce uživatelské účty k vrácení; *pageSize* označuje, kolik záznamů se má zobrazit na stránce. *TotalRecords* parametr je `ByRef` parametr, který vrátí počet celkový počet uživatelských účtů do úložiště uživatele.

> [!NOTE]
> Data vrácená `FindUsersByName` je seřazený podle uživatelského jména; kritéria řazení nejde přizpůsobit.


Využívat vlastní stránkování, ale pouze při vázání na ovládací prvek ObjectDataSource, je možné nakonfigurovat prvku GridView. Pro ovládací prvek ObjectDataSource implementovat vlastní stránkování, vyžaduje dvě metody: ten, který je předán index počátečního řádku a maximální počet záznamů, které chcete zobrazit, a vrátí přesné podmnožinu záznamů, které spadají do rozsahu; a metodu, která vrátí celkový počet záznamů stránkování prostřednictvím. `FindUsersByName` Přetížení přijímá index stránky a velikost stránky a vrátí celkový počet záznamů prostřednictvím `ByRef` parametru. Proto je nesoulad rozhraní.

Jednou z možností je vytvořit třídu proxy, která vystavuje rozhraní ObjectDataSource očekává, že a interně volá `FindUsersByName` metody. Další možnost – a jeden že pro účely tohoto článku použijeme – je vytvořit vlastní rozhraní stránkování, který budete používat namísto integrovaných rozhraní stránkování prvku GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Vytvoření první, předchozí, v dalším kroku naposledy rozhraní stránkování

První, Previous, další a poslední LinkButtons Vytvořme rozhraní stránkování. Na první prvek LinkButton, po kliknutí na přenese uživatele na první stránku dat, zatímco předchozí ho vrátit na předchozí stránku. Podobně další a poslední přesune uživatele na další a poslední stránky, v uvedeném pořadí. Přidejte čtyři ovládací prvky odkazem (LinkButton) pod `UserAccounts` ovládacího prvku GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

Dále vytvořte obslužnou rutinu události pro každou na prvek LinkButton `Click` události.

Obrázek 7 znázorňuje čtyři LinkButtons při prohlížení prostřednictvím vizuálního návrhu Web pro vývojáře.


[![Dále přidejte první, předchozí, a naposledy LinkButtons pod prvku GridView.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**Obrázek 7**: přidejte první, Previous, další a poslední LinkButtons pod prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Udržování přehledu o Index aktuální stránky

Když první uživatel navštíví `ManageUsers.aspx` stránky nebo kliknutí jeden z filtrování tlačítek, chceme zobrazit první stránku dat v prvku GridView. Po kliknutí na jednu z navigační LinkButtons, ale potřebujeme aktualizovat index stránky. Udržovat index stránky a počet záznamů zobrazených na stránce, přidejte následující dvě vlastnosti do třídy modelu code-behind na stránce:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

Podobně jako `UsernameToMatch` vlastnost, `PageIndex` vlastnost nevyřeší jeho hodnotu na zobrazení stavu. Jen pro čtení `PageSize` vlastnost vrací hodnotu pevně zakódované, 10. Můžu pozvání zúčastněným čtečky aktualizovat tuto vlastnost používat stejný vzor jako `PageIndex`a potom k posílení `ManageUsers.aspx` stránce tak, aby osoba na stránce můžete zadat, kolik uživatelských účtů zobrazených na stránce.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Načítání pouze aktuální stránku záznamy, aktualizuje se Index stránky a povolení a zakázání LinkButtons stránkovacího rozhraní

Pomocí rozhraní stránkování na místě a `PageIndex` a `PageSize` přidat vlastnosti jsme připraveni na aktualizaci `BindUserAccounts` metodu tak, že používá odpovídající `FindUsersByName` přetížení. Kromě toho jsme musí být tato metoda povolí nebo zakáže rozhraní stránkování v závislosti na tom, jaké stránky se zobrazí. Při prohlížení na první stránce data, by mělo být zakázáno první a předchozí odkazy; Dále a poslední je třeba zakázat při prohlížení poslední stránky.

Aktualizace `BindUserAccounts` metodu s následujícím kódem:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

Všimněte si, že celkový počet záznamů stránkování prostřednictvím je určeno poslední parametr `FindUsersByName` metody. Poté, co vrátí zadanou stránku uživatelské účty, čtyři LinkButtons jsou zapnutá nebo vypnutá, v závislosti na tom, zda je na první nebo poslední stránku dat zobrazení.

Posledním krokem je napsat kód pro čtyři LinkButtons `Click` obslužných rutin událostí. Tyto obslužné rutiny událostí, musíte si aktualizovat `PageIndex` vlastnost a poté znovu připojit data, která mají GridView prostřednictvím volání `BindUserAccounts` prvním, předchozí a další obslužné rutiny událostí jsou velmi jednoduché. `Click` Obslužné rutiny události pro poslední odkazem (LinkButton), ale o něco složitější protože potřebujeme k určení, kolik záznamů se zobrazuje aby bylo možné zjistit poslední index stránky.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

Obrázky 8 a 9 zobrazit rozhraní vlastní stránkování v akci. Obrázek 8 ukazuje `ManageUsers.aspx` stránce při prohlížení na první stránku dat pro všechny uživatelské účty. Všimněte si, že se zobrazují jenom 10 13 účtů. Kliknutím na odkaz Další nebo poslední vyvolá zpětné volání, aktualizace `PageIndex` 1 a vytvoří vazbu na druhé stránce uživatelské účty do mřížky (viz obrázek 9).


[![Prvních 10 uživatelské účty se zobrazí.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**Obrázek 8**: jsou zobrazeny prvních 10 uživatelských účtů ([kliknutím ji zobrazíte obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))


[![Kliknutím na následující odkaz zobrazí na druhé stránce uživatelské účty](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**Obrázek 9**: Kliknutím na odkaz na další zobrazí druhou stránku uživatelských účtů ([kliknutím ji zobrazíte obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))


## <a name="summary"></a>Souhrn

Správci často potřebují vyberte uživatele ze seznamu účtů. V předchozích kurzech zvažovali jsme i pomocí rozevíracího seznamu vyplní s uživateli, ale nejsou adekvátní i tento přístup. V tomto kurzu Prozkoumali jsme lepší alternativou: filtrovatelné rozhraní, jejichž výsledky jsou zobrazeny v stránkovaného ovládacího prvku GridView. Pomocí tohoto uživatelského rozhraní správci můžete rychle a efektivně najděte a vyberte jeden uživatelský účet mezi tisíců.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Vlastní stránkování v ASP.NET s využitím SQL serveru 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Účinné stránkování velkých objemů dat](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [Vlastní nástroj pro správu webu se zajištěním provozu](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, Autor více ASP/ASP.NET knih a zakladatelem 4GuysFromRolla.com, má pracovali Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy  *[Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott může být dostupný na adrese [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Alicja Maziarz. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na

> [!div class="step-by-step"]
> [Předchozí](unlocking-and-approving-user-accounts-cs.md)
> [další](recovering-and-changing-passwords-vb.md)
