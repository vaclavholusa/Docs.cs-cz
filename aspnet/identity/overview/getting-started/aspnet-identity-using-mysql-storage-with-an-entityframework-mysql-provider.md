---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: Pomocí MySQL úložiště pomocí zprostředkovatele MySQL EntityFramework (C#) | Microsoft Docs'
author: maumar
description: V tomto kurzu se dozvíte, jak nahradit výchozího mechanismu úložiště dat pro ASP.NET Identity EntityFramework (zprostředkovatel SQL klienta) s MySQL zajištění...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6018b4f62f95f9abffece536f345d7a16d052aac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873388"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: Pomocí MySQL úložiště pomocí zprostředkovatele EntityFramework MySQL (C#)
====================
podle [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Roberta Mcmurrayho](https://github.com/rmcmurray)

> V tomto kurzu se dozvíte, jak nahradit výchozího mechanismu úložiště dat pro [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) s EntityFramework (zprostředkovatel SQL klienta) s poskytovatelem MySQL.


V následujících tématech se budeme v tomto kurzu:

- Vytvoření databáze MySQL na Azure
- Vytvoření aplikace MVC pomocí šablony Visual Studio 2013 MVC
- Konfigurace objektu EntityFramework pro práci s poskytovatele databáze MySQL
- Spuštění aplikace ověření výsledků

Na konci tohoto kurzu budete mít aplikace MVC pomocí ASP.NET Identity úložiště používající databázi MySQL, která je hostovaná v Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Vytvoření instance databáze MySQL na Azure

1. Přihlaste se k [portál Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Klikněte na tlačítko **nový** v dolní části stránky a potom vyberte **úložiště**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. V **zvolte a rozšíření** průvodce, vyberte **databáze MySQL ClearDB**a pak klikněte na tlačítko **Další** šipky v dolní části rámečku:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Ponechte výchozí **volné** plánování, změnit **název** k **IdentityMySQLDatabase**, vyberte oblast, která je nejblíže je a pak klikněte na tlačítko **Další** šipku v dolní části rámečku:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Klikněte **nákupu** zaškrtnutí vytvoření databáze.  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Po vytvoření databáze, můžete spravovat z **doplňky** kartě v portálu pro správu. Chcete-li načíst informace o připojení pro vaši databázi, klikněte na tlačítko **informace o připojení** v dolní části stránky:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Zkopírujte připojovací řetězec kliknutím na tlačítko Kopírovat, pokud pomocí **CONNECTIONSTRING** pole a uložit ho; tyto informace dále v tomto kurzu budete používat pro aplikace MVC:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Vytvoření projektu aplikace MVC

Pokud chcete provést kroky v této části kurzu, bude nejprve musíte nainstalovat [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Po instalaci sady Visual Studio vytvořte nový projekt aplikace MVC pomocí následujících kroků:

1. Open Visual Studio 2103.
2. Klikněte na tlačítko **nový projekt** z **spustit** stránky, nebo můžete kliknout na tlačítko **soubor** nabídce a potom **nový projekt**:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Když **nový projekt** se zobrazí dialogové okno, rozbalte položku **Visual C#** v seznamu šablon, pak klikněte na **webové**a vyberte **webové aplikace ASP.NET**. Název projektu **IdentityMySQLDemo** a pak klikněte na **OK**:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. V **nový projekt ASP.NET** dialogovém okně, vyberte **MVC** šablonyPomocí výchozí možnosti; tím se konfigurace **jednotlivé uživatelské účty** jako metodu ověřování. Klikněte na tlačítko **OK**:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Konfigurace objektu EntityFramework pro práci s databázi MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Aktualizace sestavení Entity Framework pro váš projekt

Aplikace MVC, který byl vytvořen z šablony sady Visual Studio 2013 obsahuje odkaz na [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) balíček, ale musí byla aktualizace do tohoto sestavení od jeho vydání, které obsahují důležité vylepšení výkonu. Chcete-li použít tyto nejnovější aktualizace ve vaší aplikaci, použijte následující kroky.

1. Otevřete projekt MVC v sadě Visual Studio 2013.
2. Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Správce balíčků knihoven**a potom klikněte na **Konzola správce balíčků**:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. **Konzola správce balíčků** se zobrazí v dolní části sady Visual Studio. Typ &quot; **balíček aktualizace EntityFramework** &quot; a stiskněte klávesu Enter:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Nainstalujte zprostředkovatele MySQL pro EntityFramework

Aby EntityFramework pro připojení k databázi MySQL budete muset nainstalovat poskytovatele MySQL. Chcete-li to provést, otevřete **Konzola správce balíčků** a typ &quot; **MySql.Data.Entity Install-Package - předběžné**&quot;, a stiskněte klávesu Enter.

> [!NOTE]
> Toto je předběžná verze sestavení a jako takový ho může obsahovat chyby. Předběžná verze zprostředkovatele byste neměli používat v produkčním prostředí.


[Kliknutím na následující obrázek a rozbalte ji.]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Provádění změn konfigurace projektu v souboru Web.config vaší aplikace

V této části nakonfigurujete rozhraní Entity Framework pro použití poskytovatele MySQL, který jste právě nainstalovali zaregistrujte objekt pro vytváření zprostředkovatelů MySQL a přidejte připojovací řetězec z Azure.

> [!NOTE]
> Následující příklady obsahují verzi konkrétní sestavení pro MySql.Data.dll. Pokud se změní verze sestavení, musíte změnit nastavení odpovídající konfiguraci se správnou verzí.


1. Otevřete soubor Web.config pro projekt v sadě Visual Studio 2013.
2. Vyhledejte následující nastavení konfigurace, které definují výchozí zprostředkovatel databáze a objektu pro vytváření rozhraní Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Nahraďte následující text, který nakonfiguruje rozhraní Entity Framework pro použití poskytovatele MySQL těchto nastavení: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Vyhledejte &lt;connectionStrings&gt; části a nahraďte ji následujícím kódem, který definuje připojovací řetězec pro vaši databázi MySQL, která je hostovaná v Azure (Všimněte si, že hodnota providerName také byla změněna z hodnoty původní):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Přidání vlastních MigrationHistory kontextu

Entity Framework Code First používá **MigrationHistory** tabulky ke sledování změn modelu a zajistit konzistenci mezi schéma databáze a konceptuálního schématu. Ale v této tabulce nefunguje pro databázi MySQL ve výchozím nastavení protože primární klíč je příliš velká. Chcete-li opravit tuto situaci, musíte zmenšit velikost klíče pro tuto tabulku. Uděláte to tak, použijte následující postup:

1. Informace o schématu pro tuto tabulku je zachycené v **HistoryContext**, což může být upravena jako libovolný jiný **DbContext**. Uděláte to tak, přidejte nový soubor třídy s názvem **MySqlHistoryContext.cs** do projektu a nahraďte jeho obsah následujícím kódem:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Potom budete muset nakonfigurovat rozhraní Entity Framework použít upravenou **HistoryContext**, namísto výchozí nastavení. Tento krok můžete provést s využitím funkce konfigurace založené na kódu. Uděláte to tak, přidejte nový soubor třídy s názvem **MySqlConfiguration.cs** do projektu a nahraďte jeho obsah s:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Vytváření vlastních inicializátoru objektu EntityFramework pro ApplicationDbContext

MySQL zprostředkovatele, který bude uvedena v tomto kurzu aktuálně nepodporuje migraci Entity Framework, takže budete muset použít model inicializátory připojit k databázi. Protože v tomto kurzu je pomocí instance databáze MySQL na Azure, musíte vytvořit vlastní inicializátoru Entity Framework.

> [!NOTE]
> Tento krok není vyžaduje, pokud se připojujete k instanci systému SQL Server v Azure nebo pokud používáte databázi, která je hostovaná místně.


Chcete-li vytvořit vlastní inicializátoru Entity Framework pro databázi MySQL, použijte následující kroky:

1. Přidat nový soubor třídy s názvem **MySqlInitializer.cs** projektu a nahraďte je obsah následujícím kódem: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Otevřete **IdentityModels.cs** soubor pro svůj projekt, který je umístěný ve **modely** adresář a nahraďte jeho obsah následujícím textem: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Spuštění aplikace a ověřování databáze

Po dokončení kroků v předchozí části, měli byste otestovat vaši databázi. Uděláte to tak, použijte následující postup:

1. Stiskněte klávesu **kombinaci kláves Ctrl + F5** sestavení a spuštění webové aplikace.
2. Klikněte **zaregistrovat** karty v horní části stránky:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Zadejte nové uživatelské jméno a heslo a pak klikněte na tlačítko **zaregistrovat**:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. V tomto okamžiku jsou vytvořeny ASP.NET Identity tabulky v databázi MySQL, a uživatel je zaregistrován a přihlášení do aplikace:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Instalace nástroje MySQL Workbench ověřit data

1. Nainstalujte **MySQL Workbench** nástroje z [MySQL stáhne stránky](http://dev.mysql.com/downloads/windows/installer/)
2. V Průvodci instalací: **výběr funkce** vyberte **MySQL Workbench** pod **aplikace** části.
3. Spusťte aplikaci a přidejte nové připojení pomocí připojení řetězcovými daty z databáze MySQL na Azure, kterou jste vytvořili v účelem úspěchu při žebrání tohoto kurzu.
4. Po navázání připojení, zkontrolujte **ASP.NET Identity** tabulky na vytvořit **IdentityMySQLDatabase.**
5. Zobrazí se, že všechny ASP.NET Identity požadované tabulky se vytváří, jak je znázorněno na obrázku níže:  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Zkontrolujte **aspnetusers** tabulky pro instanci zkontrolujte položky při registraci nového uživatele.  
  
   [Kliknutím na následující obrázek, rozbalte ho. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
