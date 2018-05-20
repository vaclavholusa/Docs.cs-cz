---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Sestavení rozhraní RESTful API s rozhraním ASP.NET Web API | Microsoft Docs
author: rick-anderson
description: V posledních letech se stal jasné, že HTTP není je jen pro poskytovat stránky HTML. Je také efektivní platformu pro sestavování rozhraní Web API, pomocí o několik...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: cb02288e93be801a1e55852741ed1443d8d3617d
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/18/2018
---
<a name="build-restful-apis-with-aspnet-web-api"></a>Sestavení rozhraní RESTful API s rozhraním ASP.NET Web API
====================
Podle [webové táborech Team](https://twitter.com/webcamps)

> V posledních letech se stal jasné, že HTTP není je jen pro poskytovat stránky HTML. Je také efektivní platformu pro sestavování rozhraní Web API, například pomocí několik příkazů (GET, POST a tak dále) plus pár jednoduchými koncepty *identifikátory URI* a *hlavičky*. Rozhraní ASP.NET Web API je sada komponent, které zjednodušují programování HTTP. Protože je založen na modul runtime rozhraní ASP.NET MVC, webového rozhraní API automaticky zpracovává podrobnosti nízké úrovně přenos HTTP. Současně zpřístupní webového rozhraní API přirozeně programovací model protokolu HTTP. Ve skutečnosti jeden cílem webového rozhraní API je *není* abstraktní rychle když ve skutečnosti HTTP. V důsledku toho webového rozhraní API je flexibilní a snadné rozšíření. V tomto testovacím prostředí praktických použije k vytvoření jednoduché rozhraní REST API pro kontaktujte správce aplikace webového rozhraní API. Pokud vytvoříte klienta využívají rozhraní API. Styl architektury REST ukázal jako efektivní způsob, jak využít HTTP -, i když není určitě pouze platný přístup k protokolu HTTP. Kontaktujte správce zveřejní dosáhl standardu RESTful pro výpis, přidávání a odebírání kontakty, mimo jiné. Toto testovací prostředí vyžaduje základní znalosti protokolu HTTP, REST a předpokládá, že máte základní praktické znalosti jazyka HTML, JavaScript a jQuery.
> 
> > [!NOTE]
> > Má oblast vyhrazený pro rozhraní ASP.NET Web API na webu technologie ASP.NET [ https://asp.net/web-api ](https://asp.net/web-api). Tento web bude pokračovat v poskytování nejnovější informace, ukázky a zprávy související s webového rozhraní API, tak to zkuste ho často Pokud byste chtěli pustíte hlubší do obrázky vytváření vlastní webová rozhraní API k dispozici pro prakticky libovolný zařízení nebo vývojovém prostředí.
> > 
> > Rozhraní ASP.NET Web API, podobné technologie ASP.NET MVC 4, má flexibilitu z hlediska oddělení vrstvě služby z řadičů budete moci použít některé z dostupných vkládání závislostí architektury velmi snadná. Je dobré vzorku v MSDN, který ukazuje způsob použití Ninject pro vkládání závislostí v projektu webového rozhraní API ASP.NET, které si můžete stáhnout z [zde](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí praktických se dozvíte, jak:

- Implementace RESTful webová rozhraní API
- Volání rozhraní API z klienta HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Pro dokončení této praktické cvičení je vyžadován následující text:

- [Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha B](#AppendixB) pokyny o tom, jak ji nainstalovat).

<a id="Setup"></a>
### <a name="setup"></a>Instalace

**Instalace fragmenty kódu**

Pro usnadnění práce většinu kódu, který bude spravovat podél toto testovací prostředí je k dispozici jako fragmenty kódu v sadě Visual Studio. K instalaci fragmenty kódu spustit **.\Source\Setup\CodeSnippets.vsi** souboru.

Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha A:](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické cvičení zahrnuje následující cvičení:

1. [Cvičení 1: Vytvoření jen pro čtení webového rozhraní API](#Exercise1)
2. [Cvičení 2: Vytvoření webové rozhraní API pro čtení i zápis](#Exercise2)
3. [Cvičení 3: Využívají webové rozhraní API z klienta HTML](#Exercise3)

> [!NOTE]
> Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.


Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Cvičení 1: Vytvoření jen pro čtení webového rozhraní API

V tomto cvičení budete implementovat metodu GET jen pro čtení pro kontaktujte správce.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Úloha 1 – Vytvoření projektu rozhraní API

Nové šablony webových projektů ASP.NET v této úloze, použije k vytvoření webové aplikace webového rozhraní API.

1. Spustit **Visual Studio 2012 Express pro Web**, přejděte k **spustit** a typ **VS Express pro Web** stiskněte **Enter**.
2. Z **soubor** nabídce vyberte možnost **nový projekt**. Vyberte **Visual C# | Webové** projektu typu ze zobrazení stromu typ projektu a pak vyberte **webové aplikace ASP.NET MVC 4** typ projektu. Nastavení projektu **název** k *ContactManager* a **název řešení** k *začít*, pak klikněte na tlačítko **OK**.

    ![Vytvoření nové webové aplikace projektu 4.0 MVC ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image1.png "vytváření nové technologie ASP.NET MVC 4.0 projekt webové aplikace")

    *Vytvoření nové webové aplikace projektu 4.0 MVC ASP.NET*
3. V dialogu typ projektu ASP.NET MVC 4, vyberte **webového rozhraní API** typ projektu. Click **OK**.

    ![Určení typu projekt webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image2.png "určující typ projektu webového rozhraní API")

    *Určení typu projekt webového rozhraní API*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Úloha 2 – Vytvoření řadičů API obraťte se na správce

V této úloze vytvoří řadič třídy, ve kterých se bude nacházet metody rozhraní API.

1. Odstraňte soubor s názvem **ValuesController.cs** v rámci **řadiče** složku z projektu.
2. Klikněte pravým tlačítkem myši **řadiče** složky v projektu a vyberte **přidat | Řadič** v místní nabídce.

    ![Přidávání nového řadiče do projektu](build-restful-apis-with-aspnet-web-api/_static/image3.png "přidávání nového řadiče do projektu")

    *Přidávání nového řadiče do projektu*
3. V **přidat kontroler** dialog, který se zobrazí, vyberte **prázdný kontroler API** z nabídky šablony. Název třídy kontroleru **ContactController**. Potom klikněte na **přidat.**

    ![V dialogovém okně Přidat kontroler k vytvoření nového řadiče webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image4.png "pomocí dialogu přidat kontroler k vytvoření nového řadiče webového rozhraní API")

    *V dialogovém okně Přidat kontroler k vytvoření nového řadiče webového rozhraní API*
4. Přidejte následující kód, který **ContactController**.

    (Code fragment kódu - *webové rozhraní API prostředí - Ex01 - Get – metoda API*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Stiskněte klávesu **F5** k ladění aplikace. By se zobrazit výchozí domovskou stránku pro projekt webového rozhraní API.

    ![Výchozí domovskou stránku aplikace rozhraní ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "výchozí domovskou stránku aplikace rozhraní ASP.NET Web API")

    *Výchozí domovskou stránku aplikace rozhraní ASP.NET Web API*
6. V okně Internet Exploreru, stiskněte **F12** klíč otevřete **Developer Tools** okno. Klikněte **sítě** a pak klikněte **spustit zachytávání** tlačítko Začněte zachytávající síťový přenos do okna.

    ![Otevřete kartu síť a inicializaci sítě zaznamenat](build-restful-apis-with-aspnet-web-api/_static/image6.png "otevírání na síťové kartě a inicializaci sítě zachycení")

    *Otevřete kartu síť a inicializaci zachycení dat ze sítě*
7. Připojit adresu URL v adresním řádku prohlížeče s **/api/kontakt** a stiskněte klávesu enter. Podrobnosti o přenos se zobrazí v okně Sběr sítě. Všimněte si, že je typ MIME do odpovědi **application/json**. Tento příklad ukazuje, jak je výchozí formát výstupu JSON.

    ![Zobrazení výstupu webového rozhraní API žádosti v zobrazení sítě](build-restful-apis-with-aspnet-web-api/_static/image7.png "zobrazení výstupu webového rozhraní API žádosti v zobrazení sítě")

    *Zobrazení výstupu webového rozhraní API žádosti v zobrazení sítě*

    > [!NOTE]
    > Internet Explorer 10 výchozí chování v tomto okamžiku bude požádat, pokud by uživatel chtěl uložit nebo otevřít datový proud vyplývající z volání webového rozhraní API. Výstup bude textového souboru obsahujícího JSON výsledek volání adresy URL webového rozhraní API. Aby bylo možné sledovat obsah do odpovědi prostřednictvím okno nástroje vývojáři zrušit není dialogové okno.
8. Klikněte **přejít na podrobné zobrazení** tlačítko zobrazíte další podrobnosti o odpovědi toto volání rozhraní API.

    ![Přepnout na podrobné zobrazení](build-restful-apis-with-aspnet-web-api/_static/image8.png "přepnout na zobrazení podrobností")

    *Přepnout na podrobné zobrazení*
9. Klikněte **odpovědi** zobrazíte skutečná text JSON odpovědi.

    ![Zobrazení kódu JSON výstup textu v programu Sledování sítě](build-restful-apis-with-aspnet-web-api/_static/image9.png "zobrazení JSON výstup textu v programu Sledování sítě")

    *Zobrazení textu výstup JSON v programu Sledování sítě*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Úloha 3 – vytvoření kontaktní modely a posílení kontaktní řadiče

V této úloze vytvoří řadič třídy, ve kterých se bude nacházet metody rozhraní API.

1. Klikněte pravým tlačítkem myši **modely** složky a vyberte **přidat | Třída...**  v místní nabídce.

    ![Přidání nového modelu k webové aplikaci](build-restful-apis-with-aspnet-web-api/_static/image10.png "přidávání nový model do webové aplikace")

    *Přidání nového modelu do webové aplikace*
2. V **přidat novou položku** dialogové okno, název nový soubor **Contact.cs** a klikněte na tlačítko **přidat.**

    ![Vytvoření nového souboru třídy obraťte se na](build-restful-apis-with-aspnet-web-api/_static/image11.png "vytváření nového souboru kontakt – třída")

    *Vytváření nového souboru kontakt – třída*
3. Přidejte následující zvýrazněný kód, který **kontaktujte** třídy.

    (Code fragment kódu - *webové rozhraní API prostředí - Ex01 - kontaktní třída*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. V **ContactController** třídy, vyberte slovo **řetězec** v definici metoda **získat** metoda a zadejte slovo *kontaktujte*. Po slovo je zadán v, slouží jako ukazatel zobrazí na začátku slova **kontaktujte**. Buď podržte stisknutou **Ctrl** klíče a stiskněte klávesu tečka (.) nebo klikněte na ikonu pomocí myši otevřete dialog pomoc v editoru kódu vyplnit automaticky **pomocí** direktivy pro modely obor názvů.

    ![Pomocí Intellisense pomoc pro deklarace oborů názvů](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Pomocí Intellisense pomoc pro deklarace oborů názvů*
5. Upravte kód pro **získat** metoda tak to vrátí pole obraťte se na modelu instancí.

    (Code fragment kódu - *webové rozhraní API prostředí - Ex01 - vrácení seznamu kontaktů*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Stiskněte klávesu **F5** k ladění webové aplikace v prohlížeči. Chcete-li zobrazit změny provedené v výstup odezvy rozhraní API, proveďte následující kroky.

   1. Jakmile se prohlížeči se otevře, stiskněte klávesu **F12** Pokud nástrojů pro vývojáře ještě nejsou otevřené.
   2. Klikněte **sítě** kartě.
   3. Stiskněte **spustit zachytávání** tlačítko.
   4. Přidat adresu URL příponu **/api/kontakt** adresy URL v adresním řádku a stiskněte klávesu **Enter** klíč.
   5. Stiskněte **přejít na podrobné zobrazení** tlačítko.
   6. Vyberte **odpovědi** kartě. Měli byste vidět JSON řetězec představující serializovaný formu pole Kontakt instancí.

      ![JSON serializovat výstup komplexní metoda volání webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializovat výstup komplexní volání webového rozhraní API – metoda")

      *Výstup serializován do formátu JSON komplexní volání webového rozhraní API – metoda*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Úloha 4 - extrakce funkce do vrstvy služby

Tato úloha bude ukazují, jak funkce do vrstvy služby pro vývojáře k oddělení jejich funkcí služby z vrstvy řadiče, a tím umožní – opětovné použití služeb, které ve skutečnosti práci snadno rozbalte.

1. Vytvořte novou složku v kořenovém řešení a pojmenujte ji **služby**. Chcete-li to provést, klikněte pravým tlačítkem **ContactManager** projekt, vyberte **přidat** | **novou složku**, pojmenujte ji *služby*.

    ![Vytváření služeb složky](build-restful-apis-with-aspnet-web-api/_static/image14.png "složky vytváření služeb")

    *Vytváření složky služby*
2. Klikněte pravým tlačítkem myši **služby** složky a vyberte **přidat | Třída...**  v místní nabídce.

    ![Přidání nové třídy do složky služby](build-restful-apis-with-aspnet-web-api/_static/image15.png "přidání nové třídy do složky služby")

    *Přidání nové třídy do složky služby*
3. Když **přidat novou položku** otevře se dialogové okno, pojmenujte novou třídu **ContactRepository** a klikněte na tlačítko **přidat**.

    ![Vytvoření souboru třída obsahuje kód pro vrstvu služby úložiště kontaktujte](build-restful-apis-with-aspnet-web-api/_static/image16.png "vytváření souboru třída obsahuje kód pro vrstvu služby úložiště kontakt")

    *Vytvoření souboru třída obsahuje kód pro vrstvu služby úložiště kontakt*
4. Přidat pomocí direktivy k **ContactRepository.cs** souboru obor názvů modelů.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Přidejte následující zvýrazněný kód, který **ContactRepository.cs** souboru k implementaci GetAllContacts metoda.

    (Code fragment kódu - *webové rozhraní API prostředí - Ex01 - kontaktní úložiště*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Otevřete **ContactController.cs** soubor, pokud ještě není otevřené.
7. Přidejte následující příkaz using do části deklarace oboru názvů souboru.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Přidejte následující zvýrazněný kód, který **ContactController.cs** třída přidat soukromé pole představující instanci úložiště, tak, aby zbývající členy provádět třídy používat implementace služby.

    (Code fragment kódu - *webové rozhraní API prostředí - Ex01 - kontaktní řadiče*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Změnit **získat** metoda, díky které se využívají službu kontaktní úložiště.

    (Code fragment kódu - *webové rozhraní API prostředí - Ex01 - vrácení seznamu kontaktů prostřednictvím úložiště*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Umístí zarážku **ContactController**na **získat** definici metody.

   ![Přidání zarážky kontaktní řadiče](build-restful-apis-with-aspnet-web-api/_static/image17.png "přidání zarážky kontaktní řadiče")

   *Přidání zarážky kontaktní řadiče*
11. Stiskněte klávesu **F5** ke spuštění aplikace.
12. Když se otevře v prohlížeči, stiskněte klávesu **F12** otevření nástrojů pro vývojáře.
13. Klikněte **sítě** kartě.
14. Klikněte **spustit zachytávání** tlačítko.
15. Připojte na adresu URL v adresním řádku s příponou **/api/kontakt** a stiskněte klávesu **Enter** načtení kontroleru rozhraní API.
16. Visual Studio 2012 by mělo dojít jednou **získat** metoda zahájí spuštění.

   ![Pozastavení v rámci metody Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "dopadem na dřívější kód v rámci metody Get")

   *Ukončování řádků v rámci metody Get*
17. Stiskněte klávesu **F5** pokračujte.
18. Vraťte se zpátky do Internet Exploreru Pokud ještě není aktivní. Poznámka: okno sběr sítě.

    ![Sítě zobrazit v aplikaci Internet Explorer zobrazuje výsledky volání webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image19.png "sítě zobrazit v aplikaci Internet Explorer zobrazuje výsledky volání webového rozhraní API")

    *Zobrazení sítě v Internet Exploreru zobrazující výsledků volání webového rozhraní API*
19. Klikněte **přejít na podrobné zobrazení** tlačítko.
20. Klikněte **odpovědi** kartě. Všimněte si výstup JSON volání rozhraní API a jak ho reprezentuje dva kontakty načíst vrstvě služby.

    ![Zobrazení výstup JSON ze webového rozhraní API v okně nástroje pro vývojáře](build-restful-apis-with-aspnet-web-api/_static/image20.png "zobrazení výstup JSON ze webového rozhraní API v okně nástroje pro vývojáře")

    *Zobrazení výstup JSON ze webového rozhraní API v okně nástroje pro vývojáře*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Cvičení 2: Vytvoření webové rozhraní API pro čtení i zápis

V tomto cvičení budete implementovat POST a PUT metody pro kontaktujte správce k povolení s funkce pro úpravu dat.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Úloha 1 - otevření projektu webového rozhraní API

V této úloze se připravíte na vylepšení projekt webového rozhraní API, které jsou vytvořené v cvičení 1 tak, aby přijímal vstup uživatele.

1. Spustit **Visual Studio 2012 Express pro Web**, přejděte k **spustit** a typ **VS Express pro Web** stiskněte **Enter**.
2. Otevřete **začít** řešení nacházející se v **zdroj/Ex02-ReadWriteWebAPI/počáteční/** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Otevřete **Services/ContactRepository.cs** souboru.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Úloha 2 – přidání trvalosti dat funkce pro implementaci kontaktní úložiště

V této úloze bude posílení ContactRepository třídu projekt webového rozhraní API, které jsou vytvořené v cvičení 1, aby mohli zachovat a přijímají vstup uživatele a nové instance kontakt.

1. Přidejte následující konstanty, která **ContactRepository** třída představující název webového serveru mezipaměti klíče název položky později v tomto cvičení.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Přidejte konstruktor k **ContactRepository** obsahující následující kód.

    (Code fragment kódu - *webové rozhraní API prostředí - Ex02 - kontaktní úložiště konstruktor*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Upravte kód pro **GetAllContacts** metoda, jak je ukázáno níže.

    (Code fragment kódu - *webové rozhraní API prostředí - Ex02 - Get All Contacts*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Tento příklad je pro demonstrační účely a použije mezipaměti webový server jako úložné médium, tak, aby hodnoty budou být k dispozici pro více klientů současně, místo použití mechanismus relace úložiště nebo úložiště životnost požadavku. Jeden nebo použít rozhraní Entity Framework, XML úložiště nebo jiné různých místo mezipaměti webového serveru.
4. Implementovat novou metodu s názvem **SaveContact** k **ContactRepository** třída udělat práci při ukládání a kontaktovat. **SaveContact** metoda provést jeden **kontaktujte** parametr a vrátit logickou hodnotu hodnotu, která udává úspěch nebo selhání.

    (Code fragment kódu - *webové rozhraní API prostředí - Ex02 – implementace metodu SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Cvičení 3: Využívají webové rozhraní API z klienta HTML

V tomto cvičení vytvoříte klientovi HTML pro volání webového rozhraní API. Tento klient usnadní výměna dat s použitím jazyka JavaScript webového rozhraní API a výsledky se zobrazí ve webovém prohlížeči pomocí kódu HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Úloha 1 - úprava zobrazení indexu zajistit grafickým uživatelským rozhraním pro zobrazení kontaktů

V této úloze budete upravovat výchozí zobrazení indexu webové aplikace pro podporu požadavek na zobrazení seznamu existující kontaktů v prohlížeči HTML.

1. Otevřete **Visual Studio 2012 Express pro Web** Pokud ještě není otevřené.
2. Otevřete **začít** řešení nacházející se v **zdroj/Ex03-ConsumingWebAPI/počáteční/** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Otevřete **Index.cshtml** soubor umístěný ve **zobrazení Domů** složky.
4. Nahraďte kód HTML v rámci div element id **textu** tak, aby vypadal jako následující kód.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Přidejte následující kód v JavaScriptu v dolní části souboru k provedení této žádosti HTTP do webového rozhraní API.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Otevřete **ContactController.cs** soubor, pokud ještě není otevřené.
7. Umístěte zarážku na **získat** metodu **ContactController** třídy.

    ![Uvedení zarážku na metodu Get řadiče rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image21.png "umístění zarážku u metody Get řadiče rozhraní API")

    *Uvedení zarážku na metodu Get řadiče rozhraní API*
8. Stiskněte klávesu **F5** spustit projekt. Prohlížeč načte dokumentu HTML.

    > [!NOTE]
    > Ujistěte se, že procházíte adresy URL kořenového adresáře aplikace.
9. Po načtení stránky a provede jazyka JavaScript, bude dosaženo zarážce a v kontroleru bude pozastavit provádění kódu.

    ![Ladění do volání webového rozhraní API pomocí VS Express pro Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "ladění do volání webového rozhraní API pomocí VS Express pro Web")

    *Ladění do volání webového rozhraní API pomocí sady Visual Studio 2012 Express pro Web*
10. Odebrat zarážek a stiskněte klávesu **F5** nebo ladění nástrojů **pokračovat** tlačítko Pokračovat v načítání zobrazení v prohlížeči. Po dokončení volání webového rozhraní API byste měli vidět vrácená z webového rozhraní API kontaktů volání zobrazit jako položky seznamu v prohlížeči.

    ![Výsledky volání rozhraní API v prohlížeči zobrazí jako položky seznamu](build-restful-apis-with-aspnet-web-api/_static/image23.png "výsledků volání rozhraní API v prohlížeči zobrazí jako položky seznamu")

    *Výsledky volání rozhraní API v prohlížeči zobrazí jako položky seznamu*
11. Zastavte ladění.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Úloha 2 - Úprava zobrazení indexu zajistit grafickým uživatelským rozhraním pro vytvoření kontaktů

V této úloze bude pokračovat ke změně zobrazení indexu aplikace MVC. Formulář bude přidán na stránku HTML, který bude zachycení uživatelského vstupu a odeslat do webového rozhraní API pro vytvoření nového kontaktu a vytvoří se nové metody kontroleru webového rozhraní API shromažďovat data z grafického uživatelského rozhraní.

1. Otevřete **ContactController.cs** souboru.
2. Přidat novou metodu do třídy kontroleru s názvem **Post** jak je znázorněno v následujícím kódu.

    (Code fragment kódu - *webové rozhraní API prostředí - Ex03 - metodu Post*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Otevřete **Index.cshtml** souborů v sadě Visual Studio, pokud ještě není otevřené.
4. Kód HTML níže přidejte do souboru právě po neuspořádaného seznamu, které jste přidali v předchozí úloze.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. V rámci elementu skript v dolní části dokumentu přidejte následující zvýrazněný kód pro zpracování událostí kliknutí na tlačítko, které bude odeslána data do webového rozhraní API pomocí volání HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. V **ContactController.cs**, umístěte zarážku na **Post** metoda.
7. Stiskněte klávesu **F5** ke spuštění aplikace v prohlížeči.
8. Po načtení stránky v prohlížeči, zadejte název nového kontaktu a Id a klikněte na **Uložit** tlačítko.

    ![Klient HTML dokumentu načítání do prohlížeče](build-restful-apis-with-aspnet-web-api/_static/image24.png "klienta HTML dokumentu načítání do prohlížeče")

    *Dokumentu HTML klienta načítání do prohlížeče*
9. Když okna ladicího programu zalomení řádků **Post** metoda, proveďte a podívejte se na vlastnosti **obraťte se na** parametr. Hodnoty musí odpovídat data, která jste zadali ve formuláři.

    ![Obraťte se na objekt odesílána webového rozhraní API z klienta](build-restful-apis-with-aspnet-web-api/_static/image25.png "objekt kontakt odesílána webového rozhraní API z klienta")

    *Obraťte se na objekt odesílána webového rozhraní API z klienta*
10. Krok prostřednictvím metody v ladicím programu až **odpovědi** proměnná byla vytvořena. Při kontrole v **místní hodnoty –** okno v ladicím programu, uvidíte, že byly nastaveny všechny vlastnosti.

   ![Odpověď po vytvoření v ladicím programu](build-restful-apis-with-aspnet-web-api/_static/image26.png "odpověď po vytvoření v ladicím programu")

   *Odpověď po vytvoření v ladicím programu*
11. Pokud vyberete **F5** nebo klikněte na tlačítko **pokračovat** v ladicím programu bude žádost dokončena. Když přepnete zpět do prohlížeče, nového kontaktu se přidal do seznamu kontaktů uložené **ContactRepository** implementace.

   ![V prohlížeči odráží úspěšné vytvoření nové instance kontaktní](build-restful-apis-with-aspnet-web-api/_static/image27.png "prohlížeče odráží úspěšné vytvoření nové instance kontaktů")

   *Úspěšné vytvoření nové instance kontaktní odráží prohlížeče*

> [!NOTE]
> Kromě toho můžete nasadit tuto aplikaci do Azure následující [příloha C: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Toto testovací prostředí obsahuje zavedla nové architektury ASP.NET Web API a provádění RESTful webová rozhraní API pomocí rozhraní. Z tohoto místa může vytvořit nové úložiště, která usnadňuje trvalosti dat pomocí libovolný počet mechanismy a propojit služby si místo jednoduché jeden jako příklad v tomto testovacím prostředí. Webové rozhraní API podporuje mnoho dalších funkcí, například povolení komunikaci od klientů jiného typu než HTML, které jsou napsané v libovolném jazyce, který podporuje protokol HTTP a XML nebo JSON. Schopnost hostitele webového rozhraní API mimo typické webové aplikace je také možné, je možnost vytvářet vlastní formáty serializace.

Má oblast vyhrazený pro rozhraní ASP.NET Web API na webu technologie ASP.NET [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Tento web bude pokračovat v poskytování nejnovější informace, ukázky a zprávy související s webového rozhraní API, tak to zkuste ho často Pokud byste chtěli pustíte hlubší do obrázky vytváření vlastní webová rozhraní API k dispozici pro prakticky libovolný zařízení nebo vývojovém prostředí.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Příloha A: používání fragmentů kódu

S fragmenty kódu máte všechny kód, který je nutné na dosah ruky. Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](build-restful-apis-with-aspnet-web-api/_static/image28.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")

*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.
4. Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).
5. Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.

    ![Začněte psát název fragmentu](build-restful-apis-with-aspnet-web-api/_static/image29.png "začněte psát název fragmentu kódu")

    *Začněte psát název fragmentu kódu*

    ![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](build-restful-apis-with-aspnet-web-api/_static/image30.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")

    *Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*

    ![Stisknutím klávesy Tab znovu a fragmentu rozšíří](build-restful-apis-with-aspnet-web-api/_static/image31.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")

    *Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)

1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.
2. Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.
3. Vyberte relevantní fragment kódu ze seznamu, kliknutím na.

    ![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](build-restful-apis-with-aspnet-web-api/_static/image32.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")

    *Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*

    ![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](build-restful-apis-with-aspnet-web-api/_static/image33.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")

    *Vyberte relevantní fragment kódu ze seznamu, kliknutím na*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Příloha B: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>Visual Studio Express 2012 pro Web se sadou Azure SDK</em>&quot;.
2. Klikněte na **nyní nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.

    ![Nainstalovat Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "nainstalovat Visual Studio Express")

    *Nainstalovat Visual Studio Express*
4. Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Vyjádření souhlasu s podmínkami licence](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Vyjádření souhlasu s podmínkami licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Průběh instalace*
6. Po dokončení instalace, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.

    ![VS Express pro Web dlaždice](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express pro Web dlaždice*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Příloha C: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu

Tento dodatek vám ukáže, jak vytvořit nový web z portálu Azure a publikování aplikace, kterou jste získali podle testovacím prostředí, využívat výhod nasazení webu publikování funkce poskytované platformou Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Úloha 1 – Vytvoření nového webu z portálu Azure

1. Přejděte na [portálu pro správu Azure](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů společnosti Microsoft, které jsou spojené s vaším předplatným.

    > [!NOTE]
    > S Azure můžete bezplatné hostování 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu. Můžete si zaregistrovat [zde](http://aka.ms/aspnet-hol-azure).

    ![Přihlaste se k portálu Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k portálu*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](build-restful-apis-with-aspnet-web-api/_static/image40.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **výpočetní** | **webu**. Potom vyberte **rychle vytvořit** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvoření webu**.

    > [!NOTE]
    > Azure je hostitel pro spouštění v cloudu, ve kterém můžete řídit a spravovat webovou aplikaci. Možnost rychle vytvořit můžete nasadit hotové webové aplikace do Azure z mimo portál. Postup pro nastavení databáze neobsahuje.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](build-restful-apis-with-aspnet-web-api/_static/image41.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** je vytvořena.
5. Po vytvoření webu klikněte na odkaz v části **URL** sloupce. Zkontrolujte, zda je funkční nový web.

    ![Procházení na nový web](build-restful-apis-with-aspnet-web-api/_static/image42.png "procházení na nový web")

    *Procházení na nový web*

    ![Webový server spuštěn](build-restful-apis-with-aspnet-web-api/_static/image43.png "webu systémem")

    *Spuštění webu*
6. Přejděte zpět na portál a klikněte na název webu v části **název** sloupec pro zobrazení stránky pro správu.

    ![Otevření stránky Správa webu](build-restful-apis-with-aspnet-web-api/_static/image44.png "otevření stránek správu webového serveru")

    *Otevření stránek správu webového serveru*
7. V **řídicí panel** v části **rychlého přehledu** klikněte na položku **stažení profilu publikování** odkaz.

    > [!NOTE]
    > *Profilu publikování* obsahuje všechny informace požadované pro publikování webové aplikace do Azure pro každou metodu povoleno publikace. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a řetězců databází, které jsou potřebné k připojení k a ověřování na základě těchto koncových bodů, pro které je metoda publikace povolena. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pro Web** a **sadu Microsoft Visual Studio 2012** podporu čtení publikační profily k automatické konfiguraci těchto programů pro publikování webových aplikací do Azure.

    ![Na webu stažení profilu publikování](build-restful-apis-with-aspnet-web-api/_static/image45.png "stahování webové stránky profilu publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profil publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak používat tento soubor k publikování webové aplikace do Azure ze sady Visual Studio.

    ![Ukládání souboru profilu publikování](build-restful-apis-with-aspnet-web-api/_static/image46.png "ukládání profilu publikování")

    *Ukládání souboru profilu publikování*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá systému SQL Server, databáze, budete muset vytvořit databázi SQL server. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá systém SQL Server může tuto úlohu přeskočit.

1. Budete potřebovat databázi SQL serveru pro ukládání databázi aplikace. Databáze SQL servery můžete zobrazit ze svého předplatného v portálu Azure Management portal na **databází Sql** | **servery** | **řídicího panelu serveru**. Pokud nemáte server vytvořeno, můžete vytvořit jeden pomocí **přidat** tlačítka na panelu příkazů. Poznamenejte si **název serveru a adresa URL, správce přihlašovací jméno a heslo**, jako je použijete v další úkoly. Nevytvářejte databáze ještě, jak bude vytvořen v pozdější fázi.

    ![Řídicí panel serveru databáze SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "řídicího panelu serveru databáze SQL")

    *Řídicí panel serveru databáze SQL*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio, proto je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. To lze provést, klikněte na tlačítko **konfigurace**, vyberte IP adresu z **aktuální IP adresa klienta** a vkládání na **počáteční IP adresa** a **Koncová IP adresa** textová pole a kliknutím ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) tlačítko.

    ![Přidávání IP adresy klienta](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Přidávání IP adresy klienta*
3. Jednou **IP adresa klienta** je povolené IP adresy do seznamu, klikněte na **Uložit** potvrďte změny.

    ![Potvrzení změn](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Potvrzení změn*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu

1. Přejděte zpět na ASP.NET MVC 4 řešení. V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](build-restful-apis-with-aspnet-web-api/_static/image51.png "publikování aplikace")

    *Publikování webu*
2. Umožňuje naimportujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](build-restful-apis-with-aspnet-web-api/_static/image52.png "import profilu publikování")

    *Import profilu publikování*
3. Klikněte na tlačítko **ověření připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření je hotová, jakmile se zobrazí zelené zaškrtnutí zobrazí vedle tlačítko ověřit připojení.

    ![Ověření připojení](build-restful-apis-with-aspnet-web-api/_static/image53.png "ověřování připojení")

    *Ověření připojení*
4. V **nastavení** v části **databáze** části, klikněte na tlačítko vedle připojení databáze textové pole (tj. **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](build-restful-apis-with-aspnet-web-api/_static/image54.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Připojení k databázi nakonfigurujte následujícím způsobem:

   - V **název serveru** zadejte vaše databáze SQL serveru adresu URL pomocí *tcp:* předponu.
   - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
   - V **heslo** zadejte přihlašovací heslo správce serveru.
   - Zadejte nový název databáze, například: *MVC4SampleDB*.

     ![Konfigurace cílový připojovací řetězec](build-restful-apis-with-aspnet-web-api/_static/image55.png "konfigurace cílový připojovací řetězec")

     *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze, klikněte na tlačítko **Ano**.

    ![Vytvoření databáze](build-restful-apis-with-aspnet-web-api/_static/image56.png "vytváření řetězec databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL v systému Windows Azure je uvedené v rámci textbox výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na databázi SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "připojovací řetězec odkazující na databázi SQL")

    *Připojovací řetězec odkazující na databázi SQL*
8. V **Preview** klikněte na tlačítko **publikovat**.

    ![Publikování webové aplikace](build-restful-apis-with-aspnet-web-api/_static/image58.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Jakmile proces publikování dokončí, otevře se výchozí prohlížeč publikované webové stránky.

    ![Aplikace publikována do služby Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "aplikace publikována do služby Windows Azure")

    *Aplikace, které jsou publikovány do služby Azure*
