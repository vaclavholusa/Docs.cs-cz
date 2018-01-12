---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
title: "Další vzory a pokyny (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs"
author: MikeWasson
description: "Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7e97cfc3-d830-4002-8ff7-5790d1ff49e6
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
msc.type: authoredcontent
ms.openlocfilehash: 2ac18799d214777d098cc85ec6c85fd09f84a782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="more-patterns-and-guidance-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="934c2-104">Další vzory a pokyny (vytváření reálných cloudových aplikací s Azure)</span><span class="sxs-lookup"><span data-stu-id="934c2-104">More Patterns and Guidance (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="934c2-105">podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="934c2-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="934c2-106">[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="934c2-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="934c2-107">**Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie.</span><span class="sxs-lookup"><span data-stu-id="934c2-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="934c2-108">Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud.</span><span class="sxs-lookup"><span data-stu-id="934c2-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="934c2-109">Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="934c2-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="934c2-110">Jste nyní zaznamenané 13 vzorů, které poskytují pokyny o tom, jak úspěšně v cloud computing.</span><span class="sxs-lookup"><span data-stu-id="934c2-110">You've now seen 13 patterns that provide guidance on how to be successful in cloud computing.</span></span> <span data-ttu-id="934c2-111">Jsou to jenom pár schémat, která se týkají cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="934c2-111">These are just a few of the patterns that apply to cloud apps.</span></span> <span data-ttu-id="934c2-112">Zde jsou některé další témata výpočetní cloudu a prostředků, které pomáhají s nimi:</span><span class="sxs-lookup"><span data-stu-id="934c2-112">Here are some more cloud computing topics, and resources to help with them:</span></span>

- <span data-ttu-id="934c2-113">Migraci stávající místní aplikace do cloudu.</span><span class="sxs-lookup"><span data-stu-id="934c2-113">Migrating existing on-premises applications to the cloud.</span></span> 

    - <span data-ttu-id="934c2-114">[Přesunutí aplikace do cloudu](https://msdn.microsoft.com/en-us/library/ff728592.aspx).</span><span class="sxs-lookup"><span data-stu-id="934c2-114">[Moving Applications to the Cloud](https://msdn.microsoft.com/en-us/library/ff728592.aspx).</span></span> <span data-ttu-id="934c2-115">Elektronická kniha podle Microsoft Patterns and Practices.</span><span class="sxs-lookup"><span data-stu-id="934c2-115">E-book by Microsoft Patterns and Practices.</span></span> <span data-ttu-id="934c2-116">Také k dispozici jako [tištěné paperback](https://www.amazon.com/dp/1621140202).</span><span class="sxs-lookup"><span data-stu-id="934c2-116">Also available as a [hard-copy paperback](https://www.amazon.com/dp/1621140202).</span></span>
    - <span data-ttu-id="934c2-117">[Migrace Microsoft ASP.NET a IIS.NET](https://go.microsoft.com/fwlink/?LinkId=400656).</span><span class="sxs-lookup"><span data-stu-id="934c2-117">[Migrating Microsoft's ASP.NET and IIS.NET](https://go.microsoft.com/fwlink/?LinkId=400656).</span></span> <span data-ttu-id="934c2-118">Případová studie podle Roberta Mcmurrayho.</span><span class="sxs-lookup"><span data-stu-id="934c2-118">Case study by Robert McMurray.</span></span>
    - <span data-ttu-id="934c2-119">[Přesunutí 4th &amp; primátor na weby Azure](http://www.jeff.wilcox.name/2013/04/4thandmayor-azure-websites/).</span><span class="sxs-lookup"><span data-stu-id="934c2-119">[Moving 4th &amp; Mayor to Azure Web Sites](http://www.jeff.wilcox.name/2013/04/4thandmayor-azure-websites/).</span></span> <span data-ttu-id="934c2-120">Blog o Jeff Wilcox post, chronicling jeho činnost přesun webovou aplikaci z Amazon Web Services pro službu Web Apps v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="934c2-120">Blog post by Jeff Wilcox chronicling his experience moving a web app from Amazon Web Services to Web Apps in Azure App Service.</span></span>
    - [<span data-ttu-id="934c2-121">Přesunutí aplikace do Azure: jaké změny?</span><span class="sxs-lookup"><span data-stu-id="934c2-121">Moving Apps to Azure: What changes?</span></span>](https://azure.microsoft.com/en-us/documentation/videos/web-sites-internals-and-the-file-system/) <span data-ttu-id="934c2-122">Krátké video podle Stefan Schackow, vysvětluje přístupu k systému souborů ve službě Web Apps v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="934c2-122">Short video by Stefan Schackow, explains file system access in Web Apps in Azure App Service.</span></span>
    - <span data-ttu-id="934c2-123">[Azure hybridní Cloud](https://www.amazon.com/dp/B00EOP4UQW).</span><span class="sxs-lookup"><span data-stu-id="934c2-123">[Azure Hybrid Cloud](https://www.amazon.com/dp/B00EOP4UQW).</span></span> <span data-ttu-id="934c2-124">Výtisk knihy nebo elektronická kniha Danny Garber, Jamal Malik a Adam Fazio.</span><span class="sxs-lookup"><span data-stu-id="934c2-124">Hardcopy book or e-book by Danny Garber, Jamal Malik, and Adam Fazio.</span></span>
- <span data-ttu-id="934c2-125">Zabezpečení, ověřování a autorizace problémy, které jsou jedinečné pro cloudové aplikace</span><span class="sxs-lookup"><span data-stu-id="934c2-125">Security, authentication, and authorization issues unique to cloud applications</span></span>

    - [<span data-ttu-id="934c2-126">Doprovodné materiály zabezpečení Azure</span><span class="sxs-lookup"><span data-stu-id="934c2-126">Azure Security Guidance</span></span>](https://azure.microsoft.com/blog/2014/02/10/best-practices-windows-azure-websites-waws/)
    - <span data-ttu-id="934c2-127">[Microsoft Patterns and Practices - Azure pokyny](https://msdn.microsoft.com/en-us/library/dn568099.aspx).</span><span class="sxs-lookup"><span data-stu-id="934c2-127">[Microsoft Patterns and Practices - Azure Guidance](https://msdn.microsoft.com/en-us/library/dn568099.aspx).</span></span> <span data-ttu-id="934c2-128">Informace najdete v těchto pravidel vzoru vzor federované Identity.</span><span class="sxs-lookup"><span data-stu-id="934c2-128">See Gatekeeper pattern, Federated Identity pattern.</span></span>
    - <span data-ttu-id="934c2-129">[Zabezpečení sítě Azure](https://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx).</span><span class="sxs-lookup"><span data-stu-id="934c2-129">[Azure Network Security](https://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx).</span></span> <span data-ttu-id="934c2-130">Dokument White paper podle Ashin Palekar.</span><span class="sxs-lookup"><span data-stu-id="934c2-130">White paper by Ashin Palekar.</span></span>

<span data-ttu-id="934c2-131">Viz také dodatečné cloudové výpočetní vzory a dokumentaci na webu [Microsoft Patterns and Practices - Azure pokyny](https://msdn.microsoft.com/en-us/library/dn568099.aspx).</span><span class="sxs-lookup"><span data-stu-id="934c2-131">See also additional cloud computing patterns and guidance at [Microsoft Patterns and Practices - Azure Guidance](https://msdn.microsoft.com/en-us/library/dn568099.aspx).</span></span>

<a id="resources"></a>
## <a name="resources"></a><span data-ttu-id="934c2-132">Prostředky</span><span class="sxs-lookup"><span data-stu-id="934c2-132">Resources</span></span>

<span data-ttu-id="934c2-133">Další informace o tomto konkrétní tématu každý kapitol v této příručce e poskytuje odkazy na zdroje.</span><span class="sxs-lookup"><span data-stu-id="934c2-133">Each of the chapters in this e-book provides links to resources for more information about that specific topic.</span></span> <span data-ttu-id="934c2-134">Následující seznam obsahuje odkazy na přehledy doporučené vzorků a osvědčené postupy pro vývoj úspěšné cloudu s Azure.</span><span class="sxs-lookup"><span data-stu-id="934c2-134">The following list provides links to overviews of best practices and recommended patterns for successful cloud development with Azure.</span></span>

<span data-ttu-id="934c2-135">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="934c2-135">Documentation</span></span>

- <span data-ttu-id="934c2-136">[Osvědčené postupy pro návrh rozsáhlých služeb v cloudu Azure služeb](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx).</span><span class="sxs-lookup"><span data-stu-id="934c2-136">[Best Practices for the Design of Large-Scale Services on Azure Cloud Services](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx).</span></span> <span data-ttu-id="934c2-137">Dokument White paper moduly SIMM značky a Michael Thomassy.</span><span class="sxs-lookup"><span data-stu-id="934c2-137">White paper by Mark Simms and Michael Thomassy.</span></span>
- <span data-ttu-id="934c2-138">[Bezporuchový: Pokyny pro odolné cloudové architektury](https://msdn.microsoft.com/en-us/library/windowsazure/jj853352.aspx).</span><span class="sxs-lookup"><span data-stu-id="934c2-138">[Failsafe: Guidance for Resilient Cloud Architectures](https://msdn.microsoft.com/en-us/library/windowsazure/jj853352.aspx).</span></span> <span data-ttu-id="934c2-139">Dokument White paper matolin Mercuri, Ulrich Homann a Andrew Townhill.</span><span class="sxs-lookup"><span data-stu-id="934c2-139">White paper by Marc Mercuri, Ulrich Homann, and Andrew Townhill.</span></span> <span data-ttu-id="934c2-140">Webová stránka verze série videí bezporuchový.</span><span class="sxs-lookup"><span data-stu-id="934c2-140">Web page version of the FailSafe video series.</span></span>
- <span data-ttu-id="934c2-141">[Azure pokyny](https://azure.microsoft.com/en-us/develop/net/guidance/) stránku portálu oficiální dokumentaci týkající se vývoje aplikací pro Azure.</span><span class="sxs-lookup"><span data-stu-id="934c2-141">[Azure Guidance](https://azure.microsoft.com/en-us/develop/net/guidance/) Portal page for official documentation related to developing applications for Azure.</span></span>

<span data-ttu-id="934c2-142">Videa</span><span class="sxs-lookup"><span data-stu-id="934c2-142">Videos</span></span>

- <span data-ttu-id="934c2-143">[Vytváření skutečných cloudových aplikací s Azure – část 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324) a [část 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325).</span><span class="sxs-lookup"><span data-stu-id="934c2-143">[Building Real World Cloud Apps with Azure - Part 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324) and [Part 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325).</span></span> <span data-ttu-id="934c2-144">Video prezentace podle Scott Guthrie založený na elektronických knih.</span><span class="sxs-lookup"><span data-stu-id="934c2-144">Video of the presentation by Scott Guthrie that this e-book is based on.</span></span> <span data-ttu-id="934c2-145">Zobrazí se v technické Ed Austrálie v září 2013.</span><span class="sxs-lookup"><span data-stu-id="934c2-145">Presented at Tech Ed Australia in September, 2013.</span></span> <span data-ttu-id="934c2-146">Starší verzi téže prezentaci doručenou v konferenční Norská vývojáři (NDC) v červen 2013: [NDC část 1](http://vimeo.com/68215538), [NDC část 2](http://vimeo.com/68215602).</span><span class="sxs-lookup"><span data-stu-id="934c2-146">An earlier version of the same presentation was delivered at Norwegian Developers Conference (NDC) in June, 2013: [NDC part 1](http://vimeo.com/68215538), [NDC part 2](http://vimeo.com/68215602).</span></span>
- <span data-ttu-id="934c2-147">[Bezporuchový: Vytváření škálovatelné, odolné cloudové služby](https://channel9.msdn.com/Series/FailSafe).</span><span class="sxs-lookup"><span data-stu-id="934c2-147">[FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe).</span></span> <span data-ttu-id="934c2-148">Série videí devět částí Ulrich Homann, Mercuri matolin a moduly SIMM značky.</span><span class="sxs-lookup"><span data-stu-id="934c2-148">Nine-part video series by Ulrich Homann, Marc Mercuri, and Mark Simms.</span></span> <span data-ttu-id="934c2-149">Představuje 400 úroveň zobrazení o tom, jak architektury cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="934c2-149">Presents a 400-level view of how to architect cloud apps.</span></span> <span data-ttu-id="934c2-150">Tato řada se zaměřuje na teorie a důvodech doporučené vzory; Další podrobnosti postupy najdete v tématu sestavování velkých řadu podle moduly SIMM značky.</span><span class="sxs-lookup"><span data-stu-id="934c2-150">This series focuses on theory and reasons behind recommended patterns; for more how-to details, see the Building Big series by Mark Simms.</span></span>
- <span data-ttu-id="934c2-151">[Vytváření Big: Poučení vyplývající z Azure zákazníků – část 1](https://channel9.msdn.com/Events/Build/2012/3-029) a [část 2](https://channel9.msdn.com/Events/Build/2012/3-030).</span><span class="sxs-lookup"><span data-stu-id="934c2-151">[Building Big: Lessons learned from Azure customers- Part 1](https://channel9.msdn.com/Events/Build/2012/3-029) and [Part 2](https://channel9.msdn.com/Events/Build/2012/3-030).</span></span> <span data-ttu-id="934c2-152">Série videí dvě části Simon Davies a moduly označit SIMM, podobně jako u řady bezporuchový ale orientované další směrem k praktické implementace.</span><span class="sxs-lookup"><span data-stu-id="934c2-152">Two-part video series by Simon Davies and Mark Simms, similar to the FailSafe series but oriented more toward practical implementation.</span></span>

<span data-ttu-id="934c2-153">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="934c2-153">Code sample</span></span>

- <span data-ttu-id="934c2-154">[Opravte ji aplikace, která doprovází elektronická kniha](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4?cdn_id=2013-12-03-002).</span><span class="sxs-lookup"><span data-stu-id="934c2-154">[The Fix It application that accompanies this e-book](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4?cdn_id=2013-12-03-002).</span></span>
- <span data-ttu-id="934c2-155">[Cloudové služby základy v Azure v jazyce C# pro sadu Visual Studio 2012](http://aka.ms/csf).</span><span class="sxs-lookup"><span data-stu-id="934c2-155">[Cloud Service Fundamentals in Azure in C# for Visual Studio 2012](http://aka.ms/csf).</span></span> <span data-ttu-id="934c2-156">Ke stažení projekt na webu Microsoft Code Gallery zahrnuje kód a dokumentace vyvinuté pomocí Microsoft zákazníka poradní tým (KOČKA).</span><span class="sxs-lookup"><span data-stu-id="934c2-156">Downloadable project in the Microsoft Code Gallery site, includes both code and documentation developed by the Microsoft Customer Advisory Team (CAT).</span></span> <span data-ttu-id="934c2-157">Demonstruje řadu osvědčené postupy doporučená v série videí bezporuchový a sestavování velkých a v dokumentu white paper bezporuchový.</span><span class="sxs-lookup"><span data-stu-id="934c2-157">Demonstrates many of the best practices advocated in the FailSafe and Building Big video series and the FailSafe white paper.</span></span> <span data-ttu-id="934c2-158">Galerie kódů stránka taky obsahuje odkazy na rozsáhlou dokumentaci autory projektu – viz zvlášť část [cloudové služby základy wiki kolekce](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx) odkaz blue pole v horní části Popis projektu.</span><span class="sxs-lookup"><span data-stu-id="934c2-158">The Code Gallery page also links to extensive documentation by the authors of the project -- see especially the [Cloud Service Fundamentals wiki collection](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx) link in the blue box near the top of the project description.</span></span> <span data-ttu-id="934c2-159">Tento projekt a v dokumentaci pro ni stále aktivně vyvíjených, takže je lepší volbou informace o mnoho témata než podobné, ale starší dokumenty white paper.</span><span class="sxs-lookup"><span data-stu-id="934c2-159">This project and the documentation for it still actively being developed, making it a better choice for information on many topics than similar but older white papers.</span></span>

<span data-ttu-id="934c2-160">Kopírování pevný knihy</span><span class="sxs-lookup"><span data-stu-id="934c2-160">Hard copy books</span></span>

- <span data-ttu-id="934c2-161">[Cloud Computing biblické](https://www.amazon.com/dp/0470903562).</span><span class="sxs-lookup"><span data-stu-id="934c2-161">[Cloud Computing Bible](https://www.amazon.com/dp/0470903562).</span></span> <span data-ttu-id="934c2-162">Podle Barrie Sosinsky.</span><span class="sxs-lookup"><span data-stu-id="934c2-162">By Barrie Sosinsky.</span></span>
- <span data-ttu-id="934c2-163">[Verze ji! Navrhování a nasazení softwaru produkční prostředí](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213).</span><span class="sxs-lookup"><span data-stu-id="934c2-163">[Release It! Design and Deploy Production-Ready Software](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213).</span></span> <span data-ttu-id="934c2-164">Podle Michael T. Nygard.</span><span class="sxs-lookup"><span data-stu-id="934c2-164">By Michael T. Nygard.</span></span>
- <span data-ttu-id="934c2-165">[Vzory cloudové architektury: Použití Microsoft Azure](http://shop.oreilly.com/product/0636920023777.do).</span><span class="sxs-lookup"><span data-stu-id="934c2-165">[Cloud Architecture Patterns: Using Microsoft Azure](http://shop.oreilly.com/product/0636920023777.do).</span></span> <span data-ttu-id="934c2-166">Podle Wilder faktury.</span><span class="sxs-lookup"><span data-stu-id="934c2-166">By Bill Wilder.</span></span>
- <span data-ttu-id="934c2-167">[Windows Azure Platform](https://www.amazon.com/dp/1430235632).</span><span class="sxs-lookup"><span data-stu-id="934c2-167">[Windows Azure Platform](https://www.amazon.com/dp/1430235632).</span></span> <span data-ttu-id="934c2-168">Podle Tejaswi Redkar.</span><span class="sxs-lookup"><span data-stu-id="934c2-168">By Tejaswi Redkar.</span></span>
- <span data-ttu-id="934c2-169">[Windows Azure programování vzory pro začínající podniky](https://www.amazon.com/dp/1849685606).</span><span class="sxs-lookup"><span data-stu-id="934c2-169">[Windows Azure programming patterns for Start-ups](https://www.amazon.com/dp/1849685606).</span></span> <span data-ttu-id="934c2-170">Podle Riccardo Becker.</span><span class="sxs-lookup"><span data-stu-id="934c2-170">By Riccardo Becker.</span></span>
- <span data-ttu-id="934c2-171">[Microsoft Windows Azure Development kuchařka](https://www.amazon.com/dp/1849682224).</span><span class="sxs-lookup"><span data-stu-id="934c2-171">[Microsoft Windows Azure Development Cookbook](https://www.amazon.com/dp/1849682224).</span></span> <span data-ttu-id="934c2-172">Podle Neil Mackenzie.</span><span class="sxs-lookup"><span data-stu-id="934c2-172">By Neil Mackenzie.</span></span>

<span data-ttu-id="934c2-173">Nakonec při začít vytváření reálných aplikací a jejich spouštění v Azure, dřív nebo později budete pravděpodobně potřebovat pomoc od odborníků.</span><span class="sxs-lookup"><span data-stu-id="934c2-173">Finally, when you get started building real-world apps and running them in Azure, sooner or later you'll probably need assistance from experts.</span></span> <span data-ttu-id="934c2-174">Například můžete klást otázky v lokalitách komunity [fóra Azure nebo StackOverflow](https://azure.microsoft.com/en-us/support/forums/), nebo mohou kontaktovat Microsoft přímo za účelem podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="934c2-174">You can ask questions in community sites such as [Azure forums or StackOverflow](https://azure.microsoft.com/en-us/support/forums/), or you can contact Microsoft directly for Azure support.</span></span> <span data-ttu-id="934c2-175">Společnost Microsoft nabízí několik úrovní se na technickou podporu Azure: Souhrn a porovnání možností najdete v tématu [podporu Azure](https://azure.microsoft.com/en-us/support/plans/).</span><span class="sxs-lookup"><span data-stu-id="934c2-175">Microsoft offers several levels of technical support Azure: for a summary and comparison of the options, see [Azure Support](https://azure.microsoft.com/en-us/support/plans/).</span></span>

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a><span data-ttu-id="934c2-176">Potvrzování</span><span class="sxs-lookup"><span data-stu-id="934c2-176">Acknowledgments</span></span>

<span data-ttu-id="934c2-177">Tento obsah napsal tní Dykstra, Rick Anderson a Wasson Jan.</span><span class="sxs-lookup"><span data-stu-id="934c2-177">This content was written by Tom Dykstra, Rick Anderson, and Mike Wasson.</span></span> <span data-ttu-id="934c2-178">Většina původní obsah pochází [Scott Guthrie](https://weblogs.asp.net/scottgu/), a je mu zase obrázek na materiálu z moduly SIMM značky a Microsoft zákazníka poradní tým (KOČKA).</span><span class="sxs-lookup"><span data-stu-id="934c2-178">Most of the original content came from [Scott Guthrie](https://weblogs.asp.net/scottgu/), and he in turn drew on material from Mark Simms and the Microsoft Customer Advisory Team (CAT).</span></span>

<span data-ttu-id="934c2-179">Mnoho kolegy v Microsoftu recenze a komentář na pracovní verze a kódu:</span><span class="sxs-lookup"><span data-stu-id="934c2-179">Many other colleagues at Microsoft reviewed and commented on drafts and code:</span></span>

- <span data-ttu-id="934c2-180">TIM Ammann - přečetli kapitoly automatizace.</span><span class="sxs-lookup"><span data-stu-id="934c2-180">Tim Ammann - Reviewed the automation chapter.</span></span>
- <span data-ttu-id="934c2-181">Bennage Kryštof – zkontrolovat a testovat kód opravte ji.</span><span class="sxs-lookup"><span data-stu-id="934c2-181">Christopher Bennage - Reviewed and tested the Fix It code.</span></span>
- <span data-ttu-id="934c2-182">Ryan bobulovinami - přečetli kapitoly CD/CI.</span><span class="sxs-lookup"><span data-stu-id="934c2-182">Ryan Berry - Reviewed the CD/CI chapter.</span></span>
- <span data-ttu-id="934c2-183">Vittorio Bertocci - přečetli kapitoly jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="934c2-183">Vittorio Bertocci - Reviewed the SSO chapter.</span></span>
- <span data-ttu-id="934c2-184">Jan Clayton - pomohly vyřešit technické problémy ve skriptech prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="934c2-184">Chris Clayton - Helped resolve technical problems in the PowerShell scripts.</span></span>
- <span data-ttu-id="934c2-185">Conor Cunningham - přečetli kapitoly možnosti úložiště data.</span><span class="sxs-lookup"><span data-stu-id="934c2-185">Conor Cunningham - Reviewed the data storage options chapter.</span></span>
- <span data-ttu-id="934c2-186">Carlos Farre – zkontrolovat a testovat kód opravte ji pro problémy se zabezpečením.</span><span class="sxs-lookup"><span data-stu-id="934c2-186">Carlos Farre - Reviewed and tested the Fix It code for security issues.</span></span>
- <span data-ttu-id="934c2-187">Larry Franks - přečetli telemetrie a monitorování kapitoly.</span><span class="sxs-lookup"><span data-stu-id="934c2-187">Larry Franks - Reviewed the telemetry and monitoring chapter.</span></span>
- <span data-ttu-id="934c2-188">Jonathan Gao - přečetli Hadoop a MapReduce části kapitoly možnosti úložiště data.</span><span class="sxs-lookup"><span data-stu-id="934c2-188">Jonathan Gao - Reviewed Hadoop and MapReduce sections of the data storage options chapter.</span></span>
- <span data-ttu-id="934c2-189">Sidney Higa - zkontrolovány všechny kapitolám.</span><span class="sxs-lookup"><span data-stu-id="934c2-189">Sidney Higa - Reviewed all chapters.</span></span>
- <span data-ttu-id="934c2-190">Gordon Hogenson - přečetli kapitoly zdroj ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="934c2-190">Gordon Hogenson - Reviewed the source control chapter.</span></span>
- <span data-ttu-id="934c2-191">Tamra Horák - možnosti ukládání dat zkontrolovat, objektů blob a fronty kapitolám.</span><span class="sxs-lookup"><span data-stu-id="934c2-191">Tamra Myers - Reviewed data storage options, blob, and queues chapters.</span></span>
- <span data-ttu-id="934c2-192">Pranav Rastogi - přečetli kapitoly jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="934c2-192">Pranav Rastogi - Reviewed the SSO chapter.</span></span>
- <span data-ttu-id="934c2-193">Červen digestoru kterých – přidání zpracování chyb a nápovědu k prostředí PowerShell skripty pro automatizaci.</span><span class="sxs-lookup"><span data-stu-id="934c2-193">June Blender Rogers - Added error handling and help to the PowerShell automation scripts.</span></span>
- <span data-ttu-id="934c2-194">Mani Subramanian - zkontrolovány všechny kapitolám a vedla revize kódu a testování procesu pro kód opravit.</span><span class="sxs-lookup"><span data-stu-id="934c2-194">Mani Subramanian - Reviewed all chapters and led the code review and testing process for the Fix It code.</span></span>
- <span data-ttu-id="934c2-195">Shauna Tinline-Petr - přečetli dělení kapitoly data.</span><span class="sxs-lookup"><span data-stu-id="934c2-195">Shaun Tinline-Jones - Reviewed the data partitioning chapter.</span></span>
- <span data-ttu-id="934c2-196">Selcin Tukarslan - zkontrolovat kapitolám, které se týkají SQL Database a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="934c2-196">Selcin Tukarslan - Reviewed chapters that cover SQL Database and SQL Server.</span></span>
- <span data-ttu-id="934c2-197">Služby EDWARD Wu - poskytuje ukázkový kód pro kapitoly jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="934c2-197">Edward Wu - Provided sample code for the SSO chapter.</span></span>
- <span data-ttu-id="934c2-198">Guang Yang - napsali skripty pro automatizaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="934c2-198">Guang Yang - Wrote the PowerShell automation scripts.</span></span>

<span data-ttu-id="934c2-199">Členové [Microsoft Developer pokyny Advisory Council](http://aka.ms/DGAC) (DGAC) také recenze a komentář na koncepty:</span><span class="sxs-lookup"><span data-stu-id="934c2-199">Members of the [Microsoft Developer Guidance Advisory Council](http://aka.ms/DGAC) (DGAC) also reviewed and commented on drafts:</span></span>

- <span data-ttu-id="934c2-200">Jean Luc Boucho</span><span class="sxs-lookup"><span data-stu-id="934c2-200">Jean-Luc Boucho</span></span>
- <span data-ttu-id="934c2-201">Catalin Gheorghiu</span><span class="sxs-lookup"><span data-stu-id="934c2-201">Catalin Gheorghiu</span></span>
- <span data-ttu-id="934c2-202">Wouter de Kort</span><span class="sxs-lookup"><span data-stu-id="934c2-202">Wouter de Kort</span></span>
- <span data-ttu-id="934c2-203">Carlosu dos Santos</span><span class="sxs-lookup"><span data-stu-id="934c2-203">Carlos dos Santos</span></span>
- <span data-ttu-id="934c2-204">Neil Mackenzie</span><span class="sxs-lookup"><span data-stu-id="934c2-204">Neil Mackenzie</span></span>
- <span data-ttu-id="934c2-205">Dennis Persson</span><span class="sxs-lookup"><span data-stu-id="934c2-205">Dennis Persson</span></span>
- <span data-ttu-id="934c2-206">Simonu Sabat</span><span class="sxs-lookup"><span data-stu-id="934c2-206">Sunil Sabat</span></span>
- [<span data-ttu-id="934c2-207">Aleksey Sinyagin</span><span class="sxs-lookup"><span data-stu-id="934c2-207">Aleksey Sinyagin</span></span>](http://www.linkedin.com/in/sinyagin)
- <span data-ttu-id="934c2-208">Wagnera faktury</span><span class="sxs-lookup"><span data-stu-id="934c2-208">Bill Wagner</span></span>
- <span data-ttu-id="934c2-209">Michael dřeva</span><span class="sxs-lookup"><span data-stu-id="934c2-209">Michael Wood</span></span>

<span data-ttu-id="934c2-210">Jiní členové DGAC recenze a komentář na předběžná přehledu:</span><span class="sxs-lookup"><span data-stu-id="934c2-210">Other members of the DGAC reviewed and commented on the preliminary outline:</span></span>

- <span data-ttu-id="934c2-211">Damir Arh</span><span class="sxs-lookup"><span data-stu-id="934c2-211">Damir Arh</span></span>
- <span data-ttu-id="934c2-212">EDWARD Bakker</span><span class="sxs-lookup"><span data-stu-id="934c2-212">Edward Bakker</span></span>
- <span data-ttu-id="934c2-213">Srdjan Bozovic</span><span class="sxs-lookup"><span data-stu-id="934c2-213">Srdjan Bozovic</span></span>
- <span data-ttu-id="934c2-214">Jde Man kanál</span><span class="sxs-lookup"><span data-stu-id="934c2-214">Ming Man Chan</span></span>
- <span data-ttu-id="934c2-215">Gianni Rosa Gallina</span><span class="sxs-lookup"><span data-stu-id="934c2-215">Gianni Rosa Gallina</span></span>
- <span data-ttu-id="934c2-216">Paulo Morgado</span><span class="sxs-lookup"><span data-stu-id="934c2-216">Paulo Morgado</span></span>
- <span data-ttu-id="934c2-217">JASON Oliveira</span><span class="sxs-lookup"><span data-stu-id="934c2-217">Jason Oliveira</span></span>
- <span data-ttu-id="934c2-218">Alberto Poblacion</span><span class="sxs-lookup"><span data-stu-id="934c2-218">Alberto Poblacion</span></span>
- <span data-ttu-id="934c2-219">Ryan Rileymu</span><span class="sxs-lookup"><span data-stu-id="934c2-219">Ryan Riley</span></span>
- <span data-ttu-id="934c2-220">Petr PEREZ Tsisah</span><span class="sxs-lookup"><span data-stu-id="934c2-220">Perez Jones Tsisah</span></span>
- <span data-ttu-id="934c2-221">Roger Whitehead</span><span class="sxs-lookup"><span data-stu-id="934c2-221">Roger Whitehead</span></span>
- <span data-ttu-id="934c2-222">Pawel Wilkosz</span><span class="sxs-lookup"><span data-stu-id="934c2-222">Pawel Wilkosz</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="934c2-223">[Předchozí](queue-centric-work-pattern.md)
[další](the-fix-it-sample-application.md)</span><span class="sxs-lookup"><span data-stu-id="934c2-223">[Previous](queue-centric-work-pattern.md)
[Next](the-fix-it-sample-application.md)</span></span>