---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Verwendung ViewData und ViewModel-Klassen implementieren | Microsoft Docs
author: microsoft
description: "Schritt 6 zeigt wie Aktivieren der Unterstützung für umfangreichere Formular Szenarien, bearbeiten und erläutert außerdem zwei Ansätze, die verwendet werden können, um Daten aus der Controller mit Ansichten zu übergeben:..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 36b9e87cc24f74f7f2cc592afb5102709b598f74
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a><span data-ttu-id="65ecf-103">ViewData verwenden und implementieren ViewModel-Klassen</span><span class="sxs-lookup"><span data-stu-id="65ecf-103">Use ViewData and Implement ViewModel Classes</span></span>
====================
<span data-ttu-id="65ecf-104">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="65ecf-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="65ecf-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="65ecf-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="65ecf-106">Dies ist Schritt 6 mit einer kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.</span><span class="sxs-lookup"><span data-stu-id="65ecf-106">This is step 6 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="65ecf-107">Schritt 6 zeigt wie Aktivieren der Unterstützung für umfangreichere Formular Szenarien, bearbeiten und erläutert außerdem zwei Ansätze, die verwendet werden können, um Daten aus der Controller mit Ansichten zu übergeben: ViewData und ViewModel.</span><span class="sxs-lookup"><span data-stu-id="65ecf-107">Step 6 shows how enable support for richer form editing scenarios, and also discusses two approaches that can be used to pass data from controllers to views: ViewData and ViewModel.</span></span>
> 
> <span data-ttu-id="65ecf-108">Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.</span><span class="sxs-lookup"><span data-stu-id="65ecf-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a><span data-ttu-id="65ecf-109">NerdDinner Schritt 6: ViewData und ViewModel</span><span class="sxs-lookup"><span data-stu-id="65ecf-109">NerdDinner Step 6: ViewData and ViewModel</span></span>

<span data-ttu-id="65ecf-110">Wir haben eine Reihe von Formular-Post-Szenarien abgedeckt und Implementierung erläutert erstellen, aktualisieren und löschen (CRUD) unterstützt.</span><span class="sxs-lookup"><span data-stu-id="65ecf-110">We've covered a number of form post scenarios, and discussed how to implement create, update and delete (CRUD) support.</span></span> <span data-ttu-id="65ecf-111">Wir nun unsere DinnersController Implementierung weitere ergreifen und Aktivieren der Unterstützung für umfangreichere Formular Szenarien bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="65ecf-111">We'll now take our DinnersController implementation further and enable support for richer form editing scenarios.</span></span> <span data-ttu-id="65ecf-112">Während der Durchführung dieser besprechen wir zwei Ansätze, die verwendet werden können, um Daten aus der Controller mit Ansichten zu übergeben: ViewData und ViewModel.</span><span class="sxs-lookup"><span data-stu-id="65ecf-112">While doing this we'll discuss two approaches that can be used to pass data from controllers to views: ViewData and ViewModel.</span></span>

### <a name="passing-data-from-controllers-to-view-templates"></a><span data-ttu-id="65ecf-113">Übergeben von Daten von Domänencontrollern an Vorlagen anzeigen</span><span class="sxs-lookup"><span data-stu-id="65ecf-113">Passing Data from Controllers to View-Templates</span></span>

<span data-ttu-id="65ecf-114">Keines der definierenden Eigenschaften das MVC-Schema handelt es sich um die strikte "Trennung von Anliegen" sie hilft bei der zwischen den verschiedenen Komponenten einer Anwendung zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="65ecf-114">One of the defining characteristics of the MVC pattern is the strict "separation of concerns" it helps enforce between the different components of an application.</span></span> <span data-ttu-id="65ecf-115">Modelle, Controller und Ansichten jedes gut definiert und Rollen und Zuständigkeiten und Kommunikation untereinander in gut definierte Weise.</span><span class="sxs-lookup"><span data-stu-id="65ecf-115">Models, Controllers and Views each have well defined roles and responsibilities, and they communicate amongst each other in well defined ways.</span></span> <span data-ttu-id="65ecf-116">Dadurch können höher stufen Prüfbarkeit und Wiederverwendung von code.</span><span class="sxs-lookup"><span data-stu-id="65ecf-116">This helps promote testability and code reuse.</span></span>

