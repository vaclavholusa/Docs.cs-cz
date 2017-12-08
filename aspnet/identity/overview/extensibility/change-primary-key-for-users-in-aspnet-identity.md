---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: "Změnit primární klíč pro uživatele v identitě ASP.NET Identity | Microsoft Docs"
author: tfitzmac
description: "Ve Visual Studiu 2013 se výchozí webová aplikace používá hodnotu řetězce pro klíč pro uživatelské účty. ASP.NET Identity umožňuje změnit typ..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>Změnit primární klíč pro uživatele v identitě ASP.NET Identity
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Ve Visual Studiu 2013 se výchozí webová aplikace používá hodnotu řetězce pro klíč pro uživatelské účty. ASP.NET Identity umožňuje změnit typ klíče podle požadavků vaší data. Můžete například změnit typ klíče z řetězce na celé číslo.
> 
> Toto téma ukazuje, jak spustit s výchozí webové aplikace a změňte klíč účtu uživatele na celé číslo. Stejné změny můžete použít k implementaci libovolného typu klíč ve vašem projektu. Ukazuje, jak tyto změny provést ve výchozím nastavení webové aplikace, ale může použít podobné úpravy vlastní aplikace. Zobrazuje změny potřebné při práci s MVC nebo webového formuláře.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Visual Studio 2013 s aktualizací 2 (nebo novější)
> - ASP.NET Identity 2.1 nebo novější


K provedení kroků v tomto kurzu, musí mít Visual Studio 2013 Update 2 (nebo novější) a webové aplikace vytvořené z šablony webové aplikace ASP.NET. Šablona změnit ve verzi Update 3. Toto téma ukazuje, jak změnit šablonu v Update 2 a Update 3.

Toto téma obsahuje následující oddíly:

