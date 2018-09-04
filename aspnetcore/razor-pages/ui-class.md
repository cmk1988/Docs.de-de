---
title: Wiederverwendbare Razor-Benutzeroberfläche in Klassenbibliotheken mit ASP.NET Core
author: Rick-Anderson
description: Informationen zum Erstellen einer wiederverwendbaren Razor-Benutzeroberfläche in einer Klassenbibliothek
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/21/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 1f0ef59ce3f3294d6a3bde015ca34800770b1be4
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "42910010"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="e38aa-103">Erstellen einer wiederverwendbaren Benutzeroberfläche mithilfe eines RCL-Projekts in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e38aa-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="e38aa-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e38aa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e38aa-105">Ansichten, Seiten, Controller, Seitenmodelle, [Ansichtskomponenten](xref:mvc/views/view-components) und Datenmodelle im Zusammenhang mit Razor können in einer Razor-Klassenbibliothek (Razor Class Library, RCL) zusammengefasst und erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="e38aa-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="e38aa-106">Die RCL kann verpackt und wiederverwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e38aa-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="e38aa-107">Sie kann in Anwendungen eingebunden werden, wodurch sich die in der RCL enthaltenen Ansichten und Seiten überschreiben lassen.</span><span class="sxs-lookup"><span data-stu-id="e38aa-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="e38aa-108">Wenn eine Ansicht, Teilansicht oder Razor-Seite sowohl in der Web-App als auch in der RCL enthalten ist, hat das Razor-Markup (die *CSHTML*-Datei) in der Web-App Vorrang.</span><span class="sxs-lookup"><span data-stu-id="e38aa-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="e38aa-109">Dieses Feature erfordert [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="e38aa-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="e38aa-110">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e38aa-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="e38aa-111">Erstellen einer Klassenbibliothek, die eine Razor-Benutzeroberfläche enthält</span><span class="sxs-lookup"><span data-stu-id="e38aa-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e38aa-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e38aa-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e38aa-113">Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e38aa-114">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="e38aa-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="e38aa-115">Geben Sie der Bibliothek einen Namen (z.B. „RazorClassLib“), und klicken Sie anschließend auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="e38aa-116">Stellen Sie sicher, dass der Bibliotheksname nicht auf `.Views` endet, um zu verhindern, dass es zu einem Dateinamenkonflikt mit der generierten Ansichtsbibliothek kommt.</span><span class="sxs-lookup"><span data-stu-id="e38aa-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="e38aa-117">Überprüfen Sie, ob **ASP.NET Core 2.1** oder höher ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="e38aa-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="e38aa-118">Klicken Sie auf **Razor Class Library** (Razor-Klassenbibliothek) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="e38aa-119">Eine Razor-Klassenbibliothek verfügt über die folgende Projektdatei:</span><span class="sxs-lookup"><span data-stu-id="e38aa-119">A Razor Class Library has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e38aa-120">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e38aa-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e38aa-121">Führen Sie `dotnet new razorclasslib` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="e38aa-121">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="e38aa-122">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e38aa-122">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="e38aa-123">Weitere Informationen finden Sie unter [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="e38aa-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="e38aa-124">Stellen Sie sicher, dass der Bibliotheksname nicht auf `.Views` endet, um zu verhindern, dass es zu einem Dateinamenkonflikt mit der generierten Ansichtsbibliothek kommt.</span><span class="sxs-lookup"><span data-stu-id="e38aa-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

