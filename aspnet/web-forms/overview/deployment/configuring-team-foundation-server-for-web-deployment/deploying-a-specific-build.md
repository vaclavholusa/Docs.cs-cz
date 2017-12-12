---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: "Nasazení konkrétní sestavení | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje postup nasazení webových balíčků a databázové skripty z konkrétní předchozího sestavení do nového cíle, jako je pracovním nebo produkčním enviro..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: afac083c96c1396ad60275fcb55a0ec9c4c0bd44
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="deploying-a-specific-build"></a>Nasazení konkrétní sestavení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje postup nasazení webových balíčků a databázové skripty z konkrétní předchozího sestavení do nového cíle, jako je pracovním nebo produkčním prostředí.


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na soubor projektu rozdělení metody uvedené v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které dva soubory projektu & #x 2014 je řízena procesem sestavení a nasazení; o Ne, obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Dosud témata v této sadě kurz zaměřuje na postup sestavení, balíčků a nasazování webových aplikací a databází v rámci jednoho kroku nebo automatizované procesu. Ale v některé běžné scénáře, budete chtít ze sestavení do složky rozevíracího seznamu vyberte prostředky, které nasadíte. Jinými slovy nemusí být nejnovější sestavení sestavení, který chcete nasadit.

Vezměte v úvahu scénář průběžnou integraci (CI) popsané v předchozí tématu [vytváření sestavení definice, podporuje nasazení](creating-a-build-definition-that-supports-deployment.md). Jste vytvořili definice sestavení v Team Foundation Server (TFS) 2010. Pokaždé, když vývojář kontroluje kód do sady TFS, Team Build bude sestavení kódu, vytváření balíčků webové a databázové skripty jako součást procesu sestavení, spustit všechny testy jednotek a nasazení vašich prostředků v testovacím prostředí. V závislosti na zásady uchovávání informací, které jste nakonfigurovali, když jste vytvořili definice sestavení zachovají TFS určitého počtu předchozích sestavení.

![](deploying-a-specific-build/_static/image1.png)

Nyní předpokládejme, že jste provedli ověření a ověření testování proti jednu z těchto sestavení v testovacím prostředí a jste připraveni k nasazení aplikace do pracovního prostředí. Do té doby vývojáři mohou mít změnami nový kód. Nechcete, aby znovu sestavte řešení a nasazení do fázovacího prostředí a nechcete, aby k nasazení nejnovější sestavení pro testovací prostředí. Místo toho kterou chcete nasadit konkrétní build, který jste se ověřit a ověřit na serveru test.

K tomu, budete muset v Microsoft Build Engine (MSBuild) umožňují vyhledávání webových balíčků a databázové skripty, které vygenerovaly konkrétní sestavení.

## <a name="overriding-the-outputroot-property"></a>Přepisování vlastnosti OutputRoot

