# <a name="custom-model-binding-demo"></a>Ukázkový Model vlastní vazby

Můžete otestovat `ByteArrayModelBinder` spuštěním aplikace a publikování řetězec s kódováním base64, pomocí ke koncovému bodu ImageController (/ api/image /). Proparties souboru a název souboru musí určit v žádosti subjekt jako data formuláře (s použitím Postman nebo podobného nástroje). Můžete použít [tento řetězec ukázka](Base64String.txt). Výsledek se uloží do složky wwwroot nebo bitové kopie nebo odeslání se název souboru, který jste zadali.

K testování v příkladu vlastní vazby, zkuste následující koncové body: /api/authors/1 /api/authors/2 (není NALEZENA) /api/boundauthors/1 /api/boundauthors/2 (není NALEZENA) /api/boundauthors/get/1 /api/boundauthors/get/2 (ne obsahu) – Tato akce neohlásí pro Hodnota Null a vrátí, nebyl nalezen
