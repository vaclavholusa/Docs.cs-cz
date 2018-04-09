---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Vývoj aplikací ASP.NET se službou Azure Active Directory | Microsoft Docs
author: Rick-Anderson
description: Nástroje Microsoft ASP.NET tools pro službu Azure Active Directory zjednodušuje kvůli povolení ověřování pro webové aplikace hostované v Azure. Můžete použít Azure Authenti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 44bf29e099583bf9d49f2715d3ff4f748728ad8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Vývoj aplikací ASP.NET se službou Azure Active Directory
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> Nástroje Microsoft ASP.NET pro Azure Active Directory usnadňuje povolení ověřování pro webové aplikace hostované na [Azure](https://www.windowsazure.com/home/features/web-sites/). Ověřování Azure můžete použít k ověření uživatele služeb Office 365 z vaší organizace, podnikové účty synchronizované z vaší místní službou Active Directory nebo uživatelé vytvoření ve vlastní domény Azure Active Directory. Povolení ověřování systému Windows Azure nakonfiguruje aplikace k ověření uživatelů pomocí jedné [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) klienta.
> 
>  V tomto kurzu napsal Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)


Tento kurz vám ukáže, jak vytvořit aplikaci ASP.NET, který je nakonfigurován pro přihlašování pomocí [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Také se naučíte, jak zavolat rozhraní Graph API k načtení informací o aktuálně přihlášeného uživatele a jak nasadit aplikaci do Azure.

## <a name="prerequisites"></a>Požadavky

1. [Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) nebo [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -je požadována aktualizace 3 nebo vyšší.
3. Účet Azure. [Kliknutím sem](https://azure.microsoft.com/pricing/free-trial/) bezplatnou zkušební verzi, pokud již nemáte účet.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Přidejte globální správce do služby Active Directory

1. Přihlaste se k [portál pro správu Azure](https://manage.windowsazure.com/).
2. Obsahuje všechny účty Azure **výchozí adresář** – klikněte na něj a pak klikněte na tlačítko **uživatelé** v horní části stránky (viz následující obrázek).
3. Klikněte na tlačítko Přidat uživatele.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Vytvoření nového uživatele pomocí **globálního správce** role. Klikněte na tlačítko **uživatelé** z hlavní nabídky a klikněte **přidat uživatele** tlačítka na panelu příkazů.
5. V **přidat uživatele** dialogové okno, zadejte název pro nového uživatele a pak klikněte na šipku vpravo.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Zadejte uživatelské jméno a nastavte roli **globálního správce**. Globální správci potřebují alternativní e-mailovou adresu pro účely obnovení hesla. Jakmile budete hotovi, klikněte na šipku vpravo.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Na další stránce dialogového okna, klikněte na tlačítko **vytvořit**. Dočasné heslo se vytvoří pro nového uživatele a zobrazí v dialogovém okně.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
   Uložit heslo budou muset změnit heslo po prvním přihlášení. Následující obrázek ukazuje nový účet správce. Azure Active Directory musíte použít k přihlášení do aplikace, není k účtu Microsoft, které jsou také uvedené na této stránce.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Vytvoření aplikace ASP.NET

Následující postup použijte [Visual Studio Express 2013 pro Web](https://www.microsoft.com/download/details.aspx?id=40747)a vyžaduje [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. V sadě Visual Studio, klikněte na tlačítko **soubor** a potom **nový projekt**. Na **nový projekt** dialogové okno, vyberte v levé nabídce projektu Visual C# webovou a klikněte na tlačítko **OK**. Můžete také zrušte zaškrtnutí políčka **přidat službu Application Insights do projektu** Pokud nechcete, aby funkce pro vaši aplikaci.
2. V **nový projekt ASP.NET** dialogovém okně, vyberte **MVC**a potom klikněte na **změna ověřování**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Na **změna ověřování** dialogovém okně, vyberte **účty organizace**. Tyto možnosti lze automaticky registrace vaší aplikace s Azure AD a také automaticky konfigurace aplikace pro integraci se službou Azure AD. Nemusíte používat **změna ověřování** dialogovém okně můžete zaregistrovat a nakonfigurovat aplikaci, ale je mnohem jednodušší. Pokud například používáte sadu Visual Studio 2012, můžete pořád spustit ručně zaregistrovat aplikaci v portálu pro správu Azure a aktualizaci konfigurace pro integraci s Azure AD.  
   V rozevíracích nabídek vyberte **cloudu – jednotný** a **jednotné přihlašování, čtení dat adresáře**. Zadejte doménu pro váš adresář Azure AD, například (v obrázcích níže) *aricka0yahoo.onmicrosoft.com*a potom klikněte na **OK**. Název domény můžete získat z karty domén pro výchozí adresář na portálu azure (viz další image dolů).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
   Následující obrázek zobrazuje název domény z portálu Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > Volitelně můžete nakonfigurovat identifikátor ID URI aplikace, které se zaregistruje ve službě Azure AD kliknutím **další možnosti**. Identifikátor ID URI aplikace je jedinečný identifikátor pro aplikaci, která je registrovaná v Azure AD a používá je aplikace identifikovat při komunikaci se službou Azure AD. Další informace o identifikátor ID URI aplikace a další vlastnosti registrovaných aplikací najdete v tématu [v tomto tématu](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Kliknutím na zaškrtávací políčko níže pole identifikátor ID URI aplikace také můžete přepsat existující registraci ve službě Azure AD, který používá stejný identifikátor ID URI aplikace.
4. Po kliknutí na **OK**, zobrazí se dialogové okno přihlášení a budete se muset přihlásit pomocí účtu globálního správce (není účet Microsoft spojené s vaším předplatným). Pokud jste dříve vytvořili nový účet správce, bude nutné změnit heslo a potom znovu přihlásit pomocí nového hesla.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Poté, co jste úspěšně ověřen, **nový projekt ASP.NET** dialogové okno se zobrazí zvoleného ověřování (**organizační** ) a zaregistrována (adresáři,kdebudounováaplikace*aricka0yahoo.onmicrosoft.com* na obrázku níže). Pod tyto informace, zaškrtněte políčko s názvem bez přípony **hostitel v cloudu**. Pokud je toto políčko zaškrtnuto, projektu, se zřídí jako webové aplikace Azure a budou povolené pro snadné publikování později. Click **OK**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. **Webu Azure konfigurovat** zobrazí se dialogové okno, použití název lokality automaticky generovaný a oblast. Všimněte si také účet, který jste aktuálně přihlášeni v dialogovém okně. Chcete-li zajistit, že tento účet je ten, který je připojen vašeho předplatného Azure, obvykle účet Microsoft.

    > [!NOTE]
    > Tento projekt vyžaduje databázi. Musíte vybrat jednu ze stávajících databází nebo vytvořte novou. Databáze není nutná, protože projekt již používá soubor místní databázi k ukládání malé množství dat konfigurace ověřování. Při nasazení aplikace na web Azure, tato databáze není zabalené s nasazením, proto musíte vybrat jednu, která je přístupná v cloudu. Click **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Vytvoří projekt, a možnosti ověřování a možnosti webové aplikace, budou automaticky nakonfigurováni projektu. Po dokončení tohoto procesu spusťte projekt lokálně stisknutím **^ F5**. Bude nutné se přihlásit pomocí účtu organizace. Zadejte uživatelské jméno a heslo pro účet, který jste dříve vytvořili a klikněte na **přihlášení**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Po úspěšném přihlášení se zobrazí webu ASP.NET, že jste ověřována zobrazení uživatelského jména v pravém horním rohu stránky.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
   Pokud dojde k chybě:  
   Hodnota nemůže být null nebo prázdný. Název parametru: linkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
   najdete v článku [ladění](#dbg) části na konci tohoto kurzu.

## <a name="basics-of-the-graph-api"></a>Základní informace o Graph API

[Rozhraní Graph API](https://msdn.microsoft.com/library/azure/hh974476.aspx) je programovací rozhraní používá k provádění CRUD a další operace na objektech v adresáři služby Azure AD. Pokud vyberete možnost účtu organizace pro ověřování při vytváření nového projektu v sadě Visual Studio 2013, vaše aplikace už nakonfigurovaná k volání rozhraní Graph API. Tato část stručně ukazuje, jak funguje rozhraní Graph API.

1. V běžící aplikaci, klikněte na název přihlášeného uživatele v horní pravé části stránky. Tím přejdete na stránku profilu uživatele, který je akce na řadiči Domů. Můžete si všimnout, že tabulka obsahuje informace o uživateli o účet správce, že jste vytvořili dříve. Tyto informace jsou uloženy ve vašem adresáři a rozhraní Graph API je volána pro načtení těchto informací při načtení stránky.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Přejděte zpět do Visual Studio a rozbalte **řadiče** složku a potom otevřete **HomeController.cs** souboru. Zobrazí se **UserProfile()** akci, která obsahuje kód pro načtení tokenu a poté zavolat rozhraní Graph API. Tento kód je duplikována níže: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    K volání rozhraní Graph API, musíte nejdřív načíst token. Pokud je token načteny, je nutno přidat jeho řetězcová hodnota v hlavičce autorizace pro všechny následné požadavky na rozhraní Graph API. Většinu kódu výše zpracovává podrobnosti o ověřování pro službu Azure AD se získat token, pomocí tokenu pro volání pro rozhraní Graph API a pak transformace odpovědi tak, aby může být uvedené v zobrazení.

    Následující zvýrazněný řádek je nejdůležitější část diskusi: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Tento řádek představuje jméno uživatele, který má byla v odpovědi JSON deserializovat a je uvedené v zobrazení.

    Můžete volat rozhraní Graph API pomocí položky HttpClient a zpracování nezpracovaných dat sami, ale snadnější je použití [grafu klientské knihovny, která je k dispozici prostřednictvím balíčku NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). Klientská knihovna zpracuje nezpracovaná požadavků HTTP a transformaci vrácená data pro vás a výrazně usnadňuje práci s rozhraní Graph API v prostředí .NET. Zobrazit související ukázky kódu rozhraní Graph API na [Githubu.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Nasaďte aplikaci do Azure

Následující kroky ukazují postup nasazení aplikace do Azure. V dřívějších krocích nový projekt připojený s webovou aplikaci v Azure, takže je připravena k publikování v několika krocích.

1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt a vyberte **publikovat**. **Publikovat Web** zobrazí se dialogové okno s každé nastavení již nakonfigurována. Klikněte na **Další** tlačítko přejdete k **nastavení** stránky. Můžete být vyzváni k ověření; Zajistěte, aby že ověření pomocí účtu předplatného Azure (obvykle účet Microsoft) a ne organizační účet, který jste vytvořili dříve.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Zkontrolujte **povolit ověřování organizace** možnost. V **domény** pole, zadejte doménu pro váš adresář. Z **úroveň přístupu** rozevíracího seznamu, vyberte **jednotné přihlašování, čtení dat adresáře**. Můžete si všimnout, že je v již zadána předchozí databáze, které jste použili **databáze** části. Klikněte na tlačítko **publikování**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio zahájíte nasazení webu a potom zobrazí se nové okno prohlížeče. Zobrazí se výzva k ověření do vašeho adresáře ještě jednou. Jakmile jste ověření budete přesměrováni na přehled nově publikovaných webu v Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Ladění aplikace

Pokud dojde k následující chybě:   
 Hodnota nemůže být null nebo prázdný. Název parametru: linkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Nahraďte kód v *Views\Shared\\_LoginPartial.cshtml* soubor s následující:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Po spuštění aplikace, pokud má přihlášený uživatel zobrazuje "Null uživatel", odhlásit se a přihlaste zpět pomocí účtu služby Active Directory, kterou jste vytvořili dříve.

Vynikající kurz a postupujte podle je Rick Rainey [podrobné informace: weby Azure a organizační ověřování pomocí služby Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Další informace

- [Podrobné informace: Weby Azure a organizační ověřování pomocí služby Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Přehled rozhraní API služby Azure AD Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Scénáře ověřování ve službě Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Ukázky kódu Azure AD na Githubu](https://github.com/AzureADSamples)