<span data-ttu-id="65ecf-117">Wenn eine Controllerklasse entscheidet, eine HTML-Antwort an den Client zu rendern, wird die verantwortlich für die Übergabe explizit an die Vorlage Anzeigen aller Daten benötigt, um die Antwort zu rendern.</span><span class="sxs-lookup"><span data-stu-id="65ecf-117">When a Controller class decides to render an HTML response back to a client, it is responsible for explicitly passing to the view template all of the data needed to render the response.</span></span> <span data-ttu-id="65ecf-118">Ansichtsvorlagen keine Daten abrufen oder eine Anwendung die Logik – sollten nie ausführen und sollten stattdessen einschränken selbst, um nur Renderingcodes verfügen, die aus den Modelldaten/vom Controller übergebenen gesteuert wird.</span><span class="sxs-lookup"><span data-stu-id="65ecf-118">View templates should never perform any data retrieval or application logic – and should instead limit themselves to only have rendering code that is driven off of the model/data passed to it by the controller.</span></span>

<span data-ttu-id="65ecf-119">Jetzt die Modelldaten durch unsere DinnersController übergebenen Klasse, um unsere Ansichtsvorlagen ist einfach und selbsterklärend – im Falle von Details(), Edit(), Create()- und Delete() eine Liste von Dinner Objekten im Fall von Index() und eine einzelne Dinner Objekt.</span><span class="sxs-lookup"><span data-stu-id="65ecf-119">Right now the model data being passed by our DinnersController class to our view templates is simple and straight-forward – a list of Dinner objects in the case of Index(), and a single Dinner object in the case of Details(), Edit(), Create() and Delete().</span></span> <span data-ttu-id="65ecf-120">Wenn wir unsere Anwendung weitere Benutzeroberflächenfunktionen hinzufügen, werden häufig wir müssen mehr als nur diese Daten für das Rendern von HTML-Antworten in unserer Ansichtsvorlagen übergeben.</span><span class="sxs-lookup"><span data-stu-id="65ecf-120">As we add more UI capabilities to our application, we are often going to need to pass more than just this data to render HTML responses within our view templates.</span></span> <span data-ttu-id="65ecf-121">Wir möchten beispielsweise ändern Sie das Feld "Country" in unseren bearbeiten und Erstellen von Sichten wird ein HTML-Textfeld zu einer Dropdownlist.</span><span class="sxs-lookup"><span data-stu-id="65ecf-121">For example, we might want to change the "Country" field within our Edit and Create views from being an HTML textbox to a dropdownlist.</span></span> <span data-ttu-id="65ecf-122">Anstatt hartcodieren der Dropdown-Liste Ländernamen in der Vorlage anzeigen sollten wir es aus einer Liste mit unterstützten Ländern zu generieren, die wir dynamisch aufgefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="65ecf-122">Rather than hard-code the dropdown list of country names in the view template, we might want to generate it from a list of supported countries that we populate dynamically.</span></span> <span data-ttu-id="65ecf-123">Wir benötigen eine Möglichkeit, sowohl die Dinner-Objekt übergeben *und* die Liste der unterstützten Ländern aus unserem Controller zu unserem Vorlagen anzeigen.</span><span class="sxs-lookup"><span data-stu-id="65ecf-123">We will need a way to pass both the Dinner object *and* the list of supported countries from our controller to our view templates.</span></span>

<span data-ttu-id="65ecf-124">Betrachten wir zwei Möglichkeiten, die wir dies zu erreichen können.</span><span class="sxs-lookup"><span data-stu-id="65ecf-124">Let's look at two ways we can accomplish this.</span></span>

### <a name="using-the-viewdata-dictionary"></a><span data-ttu-id="65ecf-125">Verwenden das ViewData-Wörterbuch</span><span class="sxs-lookup"><span data-stu-id="65ecf-125">Using the ViewData Dictionary</span></span>

<span data-ttu-id="65ecf-126">Die Basisklasse für Controller stellt die Wörterbucheigenschaft "ViewData", die verwendet werden kann, um zusätzliche Datenelemente aus Controller mit Ansichten zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="65ecf-126">The Controller base class exposes a "ViewData" dictionary property that can be used to pass additional data items from Controllers to Views.</span></span>

