---
title: "Brání skriptování mezi servery"
author: rick-anderson
description: "Toto téma představuje webů Skriptování a techniky pro vyřešení této chyby zabezpečení v aplikaci ASP.NET Core."
keywords: "ASP.NET Core, XSS, ohrožení zabezpečení"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 95790927-2bfe-445e-b1fd-429c2c7030ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: fdb26a8338b98135cfc3f6bce9d87285e9a7eb12
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="preventing-cross-site-scripting"></a>Brání skriptování mezi servery

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Webů Skriptování je ohrožení zabezpečení, která umožňuje útočníkovi uložit skripty na straně klienta (obvykle JavaScript) do webové stránky. Když ostatní uživatelé načíst příslušných stránek, které budou spuštěny skripty útočníci, by ji ke krádeži soubory cookie a relace tokeny, změnit obsah webové stránky prostřednictvím DOM manipulaci nebo přesměrovat na jinou stránku v prohlížeči. Skriptování XSS obecně nastane, když aplikace přijímá vstup uživatele a uloží jej na stránce bez ověřování, kódování nebo ho uvozovací znaky.

## <a name="protecting-your-application-against-xss"></a>Ochrana proti XSS vaší aplikace

Na základní úrovni XSS funguje tak, přičemž aplikace do vkládání `<script>` značka do vykreslované stránky, nebo vložením `On*` událostí do elementu. Vývojáři by měl použít následující kroky prevence aby nedošlo k zavedení XSS do své aplikace.

1. Nikdy uvést nedůvěryhodné data do váš vstup, HTML, pokud postupujte podle zbývajících kroků. Nedůvěryhodné data jsou všechna data, která může být řízené útočník vstupy formuláře HTML, řetězce dotazu, hlaviček protokolu HTTP, i data source z databáze, protože útočník může být schopný porušení vaší databáze, i když jejich nelze porušení vaší aplikace.

2. Před přepnutím nedůvěryhodné data uvnitř elementu HTML ověřte, zda že se kódovaný jazykem HTML. Kódování HTML trvá znaky, jako &lt; a změní jejich do bezpečného formuláře jako &amp;lt;

3. Před uvedením nedůvěryhodné data do atribut HTML ověřte, zda že se kódování atributů HTML. Kódování atributu HTML je nadmnožinou kódování HTML a kóduje dalšími znaky, jako "a".

4. Před přepnutím nedůvěryhodné data do jazyka JavaScript umístíte data v elementu HTML, jehož obsah je načíst za běhu. Pokud to není možné zkontrolujte data je zakódován JavaScript. Kódování JavaScript trvá nebezpečné znaky pro jazyk JavaScript a nahradí je jejich šestnáctkově, například &lt; by kódovaná jako `\u003C`.

5. Před uvedením nedůvěryhodné data do řetězce dotazu adresy URL Ujistěte se, že je kódovaná adresou URL.

## <a name="html-encoding-using-razor"></a>Kódování HTML pomocí syntaxe Razor

Použít v MVC automaticky modul Razor zakóduje všechny výstup jako zdroj proměnné, pokud pracujete skutečně pevného zabránit, aby ji tak. Ji používá atribut HTML kódování pravidla při každém použití  *@*  – direktiva. Ve formátu HTML kódování atributu je nadmnožinou kódování HTML, znamená to, že nemusíte sami se týkají s jestli byste měli používat kódování HTML nebo kódování atributu HTML. Je nutné zajistit, že používáte pouze v kontextu HTML při pokusu o vložení nedůvěryhodné vstup přímo do jazyka JavaScript. Pomocníci značka bude také zakódovat vstup, které můžete použít v parametrech značky.

Proveďte následující zobrazení syntaxe Razor;

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Tato zobrazení výstupu obsah *untrustedInput* proměnné. Tato proměnná zahrnuje některé znaky, které se používají v útoky XSS, konkrétně &lt;, "a &gt;. Zkoumání zdroj zobrazuje vykreslené výstup kódovaná jako:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET MVC základní poskytuje `HtmlString` třídy, který není kódován automaticky při výstupu. To by nikdy použít v kombinaci s nedůvěryhodné vstup jako to zveřejní XSS ohrožení zabezpečení.

## <a name="javascript-encoding-using-razor"></a>Kódování JavaScript pomocí syntaxe Razor

Může nastat situace, který chcete vložit hodnotu do jazyka JavaScript ke zpracování v zobrazení. Chcete-li to provést dvěma způsoby. Nejbezpečnější způsob, jak vložit jednoduchý hodnoty je hodnota umístit do atribut dat značky a načíst ve vašem JavaScript. Příklad:

```none
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

To způsobí následující HTML

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

Které, když je spuštěna, vykreslí takto:

```none
<"123">
   <"123">
   ```

Můžete také zavolat kodér JavaScript přímo,

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

To bude vykreslení v prohlížeči takto:

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> Řetězení není nedůvěryhodné vstup v jazyce JavaScript k vytvoření DOM elementů. Měli byste použít `createElement()` a přiřadit odpovídajícím způsobem, jako hodnoty vlastností `node.TextContent=`, nebo použijte `element.SetAttribute()` / `element[attribute]=` jinak sami umístěte do založené na modelu DOM XSS.

## <a name="accessing-encoders-in-code"></a>Přístup k kodéry v kódu

Jsou k dispozici kódu dvěma způsoby kodéry HTML, JavaScript a adresu URL, můžete vložit pomocí [vkládání závislostí](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) nebo můžete použít výchozí kodéry, obsažené v `System.Text.Encodings.Web` oboru názvů. Pokud použijete výchozí kodéry pak veškeré použité na rozsahy znak považován za bezpečné se neprojeví, – výchozí kodéry nejbezpečnější kódování pravidla možné použít.

Použít konfigurovatelná kodéry prostřednictvím DI vaší konstruktory provést *HtmlEncoder*, *JavaScriptEncoder* a *UrlEncoder* parametr podle potřeby. Například;

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a>Kódování URL parametry

Pokud chcete vytvořit adresu URL řetězec dotazu s nedůvěryhodné vstup jako hodnotu pomocí `UrlEncoder` ke kódování hodnota. Například

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

Po kódování encodedValue bude obsahovat proměnné `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Mezery, uvozovky, interpunkce a další nebezpečné znaky bude procenta kódovaný k jejich šestnáctkové hodnoty, například znak mezery bude % 20.

>[!WARNING]
> Nepoužívejte nedůvěryhodné vstup jako část cesty adresy URL. Vždycky předáte nedůvěryhodné vstup jako hodnotu řetězce dotazu.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Přizpůsobení kodéry

Ve výchozím nastavení kodéry pomocí seznamu bezpečných adres nesmí být v rozsahu základní Latinské kódování Unicode a kódování všechny znaky mimo tento rozsah jako jejich ekvivalenty u kódu znaku. Toto chování také ovlivní Razor TagHelper a HtmlHelper vykreslování, jak se bude používat kodéry pro výstup vaší řetězce.

Zdůvodnění to je pro ochranu proti chyby neznámý nebo budoucí prohlížeče (předchozí chyby prohlížeče mít přerušovačů až analýza podle zpracování neanglických znaků). Pokud váš web z umístění provede výrazně využívá jiné znaky než latinku, jako je například čínština, cyrilici nebo jiné toto není pravděpodobně chování, které chcete.

Můžete přizpůsobit bezpečné seznamy kodér zahrnout rozsahy vhodné pro vaši aplikaci při spuštění, v kódu Unicode `ConfigureServices()`.

Například pomocí výchozí konfigurace můžete použít Razor HtmlHelper takto;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Při zobrazení zdroji webové stránky uvidíte, že je vykreslena následujícím způsobem s čínštině kódovaný;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Rozšíří znaky považované za bezpečné pomocí kodéru by vložení následující řádek do `ConfigureServices()` metoda v `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

Tento příklad rozšiřuje seznamu bezpečných zahrnout CjkUnifiedIdeographs rozsah kódování Unicode. Nyní by se stala Vykreslený výstup

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Seznamu bezpečných adres rozsahy jsou zadané jako grafy kódu Unicode, ne jazyky. [Standardu Unicode](http://unicode.org/) obsahuje seznam [code grafy](http://www.unicode.org/charts/index.html) můžete použít k vyhledání grafu obsahující vaše znaky. Každý kodér, Html, JavaScript a adresu Url, musí být nakonfigurované samostatně.

> [!NOTE]
> Přizpůsobení seznamu bezpečných adres ovlivňuje pouze kodéry Source prostřednictvím DI. Pokud je přímý přístup k kodér prostřednictvím `System.Text.Encodings.Web.*Encoder.Default` pak výchozí základní Latinské se použije pouze safelist.

## <a name="where-should-encoding-take-place"></a>Kam by měl být kódování provést?

Obecné přijme, postup je, že kódování probíhá v místě výstup a kódovaného hodnoty by měly být nikdy uložené v databázi. Kódování v místě výstup umožňuje změnit použití dat, například z HTML na hodnotu řetězce dotazu. Také umožňuje snadno hledání dat bez nutnosti ke kódování hodnoty před vyhledáváním a umožňuje využít výhod provedené změny a opravy chyb provedené kodérů.

## <a name="validation-as-an-xss-prevention-technique"></a>Ověření jako zabránění techniku XSS

Ověření může být užitečné nástroje omezení útoky XSS. Například jednoduché číselných řetězců obsahující pouze znaky 0-9 nespustí XSS útoku. Ověření se stane složitější měli, která chcete přijímat HTML vstup uživatele - analýza elementu input kódu HTML je obtížné, pokud není možné. Markdownu a dalších formátů text může být bezpečnější možnost pro bohaté vstup. Se nikdy spoléhají na ověření samostatně. Vždy kódování nedůvěryhodné vstup před výstup, bez ohledu na to, jaké ověření jste provedli.
