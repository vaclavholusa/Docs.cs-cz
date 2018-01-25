---
title: "Jádro ASP.NET MVC s EF Core - souběžnosti - 8, 10"
author: tdykstra
description: "Tento kurz ukazuje způsobu řešení konfliktů, když se více uživatelů aktualizace stejné entity ve stejnou dobu."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: eee84fe0fbec6ed772342d09931986994903906a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a><span data-ttu-id="5f5e2-103">Zpracování konfliktů souběžnosti – základní EF s kurz k ASP.NET MVC jádra (8 10)</span><span class="sxs-lookup"><span data-stu-id="5f5e2-103">Handling concurrency conflicts - EF Core with ASP.NET Core MVC tutorial (8 of 10)</span></span>

<span data-ttu-id="5f5e2-104">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5f5e2-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5f5e2-105">Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="5f5e2-106">Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).</span><span class="sxs-lookup"><span data-stu-id="5f5e2-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="5f5e2-107">V dřívějších kurzy zjistili, jak aktualizovat data.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-107">In earlier tutorials, you learned how to update data.</span></span> <span data-ttu-id="5f5e2-108">Tento kurz ukazuje způsobu řešení konfliktů, když se více uživatelů aktualizace stejné entity ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-108">This tutorial shows how to handle conflicts when multiple users update the same entity at the same time.</span></span>

<span data-ttu-id="5f5e2-109">Vytvoříte webové stránky, které pracovat entity oddělení a zpracování chyb souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-109">You'll create web pages that work with the Department entity and handle concurrency errors.</span></span> <span data-ttu-id="5f5e2-110">Na následujících obrázcích je upravit a odstranit stránky, včetně některé zprávy, které se zobrazí, pokud dojde ke konfliktu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-110">The following illustrations show the Edit and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Stránka Upravit oddělení](concurrency/_static/edit-error.png)