<span data-ttu-id="65ecf-127">Beispielsweise können zur Unterstützung des Szenarios, in dem wir das Textfeld "Country" in unseren Bearbeitungsansicht ändern, wird ein HTML-Textfeld zu einer Dropdownlist möchten, wir aktualisieren unsere Edit()-Aktionsmethode, um ein SelectList-Objekt (zusätzlich zum ein Dinner-Objekt) zu übergeben, die als das m verwendet werden können Odel der Dropdownlist Ländern.</span><span class="sxs-lookup"><span data-stu-id="65ecf-127">For example, to support the scenario where we want to change the "Country" textbox within our Edit view from being an HTML textbox to a dropdownlist, we can update our Edit() action method to pass (in addition to a Dinner object) a SelectList object that can be used as the model of a countries dropdownlist.</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

<span data-ttu-id="65ecf-128">Der Konstruktor für die oben genannten SelectList nimmt eine Liste der Landkreise zum Auffüllen der Drop-Downlist mit als auch den derzeit ausgewählten Wert.</span><span class="sxs-lookup"><span data-stu-id="65ecf-128">The constructor of the SelectList above is accepting a list of counties to populate the drop-downlist with, as well as the currently selected value.</span></span>

<span data-ttu-id="65ecf-129">Wir können dann aktualisieren, dass unsere Vorlage Edit.aspx anzeigen, um die Hilfsmethode Html.DropDownList() statt die Hilfsmethode Html.TextBox() zu verwenden, die wir zuvor verwendet:</span><span class="sxs-lookup"><span data-stu-id="65ecf-129">We can then update our Edit.aspx view template to use the Html.DropDownList() helper method instead of the Html.TextBox() helper method we used previously:</span></span>

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

<span data-ttu-id="65ecf-130">Die oben genannten Html.DropDownList() Hilfsmethode akzeptiert zwei Parameter.</span><span class="sxs-lookup"><span data-stu-id="65ecf-130">The Html.DropDownList() helper method above takes two parameters.</span></span> <span data-ttu-id="65ecf-131">Die erste ist der Name des HTML-Formularelements ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="65ecf-131">The first is the name of the HTML form element to output.</span></span> <span data-ttu-id="65ecf-132">Das zweite ist das "SelectList"-Modell, das es über das ViewData-Wörterbuch übergeben.</span><span class="sxs-lookup"><span data-stu-id="65ecf-132">The second is the "SelectList" model we passed via the ViewData dictionary.</span></span> <span data-ttu-id="65ecf-133">Wird der C#-"as"-Schlüsselwort verwendet, der im Wörterbuch als eine SelectList umgewandelt.</span><span class="sxs-lookup"><span data-stu-id="65ecf-133">We are using the C# "as" keyword to cast the type within the dictionary as a SelectList.</span></span>

<span data-ttu-id="65ecf-134">Und nun wir Ausführung von unserem Anwendung und den Zugriff der */Dinners/Edit/1* URL innerhalb dieser Browser wir sehen, dass unsere Bearbeitungsoberfläche aktualisiert wurde, und eine Dropdownlist Länder oder Regionen, statt ein Textfeld angezeigt:</span><span class="sxs-lookup"><span data-stu-id="65ecf-134">And now when we run our application and access the */Dinners/Edit/1* URL within our browser we'll see that our edit UI has been updated to display a dropdownlist of countries instead of a textbox:</span></span>

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

<span data-ttu-id="65ecf-135">Da wir auch zu Rendern der Vorlage bearbeiten anzeigen, von der HTTP-POST bearbeiten-Methode (in Fällen, bei Auftreten eines Fehlers), benötigen wir sicherstellen, dass wir auch diese Methode, um das Hinzufügen der SelectList ViewData beim Rendern der Vorlage anzeigen in Fehlerszenarien aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="65ecf-135">Because we also render the Edit view template from the HTTP-POST Edit method (in scenarios when errors occur), we'll want to make sure that we also update this method to add the SelectList to ViewData when the view template is rendered in error scenarios:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

<span data-ttu-id="65ecf-136">Und nun unterstützt unsere DinnersController bearbeiten Szenario eine DropDownList.</span><span class="sxs-lookup"><span data-stu-id="65ecf-136">And now our DinnersController edit scenario supports a DropDownList.</span></span>

