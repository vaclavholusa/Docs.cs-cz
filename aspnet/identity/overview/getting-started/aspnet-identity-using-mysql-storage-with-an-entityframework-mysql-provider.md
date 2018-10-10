---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: Použití úložiště MySQL zprostředkovatele EntityFramework MySQL (C#) | Dokumentace Microsoftu'
author: maumar
description: V tomto kurzu se dozvíte, jak nahradit výchozí mechanismus úložiště dat pro ASP.NET Identity objektu EntityFramework (zprostředkovatel SQL klient) s MySQL zajištění...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: f510c9bcaf83c6a68e835a7d82555653459df856
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912368"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: Použití úložiště MySQL zprostředkovatele EntityFramework MySQL (C#)
====================
podle [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> V tomto kurzu se dozvíte, jak nahradit výchozí mechanismem úložiště dat pro [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) s s MySQL zprostředkovatele EntityFramework (zprostředkovatel SQL klient).


V následujících tématech se budeme v tomto kurzu:

- Vytváří se databáze MySQL v Azure
- Vytvoření aplikace MVC pomocí šablony Visual Studio 2013 MVC
- Konfigurace objektu EntityFramework pro práci s poskytovatele databáze MySQL
- Spuštění aplikace ověřit výsledky

Na konci tohoto kurzu budete mít aplikace MVC s ASP.NET Identity ukládat, která používá databázi MySQL, který je hostován v Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Vytvoření instance databáze MySQL v Azure

1. Přihlaste se k [webu Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Klikněte na tlačítko **nový** v dolní části stránky a pak vyberte **úložiště**:

    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. V **zvolit a doplněk** průvodce, vyberte **databázi ClearDB MySQL**a potom klikněte na tlačítko **Další** šipky v dolní části rámce:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Ponechte výchozí **Free** plánovat, změnit **název** k **IdentityMySQLDatabase**, vyberte oblast, která je vám nejblíže. a poté klikněte na tlačítko **Další** šipky v dolní části rámce:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Klikněte na tlačítko **nákupní** značku zaškrtnutí dokončete vytváření databáze.

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Po vytvoření vaší databáze ji můžete spravovat z **doplňky** karta na portálu management portal. Pokud chcete načíst informace o připojení pro vaši databázi, klikněte na tlačítko **informace o připojení** v dolní části stránky:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Zkopírujte připojovací řetězec po kliknutí na tlačítko kopírování podle **CONNECTIONSTRING** pole a uložte ho; tyto informace dále v tomto kurzu budete používat pro aplikaci MVC:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Vytvoření projektu aplikace MVC

K dokončení kroků v této části kurzu, budete nejprve muset nainstalovat [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Po instalaci sady Visual Studio, použijte následující postup k vytvoření nového projektu aplikace MVC:

1. Otevřete Visual Studio 2103.
2. Klikněte na tlačítko **nový projekt** z **Start** stránky, nebo můžete kliknout na **souboru** nabídky a pak **nový projekt**:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Při **nový projekt** se zobrazí dialogové okno, rozbalte **Visual C#** v seznamu šablon klikněte **webové**a vyberte **WebováaplikaceASP.NET**. Pojmenujte svůj projekt **IdentityMySQLDemo** a potom klikněte na tlačítko **OK**:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. V **nový projekt ASP.NET** dialogového okna, vyberte **MVC** šablonyPomocí výchozí možnosti; tím se konfigurace **jednotlivé uživatelské účty** jako metodu ověřování. Klikněte na tlačítko **OK**:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Konfigurace objektu EntityFramework pro práci s databází MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Aktualizovat Entity Framework sestavení pro projekt

Aplikace MVC, který byl vytvořen z šablony sady Visual Studio 2013 obsahuje odkaz na [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) balíček, ale musí se na toto sestavení od jeho vydání aktualizace, které obsahují důležité vylepšení výkonu. Chcete-li použít tyto nejnovější aktualizace ve vaší aplikaci, použijte následující postup.

1. Otevřete svůj projekt MVC v sadě Visual Studio.
2. Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Správce balíčků NuGet**a potom klikněte na tlačítko **Konzola správce balíčků**:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. **Konzola správce balíčků** se zobrazí v dolní části sady Visual Studio. Typ &quot; **Update-Package EntityFramework** &quot; a stiskněte klávesu Enter:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Nainstalovat MySQL zprostředkovatele EntityFramework

Aby EntityFramework pro připojení k databázi MySQL budete muset nainstalovat MySQL zprostředkovatele. Chcete-li to provést, otevřete **Konzola správce balíčků** a typ &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, a potom stiskněte klávesu Enter.

> [!NOTE]
> Toto je předběžná verze sestavení, a proto může obsahovat chyby. Předběžná verze zprostředkovatele byste neměli používat v produkčním prostředí.


[Klikněte na tlačítko se rozbalí na následujícím obrázku.]

[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Provádění změn konfigurace projektu v souboru Web.config pro aplikaci

V této části budete konfigurovat Entity Framework pro použití MySQL poskytovatele, který jste právě nainstalovali, se zaregistrovat objekt pro vytváření zprostředkovatele MySQL a přidejte váš připojovací řetězec z Azure.

> [!NOTE]
> Následující příklady obsahují konkrétní sestavení verze pro MySql.Data.dll. Pokud se změní verze sestavení, musíte změnit nastavení odpovídající konfigurace se správnou verzí.


1. Otevřete soubor Web.config pro váš projekt v sadě Visual Studio 2013.
2. Vyhledejte následující nastavení konfigurace, které definují výchozí databáze zprostředkovatelů a objektů factory pro Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Nahraďte následující příkaz, který nakonfiguruje Entity Framework MySQL poskytovatel se použije tato nastavení konfigurace:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Vyhledejte &lt;connectionStrings&gt; části a nahraďte ho následujícím kódem, který bude definování připojovacího řetězce pro databázi MySQL, který je hostovaný v Azure (Všimněte si, že hodnota providerName také se změnil z původní):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Přidání vlastní místní MigrationHistory

Používá Entity Framework Code First **MigrationHistory** tabulce ke sledování změny modelu a zajistit konzistenci mezi schéma databáze a konceptuální schéma. Ale tato tabulka nefunguje pro MySQL ve výchozím nastavení protože primární klíč je moc velká. Chcete tuto situaci napravit, musíte zmenšit velikost klíče pro tuto tabulku. Chcete-li to provést, postupujte následovně:

1. Jsou zachyceny informace o schématu pro tuto tabulku **HistoryContext**, což může být změněna jako jakýkoli jiný **DbContext**. Uděláte to tak, přidejte nový soubor třídy s názvem **MySqlHistoryContext.cs** do projektu a nahraďte jeho obsah následujícím kódem:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Dále budete muset nakonfigurovat rozhraní Entity Framework pomocí upravené **HistoryContext**, namísto výchozí hodnotu. To můžete udělat pomocí možnosti konfigurace založená na kódu. To uděláte tak, přidáte nový soubor třídy s názvem **MySqlConfiguration.cs** do vašeho projektu a nahraďte jeho obsah se:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Vytváření vlastních inicializátor objektu EntityFramework pro ApplicationDbContext

MySQL zprostředkovatele, který je vybrané v tomto kurzu aktuálně nepodporuje migrace Entity Framework, tak budete muset použít model inicializátory abyste se mohli připojit k databázi. Protože v tomto kurzu používá instanci MySQL v Azure, musíte vytvořit vlastní Entity Framework inicializátor.

> [!NOTE]
> Tento krok není povinný, pokud se chcete připojit k instanci systému SQL Server v Azure nebo pokud používáte databázi, která je hostovaná v místním prostředí.


Pokud chcete vytvořit vlastní Entity Framework inicializátor pro MySQL, postupujte následovně:

1. Přidejte nový soubor třídy s názvem **MySqlInitializer.cs** je do projektu a nahraďte obsah následujícím kódem:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Otevřít **IdentityModels.cs** souboru projektu, který je umístěn v **modely** adresáře a nahraďte jeho obsah následujícím kódem:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Spuštění aplikace a ověřování databáze

Po dokončení kroků v předchozí části, měli byste otestovat vaši databázi. Chcete-li to provést, postupujte následovně:

1. Stisknutím klávesy **Ctrl + F5** sestavíte a spustíte webovou aplikaci.
2. Klikněte na tlačítko **zaregistrovat** kartě v horní části stránky:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Zadejte nové uživatelské jméno a heslo a potom klikněte na tlačítko **zaregistrovat**:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. V tomto okamžiku se vytvoří ASP.NET Identity tabulky v databázi MySQL a je uživatel zaregistrován a přihlášení do aplikace:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Instalace nástroje MySQL Workbench pro ověření dat

1. Nainstalujte **aplikace MySQL Workbench** nástroj z [stránky pro MySQL stažení](http://dev.mysql.com/downloads/windows/installer/)
2. V Průvodci instalací: **výběr součástí** kartu, vyberte možnost **aplikace MySQL Workbench** pod **aplikací** oddílu.
3. Spusťte aplikaci a přidejte nové připojení pomocí připojení řetězcovými daty z databáze Azure MySQL, kterou jste vytvořili v účelem úspěchu při žebrání tohoto kurzu.
4. Po navázání připojení, zkontrolujte **ASP.NET Identity** tabulky vytvořené na **IdentityMySQLDatabase.**
5. Zobrazí se, že všechny ASP.NET Identity požadované tabulky vytvářejí, jak je znázorněno na následujícím obrázku:

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Zkontrolujte **aspnetusers** tabulky pro instanci Hledat položky při registraci nového uživatele.

   [Klepnutím rozbalte následující obrázek. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
