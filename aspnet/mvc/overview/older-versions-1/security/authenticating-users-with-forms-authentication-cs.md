---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: Ověřování uživatelů pomocí formulářů ověřování (C#) | Dokumentace Microsoftu
author: microsoft
description: Zjistěte, jak použít atribut [Authorize] heslo chránit konkrétní stránky v aplikaci MVC. Zjistíte, jak používat správu webu příliš...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 1d06c8f26421cc9859439f664578f75657903a9a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391031"
---
<a name="authenticating-users-with-forms-authentication-c"></a>Ověřování uživatelů pomocí ověřování pomocí formulářů (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> Zjistěte, jak použít atribut [Authorize] heslo chránit konkrétní stránky v aplikaci MVC. Zjistíte, jak používat nástroj Správa webu k vytváření a správě uživatelů a rolí. Také se dozvíte, jak nakonfigurovat ukládat informace o účtu a role uživatele.


Cílem tohoto kurzu je vysvětlují, jak můžete pomocí formulářů ověřování hesla chránit zobrazení v aplikacích ASP.NET MVC. Zjistíte, jak pomocí nástroje pro správu webu vytvoření uživatelů a rolí. Také se dozvíte, jak zabránit neoprávněným uživatelům vyvolání akce kontroleru. Nakonec se dozvíte, jak nakonfigurovat, kde jsou uložená uživatelská jména a hesla.

#### <a name="using-the-web-site-administration-tool"></a>Pomocí nástroje pro správu webu

Před tím, než cokoli jiného, jsme měli spustit tak, že vytvoříte několik uživatelů a rolí. Nejjednodušší způsob, jak vytvořit nové uživatele a role je využít Visual Studio 2008 webový server nástroje pro správu. Tento nástroj můžete spustit tak, že vyberete možnost nabídky **projektu, konfigurace ASP.NET**. Alternativně můžete spustit nástroj Správa webu kliknutím (poněkud scary) ikonu kladivo narazili na světě, který se zobrazí v horní části okna Průzkumníka řešení (viz obrázek 1).

**Obrázek 1 – spouštění nástroje pro správu webu**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

V rámci nástroje pro správu webu můžete vytvořit nové uživatele a role tak, že vyberete kartu zabezpečení. Klikněte na tlačítko **vytvořit uživatele** odkaz pro vytvoření nového uživatele s názvem Stephen (viz obrázek 2). Stephen uživateli poskytnout žádné heslo, které chcete (například *tajný klíč*).

**Obrázek 2 – Vytvoření nového uživatele**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

Vytvoření nové role první povolení role a definováním jedné či několika rolí. Povolit role kliknutím **povolit role** odkaz. Dále vytvořte roli s názvem *správci* kliknutím **vytvořit nebo spravovat role** propojení (viz obrázek 3).

**Obrázek 3: vytvoření nové role**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

Nakonec vytvořte nového uživatele s názvem Sally a přidružte Sally s rolí správce v kliknutím na vytvořit odkaz a následným výběrem správci při vytváření Sally (viz obrázek 4).

**Obrázek 4 – přidání uživatele do role**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

Když všechny říká, že se provádí, byste měli mít dva nové uživatele s názvem Stephen a Sally. Měli byste také novou roli s názvem Správci. Jan je členem role Správci nástroje a Stephen není.

#### <a name="requiring-authorization"></a>Vyžadování ověření

Vyžadujete k ověření, než uživatel vyvolá akci kontroleru tak, že přidáte atribut [Authorize] na akci uživatele. Atribut [Authorize] můžete použít pro jednotlivé řadiče akce nebo můžete použít tento atribut do třídy celý kontroler.

Například kontroler v informacích 1 zpřístupňuje akci s názvem CompanySecrets(). Protože tato akce je upravený pomocí atributu [Authorize], tuto akci nelze vyvolat, pokud je ověření uživatele.

**Výpis 1 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

Vyvolat akci CompanySecrets() zadáním adresy URL/Home/CompanySecrets do adresního řádku prohlížeče a nejste ověřený uživatel, pak budete přesměrováni na zobrazení přihlášení automaticky (viz obrázek 5).

**Obrázek 5 – zobrazení přihlášení**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

Pohled pro přihlášení můžete použít k zadání uživatelského jména a hesla. Pokud si nejste registrovaní uživatelé pak můžete kliknout **zaregistrovat** odkaz přejděte do registru na zobrazení (viz obrázek 6). Zobrazení registru můžete použít k vytvoření nového uživatelského účtu.

**Obrázek 6 – zobrazení registr**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

Po úspěšném přihlášení, zobrazí se CompanySecrets zobrazení (viz obrázek 7). Ve výchozím nastavení budou nadále přihlášeni, dokud nezavřete okno prohlížeče.

**Obrázek 7 – CompanySecrets zobrazení**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Ověřování pomocí uživatelského jména nebo Role uživatele

Atribut [Authorize] můžete použít k omezení přístupu k akci kontroleru na konkrétní sadu uživatelů nebo konkrétní sadu rolí uživatelů. Například upravené kontroler Home výpis 2 obsahuje dvě nové akce s názvem StephenSecrets() a AdministratorSecrets().

**Výpis 2 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

Akci StephenSecrets() lze vyvolat pouze uživatele se uživatelské jméno, Stephen. Všichni ostatní uživatelé získat přesměrováni na zobrazení přihlášení. Vlastnost uživatele přijímá čárkami oddělený seznam názvy uživatelských účtů.

Akci AdministratorSecrets() lze vyvolat pouze uživatelé v roli správce. Jan je členem skupiny Administrators, Jana vyvolání akce AdministratorSecrets(). Všichni ostatní uživatelé získat přesměrováni na zobrazení přihlášení. Vlastnosti role přijímá čárkami oddělený seznam názvů rolí.

#### <a name="configuring-authentication"></a>Konfigurace ověřování

V tomto okamžiku bude vás možná zajímat kam ukládají informace o uživatelském účtu a role. Ve výchozím nastavení, informace jsou uloženy v databázi SQL Express (RANU) s názvem ASPNETDB.mdf umístěný v aplikaci MVC aplikace\_složku Data. Tato databáze je rozhraním ASP.NET automaticky generovat při spuštění pomocí členství.

Chcete-li zobrazit ASPNETDB.mdf databáze v okně Průzkumník řešení, musíte nejprve vyberte možnost nabídky projekt, zobrazit všechny soubory.

Pomocí výchozí databáze SQL Express je v pořádku při vývoji aplikace. Pravděpodobně ale nebudete chtít používat výchozí ASPNETDB.mdf databáze pro produkční aplikace. V takovém případě můžete změnit informace o uživatelském účtu se mají ukládat provedením následujících dvou kroků:

1. Přidat objekty databáze aplikace služby k produkční databázi – změňte připojovací řetězec vaší aplikace tak, aby odkazoval na produkční databáze

Prvním krokem je přidání všechny nezbytné databázových objektů (tabulkám a uloženým procedurám) k produkční databázi. Nejjednodušší způsob, jak přidat tyto objekty do nové databáze je využít Průvodce instalací serveru SQL pro ASP.NET (viz obrázek 8). Tento nástroj můžete spustit tak, že otevřete příkazový řádek sady Visual Studio 2008 ze skupiny programů Microsoft Visual Studio 2008 a spuštěním následujícího příkazu z příkazového řádku:

aspnet\_regsql

**Obrázek 8 – Průvodce instalací serveru SQL technologie ASP.NET**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

Průvodce instalací serveru SQL pro ASP.NET umožňuje vybrat databázi SQL serveru ve vaší síti a nainstalujte všechny databázové objekty nezbytné v aplikačních služeb technologie ASP.NET. Databázový server není musí být umístěné na místním počítači.

> [!NOTE] 
> 
> Pokud už nechcete používat Průvodce instalací serveru SQL pro ASP.NET, můžete najít skripty SQL pro přidání objektů databáze aplikace služby v následující složce:
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727


Po vytvoření nezbytné databázové objekty, budete muset upravit připojení k databázi použít v aplikaci MVC. Upravte ApplicationServices připojovací řetězec v souboru webové konfigurace (web.config) tak, aby odkazovala na produkční databázi. Například upravené připojení v zobrazení 3, odkazuje na databázi s názvem MyProductionDB (původní ApplicationServices připojovacího řetězce byla změněna na komentář).

**Výpis 3 – Web.config**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Konfigurace oprávnění databáze

Pokud používáte integrované zabezpečení k připojení k databázi je potřeba přidat správné uživatelský účet Windows jako přihlašovací údaje k vaší databázi. Správný účet závisí na tom, jestli používáte serveru ASP.NET Development Server nebo Internetová informační služba jako webový server. Správný účet také závisí na operační systém.

Pokud používáte serveru ASP.NET Development Server (výchozí webový server používá sada Visual Studio) vaše aplikace spouští v kontextu uživatelského účtu Windows. V takovém případě musíte přidat uživatelský účet Windows jako přihlašovací server databáze.

Případně pokud používáte Internetová informační služba musíte pro přidání účtu ASPNET nebo účet NT AUTORITY/NETWORK SERVICE jako přihlašovací server databáze. Pokud používáte Windows XP, přidejte účet ASPNET jako přihlašovací údaje k vaší databázi. Pokud používáte novější operační systém, jako je například Windows Vista nebo Windows Server 2008 přidejte účet NT AUTORITY/NETWORK SERVICE jako přihlášení k databázi.

Nový uživatelský účet můžete přidat k vaší databázi pomocí aplikace Microsoft SQL Server Management Studio (viz obrázek 9).

**Obrázek 9: vytvoření nové přihlašovací údaje serveru Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

Po vytvoření vyžaduje přihlášení, budete muset namapovat přihlášení uživatele databáze s rolemi správnou databázi. Klikněte dvakrát na přihlášení a vyberte kartu u mapování uživatele. Vyberte jednu nebo více rolí databáze aplikace služby. Například, aby bylo možné ověřovat uživatele, je nutné povolit aspnet\_členství\_BasicAccess databázové role. Chcete-li vytvořit nové uživatele, je potřeba povolit aspnet\_členství\_FullAccess databázová role (viz obrázek 10).

**Obrázek 10: Přidání aplikačních služeb databázové role**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>Souhrn

V tomto kurzu jste zjistili, jak používat ověřování pomocí formulářů, při sestavování aplikace ASP.NET MVC. Nejdřív jste zjistili, jak k vytvoření nových uživatelů a rolí s využitím nástroje pro správu webu. Dále jste zjistili, jak používat atribut [Authorize] k zabránit neoprávněným uživatelům v vyvolání akce kontroleru. Nakonec jste zjistili, jak nakonfigurovat svoji aplikaci MVC k uložení uživatele a informace o rolích v provozní databázi.

> [!div class="step-by-step"]
> [Next](authenticating-users-with-windows-authentication-cs.md)