### <a name="using-a-viewmodel-pattern"></a><span data-ttu-id="65ecf-137">Verwenden ein ViewModel-Muster</span><span class="sxs-lookup"><span data-stu-id="65ecf-137">Using a ViewModel Pattern</span></span>

<span data-ttu-id="65ecf-138">Der ViewData Wörterbuch Ansatz hat den Vorteil, relativ schnell und einfach zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="65ecf-138">The ViewData dictionary approach has the benefit of being fairly fast and easy to implement.</span></span> <span data-ttu-id="65ecf-139">Einige Entwickler nicht gefällt zeichenfolgenbasierte Wörterbücher verwenden, da Tippfehler zu Fehlern führen können, die zum Zeitpunkt der Kompilierung nicht abgefangen wird.</span><span class="sxs-lookup"><span data-stu-id="65ecf-139">Some developers don't like using string-based dictionaries, though, since typos can lead to errors that will not be caught at compile-time.</span></span> <span data-ttu-id="65ecf-140">Nicht typisierte ViewData-Wörterbuch ist auch mithilfe des Operators "als" oder bei Verwendung in einer Vorlage für die Sicht eine stark typisierte Sprache wie C#-Umwandlung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="65ecf-140">The un-typed ViewData dictionary also requires using the "as" operator or casting when using a strongly-typed language like C# in a view template.</span></span>

<span data-ttu-id="65ecf-141">Eine alternative Methode, die wir verwenden könnten ist häufig als "ViewModel" Muster bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="65ecf-141">An alternative approach that we could use is one often referred to as the "ViewModel" pattern.</span></span> <span data-ttu-id="65ecf-142">Bei Verwendung dieses Muster erstellen wir stark typisierter Klassen, die für unsere bestimmte Ansicht Szenarien optimiert sind und die Eigenschaften für die dynamischen Werte/Content durch unsere Ansichtsvorlagen erforderlich machen.</span><span class="sxs-lookup"><span data-stu-id="65ecf-142">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="65ecf-143">Unsere Controllerklassen können Auffüllen und diese Ansicht optimiert Klassen an unsere anzeigen, die zu verwendende Vorlage übergeben.</span><span class="sxs-lookup"><span data-stu-id="65ecf-143">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="65ecf-144">Dies ermöglicht typsicherheit, kompilierzeitüberprüfung und -Editor Intellisense in Vorlagen anzeigen.</span><span class="sxs-lookup"><span data-stu-id="65ecf-144">This enables type-safety, compile-time checking, and editor intellisense within view templates.</span></span>

<span data-ttu-id="65ecf-145">Um beispielsweise Dinner Formular Bearbeitung Szenarien erstellen wir eine "DinnerFormViewModel" wie im folgenden-Klasse zu ermöglichen, die zwei stark typisierte Eigenschaften verfügbar macht: ein Dinner-Objekt, und der SelectList-Modell erforderlich, um die Länder Dropdownlist aufzufüllen:</span><span class="sxs-lookup"><span data-stu-id="65ecf-145">For example, to enable dinner form editing scenarios we can create a "DinnerFormViewModel" class like below that exposes two strongly-typed properties: a Dinner object, and the SelectList model needed to populate the countries dropdownlist:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

<span data-ttu-id="65ecf-146">Wir können dann aktualisieren unsere Edit() Aktionsmethode zum Erstellen der DinnerFormViewModel unter Verwendung des Dinner-Objekts, die wir aus unserer Repository abrufen, und übergeben Sie ihn an unsere Vorlage anzeigen:</span><span class="sxs-lookup"><span data-stu-id="65ecf-146">We can then update our Edit() action method to create the DinnerFormViewModel using the Dinner object we retrieve from our repository, and then pass it to our view template:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

<span data-ttu-id="65ecf-147">Wir senden dann aktualisieren, die unsere Vorlage anzeigen, damit die It "DinnerFormViewModel" anstelle einer "Dinner" erwartet-Objekt nach dem Ändern des Attributs "erbt" am oberen Rand der Seite "edit.aspx" wie folgt:</span><span class="sxs-lookup"><span data-stu-id="65ecf-147">We'll then update our view template so that it expects a "DinnerFormViewModel" instead of a "Dinner" object by changing the "inherits" attribute at the top of the edit.aspx page like so:</span></span>

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

