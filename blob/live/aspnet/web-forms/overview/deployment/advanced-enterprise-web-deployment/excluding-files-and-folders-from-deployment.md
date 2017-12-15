---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: "Ausschließen von Dateien und Ordner von der Bereitstellung | Microsoft Docs"
author: jrjlee
description: "In diesem Thema wird beschrieben, wie Sie können Dateien und Ordner ausschließen aus einem Webbereitstellungspaket beim Erstellen und Packen ein Webanwendungsprojekt."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: 80810415bac473a58f60110fb9d08772e0627bd5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="excluding-files-and-folders-from-deployment"></a><span data-ttu-id="bf26d-103">Ausschließen von Dateien und Ordner von der Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="bf26d-103">Excluding Files and Folders from Deployment</span></span>
====================
<span data-ttu-id="bf26d-104">durch [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="bf26d-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="bf26d-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="bf26d-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="bf26d-106">In diesem Thema wird beschrieben, wie Sie können Dateien und Ordner ausschließen aus einem Webbereitstellungspaket beim Erstellen und Packen ein Webanwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="bf26d-106">This topic describes how you can exclude files and folders from a web deployment package when you build and package a web application project.</span></span>


<span data-ttu-id="bf26d-107">Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.</span><span class="sxs-lookup"><span data-stu-id="bf26d-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="bf26d-108">Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf der Teilung Datei Herangehensweise beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird Projekt zwei Dateien & #x 2014; eine enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an.</span><span class="sxs-lookup"><span data-stu-id="bf26d-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="bf26d-109">Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="bf26d-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="overview"></a><span data-ttu-id="bf26d-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="bf26d-110">Overview</span></span>

<span data-ttu-id="bf26d-111">Wenn Sie ein Webprojekt für die Anwendung in Visual Studio 2010 erstellen, Sie können die Publishing Web Pipeline (WPP) dieses Buildprozesses zu erweitern, indem Sie Ihre kompilierte-Anwendung in ein Webpaket zur Bereitstellung geeignete packen.</span><span class="sxs-lookup"><span data-stu-id="bf26d-111">When you build a web application project in Visual Studio 2010, the Web Publishing Pipeline (WPP) lets you extend this build process by packaging your compiled web application into a deployable web package.</span></span> <span data-ttu-id="bf26d-112">Sie können die Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) dieses Webpaket bereitgestellt auf einem remote-IIS-Webserver verwenden, oder importieren das Paket manuell über den IIS-Manager.</span><span class="sxs-lookup"><span data-stu-id="bf26d-112">You can then use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy this web package to a remote IIS web server, or import the web package manually through IIS Manager.</span></span> <span data-ttu-id="bf26d-113">Diese Verpackungsprozesses finden [erstellen und Packen Webanwendungsprojekte](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span><span class="sxs-lookup"><span data-stu-id="bf26d-113">This packaging process is explained in [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span>

<span data-ttu-id="bf26d-114">Wie Sie Einfluss haben, was in Ihrer Website enthalten ruft Paket?</span><span class="sxs-lookup"><span data-stu-id="bf26d-114">So how do you control what gets included in your web package?</span></span> <span data-ttu-id="bf26d-115">Die Einstellungen für Projektdateien in Visual Studio über die zugrunde liegenden Projektdatei bieten ausreichende Kontrolle für eine Vielzahl von Szenarien.</span><span class="sxs-lookup"><span data-stu-id="bf26d-115">The project settings in Visual Studio, through the underlying project file, provide sufficient control for a lot of scenarios.</span></span> <span data-ttu-id="bf26d-116">In einigen Fällen möchten Sie jedoch den Inhalt des Webpakets auf bestimmte zielumgebungen anzupassen.</span><span class="sxs-lookup"><span data-stu-id="bf26d-116">However, in some cases you may want to tailor the contents of your web package to specific destination environments.</span></span> <span data-ttu-id="bf26d-117">Sie möchten z. B. einen Ordner für Protokolldateien einschließen, wenn Sie Ihre Anwendung in einer testumgebung bereitstellen, aber schließen Sie den Ordner aus, wenn Sie die Anwendung in einer Staging- oder Produktionsserver Umgebung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="bf26d-117">For example, you might want to include a folder for log files when you deploy your application to a test environment but exclude the folder when you deploy the application to a staging or production environment.</span></span> <span data-ttu-id="bf26d-118">In diesem Thema erfahren Sie, wie Sie dies tun.</span><span class="sxs-lookup"><span data-stu-id="bf26d-118">This topic will show you how to do this.</span></span>

## <a name="what-gets-included-by-default"></a><span data-ttu-id="bf26d-119">Was ruft standardmäßig enthalten?</span><span class="sxs-lookup"><span data-stu-id="bf26d-119">What Gets Included by Default?</span></span>

<span data-ttu-id="bf26d-120">Wenn Sie die Projekteigenschaften des Web-Anwendung in Visual Studio Konfigurieren der **bereitzustellenden Elemente** auf in der Liste der **Web packen/veröffentlichen** Seite können Sie angeben, was in Ihrer webbereitstellung enthalten sein sollen das Paket.</span><span class="sxs-lookup"><span data-stu-id="bf26d-120">When you configure your web application project properties in Visual Studio, the **Items to deploy** list on the **Package/Publish Web** page lets you specify what you want to include in your web deployment package.</span></span> <span data-ttu-id="bf26d-121">Standardmäßig ist dies festgelegt auf **nur die Ausführung dieser Anwendung erforderliche Dateien**.</span><span class="sxs-lookup"><span data-stu-id="bf26d-121">By default, this is set to **Only files needed to run this application**.</span></span>

![](excluding-files-and-folders-from-deployment/_static/image1.png)

<span data-ttu-id="bf26d-122">Bei Auswahl **nur die Ausführung dieser Anwendung erforderliche Dateien**, WPP versucht zu bestimmen, welche Dateien für das Webpaket hinzugefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="bf26d-122">When you choose **Only files needed to run this application**, the WPP will try to determine which files should be added to the web package.</span></span> <span data-ttu-id="bf26d-123">Dies umfasst Folgendes:</span><span class="sxs-lookup"><span data-stu-id="bf26d-123">This includes:</span></span>

- <span data-ttu-id="bf26d-124">Alle Builds Ausgaben für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="bf26d-124">All the build outputs for the project.</span></span>
- <span data-ttu-id="bf26d-125">Alle Dateien mit dem Buildvorgang markiert **Content**.</span><span class="sxs-lookup"><span data-stu-id="bf26d-125">Any files marked with a build action of **Content**.</span></span>

> [!NOTE]
> <span data-ttu-id="bf26d-126">In dieser Datei ist die Logik, die bestimmt, welche Dateien sind enthalten:</span><span class="sxs-lookup"><span data-stu-id="bf26d-126">The logic that determines which files to include is contained in this file:</span></span>   
> <span data-ttu-id="bf26d-127">*%ProgramFiles%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*</span><span class="sxs-lookup"><span data-stu-id="bf26d-127">*%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*</span></span>


## <a name="excluding-specific-files-and-folders"></a><span data-ttu-id="bf26d-128">Ausschließen bestimmter Dateien und Ordner</span><span class="sxs-lookup"><span data-stu-id="bf26d-128">Excluding Specific Files and Folders</span></span>

<span data-ttu-id="bf26d-129">In einigen Fällen sollten eine präzisere Kontrolle Sie über die Dateien und Ordner bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="bf26d-129">In some cases, you'll want more fine-grained control over which files and folders are deployed.</span></span> <span data-ttu-id="bf26d-130">Wenn Sie wissen, welche mysqlsink auszuschließenden Dateien Zeit, und der Ausschluss bezieht sich auf alle zielumgebungen, können Sie einfach die **Buildvorgang** jeder Datei **keine**.</span><span class="sxs-lookup"><span data-stu-id="bf26d-130">If you know which files you want to exclude ahead of time, and the exclusion applies to all destination environments, you can simply set the **Build Action** of each file to **None**.</span></span>

<span data-ttu-id="bf26d-131">**Ausschließen bestimmte Dateien von der Bereitstellung**</span><span class="sxs-lookup"><span data-stu-id="bf26d-131">**To exclude specific files from deployment**</span></span>

1. <span data-ttu-id="bf26d-132">In der **Projektmappen-Explorer** rechten Maustaste auf die Datei, und klicken Sie dann auf **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="bf26d-132">In the **Solution Explorer** window, right-click the file, and then click **Properties**.</span></span>
2. <span data-ttu-id="bf26d-133">In der **Eigenschaften** Fenster, in der **Buildvorgang** Zeile **keine**.</span><span class="sxs-lookup"><span data-stu-id="bf26d-133">In the **Properties** window, in the **Build Action** row, select **None**.</span></span>

<span data-ttu-id="bf26d-134">Dieser Ansatz ist jedoch nicht immer praktisch.</span><span class="sxs-lookup"><span data-stu-id="bf26d-134">However, this approach is not always convenient.</span></span> <span data-ttu-id="bf26d-135">Beispielsweise empfiehlt es sich um die Dateien zu verändern und Ordner werden gemäß Ihrer zielumgebung und außerhalb von Visual Studio enthalten.</span><span class="sxs-lookup"><span data-stu-id="bf26d-135">For example, you may want to vary which files and folders are included according to your destination environment, and from outside Visual Studio.</span></span> <span data-ttu-id="bf26d-136">Angenommen, in der Beispielprojektmappe Contact Manager sehen Sie sich den Inhalt des Projekts ContactManager.Mvc:</span><span class="sxs-lookup"><span data-stu-id="bf26d-136">For example, in the Contact Manager sample solution, take a look at the contents of the ContactManager.Mvc project:</span></span>

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- <span data-ttu-id="bf26d-137">Der interne Ordner enthält einige SQL­Skripts, mit der Entwickler erstellen, löschen und lokale Datenbanken zu Entwicklungszwecken aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="bf26d-137">The Internal folder contains some SQL scripts that the developer uses to create, drop, and populate local databases for development purposes.</span></span> <span data-ttu-id="bf26d-138">"Nothing" in diesem Ordner sollte in einer Staging- oder Produktionsserver Umgebung bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="bf26d-138">Nothing in this folder should be deployed to a staging or production environment.</span></span>
- <span data-ttu-id="bf26d-139">Der Ordner "Skripts" enthält mehrere JavaScript-Dateien.</span><span class="sxs-lookup"><span data-stu-id="bf26d-139">The Scripts folder contains several JavaScript files.</span></span> <span data-ttu-id="bf26d-140">Viele dieser Dateien sind enthalten ausschließlich zur Unterstützung einer Debuggen oder IntelliSense in Visual Studio zur Verfügung stellen.</span><span class="sxs-lookup"><span data-stu-id="bf26d-140">A lot of these files are included purely to support debugging or provide IntelliSense in Visual Studio.</span></span> <span data-ttu-id="bf26d-141">Einige dieser Dateien sollten nicht auf Staging-oder produktionsumgebung bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="bf26d-141">Some of these files should not be deployed to staging or production environments.</span></span> <span data-ttu-id="bf26d-142">Allerdings empfiehlt es sich um die Bereitstellung in einer testumgebung Developer, um die Problembehandlung zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="bf26d-142">However, you may want to deploy them to a developer test environment to facilitate troubleshooting.</span></span>

<span data-ttu-id="bf26d-143">Obwohl Sie die Projektdateien zum Ausschließen bestimmter Dateien und Ordner bearbeiten können, gibt es einen einfacheren Weg.</span><span class="sxs-lookup"><span data-stu-id="bf26d-143">Although you could manipulate your project files to exclude specific files and folders, there is an easier way.</span></span> <span data-ttu-id="bf26d-144">Die WPP umfasst einen Mechanismus, um Dateien und Ordner ausschließen, indem Sie mit dem Namen Elementlisten erstellen **ExcludeFromPackageFolders** und **ExcludeFromPackageFiles**.</span><span class="sxs-lookup"><span data-stu-id="bf26d-144">The WPP includes a mechanism to exclude files and folders by building item lists named **ExcludeFromPackageFolders** and **ExcludeFromPackageFiles**.</span></span> <span data-ttu-id="bf26d-145">Sie können diesen Mechanismus erweitern, indem Sie diese Listen eigene Elemente hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="bf26d-145">You can extend this mechanism by adding your own items to these lists.</span></span> <span data-ttu-id="bf26d-146">Zu diesem Zweck müssen Sie die folgenden allgemeinen Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="bf26d-146">To do this, you need to complete these high-level steps:</span></span>

1. <span data-ttu-id="bf26d-147">Erstellen eine benutzerdefinierten Projektdatei mit dem Namen *[Projektname].wpp.targets* im gleichen Ordner wie die Projektdatei.</span><span class="sxs-lookup"><span data-stu-id="bf26d-147">Create a custom project file named *[project name].wpp.targets* in the same folder as your project file.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bf26d-148">Die *. wpp.targets* Datei muss im gleichen Ordner wie die Projektdatei der Web-Anwendung & #x 2014; wechseln z. B. *ContactManager.Mvc.csproj*& #x 2014; statt im selben Ordner wie ein anderer Benutzerdefinierte Projektdateien verwenden Sie, um die Steuerung des Builds und Bereitstellungsprozess.</span><span class="sxs-lookup"><span data-stu-id="bf26d-148">The *.wpp.targets* file needs to go in the same folder as your web application project file&#x2014;for example, *ContactManager.Mvc.csproj*&#x2014;rather than in the same folder as any custom project files you use to control the build and deployment process.</span></span>
2. <span data-ttu-id="bf26d-149">In der *. wpp.targets* Hinzufügen einer **ItemGroup** Element.</span><span class="sxs-lookup"><span data-stu-id="bf26d-149">In the *.wpp.targets* file, add an **ItemGroup** element.</span></span>
3. <span data-ttu-id="bf26d-150">In der **ItemGroup** Element hinzufügen **ExcludeFromPackageFolders** und **ExcludeFromPackageFiles** Elemente, die bestimmte Dateien und Ordner nach Bedarf ausschließen.</span><span class="sxs-lookup"><span data-stu-id="bf26d-150">In the **ItemGroup** element, add **ExcludeFromPackageFolders** and **ExcludeFromPackageFiles** items to exclude specific files and folders as required.</span></span>

<span data-ttu-id="bf26d-151">Dies ist die grundlegende Struktur dieses *. wpp.targets* Datei:</span><span class="sxs-lookup"><span data-stu-id="bf26d-151">This is the basic structure of this *.wpp.targets* file:</span></span>


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


<span data-ttu-id="bf26d-152">Beachten Sie, dass jedes Element ein Element-Metadaten-Element mit dem Namen schließt **FromTarget**.</span><span class="sxs-lookup"><span data-stu-id="bf26d-152">Note that each item includes an item metadata element named **FromTarget**.</span></span> <span data-ttu-id="bf26d-153">Dies ist ein optionaler Wert, der während des Erstellungsprozesses nicht beeinträchtigt wird. es einfach, um anzugeben, warum bestimmte Dateien oder Ordner weggelassen wurden dient, wenn jemand die Buildprotokolle überprüft.</span><span class="sxs-lookup"><span data-stu-id="bf26d-153">This is an optional value that doesn't affect the build process; it simply serves to indicate why particular files or folders were omitted if someone reviews the build logs.</span></span>

## <a name="excluding-files-and-folders-from-a-web-package"></a><span data-ttu-id="bf26d-154">Ausschließen von Dateien und Ordner von einem Webpaket</span><span class="sxs-lookup"><span data-stu-id="bf26d-154">Excluding Files and Folders from a Web Package</span></span>

<span data-ttu-id="bf26d-155">Das nächste Verfahren veranschaulicht das Hinzufügen einer *. wpp.targets* Datei ein Webanwendungsprojekt und wie die Datei zum Ausschließen bestimmter Dateien und Ordner aus dem Webpaket beim Erstellen des Projekts verwenden.</span><span class="sxs-lookup"><span data-stu-id="bf26d-155">The next procedure shows you how to add a *.wpp.targets* file to a web application project and how to use the file to exclude specific files and folders from the web package when you build your project.</span></span>

<span data-ttu-id="bf26d-156">**Zum Ausschließen von Dateien und Ordnern über ein Webbereitstellungspaket**</span><span class="sxs-lookup"><span data-stu-id="bf26d-156">**To exclude files and folders from a web deployment package**</span></span>

1. <span data-ttu-id="bf26d-157">Öffnen Sie die Projektmappe in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="bf26d-157">Open your solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="bf26d-158">In der **Projektmappen-Explorer** Fenster mit der rechten Maustaste des Projektknoten der Web-Anwendung (z. B. **ContactManager.Mvc**), zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="bf26d-158">In the **Solution Explorer** window, right-click your web application project node (for example, **ContactManager.Mvc**), point to **Add**, and then click **New Item**.</span></span>
3. <span data-ttu-id="bf26d-159">In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **XML-Datei** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="bf26d-159">In the **Add New Item** dialog box, select the **XML File** template.</span></span>
4. <span data-ttu-id="bf26d-160">In der **Namen** geben *[Projektname]***. wpp.targets** (z. B. **ContactManager.Mvc.wpp.targets**), und klicken Sie dann auf  **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="bf26d-160">In the **Name** box, type *[project name]***.wpp.targets** (for example, **ContactManager.Mvc.wpp.targets**), and then click **Add**.</span></span>

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > <span data-ttu-id="bf26d-161">Wenn Sie den Stammknoten des Projekts ein neues Element hinzufügen, wird die Datei im gleichen Ordner wie die Projektdatei erstellt.</span><span class="sxs-lookup"><span data-stu-id="bf26d-161">If you add a new item to the root node of a project, the file is created in the same folder as the project file.</span></span> <span data-ttu-id="bf26d-162">Sie können dies überprüfen, indem Sie den Ordner in Windows Explorer öffnen.</span><span class="sxs-lookup"><span data-stu-id="bf26d-162">You can verify this by opening the folder in Windows Explorer.</span></span>
5. <span data-ttu-id="bf26d-163">Fügen Sie in der Datei eine **Projekt** Element und ein **ItemGroup** Element:</span><span class="sxs-lookup"><span data-stu-id="bf26d-163">In the file, add a **Project** element and an **ItemGroup** element:</span></span>

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. <span data-ttu-id="bf26d-164">Wenn Sie aus dem Webpaket ausschließen möchten, fügen eine **ExcludeFromPackageFolders** Element der **ItemGroup** Element:</span><span class="sxs-lookup"><span data-stu-id="bf26d-164">If you want to exclude folders from the web package, add an **ExcludeFromPackageFolders** element to the **ItemGroup** element:</span></span>

    1. <span data-ttu-id="bf26d-165">In der **Include** -Attribut angegeben wird, geben Sie eine durch Semikolons getrennte Liste der Ordner, die Sie ausschließen möchten.</span><span class="sxs-lookup"><span data-stu-id="bf26d-165">In the **Include** attribute, provide a semicolon-separated list of the folders you want to exclude.</span></span>
    2. <span data-ttu-id="bf26d-166">In der **FromTarget** Metadatenelement, geben Sie einen aussagekräftigen Wert um anzugeben, warum die Ordner, wie den Namen der ausgeschlossen werden die *. wpp.targets* Datei.</span><span class="sxs-lookup"><span data-stu-id="bf26d-166">In the **FromTarget** metadata element, provide a meaningful value to indicate why the folders are being excluded, like the name of the *.wpp.targets* file.</span></span>

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. <span data-ttu-id="bf26d-167">Wenn Sie Dateien aus dem Webpaket ausschließen möchten, fügen Sie ein **ExcludeFromPackageFiles** Element an der **ItemGroup** Element:</span><span class="sxs-lookup"><span data-stu-id="bf26d-167">If you want to exclude files from the web package, add an **ExcludeFromPackageFiles** element to the **ItemGroup** element:</span></span>

    1. <span data-ttu-id="bf26d-168">In der **Include** -Attribut angegeben wird, geben Sie eine durch Semikolons getrennte Liste der Dateien, die Sie ausschließen möchten.</span><span class="sxs-lookup"><span data-stu-id="bf26d-168">In the **Include** attribute, provide a semicolon-separated list of the files you want to exclude.</span></span>
    2. <span data-ttu-id="bf26d-169">In der **FromTarget** Metadatenelement, geben Sie einen aussagekräftigen Wert um anzugeben, warum die Dateien, wie den Namen der ausgeschlossen werden die *. wpp.targets* Datei.</span><span class="sxs-lookup"><span data-stu-id="bf26d-169">In the **FromTarget** metadata element, provide a meaningful value to indicate why the files are being excluded, like the name of the *.wpp.targets* file.</span></span>

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. <span data-ttu-id="bf26d-170">Die *[Projektname].wpp.targets* Datei sollte jetzt diesem ähneln:</span><span class="sxs-lookup"><span data-stu-id="bf26d-170">The *[project name].wpp.targets* file should now resemble this:</span></span>

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. <span data-ttu-id="bf26d-171">Speichern und schließen Sie die *[Projektname].wpp.targets* Datei.</span><span class="sxs-lookup"><span data-stu-id="bf26d-171">Save and close the *[project name].wpp.targets* file.</span></span>

<span data-ttu-id="bf26d-172">Das nächste Mal Sie Builds und des Pakets, das Webanwendungsprojekt, WPP erkennt automatisch die *. wpp.targets* Datei.</span><span class="sxs-lookup"><span data-stu-id="bf26d-172">The next time you build and package your web application project, the WPP will automatically detect the *.wpp.targets* file.</span></span> <span data-ttu-id="bf26d-173">Alle Dateien und Ordner, die Sie angegeben werden nicht in der Web-Paket enthalten.</span><span class="sxs-lookup"><span data-stu-id="bf26d-173">Any files and folders you specified will not be included in the web package.</span></span>

## <a name="conclusion"></a><span data-ttu-id="bf26d-174">Schlussfolgerung</span><span class="sxs-lookup"><span data-stu-id="bf26d-174">Conclusion</span></span>

<span data-ttu-id="bf26d-175">Dieses Thema beschreibt, wie Sie bestimmte Dateien und Ordner ausschließen, wenn Sie eine Web-Paket erstellen, durch Erstellen einer benutzerdefinierten *. wpp.targets* Datei im gleichen Ordner wie die Projektdatei der Web-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="bf26d-175">This topic described how to exclude specific files and folders when you build a web package, by creating a custom *.wpp.targets* file in the same folder as your web application project file.</span></span>

## <a name="further-reading"></a><span data-ttu-id="bf26d-176">Weiterführende Themen</span><span class="sxs-lookup"><span data-stu-id="bf26d-176">Further Reading</span></span>

<span data-ttu-id="bf26d-177">Weitere Informationen zum Verwenden von benutzerdefinierter Projektdateien von Microsoft Build Engine (MSBuild) um den Bereitstellungsprozess zu steuern, finden Sie unter [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="bf26d-177">For more information on using custom Microsoft Build Engine (MSBuild) project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="bf26d-178">Weitere Informationen über das Packen und Bereitstellungsprozess, finden Sie unter [erstellen und Packen Webanwendungsprojekte](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Konfigurieren von Parametern für die Bereitstellung von Paketen](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), und [ Bereitstellen von Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md).</span><span class="sxs-lookup"><span data-stu-id="bf26d-178">For more information on the packaging and deployment process, see [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Configuring Parameters for Web Package Deployment](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), and [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="bf26d-179">[Zurück](deploying-membership-databases-to-enterprise-environments.md)
[Weiter](taking-web-applications-offline-with-web-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="bf26d-179">[Previous](deploying-membership-databases-to-enterprise-environments.md)
[Next](taking-web-applications-offline-with-web-deploy.md)</span></span>