![Odstranit stránky oddělení](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="5f5e2-113">Konflikty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="5f5e2-113">Concurrency conflicts</span></span>

<span data-ttu-id="5f5e2-114">Concurrency dojde ke konfliktu jeden uživatel zobrazí entity data ji Pokud chcete upravit, a pak jiný uživatel aktualizuje stejné entity data před první uživatel změnu je zapsána do databáze.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-114">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="5f5e2-115">Pokud nepovolíte detekce takové konflikty, kdo aktualizuje databázi přepíše poslední změny jiného uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-115">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="5f5e2-116">V mnoha aplikacích, je přijatelné toto riziko: Pokud existuje několik uživatelů nebo několik aktualizací, nebo pokud není opravdu důležité, pokud jsou některé změny přepsány, náklady na programování pro concurrency vyváží výhody.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-116">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="5f5e2-117">V takovém případě nemáte konfigurace aplikace pro zpracování konfliktů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-117">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="5f5e2-118">Pesimistické souběžnosti (uzamčení)</span><span class="sxs-lookup"><span data-stu-id="5f5e2-118">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="5f5e2-119">Pokud aplikace třeba zabránit, aby před náhodnou ztrátou dat v concurrency scénáře, je jeden ze způsobů použití uzamčení databáze.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-119">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="5f5e2-120">Tomu se říká pesimistické souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-120">This is called pessimistic concurrency.</span></span> <span data-ttu-id="5f5e2-121">Například předtím, než se pustíte do čtení řádek z databáze, můžete požádat o zámku pro jen pro čtení nebo pro přístup k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-121">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="5f5e2-122">Pokud zamknete řádek pro přístup k aktualizaci, je povoleno uzamčení řádek buď pro žádní jiní uživatelé jen pro čtení nebo aktualizace přístup, protože by získají kopii dat, která je právě probíhá změnit.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-122">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="5f5e2-123">Pokud zamknete řádek pro oprávnění jen pro čtení, ostatní také možné zamknout ho pro přístup jen pro čtení, ale není pro aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-123">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="5f5e2-124">Správa zámky má nevýhody.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-124">Managing locks has disadvantages.</span></span> <span data-ttu-id="5f5e2-125">Může být složité programu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-125">It can be complex to program.</span></span> <span data-ttu-id="5f5e2-126">Vyžaduje významné databáze správy prostředků, a jako počet uživatelů, aplikace, může to způsobit problémy s výkonem zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-126">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="5f5e2-127">Z těchto důvodů ne všechny databázové systémy podporovat pesimistické souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-127">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="5f5e2-128">Entity Framework Core poskytuje žádné integrovanou podporu pro něj, a v tomto kurzu není ukazují, jak ho implementovat.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-128">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="5f5e2-129">Optimistickou metodu souběžného zpracování</span><span class="sxs-lookup"><span data-stu-id="5f5e2-129">Optimistic Concurrency</span></span>

<span data-ttu-id="5f5e2-130">Alternativa k pesimistické souběžnosti je optimistickou metodu souběžného.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-130">The alternative to pessimistic concurrency is optimistic concurrency.</span></span> <span data-ttu-id="5f5e2-131">Optimistickou metodu souběžného znamená povolení konfliktů souběžnosti provést a reaktivní správně, pokud tomu tak je.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-131">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="5f5e2-132">Například Jana navštíví oddělení upravit stránku a změní velikost nároky pro angličtinu oddělení z $350,000.00 na 0,00 Kč.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-132">For example, Jane visits the Department Edit page and changes the Budget amount for the English department from $350,000.00 to $0.00.</span></span>

![Změna nároky na 0](concurrency/_static/change-budget.png)

<span data-ttu-id="5f5e2-134">Před kliknutím na Jana **Uložit**, Jan navštíví stejné stránce a změny pole Počáteční datum 9/1/2013 z 9/1/2007.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-134">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Změna počáteční datum na 2013](concurrency/_static/change-date.png)

<span data-ttu-id="5f5e2-136">Jana klikne **Uložit** první a zobrazí ji změnit, když prohlížeč vrátí na indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-136">Jane clicks **Save** first and sees her change when the browser returns to the Index page.</span></span>

![Nároky změnit tak, aby nula.](concurrency/_static/budget-zero.png)

<span data-ttu-id="5f5e2-138">Pak klikne na tlačítko Jan **Uložit** na stránku úpravy, která se zobrazuje nároky $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-138">Then John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="5f5e2-139">Co se stane dále je určen jak zpracování konfliktů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-139">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="5f5e2-140">Mezi možnosti patří následující:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-140">Some of the options include the following:</span></span>

* <span data-ttu-id="5f5e2-141">Můžete udržovat přehled o vlastností, které uživatel změnil a aktualizovat na odpovídající sloupce v databázi.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-141">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

     <span data-ttu-id="5f5e2-142">V ukázkovém scénáři by byl žádná data ztratí, protože různé vlastnosti aktualizoval dva uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-142">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="5f5e2-143">Při příštím někdo umožňuje anglické oddělení, uvidí Jana i Jan pro změny – počáteční datum 9/1/2013 a nároky nulové dolarů.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-143">The next time someone browses the English department, they will see both Jane's and John's changes -- a start date of 9/1/2013 and a budget of zero dollars.</span></span> <span data-ttu-id="5f5e2-144">Tato metoda aktualizace může snížit počet konfliktům, ke kterým může dojít ke ztrátě dat, ale nemůže vyhnuli ztrátě dat, pokud konkurenční změn stejnou vlastnost entity.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-144">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="5f5e2-145">Jestli Entity Framework funguje takto závisí na tom, jak implementovat aktualizace kódu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-145">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="5f5e2-146">Je často není praktické ve webové aplikaci, protože může vyžadovat, aby udržení přehledu o všechny původní hodnoty vlastností pro entitu a také nové hodnoty udržovat velkých objemů stavu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-146">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="5f5e2-147">Zachování velkých objemů stavu může ovlivnit výkon aplikace, protože buď vyžaduje prostředky serveru nebo musí být součástí webové stránky (například v skrytá pole) nebo do souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-147">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>

* <span data-ttu-id="5f5e2-148">Můžete je nechat Jan pro změnu Jana změna přepsána.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-148">You can let John's change overwrite Jane's change.</span></span>

     <span data-ttu-id="5f5e2-149">Při příštím někdo umožňuje anglické oddělení, zobrazí se 9/1/2013 a obnovený $350,000.00 hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-149">The next time someone browses the English department, they will see 9/1/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="5f5e2-150">Tento postup se nazývá *klienta Wins* nebo *poslední ve službě Wins* scénář.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-150">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="5f5e2-151">(Všechny hodnoty z klienta přednost co je v úložišti.) Jak jsme uvedli v Úvod do této části, pokud tak učiníte jakéhokoli kódování pro zpracování souběžnosti, to se stane automaticky.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-151">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>

* <span data-ttu-id="5f5e2-152">Jan pro změnu může zabránit aktualizaci v databázi.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-152">You can prevent John's change from being updated in the database.</span></span>

     <span data-ttu-id="5f5e2-153">By obvykle, zobrazí se chybová zpráva, zobrazit jeho aktuální stav data a povolit mu jeho změny znovu použijte, pokud chce je provést.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-153">Typically, you would display an error message, show him the current state of the data, and allow him to reapply his changes if he still wants to make them.</span></span> <span data-ttu-id="5f5e2-154">Tento postup se nazývá *Wins úložiště* scénář.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-154">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="5f5e2-155">(Hodnoty úložiště dat mají přednost před odeslané klientem hodnoty.) V tomto kurzu budete implementovat scénář úložiště služby Wins.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-155">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="5f5e2-156">Tato metoda zajišťuje, že jsou bez uživatele se zobrazí upozornění, na co se děje přepsat žádné změny.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-156">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="5f5e2-157">Zjišťování konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="5f5e2-157">Detecting concurrency conflicts</span></span>

<span data-ttu-id="5f5e2-158">Můžete vyřešit konflikty zpracování `DbConcurrencyException` výjimky, které vyvolá rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-158">You can resolve conflicts by handling `DbConcurrencyException` exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="5f5e2-159">Chcete-li vědět, kdy má být vyvolána tyto výjimky, musí být schopna zjistit konflikty rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-159">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="5f5e2-160">Proto musíte nakonfigurovat databázi a datový model správně.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-160">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="5f5e2-161">Některé možnosti aktivace zjišťování konfliktů, patří:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-161">Some options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="5f5e2-162">V tabulce databáze patří sledování sloupec, který slouží k určení, kdy se změnil na řádek.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-162">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="5f5e2-163">Potom můžete nakonfigurovat rozhraní Entity Framework zahrnovat tento sloupec v Where klauzule SQL aktualizace nebo odstranění příkazy.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-163">You can then configure the Entity Framework to include that column in the Where clause of SQL Update or Delete commands.</span></span>

     <span data-ttu-id="5f5e2-164">Datový typ sloupce sledování je obvykle `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-164">The data type of the tracking column is typically `rowversion`.</span></span> <span data-ttu-id="5f5e2-165">`rowversion` Hodnota je pořadové číslo, které se zvýší pokaždé, když se aktualizuje na řádek.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-165">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="5f5e2-166">V příkazu Update nebo Delete Where klauzule obsahuje původní hodnota sloupce sledování (původní verze řádku).</span><span class="sxs-lookup"><span data-stu-id="5f5e2-166">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version) .</span></span> <span data-ttu-id="5f5e2-167">Pokud se změnila řádek aktualizován jiným uživatelem, hodnota v `rowversion` sloupec je jiná než původní hodnota, takže příkaz Update nebo Delete nelze najít řádek, abyste aktualizovat z důvodu Where klauzule.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-167">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="5f5e2-168">Když najde Entity Framework, že byly aktualizovány žádné řádky aktualizace nebo odstranění příkazů (to znamená, když počet ovlivněných řádků je nulová), interpretuje, jako konflikt souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-168">When the Entity Framework finds that no rows have been updated by the Update or Delete command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>

* <span data-ttu-id="5f5e2-169">Nakonfigurujte rozhraní Entity Framework zahrnují původní hodnoty každý sloupec v tabulce v Where klauzuli Update a Delete.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-169">Configure the Entity Framework to include the original values of every column in the table in the Where clause of Update and Delete commands.</span></span>

     <span data-ttu-id="5f5e2-170">Jako první možnost, pokud se nic v řádku změnila vzhledem k tomu, že byl řádek nejdřív přečíst Where klauzule nevrátí řádek aktualizace, které rozhraní Entity Framework interpretuje jako konflikt souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-170">As in the first option, if anything in the row has changed since the row was first read, the Where clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="5f5e2-171">Pro tabulky databáze, které mají mnoho sloupců, tento přístup má za následek velké tam, kde klauzule a může vyžadovat udržovat velkých objemů stavu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-171">For database tables that have many columns, this approach can result in very large Where clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="5f5e2-172">Jak již bylo uvedeno dříve, údržbu velkých objemů stavu může ovlivnit výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-172">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="5f5e2-173">Proto se obecně nedoporučuje tento přístup, a není to metoda použitá v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-173">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

     <span data-ttu-id="5f5e2-174">Pokud chcete implementovat tento přístup k concurrency, je nutné označit všechny vlastnosti primárního klíče entity, kterou chcete sledovat souběžnosti pro přidáním `ConcurrencyCheck` je atribut.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-174">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the `ConcurrencyCheck` attribute to them.</span></span> <span data-ttu-id="5f5e2-175">Tato změna umožňuje zahrnout všechny sloupce v klauzuli Where příkazu SQL příkazy Update a Delete rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-175">That change enables the Entity Framework to include all columns in the SQL Where clause of Update and Delete statements.</span></span>

<span data-ttu-id="5f5e2-176">Ve zbývající části tohoto kurzu přidáte `rowversion` sledování vlastnost entity oddělení, vytvořte řadič a zobrazení a otestovat a ověřit, že všechno funguje správně.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-176">In the remainder of this tutorial you'll add a `rowversion` tracking property to the Department entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="5f5e2-177">Přidat vlastnost sledování do oddělení entity</span><span class="sxs-lookup"><span data-stu-id="5f5e2-177">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="5f5e2-178">V *Models/Department.cs*, přidejte sledování vlastnost s názvem RowVersion:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-178">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="5f5e2-179">`Timestamp` Atribut určuje, že v tomto sloupci, budou zahrnuty v Where klauzuli Update a Delete odeslal do databáze.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-179">The `Timestamp` attribute specifies that this column will be included in the Where clause of Update and Delete commands sent to the database.</span></span> <span data-ttu-id="5f5e2-180">Atribut se nazývá `Timestamp` protože předchozí verze systému SQL Server používá SQL `timestamp` datového typu než SQL `rowversion` jej nahradit.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-180">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` replaced it.</span></span> <span data-ttu-id="5f5e2-181">Typ formátu .NET pro `rowversion` je bajtové pole.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-181">The .NET type for `rowversion` is a byte array.</span></span>

<span data-ttu-id="5f5e2-182">Pokud dáváte přednost použijte rozhraní fluent API, můžete použít `IsConcurrencyToken` – metoda (v *Data/SchoolContext.cs*) k určení vlastnosti, sledování, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-182">If you prefer to use the fluent API, you can use the `IsConcurrencyToken` method (in *Data/SchoolContext.cs*) to specify the tracking property, as shown in the following example:</span></span>

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

<span data-ttu-id="5f5e2-183">Přidáním vlastnosti jste změnili model databáze, takže je třeba provést další migraci.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-183">By adding a property you changed the database model, so you need to do another migration.</span></span>

<span data-ttu-id="5f5e2-184">Uložte změny a sestavte projekt a potom zadejte následující příkazy v příkazovém okně:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-184">Save your changes and build the project, and then enter the following commands in the command window:</span></span>

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a><span data-ttu-id="5f5e2-185">Vytvořit řadič oddělení a zobrazení</span><span class="sxs-lookup"><span data-stu-id="5f5e2-185">Create a Departments controller and views</span></span>

<span data-ttu-id="5f5e2-186">Stejně jako dříve pro studenty, kurzy a vyučující vygenerujte řadič oddělení a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-186">Scaffold a Departments controller and views as you did earlier for Students, Courses, and Instructors.</span></span>

![Oddělení vygenerované uživatelské rozhraní](concurrency/_static/add-departments-controller.png)

<span data-ttu-id="5f5e2-188">V *DepartmentsController.cs* souboru, změňte všechny čtyři výskyty "FirstMidName" na "FullName" tak, aby oddělení správce rozevírací seznamy bude obsahovat celý název lektorem a nikoli pouze poslední název.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-188">In the *DepartmentsController.cs* file, change all four occurrences of "FirstMidName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a><span data-ttu-id="5f5e2-189">Aktualizace zobrazení Index oddělení</span><span class="sxs-lookup"><span data-stu-id="5f5e2-189">Update the Departments Index view</span></span>

<span data-ttu-id="5f5e2-190">Generování uživatelského rozhraní stroj vytvořil RowVersion sloupec v indexu zobrazení, ale toto pole by se neměly zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-190">The scaffolding engine created a RowVersion column in the Index view, but that field shouldn't be displayed.</span></span>

<span data-ttu-id="5f5e2-191">Nahraďte kód v *Views/Departments/Index.cshtml* následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-191">Replace the code in *Views/Departments/Index.cshtml* with the following code.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

<span data-ttu-id="5f5e2-192">To změní záhlaví "Oddělení", odstraní sloupec RowVersion a zobrazuje úplný název místo křestní jméno pro správce.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-192">This changes the heading to "Departments", deletes the RowVersion column, and shows full name instead of first name for the administrator.</span></span>

## <a name="update-the-edit-methods-in-the-departments-controller"></a><span data-ttu-id="5f5e2-193">Aktualizace metod úpravy v kontroleru oddělení</span><span class="sxs-lookup"><span data-stu-id="5f5e2-193">Update the Edit methods in the Departments controller</span></span>

<span data-ttu-id="5f5e2-194">V obou třídy MetadataExchangeClientMode `Edit` metoda a `Details` metody přidat `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-194">In both the HttpGet `Edit` method and the `Details` method, add `AsNoTracking`.</span></span> <span data-ttu-id="5f5e2-195">V třídy MetadataExchangeClientMode `Edit` metody přidat přes načítání pro správce.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-195">In the HttpGet `Edit` method, add eager loading for the Administrator.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

<span data-ttu-id="5f5e2-196">Nahraďte stávající kód httppost `Edit` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-196">Replace the existing code for the HttpPost `Edit` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

<span data-ttu-id="5f5e2-197">Kód začíná při pokusu o čtení oddělení aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-197">The code begins by trying to read the department to be updated.</span></span> <span data-ttu-id="5f5e2-198">Pokud `SingleOrDefaultAsync` metoda vrátí hodnotu null, z oddělení byla odstraněna jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-198">If the `SingleOrDefaultAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="5f5e2-199">V takovém případě kód používá hodnoty odeslaného formuláře vytvořit entitu oddělení tak, aby stránce Upravit můžete zobrazí znovu, zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-199">In that case the code uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="5f5e2-200">Jako alternativu nebude muset znovu vytvořit entitu oddělení, pokud se zobrazí pouze se chybová zpráva bez opakované zobrazování pole oddělení.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-200">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="5f5e2-201">Zobrazení ukládá původní `RowVersion` obdrží tuto hodnotu v hodnotě ve skrytém poli a tato metoda `rowVersion` parametr.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-201">The view stores the original `RowVersion` value in a hidden field, and this method receives that value in the `rowVersion` parameter.</span></span> <span data-ttu-id="5f5e2-202">Před voláním `SaveChanges`, budete muset uvést, původní `RowVersion` hodnoty vlastností v `OriginalValues` kolekce pro entitu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-202">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span>

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

<span data-ttu-id="5f5e2-203">Pak když rozhraní Entity Framework vytvoří příkaz SQL aktualizace, tento příkaz bude obsahovat klauzuli WHERE, která vypadá pro řádek, který má původní `RowVersion` hodnota.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-203">Then when the Entity Framework creates a SQL UPDATE command, that command will include a WHERE clause that looks for a row that has the original `RowVersion` value.</span></span> <span data-ttu-id="5f5e2-204">Pokud se příkaz UPDATE žádné řádky (žádné řádky mají původní `RowVersion` hodnotu), vyvolá rozhraní Entity Framework `DbUpdateConcurrencyException` výjimka.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-204">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value),  the Entity Framework throws a `DbUpdateConcurrencyException` exception.</span></span>

