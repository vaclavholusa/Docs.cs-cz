Nahraďte obsah *Views/HelloWorld/Index.cshtml* Razor zobrazení souboru následujícím kódem:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

Přejděte na `http://localhost:xxxx/HelloWorld`. `Index` Metoda ve `HelloWorldController` nedělalo většinu; spustili příkaz `return View();`, která zadané, že metoda by měla používat soubor šablony zobrazení k vykreslení odezva do prohlížeče. Protože jste nezadali explicitní název souboru šablony zobrazení, MVC na výchozí pomocí *Index.cshtml* zobrazení souboru v */zobrazení/HelloWorld* složky. Následující obrázek ukazuje řetězec "Hello z našich zobrazit šablonu!" pevně zakódované v zobrazení.

![Okno prohlížeče](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

Pokud okno prohlížeče je nízká (třeba na mobilním zařízení), možná budete muset přepínací tlačítko (tap) [Bootstrap navigační tlačítko](http://getbootstrap.com/components/#navbar) v pravém horním rohu zobrazíte **Domů**, **o**, a **kontakt** odkazy.

![Zvýraznění Bootstrap navigační tlačítko okno prohlížeče](~/tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>Změna zobrazení a rozložení stránky

Klepnutím na odkazy nabídky (**MvcMovie**, **Domů**, **o**). Každá stránka zobrazuje stejné rozložení nabídky. Rozložení nabídky je implementována v *Views/Shared/_Layout.cshtml* souboru. Otevřít *Views/Shared/_Layout.cshtml* souboru.

[Rozložení](xref:mvc/views/layout) šablony umožňují zadat kontejner rozložení HTML vašeho webu na jednom místě a použijte ji na několika stránkách ve vaší lokalitě. Najít `@RenderBody()` řádku. `RenderBody` je zástupný symbol, kde všechny v zobrazení konkrétní stránky, můžete vytvořit, zobrazit *zabalené* stránce rozložení. Například, pokud jste vybrali **o** odkaz, **Views/Home/About.cshtml** je zobrazení vykresleno uvnitř `RenderBody` metody.

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>Změnit název a nabídky odkaz v souboru rozložení

Do prvku název změnit `MvcMovie` k `Movie App`. Změní celý text ukotvení v šabloně rozložení z `MvcMovie` k `Movie App` a kontroler, z `Home` k `Movies` jako zvýrazněné níže:

::: moniker range="<= aspnetcore-2.0"

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout21.cshtml?highlight=7,31)]

::: moniker-end

>[!WARNING]
> Implementovali jsme ještě `Movies` ještě řadič, takže když kliknete na tento odkaz, zobrazí se chyba 404 (Nenalezeno).

Uložte změny a klepněte **o** odkaz. Všimněte si, jak se teď zobrazí jako nadpis informace o kartě prohlížeče **o - filmová aplikace** místo **o – Mvc Movie**: 

![Karta](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

Klepněte **kontakt** propojit a Všimněte si také zobrazit text nadpisu a ukotvení **filmová aplikace**. Jsme byli schopni jednou provést změnu v šabloně rozložení, a mít všechny stránky na webu nový text odkazu a nový název.

Zkontrolujte *Views/_ViewStart.cshtml* souboru:


```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart.cshtml* souboru přináší *Views/Shared/_Layout.cshtml* soubor pro každé zobrazení. Můžete použít `Layout` vlastnost pro nastavení zobrazení jiné rozložení, nebo ji nastavte na `null` , použije se žádný soubor rozložení.

Změna názvu `Index` zobrazení.

Otevřít *Views/HelloWorld/Index.cshtml*. Chcete-li provést změnu na dvou místech:

   * Text zobrazený v nadpisu v prohlížeči.
   * Sekundární záhlaví (`<h2>` element).

Budete je provedete trochu jinak, uvidíte, jaké verze kódu se změní kterou část aplikace.


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

`ViewData["Title"] = "Movie List";` v kódu nad sad `Title` vlastnost `ViewData` slovník, který "Seznam video". `Title` Vlastnost se používá v `<title>` prvek HTML na stránce rozložení:


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Uložte změny a přejděte do `http://localhost:xxxx/HelloWorld`. Všimněte si, že došlo ke změně názvu prohlížeče, záhlaví primární a sekundární záhlaví. (Pokud se nezobrazí změny v prohlížeči, pravděpodobně jste zobrazili obsah uložený v mezipaměti. Stisknutím kláves Ctrl + F5 v prohlížeči k vynucení odpověď ze serveru, který se má načíst.) Název prohlížeče se vytvoří s `ViewData["Title"]` jsme si nastavili *Index.cshtml* zobrazit šablony a další "-video aplikace" přidá soubor rozložení.

Všimněte si také způsob, jak v obsahu *Index.cshtml* zobrazit šablonu byl sloučen s *Views/Shared/_Layout.cshtml* zobrazit šablonu a odpověď o jedné HTML byl odeslán do prohlížeče. Rozložení šablony usnadňují skutečně provést změny, které platí pro všechny stránky v aplikaci. Další informace najdete tady [rozložení](xref:mvc/views/layout).

![Zobrazení seznamu Movie](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

Naše něco "data" (v tomto případě "Hello z našich zobrazit šablonu!" zpráva) je pevně zakódované, i když. Aplikace MVC má "V" (Zobrazit) a máte "C" (controller), ale žádné "M" (modelu) ještě.

## <a name="passing-data-from-the-controller-to-the-view"></a>Předání dat z Kontroleru zobrazení

Akce kontroleru jsou vyvolány v reakci na příchozí adrese URL žádosti. Třída kontroleru je místo, kde píšete kód, který zpracovává příchozí požadavky prohlížeče. Kontroler načte data ze zdroje dat a rozhodne, jaký typ odpověď k odeslání zpět do prohlížeče. Zobrazit šablony lze použít z kontroleru pro generování a formátovat odpověď ve formátu HTML v prohlížeči.

Kontrolery odpovídají za poskytování dat vyžaduje, aby šablona zobrazení k vykreslení odpovědi. Osvědčený postup: Zobrazit šablony by měl **není** provádění obchodní logiky nebo pracovat přímo s databází. Zobrazit šablonu místo toho by měla fungovat jenom s data, která je poskytována kontroleru. Zachování tohoto "oddělené oblasti zájmu" udržet váš kód čistý, možností intenzivního testování a udržovatelný.

V současné době `Welcome` metodu `HelloWorldController` třídy přijímá `name` a `ID` parametr a potom výstupy hodnoty přímo do prohlížeče. Spíše než mít řadič vykreslení této odpovědi jako řetězec, změňte kontroler místo toho použít šablonu zobrazení. Šablona zobrazení generuje dynamické odpovědi, což znamená, že odpovídající částí dat musí být předán z kontroleru zobrazení k vygenerování odpovědi. To udělat tak, že kontroler umístit dynamických dat (parametry), která vyžaduje zobrazení šablony `ViewData` slovník, který se pak můžou zobrazit šablonu.

Vraťte se na *HelloWorldController.cs* soubor a změňte `Welcome` metoda pro přidání `Message` a `NumTimes` hodnota, která se `ViewData` slovníku. `ViewData` Slovník je dynamický objekt, což znamená, že můžete vložit cokoliv, co chcete; `ViewData` objekt nemá žádné definované vlastnosti, dokud je něco v ji vložíte. [Systém vazby modelu MVC](xref:mvc/models/model-binding) automaticky mapují pojmenované parametry (`name` a `numTimes`) z řetězce dotazu do adresního řádku parametrům ve své metodě. Kompletní *HelloWorldController.cs* souboru vypadá takto:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` Objekt dictionary, obsahuje data, která bude předána do zobrazení. 

Vytvoření šablony úvodní zobrazení s názvem *Views/HelloWorld/Welcome.cshtml*.

Vytvoříte smyčka v *Welcome.cshtml* zobrazit šablonu, která se zobrazí "Hello" `NumTimes`. Nahraďte obsah *Views/HelloWorld/Welcome.cshtml* následujícím kódem:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Uložte změny a přejděte na následující adresu URL:

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Data přijatá z adresy URL a předat pomocí řadiče [vazač modelu MVC](xref:mvc/models/model-binding) . Balíčky data do kontroleru `ViewData` slovníku a, které se objekt předá do zobrazení. Zobrazení pak vykreslí data ve formátu HTML v prohlížeči.

![O zobrazení úvodní popisek a frází Hello Rick zobrazí čtyřikrát](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

V příkladu výše jsme použili `ViewData` slovníku k předání dat z kontroleru zobrazení. Později v tomto kurzu budeme používat model zobrazení k předání dat z kontroleru zobrazení. Přístup modelu zobrazení k předávání dat je obvykle mnohem upřednostňované nad `ViewData` slovníku přístup. Zobrazit [ViewModel vs ViewData vs objekt ViewBag vs TempData vs relace v MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc) Další informace.

Dobře, to bylo druh "M" pro model, ale není typ databáze. Pojďme se na to co jsme se naučili a vytvořit databázi videa.