V [ukázkové řešení](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* souboru deklaruje vlastnost s názvem **OutputRoot**. Jak již název naznačuje, jde kořenové složky, která obsahuje všechno, co se generuje procesu sestavení. V *Publish.proj* souboru, můžete zobrazit, **OutputRoot** vlastnost odkazuje na kořenový adresář pro všechny prostředky pro nasazení.

> [!NOTE]
> **OutputRoot** je název běžně používané vlastnosti. Visual C# a Visual Basic soubory projektu také deklarovat tuto vlastnost k uložení kořenový adresář pro všechny výstupy sestavení.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Pokud chcete soubor projektu do nasazení webových balíčků a databázové skripty z jiného umístění & #x 2014; jako výstupy předchozí TFS build & #x 2014; jednoduše je potřeba přepsat **OutputRoot** vlastnost. Na serveru Team Build byste měli nastavit hodnotu vlastnosti ke složce relevantní sestavení. MSBuild spustili z příkazového řádku, můžete zadat hodnotu pro **OutputRoot** jako argument příkazového řádku:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


V praxi však by také chcete přeskočit **sestavení** cíl & #x 2014; neexistuje bod při vytváření řešení, pokud nemáte v úmyslu použít výstupy sestavení. Může to uděláte tak, že zadáte cíle, které chcete spustit z příkazového řádku:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


Ale ve většině případů budete chtít vytvořit logika nasazení do definice sestavení sady TFS. Díky tomu mohou uživatelé s **fronty sestavení** oprávnění k aktivaci nasazení z jakékoli instalaci sady Visual Studio s připojením k serveru TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Vytvoření definice sestavení pro nasazení konkrétní sestavení

Následující postup popisuje, jak k vytvoření definice sestavení, která umožní uživatelům aktivační událost nasazení pro pracovní prostředí s jedním příkazem.

V takovém případě nechcete definici sestavení ve skutečnosti sestavit nic & #x 2014; je právě má být spuštěna v souboru projektu vlastní logiky nasazení. *Publish.proj* soubor obsahuje podmíněnou logiku, který přeskočí **sestavení** cíle, pokud soubor běží v Team Build. Dělá to pomocí vyhodnocení integrované **BuildingInTeamBuild** vlastnosti, která se automaticky nastaví na **true** při spuštění souboru projektu v Team Build. V důsledku toho můžete přeskočit procesu sestavení a jednoduše spusťte soubor projektu nasazení existující sestavení.

**K vytvoření definice sestavení pro ruční aktivaci nasazení**

1. V sadě Visual Studio 2010 v **Team Explorer** okno, rozbalte uzel vaší týmového projektu, klikněte pravým tlačítkem na **sestavení**a potom klikněte na **novou definici sestavení**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Na **Obecné** kartě, pojmenujte definici sestavení (například **DeployToStaging**) a volitelný popis.
3. Na **aktivační událost** vyberte **Ruční – vrácení se změnami nespouštějí nového sestavení**.
4. Na **sestavení výchozí** ve **kopie sestavení výstup do následující složky, vyřaďte** pole, zadejte cestu Universal Naming Convention (UNC) vaší složky (například  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Na **proces** ve **souboru procesu sestavení** rozevíracího seznamu, ponechejte **DefaultTemplate.xaml** vybrané. Toto je jedna z výchozích šablon procesu sestavení, které se přidají do všechny nové týmových projektů.
6. V **parametry procesu sestavení** tabulky, klikněte na tlačítko v **položky k sestavení** řádek a klikněte **třemi tečkami** tlačítko.

    ![](deploying-a-specific-build/_static/image4.png)
7. V **položky k sestavení** dialogové okno, klikněte na tlačítko **přidat**.
8. V **položky typu** rozevíracího seznamu vyberte **soubory projektu nástroje MSBuild**.
9. Přejděte do umístění souboru vlastní projektu, se kterým řízení procesu nasazení, vyberte soubor a pak klikněte na tlačítko **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. V **položky k sestavení** dialogové okno, klikněte na tlačítko **OK**.
11. V **parametry procesu sestavení** tabulky, rozbalte **Upřesnit** části.
12. V **argumenty MSBuild** řádek, zadejte umístění souboru projektu konkrétní prostředí a přidejte zástupný symbol pro umístění složky sestavení:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Budete potřebovat k přepsání **OutputRoot** hodnotu pokaždé, když fronty sestavení. Je to popsané v dalším postupu.
13. Klikněte na tlačítko **Uložit**.

Když spustíte sestavení, je potřeba aktualizovat **OutputRoot** vlastnost tak, aby odkazoval na sestavení, které chcete nasadit.

**K nasazení konkrétní sestavení z definice sestavení**

1. V **Team Explorer** okno, klikněte pravým tlačítkem na definici sestavení a pak klikněte na tlačítko **fronty nové sestavení**.

    ![](deploying-a-specific-build/_static/image7.png)
2. V **fronty sestavení** v dialogovém **parametry** rozbalte **Upřesnit** části.
3. V **argumenty MSBuild** řádek, nahraďte hodnotu **OutputRoot** vlastnost s umístění složky sestavení. Příklad:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Ujistěte se, že zahrnovat koncové lomítko na konci cestu do složky sestavení.
4. Klikněte na tlačítko **fronty**.

Když fronty sestavení, bude soubor projektu nasazení databázové skripty a webové balíčky ze složky rozevírací sestavení, kterou jste zadali v **OutputRoot** vlastnost.

## <a name="conclusion"></a>Závěr

Toto téma popisuje postup publikování materiály pro nasazení, jako je webových balíčků a databázové skripty, z předchozí konkrétní sestavení pomocí modelu nasazení souboru projektu rozdělení. Ho vysvětlení, jak lze přepsat **OutputRoot** vlastnost a jak začlenit logiku nasazení do sady TFS sestavení definice.

## <a name="further-reading"></a>Další čtení

Další informace o vytváření definic sestavení, najdete v části [vytvoření základní definice sestavení](https://msdn.microsoft.com/en-us/library/ms181716.aspx) a [definovat proces vaše sestavení](https://msdn.microsoft.com/en-us/library/ms181715.aspx). Další pokyny k sestavení služby Řízení front, najdete v části [fronty sestavení](https://msdn.microsoft.com/en-us/library/ms181722.aspx).

>[!div class="step-by-step"]
[Předchozí](creating-a-build-definition-that-supports-deployment.md)
[další](configuring-permissions-for-team-build-deployment.md)