<span data-ttu-id="5f5e2-205">Kód v bloku catch pro této výjimky získá ovlivněných oddělení entita, která má aktualizovanými hodnotami z `Entries` vlastnost na objekt výjimky.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-205">The code in the catch block for that exception gets the affected Department entity that has the updated values from the `Entries` property on the exception object.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

<span data-ttu-id="5f5e2-206">`Entries` Kolekce budou mít pouze jeden `EntityEntry` objektu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-206">The `Entries` collection will have just one `EntityEntry` object.</span></span>  <span data-ttu-id="5f5e2-207">Tento objekt můžete získat nové hodnoty zadané uživatelem a hodnot v aktuální databázi.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-207">You can use that object to get the new values entered by the user and the current database values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

<span data-ttu-id="5f5e2-208">Kód přidá vlastní chybové zprávy pro každý sloupec, který má jinou hodnot v databázi z jaké zadané uživatelem na úpravy stránky (pouze jedno pole je tady zobrazené jako stručný výtah).</span><span class="sxs-lookup"><span data-stu-id="5f5e2-208">The code adds a custom error message for each column that has database values different from what the user entered on the Edit page (only one field is shown here for brevity).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

<span data-ttu-id="5f5e2-209">Nakonec kód nastaví `RowVersion` hodnotu `departmentToUpdate` na novou hodnotu načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-209">Finally, the code sets the `RowVersion` value of the `departmentToUpdate` to the new value retrieved from the database.</span></span> <span data-ttu-id="5f5e2-210">Tento nový `RowVersion` hodnota bude uložena ve skrytém poli, když úpravy stránka se zobrazí znovu a další čas uživatel klikne na **Uložit**, pouze souběžného zpracování chyb, které dojít, protože vzniká, redisplay upravit stránky.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-210">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

