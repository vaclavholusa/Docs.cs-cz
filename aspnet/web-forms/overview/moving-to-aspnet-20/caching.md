---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Ukládání do mezipaměti | Dokumentace Microsoftu
author: microsoft
description: Znalost ukládání do mezipaměti je důležité pro správné aplikace ASP.NET. ASP.NET 1.x nabízí tři různé možnosti pro ukládání do mezipaměti; ukládání výstupu do mezipaměti...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 5c97464ee50291338a80120a86b1b86b07bc672d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755971"
---
<a name="caching"></a>Ukládání do mezipaměti
====================
podle [Microsoft](https://github.com/microsoft)

> Znalost ukládání do mezipaměti je důležité pro správné aplikace ASP.NET. ASP.NET 1.x nabízí tři různé možnosti pro ukládání do mezipaměti; ukládání výstupu do mezipaměti, ukládání do mezipaměti fragment a rozhraní API mezipaměti.


Znalost ukládání do mezipaměti je důležité pro správné aplikace ASP.NET. ASP.NET 1.x nabízí tři různé možnosti pro ukládání do mezipaměti; ukládání výstupu do mezipaměti, ukládání do mezipaměti fragment a rozhraní API mezipaměti. Technologie ASP.NET 2.0 nabízí všechny tři z těchto metod, ale přidá některé další důležité funkce. Existuje několik nové závislosti mezipaměti a vývojáři teď mají možnost vytvořit i vlastní mezipaměti závislosti. Konfigurace ukládání do mezipaměti taky vylepšili jsme výrazně v technologii ASP.NET 2.0.

## <a name="new-features"></a>Nové funkce

## <a name="cache-profiles"></a>Profily mezipaměti

Mezipaměť profily umožňují vývojářům definovat nastavení určité mezipaměti, které lze poté použít u jednotlivých stránkách. Například pokud máte některé stránky, které má vypršet z mezipaměti po 12 hodinách, můžete snadno vytvořit profil mezipaměti, který lze použít na tyto stránky. Chcete-li přidat nový profil mezipaměti, použijte &lt;outputCacheSettings&gt; oddílu v konfiguračním souboru. Například níže je konfigurační profil mezipaměti volá *twoday* , který konfiguruje doba mezipaměti 12 hodin.

[!code-xml[Main](caching/samples/sample1.xml)]

Chcete-li použít tento profil mezipaměti na konkrétní stránce, použijte atribut CacheProfile – Direktiva @ OutputCache, jak je znázorněno níže:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Vlastní mezipaměti závislosti

Vývoj v ASP.NET 1.x báječné navýšení kapacity pro vlastní mezipaměti závislosti. V technologii ASP.NET 1.x, třída CacheDependency byla zapečetěná mu vývojáři ze své vlastní třídy odvozené z něj. V technologii ASP.NET 2.0 se odebere těmto omezením a vývojáři jsou zdarma vyvíjet své vlastní závislosti vlastní mezipaměti. Třída CacheDependency umožňuje vytvoření vlastní mezipaměti závislost na základě soubory, adresáře nebo klíče k mezipaměti.

Například následující kód vytvoří novou závislost vlastní mezipaměti založené na souboru s názvem stuff.xml umístěné v kořenovém adresáři webové aplikace:

[!code-csharp[Main](caching/samples/sample3.cs)]

V tomto scénáři při změně souboru stuff.xml, položku z mezipaměti zneplatněna.

Je také možné vytvořit vlastní mezipaměti závislosti pomocí klíče k mezipaměti. Pomocí této metody, odstranění klíče mezipaměti zruší platnost dat uložených v mezipaměti. Následující příklad ukazuje toto:

[!code-csharp[Main](caching/samples/sample4.cs)]

Zrušte platnost položky, který byl vložen nad, jednoduše odeberte položky, který byl vložen do mezipaměti tak, aby fungoval jako klíče mezipaměti.

[!code-csharp[Main](caching/samples/sample5.cs)]

Všimněte si, že klíč položky, která funguje jako klíč mezipaměti musí být stejná jako hodnota přidat do pole klíče k mezipaměti.

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>Dotazování na základě závislostí mezipaměti SQL<em>(také nazývané cyklické závislosti)</em>

SQL Server 7 a 2000 použít model založený na dotazování pro závislosti mezipaměti SQL. Model založený na dotazování používá aktivační události v tabulce databáze, která se aktivuje při změně dat v tabulce. Aktivovat, aktualizace **changeId** v tabulce oznámení, která pravidelně kontroluje technologie ASP.NET. Pokud **changeId** pole se aktualizovala, ASP.NET ví, že jste změnili data a ji zruší platnost dat uložených v mezipaměti.

> [!NOTE]
> SQL Server 2005 můžete také použít model založený na dotazování, ale protože model založený na dotazování není nejúčinnější model, je vhodné použít model založených na dotazech (prodiskutována později) s SQL Server 2005.


V pořadí závislosti mezipaměti SQL pomocí modelu založeného na dotazování fungovala správně musí mít v tabulkách oznámení jsou povolená. To lze provést prostřednictvím kódu programu pomocí třídy SqlCacheDependencyAdmin nebo s použitím aspnet\_regsql.exe nástroj.

Následující příkaz zaregistruje tabulky produktů v databázi Northwind umístěné na instanci systému SQL Server s názvem *dbase* závislosti mezipaměti SQL.

[!code-console[Main](caching/samples/sample6.cmd)]

Následuje vysvětlení přepínače příkazového řádku používá ve výše uvedeném příkazu:

| **Přepínač příkazového řádku** | **Účel** |
| --- | --- |
| -S *serveru* | Určuje název serveru. |
| -ed | Určuje, že by měl databáze povolena pro funkci závislosti mezipaměti SQL. |
| -d *databáze\_název* | Určuje název databáze, která by měla být zapnutá závislosti mezipaměti SQL. |
| -E | Určuje, že aspnet\_regsql by měl používat ověřování Windows, při připojení k databázi. |
| -et | Určuje, že jsme uvolnili databázové tabulky pro závislost mezipaměti SQL. |
| -t *tabulky\_název* | Určuje název tabulky databáze, pokud chcete povolit pro závislost mezipaměti SQL. |

> [!NOTE]
> Nejsou k dispozici pro aspnet jinými přepínači\_regsql.exe. Úplný seznam, spusťte aspnet\_regsql.exe-? z příkazového řádku.


Při spuštění tohoto příkazu jsou provedeny následující změny k databázi SQL serveru:

- **AspNet\_SqlCacheTablesForChangeNotification** tabulka se přidá. Tato tabulka obsahuje jeden řádek pro každou tabulku v databázi, pro které bylo povoleno závislosti mezipaměti SQL.
- Následující uložené procedury jsou vytvořeny v databázi:


| AspNet\_SqlCachePollingStoredProcedure | Dotazy AspNet\_SqlCacheTablesForChangeNotification tabulky a vrátí všechny tabulky, které jsou povoleny pro závislost mezipaměti SQL a hodnota changeId pro každou tabulku. Tato uložená procedura se používá pro dotazování k určení, zda se data nezměnila. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Vrátí všechny tabulky povolena funkce závislosti mezipaměti SQL pomocí dotazu AspNet\_závislost mezipaměti SqlCacheTablesForChangeNotification tabulky a vrátí všechny tabulky povolené pro SQL. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Registruje tabulku závislosti mezipaměti SQL tak, že přidáte nezbytné položky v tabulce oznámení a přidá aktivační událost. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Zruší registraci tabulku závislosti mezipaměti SQL tak, že odeberete položce v tabulce oznámení a odebírá aktivační událost. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Aktualizuje tabulku oznámení zvýšením changeId změněné tabulce. ASP.NET používá tuto hodnotu k určení, zda se data nezměnila. Jak je uvedeno níže, provádí se tato uložená procedura má aktivační procedura vytvoří, když je povolená v tabulce. |


- Volá se, aktivační události SQL Server ***tabulky\_název *\_AspNet\_SqlCacheNotification\_aktivační událost** je vytvořený pro tabulku. Tato aktivační událost spouští AspNet\_SqlCacheUpdateChangeIdStoredProcedure při vložení, aktualizace nebo odstranění v tabulce.
- Role systému SQL Server volá **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** je přidána do databáze.

**Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** role systému SQL Server má oprávnění EXEC AspNet\_SqlCachePollingStoredProcedure. Aby model dotazování fungovala správně, musíte přidat váš účet procesu aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess role. Aspnet\_regsql.exe nástroj to nebude udělal za vás.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Konfigurace závislosti mezipaměti SQL na základě cyklického dotazování

Existuje několik kroků, které jsou požadovány pro konfiguraci založené na dotazování závislosti mezipaměti SQL. Prvním krokem je povolit databáze a tabulky, jak je popsáno výše. Po dokončení tohoto kroku zbývající část konfigurace vypadá takto:

- Konfiguruje se konfigurační soubor technologie ASP.NET.
- Konfigurace SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Konfigurace souboru konfigurace ASP.NET

Kromě přidání připojovací řetězec, jak je popsáno v předchozí modul, musíte také nakonfigurovat &lt;mezipaměti&gt; element s &lt;sqlCacheDependency&gt; elementu, jak je znázorněno níže:

[!code-xml[Main](caching/samples/sample7.xml)]

Tato konfigurace umožňuje na závislost mezipaměti SQL *pubs* databáze. Všimněte si, že atribut pollTime nastaven &lt;sqlCacheDependency&gt; prvek výchozí 60000 milisekund nebo 1 minuta. (Tato hodnota nemůže být menší než 500 milisekund). V tomto příkladu &lt;přidat&gt; element přidá novou databázi a přepíše pollTime nastaven, nastavení na 9000000 milisekund.

#### <a name="configuring-the-sqlcachedependency"></a>Konfigurace SqlCacheDependency

Dalším krokem je konfigurace SqlCacheDependency. Nejjednodušší způsob, jak provést tuto akci je zadat hodnotu pro atribut SqlDependency v direktivě @ Outcache následujícím způsobem:

[!code-aspx[Main](caching/samples/sample8.aspx)]

V direktivě @ OutputCache výše uvedené závislosti mezipaměti SQL je nakonfigurován pro *autoři* v tabulku *pubs* databáze. Více závislostí lze nastavit jejich oddělením středníkem takto:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Jinou metodou SqlCacheDependency konfigurace je k tomu prostřednictvím kódu programu. Následující kód vytvoří novou závislost mezipaměti SQL na *autoři* v tabulku *pubs* databáze.

[!code-csharp[Main](caching/samples/sample10.cs)]

Jednou z výhod programově definování závislosti mezipaměti SQL je, že můžete zpracovávat všechny výjimky, které mohou nastat. Pokud při pokusu o definovat závislosti mezipaměti SQL pro databázi, která nebyla povolena pro oznámení, například **databasenotenabledfornotificationexception –** , bude vyvolána výjimka. V takovém případě můžete zkusit povolit databáze pro oznámení voláním **SqlCacheDependencyAdmin.EnableNotifications** metoda a předají se jí název databáze.

Podobně, pokud se pokusíte k definování závislosti mezipaměti SQL pro tabulku, která nebyla povolena pro oznámení, **tablenotenabledfornotificationexception –** bude vyvolána výjimka. Potom můžete zavolat **SqlCacheDependencyAdmin.EnableTableForNotifications** metodu předáním název databáze a název tabulky.

Následující příklad kódu ukazuje, jak správně nakonfigurovat zpracování výjimek při konfiguraci závislosti mezipaměti SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Další informace: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Závislosti mezipaměti SQL na základě dotazu (jenom SQL Server 2005)

Pokud používáte SQL Server 2005 závislosti mezipaměti SQL, není nutné na model založený na dotazování. Při použití s SQL Server 2005, závislosti mezipaměti SQL komunikovat přímo prostřednictvím SQL připojení k instanci systému SQL Server (není potřeba žádná další konfigurace) pomocí oznámení dotazů SQL Server 2005.

Nejjednodušší způsob, jak povolit oznámení založené na dotazu je provést to deklarativně nastavením **SqlCacheDependency** atribut objektu zdroje dat k **CommandNotification** a nastavení **EnableCaching** atribut **true**. Pomocí této metody není vyžadován žádný kód. Pokud výsledek příkazu Spustit s daty změny zdroje, skončí platnost všech data v mezipaměti.

Následující příklad nastaví ovládací prvek zdroje dat pro závislost mezipaměti SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

V takovém případě případného v dotazu **SelectCommand** vrátí jiné výsledky než to udělali původně, výsledky, které jsou uložené v mezipaměti jsou neplatné.

Můžete také určit, že všechny vaše zdroje dat pro závislosti mezipaměti SQL povolit tak, že nastavíte **SqlDependency** atribut **@ OutputCache** direktivu **CommandNotification** . Následující příklad ukazuje to.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Další informace o oznámení dotazů v SQL Server 2005 najdete v článku webu knihy Online pro SQL Server.


Jinou metodou konfigurace založená na dotazech závislosti mezipaměti SQL je k tomu prostřednictvím kódu programu pomocí třídy SqlCacheDependency. Následující příklad kódu ukazuje, jak to lze provést.

[!code-csharp[Main](caching/samples/sample14.cs)]

Další informace: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Substituce mezipaměti

Ukládání do mezipaměti na stránce může výrazně zvýšit výkon webové aplikace. V některých případech však většina stránky ukládat do mezipaměti a některé fragmenty v rámci stránky a být dynamické. Například pokud vytvoříte stránku příběhy, která je zcela statická pro určitou dobu, můžete nastavit celou stránku ukládat do mezipaměti. Pokud chcete zahrnout otáčení banner ad, která se změnila na každý požadavek na stránku, část stránky, která obsahuje oznámení o inzerovaném programu musí být dynamická. Aby bylo možné ukládat do mezipaměti na stránce, ale nahraďte část obsahu dynamicky, můžete použít náhradní po mezipaměti technologie ASP.NET. Substituce mezipaměti po celé stránky je do výstupní mezipaměti s konkrétní části označené jako vyloučené z ukládání do mezipaměti. V příkladu Bannery ad AdRotator ovládací prvek umožňuje využít výhod mezipaměti po nahrazení tak, aby reklamy dynamicky vytvoří pro každého uživatele a pro každou aktualizaci stránky.

Existují tři způsoby, jak implementovat substituce mezipaměti po:

- Deklarativní pomocí ovládacího prvku nahrazení.
- Programově pomocí rozhraní API ovládacího prvku nahrazení.
- Implicitně pomocí ovládacího prvku AdRotator.

### <a name="substitution-control"></a>Náhradní ovládací prvek

Ovládací prvek substituční ASP.NET určuje oddíl stránky v mezipaměti, která je dynamicky vytvořené spíše než v mezipaměti. Na stránce do umístění, kam chcete dynamický obsah se zobrazí umístíte náhradního ovládacího prvku. Náhradní ovládací prvek v době běhu, volá metodu, která zadáte vlastnost MethodName. Metoda musí vracet řetězce, který pak nahradí obsah nahrazení ovládacího prvku. Metoda musí být statickou metodou nadřazeného ovládacího prvku Page nebo UserControl. Použití ovládacího prvku nahrazení způsobí, že ukládání do mezipaměti na straně klienta mezipaměti na serveru, změnit tak, aby na stránce nebudou zapisována do mezipaměti na straně klienta. Tím se zajistí, že budoucí požadavky na stránku pro volání metody znovu se vygenerovat dynamický obsah.

### <a name="substitution-api"></a>Náhradní rozhraní API

K vytvoření dynamického obsahu pro stránky v mezipaměti prostřednictvím kódu programu, můžete volat [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) metodě v kódu stránky, předá jako parametr název metody. Metoda, která zpracovává vytvořit dynamický obsah přebírá jediný [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) parametr a vrátí hodnotu typu string. Vrácený řetězec je obsah, který bude nahrazena v daném umístění. Výhodou volání metody WriteSubstitution namísto použití náhradní ovládací pomocí deklarace je, že můžete volat metodu libovolného objektu, namísto volání statické metody na stránku nebo uživatelský ovládací prvek objektu.

Volání metody WriteSubstitution způsobí, že ukládání do mezipaměti na straně klienta mezipaměti na serveru, změnit tak, aby na stránce nebudou zapisována do mezipaměti na straně klienta. Tím se zajistí, že budoucí požadavky na stránku pro volání metody znovu se vygenerovat dynamický obsah.

### <a name="adrotator-control"></a>Funkce AdRotator ovládacího prvku

Funkce AdRotator, který implementuje serverový ovládací prvek mezipaměti po nahrazení podporu interně. Pokud ovládací prvek AdRotator na stránce, zobrazí se pak jedinečný oznámení o inzerovaných programech s každým požadavkem, bez ohledu na to, zda je nadřazená stránka uložené v mezipaměti. V důsledku toho je stránka, která obsahuje ovládací prvek AdRotator pouze v mezipaměti na serveru.

## <a name="controlcachepolicy-class"></a>Třída ControlCachePolicy

Třída ControlCachePolicy umožňují programové řízení fragment ukládání do mezipaměti pomocí uživatelské ovládací prvky. Vloží uživatelské ovládací prvky v rámci technologie ASP.NET [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) instance. Třída BasePartialCachingControl představuje uživatelský ovládací prvek, který má výstup, povoleno ukládání do mezipaměti.

Když zobrazujete [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) vlastnost [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) ovládací prvek, bude vždycky přijímat platný objekt ControlCachePolicy. Ale pokud přistupujete [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) vlastnost [uživatelský ovládací prvek](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) ovládacího prvku, obdržíte platný objekt ControlCachePolicy pouze v případě, že uživatelský ovládací prvek je již uzavřenou BasePartialCachingControl ovládacího prvku. Pokud není zabalena, ControlCachePolicy objekt vrácený vlastností se při pokusu o manipulaci s, protože nemá žádné přidružené BasePartialCachingControl vyvolat výjimky. Pokud chcete zjistit, zda instanci uživatelský ovládací prvek podporuje ukládání do mezipaměti bez generování událostí výjimky, zkontrolujte [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) vlastnost.

Používání třídy ControlCachePolicy je jedním z několika způsoby, jak můžete povolit ukládání výstupu do mezipaměti. Následující seznam popisuje metody, které vám umožní povolit ukládání výstupu do mezipaměti:

- Použití [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) směrnice umožňující ukládání výstupu do mezipaměti v deklarativní scénáře.
- Použití [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) atribut pro povolení ukládání do mezipaměti pro uživatelský ovládací prvek v souboru kódu na pozadí.
- Třída ControlCachePolicy slouží k nastavení mezipaměti prostřednictvím kódu programu scénáře, ve kterých pracujete BasePartialCachingControl instancí, které byly mezipaměti povolena pomocí jedné z výše uvedených metod a dynamicky načtené pomocí možnosti [System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) metody.

ControlCachePolicy instance lze úspěšně ovládat pouze mezi Init a provedení operace PreRender fází životního cyklu ovládacího prvku. Pokud upravíte objekt ControlCachePolicy po fázi PreRender, ASP.NET vyvolá výjimku, protože všechny změny provedené po vykreslení ovládacího prvku nemůže mít vliv na skutečně nastavení mezipaměti (ovládací prvek je v mezipaměti během fáze vykreslení). Nakonec instanci ovládacího prvku uživatel (a proto jeho objekt ControlCachePolicy) je k dispozici pouze pro programovou manipulaci se ve skutečnosti je vykreslen.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Změny v konfiguraci ukládání do mezipaměti – &lt;ukládání do mezipaměti&gt; – Element

Existuje několik změn pro konfiguraci ukládání do mezipaměti v technologii ASP.NET 2.0. &lt;Ukládání do mezipaměti&gt; element je nového v technologii ASP.NET 2.0 a umožní vám provádět změny v konfiguraci ukládání do mezipaměti v konfiguračním souboru. Následující atributy jsou k dispozici.

| **– Element** | **Popis** |
| --- | --- |
| **mezipaměť** | Volitelný element. Definuje globální nastavení mezipaměti aplikace. |
| **outputCache** | Volitelný element. Určuje nastavení výstupní mezipaměti pro celou aplikaci. |
| **outputCacheSettings** | Volitelný element. Určuje nastavení výstupní mezipaměti, které mohou být použity na stránky v aplikaci. |
| **sqlCacheDependency** | Volitelný element. Nakonfiguruje závislosti mezipaměti SQL pro aplikaci ASP.NET. |

### <a name="the-ltcachegt-element"></a>&lt;Mezipaměti&gt; – Element

Následující atributy jsou k dispozici v &lt;mezipaměti&gt; element:

| **Atribut** | **Popis** |
| --- | --- |
| **disableMemoryCollection** | Volitelné **logická** atribut. Získá nebo nastaví hodnotu určující, jestli je zakázaná mezipaměť kolekce, která nastane, pokud se tento počítač je přetížena paměť. |
| **disableExpiration** | Volitelné **logická** atribut. Získá nebo nastaví hodnotu určující, zda je doba vypršení platnosti mezipaměti zakázán. Pokud je zakázán, nevyprší platnost položek v mezipaměti a pozadí úklidu vypršela platnost mezipaměti položek se neděje. |
| **privateBytesLimit** | Volitelné **Int64** atribut. Získá nebo nastaví hodnotu indikující, že maximální velikost nesdílených bajtů aplikace předtím, než se mezipaměť začne vyprazdňování neplatné položky a pokus o uvolnění paměti. Toto omezení zahrnuje jak paměť používanou mezipaměti stejně jako normální paměti režie z běžící aplikaci. Nastavení 0 označuje, že technologie ASP.NET bude používat vlastní heuristické metody pro určení, kdy se má spustit opětovné získání paměti. |
| **percentagePhysicalMemoryUsedLimit** | Volitelné **Int32** atribut. Získá nebo nastaví hodnotu indikující, že maximální procento počítače, do fyzické paměti, které mohou být spotřebovány aplikace předtím, než se mezipaměť začne vyprazdňování neplatné položky a pokus o uvolnění paměti, že toto využití paměti zahrnuje obě paměť používanou mezipaměti i jako využití paměti na normální spuštěné aplikaci. Nastavení 0 označuje, že technologie ASP.NET bude používat vlastní heuristické metody pro určení, kdy se má spustit opětovné získání paměti. |
| **privateBytesPollTime** | Volitelné **TimeSpan** atribut. Získá nebo nastaví hodnotu, která časový interval mezi dotazování pro využití paměti aplikace Nesdílené bajty. |

### <a name="the-ltoutputcachegt-element"></a>&lt;OutputCache&gt; – Element

Jsou k dispozici pro následující atributy &lt;outputCache&gt; elementu.


|       <strong>Atribut</strong>        |                                                                                                                                                                                                                                                       <strong>Popis</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Volitelné <strong>logická</strong> atribut. Povolí nebo zakáže stránku výstupní mezipaměti. Pokud je zakázané, žádné stránky jsou ukládány do mezipaměti bez ohledu na nastavení deklarativní nebo prostřednictvím kódu programu. Výchozí hodnota je <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Volitelné <strong>logická</strong> atribut. Povolí nebo zakáže fragment mezipaměti aplikace. Pokud je zakázané, žádné stránky, jsou uložené v mezipaměti bez ohledu na to [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) direktiva nebo ukládání do mezipaměti profil použít. Zahrnuje typu cache-control záhlaví označující, že nadřazený proxy serverů, jakož i klienty prohlížeče by se neměly pokoušet výstup stránky do mezipaměti. Výchozí hodnota je <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Volitelné <strong>logická</strong> atribut. Získá nebo nastaví hodnotu označující, zda <strong>mezipaměti – ovládací prvek: privátní</strong> hlavičku posílá modul výstupní mezipaměti ve výchozím nastavení. Výchozí hodnota je <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Volitelné <strong>logická</strong> atribut. Povolí nebo zakáže posílání Http "<strong>Vary: \</ strong ><em>" záhlaví v odpovědi. Výchozím nastavením false, "</em>* Vary: \* <strong>" hlavičku posílá pro stránky výstupu do mezipaměti. Při odeslání hlavičce Vary umožňuje různé verze do mezipaměti na základě zadaných v hlavičce Vary. Například <em>Vary: uživatel-agentů</em> uloží různé verze stránky na základě uživatelského agenta odesíláním žádosti. Výchozí hodnota je ** false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>&lt;OutputCacheSettings&gt; – Element

&lt;OutputCacheSettings&gt; element umožňuje pro vytváření mezipaměti profilů, jak je uvedeno výše. Pouze podřízený element pro &lt;outputCacheSettings&gt; prvek je &lt;outputCacheProfiles&gt; – element pro konfiguraci mezipaměti profilů.

### <a name="the-ltsqlcachedependencygt-element"></a>The &lt;sqlCacheDependency&gt; Element

Jsou k dispozici pro následující atributy &lt;sqlCacheDependency&gt; elementu.

| **Atribut** | **Popis** |
| --- | --- |
| **Povoleno** | Vyžaduje **logická** atribut. Označuje, zda jsou změny pro dotazování. |
| **pollTime** | Volitelné **Int32** atribut. Nastaví frekvenci, s kterým SqlCacheDependency dotazuje na změny v tabulce databáze. Tato hodnota odpovídá počet milisekund mezi po sobě následujících dotazech. Nejde ji nastavit na míň než 500 milisekund. Výchozí hodnota je 1 minuta. |

### <a name="more-information"></a>Další informace

Není k dispozici další informace, měli byste vědět, o konfiguraci mezipaměti.

- Pokud není nastavený limit nesdílených bajtů procesu pracovního procesu, mezipaměti bude používat jednu z těchto omezení: 

    - x86 2 GB: 800 MB nebo 60 % fyzické paměti RAM, podle toho, co je menší
    - x86 3 GB: 1 800 MB nebo 60 % fyzické paměti RAM, podle toho, co je menší
    - x 64: 1 terabajt nebo 60 % fyzické paměti RAM podle toho, co je menší
- Pokud obě pracovní proces soukromých bajtů omezení a &lt;mezipaměti privateBytesLimit /&gt; je nastaveno, mezipaměť používat minimálně dva.
- Stejně jako v 1.x, vytvoříme rozevírací položky mezipaměti a volání uvolňování paměti. Shromážděte dvou důvodů: 

    - Nepovedlo se velmi blízko limitu Nesdílené bajty
    - Dostupná paměť je téměř nebo menší než 10 %
- Efektivně můžete zakázat uvolnění dočasné paměti a mezipaměti pro nedostatek dostupné paměti tak, že nastavíte &lt;mezipaměti percentagePhysicalMemoryUseLimit /&gt; do 100.
- Na rozdíl od 1.x, pozastaví 2.0 uvolnění dočasné paměti a shromažďování volání, pokud poslední uvolnění GC. Shromáždit snížení Nesdílené bajty nebo velikosti spravované haldy ve více než 1 % omezení paměti (v mezipaměti).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Závislosti vlastní mezipaměti

1. Vytvořte nový web.
2. Přidat nový soubor XML s názvem cache.xml a uložte ho do kořenového adresáře webové aplikace.
3. Přidejte následující kód na stránku\_metoda v kódu default.aspx zátěže: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Přidejte následující text k horní části default.aspx v zobrazení zdroje: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Procházejte Default.aspx. Co říká čas?
6. Aktualizujte prohlížeč. Co říká čas?
7. Otevřete cache.xml a přidejte následující kód: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Uložte cache.xml.
9. Aktualizujte svůj prohlížeč. Co říká čas?
10. Vysvětluje, proč aktualizaci místo hodnoty uložené v mezipaměti:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Praktické cvičení 2: Použití závislostí mezipaměti na základě cyklického dotazování

Toto testovací prostředí používá projekt, který jste vytvořili v předchozí modul, který umožňuje úpravy dat v databázi Northwind pomocí ovládacího prvku GridView a prvku DetailsView.

1. Otevřete projekt v sadě Visual Studio 2005.
2. Spustit aspnet\_regsql nástroj umožňující databáze a tabulky produktů v databázi Northwind. Použijte následující příkaz z příkazového řádku aplikace Visual Studio: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Do souboru web.config přidejte následující: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Přidání nového webového formuláře volá showdata.aspx.
5. Přidejte následující @ direktivy outputcache showdata.aspx stránky: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Přidejte následující kód na stránku\_zatížení showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Přidat nový ovládací prvek SqlDataSource showdata.aspx a nakonfigurujte ho na použití připojení k databázi Northwind. Klikněte na tlačítko Další.
8. Vyberte zaškrtávací políčka ProductName a ProductID a klikněte na tlačítko Další.
9. Klikněte na tlačítko Dokončit.
10. Přidání nového ovládacího prvku GridView showdata.aspx stránku.
11. Zvolte SqlDataSource1 z rozevíracího seznamu.
12. Uložit a procházet showdata.aspx. Poznamenejte si dobu zobrazí.
