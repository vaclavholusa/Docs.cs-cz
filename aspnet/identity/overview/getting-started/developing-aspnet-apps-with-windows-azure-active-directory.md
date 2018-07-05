---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Vývoj aplikací ASP.NET pomocí Azure Active Directory | Dokumentace Microsoftu
author: Rick-Anderson
description: Nástroje Microsoft ASP.NET pro Azure Active Directory umožňuje snadno zajistit ověřování pro webové aplikace hostované v Azure. Můžete použít Azure Authenti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: ''
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 296c897887ea55a2e786d0229b0766e608e4edfb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388277"
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Vývoj aplikací ASP.NET pomocí Azure Active Directory
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

Microsoft ASP.NET tools pro Azure Active Directory zjednodušuje povolení ověřování pro webové aplikace hostované na [Azure](https://www.windowsazure.com/home/features/web-sites/). Ověřování Azure můžete použít k ověření uživatelů Office 365 z vaší organizace, podnikové účty synchronizované z vaší místní Active Directory nebo uživatelé vytvoření ve vlastní doméně Azure Active Directory. Když se povolí ověřování Windows Azure nakonfiguruje vaše aplikace k ověřování uživatelů pomocí jediného [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) tenanta.

Tomto kurzu se dozvíte, jak vytvořit aplikaci ASP.NET, který je nakonfigurovaný pro přihlašování pomocí [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Dozvíte se taky, jak voláním rozhraní Graph API a získat informace o aktuálně přihlášeného uživatele a jak nasadit aplikaci do Azure.

## <a name="prerequisites"></a>Požadavky

1. [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) nebo [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) – s aktualizací Update 3 nebo vyšší se vyžaduje.
3. Účet Azure. [Kliknutím sem](https://azure.microsoft.com/pricing/free-trial/) bezplatné zkušební verzi, pokud ještě nemáte účet.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Přidání globální správce služby Active Directory

1. Přihlaste se k [portál pro správu Azure](https://manage.windowsazure.com/).
2. Obsahuje všechny účty v Azure **výchozí adresář** – klikněte na něj a potom klikněte na **uživatelé** kartě v horní části stránky (viz obrázek níže).
3. Klikněte na Přidat uživatele.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Vytvoření nového uživatele s **globálního správce** role. Klikněte na tlačítko **uživatelé** z hlavní nabídky a pak klikněte na tlačítko **přidat uživatele** tlačítko na panelu příkazů.
5. V **přidat uživatele** dialogové okno, zadejte název pro nové uživatele a potom klikněte na šipku vpravo.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Zadejte uživatelské jméno a nastavte roli na **globálního správce**. Globální správci vyžadují alternativní e-mailovou adresu pro účely obnovení hesla. Jakmile budete hotovi, klikněte na šipku vpravo.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Na další stránce dialogového okna, klikněte na tlačítko **vytvořit**. Dočasné heslo, bude vytvořena pro nového uživatele a zobrazí v dialogovém okně.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
   Uložit heslo se bude vyžadovat změnu hesla po prvním přihlášení. Následující obrázek ukazuje nového účtu správce. Azure Active Directory musíte použít k přihlášení do vaší aplikace, ne účet Microsoft zobrazen také na této stránce.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Vytvoření aplikace ASP.NET

Následující kroky použijte [Visual Studio Express 2013 for Web](https://www.microsoft.com/download/details.aspx?id=40747)a vyžaduje [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. V sadě Visual Studio, klikněte na tlačítko **souboru** a potom **nový projekt**. Na **nový projekt** dialogové okno, vyberte v levé nabídce projektu Visual C# Web a klikněte na tlačítko **OK**. Můžete také zrušit zaškrtnutí **přidat službu Application Insights do projektu** Pokud nechcete funkci pro vaši aplikaci.
2. V **nový projekt ASP.NET** dialogového okna, vyberte **MVC**a potom klikněte na tlačítko **změna ověřování**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Na **změna ověřování** dialogového okna, vyberte **účty organizace**. Tyto možnosti je možné automaticky zaregistrovat aplikaci v Azure AD také automaticky nakonfigurovat svoji aplikaci můžete integrovat s Azure AD. Není nutné použít **změna ověřování** dialogové okno pro registraci a konfiguraci vaší aplikace, ale je snazší. Pokud například používáte sadu Visual Studio 2012, můžete ručně zaregistrovat aplikaci v portálu pro správu Azure a aktualizaci konfigurace můžete integrovat s Azure AD.  
   V rozevíracích nabídkách, vyberte **Cloud – jedna organizace** a **jednotného přihlašování, čtení dat adresáře**. Zadejte doménu pro váš adresář Azure AD, například (v následující obrázky) *aricka0yahoo.onmicrosoft.com*a potom klikněte na tlačítko **OK**. Název domény můžete získat z karty domén pro výchozí adresář na portálu azure portal (viz následující obrázek dolů).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
   Následující obrázek ukazuje název domény z portálu Azure portal.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > Volitelně můžete nakonfigurovat identifikátor URI ID aplikace, která se zaregistruje ve službě Azure AD kliknutím **další možnosti**. Identifikátor URI ID aplikace je jedinečný identifikátor pro aplikaci, která je zaregistrovaný ve službě Azure AD a používá aplikace k identifikaci při komunikaci s Azure AD. Další informace o identifikátor URI ID aplikace a další vlastnosti registrovaných aplikací najdete v tématu [v tomto tématu](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Kliknutím na zaškrtávací políčko pod polem pro identifikátor URI ID aplikace můžete také přepsat existující registraci ve službě Azure AD, který používá stejný identifikátor URI ID aplikace.
4. Po kliknutí na tlačítko **OK**, zobrazí se dialogové okno přihlášení, a budete se muset přihlásit pomocí účtu globálního správce (ne účet Microsoft spojený s vaším předplatným). Pokud jste dříve vytvořili nový účet správce, bude nutné změnit heslo a potom znovu přihlásit pomocí nového hesla.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Po byli jste úspěšně ověřeni, **nový projekt ASP.NET** dialogového okna se zobrazí podle vašeho výběru ověřování (**organizační** ) a zaregistrována (adresáře,kdebudounovéaplikace*aricka0yahoo.onmicrosoft.com* na obrázku níže). Níže uvedené informace, zaškrtněte políčko s popiskem **hostovat v cloudu**. Pokud toto políčko zaškrtnuto, projekt se dá zřídit jako webovou aplikaci Azure a budou povolené pro snadné publikování později. Klikněte na tlačítko **OK**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. **Web konfigurovat Azure** zobrazí se dialogové okno, použití lokality automaticky generovaný název a oblast. Všimněte si také účet, který jste aktuálně přihlášeni do dialogového okna. Chcete, aby se zajistilo, že tento účet je ten, který je přiřazena vašemu předplatnému Azure, obvykle s účtem Microsoft.

    > [!NOTE]
    > Tento projekt vyžaduje databázi. Musíte vybrat jednu z existujících databází, nebo vytvořte novou. Databázi se totiž lokálního databázového souboru projektu již používá k ukládání malé množství dat konfigurace ověřování. Při nasazování aplikace na web Azure, tato databáze není zabalena s nasazením, proto je nutné zvolit ten, který je přístupný v cloudu. Klikněte na tlačítko **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Vytvoří projekt, a možnosti ověřování a možnosti webové aplikace se automaticky nakonfigurují s projektem. Po dokončení tohoto procesu spusťte projekt lokálně stisknutím kombinace kláves **^ F5**. Budete muset k přihlášení pomocí účtu organizace. Zadejte uživatelské jméno a heslo pro účet, který jste dříve vytvořili a klikněte na tlačítko **přihlášení**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Po úspěšném přihlášení se zobrazí web ASP.NET, že ověření zobrazením uživatelské jméno v pravém horním rohu stránky.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
   Pokud dojde k chybě:  
   Hodnota nemůže být null ani prázdný. Název parametru: text odkazu   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
   Zobrazit [ladění](#dbg) oddílu na konci tohoto kurzu.

## <a name="basics-of-the-graph-api"></a>Základní informace o rozhraní Graph API

[Rozhraní Graph API](https://msdn.microsoft.com/library/azure/hh974476.aspx) je programové rozhraní, které umožňuje provádět CRUD a další operace na objektech v adresáři služby Azure AD. Pokud vyberete možnost účet organizace pro ověřování při vytváření nového projektu v sadě Visual Studio 2013, vaše aplikace již nakonfigurován na volání rozhraní Graph API. Tato část stručně popisuje, jak funguje rozhraní Graph API.

1. Ve spuštěné aplikaci, klikněte na jméno přihlášeného uživatele v horní části stránky. Tím se dostanete na stránku profilu uživatele, která je akce v Kontroleru Domů. Můžete si všimnout, že tabulka obsahuje informace o uživateli o účet správce, kterou jste vytvořili dříve. Tyto informace jsou uloženy ve vašem adresáři a rozhraní Graph API je volána pro načtení těchto informací při načtení stránky.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Přejděte zpět do sady Visual Studio a rozbalte **řadiče** složku a pak otevřete **HomeController.cs** souboru. Zobrazí se vám **UserProfile()** akci, která obsahuje kód pro načtení tokenu a poté zavolat rozhraní Graph API. Tento kód je duplicitní níže: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Pro volání rozhraní Graph API, musíte nejprve získat token. Když je načten token, její řetězcová hodnota musí být připojeno v hlavičce autorizace pro všechny následné žádosti na rozhraní Graph API. Většinu kódu nad zpracovává podrobnosti o ověřování v Azure AD za účelem získání tokenu pomocí tokenu pro volání rozhraní Graph API a pak transformace odpověď tak, aby ji lze zobrazit v zobrazení.

    Následující zvýrazněný řádek je nejdůležitější část pro diskuse: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Tento řádek představuje jméno uživatele, který má byla deserializovat z odpovědi JSON a jsou zobrazena v zobrazení.

    Můžete volání rozhraní Graph API pomocí položky HttpClient a zpracování nezpracovaných dat sami, ale snadnější způsob je použít [grafu klientskou knihovnu, která je k dispozici prostřednictvím NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). Klientská knihovna zpracovává nezpracovaných žádostí HTTP a transformace vrácená data pro vás a výrazně usnadňuje práci s rozhraním Graph API v prostředí .NET. Zobrazit související ukázky kódu rozhraní Graph API na [Githubu.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Nasazení aplikace do Azure

Následující kroky ukazují postup nasazení aplikace do Azure. V dřívějších krocích připojen nový projekt s webovou aplikací v Azure, tak, aby byl připraven k publikování několika krocích.

1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt a vyberte **publikovat**. **Publikování webu** zobrazí se dialogové okno s každou nastavení jsou už nakonfigurovaná. Klikněte na **Další** tlačítko přejdete na **nastavení** stránky. Můžete být vyzváni k ověření; Ujistěte se, že ověření pomocí účtu předplatného Azure (obvykle s účtem Microsoft) a organizační účet, který jste vytvořili dříve.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Zkontrolujte, **povolit ověřování organizace** možnost. V **domény** pole, zadejte doménu pro svůj adresář. Z **úroveň přístupu** rozevíracího seznamu, vyberte **jednotného přihlašování, čtení dat adresáře**. Všimněte si, že jsou v již vyplněné předchozí databáze, které jste použili **databází** oddílu. Klikněte na tlačítko **publikovat**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio se začne nasazovat vašeho webu a pak se zobrazí nové okno prohlížeče. Můžete být vyzváni k ověření do svého adresáře ještě jednou. Po ověření, budete přesměrováni na přehled nově publikovaných webu v Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Ladění aplikace

Pokud se zobrazí následující chyba:   
 Hodnota nemůže být null ani prázdný. Název parametru: text odkazu   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Nahraďte kód v *Views\Shared\\_LoginPartial.cshtml* souboru následujícím kódem:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Po spuštění aplikace, pokud se přihlášený uživatel zobrazí "Null User", odhlaste se a zase přihlásit se pomocí účtu Active Directory, kterou jste vytvořili dříve.

Rick Rainey je vynikající kurzu postupovat podle [podrobně: Azure Websites a ověřování organizace pomocí služby Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Další informace

- [– Podrobné informace: Ověřování organizace pomocí služby Azure AD a Azure Websites](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Přehled Azure AD Graph API](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Scénáře ověřování ve službě Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Ukázky kódu Azure AD na Githubu](https://github.com/AzureADSamples)