<span data-ttu-id="5f5e2-211">`ModelState.Remove` Příkaz není nutná, protože `ModelState` má starý `RowVersion` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-211">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="5f5e2-212">V zobrazení `ModelState` hodnota pole má přednost před hodnoty vlastností modelu, pokud obě existuje.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-212">In the view, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-department-edit-view"></a><span data-ttu-id="5f5e2-213">Aktualizace zobrazení upravit oddělení</span><span class="sxs-lookup"><span data-stu-id="5f5e2-213">Update the Department Edit view</span></span>

<span data-ttu-id="5f5e2-214">V *Views/Departments/Edit.cshtml*, proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-214">In *Views/Departments/Edit.cshtml*, make the following changes:</span></span>

* <span data-ttu-id="5f5e2-215">Přidání skrytá pole Uložit `RowVersion` hodnotu vlastnosti, hned za skryté pole pro `DepartmentID` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-215">Add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property.</span></span>

* <span data-ttu-id="5f5e2-216">Přidejte do seznamu rozevíracího seznamu možnost "Vyberte Administrator".</span><span class="sxs-lookup"><span data-stu-id="5f5e2-216">Add a "Select Administrator" option to the drop-down list.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a><span data-ttu-id="5f5e2-217">Test souběžnosti konfliktů na stránce Upravit</span><span class="sxs-lookup"><span data-stu-id="5f5e2-217">Test concurrency conflicts in the Edit page</span></span>

