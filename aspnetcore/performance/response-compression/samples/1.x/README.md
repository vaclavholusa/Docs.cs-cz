# <a name="response-compression-sample-application-aspnet-core-1x"></a>Odpověď komprese ukázkovou aplikaci (ASP.NET Core 1.x)

Tento příklad ukazuje použití ASP.NET Core 1.x middlewaru odpovědi kompresi kompresi odpovědí HTTP pro. Vzorek ukazuje Gzip a komprese Vlastní zprostředkovatelé pro textové a obrázkové odpovědi a ukazuje, jak přidat typ MIME pro kompresi. Ukázka 2.x ASP.NET Core, najdete v části [odpovědi komprese ukázkovou aplikaci (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).

## <a name="examples-in-this-sample"></a>Příklady v této ukázce
* `GzipCompressionProvider`
  * `text/plain`
    * **/**-Lorem Ipsum textový soubor odpovědi na 2,044 bajtů, které bude komprimovat 927 bajtů
    * **/testfile1kb.txt** -textový soubor odpovědi na 1,033 bajtů, které bude komprimovat 47 bajtů
    * **/ skapat** -odpovědi vystavené jako jeden znaků v 1 druhý intervalech 
  * `image/svg+xml`
    * **/Banner.SVG** -odpovědi bitové kopie A škálovatelné grafiky SVG (Vector) v 9,707 bajtů, které bude komprimovat 4,459 bajtů
* `CustomCompressionProvider`<br>Ukazuje, jak implementovat vlastní komprese poskytovatele pro použití s middlewarem

Pokud požadavek obsahuje `Accept-Encoding` přidá hlavičku, ukázky `Vary: Accept-Encoding` hlavičky odpovědi. `Vary` Záhlaví dá pokyn mezipamětí udržovat více kopií v reakci na alternativní hodnoty `Accept-Encoding`, takže komprimované (gzip) i nekomprimované verze jsou uložená v mezipaměti pro systémy, které můžete buď přijmout komprimované nebo nekomprimované odpověď.

## <a name="using-the-sample"></a>Pomocí vzorku
1. Vytvořte pomocí příkazu žádost o [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), nebo [Postman](https://www.getpostman.com/) k aplikaci bez `Accept-Encoding` záhlaví a Poznámka datové části odpovědi, velikost reakce a hlavičky odpovědi.
2. Přidat `Accept-Encoding: gzip` záhlaví a poznamenejte si velikost komprimované odpovědi a záhlaví odpovědi. Zobrazí velikost odpovědi vyřadit a `Content-Encoding: gzip` hlavička odpovědi je zahrnuta v ukázkové aplikace. Když se podíváte na text odpovědi pro Lorem Ipsum nebo **testfile1kb.txt** odpověď, a zobrazí, text je komprimované a nečitelné.
3. Přidat `Accept-Encoding: mycustomcompression` záhlaví a poznamenejte si hlavičky odpovědi. `CustomCompressionProvider` Je prázdná implementace, která není ve skutečnosti komprimovat odpovědi, ale můžete vytvořit vlastní kompresi datového proudu obálka pro `CreateStream()` metoda.
