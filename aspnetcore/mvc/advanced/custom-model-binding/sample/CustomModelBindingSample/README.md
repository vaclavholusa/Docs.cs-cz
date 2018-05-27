# <a name="custom-model-binding-demo"></a>Ukázkový Model vlastní vazby

Test `ByteArrayModelBinder` spuštěním aplikace a publikování řetězec s kódováním base64 tak, aby `ImageController` koncový bod (`/api/image/`). Zadejte vlastnosti souboru a název souboru v textu žádosti jako data formuláře (pomocí [Postman](https://www.getpostman.com/) nebo podobného nástroje). Můžete použít [tento řetězec ukázka](Base64String.txt). Výsledek je uložen v *wwwroot nebo bitové kopie nebo nahráváte* složku s zadaný název souboru.

K testování v příkladu vlastní vazby, zkuste následující koncové body:

* /API/authors/1
* /API/authors/2 (není NALEZENA)
* /API/boundauthors/1
* /API/boundauthors/2 (není NALEZENA)
* /API/boundauthors/Get/1
* /API/boundauthors/Get/2 (ne obsahu) &ndash; tato akce neohlásí pro hodnotu null a vrátí *404 nebyl nalezen*.
