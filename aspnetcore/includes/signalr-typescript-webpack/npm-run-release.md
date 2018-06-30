```console
npm run release
```

Tento příkaz vypočítá prostředky klienta ke zpracování při spuštění aplikace. Prostředky jsou umístěny v *wwwroot* složky.

Webpack dokončit následující úlohy:

* Vymazat obsah *wwwroot* adresáře.
* Převést TypeScript na JavaScript&mdash;tento proces se označuje jako *transpilation*.
* Pozměnění generovaného JavaScript pro zmenšení velikosti souborů&mdash;tento proces se označuje jako *minimalizace*.
* Zkopírovat zpracované soubory HTML, JavaScript a CSS z *src* k *wwwroot* adresáře.
* Vložit následující prvky do *wwwroot/index.html* souboru:
    * A `<link>` značka, odkazující *wwwroot/main.\< Hodnota hash\>.css* souboru. Tato značka je umístěna bezprostředně před uzavírací `</head>` značky.
    * A `<script>` značky, odkazující minifikovaný *wwwroot/main.\< Hodnota hash\>.js* souboru. Tato značka je umístěna bezprostředně před uzavírací `</body>` značky.