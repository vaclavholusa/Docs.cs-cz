---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Serverové ovládací prvky | Dokumentace Microsoftu
author: microsoft
description: ASP.NET 2.0 vylepšuje serverových ovládacích prvků v mnoha způsoby. V tomto modulu probereme, některé architektury se změní způsob, jak ASP.NET 2.0 a Visual Studio 200...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: da06429f3949a47a02fccef45666d1220781e473
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837066"
---
<a name="server-controls"></a>Serverové ovládací prvky
====================
podle [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 vylepšuje serverových ovládacích prvků v mnoha způsoby. V tomto modulu probereme některé architektury změny způsobu, technologii ASP.NET 2.0 a Visual Studio 2005 se zabývá serverových ovládacích prvků.


ASP.NET 2.0 vylepšuje serverových ovládacích prvků v mnoha způsoby. V tomto modulu probereme některé architektury změny způsobu, technologii ASP.NET 2.0 a Visual Studio 2005 se zabývá serverových ovládacích prvků.

## <a name="view-state"></a>Stav zobrazení

Primární změny v zobrazení stavu v ASP.NET 2.0 je výrazné snížení velikosti. Vezměte v úvahu stránku s pouze ovládacím prvkem kalendáře na něj. Zde je zobrazení stavu v ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Teď tady je stav zobrazení na stejné stránce v technologii ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

To je poměrně významnou změnu, a vzhledem k tomu, že je stav zobrazení vzájemně přenesou přenosu, tato změna může dávají vývojářům zvýšení výkonu. Snížení velikosti stav zobrazení je z velké části tak, jak nakládáme interně. Mějte na paměti, že řetězec s kódováním zobrazení stavu je s kódováním Base64. Abyste lépe pochopili změnu v zobrazení stavu v ASP.NET 2.0, dopřejeme si prohlédněte dekódovaný hodnoty z výše uvedených příkladech.

Tady je stav 1.1 zobrazení dekódovat:

[!code-css[Main](server-controls/samples/sample3.css)]

To může vypadat trochu nesrozumitelné, ale zde neexistuje vzor tady. V technologii ASP.NET 1.x, jsme jednotlivé znaky slouží k identifikaci datových typů a oddělených hodnot pomocí &lt; &gt; znaků. "T" v příkladu stavu zobrazení výše představuje trojici. Trojici obsahuje dvojice kolekce ArrayLists ("l" představuje třídu ArrayList.) Jedna z těchto kolekce ArrayLists obsahuje typ Int32 ("i") s hodnotou 1 a druhý obsahuje jiné trojici. Trojici obsahuje pár instance třídy ArrayList, atd. Důležité pamatovat je, že používáme trojice, které obsahují páry, uvedeme typy dat prostřednictvím písmenem a používáme &lt; a &gt; znaků jako oddělovače.

V technologii ASP.NET 2.0 vypadá trochu jiná dekódovaný zobrazení stavu.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Měli byste zaznamenat velkou změnu vzhledu dekódovaný zobrazení stavu. Tato změna má několik podchycení architektury. Zobrazení stavu v ASP.NET 1.x používaný k serializaci dat LosFormatter. Ve verzi 2.0 používáme novou třídu ObjectStateFormatter. Tato třída je navržená speciálně pro serializaci a deserializaci zobrazit stav a stav ovládacího prvku. (Stav ovládacího prvku se budeme v další části.) Existuje mnoho výhod získali změnou metody, pomocí kterého serializace a deserializace proběhnout. Jednou z nejvíce výrazné je skutečnost, že na rozdíl od LosFormatter, který používá Textwriteru, používá ObjectStateFormatter BinaryWriter. To umožňuje technologii ASP.NET 2.0 k uložení stavu zobrazení řady bajtů namísto řetězce. Vezměme si jako příklad celé číslo. V technologii ASP.NET 1.1 vyžaduje celé číslo 4 bajty stavu zobrazení. V technologii ASP.NET 2.0 že stejné celé číslo vyžaduje pouze 1 bajt. Další vylepšení byly provedeny ke snížení množství stav zobrazení, která je uložena. Hodnoty data a času, například jsou nyní uloženy pomocí TickCount namísto řetězce.

Jako by všechny, které nebyly dostatečně, byla skutečnost, že byl jeden ze největší příjemci stav zobrazení v 1.x DataGrid a podobně jako ovládací prvky i zvláštní pozornost. Hlavní nevýhodou ovládacích prvků, například DataGrid, kde se týká stavu zobrazení je, že často obsahuje velké množství opakující se informace. V technologii ASP.NET 1.x, který opakuje informace uložil jednoduše nad a nad znovu výsledkem opakovaném zobrazení stavu. V technologii ASP.NET 2.0 používáme k ukládání těchto dat novou třídu IndexedString. Pokud se řetězec opakovat, uložíme právě token IndexedString a index v tabulce spuštěných objektů IndexedString.

## <a name="control-state"></a>Stav ovládacího prvku

Jednou z hlavních gripes, které měly vývojářů stav zobrazení bylo velikost, která se přidá do datové části HTTP. Jak už jsme zmínili, je jedním z nejlepší spotřebitelů stav zobrazení ovládacího prvku DataGrid. Aby se zabránilo obrovské množství generované třídy DataGrid stav zobrazení, celá řada vývojářů jednoduše zakázaná zobrazení stavu pro tento ovládací prvek. Toto řešení se bohužel vždy problém. Zobrazení stavu v ASP.NET 1.x obsahuje nejen data nezbytná pro správné fungování ovládacího prvku. Také obsahuje informace týkající se stavu ovládacího prvku uživatelského rozhraní. To znamená, že pokud chcete povolit stránkování v prvku DataGrid, je nutné povolit stav zobrazení, i v případě, že nepotřebujete všechny informace uživatelského rozhraní, které zobrazit stav obsahuje. Jde o rigidní scénář.

Stav ovládacího prvku v technologii ASP.NET 2.0, řeší tento problém krásně prostřednictvím Úvod stav ovládacího prvku. Stav ovládacího prvku obsahuje data, která je nezbytně nutné pro správné funkce ovládacího prvku. Na rozdíl od zobrazení stavu nelze zakázat stav ovládacího prvku. Proto je důležité, že data jsou uložená v stav ovládacího prvku pečlivě kontrolován.

> [!NOTE]
> Stav ovládacího prvku se ukládají společně s stav zobrazení v \_ \_stav zobrazení skrytého pole formuláře.


Toto video je návod, zobrazení stavu a stav ovládacího prvku.


![](server-controls/_static/image1.png)


[Otevřít Video na celou obrazovku](server-controls/_static/state1.wmv)


V pořadí pro serverový ovládací prvek pro čtení a zápis k řízení stavu je třeba provést tři kroky.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Krok 1: Volání metody RegisterRequiresControlState

Metoda RegisterRequiresControlState informuje technologie ASP.NET, ovládací prvek musí zachovat stav ovládacího prvku. Trvá jeden argument typu ovládacího prvku, který je ovládací prvek, který se registruje.

Je důležité si uvědomit, registrace nezůstanou zachována žádosti z požadavku. Proto tato metoda musí být volána u každého požadavku, pokud je ovládací prvek se zachovat stav ovládacího prvku. Doporučuje se, že v funkce OnInit volat metodu.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Krok 2: Přepsání SaveControlState

Metoda SaveControlState uloží ovládací prvek změny stavu pro ovládací prvek od posledního postbacku. Vrátí objekt představující stav ovládacího prvku.

## <a name="step-3-override-loadcontrolstate"></a>Krok 3: Přepsání LoadControlState

Metoda LoadControlState načte uložený stav do ovládacího prvku. Tato metoda přebírá jeden argument typu objekt, který obsahuje uložený stav ovládacího prvku.

## <a name="full-xhtml-compliance"></a>Dodržování předpisů úplné XHTML

Každý vývojář webové ví důležitost standardy ve webových aplikacích. Zachování založené na standardech vývojové prostředí ASP.NET 2.0 je plně kompatibilní XHTML. Proto všechny značky jsou vykreslené podle standardů XHTML v prohlížečích podporujících formát HTML 4.0 nebo vyšší.

Definice DOCTYPE v technologii ASP.NET 1.1 se následujícím způsobem:

[!code-html[Main](server-controls/samples/sample6.html)]

V technologii ASP.NET 2.0 výchozí definici DOCTYPE vypadá takto:

[!code-html[Main](server-controls/samples/sample7.html)]

Pokud se rozhodnete, můžete změnit výchozí předpisů XHML prostřednictvím xhtmlConformance uzlu v konfiguračním souboru. Například následující uzel v souboru web.config se změní XHTML dodržování předpisů na XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Pokud se rozhodnete, můžete také nakonfigurovat ASP.NET použít starší verzi konfigurace v technologii ASP.NET použita 1.x následujícím způsobem:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Adaptivní vykreslování pomocí adaptérů

V technologii ASP.NET 1.x, konfigurační soubor obsažených &lt;browserCaps&gt; části, která naplní objekt HttpBrowserCapabilities. Tento objekt povolené vývojář, abyste mohli zjistit, jaké zařízení konkrétního požadavku a vykreslení kódu odpovídajícím způsobem. V technologii ASP.NET 2.0 modelu vylepšeno a nyní používá novou třídu ControlAdapter. Třída ControlAdapter přepíše událostí do ovládacího prvku životního cyklu a řídí vykreslování ovládacích prvků na základě možnosti uživatelského agenta. Soubor definice prohlížeče (soubor s příponou souboru Browser) uložených v c:\windows\microsoft.net\framework\v2.0 jsou definovány možnosti konkrétního uživatelského agenta. \* \* \* \*\CONFIG\Browsers složky.

> [!NOTE]
> Třída ControlAdapter je abstraktní třída.


Podobně jako &lt;browserCaps&gt; kapitoly 1.x souboru s definicí prohlížeče pomocí regulárního výrazu pro analýzu řetězce agenta uživatele za účelem zjištění požadujícího prohlížeče. To je pro tento uživatelský agent definuje konkrétní funkce. ControlAdapter vykreslí ovládací prvek prostřednictvím metody vykreslení. Proto pokud přepíšete metodu vykreslování, neměli by jste volat vykreslení v základní třídě. To může způsobit vykreslování na výskyt dvakrát, jednou pro adaptér a jednou pro samotný ovládací prvek.

## <a name="developing-a-custom-adapter"></a>Vývoj vlastního adaptéru

Dědění z ControlAdapter můžete vyvíjet vlastní adaptér. Kromě toho může dědit z abstraktní třídy PageAdapter v případech, kde je potřeba adaptér pro stránku. Mapování ovládacích prvků do vlastního adaptéru se provádí prostřednictvím &lt;controlAdapters&gt; prvku v souboru s definicí prohlížeče. Například následující kód XML z definičního souboru prohlížeče mapuje ovládací prvek nabídky MenuAdapter třídy:

[!code-html[Main](server-controls/samples/sample10.html)]

Pomocí tohoto modelu, stane se poměrně snadné řízení vývojář cílit na konkrétní zařízení nebo prohlížeče. Je to také úplně jednoduché vývojář má úplnou kontrolu nad způsob vykreslení stránky na všech zařízeních.

## <a name="per-device-rendering"></a>Vykreslování podle zařízení

Vlastnosti ovládacích prvků serveru v technologii ASP.NET 2.0 může být zadané zařízení pomocí předpony specifické pro prohlížeč. Například následující kód změní Text popisku, v závislosti na zařízení se používá k procházení stránky.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Při zobrazení stránky, které obsahují tento popisek v prohlížeči z aplikace Internet Explorer, popisek se zobrazí text říká "Přecházíte z aplikace Internet Explorer." Při zobrazení stránky v prohlížeči z Firefox, popisek se zobrazí text "Přecházíte z Firefox." Při zobrazení stránky v prohlížeči z jakéhokoli zařízení, se zobrazí "Přecházíte z neznámého zařízení." Nějaká vlastnost se dá nastavit pomocí této speciální syntaxe.

## <a name="setting-focus"></a>Nastavení fokusu

Vývoj v ASP.NET 1.x najdete výběr častých o tom, jak nastavit počáteční fokus na určitý ovládací prvek. Například na přihlašovací stránku, je užitečné mít textového pole ID uživatele získat fokus při prvním načtení stránky. V technologii ASP.NET 1.x to povinné, některé skriptu na straně klienta. I když takového skriptu je jednoduchá, není již nezbytné v technologii ASP.NET 2.0 díky metody SetFocus. Metoda SetFocus přijímá jeden argument určující ovládací prvek, který by měl být vybrán. Tento argument může být buď ID klienta ovládacího prvku jako řetězec nebo název serverového ovládacího prvku jako objekt ovládacího prvku. Například pokud chcete nastavit počáteční fokus na ovládací prvek TextBox txtUserID volá se při prvním načtení stránky, přidejte následující kód na stránku\_zatížení:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--nebo

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 používá k vykreslení na straně klienta funkce, která nastaví fokus obslužná rutina Webresource.axd (jak jsme vysvětlili výše). Název funkce na straně klienta je webový formulář\_AutoFocus, jak je znázorněno zde:

[!code-html[Main](server-controls/samples/sample14.html)]

Alternativně můžete použít metodu fokus pro ovládací prvek nastavit počáteční fokus na ovládací prvek. Metoda fokus je odvozena ze třídy Control a je k dispozici pro všechny ovládací prvky technologie ASP.NET 2.0. Je také možné nastavit fokus na určitý ovládací prvek, když dojde k chybě ověřování. Který se budeme v novějším modulu.

## <a name="new-server-controls-in-aspnet-20"></a>Nové serverové ovládací prvky v technologii ASP.NET 2.0

Toto jsou nové serverové ovládací prvky v technologii ASP.NET 2.0. Přejdete do více podrobností na některé z nich v pozdější moduly.

## <a name="imagemap-control"></a>Obrazová mapa ovládacího prvku

Ovládací prvek obrazová mapa umožňuje přidat hotspotům bitovou kopii, která může iniciovat příspěvek zpět nebo přejděte na adresu URL. Existují tři typy vzniku hotspotů k dispozici. Kruhový aktivní bod RectangleHotSpot a PolygonHotSpot. Aktivní oblasti budou přidány prostřednictvím editor kolekce v sadě Visual Studio nebo programově v kódu. Není k dispozici pro kreslení hotspotů na imagi bez uživatelského rozhraní. Souřadnice a velikost nebo radius aktivního bodu je třeba zadat deklarativně. Neexistuje žádný vizuální znázornění aktivního bodu v návrháři. Pokud aktivní bod je nakonfigurován pro navigaci na adresu URL, adresa URL je zadáno pomocí vlastnosti NavigateUrl aktivního bodu. V případě příspěvek jenom aktivní, PostBackValue vlastnost umožňuje předat řetězec v příspěvku zpět, který se dá načíst v kódu na straně serveru.


![Editor kolekce aktivních bodů v sadě Visual Studio](server-controls/_static/image1.jpg)

**Obrázek 1**: Editor kolekce aktivních bodů v sadě Visual Studio


## <a name="bulletedlist-control"></a>Ovládací prvek BulletedList

Ovládací prvek BulletedList je seznam s odrážkami můžete snadno představovat data vázaná. V seznamu lze provést řazení (číslovaný) nebo prostřednictvím vlastnosti BulletStyle Neseřazený. Každá položka v seznamu je reprezentován objektem ListItem.


![Ovládacího prvku BulletedList v sadě Visual Studio](server-controls/_static/image1.gif)

**Obrázek 2**: ovládacího prvku BulletedList v sadě Visual Studio


## <a name="hiddenfield-control"></a>Ovládací prvek HiddenField

Ovládací prvek HiddenField přidá skrytého pole na stránku, jehož hodnota je k dispozici v kódu na straně serveru. Hodnota skrytého pole obecně očekává se nezměnily mezi voláními příspěvek. Je však možné změnit hodnotu před publikovat zpět uživateli se zlými úmysly. V takovém případě bude ovládací prvek HiddenField vyvolat událost ValueChanged. Pokud máte v ovládacím prvku HiddenField citlivé informace a chcete mít jistotu, že zůstane beze změny, můžete zpracovávat události ValueChanged ve vašem kódu.

## <a name="fileupload-control"></a>Ovládací prvek fileUpload

Ovládací prvek FileUpload v technologii ASP.NET 2.0 umožňuje nahrát soubory na webový server prostřednictvím stránky ASP.NET. Tento ovládací prvek je podobný třídě HtmlInputFile 1.x ASP.NET s několika výjimkami. V technologii ASP.NET 1.x, se doporučuje, aby bylo možné zjistit, pokud jste měli soubor dobré vlastnost PostedFile zkontrolovat hodnotu null. Ovládací prvek FileUpload v technologii ASP.NET 2.0 přidá novou vlastnost HasFile, můžete použít ke stejnému účelu, a je o něco mnohem efektivnější.

Vlastnost PostedFile je stále k dispozici pro přístup k objektu HttpPostedFile, ale některé funkce HttpPostedFile je nyní k dispozici vnitřně s ovládacím prvkem FileUpload. Například pro ukládání nahraného souboru v technologii ASP.NET 1.x, můžete volat metoda SaveAs pro objekt HttpPostedFile. Použití ovládacího prvku FileUpload v technologii ASP.NET 2.0, by volat metoda SaveAs na samotný ovládací prvek FileUpload.

Další významné změny v chování 2.0 (a pravděpodobně nejdůležitější změny) je, že není již nutné načíst celý soubor nahrané do paměti před uložením. V 1.x, odeslaný soubor uloží jenom do paměti před zápisem na disk. Tato architektura brání nahrávání velkých souborů.

Atribut requestLengthDiskThreshold elementu httpRuntime v technologii ASP.NET 2.0, umožňuje nakonfigurovat, kolik kilobajtů jsou uloženy ve vyrovnávací paměti v paměti před zápisem na disk.

**Důležité**: dokumentace MSDN (a dokumentace jinde) určuje, že tato hodnota je v bajtech (nikoli v kilobajtech) a že výchozí hodnota je 256. Hodnota je ve skutečnosti určena v kilobajtech a výchozí hodnota 80. Tím, že výchozí hodnota 80 kB, zajišťujeme, že vyrovnávací paměť nemá na konci na velkých objektových haldách.

## <a name="wizard-control"></a>Ovládací prvek Průvodce

Je celkem běžné dojde jenom se pokus o získání informací v řadě výraz "stránky" pomocí panelů nebo a současný přechod ze stránky na stránku vývojáře využívající technologii ASP.NET. Častěji omezené úsilí je frustrující a je časově náročné. Nový ovládací prvek Průvodce řeší problémy tím, že pro lineární a nelineárních kroky v Průvodci rozhraní, které uživatelé znají. Ovládací prvek Průvodce představuje vstupní formuláře v sérii kroků. Každý krok je určitého typu určeném vlastností StepType ovládacího prvku. Typy krok k dispozici jsou následující:

| **Typ kroku** | **Vysvětlení** |
| --- | --- |
| Auto | Průvodce automaticky určí typ kroku na základě jeho pozice v rámci hierarchie kroku. |
| Spustit | Prvním krokem, často používají k úvodní příkazu. |
| Krok | Normální kroku. |
| Dokončit | Posledním krokem obvykle používají k zobrazení tlačítko a dokončete průvodce. |
| Dokončení | Představuje zprávu komunikaci úspěch nebo neúspěch. |

> [!NOTE]
> Ovládací prvek Průvodce uchovává informace o stavu pomocí stav ovládacích prvků technologie ASP.NET. Proto vlastnost EnableViewState lze nastavit na hodnotu false, bez jakékoli způsobit škody.


Toto video je návod ovládacího prvku Wizard.


![](server-controls/_static/image2.png)


[Otevřít Video na celou obrazovku](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Lokalizace ovládacího prvku

Ovládací prvek Localize je podobný prvku Literal control. Má však Localize ovládacího prvku **režimu** vlastnost, která určuje, jak je vykreslen kód, který se přidá do ní. Vlastnost Mode podporuje následující hodnoty:

| **Režim** | **Vysvětlení** |
| --- | --- |
| Transformace | Značek je transformovány podle protokolu prohlížeče, který zadal žádost. |
| Průchod | Značka se vykreslí jako-je. |
| Kódování | Kód, který se přidá do ovládacího prvku je zakódován pomocí HtmlEncode. |

## <a name="multiview-and-view-controls"></a>MultiView a ovládací prvky zobrazení

Ovládací prvek MultiView funguje jako kontejner pro ovládací prvky zobrazení a ovládací prvek zobrazení funguje jako kontejner pro ostatní ovládací prvky (podobně jako ovládací prvek Panel). Každé zobrazení v ovládacím prvku MultiView představuje jeden ovládací prvek zobrazení. První zobrazení ovládacího prvku MultiView je zobrazení 0, zobrazení 1, druhý atd. Zobrazení můžete přepínat zadáním vlastnost ActiveViewIndex ovládacího prvku MultiView.

## <a name="substitution-control"></a>Náhradní ovládací prvek

Náhradní ovládací prvek se používá ve spojení s ukládáním do mezipaměti technologie ASP.NET. V případech, kdy budete chtít využít výhod ukládání do mezipaměti, ale že obsahuje části stránky, který se musí aktualizovat na každý požadavek (jinými slovy, části stránky, které jsou vyloučené z ukládání do mezipaměti) komponenta nahrazení poskytuje vynikající řešení. Ovládací prvek nevykreslí skutečně žádný výstup sama o sobě. Místo toho je vázán na metodu v kódu na straně serveru. Když je zobrazení stránky vyžadováno, je volána metoda a vrácená značka je vykreslen místo nahrazení ovládacího prvku.

Metoda, ke kterému je vázán náhradní ovládací prvek je zadáno pomocí **MethodName** vlastnost. Tato metoda musí splňovat následující kritéria:

- Musí být statické (sdílené v jazyce Visual Basic) metody.
- Přijímá jeden parametr typu objektu HttpContext.
- Vrátí řetězec představující kód, který by měly nahradit ovládací prvek na stránce.

Náhradní ovládací prvek nemá schopnost upravovat libovolný ovládací prvek na stránce, ale nemá přístup k aktuální položka HttpContext prostřednictvím jeho parametru.

## <a name="gridview-control"></a>Ovládací prvek GridView

Ovládací prvek GridView je náhradou za ovládacího prvku DataGrid. Tento ovládací prvek se budeme podrobněji v novějším modulu.

## <a name="detailsview-control"></a>Ovládací prvek DetailsView

Ovládací prvek DetailsView můžete zobrazit jeden záznam ze zdroje dat a upravit nebo odstranit. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="formview-control"></a>Ovládacího prvku FormView

Ovládacího prvku FormView slouží k zobrazení jeden záznam ze zdroje dat v konfigurovatelné rozhraní. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="accessdatasource-control"></a>Ovládací prvek AccessDataSource

Ovládací prvek AccessDataSource se používá k vytvořit datovou vazbu s: databázi aplikace Access. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="objectdatasource-control"></a>Ovládací prvek ObjectDataSource

Ovládací prvek ObjectDataSource se používá pro podporu třívrstvé architektuře tak, aby ovládacích prvků může být vázaný na data do střední vrstvy obchodní objekt, na rozdíl od dvouvrstvém modelu kde vazby ovládacích prvků přímo ke zdroji dat. Probereme podrobněji v novějším modulu.

## <a name="xmldatasource-control"></a>Ovládací prvek XmlDataSource

Ovládací prvek XmlDataSource je použitého k vazbě dat na zdroj dat XML. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="sitemapdatasource-control"></a>Ovládací prvek SiteMapDataSource

Ovládací prvek SiteMapDataSource poskytuje datové vazby pro ovládací prvky navigace lokality na základě mapy webu. Probereme podrobněji v novějším modulu.

## <a name="sitemappath-control"></a>Ovládací prvky SiteMapPath ovládacího prvku

Ovládací prvky SiteMapPath ovládací prvek se zobrazuje řada navigačních odkazů se obvykle označuje jako s popisem cesty. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="menu-control"></a>Menu – ovládací prvek

Ovládací prvek nabídky zobrazí dynamické nabídky pomocí DHTML. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="treeview-control"></a>TreeView – ovládací prvek

TreeView – ovládací prvek slouží k zobrazení dat hierarchickém stromovém zobrazení. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="login-control"></a>Ovládací prvek Login

Ovládací prvek Login poskytuje mechanismus pro přihlašování do webové stránky. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="loginview-control"></a>Ovládací prvek zobrazení přihlášení

Ovládacího prvku LoginView umožňuje zobrazit různé šablony služby na základě stavu přihlášení uživatele. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="passwordrecovery-control"></a>Ovládací prvek PasswordRecovery

Ovládací prvek PasswordRecovery slouží k načtení zapomenuté heslo. Uživatelé aplikace technologie ASP.NET. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="loginstatus"></a>Stavu přihlášení

Ovládací prvek stavu přihlášení zobrazí stav přihlášení uživatele. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="loginname"></a>LoginName

Prvek přihlašovacího jména zobrazí uživatelské jméno uživatele po někdo přihlašuje aplikace technologie ASP.NET. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard se dají konfigurovat průvodce, který dává uživatelům možnost vytvořit účet členství technologie ASP.NET pro použití v aplikaci technologie ASP.NET. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="changepassword"></a>Metodu ChangePassword

Ovládací prvek ChangePassword umožňuje uživatelům změnit si heslo aplikace technologie ASP.NET. To se budeme věnovat jednotlivě podrobněji v novějším modulu.

## <a name="various-webparts"></a>Různé webové části

Dodává se s různými částmi webové technologie ASP.NET 2.0. Ty se budeme podrobně v novějším modulu.
