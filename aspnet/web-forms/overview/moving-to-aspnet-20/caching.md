---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: "Ukládání do mezipaměti | Microsoft Docs"
author: microsoft
description: "Představu o ukládání do mezipaměti je důležité pro dobře provádění aplikaci ASP.NET. ASP.NET 1.x nabízí tři různé možnosti pro ukládání do mezipaměti; ukládání výstupu do mezipaměti..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: d3ef613f625d862314eb0bb60f083f60bb2317e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="caching"></a>Ukládání do mezipaměti
====================
podle [Microsoft](https://github.com/microsoft)

> Představu o ukládání do mezipaměti je důležité pro dobře provádění aplikaci ASP.NET. ASP.NET 1.x nabízí tři různé možnosti pro ukládání do mezipaměti; ukládání výstupu do mezipaměti, fragment ukládání do mezipaměti a mezipaměti rozhraní API.


Představu o ukládání do mezipaměti je důležité pro dobře provádění aplikaci ASP.NET. ASP.NET 1.x nabízí tři různé možnosti pro ukládání do mezipaměti; ukládání výstupu do mezipaměti, fragment ukládání do mezipaměti a mezipaměti rozhraní API. Technologie ASP.NET 2.0 nabízí všechny tři z těchto metod, ale přidá některé důležité další funkce. Existuje několik nové závislosti mezipaměti a vývojáři Teď máte možnost vytvořit vlastní mezipaměti závislosti také. Konfigurace ukládání do mezipaměti se také výrazně zlepšila v technologii ASP.NET 2.0.

## <a name="new-features"></a>Nové funkce

## <a name="cache-profiles"></a>Profily mezipaměti

Mezipaměť profily umožňují vývojářům k definování nastavení konkrétní mezipaměti, které lze použít na jednotlivé stránky. Například pokud máte nějaké stránky, které by měl z mezipaměti vyprší po 12 hodinách, můžete snadno vytvořit profil mezipaměti, který můžete použít tyto stránek. Chcete-li přidat nový profil mezipaměti, použijte &lt;outputCacheSettings&gt; oddíl v konfiguračním souboru. Níže je konfigurace profilu mezipaměti názvem například *twoday* , nakonfiguruje mezipaměti dobu trvání 12 hodin.

[!code-xml[Main](caching/samples/sample1.xml)]

Použít tento profil mezipaměti na konkrétní stránku, použijte atribut CacheProfile direktivy @ OutputCache, jak je uvedeno níže:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Vlastní mezipaměti závislosti

Vývojáři ASP.NET 1.x Báječné pro vlastní mezipaměti závislosti. V technologii ASP.NET 1.x, třída CacheDependency byla zapečetěná které prevented vývojářům provádět odvození vlastní třídy z něj. V aplikaci ASP.NET 2.0 tohoto omezení odstraněno a vývojáři mohou vyvíjet vlastní závislosti vlastní mezipaměti. Třída CacheDependency umožňuje vytvoření závislosti vlastní mezipaměti na základě souborů, adresáře nebo klíče mezipaměti.

Například následující kód vytvoří novou vlastní mezipaměti závislost podle do souboru s názvem stuff.xml umístěného v kořenovém webové aplikace:

[!code-csharp[Main](caching/samples/sample3.cs)]

Tento scénář když se změní soubor stuff.xml, položky v mezipaměti, je neplatná.

Je také možné vytvořit vlastní mezipaměti závislost pomocí klíče mezipaměti. Tuto metodu použijete, odstranění klíče mezipaměti zrušíte platnost data uložená v mezipaměti. Následující příklad ilustruje toto:

[!code-csharp[Main](caching/samples/sample4.cs)]

Zneplatní položce, který byl vložen výše, stačí odstraňte položku, který byl vložen do mezipaměti, aby fungoval jako klíče mezipaměti.

[!code-csharp[Main](caching/samples/sample5.cs)]

Všimněte si klíč položky, který funguje jako klíč mezipaměti musí být stejná jako hodnota přidat do pole klíče mezipaměti.

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>Na základě dotazování závislosti mezipaměti SQL*(také nazývané cyklické závislosti)*

SQL Server 7 a 2000 pomocí modelu na základě dotazování pro závislosti mezipaměti SQL. Na tabulku databáze, která se aktivuje při změně dat v tabulce používá model na základě cyklického dotazování aktivační událost. Který aktivovat aktualizace **changeId** v tabulce oznámení, která pravidelně kontroluje ASP.NET. Pokud **changeId** pole se aktualizovala, ASP.NET ví, že data změnily a by způsobila neplatnost data uložená v mezipaměti.

> [!NOTE]
> SQL Server 2005 můžete také použít modelu na základě dotazování, ale protože modelu na základě dotazování není nejúčinnější modelu, je vhodné použít model na základě dotazů (popsané později) s SQL Server 2005.


Aby závislost SQL mezipaměti pomocí modelu na základě dotazování fungovala správně musí mít v tabulkách oznámení povolena. Můžete to provést programově pomocí třídy SqlCacheDependencyAdmin nebo pomocí aspnet\_regsql.exe nástroj.

Následující příkazový řádek zaregistruje tabulky produktů v databázi Northwind nachází na instanci systému SQL Server s názvem *dbase* závislosti mezipaměti SQL.

[!code-console[Main](caching/samples/sample6.cmd)]

Následuje vysvětlení použít ve výše uvedeném příkazu přepínače příkazového řádku:

| **Přepínač příkazového řádku** | **Účel** |
| --- | --- |
| -S *serveru* | Určuje název serveru. |
| -ed | Určuje, že se má povolit databázi pro závislost mezipaměti SQL. |
| -d *databáze\_název* | Určuje název databáze, která by měla být zapnutá mezipaměti závislost SQL. |
| -E | Určuje, že aspnet\_regsql by měl použít ověřování systému Windows, pokud se připojujete k databázi. |
| -et | Určuje, povolit databázové tabulky pro závislost mezipaměti SQL. |
| -t *tabulky\_název* | Určuje název tabulky databáze, pokud chcete povolit pro mezipaměť závislost SQL. |

> [!NOTE]
> Nejsou k dispozici pro aspnet další přepínače\_regsql.exe. Úplný seznam, spusťte aspnet\_regsql.exe-? z příkazového řádku.


Pokud tento příkaz spustí jsou provedeny následující změny k databázi systému SQL Server:

- **AspNet\_SqlCacheTablesForChangeNotification** tabulka je přidána. Tato tabulka obsahuje jeden řádek pro každou tabulku v databázi, pro které bylo povoleno závislost SQL mezipaměti.
- Následující uložené procedury jsou vytvořeny v databázi:


| AspNet\_SqlCachePollingStoredProcedure | Dotazy AspNet\_SqlCacheTablesForChangeNotification tabulce a vrátí všechny tabulky, které jsou povoleny pro mezipaměť závislost SQL a hodnotu changeId pro každou tabulku. Tato uložená procedura se používá pro cyklické dotazování k určení, pokud se změnily data. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Vrátí všechny tabulky povolené pro závislost mezipaměti SQL pomocí dotazu AspNet\_SqlCacheTablesForChangeNotification tabulce a vrátí všechny tabulky povolené pro SQL závislosti mezipaměti. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Zaregistruje tabulku pro závislost mezipaměti SQL přidáním nezbytné položky v tabulce oznámení a přidá aktivační událost. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Zruší registraci tabulku pro závislost mezipaměti SQL odebráním položku v tabulce oznámení a odebírá aktivační událost. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Aktualizace tabulky oznámení podle zvyšování changeId změněné tabulky. ASP.NET používá k určení, pokud data změnili tuto hodnotu. Jak je uvedeno níže, tato uložená procedura se spustí aktivační procedura vytvoří, když je povolena v tabulce. |


- Aktivační události SQL Server volá  ***tabulky\_název*\_AspNet\_SqlCacheNotification\_aktivační událost** se vytvoří pro tabulku. Této aktivační události provede AspNet\_SqlCacheUpdateChangeIdStoredProcedure při vložení, aktualizaci nebo odstranění v tabulce.
- Role systému SQL Server volá **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** je přidáno do databáze.

**Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** role systému SQL Server má oprávnění EXEC k AspNet\_SqlCachePollingStoredProcedure. Aby dotazování modelu, který má fungovat správně, musíte přidat účet procesu aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess role. Aspnet\_regsql.exe nástroj nebude to pro vás.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Konfigurace mezipaměti závislosti na základě dotazování SQL

Existuje několik kroků, které jsou požadovány pro konfiguraci na základě dotazování závislosti mezipaměti SQL. Prvním krokem je povolit databáze a tabulky, jak je popsáno výše. Po dokončení tohoto kroku zbytek konfigurace je následující:

- Konfiguruje se konfigurační soubor technologie ASP.NET.
- Konfigurace SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Konfigurace konfigurační soubor technologie ASP.NET

Kromě přidání připojovací řetězec, jak je popsáno v předchozí modul, je nutné také nakonfigurovat &lt;mezipaměti&gt; element s &lt;sqlCacheDependency&gt; element, jak je uvedeno níže:

[!code-xml[Main](caching/samples/sample7.xml)]

Tato konfigurace umožňuje závislost SQL mezipaměti na *pubs* databáze. Všimněte si, že atribut pollTime &lt;sqlCacheDependency&gt; element výchozí 60000 milisekundách nebo 1 minuta. (Tato hodnota nemůže být menší než 500 milisekund). V tomto příkladu &lt;přidat&gt; element přidá novou databázi a přepíše pollTime, jeho nastavení na hodnotu 9000000 milisekundách.

#### <a name="configuring-the-sqlcachedependency"></a>Konfigurace SqlCacheDependency

Dalším krokem je konfigurace SqlCacheDependency. Nejjednodušší způsob, jak provést tuto akci je zadejte hodnotu pro atribut SqlDependency v direktivě @ Outcache následujícím způsobem:

[!code-aspx[Main](caching/samples/sample8.aspx)]

Ve výše uvedené direktivy @ OutputCache závislost SQL mezipaměti je nakonfigurován pro *autoři* tabulky v *pubs* databáze. Více závislostí můžete nakonfigurovat jejich oddělením středníkem takto:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Další metodou konfigurace SqlCacheDependency je k tomu prostřednictvím kódu programu. Následující kód vytvoří novou mezipaměť závislost SQL na *autoři* tabulky v *pubs* databáze.

[!code-csharp[Main](caching/samples/sample10.cs)]

Jednou z výhod prostřednictvím kódu programu definice mezipaměti závislost SQL je, že lze zpracovat všechny výjimky, které může dojít k. Například, pokud se pokusíte definovat mezipaměti závislost SQL pro databázi, která nebyla povolena pro oznámení **databasenotenabledfornotificationexception –** dojde k výjimce. V takovém případě můžete zkusit povolit databázi pro oznámení při volání **SqlCacheDependencyAdmin.EnableNotifications** metoda a předejte jí název databáze.

Podobně, pokud se pokusíte definovat mezipaměti závislost SQL pro tabulku, která nebyla povolena pro oznámení, **tablenotenabledfornotificationexception –** bude vyvolána. Potom můžete zavolat **SqlCacheDependencyAdmin.EnableTableForNotifications** metoda předání název databáze a název tabulky.

Následující příklad kódu ukazuje, jak správně nakonfigurovat výjimek při konfiguraci závislosti mezipaměti SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Další informace: [https://msdn.microsoft.com/en-us/library/t9x04ed2.aspx](https://msdn.microsoft.com/en-us/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Závislosti mezipaměti na základě dotazů SQL (pouze SQL Server 2005)

Pokud používáte systém SQL Server 2005 pro závislost mezipaměti SQL, není nutné modelu na základě cyklického dotazování. Při použití s SQL Server 2005, SQL mezipaměti závislosti komunikovat přímo přes připojení SQL k instanci systému SQL Server (není potřeba žádná další konfigurace) pomocí oznámení dotazů SQL Server 2005.

Nejjednodušší způsob, jak povolit oznámení založené na dotazu je provést to deklarativně nastavením **SqlCacheDependency** objekt zdroje dat do atribut **CommandNotification** a nastavení **EnableCaching** atribut **true**. Tuto metodu použijete, je požadován žádný kód. Pokud výsledek příkaz spustit pro data změny zdrojů, dojde ke zrušení platnosti data v mezipaměti.

Následující příklad konfiguruje ovládací prvek zdroje dat pro závislost mezipaměti SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

V tomto případě, pokud dotaz zadané v **SelectCommand** vrátí výsledku jiný než to udělali původně, jsou zneplatněné výsledky, které jsou uložené v mezipaměti.

Můžete také určit, že všechny vaše zdroje dat pro závislosti mezipaměti SQL povolit nastavením **SqlDependency** atribut **@ OutputCache** direktivy k **CommandNotification** . Následující příklad znázorňuje to.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Další informace o oznámení dotazu se v systému SQL Server 2005 najdete v článku webu knihy Online pro SQL Server.


Další metodou konfiguraci závislosti mezipaměti na základě dotazů SQL je k tomu programově pomocí třídy SqlCacheDependency. Následující příklad kódu ukazuje, jak to lze provést.

[!code-csharp[Main](caching/samples/sample14.cs)]

Další informace: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Substituce mezipaměti

Ukládání do mezipaměti na stránce může výrazně zvýšit výkon webové aplikace. Ale v některých případech musíte většinu stránky do mezipaměti a některé fragmenty v rámci stránky být dynamické. Například pokud vytvoříte stránky scénářů zprávy, která je po určitou dobu zcela statická, můžete nastavit celou stránku ukládat do mezipaměti. Pokud jste chtěli připojit rotační hlavičku ad, který změnil na každý požadavek na stránku, pak část stránky, která obsahuje oznámení o inzerovaném programu musí být dynamické. Abyste mohli do mezipaměti na stránce, ale nahraďte obsah dynamicky, můžete použít nahrazení po mezipaměti technologie ASP.NET. Celá stránka s mezipaměti po nahrazení, je do výstupní mezipaměti s konkrétní části označena jako výjimka z ukládání do mezipaměti. V příkladu hlaviček ad ovládacího prvku AdRotator umožňuje využít výhod nahrazení po mezipaměti, aby reklamy dynamicky vytvořit pro každého uživatele a pro každou aktualizací stránky.

Existují tři způsoby, jak implementovat nahrazení po mezipaměti:

- Deklarativně pomocí ovládacího prvku nahrazení.
- Programově pomocí rozhraní API ovládacího prvku nahrazení.
- Implicitně pomocí ovládacího prvku AdRotator.

### <a name="substitution-control"></a>Náhradní ovládací prvek

Ovládací prvek ASP.NET nahrazení určuje část stránky uložené v mezipaměti, který se vytváří dynamicky spíše než v mezipaměti. Na stránce do umístění, kam chcete dynamický obsah, než se objeví umístíte ovládacího prvku nahrazení. V době běhu náhradní ovládací volá metodu, která zadáte s MethodName vlastností. Metoda musí vrátit řetězec, který nahradí obsah ovládacího prvku nahrazení. Metoda musí být na stránce nebo UserControl ovládacího prvku obsahujícího statickou metodu. Použití ovládacího prvku nahrazení způsobí, že možnost ukládání do mezipaměti na straně klienta se musí změnit mezipaměti na serveru, tak, aby stránce nebudou zapisována do mezipaměti na straně klienta. Tím se zajistí, že budoucí požadavky na stránku voláním metody znovu vygenerovat dynamický obsah.

### <a name="substitution-api"></a>Nahrazení rozhraní API

Chcete-li vytvořit dynamický obsah pro stránku v mezipaměti prostřednictvím kódu programu, můžete zavolat [WriteSubstitution](https://msdn.microsoft.com/en-us/library/system.web.httpresponse.writesubstitution.aspx) metodu v kódu stránky a předejte jí název metody jako parametr. Metoda, která zpracovává vytvoření dynamického obsahu přebírá jediný [HttpContext](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.aspx) parametr a vrátí řetězec. Vrácený řetězec je obsah, který bude nahrazena v daném umístění. Výhodou volání metody WriteSubstitution místo deklarativně pomocí ovládacího prvku nahrazení je můžete volat metodu libovolného objektu místo volání statickou metodu stránky nebo UserControl objektu.

Volání metody WriteSubstitution způsobí, že možnost ukládání do mezipaměti na straně klienta se musí změnit mezipaměti na serveru, tak, aby stránce nebudou zapisována do mezipaměti na straně klienta. Tím se zajistí, že budoucí požadavky na stránku voláním metody znovu vygenerovat dynamický obsah.

### <a name="adrotator-control"></a>Ovládací prvek AdRotator

AdRotator, který implementuje ovládací prvek serveru podporu pro nahrazení po mezipaměti interně. Pokud ovládací prvek AdRotator na stránku, se budou vykreslovat jedinečné reklamy na každý požadavek, bez ohledu na to, jestli je nadřazená stránka uložené v mezipaměti. Na stránce, která zahrnuje ovládací prvek AdRotator v důsledku toho je pouze v mezipaměti na serveru.

## <a name="controlcachepolicy-class"></a>ControlCachePolicy – třída

Třída ControlCachePolicy umožňuje programovací řízení fragmentu ukládání do mezipaměti pomocí uživatelské ovládací prvky. Vloží uživatelské ovládací prvky v rámci ASP.NET [BasePartialCachingControl](https://msdn.microsoft.com/en-us/library/system.web.ui.basepartialcachingcontrol.aspx) instance. Třída BasePartialCachingControl představuje uživatelský ovládací prvek, který má výstup povoleno ukládání do mezipaměti.

Při přístupu [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/en-us/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) vlastnost [PartialCachingControl](https://msdn.microsoft.com/en-us/library/system.web.ui.partialcachingcontrol.aspx) ovládací prvek, zobrazí se vždy platný objekt ControlCachePolicy. Ale pokud máte přístup k [UserControl.CachePolicy](https://msdn.microsoft.com/en-us/library/system.web.ui.usercontrol.cachepolicy.aspx) vlastnost [UserControl](https://msdn.microsoft.com/en-us/library/system.web.ui.usercontrol.aspx) řízení, obdržíte platný objekt ControlCachePolicy pouze v případě, že uživatelský ovládací prvek již zabalený Ovládací prvek BasePartialCachingControl. Pokud ji není zabalená, ControlCachePolicy objekt vrácený vlastností se při pokusu o manipulaci s, protože nemá přidružené BasePartialCachingControl generování výjimek. Pokud chcete zjistit, zda je instance UserControl podporuje ukládání do mezipaměti bez generování událostí výjimky, zkontrolujte [SupportsCaching](https://msdn.microsoft.com/en-us/library/system.web.ui.controlcachepolicy.supportscaching.aspx) vlastnost.

Používání třídy ControlCachePolicy je jedním z několika způsoby, můžete povolit ukládání výstupu do mezipaměti. Následující seznam popisuje metody, které můžete použít k povolení ukládání výstupu do mezipaměti:

- Použití [@ OutputCache](https://msdn.microsoft.com/en-us/library/hdxfb6cy.aspx) – direktiva povolit ukládání výstupu do mezipaměti v deklarativní scénáře.
- Použití [PartialCachingAttribute](https://msdn.microsoft.com/en-us/library/system.web.ui.partialcachingattribute.aspx) atribut pro povolení ukládání do mezipaměti pro uživatelský ovládací prvek v souboru kódu na pozadí.
- Použijte ControlCachePolicy třídu k určení nastavení mezipaměti v programové scénáře, ve kterých pracujete s BasePartialCachingControl instancí, které byly mezipaměť povolená pomocí jedné z výše uvedených metod a dynamicky načíst pomocí [System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/en-us/library/system.web.ui.templatecontrol.loadcontrol.aspx) metoda.

ControlCachePolicy instance smí uživatel manipulovat úspěšně jenom mezi Init a PreRender fázích životního cyklu ovládacího prvku. Pokud upravíte objekt ControlCachePolicy po fázi PreRender, ASP.NET vyvolá výjimku, protože veškeré změny provedené po vykreslení ovládacího prvku nelze ve skutečnosti ovlivňují nastavení mezipaměti (ovládacího prvku do mezipaměti při vykreslení fáze). Nakonec uživatelskou instanci ovládacího prvku (a proto jeho objekt ControlCachePolicy) je dostupná jenom pro programové manipulaci ve skutečnosti je vykreslen.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Změny konfigurace ukládání do mezipaměti – &lt;ukládání do mezipaměti&gt; – Element

Existuje několik změn v konfiguraci ukládání do mezipaměti v technologii ASP.NET 2.0. &lt;Ukládání do mezipaměti&gt; elementu je nového v technologii ASP.NET 2.0 a umožní vám provádět změny v konfiguraci ukládání do mezipaměti v konfiguračním souboru. Následující atributy jsou k dispozici.

| **Element** | **Popis** |
| --- | --- |
| **mezipaměti** | Volitelný element. Definuje globální nastavení mezipaměti aplikace. |
| **outputCache** | Volitelný element. Určuje nastavení výstupní mezipaměti celou aplikaci. |
| **outputCacheSettings** | Volitelný element. Určuje nastavení výstupní mezipaměti, které mohou být použity na stránky v aplikaci. |
| **sqlCacheDependency** | Volitelný element. Nakonfiguruje závislosti mezipaměti SQL pro aplikaci ASP.NET. |

### <a name="the-ltcachegt-element"></a>&lt;Mezipaměti&gt; – Element

Následující atributy jsou k dispozici v &lt;mezipaměti&gt; element:

| **Atribut** | **Popis** |
| --- | --- |
| **disableMemoryCollection** | Volitelné **Boolean** atribut. Získá nebo nastaví hodnotu, která určuje, jestli je zakázaná mezipaměť kolekce, která nastane, když je počítač je paměť přetížena. |
| **disableExpiration** | Volitelné **Boolean** atribut. Získá nebo nastaví hodnotu určující, zda je neaktivní doba vypršení platnosti mezipaměti. Pokud je zakázáno, nevyprší platnost položek v mezipaměti a proces úklidu pozadí položek vypršela platnost mezipaměti nedojde. |
| **privateBytesLimit** | Volitelné **Int64** atribut. Získá nebo nastaví hodnotu, což značí, že maximální velikost nesdílených bajtů aplikace před mezipaměť začne vyprazdňovat neplatné položky a pokouší získat paměť. Toto omezení se vztahuje jak paměti, které do mezipaměti a také režii běžné paměti běžící aplikaci. Nastavení 0 označuje, že technologie ASP.NET použije vlastní heuristické metody pro určení, kdy začne recyklovat paměť. |
| **percentagePhysicalMemoryUsedLimit** | Volitelné **Int32** atribut. Získá nebo nastaví hodnotu určující maximální procento počítače a fyzické paměti, která mohou být spotřebovávána aplikace před mezipaměť začne vyprazdňovat neplatné položky a pokus o uvolnění paměti, že tento využití paměti zahrnuje obě paměti, které také do mezipaměti jako využití paměti na normální běžící aplikaci. Nastavení 0 označuje, že technologie ASP.NET použije vlastní heuristické metody pro určení, kdy začne recyklovat paměť. |
| **privateBytesPollTime** | Volitelné **časový interval** atribut. Získá nebo nastaví hodnotu, která určuje časový interval mezi dotazování pro využití paměti nesdílených bajtů aplikace. |

### <a name="the-ltoutputcachegt-element"></a>&lt;OutputCache&gt; – Element

Následující atributy jsou k dispozici pro &lt;outputCache&gt; elementu.

| **Atribut** | **Popis** |
| --- | --- |
| **enableOutputCache** | Volitelné **Boolean** atribut. Povolí nebo zakáže výstupní mezipaměti stránky. Pokud zakázané, jsou bez ohledu na nastavení programový nebo deklarativní mezipaměti žádné stránky. Výchozí hodnota je **true**. |
| **enableFragmentCache** | Volitelné **Boolean** atribut. Povolí nebo zakáže mezipaměť fragmentu aplikace. Pokud zakázané, se žádné stránky do mezipaměti, bez ohledu na to [@ OutputCache](https://msdn.microsoft.com/en-us/library/hdxfb6cy.aspx) – direktiva nebo ukládání do mezipaměti profil. Zahrnuje typu cache-control záhlaví označující, že nadřazený proxy serverů, jakož i klienty prohlížeče neměli výstup stránky do mezipaměti. Výchozí hodnota je **false**. |
| **sendCacheControlHeader** | Volitelné **Boolean** atribut. Získá nebo nastaví hodnotu, která určuje zda **mezipaměti – ovládací prvek: privátní** záhlaví odesílají modul výstupní mezipaměti ve výchozím nastavení. Výchozí hodnota je **false**. |
| **omitVaryStar** | Volitelné **Boolean** atribut. Povolí nebo zakáže odesílání Http "**měnit: \*** " hlavičky v odpovědi. S výchozím nastavením false, "**měnit: \*** " záhlaví je odeslána pro stránky výstupu do mezipaměti. Při odeslání měnit hlavičky umožňuje pro různé verze ukládat do mezipaměti na základě zadaných v hlavičce měnit. Například *měnit: uživatel-agenty* uloží různé verze na základě uživatelského agenta vydání žádosti o stránky. Výchozí hodnota je **false**. |

### <a name="the-ltoutputcachesettingsgt-element"></a>&lt;OutputCacheSettings&gt; – Element

&lt;OutputCacheSettings&gt; element umožňuje pro vytvoření profilů mezipaměti výše popsané. Pouze podřízený element pro &lt;outputCacheSettings&gt; elementu je &lt;outputCacheProfiles&gt; element pro konfiguraci profilů mezipaměti.

### <a name="the-ltsqlcachedependencygt-element"></a>&lt;SqlCacheDependency&gt; – Element

Následující atributy jsou k dispozici pro &lt;sqlCacheDependency&gt; elementu.

| **Atribut** | **Popis** |
| --- | --- |
| **povoleno** | Požadované **Boolean** atribut. Určuje, zda jsou změny dotazován na. |
| **pollTime** | Volitelné **Int32** atribut. Nastaví frekvenci, se kterým SqlCacheDependency dotazuje na změny v tabulce databáze. Tato hodnota odpovídá počet milisekund, po mezi po sobě následujících dotazech. Nejde ji nastavit na míň než 500 milisekund. Výchozí hodnota je 1 minuta. |

### <a name="more-information"></a>Další informace

Existuje nějaké další informace, které byste měli znát z o konfiguraci mezipaměti.

- Pokud není nastavený limit pracovní proces nesdílených bajtů, mezipaměti používat jednu z následujících omezení: 

    - x86 2 GB: 800 MB nebo 60 % fyzické paměti RAM, podle toho, která je menší
    - x86 3 GB: 1 800 MB nebo 60 % fyzické paměti RAM, podle toho, která je menší
    - x 64: 1 terabajt nebo 60 % fyzické paměti RAM podle toho, která je menší
- Pokud oba pracovní proces soukromých bajtů omezení a &lt;mezipaměti privateBytesLimit /&gt; jsou nastavené, mezipaměti bude používat minimálně dva.
- Stejně jako v 1.x, jsme vyřaďte položky mezipaměti a volání GC. Shromážděte dvou důvodů: 

    - Jsme jsou velmi podobné limit nesdílených bajtů
    - Dostupná paměť je near nebo menší než 10 %
- Můžete efektivně zakázat uvolnění dočasné paměti a mezipaměti pro nedostatek dostupné paměti nastavením &lt;mezipaměti percentagePhysicalMemoryUseLimit /&gt; do 100.
- Na rozdíl od 1.x, 2.0 bude pozastavit volání uvolnění dočasné paměti a shromažďování Pokud poslední GC. Shromažďování ke snížení nesdílených bajtů nebo velikost spravovaných haldách o více než 1 % omezení paměti (mezipaměť).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Závislosti vlastní mezipaměti

1. Vytvoření nového webu.
2. Přidat nový soubor XML s názvem cache.xml a uložte ho do kořenového adresáře webové aplikace.
3. Přidejte následující kód na stránku\_načíst metodu v kódu default.aspx: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Přidejte následující do horní části default.aspx v zobrazení zdroje: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Procházejte Default.aspx. Co říká čas?
6. Aktualizujte stránku prohlížeče. Co říká čas?
7. Otevřete cache.xml a přidejte následující kód: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Uložte cache.xml.
9. Aktualizujte webový prohlížeč. Co říká čas?
10. Vysvětlit, proč čas aktualizovat místo zobrazení uložené v mezipaměti hodnoty:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratoř 2: Použití závislostí na základě dotazování mezipaměti

Toto testovací prostředí používá projekt, který jste vytvořili v předchozí modul, který umožňuje úpravy dat v databázi Northwind prostřednictvím ovládacího prvku GridView a DetailsView.

1. Otevřete projekt v sadě Visual Studio 2005.
2. Spustit aspnet\_regsql nástroj v databázi Northwind povolit databáze a tabulky produktů. Použijte následující příkaz z Visual Studio – příkazový řádek: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Přidejte následující do souboru web.config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Přidáte nový webový formulář, názvem showdata.aspx.
5. Přidejte následující @ outputcache direktiva na stránku showdata.aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Přidejte následující kód na stránku\_zatížení showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Přidat nový ovládací prvek SqlDataSource showdata.aspx a nakonfigurovat ho na použití připojení k databázi Northwind. Klikněte na tlačítko Další.
8. Vyberte zaškrtávací políčka ProductName a ProductID a klikněte na tlačítko Další.
9. Klikněte na tlačítko Dokončit.
10. Přidáte nové GridView na stránku showdata.aspx.
11. Z rozevíracího seznamu vyberte SqlDataSource1.
12. Uložte a procházet showdata.aspx. Poznamenejte si dobu zobrazí.
