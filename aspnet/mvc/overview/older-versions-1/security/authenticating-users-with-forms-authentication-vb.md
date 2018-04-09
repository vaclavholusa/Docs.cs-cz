---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: Ověřování uživatelů s formulářové ověřování (VB) | Microsoft Docs
author: microsoft
description: Naučte se používat atribut [autorizovat] heslo k ochraně konkrétní stránky v aplikaci MVC. Zjistíte, jak používat správu webu příliš...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ff425a4c9728de2eec3d0c94e76cb51a15de487
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="authenticating-users-with-forms-authentication-vb"></a>Ověřování uživatelů pomocí ověřování pomocí formulářů (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> Naučte se používat atribut [autorizovat] heslo k ochraně konkrétní stránky v aplikaci MVC. Zjistíte, jak používat nástroj pro správu webu k vytváření a správě uživatelů a rolí. Také zjistíte, jak nakonfigurovat, se uloží informace o účtu a role uživatele.


Cílem tohoto kurzu je vysvětlují, jak můžete použít formuláře ověřování heslo k ochraně zobrazení v aplikacích ASP.NET MVC. Zjistíte, jak používat nástroj pro správu webového serveru k vytvoření uživatelů a rolí. Také zjistíte, jak zabránit neoprávněným uživatelům v vyvolání akce kontroleru. Nakonec zjistíte, jak nakonfigurovat, kde jsou uložená uživatelská jména a hesla.

#### <a name="using-the-web-site-administration-tool"></a>Pomocí nástroje Správa webu

Před provedeme cokoliv jiného, jsme měli začít vytváření některých uživatelů a rolí. Nejjednodušší způsob, jak vytvořit nové uživatele a role je využít nástroj pro správu Visual Studio 2008 webu. Tento nástroj můžete spustit výběrem možnosti nabídky **projektu, konfigurace ASP.NET**. Alternativně můžete spustit nástroj Správa webu kliknutím na ikonu (poněkud strach) v provede stiskne na světě, který se zobrazí v horní části okna Průzkumníka řešení (viz obrázek 1).

**Obrázek 1 – spustit nástroj Správa webu**

![clip_image002[4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

V rámci nástroj pro správu webu při vytváření nových uživatelů a rolí na kartě Zabezpečení dialogu. Klikněte **vytvořit uživateli** odkaz pro vytvoření nového uživatele s názvem Stephen (viz obrázek 2). Zadejte uživatele Stephen heslem, které chcete (například *tajný klíč*).

**Obrázek 2 – Vytvoření nového uživatele**

![clip_image004[4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

Při vytváření nové role první povolení role a definování jednu nebo více rolí. Povolit role kliknutím **povolit role** odkaz. Dále vytvořte roli s názvem *správci* kliknutím **vytvořit ani spravovat role** odkaz (viz obrázek 3).

**Obrázek 3: vytvoření nové role**

![clip_image006[4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

Nakonec vytvořte nového uživatele s názvem Jan a přidružte Jan role správců tak, že kliknete na odkaz vytvořit uživatele výběr správci při vytváření Jan (viz obrázek 4).

**Obrázek 4 – přidání uživatele k roli**

![clip_image008[4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

Když všechny je uvedená a provádí, měli byste mít dva nové uživatele s názvem Stephen a Jan. Musí být také novou roli s názvem Správci. Jan je členem role Správci nástroje a Stephen není.

#### <a name="requiring-authorization"></a>Vyžadující ověření

Může vyžadovat uživatel ověřený, než uživatel vyvolá akce kontroleru přidáním atribut [autorizovat] na akci. Atribut [autorizovat] můžete použít pro jednotlivé řadiče akce nebo můžete použít tento atribut třídy celý kontroler.

Například řadič v výpis 1 zpřístupní akci s názvem CompanySecrets(). Protože tato akce je upraven pomocí atributu [Authorize], nelze vyvolat tuto akci, pokud je uživatel ověřen.

**Výpis 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

Pokud volání akce CompanySecrets() zadáním adresy URL/Home nebo CompanySecrets na panelu Adresa prohlížeče a nejste ověřeného uživatele, pak budete přesměrováni na přihlášení zobrazení automaticky (viz obrázek 5).

**Obrázek 5 – zobrazení přihlášení**

![clip_image010[4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

Zobrazení přihlášení můžete použít k zadání uživatelského jména a hesla. Pokud si nejste registrovaní uživatelé pak můžete kliknout na **zaregistrovat** odkazu přejděte na rejstříku zobrazení (viz obrázek 6). Registrace zobrazení můžete vytvořit nový uživatelský účet.

**Obrázek 6 – registrace zobrazení**

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

Po úspěšném přihlášení, uvidíte CompanySecrets zobrazení (viz obrázek 7). Ve výchozím nastavení budou nadále přihlášení, dokud nezavřete okno prohlížeče.

**Obrázek 7 – CompanySecrets zobrazení**

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorizace uživatelské jméno nebo roli uživatele

Atribut [autorizovat] můžete použít k omezení přístupu k akci kontroleru na konkrétní sadu uživatelů nebo konkrétní sadu rolí uživatele. Například upravené domovské řadiče v výpis 2 obsahuje dvě nové akce s názvem StephenSecrets() a AdministratorSecrets().

**Výpis 2 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

Pouze uživatel s uživatelským jménem Stephen vyvolat StephenSecrets() akce. Všichni ostatní uživatelé přesměrováni na přihlášení zobrazení. Vlastnost uživatele akceptuje čárkami oddělený seznam názvy uživatelských účtů.

Jenom uživatelé v roli správce můžete vyvolat AdministratorSecrets() akce. Protože Jan je členem skupiny Administrators, Jana vyvolání akce AdministratorSecrets(). Všichni ostatní uživatelé přesměrováni na přihlášení zobrazení. Vlastnost role akceptuje čárkami oddělený seznam názvů rolí.

#### <a name="configuring-authentication"></a>Konfigurace ověřování

V tomto okamžiku možná se Ptáte se se uloží informace o uživatelském účtu a role. Ve výchozím nastavení je informace uložené v databázi SQL Express (RANU) s názvem ASPNETDB.mdf umístěný v aplikaci aplikace MVC\_složku Data. Tato databáze je generován rozhraní ASP.NET automaticky při spuštění pomocí členství.

Chcete-li zobrazit databázi ASPNETDB.mdf v okně Průzkumníka řešení, musíte nejdřív vybrat možnost nabídky projekt, zobrazit všechny soubory.

Nastavit výchozí databázi SQL Express je v pořádku při vývoji aplikace. Pravděpodobně však nebudou chcete použít výchozí ASPNETDB.mdf databáze pro produkční aplikace. V takovém případě můžete změnit, se uloží informace o uživatelském účtu pomocí následujících dvou kroků:

1. Přidat objekty databáze aplikace služby k vaší databázi provozního – změnit připojovací řetězec aplikace tak, aby odkazoval na vaši produkční databázi

Prvním krokem je přidání všechny potřebné databázové objekty (tabulky a uložené procedury) pro vaši produkční databázi. Nejjednodušší způsob, jak přidat tyto objekty pro novou databázi je využít Průvodce instalací serveru SQL pro technologii ASP.NET (viz obrázek 8). Tento nástroj můžete spustit ze skupiny programu Microsoft Visual Studio 2008 otevřením Visual Studio 2008 příkazový řádek a spuštěním následujícího příkazu z příkazového řádku:

aspnet\_regsql

**Obrázek 8 – Průvodce instalací serveru SQL technologie ASP.NET**

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

Průvodce instalací serveru SQL technologie ASP.NET umožňuje vybrat databázi systému SQL Server v síti a nainstalujte všechny databázové objekty, které vyžadují služby aplikace ASP.NET. Databázový server není potřeba nacházet na místním počítači.

> [!NOTE]
> Pokud nechcete použít Průvodce instalací serveru SQL pro ASP.NET, můžete najít skripty SQL pro přidání aplikace služby databázové objekty v následující složce:
> 
> 
> C:\Windows\Microsoft.NET\Framework\v2.0.50727


Po vytvoření nezbytné databázové objekty, budete muset upravit připojení k databázi používat aplikaci MVC. Upravte připojovací řetězec ApplicationServices v souboru webové konfigurace (web.config) tak, aby ukazoval na produkční databázi. Například upravené připojení v výpis 3 odkazuje na databázi s názvem MyProductionDB (původní připojovací řetězec ApplicationServices byla změněna na komentář).

**Výpis 3 – soubor Web.config**

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Konfigurace oprávnění databáze

Pokud používáte integrované zabezpečení pro připojení k databázi je potřeba přidat správné uživatelský účet systému Windows jako přihlašovací údaje k vaší databázi. Správný účet, závisí na tom, jestli používáte vývojový Server ASP.NET nebo služby IIS jako webový server. Správné uživatelský účet také závisí na operačním systému.

Pokud používáte je ASP.NET Development Server (výchozí webový server využívá sada Visual Studio) vaší aplikace spustí v kontextu uživatelského účtu systému Windows. V takovém případě musíte přidat uživatelský účet systému Windows jako server přihlášení databáze.

Případně pokud používáte Internetová informační služba bude nutné přidání účtu ASPNET nebo účet NT AUTORITY nebo síťové služby jako server přihlášení databáze. Pokud používáte systém Windows XP přidejte účtu ASPNET jako přihlašovací údaje k vaší databázi. Pokud používáte novější operačního systému, například Windows Vista nebo Windows Server 2008, přidejte účet služby NT AUTORITY nebo SÍŤOVÝCH jako přihlášení k databázi.

Můžete přidat nový uživatelský účet k vaší databázi pomocí Microsoft SQL Server Management Studio (viz obrázek 9).

**Obrázek 9 – vytvoření nové přihlašovací údaje systému Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

Jakmile vytvoříte požadované přihlášení, budete muset mapovat přihlášení uživatele databáze s správné databázové role. Dvakrát klikněte na přihlášení a vyberte kartu Mapování uživatele. Vyberte jednu nebo více rolí databáze aplikace služby. Například, aby bylo možné ověřovat uživatele, je nutné povolit aspnet\_členství\_BasicAccess databázové role. Chcete-li vytvořit nové uživatele, musíte povolit aspnet\_členství\_FullAccess databázové role (viz obrázek 10).

**Obrázek 10: přidání role databáze aplikačních služeb**

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a>Souhrn

V tomto kurzu jste zjistili, jak používat ověřování pomocí formulářů, při vytváření aplikace ASP.NET MVC. Nejprve jste zjistili, jak vytvořit nové uživatele a role a využívají nástroj pro správu webu. V dalším kroku jste zjistili, jak pomocí atributu [autorizovat] zabránit neoprávněným uživatelům v vyvolání akce kontroleru. Nakonec jste zjistili, jak nakonfigurovat aplikace MVC uložit uživatele a informace o rolích do provozní databáze.

> [!div class="step-by-step"]
> [Předchozí](preventing-javascript-injection-attacks-cs.md)
> [další](authenticating-users-with-windows-authentication-vb.md)
