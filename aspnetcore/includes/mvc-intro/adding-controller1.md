Vzor architektury Model-View-Controller (MVC) rozděluje aplikace do tří hlavních součástí: **M**odelu, **V**obrazit, a **C**ontroller. Vzor MVC pomáhá vytvářet aplikace, které jsou více možností intenzivního testování a aktualizace je snazší než tradiční monolitický aplikace. Aplikace založené na rozhraní MVC obsahovat:

* **M**odels: tříd, které představují data aplikace. Vynutit obchodní pravidla pro tato data pomocí tříd modelu logiku ověření. Model objektů obvykle načíst a ukládají stav modelu v databázi. V tomto kurzu `Movie` model načte film data z databáze, poskytuje k zobrazení nebo aktualizuje ji. Aktualizovaná data jsou zapsána do databáze.

* **V**iews: zobrazení jsou komponenty, které zobrazují aplikace uživatelské rozhraní (UI). Obecně platí toto uživatelské rozhraní zobrazí data modelu.

* **C**ontrollers: třídy, které zpracovávají požadavky prohlížeče. Načítají data modelu a volání Zobrazit šablony, které vrátí odpověď. V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce. Například kontroler zpracovává hodnoty trasy dat a řetězce dotazu a předává tyto hodnoty modelu. Model může použít tyto hodnoty dotazu databázi. Například `http://localhost:1234/Home/About` má data trasy `Home` (řadič) a `About` (metoda akce se má volat v domácí řadiči). `http://localhost:1234/Movies/Edit/5`je požadavek na Upravit na video s ID = 5, pomocí film řadiče.  Budeme mluvit o data trasy později v tomto kurzu.

Vzor MVC pomáhá vytvářet aplikace, které separují jednotlivé aspekty aplikace (logika vstupu, obchodní logika a logika uživatelského rozhraní) současně poskytují volné párování mezi těmito elementy. Vzor Určuje, kde by jednotlivé typy logiky umístěny v aplikaci. Logika uživatelského rozhraní patří do zobrazení. Logika vstupu patří do kontroleru. Obchodní logika patří do modelu. Tato separace pomáhá zvládnout složitost při sestavování aplikace, protože umožňuje pracovat na jeden aspekt jejich implementace v čase bez dopadu na kód jiného. Například můžete pracovat na zobrazení kódu bez v závislosti na obchodní logiku kód.

Zahrnují tyto koncepty z této série kurzu jsme ukazují, jak je používat k vytvoření filmová aplikace. Obsahuje složky pro projekt MVC *řadiče* a *zobrazení*. A *modely* se přidá složka v pozdější fázi.

