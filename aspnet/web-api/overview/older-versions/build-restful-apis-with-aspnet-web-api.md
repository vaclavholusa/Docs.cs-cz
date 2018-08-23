---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Vytvoření rozhraní RESTful API s rozhraním ASP.NET Web API | Dokumentace Microsoftu
author: rick-anderson
description: V posledních letech se stal jasné, že protokol HTTP není jen pro poskytovat stránky HTML. Je také výkonnou platformu pro vytváření webových rozhraní API, pomocí několika málo o...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 00304f67138873318b63c5e2ad0cd69aa7449521
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756002"
---
<a name="build-restful-apis-with-aspnet-web-api"></a>Vytvoření rozhraní RESTful API s rozhraním ASP.NET Web API
====================
podle [Campy Web týmu](https://twitter.com/webcamps)

> V posledních letech se stal jasné, že protokol HTTP není jen pro poskytovat stránky HTML. Je také výkonnou platformu pro vytváření webových rozhraní API, pomocí několika příkazů (GET, POST a tak dále) a navíc pár jednoduchými koncepty, jako *identifikátory URI* a *záhlaví*. ASP.NET Web API je sada komponent, které zjednodušují programování HTTP. Protože je postavený na modul runtime ASP.NET MVC, webové rozhraní API automaticky zpracovává podrobnosti nízké úrovně přenos HTTP. Ve stejnou dobu přirozeně webového rozhraní API poskytuje programovací model protokolu HTTP. Ve skutečnosti je jedním z cílů webového rozhraní API *není* abstrakci ve skutečnosti je http. V důsledku toho webového rozhraní API je flexibilní a snadno rozšířit. V této praktické testovací prostředí použijete webového rozhraní API k vytvoření jednoduchého rozhraní REST API pro aplikace požádejte správce. Pokud vytvoříte klienta k využití rozhraní API. Styl architektury REST ukázal být účinný způsob, jak využít HTTP – i když to určitě není platné pouze přístup k protokolu HTTP. Požádejte správce, bude vystavovat RESTful pro výpis, přidávání a odebírání kontakty, mimo jiné. Toto testovací prostředí vyžaduje základní znalost HTTP, REST a předpokládá, že máte základní znalosti pracovních HTML, JavaScript a jQuery.
> 
> > [!NOTE]
> > Webu technologie ASP.NET je vyhrazený pro rozhraní ASP.NET Web API v oblasti [ https://asp.net/web-api ](https://asp.net/web-api). Tento web i nadále bude poskytovat nejnovější informace, ukázky a zpráv týkající se webového rozhraní API, takže vrácení se často Pokud byste chtěli delve hlouběji do obrázky vytváření vlastních webových rozhraní API k dispozici na prakticky jakékoli zařízení nebo vývoj rozhraní framework.
> > 
> > Rozhraní ASP.NET Web API, podobně jako na ASP.NET MVC 4, má velkou flexibilitu z hlediska oddělení vrstva služby z řadiče, abyste mohli používat některé z dostupných injektáž závislostí architektury poměrně snadné. Na webu MSDN, který ukazuje, jak používat Ninject pro injektáž závislostí v projektu webového rozhraní API ASP.NET, který si můžete stáhnout z je dobrý vzorek [tady](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V této praktická cvičení se dozvíte, jak:

- Implementovat rozhraní RESTful webového rozhraní API
- Volání rozhraní API z klienta HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

K dokončení této praktické testovací prostředí jsou vyžadovány následující položky:

- [Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [dodatku B](#AppendixB) pokyny k jeho instalaci).

<a id="Setup"></a>
### <a name="setup"></a>Instalace

**Instalace fragmenty kódu**

Pro usnadnění práce velkou část kódu, které budete spravovat podél tohoto testovacího prostředí je k dispozici jako fragmenty kódu sady Visual Studio. K instalaci spustit fragmenty kódu **.\Source\Setup\CodeSnippets.vsi** souboru.

Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí dodatku A:](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické testovací prostředí obsahuje následující cvičení:

1. [Cvičení 1: Vytvoření webového rozhraní API jen pro čtení](#Exercise1)
2. [Cvičení 2: Vytvoření webového rozhraní API pro čtení a zápis](#Exercise2)
3. [Cvičení 3: Využívají webové rozhraní API z klienta HTML](#Exercise3)

> [!NOTE]
> Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.


Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Cvičení 1: Vytvoření webového rozhraní API jen pro čtení

V tomto cvičení implementujete metody GET jen pro čtení pro kontaktujte správce.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Úloha 1 – Vytvoření projektu rozhraní API

V této úloze použijete nové šablony webových projektů ASP.NET a vytvořte webovou aplikaci webového rozhraní API.

1. Spustit **Visual Studio 2012 Express pro Web**, přejděte k **Start** a typ **VS Express for Web** stiskněte **Enter**.
2. Z **souboru** nabídce vyberte možnost **nový projekt**. Vyberte **Visual C# | Web** typ ve stromovém zobrazení projektu typu projektu a pak vyberte **webové aplikace ASP.NET MVC 4** typ projektu. Nastavení projektu **název** k *ContactManager* a **název řešení** k *začít*, pak klikněte na tlačítko **OK**.

    ![Vytváří se nové technologie ASP.NET MVC 4.0 projekt webové aplikace](build-restful-apis-with-aspnet-web-api/_static/image1.png "vytváření nové technologie ASP.NET MVC 4.0 projekt webové aplikace")

    *Vytváří se nové technologie ASP.NET MVC 4.0 projekt webové aplikace*
3. V dialogu typu projektu ASP.NET MVC 4, vyberte **webového rozhraní API** typ projektu. Klikněte na tlačítko **OK**.

    ![Určení typu projektu webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image2.png "určující typ projektu webového rozhraní API")

    *Určení typu projektu webového rozhraní API*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Úloha 2 – Vytvoření Kontrolery rozhraní API Správce kontaktů

V této úloze vytvoříte třídy kontroleru, ve kterých se bude nacházet metody rozhraní API.

1. Odstranit soubor s názvem **ValuesController.cs** v rámci **řadiče** složku z projektu.
2. Klikněte pravým tlačítkem myši **řadiče** složky v projektu a vyberte **přidat | Kontroler** v místní nabídce.

    ![Přidání do projektu nový kontroler](build-restful-apis-with-aspnet-web-api/_static/image3.png "přidání nového řadiče do projektu")

    *Přidání nového řadiče do projektu*
3. V **přidat kontroler** dialogové okno, které se zobrazí, vyberte **prázdný kontroler API** z nabídky šablon. Název třídy kontroleru **ContactController**. Potom klikněte na **přidat.**

    ![Chcete-li vytvořit nový kontroler webového rozhraní API pomocí dialogového okna Přidat kontroler](build-restful-apis-with-aspnet-web-api/_static/image4.png "pomocí dialogového okna Přidat kontroler vytvořit nový kontroler Web API")

    *Chcete-li vytvořit nový kontroler webového rozhraní API pomocí dialogového okna Přidat kontroler*
4. Přidejte následující kód, který **ContactController**.

    (Fragment - kódu *webové rozhraní API Lab – Ex01 - Get – metoda API*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Stisknutím klávesy **F5** k ladění aplikace. By se zobrazit výchozí domovskou stránku projektu webového rozhraní API.

    ![Výchozí domovskou stránku aplikace ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "výchozí domovskou stránku aplikace ASP.NET Web API")

    *Výchozí domovskou stránku aplikace ASP.NET Web API*
6. V okně Internet Exploreru, stiskněte **F12** klávesy otevřete **vývojářské nástroje** okna. Klikněte na tlačítko **sítě** kartu a potom klikněte na tlačítko **spustit zachytávání** spusťte zachytávání síťového provozu do okna.

    ![Otevřete kartu síť a inicializaci sítě zachycení](build-restful-apis-with-aspnet-web-api/_static/image6.png "otevřete kartu síť a inicializaci sítě zachytávání")

    *Otevřete kartu síť a inicializaci zachytávání sítě*
7. Připojte k adrese URL v adresním řádku prohlížeče s **/api/contact** a stiskněte klávesu enter. Přenos podrobností se zobrazí v okně zachytávání sítě. Všimněte si, že je typ MIME odpovědi **application/json**. Tento příklad ukazuje, jak je výchozí formát výstupu JSON.

    ![Zobrazení výstupu požadavek webového rozhraní API v zobrazení sítě](build-restful-apis-with-aspnet-web-api/_static/image7.png "zobrazení výstupu požadavek webového rozhraní API v okně sítě")

    *Zobrazení výstupu požadavek webového rozhraní API v okně sítě*

    > [!NOTE]
    > Výchozí chování Internet Explorer 10 v tomto okamžiku bude dotaz, jestli uživatel chtěli uložit nebo otevřít datový proud výsledek volání webového rozhraní API. Výstup bude textový soubor obsahující JSON výsledek volání adresy URL webového rozhraní API. Nerušit dialogového okna, aby bylo možné sledovat obsah odpovědi pomocí panelu nástrojů pro vývojáře.
8. Klikněte na tlačítko **přejít na podrobnosti přehledu** zobrazíte další podrobnosti o odpovědi toto volání rozhraní API.

    ![Přepnout do zobrazení podrobné](build-restful-apis-with-aspnet-web-api/_static/image8.png "přepnout na zobrazení podrobností")

    *Přepnout na podrobné zobrazení*
9. Klikněte na tlačítko **tělo odpovědi** kartu k zobrazení vlastní text JSON odpovědi.

    ![Zobrazení JSON výstupu textu v nástroji Sledování sítě](build-restful-apis-with-aspnet-web-api/_static/image9.png "zobrazení ve formátu JSON výstupu textu v nástroji Sledování sítě")

    *Zobrazení textového výstupu JSON v nástroji Sledování sítě*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Úloha 3 – vytvoření kontaktu modely a rozšiřují Contactcontroller

V této úloze vytvoříte třídy kontroleru, ve kterých se bude nacházet metody rozhraní API.

1. Klikněte pravým tlačítkem myši **modely** a pak zvolte položku **přidat | Třída...**  v místní nabídce.

    ![Přidání nového modelu k webové aplikaci](build-restful-apis-with-aspnet-web-api/_static/image10.png "přidání nového modelu do webové aplikace")

    *Přidání nového modelu do webové aplikace*
2. V **přidat novou položku** dialogového okna, název nového souboru **Contact.cs** a klikněte na tlačítko **přidat.**

    ![Vytváří se nový soubor třídy kontakt](build-restful-apis-with-aspnet-web-api/_static/image11.png "vytváří se nový soubor třídy kontaktu")

    *Vytváří se nový soubor třídy kontaktu*
3. Přidejte následující zvýrazněný kód do **kontakt** třídy.

    (Fragment - kódu *webové API Lab – Ex01 - kontaktní třídy*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. V **ContactController** vyberte slovo **řetězec** v definici metody **získat** metody a typu slovo *kontakt*. Jakmile je zadali slovo, indikátor se zobrazí na začátku slova **kontakt**. Buď podržte stisknutou klávesu **Ctrl** klíče a stiskněte klávesu tečka (.) nebo klikněte na ikonu myší a otevřete dialogové okno pomoc v editoru kódu k automatickému vyplnění **pomocí** směrnice pro modely obor názvů.

    ![Pomocí technologie IntelliSense pro deklarace oboru názvů](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Pomocí technologie IntelliSense pro deklarace oboru názvů*
5. Upravte kód **získat** metodu tak, že vrátí pole instancí modelu kontaktu.

    (Fragment - kódu *webové rozhraní API testovacího prostředí – Ex01 - vrácení seznamu kontaktů*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Stisknutím klávesy **F5** ladění webové aplikace v prohlížeči. Chcete-li zobrazit změny provedené v výstup odezvy rozhraní API, postupujte následovně.

   1. Až se otevře v prohlížeči, klikněte na **F12** Pokud nástroje pro vývojáře zatím nejsou otevřené.
   2. Klikněte na tlačítko **sítě** kartu.
   3. Stisknutím klávesy **spustit zachytávání** tlačítko.
   4. Přidat přípona adresy URL **/api/contact** adresy URL v adresním řádku a stisknutím klávesy **Enter** klíč.
   5. Stisknutím klávesy **přejít na podrobnou zobrazení** tlačítko.
   6. Vyberte **tělo odpovědi** kartu. Měli byste vidět řetězec JSON představující serializovanou formu pole instancí kontaktu.

      ![JSON serializovat výstup komplexní volání metody webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializovat výstup komplexní volání webového rozhraní API – metoda")

      *Výstup ve formátu JSON serializovat komplexní volání metody webového rozhraní API*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Úloha 4 – extrahování funkce do vrstvy služeb

Tato úloha popisuje, jak extrahovat funkci do vrstvy služby k tomu, aby pro vývojáře k oddělení jejich funkcí služby z řadiče vrstvy, a tím umožní opětovné použití služeb, které skutečně pracovat.

1. Vytvořte novou složku v kořenovém adresáři řešení s názvem **služby**. Chcete-li to provést, klikněte pravým tlačítkem na **ContactManager** projekt, vyberte **přidat** | **novou složku**, pojmenujte ho *služby*.

    ![Vytvoření složky služby](build-restful-apis-with-aspnet-web-api/_static/image14.png "složku vytvoření služby")

    *Vytváření složky služby*
2. Klikněte pravým tlačítkem myši **služby** a pak zvolte položku **přidat | Třída...**  v místní nabídce.

    ![Přidání nové třídy do složky služby](build-restful-apis-with-aspnet-web-api/_static/image15.png "přidání nové třídy do složky služby")

    *Přidání nové třídy do složky služby*
3. Když **přidat novou položku** se zobrazí dialogové okno, pojmenujte novou třídu **ContactRepository** a klikněte na tlačítko **přidat**.

    ![Vytváří se soubor třídy tak, aby obsahovala kód pro vrstvu služby úložiště kontakt](build-restful-apis-with-aspnet-web-api/_static/image16.png "vytváření souboru třídy tak, aby obsahovala kód pro vrstvu služby úložiště kontaktu")

    *Vytváří se soubor třídy tak, aby obsahovala kód pro vrstvu služby úložiště kontaktu*
4. Přidat using direktivu **ContactRepository.cs** soubor obsahoval obor názvů modelů.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Přidejte následující zvýrazněný kód do **ContactRepository.cs** souboru o implementaci GetAllContacts metody.

    (Fragment - kódu *webové kontaktní úložiště API Lab – Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Otevřete **ContactController.cs** souboru, pokud již není otevřen.
7. Přidejte následující příkaz using do části deklarace oboru názvů souboru.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Přidejte následující zvýrazněný kód do **ContactController.cs** třídy přidat soukromé pole k reprezentaci instance úložiště, tak, aby zbývající část třídy členové mohou provádět pomocí implementace služby.

    (Fragment - kódu *webové rozhraní API Lab – Ex01 - Contactcontroller*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Změnit **získat** metodu tak, že díky využívání služby Kontaktní úložiště.

    (Fragment - kódu *webové rozhraní API testovacího prostředí – Ex01 - vrácení seznamu kontaktů prostřednictvím úložiště*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Přidejte zarážku na **ContactController**společnosti **získat** definice metody.

   ![Přidání zarážky contactcontroller](build-restful-apis-with-aspnet-web-api/_static/image17.png "přidání zarážky contactcontroller")

   *Přidání zarážky contactcontroller*
11. Stisknutím klávesy **F5** ke spuštění aplikace.
12. Když se otevře v prohlížeči, stiskněte klávesu **F12** otevřete vývojářské nástroje.
13. Klikněte na tlačítko **sítě** kartu.
14. Klikněte na tlačítko **spustit zachytávání** tlačítko.
15. Připojte k adrese URL do adresního řádku s příponou **/api/contact** a stiskněte klávesu **Enter** načtení kontroleru rozhraní API.
16. Visual Studio 2012 by měl přerušení jednou **získat** metoda zahájí vykonávání.

   ![Dopadem na dřívější kód v rámci metody Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "dopadem na dřívější kód v rámci metody Get")

   *Dopadem na dřívější kód v rámci metody Get*
17. Stisknutím klávesy **F5** pokračujte.
18. Vraťte se do Internet Exploreru Pokud již není aktivní. Poznámka: v okně zachytávání sítě.

    ![Sítě zobrazit v aplikaci Internet Explorer zobrazující výsledky volání webového rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image19.png "sítě zobrazit v aplikaci Internet Explorer zobrazující výsledky volání webového rozhraní API")

    *Zobrazení sítě v aplikaci Internet Explorer zobrazující výsledky volání webového rozhraní API*
19. Klikněte na tlačítko **přejít na podrobnou zobrazení** tlačítko.
20. Klikněte na tlačítko **tělo odpovědi** kartu. Všimněte si, výstup JSON volání rozhraní API a jak ho reprezentuje dva kontakty načítána pro vrstvu služby.

    ![Zobrazení výstupu JSON z webového rozhraní API v okně nástroje pro vývojáře](build-restful-apis-with-aspnet-web-api/_static/image20.png "zobrazení výstupu JSON z webového rozhraní API v okně nástroje pro vývojáře")

    *Zobrazení výstupu JSON z webového rozhraní API v okně nástroje pro vývojáře*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Cvičení 2: Vytvoření webového rozhraní API pro čtení a zápis

V tomto cvičení budete implementovat POST a PUT metody ho povolit funkce pro úpravu dat kontaktujte správce.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Úloha 1 – otevření projektu webového rozhraní API

V této úloze si připravit k vylepšení projekt webového rozhraní API vytvořené v cvičení 1 tak, aby mohl přijímat uživatelský vstup.

1. Spustit **Visual Studio 2012 Express pro Web**, přejděte k **Start** a typ **VS Express for Web** stiskněte **Enter**.
2. Otevřít **začít** řešení nachází v **zdroj/Ex02-ReadWriteWebAPI/počáteční/** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Otevřít **Services/ContactRepository.cs** souboru.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Úloha 2 – Přidání funkce trvalosti dat pro implementaci kontaktní úložiště

V této úloze se rozšířit třídu ContactRepository projekt webového rozhraní API tak, aby ho zachovat a přijímat uživatelský vstup a nový kontakt instance vytvořené v cvičení 1.

1. Přidejte následující konstantu pro **ContactRepository** třídy představující název je název webového serveru mezipaměti položky klíče dále v tomto cvičení.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Přidejte konstruktor k **ContactRepository** obsahující následující kód.

    (Fragment - kódu *webové konstruktor kontaktní úložiště API Lab – Ex02 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Upravte kód **GetAllContacts** způsob, jak je znázorněno níže.

    (Fragment - kódu *webové rozhraní API Lab – Ex02 - Get All Contacts*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > V tomto příkladu je pro demonstrační účely a použije mezipaměť webový server jako úložné médium, tak, aby hodnoty budou mít k dispozici víc klientů současně, místo použití mechanismus pro ukládání relace nebo životnost úložiště požadavku. Entity Framework, XML úložiště nebo jiných různých jeden může použít místo mezipaměti webového serveru.
4. Implementovat novou metodu s názvem **SaveContact** k **ContactRepository** třídy práci uložit kontaktu. **SaveContact** metoda by měla přijímat jeden **kontakt** parametry a návratovým logická hodnota označující úspěšné nebo neúspěšné.

    (Fragment - kódu *webové rozhraní API Lab – Ex02 – implementace metody SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Cvičení 3: Využívají webové rozhraní API z klienta HTML

V tomto cvičení vytvoříte klienta HTML do volání webového rozhraní API. Tento klient usnadní výměna dat s webovým rozhraním API prostřednictvím JavaScriptu a výsledky se zobrazí ve webovém prohlížeči pomocí značky jazyka HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Úloha 1 - úprava zobrazení Index k poskytnutí grafické uživatelské rozhraní pro zobrazení Kontakty

V této úloze budete upravovat výchozí Index zobrazení webové aplikaci, aby podporovala požadavky na zobrazení seznamu existující kontaktů v prohlížeče HTML.

1. Otevřete **Visual Studio 2012 Express pro Web** Pokud ještě není otevřený.
2. Otevřít **začít** řešení nachází v **zdroj/Ex03-ConsumingWebAPI/počáteční/** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Otevřít **Index.cshtml** se nachází v **zobrazení Domů** složky.
4. Nahraďte identifikátorem kódu HTML do elementu div **tělo** tak, aby vypadal jako v následujícím kódu.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Přidejte následující kód jazyka Javascript v dolní části souboru, který se provést požadavek HTTP do webového rozhraní API.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Otevřete **ContactController.cs** souboru, pokud již není otevřen.
7. Umístit zarážky na **získat** metodu **ContactController** třídy.

    ![Umístění zarážky na metodu Get v kontroleru rozhraní API](build-restful-apis-with-aspnet-web-api/_static/image21.png "umístěním zarážky na metodu Get v kontroleru rozhraní API")

    *Umístění zarážky na metodu Get v kontroleru rozhraní API*
8. Stisknutím klávesy **F5** spusťte projekt. Prohlížeč načte dokument HTML.

    > [!NOTE]
    > Ujistěte se, že přecházíte kořenovou adresu URL vaší aplikace.
9. Jakmile stránka načte a spustí jazyka JavaScript, bude dosaženo zarážkou a pozastaví provádění kódu v kontroleru.

    ![Ladění do volání webového rozhraní API pomocí VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "ladění do volání webového rozhraní API pomocí VS Express for Web")

    *Ladění do volání webového rozhraní API pomocí sady Visual Studio 2012 Express pro Web*
10. Odebrat zarážku a stisknutím klávesy **F5** nebo ladění nástrojů **pokračovat** tlačítka pokračovat v načítání zobrazení v prohlížeči. Po dokončení volání webového rozhraní API byste měli vidět kontakty vrácená z webového rozhraní API volat jako seznam položek v prohlížeči.

    ![Výsledky volání rozhraní API, zobrazí v prohlížeči jako položky seznamu](build-restful-apis-with-aspnet-web-api/_static/image23.png "výsledky volání rozhraní API, zobrazí v prohlížeči jako položky seznamu")

    *Výsledky volání rozhraní API, zobrazí v prohlížeči jako položky seznamu*
11. Zastavte ladění.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Úloha 2 - Úprava zobrazení Index k poskytnutí grafickým uživatelským rozhraním pro vytváření kontaktů

V této úloze budete i nadále upravovat zobrazení indexu aplikace MVC. Formuláře bude přidán na stránku HTML, který bude zachycení uživatelského vstupu a odeslat do webového rozhraní API k vytvoření nového kontaktu a vytvoří se nové metody kontroleru webového rozhraní API shromažďovat data z grafického uživatelského rozhraní.

1. Otevřít **ContactController.cs** souboru.
2. Přidejte novou metodu s názvem třídy kontroleru **příspěvek** jak je znázorněno v následujícím kódu.

    (Fragment - kódu *webové rozhraní API Lab – Ex03 – Metoda Post*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Otevřete **Index.cshtml** souboru v sadě Visual Studio, pokud již není otevřen.
4. Přidejte následující kód HTML do souboru bezprostředně po Neseřazený seznam, který jste přidali v předchozím úkolu.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. V rámci elementu skript v dolní části dokumentu přidejte následující zvýrazněný kód pro zpracování události kliknutí na tlačítko, které bude publikovat data k webovému rozhraní API pomocí volání rozhraní HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. V **ContactController.cs**, umístit zarážky na **příspěvek** metody.
7. Stisknutím klávesy **F5** ke spuštění aplikace v prohlížeči.
8. Po načtení stránky v prohlížeči zadejte jméno nového kontaktu a Id a klikněte **Uložit** tlačítko.

    ![Klient HTML dokumentu načítání do prohlížeče](build-restful-apis-with-aspnet-web-api/_static/image24.png "načíst dokument klienta HTML v prohlížeči")

    *Dokument HTML klienta načítání do prohlížeče*
9. Při přestane fungovat okně ladicího programu **příspěvek** způsob zobrazení vlastností **kontaktovat** parametr. Hodnoty musí odpovídat data, která jste zadali ve formuláři.

    ![Objekt kontakt odesílány do webového rozhraní API z klienta](build-restful-apis-with-aspnet-web-api/_static/image25.png "Contact objekt odesílány do webového rozhraní API z klienta")

    *Objekt kontakt odesílány do webového rozhraní API z klienta*
10. Krok prostřednictvím metody v ladicím programu, dokud **odpovědi** proměnná byla vytvořena. Při kontrole v **lokální** okno v ladicím programu, uvidíte, že jsou nastavené všechny vlastnosti.

   ![Odpověď po vytvoření v ladicím programu](build-restful-apis-with-aspnet-web-api/_static/image26.png "odpověď po vytvoření v ladicím programu")

   *Odpověď po vytvoření v ladicím programu*
11. Pokud stisknete **F5** nebo klikněte na tlačítko **pokračovat** v ladicím programu se požadavek dokončí. Když přepnete zpět do prohlížeče, nový kontakt je přidaný do seznamu kontaktů uložené **ContactRepository** implementace.

   ![Po úspěšném vytvoření nové instance kontaktní odráží v prohlížeči](build-restful-apis-with-aspnet-web-api/_static/image27.png "odráží prohlížeče po úspěšném vytvoření nové instance kontaktu")

   *Po úspěšném vytvoření nové instance kontaktní odráží v prohlížeči*

> [!NOTE]
> Kromě toho můžete tuto aplikaci nasadíte do Azure následujícím [příloha C: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Toto testovací prostředí představil novou architekturou ASP.NET Web API a provádění RESTful webová rozhraní API pomocí rozhraní. Z tohoto místa můžete vytvořit nové úložiště, která usnadňuje trvalost dat pomocí libovolného počtu mechanismy a nastavit tuto službu místo jednoduchých zadal jako příklad v tomto testovacím prostředí. Webové rozhraní API podporuje řadu dalších funkcí, jako jsou povolení komunikace z klientů jiného typu než HTML, které jsou napsané v libovolném jazyce, který podporuje protokol HTTP a JSON nebo XML. Možnost hostování webového rozhraní API mimo typické webové aplikace je také možné, jakož i je schopnost vytvářet vlastní formáty serializace.

Webu technologie ASP.NET je vyhrazený pro rozhraní ASP.NET Web API v oblasti [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Tento web i nadále bude poskytovat nejnovější informace, ukázky a zpráv týkající se webového rozhraní API, takže vrácení se často Pokud byste chtěli delve hlouběji do obrázky vytváření vlastních webových rozhraní API k dispozici na prakticky jakékoli zařízení nebo vývoj rozhraní framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Příloha A: používání fragmentů kódu

Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky. Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](build-restful-apis-with-aspnet-web-api/_static/image28.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")

*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.
4. Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).
5. Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.

    ![Začněte psát název fragmentu kódu](build-restful-apis-with-aspnet-web-api/_static/image29.png "začněte psát název fragmentu kódu")

    *Začněte psát název fragmentu kódu*

    ![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](build-restful-apis-with-aspnet-web-api/_static/image30.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")

    *Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*

    ![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](build-restful-apis-with-aspnet-web-api/_static/image31.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")

    *Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)

1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.
2. Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.
3. Kliknutím na vyberte relevantní fragment kódu ze seznamu.

    ![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](build-restful-apis-with-aspnet-web-api/_static/image32.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")

    *Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*

    ![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](build-restful-apis-with-aspnet-web-api/_static/image33.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")

    *Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Příloha B: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Azure SDK</em>&quot;.
2. Klikněte na **nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.

    ![Instalace sady Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "instalace sady Visual Studio Express")

    *Instalace sady Visual Studio Express*
4. Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Přijetí podmínek licence](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Přijetí podmínek licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Průběh instalace*
6. Až instalace skončí, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.

    ![VS Express for Web dlaždice](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express for Web dlaždice*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Příloha C: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu

Tento dodatek se ukazují, jak vytvořit nový web z portálu Azure Portal a publikovat aplikace, kterou jste získali podle testovacího prostředí, využít Webdeploy publikační funkci poskytovaný platformou Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Úloha 1 – Vytvoření nového webu na webu Azure Portal

1. Přejděte [portálu pro správu Azure](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů Microsoft spojených s vaším předplatným.

    > [!NOTE]
    > S Azure můžete zadarmo hostovat 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu. Můžete se zaregistrovat [tady](http://aka.ms/aspnet-hol-azure).

    ![Přihlaste se k portálu Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k portálu*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](build-restful-apis-with-aspnet-web-api/_static/image40.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **Compute** | **webu**. Potom vyberte **rychlé vytvoření** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvořit web**.

    > [!NOTE]
    > Azure je hostitel pro webovou aplikaci spuštěnou v cloudu, který může řídit a spravovat. Možnost rychle vytvořit můžete nasadit hotové webové aplikace do Azure z mimo portál. Nezahrnuje kroky pro vytvoření databáze.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](build-restful-apis-with-aspnet-web-api/_static/image41.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** se vytvoří.
5. Po vytvoření webové stránky, klikněte na odkaz v části **URL** sloupce. Zkontrolujte, jestli funguje nový web.

    ![Na nový web](build-restful-apis-with-aspnet-web-api/_static/image42.png "přechodu na nový web")

    *Procházení na nový web*

    ![Spuštění webu](build-restful-apis-with-aspnet-web-api/_static/image43.png "spuštění webové stránky")

    *Spuštění webové stránky*
6. Přejděte zpět na portál a klikněte na název webové stránky v části **název** sloupec, který se zobrazí na stránkách správy.

    ![Otevřete správu webových stránek](build-restful-apis-with-aspnet-web-api/_static/image44.png "otevřete správu webových stránek")

    *Otevřete správu webových stránek*
7. V **řídicí panel** stránce v části **rychlý přehled** klikněte na tlačítko **stáhnout profil publikování** odkaz.

    > [!NOTE]
    > *Profil publikování* obsahuje všechny informace požadované pro publikování webových aplikací do Azure pro každou metodu povoleno publikování. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a databázové řetězce požadované k připojení a ověřování na základě jednotlivých koncových bodů, u kterých je povolena metoda publikace. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** a **Microsoft Visual Studio 2012** podporují čtení publikační profily k automatizaci konfigurace z těchto programů pro publikování webových aplikací do Azure.

    ![Stahování webové stránky publikovat profil](build-restful-apis-with-aspnet-web-api/_static/image45.png "stahování webové stránky profil publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profilu publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak publikovat webovou aplikaci do Azure ze sady Visual Studio pomocí tohoto souboru.

    ![Ukládání souboru profilu publikování](build-restful-apis-with-aspnet-web-api/_static/image46.png "ukládá se profil publikování")

    *Ukládá se profil publikování*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá SQL Server databáze, budete muset vytvořit server služby SQL Database. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá SQL Server může tuto úlohu přeskočit.

1. Pro uložení databáze aplikace budete potřebovat databázi SQL serveru. Servery SQL Database můžete zobrazit ze svého předplatného na portálu Azure Management portal na **databází Sql** | **servery** | **řídicího panelu serveru**. Pokud nemáte server vytvořili, můžete vytvořit jednu **přidat** tlačítko na panelu příkazů. Poznamenejte si **název serveru a adresu URL, správce přihlašovací jméno a heslo**, jak je použijete v další úkoly. Nevytvářet databáze, protože vytvoří se v pozdější fázi.

    ![Řídicí panel serveru SQL Database](build-restful-apis-with-aspnet-web-api/_static/image47.png "řídicího panelu serveru SQL Database")

    *Řídicí panel serveru SQL Database*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio z tohoto důvodu je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. Chcete-li to mohli udělat, klikněte na tlačítko **konfigurovat**, vyberte IP adresu z **aktuální IP adresa klienta** a vložte ho na **počáteční IP adresa** a **Koncová IP adresa** textová pole a klikněte na tlačítko ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) tlačítko.

    ![Přidat IP adresu klienta](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Přidat IP adresu klienta*
3. Jednou **IP adresa klienta** je přidat do povolených IP adres klikněte na tlačítko na **Uložit** potvrďte provedené změny.

    ![Potvrzení změn](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Potvrzení změn*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nasazení webu

1. Vraťte se do řešení ASP.NET MVC 4. V **Průzkumníka řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](build-restful-apis-with-aspnet-web-api/_static/image51.png "publikování aplikace")

    *Publikování na webu*
2. Importujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](build-restful-apis-with-aspnet-web-api/_static/image52.png "import profilu publikování")

    *Import publikačního profilu*
3. Klikněte na tlačítko **ověřit připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření bylo dokončeno, jakmile se zobrazí zelené zaškrtnutí vedle tlačítka ověřit připojení.

    ![Ověřuje se připojení](build-restful-apis-with-aspnet-web-api/_static/image53.png "ověřuje se připojení")

    *Ověřuje se připojení*
4. V **nastavení** stránce v části **databází** klikněte na tlačítko vedle textového pole připojení k databázi (to znamená **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](build-restful-apis-with-aspnet-web-api/_static/image54.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Konfigurace připojení k databázi následujícím způsobem:

   - V **název serveru** zadejte vaše pomocí adresy URL databáze SQL serveru *tcp:* předponu.
   - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
   - V **heslo** zadejte heslo pro přihlašovací jméno správce serveru.
   - Zadejte nový název databáze, například: *MVC4SampleDB*.

     ![Konfigurace cílový připojovací řetězec](build-restful-apis-with-aspnet-web-api/_static/image55.png "konfigurace cílový připojovací řetězec")

     *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze klikněte na **Ano**.

    ![Vytvoření databáze](build-restful-apis-with-aspnet-web-api/_static/image56.png "vytvoření řetězce databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL ve službě Windows Azure se zobrazí v textovém poli výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "připojovací řetězec odkazující na SQL Database")

    *Připojovací řetězec odkazující na SQL Database*
8. V **ve verzi Preview** klikněte na **publikovat**.

    ![Publikování webové aplikace](build-restful-apis-with-aspnet-web-api/_static/image58.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Až se proces publikování dokončí, otevře se váš výchozí prohlížeč publikovaného webu.

    ![Publikování aplikace do Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "publikování aplikace do Windows Azure")

    *Aplikace publikovaná do Azure*