- [Změnit typ klíče v třídu Identity uživatelů](#userclass)
- [Přidat vlastní třídy Identity, které používají typ klíče](#customclass)
- [Změna kontextu třídy a uživatel správce chcete použít typ klíče](#context)
- [Změna konfigurace spuštění používat typ klíče](#startup)
- [Pro MVC s aktualizací 2 změňte AccountController předat typ klíče](#mvcupdate2)
- [Pro MVC s aktualizací 3 změňte AccountController a ManageController předávat typ klíče](#mvcupdate3)
- [Pro webových formulářů s aktualizací 2 změňte účet stránky předat typ klíče](#webformsupdate2)
- [Pro webových formulářů s aktualizací 3 změňte účet stránky předat typ klíče](#webformsupdate3)
- [Spuštění aplikace](#run)
- [Další prostředky](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Změnit typ klíče v třídu Identity uživatelů

Ve vašem projektu vytvořené z šablony webové aplikace ASP.NET určíte, že třídě ApplicationUser používat celé pro klíč pro uživatelské účty. IdentityModels.cs, změňte třídě ApplicationUser dědění z IdentityUser, která má typ **int** pro obecný parametr TKey. Také předáte názvy tři vlastní třídy, které nebyly dosud implementována.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Změnili jste typ klíče, ale ve výchozím nastavení, zbývající aplikace stále předpokládá, že klíč je řetězec. Je třeba explicitně určit typ klíče v kódu, který předpokládá řetězec.

V **ApplicationUser** třídy, změňte **GenerateUserIdentityAsync** tak, aby zahrnoval int, jak je znázorněno v následující zvýrazněný kód. Tato změna není nutné u projektů webové formuláře se šablonou Update 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Přidat vlastní třídy Identity, které používají typ klíče

Další Identity třídy, jako je například IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, objektu UserStore, úložiště RoleStore, jsou stále nastavit tak, aby použije klíč pro řetězec. Vytvořte nové verze těchto tříd, které zadejte celé číslo pro klíč. Není potřeba poskytnout mnohem implementaci kódu v těchto tříd, především právě nastavujete int jako klíč.

IdentityModels.cs souboru přidejte následující třídy.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Změna kontextu třídy a uživatel správce chcete použít typ klíče

V IdentityModels.cs, změňte definici **ApplicationDbContext** přizpůsobit třídu do nové třídy a **int** klíče, jak je znázorněno v zvýrazněný kód.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Parametr ThrowIfV1Schema již není platný v konstruktoru. Změňte konstruktoru, nesplňuje ThrowIfV1Schema hodnotu.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Otevřete IdentityConfig.cs a změňte **ApplicationUserManger** třídu se má použít nový uživatel uložit třídu pro zachování dat a **int** klíče.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

V šabloně Update 3 musíte změnit ApplicationSignInManager třídy.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Změna konfigurace spuštění používat typ klíče

V Startup.Auth.cs nahraďte kód funkce OnValidateIdentity, jak je znázorněno dole. Všimněte si, že definici getUserIdCallback analyzuje hodnotu řetězce na celé číslo.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Pokud váš projekt nerozpoznává obecnou implementaci **reprezentuje GetUserId** metody, budete muset aktualizovat balíček ASP.NET Identity NuGet s verzí 2.1

Provedli jste mnoho změn infrastruktury třídy používané ASP.NET Identity. Pokud se pokusíte kompilace projektu, si všimnete mnoho chyb. Naštěstí zbývající chyby jsou podobné. Identity – třída očekává celé číslo klíče, ale řadič (nebo webového formuláře) je předávání hodnotu řetězce. V každém případě je potřeba převést řetězec na a celé číslo při volání **reprezentuje GetUserId&lt;int&gt;**. Můžete buď fungovat prostřednictvím z kompilace v seznamu chyb nebo postupujte podle níže změny.

Zbývající změny závisí na typu projektu vytváříte a aktualizace, které jste nainstalovali v sadě Visual Studio. Můžete přejít přímo na odpovídající část prostřednictvím následujících odkazů

- [Pro MVC s aktualizací 2 změňte AccountController předat typ klíče](#mvcupdate2)
- [Pro MVC s aktualizací 3 změňte AccountController a ManageController předávat typ klíče](#mvcupdate3)
- [Pro webových formulářů s aktualizací 2 změňte účet stránky předat typ klíče](#webformsupdate2)
- [Pro webových formulářů s aktualizací 3 změňte účet stránky předat typ klíče](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Pro MVC s aktualizací 2 změňte AccountController předat typ klíče

Otevřete soubor AccountController.cs. Budete muset změnit následující metody.

**ConfirmEmail** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Zrušit přidružení** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Teď můžete [spusťte aplikaci](#run) a registraci nového uživatele.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Pro MVC s aktualizací 3 změňte AccountController a ManageController předávat typ klíče

Otevřete soubor AccountController.cs. Budete muset změnit metodu.

**ConfirmEmail** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Otevřete soubor ManageController.cs. Budete muset změnit následující metody.

**Index** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**RemoveLogin** metody

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**AddPhoneNumber** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**VerifyPhoneNumber** metody

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**Změna hesla byla** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber** – metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Teď můžete [spusťte aplikaci](#run) a registraci nového uživatele.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Pro webových formulářů s aktualizací 2 změňte účet stránky předat typ klíče

Pro webových formulářů s aktualizací 2 budete muset změnit na následujících stránkách.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Teď můžete [spusťte aplikaci](#run) a registraci nového uživatele.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Pro webových formulářů s aktualizací 3 změňte účet stránky předat typ klíče

Pro webových formulářů s aktualizací 3 budete muset změnit na následujících stránkách.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Spuštění aplikace

Dokončení všechny požadované změny pro výchozí šablony webové aplikace. Spusťte aplikaci a zaregistrovat nového uživatele. Po registraci uživatele, si všimnete, že AspNetUsers tabulka obsahuje sloupec Id, který je celé číslo.

![nový primární klíč](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Pokud jste předtím vytvořili ASP.NET Identity tabulky pomocí jiného primárního klíče, budete muset provést další změny. Pokud je to možné odstraňte stávající databázi. Databáze bude znovu vytvořena s správné návrhu při spuštění webové aplikace a přidat nového uživatele. Pokud se odstranění není možné, spustit migrace code first, chcete-li změnit tabulky. Ale nový primární klíč celé číslo nebude nastavit jako vlastnost SQL IDENTITY v databázi. Je nutné ručně nastavit sloupec Id jako IDENTITY.

<a id="other"></a>
## <a name="other-resources"></a>Další zdroje

- [Přehled zprostředkovatelů vlastní úložiště pro identitu ASP.NET](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migrace existující web z členství SQL na identitě ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migrace dat Universal zprostředkovatele členství a uživatelské profily mají být ASP.NET Identity](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Ukázková aplikace](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) se změněné primárním klíčem