<span data-ttu-id="5f5e2-218">Spusťte aplikaci a přejděte na stránku oddělení Index.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-218">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="5f5e2-219">Klikněte pravým tlačítkem myši **upravit** hypertextový odkaz pro angličtinu oddělení a vyberte **otevřít na nové kartě**, klikněte **upravit** hypertextový odkaz pro angličtinu oddělení.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-219">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**, then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="5f5e2-220">Karty dvě prohlížeče zobrazuje teď stejné informace.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-220">The two browser tabs now display the same information.</span></span>

<span data-ttu-id="5f5e2-221">Změňte pole na první kartě prohlížeče a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-221">Change a field in the first browser tab and click **Save**.</span></span>

![Upravit oddělení stránka 1 po změně](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="5f5e2-223">Prohlížeč zobrazí stránku Index s změněné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-223">The browser shows the Index page with the changed value.</span></span>

<span data-ttu-id="5f5e2-224">Změňte pole v druhé kartu prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-224">Change a field in the second browser tab.</span></span>

![Upravit oddělení stránka 2 po změně](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="5f5e2-226">Klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-226">Click **Save**.</span></span> <span data-ttu-id="5f5e2-227">Zobrazí chybovou zprávu:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-227">You see an error message:</span></span>

![Oddělení upravit stránku chybová zpráva](concurrency/_static/edit-error.png)

<span data-ttu-id="5f5e2-229">Klikněte na tlačítko **Uložit** znovu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-229">Click **Save** again.</span></span> <span data-ttu-id="5f5e2-230">Hodnota, kterou jste zadali na kartě druhý prohlížeče je uložit.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-230">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="5f5e2-231">Zobrazí uložené hodnoty, když se zobrazí stránka indexu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-231">You see the saved values when the Index page appears.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="5f5e2-232">Odstranit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="5f5e2-232">Update the Delete page</span></span>

<span data-ttu-id="5f5e2-233">Rozhraní Entity Framework pro stránku odstranit zjistí souběžnosti konflikty způsobené někdo jinak úpravy oddělení podobným způsobem.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-233">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="5f5e2-234">Když třídy MetadataExchangeClientMode `Delete` metoda zobrazí potvrzení zobrazení, zobrazení zahrnuje původní `RowVersion` hodnota ve skrytém poli.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-234">When the HttpGet `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="5f5e2-235">Hodnota je pak možné HttpPost `Delete` metoda, která je volána, když uživatel potvrdí odstranění.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-235">That value is then available to the HttpPost `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="5f5e2-236">Rozhraní Entity Framework vytvoří příkaz SQL odstranit, obsahuje klauzuli WHERE s původní `RowVersion` hodnota.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-236">When the Entity Framework creates the SQL DELETE command, it includes a WHERE clause with the original `RowVersion` value.</span></span> <span data-ttu-id="5f5e2-237">Pokud výsledky příkazu v nulový počet řádků (tj. řádek byl změněn, jakmile se zobrazí stránka potvrzení odstranění), je vyvolána výjimka souběžnosti a třídy MetadataExchangeClientMode `Delete` metoda je volána k chybě příznak nastaven na hodnotu true, chcete-li znovu zobrazit potvrzovací stránku s chybovou zprávou.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-237">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the HttpGet `Delete` method is called with an error flag set to true in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="5f5e2-238">Je také možné, že byly ovlivňuje nulový počet řádků, protože řádek byl odstraněn jiným uživatelem, tak v takovém případě se zobrazí žádná chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-238">It's also possible that zero rows were affected because the row was deleted by another user, so in that case no error message is displayed.</span></span>

### <a name="update-the-delete-methods-in-the-departments-controller"></a><span data-ttu-id="5f5e2-239">Aktualizace metod odstranit v kontroleru oddělení</span><span class="sxs-lookup"><span data-stu-id="5f5e2-239">Update the Delete methods in the Departments controller</span></span>

<span data-ttu-id="5f5e2-240">V *DepartmentsController.cs*, nahraďte třídy MetadataExchangeClientMode `Delete` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-240">In *DepartmentsController.cs*, replace the HttpGet `Delete` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

<span data-ttu-id="5f5e2-241">Metodu je možné zadat volitelný parametr, který označuje, zda stránky se se zobrazí znovu po chybě souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-241">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="5f5e2-242">Pokud tento příznak má hodnotu true a oddělení zadaný už existuje, byla odstraněna jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-242">If this flag is true and the department specified no longer exists, it was deleted by another user.</span></span> <span data-ttu-id="5f5e2-243">V takovém případě kód přesměruje na indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-243">In that case, the code redirects to the Index page.</span></span>  <span data-ttu-id="5f5e2-244">Pokud tento příznak má hodnotu true a oddělení neexistuje, bylo změněno jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-244">If this flag is true and the Department does exist, it was changed by another user.</span></span> <span data-ttu-id="5f5e2-245">V takovém případě kód odešle chybovou zprávu pomocí zobrazení `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-245">In that case, the code sends an error message to the view using `ViewData`.</span></span>  

<span data-ttu-id="5f5e2-246">Nahraďte kód v HttpPost `Delete` – metoda (s názvem `DeleteConfirmed`) s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-246">Replace the code in the HttpPost `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

<span data-ttu-id="5f5e2-247">V automaticky generovaný kód, který právě nahrazen tato metoda povoleny pouze ID záznamu:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-247">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

<span data-ttu-id="5f5e2-248">Změnili jste tento parametr k oddělení instanci entity vytvořené vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-248">You've changed this parameter to a Department entity instance created by the model binder.</span></span> <span data-ttu-id="5f5e2-249">To umožňuje přístup EF hodnota vlastnosti RowVersion kromě klíč záznamu.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-249">This gives EF access to the RowVersion property value in addition to the record key.</span></span>

```csharp
public async Task<IActionResult> Delete(Department department)
```

<span data-ttu-id="5f5e2-250">Také jste změnili název metody akce z `DeleteConfirmed` k `Delete`.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-250">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="5f5e2-251">Automaticky generovaný kód používá název `DeleteConfirmed` umožnit metoda HttpPost jedinečný podpis.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-251">The scaffolded code used the name `DeleteConfirmed` to give the HttpPost method a unique signature.</span></span> <span data-ttu-id="5f5e2-252">(Modulu CLR vyžaduje přetížené metody, mít parametry jinou metodu.) Teď, když podpisy jsou jedinečné, můžete přilepit s konvence MVC a použít pro odstranění metody HttpPost a třídy MetadataExchangeClientMode stejný název.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-252">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the HttpPost and HttpGet delete methods.</span></span>

<span data-ttu-id="5f5e2-253">Pokud oddělení byl již odstraněn, `AnyAsync` metoda vrátí hodnotu false a aplikace právě přejde zpět na metodu Index.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-253">If the department is already deleted, the `AnyAsync` method returns false and the application just goes back to the Index method.</span></span>

<span data-ttu-id="5f5e2-254">Pokud je chyba souběžnosti zachycena, kód znovu zobrazí stránka potvrzení odstranění a poskytuje příznak, které označují, že by měl zobrazit chybovou zprávu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-254">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="5f5e2-255">Aktualizace zobrazení odstranění</span><span class="sxs-lookup"><span data-stu-id="5f5e2-255">Update the Delete view</span></span>

<span data-ttu-id="5f5e2-256">V *Views/Departments/Delete.cshtml*, nahraďte následující kód, který přidá na pole zpráva Chyba a skrytá pole vlastností DepartmentID a RowVersion automaticky generovaný kód.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-256">In *Views/Departments/Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="5f5e2-257">Změny se zvýrazněnou.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-257">The changes are highlighted.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

<span data-ttu-id="5f5e2-258">Díky následující změny:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-258">This makes the following changes:</span></span>

* <span data-ttu-id="5f5e2-259">Přidá chybovou zprávu mezi `h2` a `h3` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-259">Adds an error message between the `h2` and `h3` headings.</span></span>

* <span data-ttu-id="5f5e2-260">Nahradí FullName v FirstMidName **správce** pole.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-260">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>

* <span data-ttu-id="5f5e2-261">Odebere pole RowVersion.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-261">Removes the RowVersion field.</span></span>

* <span data-ttu-id="5f5e2-262">Přidá skryté pole pro `RowVersion` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-262">Adds a hidden field for the `RowVersion` property.</span></span>

<span data-ttu-id="5f5e2-263">Spusťte aplikaci a přejděte na stránku oddělení Index.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-263">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="5f5e2-264">Klikněte pravým tlačítkem myši **odstranit** hypertextový odkaz pro angličtinu oddělení a vyberte **otevřít na nové kartě**, na první kartě klikněte na **upravit** hypertextový odkaz pro angličtinu oddělení.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-264">Right-click the **Delete** hyperlink for the English department and select **Open in new tab**, then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="5f5e2-265">V prvním okně, změňte jednu z hodnot a klikněte na tlačítko **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="5f5e2-265">In the first window, change one of the values, and click **Save**:</span></span>

![Stránka Upravit oddělení po změně před odstranění](concurrency/_static/edit-after-change-for-delete.png)

<span data-ttu-id="5f5e2-267">Na kartě druhý klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-267">In the second tab, click **Delete**.</span></span> <span data-ttu-id="5f5e2-268">Zobrazí se zpráva Chyba souběžnosti a hodnoty oddělení aktualizovaly s co je aktuálně v databázi.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-268">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Stránka potvrzení odstranění oddělení s chybou souběžnosti](concurrency/_static/delete-error.png)

<span data-ttu-id="5f5e2-270">Pokud kliknete na tlačítko **odstranit** znovu, budete přesměrováni na indexovou stránku, která ukazuje, že byla odstraněna z oddělení.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-270">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="update-details-and-create-views"></a><span data-ttu-id="5f5e2-271">Podrobné informace o aktualizaci a vytvořit zobrazení</span><span class="sxs-lookup"><span data-stu-id="5f5e2-271">Update Details and Create views</span></span>

<span data-ttu-id="5f5e2-272">Můžete volitelně vyčistit automaticky generovaný kód v podrobnostech a vytvořit zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-272">You can optionally clean up scaffolded code in the Details and Create views.</span></span>

<span data-ttu-id="5f5e2-273">Nahraďte kód v *Views/Departments/Details.cshtml* odstranění RowVersion sloupce a zobrazit úplný název tohoto správce.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-273">Replace the code in *Views/Departments/Details.cshtml* to delete the RowVersion column and show the full name of the Administrator.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

<span data-ttu-id="5f5e2-274">Nahraďte kód v *Views/Departments/Create.cshtml* pro přidání do rozevíracího seznamu vyberte možnost.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-274">Replace the code in *Views/Departments/Create.cshtml* to add a Select option to the drop-down list.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a><span data-ttu-id="5f5e2-275">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5f5e2-275">Summary</span></span>

<span data-ttu-id="5f5e2-276">Tím dokončíte Úvod pro zpracování konfliktů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-276">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="5f5e2-277">Další informace o způsobu zpracování souběžnost v EF jádra najdete v tématu [konfliktů souběžnosti](https://docs.microsoft.com/ef/core/saving/concurrency).</span><span class="sxs-lookup"><span data-stu-id="5f5e2-277">For more information about how to handle concurrency in EF Core, see [Concurrency conflicts](https://docs.microsoft.com/ef/core/saving/concurrency).</span></span> <span data-ttu-id="5f5e2-278">Další kurz ukazuje, jak implementovat tabulky za hierarchie dědičnosti pro lektorem a Student entity.</span><span class="sxs-lookup"><span data-stu-id="5f5e2-278">The next tutorial shows how to implement table-per-hierarchy inheritance for the Instructor and Student entities.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5f5e2-279">[Předchozí](update-related-data.md)
[další](inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="5f5e2-279">[Previous](update-related-data.md)
[Next](inheritance.md)</span></span>  