<span data-ttu-id="65ecf-148">Sobald wir dies tun, werden Intellisense der Eigenschaft "Model" in unseren Vorlage anzeigen aktualisiert werden, um das Objektmodell des Typs DinnerFormViewModel wider, die wir ihn übergeben werden:</span><span class="sxs-lookup"><span data-stu-id="65ecf-148">Once we do this, the intellisense of the "Model" property within our view template will be updated to reflect the object model of the DinnerFormViewModel type we are passing it:</span></span>

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

<span data-ttu-id="65ecf-149">Wir können unsere Code anzeigen aus sie arbeiten dann aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="65ecf-149">We can then update our view code to work off of it.</span></span> <span data-ttu-id="65ecf-150">Beachten Sie, dass unten wie wir nicht ändern werden die Namen der Eingabeelemente erstellt wird (der Formularelemente noch den Namen "Title", "Land") – aber aktualisieren wir die HTML-Hilfsmethoden zum Abrufen der Werte, die mit der DinnerFormViewModel-Klasse:</span><span class="sxs-lookup"><span data-stu-id="65ecf-150">Notice below how we are not changing the names of the input elements we are creating (the form elements will still be named "Title", "Country") – but we are updating the HTML Helper methods to retrieve the values using the DinnerFormViewModel class:</span></span>

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

<span data-ttu-id="65ecf-151">Wir werden unsere bearbeiten Post-Methode zur Verwendung der DinnerFormViewModel-Klasse, beim Rendern der Fehler auch aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="65ecf-151">We'll also update our Edit post method to use the DinnerFormViewModel class when rendering errors:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

<span data-ttu-id="65ecf-152">Wir können auch aktualisieren unsere Create()-Aktionsmethoden, die genaue erneut zu verwenden dieselbe *DinnerFormViewModel* Klasse Länder und Regionen DropDownList innerhalb dieser ebenfalls zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="65ecf-152">We can also update our Create() action methods to re-use the exact same *DinnerFormViewModel* class to enable the countries DropDownList within those as well.</span></span> <span data-ttu-id="65ecf-153">Im folgenden sehen Sie die HTTP-GET-Implementierung:</span><span class="sxs-lookup"><span data-stu-id="65ecf-153">Below is the HTTP-GET implementation:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

<span data-ttu-id="65ecf-154">Im folgenden sehen Sie die Implementierung der Methode HTTP-POST zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="65ecf-154">Below is the implementation of the HTTP-POST Create method:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

<span data-ttu-id="65ecf-155">Und jetzt die Bildschirmen bearbeiten und erstellen Drop Downlists für das Land Entnahme unterstützen.</span><span class="sxs-lookup"><span data-stu-id="65ecf-155">And now both our Edit and Create screens support drop-downlists for picking the country.</span></span>

### <a name="custom-shaped-viewmodel-classes"></a><span data-ttu-id="65ecf-156">Benutzerdefinierte geformten ViewModel-Klassen</span><span class="sxs-lookup"><span data-stu-id="65ecf-156">Custom-shaped ViewModel classes</span></span>

<span data-ttu-id="65ecf-157">Im obigen Szenario macht das Modellobjekt Dinner als Eigenschaft zusammen mit einer unterstützenden SelectList Modelleigenschaft direkt unsere DinnerFormViewModel-Klasse verfügbar.</span><span class="sxs-lookup"><span data-stu-id="65ecf-157">In the scenario above, our DinnerFormViewModel class directly exposes the Dinner model object as a property, along with a supporting SelectList model property.</span></span> <span data-ttu-id="65ecf-158">Dieser Ansatz funktioniert gut für Szenarien, in dem die HTML-UI, die wir in unserer Ansichtenvorlage erstellen möchten relativ am ehesten unsere domänenmodellobjekten entspricht.</span><span class="sxs-lookup"><span data-stu-id="65ecf-158">This approach works fine for scenarios where the HTML UI we want to create within our view template corresponds relatively closely to our domain model objects.</span></span>

