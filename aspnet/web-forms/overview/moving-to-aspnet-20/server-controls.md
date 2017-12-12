---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: "Ovládací prvky serveru | Microsoft Docs"
author: microsoft
description: "ASP.NET 2.0 vylepšuje ovládací prvky serveru mnoha způsoby. V tomhle module jsme zaměříme některé architektury změny způsob technologii ASP.NET 2.0 a Visual Studio 200..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 09f1a2e4de024e5778e69fdd691d9cb0040459f3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="server-controls"></a>Ovládací prvky serveru
====================
podle [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 vylepšuje ovládací prvky serveru mnoha způsoby. V tomhle module jsme zaměříme některé architektury změny jako technologii ASP.NET 2.0 a Visual Studio 2005 obchodů s ovládacími prvky serveru.


ASP.NET 2.0 vylepšuje ovládací prvky serveru mnoha způsoby. V tomhle module jsme zaměříme některé architektury změny jako technologii ASP.NET 2.0 a Visual Studio 2005 obchodů s ovládacími prvky serveru.

## <a name="view-state"></a>Stav zobrazení

Primární změny ve stavu zobrazení v technologii ASP.NET 2.0 je k výraznému snížení velikosti. Vezměte v úvahu zobrazí stránka s pouze ovládacího prvku kalendář na něm. Zde je stav zobrazení v technologii ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Nyní zde je stav zobrazení na stejné stránce v technologii ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Které poměrně významné změny, a vzhledem k tomu, že je stav zobrazení a zpět přenášejí sítě, tato změna může poskytnout vývojáři k výraznému zvýšení výkonu. Snížení velikosti zobrazení stavu je z velké části kvůli způsobu, jakým ho jsme interně zpracovat. Mějte na paměti, že řetězec kódovaný zobrazení stavu je s kódováním Base64. Abyste lépe pochopili, změny v zobrazení stavu v technologii ASP.NET 2.0, můžeme Podíváme se na dekódované hodnoty z výše uvedených příkladech.

Tady je stav zobrazení 1.1 dekódovat:

[!code-css[Main](server-controls/samples/sample3.css)]

To může vypadat trochu nesrozumitelné, ale je tady se vzorem. V technologii ASP.NET 1.x, můžeme použít jeden znaků k identifikaci datové typy a oddělený hodnoty pomocí &lt; &gt; znaků. "T" v zobrazení stavu ukázce výše představuje trojdílná. Trojdílná obsahuje pár ArrayLists ("l" představuje na ArrayList.) Jeden z těchto ArrayLists obsahuje typu intl32 ("i") s hodnotou 1 a druhý obsahuje jiné trojdílná. Trojdílná obsahuje pár ArrayLists atd. Je důležité si pamatovat je, že používáme trojice, které obsahují páry, jsme identifikovat typy dat prostřednictvím písmeno a používáme &lt; a &gt; znaků jako oddělovače.

V aplikaci ASP.NET 2.0 vypadá trochu jinak dekódované zobrazení stavu.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Měli byste zaznamenat obrovské změnu vzhled dekódované zobrazení stavu. Tato změna má několik podchycení architektury. Zobrazení stavu technologie ASP.NET 1.x používaný k serializaci dat LosFormatter. V 2.0 použijeme novou třídu ObjectStateFormatter. Tato třída byla určená speciálně pro podporu serializace a deserializace stav zobrazení a řízení stavu. (Řízení stavu se budeme v další části.) Existuje mnoho výhody změnou metody, pomocí kterého serializace a deserializace provést. Jedním z nejvíce výrazné je fakt, že na rozdíl od LosFormatter, který používá TextWriter, používá ObjectStateFormatter BinaryWriter. To umožňuje technologii ASP.NET 2.0 k uložení stavu zobrazení řady bajtů namísto řetězců. Využít, například celé číslo. V technologii ASP.NET 1.1 vyžaduje celé 4 bajtů zobrazení stavu. V aplikaci ASP.NET 2.0 že stejné číslo vyžaduje pouze 1 bajtů. Další vylepšení byly provedeny snížit množství stav zobrazení, která je uložena. Hodnoty data a času, například jsou nyní uloženy pomocí TickCount místo řetězec.

Jako by všechny této nebyly dostatek, zvláštní pozornost byla věnována na skutečnost, že byl jeden ze největší příjemci zobrazení stavu v 1.x DataGrid a podobné ovládacích prvků. Hlavní nevýhodou ovládacích prvků, jako je například DataGrid, kde je stav zobrazení problémem je, že často obsahuje velké množství opakovaných informací. V technologii ASP.NET 1.x, který opakuje informace uložil jednoduše přes a přes znovu výsledkem opakovaném zobrazení stavu. V aplikaci ASP.NET 2.0 použijeme novou třídu IndexedString k uložení těchto údajů. Pokud se řetězec opakovat, uložíme právě token IndexedString a index ve spuštěné tabulce objektů IndexedString.

## <a name="control-state"></a>Řízení stavu

Jedním z hlavních gripes, které vývojáři měl stav zobrazení se velikost, která ho přidat do datové části protokolu HTTP. Jak už jsme zmínili, mezi největší příjemci ve stavu zobrazení je DataGrid – ovládací prvek. Abyste se vyhnuli obrovské množství vygenerovaných DataGrid stav zobrazení, celá řada vývojářů jednoduše zakázána stav zobrazení pro ovládací prvek. Toto řešení nebyla bohužel vždy problém. Zobrazení stavu technologie ASP.NET 1.x obsahuje pouze data nezbytná pro správné fungování ovládacího prvku. Také obsahuje informace o stavu uživatelského ovládacího prvku rozhraní. To znamená, že pokud chcete, aby bylo možné stránkování v kolekci DataGrid, je nutné povolit stav zobrazení, i v případě nepotřebujete všechny informace uživatelského rozhraní, které zobrazit stav obsahuje. Je vše nebo nic scénář.

V aplikaci ASP.NET 2.0 řeší řízení stavu tohoto problému vhodně prostřednictvím zavedení řízení stavu. Řízení stavu obsahuje data, která je nezbytně nutné pro správné fungování ovládacího prvku. Na rozdíl od zobrazení stavu nelze zakázat řízení stavu. Proto je důležité, že se data ukládají v řízení stavu pečlivě řídí.

> [!NOTE]
> Řízení stavu jako trvalý, spolu se stavem zobrazení v \_ \_VIEWSTATE skryté pole formuláře.


Toto video je návod, zobrazení stavu a stavu ovládacího prvku.


![](server-controls/_static/image1.png)


[Otevřete Video přes celou obrazovku](server-controls/_static/state1.wmv)


Aby ovládacího prvku serveru ke čtení a zápisu pro řízení stavu je třeba provést tři kroky.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Krok 1: Volání metody RegisterRequiresControlState

Metoda RegisterRequiresControlState informuje ASP.NET, že ovládacího prvku musí zachovat řízení stavu. Trvá jeden argument typu ovládacího prvku, který je ovládací prvek, která je registrována.

Je důležité si uvědomit, že registrace není zachována z požadavku na žádost. Proto tato metoda musí být volána u každého požadavku, pokud ovládací prvek je zachování stavu ovládacího prvku. Doporučuje se, že se v funkce OnInit volání metody.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Krok 2: Přepsání SaveControlState

Metoda SaveControlState uloží změny stavu pro ovládací prvek řízení od poslední post zpět. Vrátí objekt představující stav ovládacího prvku.

## <a name="step-3-override-loadcontrolstate"></a>Krok 3: Přepsání LoadControlState

Metoda LoadControlState načte uložený stav do ovládacího prvku. Metoda přijímá jeden argument typu objektu, který obsahuje uložený stav pro ovládací prvek.

## <a name="full-xhtml-compliance"></a>Úplné XHTML dodržování předpisů

Všechny Web developer zná význam standardy ve webových aplikacích. Aby byla zachována založených na standardech vývojového prostředí, technologii ASP.NET 2.0 je plně kompatibilní XHTML. Proto jsou všechny značky vykreslené podle normy XHTML v prohlížečích podporujících formát HTML 4.0 nebo vyšší.

Definice DOCTYPE v technologii ASP.NET 1.1 byla následujícím způsobem:

[!code-html[Main](server-controls/samples/sample6.html)]

V aplikaci ASP.NET 2.0 výchozí definici DOCTYPE vypadá takto:

[!code-html[Main](server-controls/samples/sample7.html)]

Pokud si zvolíte, můžete změnit výchozí XHML dodržování prostřednictvím uzlu xhtmlConformance v konfiguračním souboru. Například následující uzly v souboru web.config se změní XHTML dodržování předpisů na XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Pokud si zvolíte, můžete také nakonfigurovat technologii ASP.NET pomocí starší verzi konfigurace, které jsou použity v technologii ASP.NET 1.x následujícím způsobem:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Adaptivního vykreslování pomocí adaptérů

V technologii ASP.NET 1.x konfiguračního souboru obsažené &lt;browserCaps&gt; oddíl, který vyplní objekt HttpBrowserCapabilities. Tento objekt povolený vývojář zjistit, jaké zařízení se vytváření konkrétního požadavku a odpovídajícím způsobem vykreslení kódu. V aplikaci ASP.NET 2.0 modelu je vylepšený a teď používá nová třída ControlAdapter. Třída ControlAdapter přepsání události v průběhu životního cyklu ovládacího prvku a řídí vykreslování ovládacích prvků na základě možnosti uživatelského agenta. Soubor definice prohlížeče (soubor s příponou souboru Browser) uložené v c:\windows\microsoft.net\framework\v2.0 jsou definovány možnosti konkrétního uživatelského agenta. \* \* \* \*Složky \CONFIG\Browsers.

> [!NOTE]
> Třída ControlAdapter je abstraktní třída.


Podobně jako &lt;browserCaps&gt; oddíl v 1.x, prohlížeč definiční soubor se používá regulární výraz k analýze řetězec uživatelského agenta za účelem zjištění požadujícího prohlížeče. Ho je definuje konkrétní možnosti pro tento uživatelský agent. ControlAdapter vykreslí prostřednictvím metody vykreslení ovládacího prvku. Proto pokud přepíšete metodu vykreslování, by neměla volat vykreslení na základní třídy. Díky tomu může způsobit vykreslování proběhnout dvakrát, jednou pro adaptér a jednou pro samotném ovládacím prvku.

## <a name="developing-a-custom-adapter"></a>Vývoj vlastní adaptér

Dědění ze ControlAdapter můžete vyvíjet vlastní vlastní adaptéru. Kromě toho můžete dědění z abstraktní třídy PageAdapter v případech, kde je potřeba adaptér pro stránku. Mapování ovládacích prvků, aby váš vlastní adaptér se provádí prostřednictvím &lt;controlAdapters&gt; element v souboru definice prohlížeče. Například následující kód XML z definičního souboru prohlížeče mapy ovládacího prvku nabídky k třídě MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Pomocí tohoto modelu, stane se poměrně snadno vývojář řízení cílit na určité zařízení nebo prohlížeče. Je také úplně jednoduché pro vývojáře mít plnou kontrolu nad způsob vykreslení stránky na každé zařízení.

## <a name="per-device-rendering"></a>Vykreslování na zařízení

Vlastnosti ovládacích prvků serveru v technologii ASP.NET 2.0 může být zadaný podle zařízení pomocí předpony specifické pro prohlížeč. Například následující kód změní Text popisku, který v závislosti na zařízení, která je používána pro přejděte na stránku.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Když z Internet Exploreru v prohlížeči stránku obsahující tento popisek popisek zobrazí text oznámením "Procházíte z Internet Exploreru." Při zobrazení stránky v prohlížeči z Firefox, popisek se zobrazí text "Procházíte z Firefox." Při zobrazení stránky v prohlížeči z jiných zařízení, se zobrazí "Procházíte z neznámého zařízení." Vlastnost, lze pomocí této syntaxe speciální.

## <a name="setting-focus"></a>Nastavení fokusu

Vývojáři ASP.NET 1.x časté o tom, jak nastavit počáteční fokus na určitý ovládací prvek. Například na přihlašovací stránku, je užitečné používat textové pole ID uživatele získat fokus při prvním načtení stránky. V technologii ASP.NET 1.x, tato funkce vyžaduje, zápis některých klientský skript. I když takového skriptu je jednoduchý úkol, je už nezbytné v technologii ASP.NET 2.0 díky metody SetFocus. Metody SetFocus přijímá jeden argument indikujících řízení, které by měly dostávat fokus. Tento argument může být buď ID klienta ovládacího prvku jako řetězec, nebo název ovládacího prvku serveru jako objekt ovládacího prvku. Například nastavit počáteční fokus na ovládací prvek textové pole názvem txtUserID při prvním načtení stránky, přidejte následující kód na stránku\_zatížení:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--nebo

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 obslužná rutina Webresource.axd (jak jsme vysvětlili výše) používá k vykreslení klientské funkce, která nastaví fokus. Název funkce na straně klienta je webový formulář\_AutoFocus, jak je vidět tady:

[!code-html[Main](server-controls/samples/sample14.html)]

Alternativně můžete použít metodu fokus pro ovládací prvek nastavit počáteční fokus u daného ovládacího prvku. Metoda fokus odvozena ze třídy Control a je k dispozici pro všechny ovládací prvky ASP.NET 2.0. Také je možné nastavit fokus určitý ovládací prvek, když dojde k chybě ověření. Která se bude vztahovat v novější modulu.

## <a name="new-server-controls-in-aspnet-20"></a>Nové ovládací prvky serveru technologie ASP.NET 2.0

Níže jsou uvedeny nové serverové ovládací prvky v technologii ASP.NET 2.0. Přejdete do více podrobností u některých z nich v novější moduly.

## <a name="imagemap-control"></a>Ovládací prvek obrazová mapa

Ovládací prvek obrazová mapa umožňuje přidat hotspotům na obrázek, který můžete zahájit příspěvku na zpět nebo přejděte na adresu URL. Existují tři typy hotspotům není k dispozici. Kruhový aktivní bod, RectangleHotSpot a PolygonHotSpot. Aktivní body jsou přidané pomocí editor kolekce v sadě Visual Studio nebo z programu v kódu. Není k dispozici pro kreslení hotspotům na bitovou kopii žádné uživatelské rozhraní. Souřadnice a velikost nebo radius aktivního bodu musí být zadán deklarativně. Je také žádné vizuální znázornění aktivního bodu v návrháři. Pokud aktivní bod je konfigurován pro přechod na adresu URL, adresa URL je určena přes vlastnost NavigateUrl aktivního bodu. V případě příspěvku na zpět hotspotů PostBackValue vlastnost umožňuje předat řetězec v blogu zpět, který se dá načíst v kódu na straně serveru.


![Editor kolekce aktivních bodů v sadě Visual Studio](server-controls/_static/image1.jpg)

**Obrázek 1**: hotspotů Editor kolekce v sadě Visual Studio


## <a name="bulletedlist-control"></a>Ovládací prvek BulletedList

Ovládací prvek BulletedList je seznam s odrážkami, který lze snadno představovat data vázaná. V seznamu lze provést řazení (číslovaný) nebo je neuspořádaného přes vlastnost BulletStyle. Každá položka v seznamu je reprezentována ListItem objektu.


![Ovládací prvek BulletedList v sadě Visual Studio](server-controls/_static/image1.gif)

**Obrázek 2**: BulletedList ovládací prvek v sadě Visual Studio


## <a name="hiddenfield-control"></a>Ovládací prvek HiddenField

Ovládací prvek HiddenField přidá skryté pole formuláře na stránku, jehož hodnota je k dispozici v kódu na straně serveru. Hodnota skryté pole formuláře se obecně očekává zůstanou nezměněny mezi voláními post. Je však možné, uživatel se zlými úmysly Chcete-li změnit hodnotu před odeslat zpět. Pokud k tomu dojde, ovládacího prvku HiddenField vyvolá událost ValueChanged. Pokud máte v ovládacím prvku HiddenField citlivé informace a chcete zajistit, že zůstává beze změny, by měl zpracovat událost ValueChanged ve vašem kódu.

## <a name="fileupload-control"></a>Ovládací prvek odesílání souborů při odpovědích

Odesílání souborů při odpovědích ovládací prvek v technologii ASP.NET 2.0 umožňuje odeslat soubory na webový server prostřednictvím stránky ASP.NET. Tento ovládací prvek je velmi podobný Třída HtmlInputFile 1.x ASP.NET s několika výjimkami. V technologii ASP.NET 1.x, se doporučuje, aby bylo možné zjistit, pokud jste měli soubor dobrý vlastnost PostedFile zkontrolovat hodnotu null. V technologii ASP.NET 2.0 ovládacího prvku odesílání souborů při odpovědích přidá novou vlastnost HasFile, můžete použít k tomuto účelu a trochu je efektivnější.

Vlastnost PostedFile je stále k dispozici pro přístup k objektu HttpPostedFile, ale některé funkce HttpPostedFile je nyní k dispozici vnitřně pomocí ovládacího prvku odesílání souborů při odpovědích. Například pro uložení souboru nahrané v ASP.NET 1.x, zavolejte metodu uložit jako HttpPostedFile objektu. Použití ovládacího prvku odesílání souborů při odpovědích v technologii ASP.NET 2.0, by volat metodu uložit jako u ovládacího prvku odesílání souborů při odpovědích sám sebe.

Další důležitá změna v chování 2.0 (a pravděpodobně nejvýznamnějších změn) je, že už není potřeba načíst celý soubor nahrané do paměti před uložením. V 1.x, odeslaný soubor je uložen zcela do paměti před zápisu na disk. Tato architektura zabraňuje nahrávání velkých souborů.

V aplikaci ASP.NET 2.0, atribut requestLengthDiskThreshold elementu httpRuntime můžete konfigurovat počet kilobajtů jsou ukládány do vyrovnávací paměti v paměti před zápisu na disk.

**Důležité**: dokumentace MSDN (a dokumentaci jinde) určuje, že tato hodnota je v bajtech (nikoli v kilobajtech) a že výchozí hodnota je 256. Hodnota je ve skutečnosti zadány v kilobajtech a výchozí hodnota je 80. Tak, že výchozí hodnota 80 kB, je zajištěno, že vyrovnávací paměť nemá na konci v haldě velkého objektu.

## <a name="wizard-control"></a>Ovládací prvek Průvodce

Je celkem běžné setkat usilující se pokus o získání informací v řadě "stránek" pomocí panelů nebo přenesení stránky vývojáře využívající technologii ASP.NET. Častěji omezené úsilí je frustrující a časově náročné. Nový ovládací prvek Průvodce řeší tím, že pro lineární a lineární kroky v Průvodci rozhraní, které uživatelé se seznámíte s problémy. Ovládací prvek Průvodce uvede vstupní formuláře v sérii kroků. Každý krok je konkrétní typ určeného vlastností StepType ovládacího prvku. Typy krok k dispozici jsou následující:

| **Typ kroku** | **Vysvětlení** |
| --- | --- |
| Auto | Průvodce automaticky určuje typ kroku založené na jejich umístění v rámci hierarchie krok. |
| Spustit | Prvním krokem, často používají k úvodní příkaz. |
| Krok | Normální krok. |
| Dokončit | Poslední krok, obvykle používá k dispozici tlačítko a dokončete průvodce. |
| Dokončení | Představuje zprávu komunikaci úspěch nebo selhání. |

> [!NOTE]
> Ovládací prvek Průvodce uchovává informace o jejím stavu použití stavu ovládacích prvků technologie ASP.NET. Proto vlastnost EnableViewState lze nastavit na hodnotu false, bez jakékoli taky způsobit škody.


Toto video je návod, ovládacího prvku průvodce.


![](server-controls/_static/image2.png)


[Otevřete Video přes celou obrazovku](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Lokalizace ovládacího prvku

Ovládací prvek Localize je podobná prvku Literal control. Ovládací prvek Localize má však **režimu** vlastnost, která řídí vykreslení značek, která je přidána. Vlastnost režimu podporuje následující hodnoty:

| **Režim** | **Vysvětlení** |
| --- | --- |
| Transformace | Značka je transformovat podle protokolu prohlížeče, který zadal žádost. |
| Atribut PassThrough | Značka je vykreslen jako-je. |
| Kódování | Značky, který je přidán do ovládacího prvku jsou zakódovány pomocí HtmlEncode. |

## <a name="multiview-and-view-controls"></a>MultiView a ovládací prvky zobrazení

Ovládací prvek MultiView slouží jako kontejner pro ovládací prvky zobrazení, a ovládacího prvku zobrazení jako kontejner (podobně jako ovládací prvek Panel) pro další ovládací prvky. Jednotlivých zobrazení v ovládacím prvku MultiView je reprezentována jeden ovládací prvek zobrazení. První zobrazení ovládacího prvku MultiView je zobrazení 0, druhý zobrazení 1, atd. Zobrazení lze přepínat zadáním vlastnost ActiveViewIndex MultiView ovládacího prvku.

## <a name="substitution-control"></a>Náhradní ovládací prvek

Náhradní ovládací se používá ve spojení s ukládáním do mezipaměti technologie ASP.NET. V případech, kdy chcete využít výhod ukládání do mezipaměti, ale máte části stránky, který se musí aktualizovat na každý požadavek (jinými slovy, části stránky, které jsou vyloučené z ukládání do mezipaměti) poskytuje komponentu nahrazení vynikající řešení. Ovládací prvek není ve skutečnosti vykreslen žádný výstup svoje vlastní. Místo toho je vázána na metodu v kódu na straně serveru. Když je zobrazení stránky vyžadováno, je volána metoda a vrácená značka je vykreslen místo náhradní ovládací.

Metoda, ke kterému je náhradní ovládací prvek vázán je zadán prostřednictvím **MethodName** vlastnost. Tato metoda musí splňovat následující kritéria:

- Musí být metoda statické (sdílené v jazyce Visual Basic).
- Přijímá jeden parametr typu položka HttpContext.
- Vrátí řetězec představující kód, který má nahradit ovládacího prvku na stránce.

Náhradní ovládací nemá schopnost upravovat libovolný ovládací prvek na stránce, ale nemá přístup k aktuální položka HttpContext pomocí jeho parametru.

## <a name="gridview-control"></a>Rutina GridView ovládací prvek

Rutina GridView řízení je náhradou DataGrid – ovládací prvek. Tento ovládací prvek se budeme podrobněji v novější modulu.

## <a name="detailsview-control"></a>Ovládací prvek DetailsView

Ovládací prvek DetailsView můžete zobrazit jeden záznam ze zdroje dat a upravit nebo odstranit. Je o něm zmínka v podrobněji modul novější.

## <a name="formview-control"></a>Ovládací prvek FormView

Ovládací prvek FormView se používá k zobrazení jeden záznam ze zdroje dat v konfigurovat rozhraní. Je o něm zmínka v podrobněji modul novější.

## <a name="accessdatasource-control"></a>Ovládací prvek AccessDataSource

Ovládací prvek AccessDataSource se používá pro datové vazby databáze Access. Je o něm zmínka v podrobněji modul novější.

## <a name="objectdatasource-control"></a>Ovládacího prvku ObjectDataSource

Na podporu třívrstvá architektura, která ovládacích prvků může být vázané na data na střední vrstvě obchodní objekt oproti model Dvojúrovňová kde vazby ovládacích prvků přímo ke zdroji dat se používá ovládacího prvku ObjectDataSource. Bude se podrobněji popsána v novější modulu.

## <a name="xmldatasource-control"></a>Ovládacího prvku

Ovládacího prvku se používá k vazbě dat na zdroj dat XML. Je o něm zmínka v podrobněji modul novější.

## <a name="sitemapdatasource-control"></a>Ovládací prvek SiteMapDataSource

Ovládací prvek SiteMapDataSource poskytuje datové vazby pro ovládací prvky pro navigaci lokality na základě lokality mapy. Bude se podrobněji popsána v novější modulu.

## <a name="sitemappath-control"></a>Ovládací prvek SiteMapPath

Ovládací prvek SiteMapPath zobrazí řadu navigační odkazy, které jsou obvykle označuje jako cesta ke stránce. Je o něm zmínka v podrobněji modul novější.

## <a name="menu-control"></a>Menu – ovládací prvek

Dynamické nabídky pomocí DHTML zobrazí ovládací prvek nabídky. Je o něm zmínka v podrobněji modul novější.

## <a name="treeview-control"></a>TreeView – ovládací prvek

TreeView – ovládací prvek se používá k zobrazení hierarchickou stromovou strukturu data. Je o něm zmínka v podrobněji modul novější.

## <a name="login-control"></a>Ovládací prvek přihlášení

Ovládací prvek pro přihlášení poskytuje mechanismus pro přihlášení k webu. Je o něm zmínka v podrobněji modul novější.

## <a name="loginview-control"></a>Ovládací prvek LoginView

Ovládací prvek LoginView umožňuje zobrazení různých šablon na základě stavu přihlášení uživatele. Je o něm zmínka v podrobněji modul novější.

## <a name="passwordrecovery-control"></a>Ovládací prvek PasswordRecovery

Ovládací prvek PasswordRecovery slouží k načtení zapomenutým heslům uživatelé aplikací technologie ASP.NET. Je o něm zmínka v podrobněji modul novější.

## <a name="loginstatus"></a>loginStatus

Tento ovládací prvek LoginStatus zobrazí stav přihlášení uživatele. Je o něm zmínka v podrobněji modul novější.

## <a name="loginname"></a>LoginName

Ovládací prvek LoginName zobrazí uživatelské jméno uživatele po protokolována do aplikace ASP.NET. Je o něm zmínka v podrobněji modul novější.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard je konfigurovat průvodce, který poskytuje uživatelům možnost vytvoření účtu členství technologie ASP.NET pro použití v aplikaci ASP.NET. Je o něm zmínka v podrobněji modul novější.

## <a name="changepassword"></a>Změna hesla byla

Změna hesla byla řízení umožňuje uživatelům změnit své heslo pro aplikaci ASP.NET. Je o něm zmínka v podrobněji modul novější.

## <a name="various-webparts"></a>Různé webové části

ASP.NET 2.0 se dodává s různými webové části. Ty se budeme podrobně v novější modulu.
