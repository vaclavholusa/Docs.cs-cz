---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: Vytváření rozhraní vybrat jeden uživatelský účet z mnoha (VB) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu vytvoříme uživatelské rozhraní s stránkové, filtrování mřížky. Konkrétně naše uživatelské rozhraní se skládá z řady LinkButtons pro...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: 56f4d72993bfcb9629d6b4cd08efe0da6dea2486
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>Vytváření rozhraní vybrat jeden uživatelský účet z mnoha (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> V tomto kurzu vytvoříme uživatelské rozhraní s stránkové, filtrování mřížky. Konkrétně naše uživatelské rozhraní bude obsahovat řadu LinkButtons pro filtrování výsledků podle počáteční písmeno uživatelské jméno a ovládací prvek GridView zobrazíte odpovídající uživatelé. Začneme vypsáním všech uživatelských účtů v GridView. Potom v kroku 3, přidáme filtr LinkButtons. Krok 4 zjistí stránkování filtrované výsledky. Rozhraní vytvořená v kroky 2 až 4 se použije v dalších kurzech k provádění úloh správy pro konkrétní uživatelský účet.


## <a name="introduction"></a>Úvod

V <a id="_msoanchor_1"> </a> [ *přiřazení rolí uživatelů* ](../roles/assigning-roles-to-users-vb.md) kurzu jsme vytvořili základní rozhraní pro správce vyberte uživatele a spravovat svůj role. Konkrétně rozhraní zobrazí správce rozevírací seznam všech uživatelů. Taková rozhraní je vhodná, pokud existují, ale s deseti uživatelské účty, ale nepraktické pro weby se stovkami nebo tisíci účtů. Stránkové, filtrování mřížky je vhodnější uživatelské rozhraní pro weby s velké uživatelské základny.

V tomto kurzu vytvoříme uživatelské rozhraní. Konkrétně naše uživatelské rozhraní bude obsahovat řadu LinkButtons pro filtrování výsledků podle počáteční písmeno uživatelské jméno a ovládací prvek GridView zobrazíte odpovídající uživatelé. Začneme vypsáním všech uživatelských účtů v GridView. Potom v kroku 3, přidáme filtr LinkButtons. Krok 4 zjistí stránkování filtrované výsledky. Rozhraní vytvořená v kroky 2 až 4 se použije v dalších kurzech k provádění úloh správy pro konkrétní uživatelský účet.

Můžeme začít!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1: Přidání nových stránek ASP.NET

V tomto kurzu a další dvě jsme se prověřují různých funkcí spojených se správy a možnosti. Potřebujeme řadu stránek ASP.NET implementovat témata zkontrolován v rámci těchto kurzů. Umožňuje vytvořit tyto stránek a aktualizovat mapy webu.

Začněte vytvořením novou složku v projektu s názvem `Administration`. Dál přidejte dva nové stránky ASP.NET do složky, propojení každou stránku s `Site.master` stránky předlohy. Název stránky:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Také přidat do kořenového adresáře webu dvěma stránkami: `ChangePassword.aspx` a `RecoverPassword.aspx`.

Tyto čtyři stránky měli v tomto okamžiku dva ovládací prvky obsahu, jeden pro každou stránku předlohy ContentPlaceHolders: `MainContent` a `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

Chceme zobrazit kód výchozí stránky předlohy pro `LoginContent` ContentPlaceHolder pro tyto stránek. Proto odebrat deklarativní `Content2` obsahu ovládacího prvku. Až to uděláte, na stránkách značek by měl obsahovat pouze jeden prvek obsahu.

Stránky ASP.NET v `Administration` složky jsou určené výhradně pro administrativní uživatele. Jsme přidali roli správce systému v <a id="_msoanchor_2"> </a> [ *vytváření a Správa rolí* ](../roles/creating-and-managing-roles-vb.md) kurzu; omezit přístup k tyto dvě stránky do této role. K tomu, přidejte `Web.config` do souboru `Administration` složky a nakonfigurujte jeho `<authorization>` element přijmout uživatele v roli správce a to tak, aby odepřel všechny ostatní.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

Vašeho projektu Průzkumníku řešení v tomto okamžiku by mělo vypadat jako na uvedené na obrázku 1 snímku obrazovky.


[![Čtyři nové stránky a v souboru Web.config byly přidány k webu](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**Obrázek 1**: čtyři nové stránky a `Web.config` souboru byly přidány na web ([Kliknutím zobrazit obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))


Nakonec aktualizujte mapy webu (`Web.sitemap`) zahrnout položku s `ManageUsers.aspx` stránky. Přidejte následující kód XML po `<siteMapNode>` jsme přidali pro kurzy role.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

S mapy webu aktualizovat přejděte na web prostřednictvím prohlížeče. Jak je vidět na obrázku 2, navigaci vlevo teď obsahuje položky pro správu kurzy.


[![Mapy webu obsahuje uzel s názvem Správa uživatele](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**Obrázek 2**: mapy webu obsahuje uzel s názvem Správa uživatele ([Kliknutím zobrazit obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Krok 2: Výpis všechny uživatelské účty v GridView

Naším cílem end pro účely tohoto kurzu je vytvoření stránkové, filtrování mřížky, pomocí kterého můžete správce vyberte uživatelský účet pro správu. Začněme tím výpis *všechny* uživatele v GridView. Po dokončení přidáme filtrování a stránkování rozhraní a funkce.

Otevřete `ManageUsers.aspx` stránky v `Administration` složky a přidat GridView, nastavení jeho `ID` k `UserAccounts` za chvíli jsme psát kód pro vazbu sadu uživatelské účty pomocí GridView `Membership` třídy `GetAllUsers` metoda. Jak je popsáno v dřívější kurzy `GetAllUsers` metoda vrátí `MembershipUserCollection` objekt, který je kolekce z `MembershipUser` objekty. Každý `MembershipUser` v kolekci obsahuje vlastnosti, například `UserName`, `Email`, `IsApproved`a tak dále.

Chcete-li zobrazit informace o účtu požadovaného uživatele v GridView, nastavte prvku GridView `AutoGenerateColumns` vlastnost na hodnotu False a přidejte BoundFields pro `UserName`, `Email`, a `Comment` vlastnosti a CheckBoxFields pro `IsApproved`, `IsLockedOut`, a `IsOnline` vlastnosti. Tuto konfiguraci můžete použít, prostřednictvím deklarativní ovládací prvek nebo dialogové okno pole. Obrázek 3 ukazuje snímek obrazovky polí, dialogové okno po políčka pole automaticky generovat bylo zrušeno a BoundFields a CheckBoxFields byly přidány a nakonfigurovaná.


[![Přidejte tři BoundFields a tři CheckBoxFields do GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**Obrázek 3**: Přidejte tři BoundFields a tři CheckBoxFields k GridView ([Kliknutím zobrazit obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))


Po dokončení konfigurace vaší GridView, ujistěte se, že jeho deklarativní vypadá přibližně takto:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

Dále je potřeba psát kód, který se váže k GridView uživatelské účty. Vytvořit metodu s názvem `BindUserAccounts` k provedení této úlohy a pak zavolají z `Page_Load` obslužné rutiny události při první návštěvě stránky.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

Za chvíli testovací stránka prostřednictvím prohlížeče. Jak ukazuje obrázek 4, `UserAccounts` GridView zobrazí uživatelské jméno, e-mailovou adresu a další informace o příslušném účtu pro všechny uživatele v systému.


[![Uživatelské účty, jsou uvedeny v prvku GridView.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**Obrázek 4**: uživatelské účty, jsou uvedeny v GridView ([Kliknutím zobrazit obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Krok 3: Filtrování výsledků podle první písmeno uživatelského jména

Aktuálně `UserAccounts` zobrazuje rutina GridView *všechny* uživatelské účty. Pro weby se stovkami nebo tisíci uživatelských účtů je nutné si že tento uživatel moct rychle porovnat dolů zobrazené účty. To můžete provést přidáním filtrování LinkButtons na stránku. Umožňuje přidat na stránku 27 LinkButtons: jednu s názvem všechny společně s jeden LinkButton pro každý písmeno abecedy. Pokud na stránce klepnete všechny LinkButton, GridView zobrazí všechny uživatele. Pokud klepnutím na konkrétní písmeno, zobrazí se pouze na uživatele, jejichž uživatelské jméno při spuštění vybraného písmeno.

Naše první krokem je přidání 27 ovládacích prvků LinkButton. Jednou z možností je vytvořit 27 LinkButtons deklarativně, po jednom. Flexibilnější možných přístupů je použít ovládacího prvku opakovače s `ItemTemplate` , vykreslí LinkButton a pak sváže opakovače jako možnosti filtrování `String` pole.

Spuštění přidáním prvku Repeater na stránku výše `UserAccounts` GridView. Nastavit opakovače `ID` vlastnost `FilteringUI` konfigurace šablon opakovače tak, aby jeho `ItemTemplate` vykreslí LinkButton jejichž `Text` a `CommandName` vlastnosti je vázána na aktuální pole elementu. Jak jsme viděli v <a id="_msoanchor_3"> </a> [ *přiřazení rolí uživatelů* ](../roles/assigning-roles-to-users-vb.md) kurz, můžete to provést pomocí `Container.DataItem` Syntaxe datové vazby. Použít opakovače `SeparatorTemplate` zobrazíte svislice mezi každého odkazu.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

K naplnění tento opakovače s požadovanými možnostmi filtrování, vytvořte metodu s názvem `BindFilteringUI`. Nezapomeňte volat tuto metodu z `Page_Load` obslužné rutiny události při prvním načtení stránky.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

Tato metoda určuje možnosti filtrování jako elementy v `String` pole `filterOptions` pro každý prvek v poli opakovače vykreslí LinkButton s jeho `Text` a `CommandName` vlastnosti, které jsou přidruženy k hodnotě pole element.

Obrázek 5 ukazuje `ManageUsers.aspx` v případě, že zobrazit pomocí prohlížeče.


[![Opakovače uvádí 27 filtrování LinkButtons](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**Obrázek 5**: opakovače uvádí 27 filtrování LinkButtons ([Kliknutím zobrazit obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))


> [!NOTE]
> Uživatelská jména, může začínat libovolný znak, včetně čísel a interpunkčních znamének. Chcete-li zobrazit tyto účty, správce bude muset používat všechny LinkButton možnost. Alternativně můžete přidat LinkButton vrátit všechny uživatelské účty, které začínají číslem. Nechat to jako cvičení pro čtečku.


Kliknutím na některé z filtrování LinkButtons způsobí, že zpětné volání a vyvolá opakovače `ItemCommand` událostí, ale neexistuje žádná změna v mřížce, protože jsme ještě k psaní jakéhokoli kódu filtrování výsledků. `Membership` Třída zahrnuje [ `FindUsersByName` metoda](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) , který vrací tyto uživatelské účty, jejichž uživatelské jméno odpovídá vzoru zadaný hledání. Tato metoda jsme můžete použít k načtení pouze tyto uživatelské účty, jejichž uživatelská jména začínat písmenem určeného `CommandName` z filtrované LinkButton, kterou jste klepli.

Začněte tím, že aktualizace `ManageUser.aspx` kódu stránky třídy tak, že obsahují vlastnost s názvem `UsernameToMatch` tuto vlastnost potrvají řetězec filtru uživatelské jméno napříč postback:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

`UsernameToMatch` Jeho hodnota je přiřazen do vlastnosti se ukládají `ViewState` kolekce pomocí klíče UsernameToMatch. Pokud hodnota této vlastnosti je pro čtení, zkontroluje, zda existuje hodnota v `ViewState` kolekce; v opačném případě se vrátí výchozí hodnotu, prázdný řetězec. `UsernameToMatch` Vlastnost jádro vykazuje běžný vzor, a to uložením hodnotu, pokud chcete zobrazit stav tak, aby všechny změny vlastnosti jsou nastavené jako trvalé mezi postback. Další informace o tomto vzoru, najdete v tématu [stav zobrazení ASP.NET Principy](https://msdn.microsoftn-us/library/ms972976.aspx).

Potom aktualizujte `BindUserAccounts` tak místo volání metody `Membership.GetAllUsers`, zavolá `Membership.FindUsersByName`a předejte hodnotu `UsernameToMatch` vlastnost připojená se zástupným znakem SQL %.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

Zobrazit pouze uživatele, jejichž uživatelské jméno začíná písmenem A, nastavte `UsernameToMatch` vlastnosti A a pak zavolají `BindUserAccounts` výsledkem by volání `Membership.FindUsersByName("A%")`, která vrátí všechny uživatele jejichž uživatelské jméno začíná A. Podobně vrátit *všechny* přiřadit prázdný řetězec pro uživatele, `UsernameToMatch` vlastnost tak, aby `BindUserAccounts` vyvolá metoda `Membership.FindUsersByName("%")`, a tím vrátí všechny uživatelské účty.

Vytvoření obslužné rutiny události pro opakovače `ItemCommand` událostí. Tato událost se vyvolá při každém kliknutí filtru LinkButtons na; je předán kliknutelnou LinkButton `CommandName` hodnotu prostřednictvím `RepeaterCommandEventArgs` objektu. Je potřeba přiřadit příslušná hodnota, která má `UsernameToMatch` vlastnost a potom volání `BindUserAccounts` metoda. Pokud `CommandName` je vše, přiřaďte prázdný řetězec k `UsernameToMatch` tak, aby se zobrazují všechny uživatelské účty. Jinak přiřadit `CommandName` hodnotu `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

S tímto kódem na místě otestujte funkci filtrování. Když je nejdřív navštívené stránky, se zobrazí všechny uživatelské účty (odkazuje zpět na obrázku 5). Kliknutím na tlačítko A LinkButton způsobí zpětné volání a vyfiltruje výsledky, pouze uživatelské účty, které začínají A zobrazení.


[![Pomocí filtrování LinkButtons můžete zobrazit uživatele, jejichž uživatelské jméno začíná písmenem určité](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**Obrázek 6**: pomocí LinkButtons filtrování můžete zobrazit tyto uživatele jejichž uživatelské jméno začíná písmenem některých ([Kliknutím zobrazit obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Krok 4: Aktualizace GridView používat stránkování

Rutina GridView zobrazen v obrázcích 5 a 6 obsahuje seznam všech záznamů vrácených `FindUsersByName` metoda. Pokud jsou stovkami nebo tisíci uživatelské účty to může vést k přetížení informace při zobrazení všech účtů (jako je případ, když kliknete na všechny LinkButton nebo když původně na stránce). Chcete-li k dispozici uživatelské účty ve více spravovatelných blocích, nakonfigurujeme GridView zobrazíte 10 uživatelských účtů najednou.

Ovládací prvek GridView nabízí dva typy stránkování:

- **Výchozí stránkování** – snadno implementovat, ale neefektivní. Stručně řečeno, výchozí stránkování GridView očekává *všechny* záznamů z datového zdroje. Potom zobrazí jenom na příslušnou stránku záznamů.
- **Vlastní stránkování** -vyžaduje více práce implementovat, ale je efektivnější než výchozí stránkování, protože s vlastní stránkování data zdroj vrátí jenom přesné sadu záznamů pro zobrazení.

Při procházení tisíce záznamů, může být poměrně dlouhou výkonu rozdíl mezi výchozími a vlastní stránkování. Vzhledem k tomu, že jsme sestavení může toto rozhraní předpokladu, že se stovkami nebo tisíci uživatelské účty, můžeme použít vlastní stránkování.

> [!NOTE]
> Podrobnější informace o rozdílech mezi výchozí a vlastní stránkování, jakož i na výzvy účastnící se implementuje vlastní stránkování, najdete v části [efektivně stránkování prostřednictvím velké objemy dat](https://asp.net/learn/data-access/tutorial-25-vb.aspx). Některé analýzy výkonu rozdíl mezi výchozími a vlastní stránkování, najdete v části [vlastní stránkování v technologii ASP.NET s SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Chcete-li implementovat vlastní stránkování potřebujeme některé mechanismus, pomocí kterých se budou načítat přesné podmnožinu záznamů v zobrazení GridView. Dobrá zpráva je, že `Membership` třídy `FindUsersByName` metoda má přetížení, které umožňuje zadejte index stránky a velikost stránky a vrátí pouze uživatelské účty, které spadají do tohoto rozsahu záznamů.

Konkrétně toto přetížení má následující podpis: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

*Index stránky* parametr určuje stránce uživatelských účtů vrátit; *pageSize* Určuje, kolik zaznamenává zobrazených na stránce. *TotalRecords* parametr `ByRef` parametr, který vrátí počet celkový počet uživatelských účtů v úložišti uživatele.

> [!NOTE]
> Data vrácená `FindUsersByName` je seřazen podle uživatelského jména; kritéria řazení nelze přizpůsobit.


GridView může být nakonfigurován pro využívat vlastní stránkování, ale když vázaný jenom na ovládacího prvku ObjectDataSource. Ovládacího prvku ObjectDataSource implementovat vlastní stránkování, vyžaduje dvě metody: jeden, který je předán počáteční index řádku a maximální počet záznamů, které chcete zobrazit, a vrátí přesné podmnožinu záznamů, které spadají do této značky span; a metoda, která vrátí celkový počet záznamů stránkování prostřednictvím. `FindUsersByName` Přetížení přijímá index stránky a velikost stránky a vrátí celkový počet záznamů prostřednictvím `ByRef` parametr. Proto neshodě rozhraní rozhraní sem.

Jednou z možností je vytvořit třídu proxy, která vystavuje rozhraní ObjectDataSource očekává a interně volá `FindUsersByName` metoda. Další možnost – a jeden že použijeme k tomuto článku – je vytvořte vlastní rozhraní stránkování a použijte místo prvku GridView předdefinované stránkování rozhraní.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Vytvoření první, předchozí, v dalším kroku poslední rozhraní stránkování

První, předchozí, další a poslední LinkButtons Vytvořme rozhraní stránkování. První LinkButton, po kliknutí na přenese uživatele na první stránku dat, zatímco předchozí mu vrátit na předchozí stránku. Podobně další a poslední přesune uživatele na další a poslední stránku, v uvedeném pořadí. Přidání ovládacích prvků čtyři LinkButton pod `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

Dále vytvořte obslužnou rutinu události pro každou z LinkButton `Click` události.

Obrázek 7 znázorňuje čtyři LinkButtons při zobrazení v zobrazení Visual Web Developer návrhu.


[![Přidat příště první, předchozí, a poslední LinkButtons pod GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**Obrázek 7**: přidat první, předchozí, další a poslední LinkButtons pod the GridView ([Kliknutím zobrazit obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Udržování přehledu o Index aktuální stránky

Když první uživatel navštíví `ManageUsers.aspx` stránky nebo klikne na jednu z filtrování tlačítka, chceme zobrazit na první stránku dat v GridView. Když uživatel klikne jeden navigace LinkButtons, ale je potřeba aktualizovat index stránky. Pokud chcete zachovat index stránky a počet záznamů zobrazených na stránce, přidejte následující dvě vlastnosti do třídy kódu stránky:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

Jako `UsernameToMatch` vlastnost, `PageIndex` vlastnost potrvají jeho hodnota stavu zobrazení. Jen pro čtení `PageSize` vlastnost vrací hodnotu pevně, 10. Mohu pozvat zúčastněným čtečky k aktualizaci této vlastnosti pomocí stejného vzoru jako `PageIndex`a potom k posílení `ManageUsers.aspx` stránky tak, že uživatel na stránce můžete určit, kolik uživatelské účty zobrazených na stránce.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Načtení právě k aktuální stránce záznamů, aktualizace Index stránky a povolení a zakázání LinkButtons rozhraní stránkování

Rozhraní stránkování v místě a `PageIndex` a `PageSize` vlastnosti přidat, jsme připraveni na aktualizaci `BindUserAccounts` metoda tak, aby používala odpovídající `FindUsersByName` přetížení. Kromě toho je potřeba mít tato metoda povolí nebo zakáže rozhraní stránkování v závislosti na tom, jaké stránka se zobrazuje. Při zobrazení na první stránku dat, by mělo být zakázáno odkazy první a předchozí; V dalším a poslední je třeba zakázat při zobrazení poslední stránky.

Aktualizace `BindUserAccounts` metoda následujícím kódem:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

Všimněte si, že celkový počet záznamů stránkování prostřednictvím je dáno poslední parametr `FindUsersByName` metoda. Poté, co vrátí zadanou stránku uživatelských účtů, čtyři LinkButtons jsou zapnutá nebo vypnutá, v závislosti na tom, jestli na první nebo poslední stránku dat zobrazení.

Posledním krokem je napsat kód pro čtyři LinkButtons `Click` obslužné rutiny událostí. V těchto obslužných rutinách událostí muset aktualizovat `PageIndex` vlastnost konkrétním data, která mají GridView prostřednictvím volání `BindUserAccounts` obslužné rutiny událostí první, předchozí a další jsou velmi jednoduché. `Click` Obslužné rutiny události pro poslední LinkButton, ale je trochu složitější vzhledem k tomu, že je potřeba určit, zobrazují se počet záznamů, aby bylo možné zjistit poslední index stránky.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

Následující obrázky 8 a 9 zobrazit vlastní rozhraní stránkování v akci. Obrázek 8 ukazuje `ManageUsers.aspx` stránka při prohlížení na první stránku dat pro všechny uživatelské účty. Všimněte si, že se zobrazují jenom 10 13 účtů. Kliknutím na odkaz Další nebo poslední způsobí, že zpětné volání, aktualizace `PageIndex` na 1 a vazby druhé stránce uživatelské účty do mřížky (viz obrázek 9).


[![První uživatelské účty 10 se zobrazí.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**Obrázek 8**: první 10 uživatelské účty se zobrazují ([Kliknutím zobrazit obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))


[![Kliknutím na odkaz na další zobrazí stránku druhý uživatelských účtů](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**Obrázek 9**: Kliknutím na odkaz na další zobrazí druhé stránce uživatelské účty ([Kliknutím zobrazit obrázek v plné velikosti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))


## <a name="summary"></a>Souhrn

Správci často potřebují vyberte uživatele ze seznamu účtů. V předchozích kurzech jsme se podívali na pomocí rozevíracího seznamu naplněný uživatele, ale tento přístup není správné škálování. V tomto kurzu jsme prozkoumali lepší alternativou: Filtrování rozhraní, jejichž výsledky se zobrazí v stránkové GridView. S tímto rozhraním uživatele Správci může rychle a efektivně vyhledejte a vyberte jeden uživatelský účet mezi tisíců.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Vlastní stránkování v technologii ASP.NET se systémem SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Efektivní stránkování prostřednictvím velkých objemů dat.](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [Vrácení vlastní nástroj pro správu webu](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Alicja Maziarz. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v

> [!div class="step-by-step"]
> [Předchozí](unlocking-and-approving-user-accounts-cs.md)
> [další](recovering-and-changing-passwords-vb.md)
