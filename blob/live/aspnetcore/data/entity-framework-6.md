---
title: Erste Schritte mit ASP.NET Core und Entity Framework 6
author: tdykstra
description: In diesem Artikel wird die Verwendung von Entity Framework 6 in einer ASP.NET Core-Anwendung veranschaulicht.
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 51445b8c110ad618aeb680148ccf4304a45ee16e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="79e97-103">Erste Schritte mit ASP.NET Core und Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="79e97-103">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="79e97-104">Durch [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="79e97-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="79e97-105">In diesem Artikel wird die Verwendung von Entity Framework 6 in einer ASP.NET Core-Anwendung veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="79e97-105">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="79e97-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="79e97-106">Overview</span></span>

<span data-ttu-id="79e97-107">Um Entity Framework 6 verwenden zu können, muss das Projekt für .NET Framework kompilieren, da Entity Framework 6 .NET Core nicht unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="79e97-107">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="79e97-108">Wenn Sie plattformübergreifende Funktionen benötigen, müssen so aktualisieren Sie auf [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="79e97-108">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="79e97-109">Die empfohlene Methode zum Verwenden von Entity Framework 6 in einer ASP.NET Core-Anwendung ist der Kontext EF6 versetzt, und Modellklassen in einer Klassenbibliothek-Projekt, dessen Ziel vollständige Framework.</span><span class="sxs-lookup"><span data-stu-id="79e97-109">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="79e97-110">Fügen Sie einen Verweis auf die Klassenbibliothek aus dem ASP.NET Core-Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="79e97-110">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="79e97-111">Finden Sie im Beispiel [Visual Studio-Projektmappe mit Projekten von EF6 und ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="79e97-111">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="79e97-112">Einen EF6 Kontext kann nicht in einem Projekt auf ASP.NET Core gesetzt werden, da Projekte von .NET Core alle Funktionen nicht unterstützen, die z. B. EF6 Befehle *Enable-Migrations* erfordern.</span><span class="sxs-lookup"><span data-stu-id="79e97-112">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="79e97-113">Unabhängig von Projekttyp, in dem Sie den Kontext EF6 suchen, können nur EF6-Befehlszeilentools mit einem EF6 Kontext aus.</span><span class="sxs-lookup"><span data-stu-id="79e97-113">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="79e97-114">Beispielsweise `Scaffold-DbContext` steht nur in Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="79e97-114">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="79e97-115">Wenn Sie einer Datenbank in einem Modell EF6 zurückentwickeln müssen, finden Sie unter [Code First mit einer vorhandenen Datenbank](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="79e97-115">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="79e97-116">Verweis vollständigen Framework und EF6 im Projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79e97-116">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="79e97-117">Das ASP.NET Core-Projekt muss .NET Framework und EF6 verweisen.</span><span class="sxs-lookup"><span data-stu-id="79e97-117">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="79e97-118">Z. B. die *csproj* -Datei des Projekts zu ASP.NET Core wird ähnlich wie im folgenden Beispiel aussehen (nur die relevanten Teile der Datei werden angezeigt).</span><span class="sxs-lookup"><span data-stu-id="79e97-118">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="79e97-119">Wenn Sie ein neues Projekt erstellen, verwenden Sie die **ASP.NET-Webanwendung für Core ((.NET Framework)** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="79e97-119">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="79e97-120">Behandeln von Verbindungszeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="79e97-120">Handle connection strings</span></span>

<span data-ttu-id="79e97-121">Die EF6-Befehlszeilentools, die Sie in das Klassenbibliotheksprojekt EF6 verwenden erfordern einen Standardkonstruktor damit Kontext instanziiert werden können.</span><span class="sxs-lookup"><span data-stu-id="79e97-121">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="79e97-122">Aber Sie möchten wahrscheinlich angeben, dass die Verbindungszeichenfolge im ASP.NET Core-Projekt in diesem Fall die Kontext-Konstruktor verwendet einen Parameter benötigen, der Sie in der Verbindungszeichenfolge übergeben können.</span><span class="sxs-lookup"><span data-stu-id="79e97-122">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="79e97-123">Hier ist ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="79e97-123">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="79e97-124">Da EF6 Kontext nicht über einen parameterlosen Konstruktor verfügt, wird Ihre EF6-Projekt verfügt über eine Implementierung bereitstellen [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="79e97-124">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="79e97-125">Die Befehlszeilentools EF6 findet und den Kontext instanziiert werden können, dass diese Implementierung einsetzen.</span><span class="sxs-lookup"><span data-stu-id="79e97-125">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="79e97-126">Hier ist ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="79e97-126">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="79e97-127">In diesem Beispielcode die `IDbContextFactory` Implementierung werden in eine hartcodierte Verbindungszeichenfolge übergeben.</span><span class="sxs-lookup"><span data-stu-id="79e97-127">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="79e97-128">Dies ist die Verbindungszeichenfolge, die die Befehlszeilentools verwenden.</span><span class="sxs-lookup"><span data-stu-id="79e97-128">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="79e97-129">Sie sollten eine Strategie, um sicherzustellen, dass die Klassenbibliothek dieselbe Verbindungszeichenfolge verwendet, die die aufrufende Anwendung verwendet.</span><span class="sxs-lookup"><span data-stu-id="79e97-129">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="79e97-130">Beispielsweise konnte den Wert von einer Umgebungsvariablen in beiden Projekten abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="79e97-130">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="79e97-131">Abhängigkeitsinjektion in ASP.NET Core Projekt einrichten</span><span class="sxs-lookup"><span data-stu-id="79e97-131">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="79e97-132">Die Core-Projekts *Startup.cs* Datei, richten Sie den Kontext EF6 für abhängigkeiteneinschleusung (DI) in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="79e97-132">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="79e97-133">EF Kontextobjekte sollte für eine pro Anforderung Lebensdauer begrenzt werden.</span><span class="sxs-lookup"><span data-stu-id="79e97-133">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="79e97-134">Eine Instanz des Kontexts erhalten in Ihren Domänencontrollern von mithilfe der Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="79e97-134">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="79e97-135">Der Code ist ähnlich, was Sie für einen Kontext EF Core schreiben würden:</span><span class="sxs-lookup"><span data-stu-id="79e97-135">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="79e97-136">Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="79e97-136">Sample application</span></span>

<span data-ttu-id="79e97-137">Eine funktionierende beispielanwendung finden Sie unter der [Beispiel Visual Studio-Projektmappe](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) , die in diesem Artikel begleitet.</span><span class="sxs-lookup"><span data-stu-id="79e97-137">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="79e97-138">In diesem Beispiel kann durch die folgenden Schritte in Visual Studio von Grund auf neu erstellt werden:</span><span class="sxs-lookup"><span data-stu-id="79e97-138">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="79e97-139">Erstellen Sie eine Projektmappe an.</span><span class="sxs-lookup"><span data-stu-id="79e97-139">Create a solution.</span></span>

* <span data-ttu-id="79e97-140">**Hinzufügen eines neuen Projekts > Web > ASP.NET Core-Webanwendung ((.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="79e97-140">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="79e97-141">**Hinzufügen eines neuen Projekts > klassische Windows-Desktop >-Klassenbibliothek ((.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="79e97-141">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="79e97-142">In **Package Manager Console** (PMC) für beide Projekte, führen Sie den Befehl `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="79e97-142">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="79e97-143">Erstellen Sie in das Klassenbibliotheksprojekt Modell Datenklassen und Context-Klasse und eine Implementierung von `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="79e97-143">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="79e97-144">Führen Sie die Befehle in PMC für das Klassenbibliotheksprojekt, `Enable-Migrations` und `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="79e97-144">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="79e97-145">Wenn Sie das ASP.NET Core-Projekt als Startprojekt festgelegt haben, fügen Sie `-StartupProjectName EF6` auf diese Befehle.</span><span class="sxs-lookup"><span data-stu-id="79e97-145">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="79e97-146">Fügen Sie in der Core-Projekt einen Projektverweis auf das Klassenbibliotheksprojekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="79e97-146">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="79e97-147">Im Kern-Projekt in *Startup.cs*, registrieren Sie den Kontext für DI.</span><span class="sxs-lookup"><span data-stu-id="79e97-147">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="79e97-148">Im Kern-Projekt in *appsettings.json*, fügen Sie der Verbindungszeichenfolge angegeben.</span><span class="sxs-lookup"><span data-stu-id="79e97-148">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="79e97-149">Fügen Sie das Projekt Core einem Controller und Ansichten, um sicherzustellen, dass Sie lesen und Schreiben von Daten können.</span><span class="sxs-lookup"><span data-stu-id="79e97-149">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="79e97-150">(Beachten Sie, dass ASP.NET Core MVC-Gerüstbau nicht mit dem EF6-Kontext, die von der Klassenbibliothek verwiesen funktioniert.)</span><span class="sxs-lookup"><span data-stu-id="79e97-150">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="79e97-151">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="79e97-151">Summary</span></span>

<span data-ttu-id="79e97-152">Dieser Artikel hat grundlegende Leitfäden zum Verwenden von Entity Framework 6 in einer ASP.NET Core-Anwendung bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="79e97-152">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79e97-153">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="79e97-153">Additional Resources</span></span>

* [<span data-ttu-id="79e97-154">Entity Framework - Code-basierte Konfiguration</span><span class="sxs-lookup"><span data-stu-id="79e97-154">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)