<span data-ttu-id="65ecf-159">Ist für Szenarien, in denen dies nicht, zutrifft, wird eine Option, die Sie verwenden können zum Erstellen einer benutzerdefinierten geformten ViewModel-Klasse, deren Objektmodell ist mehr für die Nutzung durch die Sicht – optimiert und kann die grundlegend von der zugrunde liegenden Modellobjekts Domäne aussehen.</span><span class="sxs-lookup"><span data-stu-id="65ecf-159">For scenarios where this isn't the case, one option that you can use is to create a custom-shaped ViewModel class whose object model is more optimized for consumption by the view – and which might look completely different from the underlying domain model object.</span></span> <span data-ttu-id="65ecf-160">Beispielsweise konnte es anderen Eigenschaftennamen und/oder Aggregieren Eigenschaften, die aus mehreren Modellobjekte gesammelt ausgesetzt.</span><span class="sxs-lookup"><span data-stu-id="65ecf-160">For example, it could potentially expose different property names and/or aggregate properties collected from multiple model objects.</span></span>

<span data-ttu-id="65ecf-161">Benutzerdefinierte geformten ViewModel-Klassen kann sowohl zum Übergeben von Daten von Domänencontrollern mit Ansichten zu rendern sowie Formulardaten, die an eine Controlleraktionsmethode zurückgesendet Bewältigung beitragen verwendet.</span><span class="sxs-lookup"><span data-stu-id="65ecf-161">Custom-shaped ViewModel classes can be used both to pass data from controllers to views to render, as well as to help handle form data posted back to a controller's action method.</span></span> <span data-ttu-id="65ecf-162">Weiter unten in diesem Szenario müssen Sie möglicherweise die Aktionsmethode ein ViewModel-Objekt mit den Daten senden Formulars aktualisieren, und verwenden Sie das ViewModel-Instanz zuordnen oder eine tatsächliche Modell Domänenobjekt abrufen.</span><span class="sxs-lookup"><span data-stu-id="65ecf-162">For this later scenario, you might have the action method update a ViewModel object with the form-posted data, and then use the ViewModel instance to map or retrieve an actual domain model object.</span></span>

<span data-ttu-id="65ecf-163">Benutzerdefinierte geformten ViewModel-Klassen bieten ein hohes Maß an Flexibilität und etwas zu einem beliebigen Zeitpunkt zu forschen, die Sie Renderingcodes innerhalb Ihrer Vorlagen anzeigen oder den Formular-Buchung Code innerhalb der Aktionsmethoden zu kompliziert abzurufenden ab finden sind.</span><span class="sxs-lookup"><span data-stu-id="65ecf-163">Custom-shaped ViewModel classes can provide a great deal of flexibility, and are something to investigate any time you find the rendering code within your view templates or the form-posting code inside your action methods starting to get too complicated.</span></span> <span data-ttu-id="65ecf-164">Dies ist häufig ein Anzeichen, dass Ihre Domänenmodelle ordnungsgemäß an die Benutzeroberfläche nicht übereinstimmen, die Sie generieren und eine benutzerdefinierte geformten ViewModel Zwischenklasse helfen kann.</span><span class="sxs-lookup"><span data-stu-id="65ecf-164">This is often a sign that your domain models don't cleanly correspond to the UI you are generating, and that an intermediate custom-shaped ViewModel class can help.</span></span>

### <a name="next-step"></a><span data-ttu-id="65ecf-165">Nächster Schritt</span><span class="sxs-lookup"><span data-stu-id="65ecf-165">Next Step</span></span>

<span data-ttu-id="65ecf-166">Jetzt betrachten wir Verwendung Replikatsgruppenelemente und Master-Seiten erneut verwenden und die Benutzeroberfläche von der Anwendung gemeinsam genutzt.</span><span class="sxs-lookup"><span data-stu-id="65ecf-166">Let's now look at how we can use partials and master-pages to re-use and share UI across our application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="65ecf-167">[Zurück](provide-crud-create-read-update-delete-data-form-entry-support.md)
[Weiter](re-use-ui-using-master-pages-and-partials.md)</span><span class="sxs-lookup"><span data-stu-id="65ecf-167">[Previous](provide-crud-create-read-update-delete-data-form-entry-support.md)
[Next](re-use-ui-using-master-pages-and-partials.md)</span></span>