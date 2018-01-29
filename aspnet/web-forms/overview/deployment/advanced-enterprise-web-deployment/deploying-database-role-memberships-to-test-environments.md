---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: "Bereitstellen von Datenbank-Rollenmitgliedschaften für Testumgebungen | Microsoft Docs"
author: jrjlee
description: "Dieses Thema beschreibt, wie Benutzerkonten Datenbankrollen als Teil einer Bereitstellung von Projektmappen in einer testumgebung hinzufügen. Beim Bereitstellen einer Lösung mit..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 226c28622f76e866fba1fc33cf9b9b7a01e5295b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="deploying-database-role-memberships-to-test-environments"></a><span data-ttu-id="4511d-104">Bereitstellen von Datenbank-Rollenmitgliedschaften für Testumgebungen</span><span class="sxs-lookup"><span data-stu-id="4511d-104">Deploying Database Role Memberships to Test Environments</span></span>
====================
<span data-ttu-id="4511d-105">durch [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="4511d-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="4511d-106">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="4511d-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="4511d-107">Dieses Thema beschreibt, wie Benutzerkonten Datenbankrollen als Teil einer Bereitstellung von Projektmappen in einer testumgebung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4511d-107">This topic describes how to add user accounts to database roles as part of a solution deployment to a test environment.</span></span>
> 
> <span data-ttu-id="4511d-108">Beim Bereitstellen einer Projektmappe ein Datenbankprojekt in einer Staging- oder Produktionsserver-Umgebung mit sollen nicht in der Regel den Entwickler, die das Hinzufügen von Benutzerkonten für Datenbankrollen automatisieren.</span><span class="sxs-lookup"><span data-stu-id="4511d-108">When you deploy a solution containing a database project to a staging or production environment, you typically don't want the developer to automate the addition of user accounts to database roles.</span></span> <span data-ttu-id="4511d-109">In den meisten Fällen der Entwickler weiß nicht, welche Benutzerkonten müssen die Datenbankrollen hinzugefügt werden, und diese Anforderungen können jederzeit ändern.</span><span class="sxs-lookup"><span data-stu-id="4511d-109">In most cases, the developer won't know which user accounts need to be added to which database roles, and these requirements could change at any time.</span></span> <span data-ttu-id="4511d-110">Beim Bereitstellen einer Projektmappe mit einem Datenbankprojekt eine Entwicklungs- oder testumgebung, ist die Situation in der Regel sehr unterschiedliche:</span><span class="sxs-lookup"><span data-stu-id="4511d-110">However, when you deploy a solution containing a database project to a development or test environment, the situation is usually rather different:</span></span>
> 
> - <span data-ttu-id="4511d-111">Die erneut bereitstellen in der Regel der Lösung in regelmäßigen Abständen, häufig mehrmals am Tag.</span><span class="sxs-lookup"><span data-stu-id="4511d-111">The developer typically re-deploys the solution on a regular basis, often several times a day.</span></span>
> - <span data-ttu-id="4511d-112">Die Datenbank wird in der Regel bei jeder Bereitstellung neu erstellt das bedeutet, dass der Datenbankbenutzer erstellt und nach jeder Bereitstellung Rollen hinzugefügt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="4511d-112">The database is typically re-created on every deployment, which means that database users must be created and added to roles after every deployment.</span></span>
> - <span data-ttu-id="4511d-113">Normalerweise hat der Entwickler die vollständige Kontrolle über die zielumgebung für Entwicklungs- oder Testserver.</span><span class="sxs-lookup"><span data-stu-id="4511d-113">The developer typically has full control over the target development or test environment.</span></span>
> 
> <span data-ttu-id="4511d-114">In diesem Szenario ist es häufig von Vorteil automatisch Datenbankbenutzer erstellen und Zuweisen von Mitgliedschaften in der Datenbankrolle im Rahmen des Bereitstellungsprozesses.</span><span class="sxs-lookup"><span data-stu-id="4511d-114">In this scenario, it's often beneficial to automatically create database users and assign database role memberships as part of the deployment process.</span></span>
> 
> <span data-ttu-id="4511d-115">Der wichtigste Faktor ist, dass dieser Vorgang angehören soll bedingten basierend auf der zielumgebung.</span><span class="sxs-lookup"><span data-stu-id="4511d-115">The key factor is that this operation needs to be conditional based on the target environment.</span></span> <span data-ttu-id="4511d-116">Wenn Sie auf einem Staging- oder in einer produktiven Umgebung bereitstellen, möchten Sie überspringen Sie den Vorgang.</span><span class="sxs-lookup"><span data-stu-id="4511d-116">If you're deploying to a staging or a production environment, you want to skip the operation.</span></span> <span data-ttu-id="4511d-117">Wenn Sie dem Entwickler bereitstellen oder Umgebung testen, sollten Sie Rollenmitgliedschaften ohne weiteren Eingriff bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="4511d-117">If you're deploying to a developer or test environment, you want to deploy role memberships without further intervention.</span></span> <span data-ttu-id="4511d-118">In diesem Thema wird beschrieben, ein Ansatz besteht darin, die Sie verwenden können, um dieser Herausforderung zu begegnen.</span><span class="sxs-lookup"><span data-stu-id="4511d-118">This topic describes one approach you can use to address this challenge.</span></span>


<span data-ttu-id="4511d-119">Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.</span><span class="sxs-lookup"><span data-stu-id="4511d-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="4511d-120">Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf der Teilung Datei Herangehensweise beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird Projekt zwei Dateien & #x 2014; eine enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an.</span><span class="sxs-lookup"><span data-stu-id="4511d-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="4511d-121">Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="4511d-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="4511d-122">Übersicht über den Task</span><span class="sxs-lookup"><span data-stu-id="4511d-122">Task Overview</span></span>

<span data-ttu-id="4511d-123">In diesem Thema wird Folgendes vorausgesetzt:</span><span class="sxs-lookup"><span data-stu-id="4511d-123">This topic assumes that:</span></span>

- <span data-ttu-id="4511d-124">Verwenden Sie die Teilung Projekt Datei Vorgehensweise bei der Bereitstellung von Projektmappen, wie in beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="4511d-124">You use the split project file approach to solution deployment, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span>
- <span data-ttu-id="4511d-125">Sie rufen VSDBCMD aus der Projektdatei, um das Datenbankprojekt bereitstellen, wie in beschrieben [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="4511d-125">You call VSDBCMD from the project file to deploy your database project, as described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

<span data-ttu-id="4511d-126">Zum Erstellen von Datenbankbenutzern und Rollenmitgliedschaften zuweisen, wenn Sie ein Datenbankprojekt in einer testumgebung bereitstellen, müssen Sie Folgendes ausführen:</span><span class="sxs-lookup"><span data-stu-id="4511d-126">To create database users and assign role memberships when you deploy a database project to a test environment, you'll need to:</span></span>

- <span data-ttu-id="4511d-127">Erstellen Sie ein Transact Structured Query Language (Transact-SQL)-Skript, das die notwendige Änderungen vornimmt.</span><span class="sxs-lookup"><span data-stu-id="4511d-127">Create a Transact Structured Query Language (Transact-SQL) script that makes the necessary database changes.</span></span>
- <span data-ttu-id="4511d-128">Erstellen Sie ein Microsoft Build Engine (MSBuild)-Ziel, das das Hilfsprogramm sqlcmd.exe verwendet, um das SQL-Skript auszuführen.</span><span class="sxs-lookup"><span data-stu-id="4511d-128">Create a Microsoft Build Engine (MSBuild) target that uses the sqlcmd.exe utility to run the SQL script.</span></span>
- <span data-ttu-id="4511d-129">Konfigurieren Sie die Projektdateien zum Ziel aufrufen, wenn Sie die Projektmappe in einer testumgebung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="4511d-129">Configure your project files to invoke the target when you're deploying your solution to a test environment.</span></span>

<span data-ttu-id="4511d-130">In diesem Thema erfahren Sie, wie Sie jede der folgenden Verfahren ausführen.</span><span class="sxs-lookup"><span data-stu-id="4511d-130">This topic will show you how to perform each of these procedures.</span></span>

## <a name="scripting-the-database-role-memberships"></a><span data-ttu-id="4511d-131">Die Mitgliedschaften in Datenbankrollen Scripting</span><span class="sxs-lookup"><span data-stu-id="4511d-131">Scripting the Database Role Memberships</span></span>

<span data-ttu-id="4511d-132">Können Sie ein Transact-SQL-Skript in viele verschiedene Arten erstellen, und wählen Sie in einem beliebigen Speicherort.</span><span class="sxs-lookup"><span data-stu-id="4511d-132">You can create a Transact-SQL script in a lot of different ways, and in any location you choose.</span></span> <span data-ttu-id="4511d-133">Der einfachste Ansatz ist die Erstellung des Skripts innerhalb der Projektmappe in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="4511d-133">The easiest approach is to create the script within your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="4511d-134">**So erstellen ein SQL-Skript**</span><span class="sxs-lookup"><span data-stu-id="4511d-134">**To create a SQL script**</span></span>

1. <span data-ttu-id="4511d-135">In der **Projektmappen-Explorer** Fenster, erweitern Sie Ihr Datenbankprojekt-Knoten.</span><span class="sxs-lookup"><span data-stu-id="4511d-135">In the **Solution Explorer** window, expand your database project node.</span></span>
2. <span data-ttu-id="4511d-136">Mit der rechten Maustaste die **Skripts** Ordner, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="4511d-136">Right-click the **Scripts** folder, point to **Add**, and then click **New Folder**.</span></span>
3. <span data-ttu-id="4511d-137">Typ **Test** als Name des Ordners aus, und drücken Sie dann die EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="4511d-137">Type **Test** as the folder name, and then press Enter.</span></span>
4. <span data-ttu-id="4511d-138">Mit der rechten Maustaste die **Test** Ordner, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **Skript**.</span><span class="sxs-lookup"><span data-stu-id="4511d-138">Right-click the **Test** folder, point to **Add**, and then click **Script**.</span></span>
5. <span data-ttu-id="4511d-139">In der **neues Element hinzufügen** Dialogfeld Feld, geben Sie dem Skript einen aussagekräftigen Namen (z. B. **AddRoleMemberships.sql**), und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="4511d-139">In the **Add New Item** dialog box, give your script a meaningful name (for example, **AddRoleMemberships.sql**), and then click **Add**.</span></span>

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. <span data-ttu-id="4511d-140">In der *AddRoleMemberships.sql* Datei, Hinzufügen von Transact-SQL-Anweisungen, die:</span><span class="sxs-lookup"><span data-stu-id="4511d-140">In the *AddRoleMemberships.sql* file, add Transact-SQL statements that:</span></span>

    1. <span data-ttu-id="4511d-141">Erstellen Sie einen Datenbankbenutzer für die SQL Server-Anmeldung, die auf Ihre Datenbank zugreifen.</span><span class="sxs-lookup"><span data-stu-id="4511d-141">Create a database user for the SQL Server login that will access your database.</span></span>
    2. <span data-ttu-id="4511d-142">Den Datenbankbenutzer und alle erforderlichen Datenbankrollen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="4511d-142">Add the database user to any required database roles.</span></span>
7. <span data-ttu-id="4511d-143">Die Datei sollte diesem ähneln:</span><span class="sxs-lookup"><span data-stu-id="4511d-143">The file should resemble this:</span></span>

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. <span data-ttu-id="4511d-144">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="4511d-144">Save the file.</span></span>

## <a name="executing-the-script-on-the-target-database"></a><span data-ttu-id="4511d-145">Ausführen des Skripts in der Zieldatenbank</span><span class="sxs-lookup"><span data-stu-id="4511d-145">Executing the Script on the Target Database</span></span>

<span data-ttu-id="4511d-146">Im Idealfall würden Sie alle erforderlichen Transact-SQL-Skripts als Teil eines Skripts nach der Bereitstellung ausführen, wenn Sie das Datenbankprojekt bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="4511d-146">Ideally, you'd run any required Transact-SQL scripts as part of a post-deployment script when you deploy your database project.</span></span> <span data-ttu-id="4511d-147">Allerdings zulassen nicht Skripts nach der Bereitstellung Logik bedingt auf Grundlage Projektmappenkonfigurationen oder Buildeigenschaften Ausführung.</span><span class="sxs-lookup"><span data-stu-id="4511d-147">However, post-deployment scripts don't allow you to execute logic conditionally based on solution configurations or build properties.</span></span> <span data-ttu-id="4511d-148">Die Alternative besteht darin, durch das Erstellen der SQL-Skripts direkt aus der MSBuild-Projektdatei Ausführen einer **Ziel** Element, das einen sqlcmd.exe-Befehl ausführt.</span><span class="sxs-lookup"><span data-stu-id="4511d-148">The alternative is to run your SQL scripts directly from the MSBuild project file, by creating a **Target** element that executes a sqlcmd.exe command.</span></span> <span data-ttu-id="4511d-149">Mit diesem Befehl können Sie das Skript in der Zieldatenbank ausführen:</span><span class="sxs-lookup"><span data-stu-id="4511d-149">You can use this command to run your script on the target database:</span></span>


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> <span data-ttu-id="4511d-150">Weitere Informationen zu Befehlszeilenoptionen von "Sqlcmd", finden Sie unter [Hilfsprogramms "Sqlcmd"](https://msdn.microsoft.com/library/ms162773.aspx).</span><span class="sxs-lookup"><span data-stu-id="4511d-150">For more information on sqlcmd command-line options, see [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).</span></span>


<span data-ttu-id="4511d-151">Bevor Sie diesen Befehl in einer MSBuild-Ziel einbetten, müssen Sie berücksichtigen, unter welchen Bedingungen das Skript ausgeführt werden soll:</span><span class="sxs-lookup"><span data-stu-id="4511d-151">Before you embed this command in an MSBuild target, you need to consider under what conditions you want the script to run:</span></span>

- <span data-ttu-id="4511d-152">Die Zieldatenbank muss vorhanden sein, bevor Sie die Rollenmitgliedschaften ändern.</span><span class="sxs-lookup"><span data-stu-id="4511d-152">The target database must exist before you change its role memberships.</span></span> <span data-ttu-id="4511d-153">Daher müssen Sie dieses Skript ausführen *nach* der datenbankbereitstellung.</span><span class="sxs-lookup"><span data-stu-id="4511d-153">As such, you need to run this script *after* the database deployment.</span></span>
- <span data-ttu-id="4511d-154">Sie müssen eine Bedingung enthalten, sodass das Skript nur für testumgebungen ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="4511d-154">You need to include a condition so that the script is only executed for test environments.</span></span>
- <span data-ttu-id="4511d-155">Wenn Sie eine "Was-wäre-wenn" Bereitstellung & #x 2014; ausführen, mit anderen Worten, darf nicht Wenn Sie Bereitstellungsskripts generieren aber nicht tatsächlich ausgeführt wird, diese & #x 2014; Sie das SQL-Skript ausführen.</span><span class="sxs-lookup"><span data-stu-id="4511d-155">If you're running a "what if" deployment&#x2014;in other words, if you're generating deployment scripts but not actually running them&#x2014;you shouldn't run the SQL script.</span></span>

<span data-ttu-id="4511d-156">Bei Verwendung in beschriebenen Ansatz der Teilung Projekt Datei [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), wie der Kontakt-Manager-Beispielprojektmappe zeigt, können Sie in den erstellungsanweisungen aufteilen, für das SQL-Skript wie folgt:</span><span class="sxs-lookup"><span data-stu-id="4511d-156">If you're using the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), as demonstrated by the Contact Manager sample solution, you can split the build instructions for your SQL script like this:</span></span>

- <span data-ttu-id="4511d-157">Alle erforderlichen umgebungsspezifische Eigenschaften zusammen mit der Eigenschaft, die bestimmt, ob Berechtigungen bereitstellen sollten in der Projektdatei umgebungsspezifische wechseln (z. B. *Env Dev.proj*).</span><span class="sxs-lookup"><span data-stu-id="4511d-157">Any required environment-specific properties, together with the property that determines whether to deploy permissions, should go in the environment-specific project file (for example, *Env-Dev.proj*).</span></span>
- <span data-ttu-id="4511d-158">Das MSBuild-Ziel selbst sowie alle Eigenschaften, die nicht zwischen zielumgebungen, ändern sollte in der universelle Projektdatei wechseln (z. B. *Publish.proj*).</span><span class="sxs-lookup"><span data-stu-id="4511d-158">The MSBuild target itself, together with any properties that will not change between destination environments, should go in the universal project file (for example, *Publish.proj*).</span></span>

<span data-ttu-id="4511d-159">In der Projektdatei umgebungsspezifische müssen Sie definieren den Namen des Datenbankservers, den Namen der Zieldatenbank und eine boolesche Eigenschaft, die dem Benutzer, die angeben, ob Sie Rollenmitgliedschaften bereitstellen kann.</span><span class="sxs-lookup"><span data-stu-id="4511d-159">In the environment-specific project file, you need to define the database server name, the target database name, and a Boolean property that lets the user specify whether to deploy role memberships.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


<span data-ttu-id="4511d-160">In der Datei universelles Projekt müssen Sie geben den Speicherort der des ausführbaren "Sqlcmd" und den Speicherort des SQL-Skripts, die Sie ausführen möchten.</span><span class="sxs-lookup"><span data-stu-id="4511d-160">In the universal project file, you need to provide the location of the sqlcmd executable and the location of the SQL script you want to run.</span></span> <span data-ttu-id="4511d-161">Diese Eigenschaften bleiben unabhängig von der zielumgebung verbunden.</span><span class="sxs-lookup"><span data-stu-id="4511d-161">These properties will remain the same regardless of the destination environment.</span></span> <span data-ttu-id="4511d-162">Sie müssen außerdem eine MSBuild-Ziel zum Ausführen von Sqlcmd-Befehl zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4511d-162">You also need to create an MSBuild target to execute the sqlcmd command.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


<span data-ttu-id="4511d-163">Beachten Sie, dass Sie den Speicherort der ausführbaren SQLCMD als eine statische Eigenschaft hinzufügen, wie dies nützlich, um andere Ziele werden konnte.</span><span class="sxs-lookup"><span data-stu-id="4511d-163">Notice that you add the location of the sqlcmd executable as a static property, as this could be useful to other targets.</span></span> <span data-ttu-id="4511d-164">Im Gegensatz dazu definieren Sie den Speicherort der SQL-Skript und die Syntax der Sqlcmd-Befehl als dynamische Eigenschaften in das Ziel, da sie nicht erforderlich sind, bevor das Ziel ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="4511d-164">In contrast, you define the location of your SQL script and the syntax of the sqlcmd command as dynamic properties within the target, as they will not be required before the target is executed.</span></span> <span data-ttu-id="4511d-165">In diesem Fall die **DeployTestDBPermissions** Ziel wird nur ausgeführt werden, wenn diese Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="4511d-165">In this case, the **DeployTestDBPermissions** target will only be executed if these conditions are met:</span></span>

- <span data-ttu-id="4511d-166">Die **DeployTestDBRoleMemberships** -Eigenschaftensatz auf **"true"**.</span><span class="sxs-lookup"><span data-stu-id="4511d-166">The **DeployTestDBRoleMemberships** property is set to **true**.</span></span>
- <span data-ttu-id="4511d-167">Der Benutzer wurde nicht angegeben. ein **"WhatIf" = "true"** Flag.</span><span class="sxs-lookup"><span data-stu-id="4511d-167">The user hasn't specified a **WhatIf=true** flag.</span></span>

<span data-ttu-id="4511d-168">Schließlich, vergessen Sie nicht, die Ziel aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="4511d-168">Finally, don't forget to invoke the target.</span></span> <span data-ttu-id="4511d-169">In der *Publish.proj* Datei hierzu können Sie durch Hinzufügen des Ziels, um die Liste der Abhängigkeiten für den standardmäßigen **FullPublish** Ziel.</span><span class="sxs-lookup"><span data-stu-id="4511d-169">In the *Publish.proj* file, you can do this by adding the target to the dependency list for the default **FullPublish** target.</span></span> <span data-ttu-id="4511d-170">Sie müssen sicherstellen, dass die **DeployTestDBPermissions** Ziel nicht ausgeführt, bis die **PublishDbPackages** Ziel ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="4511d-170">You need to ensure that the **DeployTestDBPermissions** target is not executed until the **PublishDbPackages** target has been executed.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a><span data-ttu-id="4511d-171">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="4511d-171">Conclusion</span></span>

<span data-ttu-id="4511d-172">In diesem Thema beschrieben eine Möglichkeit, in dem Sie hinzufügen können Datenbankbenutzer und Rollenmitgliedschaften als eine Aktion nach der Bereitstellung, wenn Sie ein Datenbankprojekt bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="4511d-172">This topic described one way in which you can add database users and role memberships as a post-deployment action when you deploy a database project.</span></span> <span data-ttu-id="4511d-173">Dies ist in der Regel hilfreich, wenn Sie regelmäßig neu, eine Datenbank in einer testumgebung erstellen, aber es in der Regel vermieden werden, sollte Wenn Sie Datenbanken auf Staging-oder produktionsumgebung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="4511d-173">This is typically useful when you regularly re-create a database in a test environment, but it should usually be avoided when you deploy databases to staging or production environments.</span></span> <span data-ttu-id="4511d-174">Daher sollten Sie sicherstellen, dass Sie die erforderliche bedingte Logik verwenden, sodass Datenbankbenutzer und Rollenmitgliedschaften nur erstellt werden, wenn er dazu geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="4511d-174">As such, you should ensure that you use the necessary conditional logic so that database users and role memberships are only created when it's appropriate to do so.</span></span>

## <a name="further-reading"></a><span data-ttu-id="4511d-175">Weiterführende Themen</span><span class="sxs-lookup"><span data-stu-id="4511d-175">Further Reading</span></span>

<span data-ttu-id="4511d-176">Weitere Informationen zur Verwendung von VSDBCMD Datenbankprojekte bereitstellen, finden Sie unter [Datenbankprojekte bereitstellen](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span><span class="sxs-lookup"><span data-stu-id="4511d-176">For more information on using VSDBCMD to deploy database projects, see [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="4511d-177">Anleitungen zum Anpassen von Datenbank-Bereitstellungen für unterschiedliche zielumgebungen finden Sie unter [Anpassen von Datenbank-Bereitstellungen für mehrere Umgebungen](customizing-database-deployments-for-multiple-environments.md).</span><span class="sxs-lookup"><span data-stu-id="4511d-177">For guidance on customizing database deployments for different target environments, see [Customizing Database Deployments for Multiple Environments](customizing-database-deployments-for-multiple-environments.md).</span></span> <span data-ttu-id="4511d-178">Weitere Informationen finden Sie unter benutzerdefinierte MSBuild-Projektdateien um den Bereitstellungsprozess zu steuern, finden Sie unter [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="4511d-178">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="4511d-179">Weitere Informationen zu Befehlszeilenoptionen von "Sqlcmd", finden Sie unter [Hilfsprogramms "Sqlcmd"](https://msdn.microsoft.com/library/ms162773.aspx).</span><span class="sxs-lookup"><span data-stu-id="4511d-179">For more information on sqlcmd command-line options, see [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4511d-180">[Zurück](customizing-database-deployments-for-multiple-environments.md)
[Weiter](deploying-membership-databases-to-enterprise-environments.md)</span><span class="sxs-lookup"><span data-stu-id="4511d-180">[Previous](customizing-database-deployments-for-multiple-environments.md)
[Next](deploying-membership-databases-to-enterprise-environments.md)</span></span>