------
<span data-ttu-id="e38aa-125">Fügen Sie der RCL Razor-Dateien hinzu.</span><span class="sxs-lookup"><span data-stu-id="e38aa-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="e38aa-126">ASP.NET Core-Vorlagen wird davon ausgegangen, der RCL Inhalt befindet sich in der *Bereiche* Ordner.</span><span class="sxs-lookup"><span data-stu-id="e38aa-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="e38aa-127">Finden Sie unter [RCL Seitenlayout](#afs) zum Erstellen der Inhalte in einer RCL, die verfügbar gemacht `~/Pages` statt `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="e38aa-127">See [RCL Pages layout](#afs) to create a RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="e38aa-128">Verweisen auf RCL-Inhalte</span><span class="sxs-lookup"><span data-stu-id="e38aa-128">Referencing Razor Class Library content</span></span>

<span data-ttu-id="e38aa-129">Folgende Komponenten können auf die RCL verweisen:</span><span class="sxs-lookup"><span data-stu-id="e38aa-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="e38aa-130">NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="e38aa-130">NuGet package.</span></span> <span data-ttu-id="e38aa-131">Weitere Informationen finden Sie unter [Creating NuGet packages](/nuget/create-packages/creating-a-package) (Erstellen von NuGet-Paketen), [dotnet add package](/dotnet/core/tools/dotnet-add-package) und [Erstellen und Veröffentlichen eines NuGet-Pakets](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="e38aa-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="e38aa-132">*{ProjectName}.csproj*-Dateien.</span><span class="sxs-lookup"><span data-stu-id="e38aa-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="e38aa-133">Weitere Informationen finden Sie unter [dotnet add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="e38aa-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="e38aa-134">Exemplarische Vorgehensweise: Erstellen eines RCL-Projekts und Verwenden des Projekts über ein Razor-Seiten-Projekt</span><span class="sxs-lookup"><span data-stu-id="e38aa-134">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="e38aa-135">Sie können das [vollständige Projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) herunterladen und testen, anstatt es zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e38aa-135">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="e38aa-136">Der Beispieldownload enthält zusätzlichen Code sowie Links, die das Testen des Projekts erleichtern.</span><span class="sxs-lookup"><span data-stu-id="e38aa-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="e38aa-137">[In diesem GitHub-Issue](https://github.com/aspnet/Docs/issues/6098) können Sie in Form von Kommentaren Feedback zu Unterschieden zwischen Beispieldownloads und ausführlichen Anleitungen geben.</span><span class="sxs-lookup"><span data-stu-id="e38aa-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="e38aa-138">Testen der heruntergeladenen App</span><span class="sxs-lookup"><span data-stu-id="e38aa-138">Test the download app</span></span>

<span data-ttu-id="e38aa-139">Wenn Sie die vollständige App nicht heruntergeladen haben und das Beispielprojekt selbst erstellen möchten, können Sie mit dem [nächsten Abschnitt](#create-a-razor-class-library) fortfahren.</span><span class="sxs-lookup"><span data-stu-id="e38aa-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e38aa-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e38aa-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e38aa-141">Öffnen Sie die *SLN*-Datei in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e38aa-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="e38aa-142">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="e38aa-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e38aa-143">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e38aa-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e38aa-144">Erstellen Sie über die Eingabeaufforderung im *cli*-Verzeichnis die RCL und die Web-App.</span><span class="sxs-lookup"><span data-stu-id="e38aa-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="e38aa-145">Wechseln Sie ins Verzeichnis *WebApp1*, und führen Sie die App aus:</span><span class="sxs-lookup"><span data-stu-id="e38aa-145">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="e38aa-146">Folgen Sie den Anweisungen in [Testen des WebApp1-Projekts](#test).</span><span class="sxs-lookup"><span data-stu-id="e38aa-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="e38aa-147">Erstellen einer RCL</span><span class="sxs-lookup"><span data-stu-id="e38aa-147">Create a Razor Class Library</span></span>

<span data-ttu-id="e38aa-148">In diesem Abschnitt wird das Erstellen einer RCL beschrieben.</span><span class="sxs-lookup"><span data-stu-id="e38aa-148">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="e38aa-149">Dabei werden der RCL Razor-Dateien hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e38aa-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e38aa-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e38aa-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e38aa-151">Erstellen Sie das RCL-Projekt:</span><span class="sxs-lookup"><span data-stu-id="e38aa-151">Create the RCL project:</span></span>

* <span data-ttu-id="e38aa-152">Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e38aa-153">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="e38aa-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="e38aa-154">Benennen Sie die app **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="e38aa-155">Überprüfen Sie, ob **ASP.NET Core 2.1** oder höher ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="e38aa-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="e38aa-156">Klicken Sie auf **Razor Class Library** (Razor-Klassenbibliothek) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="e38aa-157">Fügen Sie eine Razor-Datei für die Teilansicht mit dem Namen „_Message.cshtml“ unter *RazorUIClassLib/Areas/MyFeature/Pages/Shared* hinzu.</span><span class="sxs-lookup"><span data-stu-id="e38aa-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e38aa-158">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e38aa-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e38aa-159">Führen Sie die folgenden Befehle über die Befehlszeile aus:</span><span class="sxs-lookup"><span data-stu-id="e38aa-159">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="e38aa-160">Die obenstehenden Befehle haben folgende Konsequenzen:</span><span class="sxs-lookup"><span data-stu-id="e38aa-160">The preceding commands:</span></span>

* <span data-ttu-id="e38aa-161">Die `RazorUIClassLib`-RCL wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="e38aa-161">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="e38aa-162">Die Seite „Razor_Message“ wird erstellt und der RCL hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e38aa-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="e38aa-163">Durch den `-np`-Parameter wird die Seite ohne `PageModel` erstellt.</span><span class="sxs-lookup"><span data-stu-id="e38aa-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="e38aa-164">Die Datei [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) wird erstellt und der RCL hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e38aa-164">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="e38aa-165">Die Datei „_ViewStart.cshtml“ ist erforderlich, um das Layout des Razor-Seiten-Projekts (das im nächsten Abschnitt hinzugefügt wird) verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="e38aa-165">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

------

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="e38aa-166">Fügen Sie dem Projekt Razor-Dateien und -Ordner hinzu:</span><span class="sxs-lookup"><span data-stu-id="e38aa-166">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="e38aa-167">Ersetzen Sie das Markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="e38aa-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="e38aa-168">Ersetzen Sie das Markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="e38aa-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="e38aa-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` ist erforderlich, um die Teilansicht `<partial name="_Message" />` verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="e38aa-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="e38aa-170">Wenn Sie nicht die `@addTagHelper`-Anweisung einbinden möchten, können Sie die Datei *_ViewImports.cshtml* hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e38aa-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="e38aa-171">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e38aa-171">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="e38aa-172">Weitere Informationen zu „_ViewImports.cshtml“ finden Sie unter [Importieren gemeinsam verwendeter Anweisungen](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="e38aa-172">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="e38aa-173">Erstellen Sie die Klassenbibliothek, um sicherzustellen, dass keine Compilerfehler vorliegen:</span><span class="sxs-lookup"><span data-stu-id="e38aa-173">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="e38aa-174">Die Buildausgabe enthält *RazorUIClassLib.dll* und *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="e38aa-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="e38aa-175">*RazorUIClassLib.Views.dll* enthält den kompilierten Razor-Inhalt.</span><span class="sxs-lookup"><span data-stu-id="e38aa-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="e38aa-176">Verwenden der Razor-Benutzeroberflächenbibliothek über ein Razor-Seiten-Projekt</span><span class="sxs-lookup"><span data-stu-id="e38aa-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e38aa-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e38aa-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e38aa-178">Erstellen Sie die Web-App mit Razor-Seiten:</span><span class="sxs-lookup"><span data-stu-id="e38aa-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="e38aa-179">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe und anschließend auf **Hinzufügen** >  **Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="e38aa-180">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="e38aa-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="e38aa-181">Legen Sie als Namen für die App **WebApp1** fest.</span><span class="sxs-lookup"><span data-stu-id="e38aa-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="e38aa-182">Überprüfen Sie, ob **ASP.NET Core 2.1** oder höher ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="e38aa-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="e38aa-183">Klicken Sie auf **Webanwendung** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="e38aa-184">Klicken Sie im **Projektmappen-Explorer** zuerst mit der rechten Maustaste auf **WebApp1** und anschließend auf **Als Startprojekt festlegen**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="e38aa-185">Klicken Sie im **Projektmappen-Explorer** zuerst mit der rechten Maustaste auf **WebApp1** und anschließend auf **Buildabhängigkeiten** > **Projektabhängigkeiten**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="e38aa-186">Aktivieren Sie für **WebApp1** die Abhängigkeit **RazorUIClassLib**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="e38aa-187">Klicken Sie im **Projektmappen-Explorer** zuerst mit der rechten Maustaste auf **WebApp1** und anschließend auf **Hinzufügen** > **Verweis**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="e38aa-188">Aktivieren Sie im Dialogfeld **Verweis-Manager** das Kontrollkästchen neben **RazorUIClassLib**, und klicken Sie anschließend auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="e38aa-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="e38aa-189">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="e38aa-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e38aa-190">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e38aa-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e38aa-191">Erstellen Sie eine Web-App mit Razor-Seiten und eine Projektmappendatei, die die Razor-Seiten-App und die RCL enthält:</span><span class="sxs-lookup"><span data-stu-id="e38aa-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

<span data-ttu-id="e38aa-192">Erstellen Sie die Web-App, und führen Sie sie aus:</span><span class="sxs-lookup"><span data-stu-id="e38aa-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="e38aa-193">Testen des WebApp1-Projekts</span><span class="sxs-lookup"><span data-stu-id="e38aa-193">Test WebApp1</span></span>

<span data-ttu-id="e38aa-194">Überprüfen Sie, ob die Razor-Benutzeroberflächenbibliothek verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e38aa-194">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="e38aa-195">Wechseln Sie zu `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="e38aa-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="e38aa-196">Überschreiben von Ansichten, Teilansichten und Seiten</span><span class="sxs-lookup"><span data-stu-id="e38aa-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="e38aa-197">Wenn eine Ansicht, Teilansicht oder Razor-Seite sowohl in der Web-App als auch in der RCL enthalten ist, hat das Razor-Markup (die *CSHTML*-Datei) in der Web-App Vorrang.</span><span class="sxs-lookup"><span data-stu-id="e38aa-197">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="e38aa-198">Wenn Sie z.B. *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* zu WebApp1 hinzufügen, hat Page1 in WebApp1 Vorrang vor Page1 in der RCL.</span><span class="sxs-lookup"><span data-stu-id="e38aa-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="e38aa-199">Ob Page1 in WebApp1 tatsächlich die Priorität zugeordnet wird, können Sie testen, indem Sie im Beispieldownload *WebApp1/Areas/MyFeature2* in *WebApp1/Areas/MyFeature* umbenennen.</span><span class="sxs-lookup"><span data-stu-id="e38aa-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="e38aa-200">Kopieren Sie die Teilansicht *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* nach *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e38aa-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="e38aa-201">Aktualisieren Sie das Markup, um den neuen Speicherort anzugeben.</span><span class="sxs-lookup"><span data-stu-id="e38aa-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="e38aa-202">Erstellen Sie die App, und führen Sie sie aus, um sicherzustellen, dass die Version mit der Teilansicht der App verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e38aa-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="e38aa-203">RCL Seitenlayout</span><span class="sxs-lookup"><span data-stu-id="e38aa-203">RCL Pages layout</span></span>

<span data-ttu-id="e38aa-204">Um RCL Inhalt verweisen, als wäre es Teil der Web-app-Ordner "Pages", erstellen Sie die RCL-Projekt, mit der folgenden Dateistruktur:</span><span class="sxs-lookup"><span data-stu-id="e38aa-204">To reference RCL content as though it is part of the web app's Pages folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="e38aa-205">*RazorUIClassLib/Seiten*</span><span class="sxs-lookup"><span data-stu-id="e38aa-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="e38aa-206">*RazorUIClassLib/Pages/Shared*</span><span class="sxs-lookup"><span data-stu-id="e38aa-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="e38aa-207">Nehmen wir an *RazorUIClassLib/Pages/Shared* enthält zwei partielle Dateien, *_Header.cshtml* und *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e38aa-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files, *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="e38aa-208">Die <partial> können Tags hinzugefügt werden *"_Layout.cshtml"* Datei:</span><span class="sxs-lookup"><span data-stu-id="e38aa-208">The <partial> tags could be added to *_Layout.cshtml* file:</span></span> 
  
```
  <body>
    <partial name="_Header">
    @RenderBody()
    <partial name="_Footer">
  </body>
```