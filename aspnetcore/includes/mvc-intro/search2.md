<!--
[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="60c40-101">Předchozí `Index` metoda:</span><span class="sxs-lookup"><span data-stu-id="60c40-101">The previous `Index` method:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

<span data-ttu-id="60c40-102">Aktualizovaný `Index` metoda s `id` parametr:</span><span class="sxs-lookup"><span data-stu-id="60c40-102">The updated `Index` method with `id` parameter:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

<span data-ttu-id="60c40-103">Název hledání můžete nyní předat jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="60c40-103">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![Index zobrazení s neodstraněných Wordu přidat adresu Url a vrácený film seznam dvou filmy, Ghostbusters a Ghostbusters 2](../../tutorials/first-mvc-app/search/_static/g2.png)

<span data-ttu-id="60c40-105">Nelze však budou uživatelé chcete upravit adresu URL pokaždé, když chtějí hledat film.</span><span class="sxs-lookup"><span data-stu-id="60c40-105">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="60c40-106">Proto nyní přidáte prvky uživatelského rozhraní na jejich filtrovat filmy.</span><span class="sxs-lookup"><span data-stu-id="60c40-106">So now you'll add UI elements to help them filter movies.</span></span> <span data-ttu-id="60c40-107">Pokud jste změnili podpis `Index` metoda k testování jak předat trasy vázané `ID` nastavte zpátky tak, aby trvá parametr s názvem, parametr `searchString`:</span><span class="sxs-lookup"><span data-stu-id="60c40-107">If you changed the signature of the `Index` method to test how to pass the route-bound `ID` parameter, change it back so that it takes a parameter named `searchString`:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

<span data-ttu-id="60c40-108">Otevřete *Views/Movies/Index.cshtml* souboru a přidejte `<form>` značek zvýrazněná níže:</span><span class="sxs-lookup"><span data-stu-id="60c40-108">Open the *Views/Movies/Index.cshtml* file, and add the `<form>` markup highlighted below:</span></span>

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

<span data-ttu-id="60c40-109">HTML `<form>` značku používá [pomocná značku formuláře](../../mvc/views/working-with-forms.md), takže při odesílání formuláře řetězec filtru je odeslána do `Index` akce filmy řadiče.</span><span class="sxs-lookup"><span data-stu-id="60c40-109">The HTML `<form>` tag uses the [Form Tag Helper](../../mvc/views/working-with-forms.md), so when you submit the form, the filter string is posted to the `Index` action of the movies controller.</span></span> <span data-ttu-id="60c40-110">Uložte změny a pak testování filtru.</span><span class="sxs-lookup"><span data-stu-id="60c40-110">Save your changes and then test the filter.</span></span>

![Index zobrazení s neodstraněných word zadali do textového pole Název filtru](../../tutorials/first-mvc-app/search/_static/filter.png)

<span data-ttu-id="60c40-112">Neexistuje žádné `[HttpPost]` přetížení z `Index` metoda, jak jste očekávali.</span><span class="sxs-lookup"><span data-stu-id="60c40-112">There's no `[HttpPost]` overload of the `Index` method as you might expect.</span></span> <span data-ttu-id="60c40-113">Není nutné, protože metoda není změny stavu aplikace a právě filtrování dat.</span><span class="sxs-lookup"><span data-stu-id="60c40-113">You don't need it, because the method isn't changing the state of the app, just filtering data.</span></span>

<span data-ttu-id="60c40-114">Můžete přidat následující `[HttpPost] Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="60c40-114">You could add the following `[HttpPost] Index` method.</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

<span data-ttu-id="60c40-115">`notUsed` Parametr se používá k vytvoření přetížení pro `Index` metoda.</span><span class="sxs-lookup"><span data-stu-id="60c40-115">The `notUsed` parameter is used to create an overload for the `Index` method.</span></span> <span data-ttu-id="60c40-116">Budeme mluvit o který později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="60c40-116">We'll talk about that later in the tutorial.</span></span>

<span data-ttu-id="60c40-117">Pokud chcete přidat tuto metodu, původce volání akce odpovídá `[HttpPost] Index` metody a `[HttpPost] Index` metoda by spustit jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="60c40-117">If you add this method, the action invoker would match the `[HttpPost] Index` method, and the `[HttpPost] Index` method would run as shown in the image below.</span></span>

![Okno prohlížeče s odpovědí aplikace z indexu HttpPost: na neodstraněných filtr](../../tutorials/first-mvc-app/search/_static/fo.png)

<span data-ttu-id="60c40-119">Ale i v případě, přidejte tuto `[HttpPost]` verzi `Index` metoda, existuje omezení v tom, jak to všechny byl implementován.</span><span class="sxs-lookup"><span data-stu-id="60c40-119">However, even if you add this `[HttpPost]` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="60c40-120">Představte si, že chcete bookmark konkrétní hledání nebo chcete poslat odkaz přátel, chcete-li zobrazit stejný filtrovaný seznam filmy mohou klepnutím.</span><span class="sxs-lookup"><span data-stu-id="60c40-120">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="60c40-121">Všimněte si, že adresa URL požadavku HTTP POST je stejný jako adresu URL pro požadavek GET (localhost:xxxxx nebo filmy nebo Index) – chybí informace o vyhledávání v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="60c40-121">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL.</span></span> <span data-ttu-id="60c40-122">Hledání řetězce informace jsou odeslány na server jako [formuláři hodnota pole](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span><span class="sxs-lookup"><span data-stu-id="60c40-122">The search string information is sent to the server as a [form field value](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span></span> <span data-ttu-id="60c40-123">Můžete ověřit, která pomocí nástrojů pro vývojáře prohlížeče nebo vynikající [nástroj Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="60c40-123">You can verify that with the browser Developer tools or the excellent [Fiddler tool](http://www.telerik.com/fiddler).</span></span> <span data-ttu-id="60c40-124">Následující obrázek ukazuje vývojářské nástroje prohlížeče Chrome:</span><span class="sxs-lookup"><span data-stu-id="60c40-124">The image below shows the Chrome browser Developer tools:</span></span>

![Síťové kartě nástrojů pro vývojáře v Microsoft Edge zobrazující textu žádosti s hodnotou searchString neodstraněných](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

<span data-ttu-id="60c40-126">Můžete zobrazit parametr vyhledávání a [XSRF](../../security/anti-request-forgery.md) tokenu v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="60c40-126">You can see the search parameter and [XSRF](../../security/anti-request-forgery.md) token in the request body.</span></span> <span data-ttu-id="60c40-127">Všimněte si, jak je uvedeno v předchozí kurz [pomocná značku formuláře](../../mvc/views/working-with-forms.md) generuje [XSRF](../../security/anti-request-forgery.md) token proti padělání.</span><span class="sxs-lookup"><span data-stu-id="60c40-127">Note, as mentioned in the previous tutorial, the [Form Tag Helper](../../mvc/views/working-with-forms.md) generates an [XSRF](../../security/anti-request-forgery.md) anti-forgery token.</span></span> <span data-ttu-id="60c40-128">Jsme nejsou úpravy dat, takže potřebujeme si ověřit token v metodě řadiče.</span><span class="sxs-lookup"><span data-stu-id="60c40-128">We're not modifying data, so we don't need to validate the token in the controller method.</span></span>

<span data-ttu-id="60c40-129">Protože parametr vyhledávání je v těle žádosti a nikoliv adresu URL, nemůže zaznamenat tyto informace vyhledávání záložku nebo sdílet s ostatními.</span><span class="sxs-lookup"><span data-stu-id="60c40-129">Because the search parameter is in the request body and not the URL, you can't capture that search information to bookmark or share with others.</span></span> <span data-ttu-id="60c40-130">Tuto opravu jsme budete udělat tak, že zadáte, by měl být požadavek `HTTP GET`.</span><span class="sxs-lookup"><span data-stu-id="60c40-130">We'll fix this by specifying the request should be `HTTP GET`.</span></span>
