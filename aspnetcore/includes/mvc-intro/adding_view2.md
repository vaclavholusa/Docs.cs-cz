Nahraďte obsah *Views/HelloWorld/Index.cshtml* zobrazení souboru nástroje Razor následujícím kódem:

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

Přejděte na `http://localhost:xxxx/HelloWorld`. `Index` Metoda v `HelloWorldController` nebyla dělat mnohem; byl spuštěn příkaz `return View();`, které určené, že metoda by měla používat soubor šablony zobrazení k vykreslení odpovědi do prohlížeče. Vzhledem k tomu, že nebyla výslovně zadat název souboru šablony zobrazení, uvedena MVC pomocí *Index.cshtml* zobrazení souboru v */zobrazení/HelloWorld* složky. Následující obrázek ukazuje text "Hello z našich zobrazit šablonu!" pevně v zobrazení.

![Okno prohlížeče](../../tutorials/first-mvc-app/adding-view/_static/hell_template.png)

Pokud okno prohlížeče je malá (například na mobilní zařízení), možná budete muset přepnutí (tap) [Bootstrap navigační tlačítko](http://getbootstrap.com/components/#navbar) v pravé horní zobrazíte **Domů**, **o**, a **kontaktujte** odkazy.

![Okno prohlížeče zvýraznění Bootstrap navigační tlačítko](../../tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>Změna zobrazení a rozložení stránky

Klepněte na nabídku odkazy (**MvcMovie**, **Domů**, **o**). Každé stránce, zobrazí se stejné rozvržení nabídky. Nabídka rozložení je implementována ve *Views/Shared/_Layout.cshtml* souboru. Otevřete *Views/Shared/_Layout.cshtml* souboru.

[Rozložení](xref:mvc/views/layout) šablony umožňují zadat rozložení kontejneru HTML vašeho webu na jednom místě a následně použít na více stránkách ve vaší lokalitě. Najít `@RenderBody()` řádku. `RenderBody`je zástupný symbol kde všechny na zobrazení konkrétní stránky, můžete vytvořit zobrazení, *zabalené* na stránce rozložení. Například, pokud jste vybrali **o** odkaz, **Views/Home/About.cshtml** zobrazení vykresleno uvnitř `RenderBody` metoda.

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>Změnit název a nabídky odkaz v souboru rozložení

Změňte obsah elementu název. Změňte text ukotvení v šabloně rozložení "Filmová aplikace" a kontroler, z `Home` k `Movies` jako zvýrazněná níže:

Poznámka: Na technologii ASP.NET 2.0 základní verzi se mírně liší. Neobsahuje `@inject ApplicationInsights` a `@Html.Raw(JavaScriptSnippet.FullScript)`.

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

>[!WARNING]
> Implementovali jsme nebyly `Movies` ještě řadiče, takže když kliknete na tento odkaz, získáte chybu 404 (nebyl nalezen).

Uložte změny a klepněte **o** odkaz. Všimněte si, jak teď zobrazuje název na kartě prohlížeče **o - filmová aplikace** místo **o - Mvc film**. Klepněte **kontaktujte** propojení a Všimněte si, že také zobrazuje **filmová aplikace**. Jsme byli schopni jednou proveďte změny v šabloně rozložení a mít všechny stránky webu odrážet nový text odkazu a nový název.

Zkontrolujte *Views/_ViewStart.cshtml* souboru:


```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart.cshtml* souboru přináší *Views/Shared/_Layout.cshtml* soubor do jednotlivých zobrazení. Můžete použít `Layout` vlastnost nastavit různé rozložení zobrazení, nebo ji nastavte na `null` , použije se žádný soubor rozložení.

Změňte název `Index` zobrazení.

Otevřete *Views/HelloWorld/Index.cshtml*. Existují dvě místa změnit:

   * Text, který se zobrazí v názvu prohlížeče.
   * Sekundární hlavičky (`<h2>` element).

Budete je provedete mírně lišit, abyste viděli, které bit kódu změní kterou částí aplikace.


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

`ViewData["Title"] = "Movie List";`v kódu výše nastaví `Title` vlastnost `ViewData` slovník "Film seznamu". `Title` Vlastnost se používá v `<title>` prvek HTML na stránce rozložení:


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Uložte změny a přejděte do `http://localhost:xxxx/HelloWorld`. Všimněte si, že došlo ke změně záhlaví prohlížeče, záhlaví primární a sekundární záhlaví. (Pokud nevidíte změny v prohlížeči, může být zobrazil obsah uložený v mezipaměti. Stisknutím kláves Ctrl + F5 v prohlížeči na vynucení odpovědi ze serveru načíst.) Záhlaví prohlížeče je vytvořena s `ViewData["Title"]` nastaví *Index.cshtml* zobrazit šablony a další "-filmová aplikace" přidat v souboru rozložení.

Všimněte si také jak obsah *Index.cshtml* zobrazit šablonu se sloučil s *Views/Shared/_Layout.cshtml* zobrazit šablonu a odpověď o jedné HTML byl odeslán do prohlížeče. Šablony rozložení skutečně usnadňují provést změny, které platí pro všechny stránky v aplikaci. Další informace najdete v další [rozložení](../../mvc/views/layout.md).

![Zobrazení seznamu filmu](../../tutorials/first-mvc-app/adding-view/_static/hell3.png)

Naše trocha "data" (v tomto případě "Hello z našich zobrazit šablonu!" zpráva) je pevně zakódovaná, přestože. Aplikace MVC má "V" (zobrazení) a máte hodně "C" (řadič), ale žádné "M" (modelu) ještě.

## <a name="passing-data-from-the-controller-to-the-view"></a>Předání dat z řadiče zobrazení

Akce kontroleru je vyvolán v odpovědi na požadavek příchozí adresy URL. Třída kontroleru je, kde můžete napsat kód, který zpracovává příchozí požadavky prohlížeče. Řadičem načte data ze zdroje dat a rozhodne, jaký typ odpověď k odeslání zpět do prohlížeče. Zobrazit šablony lze z řadiče pro vygenerování a formátování odpovědi HTML do prohlížeče.

Řadiče jsou zajišťuje data nutná pro šablonu zobrazení k vykreslení odpovědi. Osvědčeným postupem: by měl zobrazit šablony **není** provést obchodní logiky nebo pracovat přímo s databází. Místo toho by měla zobrazit šablonu fungovat jenom s data, která je k němu poskytované řadičem. Zachování tento "oddělené oblasti zájmu" vám pomůže uchovávat kódu čistá, možností intenzivního testování a udržovatelný.

V současné době `Welcome` metoda v `HelloWorldController` třídy trvá `name` a `ID` parametr a potom výstupy hodnoty přímo do prohlížeče. Místo mít řadič vykreslení této odpovědi jako řetězec, umožňuje změnit řadič místo toho použít šablonu zobrazení. Zobrazit šablonu způsobí vygenerování dynamických odpovědí, která znamená, že potřebujete předat příslušné bits dat z řadiče zobrazení za účelem vygenerování odpovědi. To provedete tak, že řadiče put dynamických dat (parametry), která vyžaduje zobrazení šablony `ViewData` slovník, který šablona zobrazení můžete poté přistoupit.

Vraťte se do *HelloWorldController.cs* soubor a změňte `Welcome` metody přidat `Message` a `NumTimes` hodnotu `ViewData` slovníku. `ViewData` Je dynamický objekt, což znamená všechno můžete vložit do ní; slovník `ViewData` objekt nemá žádné definované vlastnosti, dokud vložíte něco uvnitř ho. [Systému vazby modelu MVC](xref:mvc/models/model-binding) automaticky mapuje pojmenované parametry (`name` a `numTimes`) z řetězce dotazu v panelu Adresa parametry ve své metodě. Kompletní *HelloWorldController.cs* soubor vypadá takto:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` Objekt slovníku obsahuje data, která bude předána do zobrazení. 

Vytvořit šablonu úvodní zobrazení s názvem *Views/HelloWorld/Welcome.cshtml*.

Vytvoříte smyčku v *Welcome.cshtml* zobrazit šablonu, která zobrazuje "text Hello" `NumTimes`. Nahraďte obsah *Views/HelloWorld/Welcome.cshtml* následujícím kódem:

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Uložte změny a přejděte na následující adresu URL:

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Data jsou převzaty z adresy URL a předaný pomocí řadiče [vazač modelu MVC](xref:mvc/models/model-binding) . Balíčky data do kontroleru `ViewData` slovníku a předává, které objektu do zobrazení. Zobrazení pak vykreslí data ve formátu HTML v prohlížeči.

![O zobrazení úvodní štítky a frázi Hello Rick zobrazí čtyřikrát](../../tutorials/first-mvc-app/adding-view/_static/rick.png)

V ukázce výše jsme použili `ViewData` slovník k předávání dat z řadiče zobrazení. Později v tomto kurzu budeme používat model zobrazení k předávání dat z řadiče zobrazení. Přístup modelu zobrazení k předávání dat je obvykle mnohem upřednostňované přes `ViewData` slovníku přístup. V tématu [ViewModel vs ViewData vs ViewBag vs TempData vs relace v MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc) Další informace.

Dobře, který byl druh "M" pro model, ale není typ databáze. Podívejme se, co jsme jste se naučili a vytvořit databázi filmy.