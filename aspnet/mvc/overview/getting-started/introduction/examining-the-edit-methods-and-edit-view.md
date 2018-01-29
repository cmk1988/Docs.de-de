---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: "Prüfen der Methoden bearbeiten und die Bearbeitungsansicht | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: d7e1ba503b8aa815cebf431d2f5ffc9436b3575b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a><span data-ttu-id="64050-102">Prüfen der Methoden bearbeiten und die Bearbeitungsansicht</span><span class="sxs-lookup"><span data-stu-id="64050-102">Examining the Edit Methods and Edit View</span></span>
====================
<span data-ttu-id="64050-103">Durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="64050-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="64050-104">In diesem Abschnitt werden Sie untersucht den generierten `Edit` Aktionsmethoden und Ansichten für den Film-Controller.</span><span class="sxs-lookup"><span data-stu-id="64050-104">In this section, you'll examine the generated `Edit` action methods and views for the movie controller.</span></span> <span data-ttu-id="64050-105">Doch zunächst dauert eine kurze Abzweigung zu dem Veröffentlichungsdatum zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="64050-105">But first will take a short diversion to make the release date look better.</span></span> <span data-ttu-id="64050-106">Öffnen der *Models\Movie.cs* Datei, und fügen Sie den folgenden hervorgehobenen Zeilen hinzu:</span><span class="sxs-lookup"><span data-stu-id="64050-106">Open the *Models\Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

<span data-ttu-id="64050-107">Sie können auch die Datumskultur wie folgt bestimmte vornehmen:</span><span class="sxs-lookup"><span data-stu-id="64050-107">You can also make the date culture specific like this:</span></span>

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

<span data-ttu-id="64050-108">Wir behandeln [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) im nächsten Tutorial.</span><span class="sxs-lookup"><span data-stu-id="64050-108">We'll cover [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) in the next tutorial.</span></span> <span data-ttu-id="64050-109">Das [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx)-Attribut gibt an, was für den Namen eines Felds angezeigt werden soll (in diesem Fall „Release Date“ anstatt „ReleaseDate“).</span><span class="sxs-lookup"><span data-stu-id="64050-109">The [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="64050-110">Die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) Attribut gibt den Typ der Daten, die in diesem Fall ist es ein Datum ist, damit die Informationen gespeichert werden, in das Feld nicht angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="64050-110">The [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribute specifies the type of the data, in this case it's a date, so the time information stored in the field is not displayed.</span></span> <span data-ttu-id="64050-111">Die [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Attribut ist erforderlich, für einen Programmfehler in Chrome-Browser, die Datums-und Uhrzeitformate falsch gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="64050-111">The [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute is needed for a bug in the Chrome browser that renders date formats incorrectly.</span></span>

<span data-ttu-id="64050-112">Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller.</span><span class="sxs-lookup"><span data-stu-id="64050-112">Run the application and browse to the `Movies` controller.</span></span> <span data-ttu-id="64050-113">Halten Sie den Mauszeiger über ein **bearbeiten** Link, um die URL anzuzeigen, die es verknüpft.</span><span class="sxs-lookup"><span data-stu-id="64050-113">Hold the mouse pointer over an **Edit** link to see the URL that it links to.</span></span>

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

<span data-ttu-id="64050-115">Die **bearbeiten** Link wurde generiert, indem die `Html.ActionLink` Methode in der *Views\Movies\Index.cshtml* anzeigen:</span><span class="sxs-lookup"><span data-stu-id="64050-115">The **Edit** link was generated by the `Html.ActionLink` method in the *Views\Movies\Index.cshtml* view:</span></span>

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

<span data-ttu-id="64050-117">Die `Html` Objekt ist ein Hilfsprogramm, das verfügbar, die mit einer Eigenschaft gemacht wird für die [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="64050-117">The `Html` object is a helper that's exposed using a property on the [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) base class.</span></span> <span data-ttu-id="64050-118">Die `ActionLink` Methode des Hilfsprogramms ganz einfach zum dynamischen Generieren von HTML-Links, die mit Aktionsmethoden im Controller verknüpft.</span><span class="sxs-lookup"><span data-stu-id="64050-118">The `ActionLink` method of the helper makes it easy to dynamically generate HTML hyperlinks that link to action methods on controllers.</span></span> <span data-ttu-id="64050-119">Das erste Argument für die `ActionLink` Methode ist der Text des Links zum Rendern (z. B. `<a>Edit Me</a>`).</span><span class="sxs-lookup"><span data-stu-id="64050-119">The first argument to the `ActionLink` method is the link text to render (for example, `<a>Edit Me</a>`).</span></span> <span data-ttu-id="64050-120">Das zweite Argument ist der Name der aufzurufenden Aktionsmethode (In diesem Fall die `Edit` Aktion).</span><span class="sxs-lookup"><span data-stu-id="64050-120">The second argument is the name of the action method to invoke (In this case, the `Edit` action).</span></span> <span data-ttu-id="64050-121">Das letzte Argument ist ein [anonymes Objekt](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , generiert die Routendaten (in diesem Fall wird die ID des 4).</span><span class="sxs-lookup"><span data-stu-id="64050-121">The final argument is an [anonymous object](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) that generates the route data (in this case, the ID of 4).</span></span>

<span data-ttu-id="64050-122">Der generierte Link in der vorherigen Abbildung dargestellt ist `http://localhost:1234/Movies/Edit/4`.</span><span class="sxs-lookup"><span data-stu-id="64050-122">The generated link shown in the previous image is `http://localhost:1234/Movies/Edit/4`.</span></span> <span data-ttu-id="64050-123">Die Standardroute (in vielen Branchen *App\_Start\RouteConfig.cs*) nimmt das URL-Muster `{controller}/{action}/{id}`.</span><span class="sxs-lookup"><span data-stu-id="64050-123">The default route (established in *App\_Start\RouteConfig.cs*) takes the URL pattern `{controller}/{action}/{id}`.</span></span> <span data-ttu-id="64050-124">Aus diesem Grund ASP.NET übersetzt `http://localhost:1234/Movies/Edit/4` in eine Anforderung an die `Edit` Aktionsmethode des der `Movies` Controller mit dem Parameter `ID` gleich 4.</span><span class="sxs-lookup"><span data-stu-id="64050-124">Therefore, ASP.NET translates `http://localhost:1234/Movies/Edit/4` into a request to the `Edit` action method of the `Movies` controller with the parameter `ID` equal to 4.</span></span> <span data-ttu-id="64050-125">Überprüfen Sie den folgenden Code aus der *App\_Start\RouteConfig.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="64050-125">Examine the following code from the *App\_Start\RouteConfig.cs* file.</span></span> <span data-ttu-id="64050-126">Die [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) Methode wird verwendet, um HTTP-Anforderungen an die richtige Methode der Controller- und weiterleiten, und geben Sie den optionalen ID-Parameter.</span><span class="sxs-lookup"><span data-stu-id="64050-126">The [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) method is used to route HTTP requests to the correct controller and action method and supply the optional ID parameter.</span></span> <span data-ttu-id="64050-127">Die [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) Methode dient auch durch die [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) wie z. B. `ActionLink` zum Generieren von URLs anhand des Controllers, Aktionsmethode und keine Routendaten.</span><span class="sxs-lookup"><span data-stu-id="64050-127">The [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) method is also used by the [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) such as `ActionLink` to generate URLs given the controller, action method and any route data.</span></span>

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

<span data-ttu-id="64050-128">Sie können auch die Aktionsmethodenparameter über eine Abfragezeichenfolge übergeben.</span><span class="sxs-lookup"><span data-stu-id="64050-128">You can also pass action method parameters using a query string.</span></span> <span data-ttu-id="64050-129">Beispielsweise die URL `http://localhost:1234/Movies/Edit?ID=3` übergibt außerdem den Parameter `ID` 3 der `Edit` Aktionsmethode von der `Movies` Controller.</span><span class="sxs-lookup"><span data-stu-id="64050-129">For example, the URL `http://localhost:1234/Movies/Edit?ID=3` also passes the parameter `ID` of 3 to the `Edit` action method of the `Movies` controller.</span></span>

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

<span data-ttu-id="64050-131">Öffnen der `Movies` Controller.</span><span class="sxs-lookup"><span data-stu-id="64050-131">Open the `Movies` controller.</span></span> <span data-ttu-id="64050-132">Die beiden `Edit` Aktionsmethoden unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="64050-132">The two `Edit` action methods are shown below.</span></span>

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

<span data-ttu-id="64050-133">Beachten Sie, dass der zweiten `Edit`-Aktionsmethode das `HttpPost`-Attribut vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="64050-133">Notice the second `Edit` action method is preceded by the `HttpPost` attribute.</span></span> <span data-ttu-id="64050-134">Dieses Attribut gibt an, dass die Überladung von der `Edit` Methode kann nur für die POST-Anforderungen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="64050-134">This attribute specifies that the overload of the `Edit` method can be invoked only for POST requests.</span></span> <span data-ttu-id="64050-135">Sie können Werte anwenden der `HttpGet` Attribut mit dem ersten bearbeiten Methode, aber dies ist nicht erforderlich, da dies die Standardeinstellung ist.</span><span class="sxs-lookup"><span data-stu-id="64050-135">You could apply the `HttpGet` attribute to the first edit method, but that's not necessary because it's the default.</span></span> <span data-ttu-id="64050-136">(Verweisen wir auf Aktionsmethoden, die implizit zugewiesen sind die `HttpGet` Attribut `HttpGet` Methoden.) Die [binden](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) Attribut ist eine andere wichtige Sicherheitsmechanismus, der verhindert, dass Hacker zu stark Senden von Daten mit dem Modell.</span><span class="sxs-lookup"><span data-stu-id="64050-136">(We'll refer to action methods that are implicitly assigned the `HttpGet` attribute as `HttpGet` methods.) The [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute is another important security mechanism that keeps hackers from over-posting data to your model.</span></span> <span data-ttu-id="64050-137">Sie sollten nur Eigenschaften in der Bind-Attribut enthalten, die Sie ändern möchten.</span><span class="sxs-lookup"><span data-stu-id="64050-137">You should only include properties in the bind attribute that you want to change.</span></span> <span data-ttu-id="64050-138">Informieren Sie sich über Overposting und die Bind-Attribut in meinem [overposting Sicherheitshinweis](https://go.microsoft.com/fwlink/?LinkId=317598).</span><span class="sxs-lookup"><span data-stu-id="64050-138">You can read about overposting and the bind attribute in my [overposting security note](https://go.microsoft.com/fwlink/?LinkId=317598).</span></span> <span data-ttu-id="64050-139">In dem einfachen Wiederherstellungsmodell, die in diesem Lernprogramm verwendet wird werden alle Daten im Modell gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="64050-139">In the simple model used in this tutorial, we will be binding all the data in the model.</span></span> <span data-ttu-id="64050-140">Die [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) Attribut wird verwendet, um zu verhindern, dass Fälschung einer Anforderung und wird mit-Rückrufvertrag `@Html.AntiForgeryToken()` in die Ansichtsdatei bearbeiten (*Views\Movies\Edit.cshtml*), ein Teil wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="64050-140">The [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) attribute is used to prevent forgery of a request and is paired up with `@Html.AntiForgeryToken()` in the edit view file (*Views\Movies\Edit.cshtml*), a portion is shown below:</span></span>

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

<span data-ttu-id="64050-141">`@Html.AntiForgeryToken()`generiert eine verborgene antifälschungs-Token, das im übereinstimmen muss die `Edit` Methode der `Movies` Controller.</span><span class="sxs-lookup"><span data-stu-id="64050-141">`@Html.AntiForgeryToken()` generates a hidden form anti-forgery token that must match in the `Edit` method of the `Movies` controller.</span></span> <span data-ttu-id="64050-142">Erfahren Sie mehr über Cross-Site request Fälschung (auch bekannt als XSRF oder FORGERY) in meinem Lernprogramm [XSRF/vom CSRF-Schutz in MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).</span><span class="sxs-lookup"><span data-stu-id="64050-142">You can read more about Cross-site request forgery (also known as XSRF or CSRF) in my tutorial [XSRF/CSRF Prevention in MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).</span></span>

<span data-ttu-id="64050-143">Die `HttpGet` `Edit` Methode nimmt die Film-ID-Parameter, mit dem Entity Framework Film sucht `Find` -Methode, und gibt den ausgewählten Film an die Bearbeitungsansicht zurück.</span><span class="sxs-lookup"><span data-stu-id="64050-143">The `HttpGet` `Edit` method takes the movie ID parameter, looks up the movie using the Entity Framework `Find` method, and returns the selected movie to the Edit view.</span></span> <span data-ttu-id="64050-144">Wenn Sie ein Film nicht gefunden werden kann, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="64050-144">If a movie cannot be found, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) is returned.</span></span> <span data-ttu-id="64050-145">Als das Gerüstsystem die Bearbeitungsansicht erstellt hat, wurde die `Movie`-Klasse überprüft und Code zum Rendern der `<label>`- und `<input>`-Elemente für jede Eigenschaft der Klasse erstellt.</span><span class="sxs-lookup"><span data-stu-id="64050-145">When the scaffolding system created the Edit view, it examined the `Movie` class and created code to render `<label>` and `<input>` elements for each property of the class.</span></span> <span data-ttu-id="64050-146">Das folgende Beispiel zeigt die Bearbeitungsansicht, die von der visual Studio-Gerüstbau-System generiert wurde:</span><span class="sxs-lookup"><span data-stu-id="64050-146">The following example shows the Edit view that was generated by the visual studio scaffolding system:</span></span>

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

<span data-ttu-id="64050-147">Beachten Sie, wie die Vorlage für die Sicht hat eine `@model MvcMovie.Models.Movie` -Anweisung am Anfang der Datei – Dies gibt an, dass die Sicht erwartet, das Modell für die Vorlage Sicht vom Typ dass `Movie`.</span><span class="sxs-lookup"><span data-stu-id="64050-147">Notice how the view template has a `@model MvcMovie.Models.Movie` statement at the top of the file — this specifies that the view expects the model for the view template to be of type `Movie`.</span></span>

<span data-ttu-id="64050-148">Der scaffolded Code verwendet mehrere *Hilfsmethoden* um das HTML-Markup zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="64050-148">The scaffolded code uses several *helper methods* to streamline the HTML markup.</span></span> <span data-ttu-id="64050-149">Die [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Hilfsprogramm zeigt den Namen des Felds (&quot;Titel&quot;, &quot;ReleaseDate&quot;, &quot;"Genre"&quot;, oder &quot;Preis &quot;).</span><span class="sxs-lookup"><span data-stu-id="64050-149">The [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) helper displays the name of the field (&quot;Title&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, or &quot;Price&quot;).</span></span> <span data-ttu-id="64050-150">Die [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) -Hilfsmethode rendert ein HTML `<input>` Element.</span><span class="sxs-lookup"><span data-stu-id="64050-150">The [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) helper renders an HTML `<input>` element.</span></span> <span data-ttu-id="64050-151">Die [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Hilfsprogramm zeigt Fehlermeldungen Validierung dieser Eigenschaft zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="64050-151">The [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) helper displays any validation messages associated with that property.</span></span>

<span data-ttu-id="64050-152">Führen Sie die Anwendung, und navigieren Sie zu der */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="64050-152">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="64050-153">Klicken Sie auf einen Link **Bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="64050-153">Click an **Edit** link.</span></span> <span data-ttu-id="64050-154">Zeigen Sie im Browser den Quelltext für die Seite an.</span><span class="sxs-lookup"><span data-stu-id="64050-154">In the browser, view the source for the page.</span></span> <span data-ttu-id="64050-155">Der HTML-Code für das Form-Element wird unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="64050-155">The HTML for the form element is shown below.</span></span>

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

<span data-ttu-id="64050-156">Die `<input>` Elemente sind in einem HTML- `<form>` Element, dessen `action` -Attribut festgelegt ist, an die */Filme/bearbeiten* URL.</span><span class="sxs-lookup"><span data-stu-id="64050-156">The `<input>` elements are in an HTML `<form>` element whose `action` attribute is set to post to the */Movies/Edit* URL.</span></span> <span data-ttu-id="64050-157">Die Formulardaten werden an den Server gesendet werden bei der **speichern** geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="64050-157">The form data will be posted to the server when the **Save** button is clicked.</span></span> <span data-ttu-id="64050-158">Die zweite Linie zeigt die ausgeblendeten [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) generierte Token durch die `@Html.AntiForgeryToken()` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="64050-158">The second line shows the hidden [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generated by the `@Html.AntiForgeryToken()` call.</span></span>

## <a name="processing-the-post-request"></a><span data-ttu-id="64050-159">Verarbeiten der POST-Anforderung</span><span class="sxs-lookup"><span data-stu-id="64050-159">Processing the POST Request</span></span>

<span data-ttu-id="64050-160">Die folgende Liste zeigt die `HttpPost`-Version der `Edit`-Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="64050-160">The following listing shows the `HttpPost` version of the `Edit` action method.</span></span>

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

<span data-ttu-id="64050-161">Die [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) Attribut überprüft die [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) generierte Token durch die `@Html.AntiForgeryToken()` in der Ansicht aufrufen.</span><span class="sxs-lookup"><span data-stu-id="64050-161">The [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) attribute validates the [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generated by the `@Html.AntiForgeryToken()` call in the view.</span></span>

<span data-ttu-id="64050-162">Die [ASP.NET MVC-modellbindung](https://msdn.microsoft.com/library/dd410405.aspx) nimmt die übermittelte Formularwerte und erstellt eine `Movie` -Objekt, das als übergeben wird, die `movie` Parameter.</span><span class="sxs-lookup"><span data-stu-id="64050-162">The [ASP.NET MVC model binder](https://msdn.microsoft.com/library/dd410405.aspx) takes the posted form values and creates a `Movie` object that's passed as the `movie` parameter.</span></span> <span data-ttu-id="64050-163">Die `ModelState.IsValid`-Methode stellt sicher, dass die im Formular übermittelten Daten verwendet werden können, um ein `Movie`-Objekt zu ändern (bearbeiten oder aktualisieren).</span><span class="sxs-lookup"><span data-stu-id="64050-163">The `ModelState.IsValid` method verifies that the data submitted in the form can be used to modify (edit or update) a `Movie` object.</span></span> <span data-ttu-id="64050-164">Wenn die Daten gültig ist, wird die Filmdaten gespeichert, auf die `Movies` Auflistung von der `db(MovieDBContext` Instanz).</span><span class="sxs-lookup"><span data-stu-id="64050-164">If the data is valid, the movie data is saved to the `Movies` collection of the `db(MovieDBContext` instance).</span></span> <span data-ttu-id="64050-165">Die neue Filmdaten in der Datenbank gespeichert ist, durch Aufrufen der `SaveChanges` Methode `MovieDBContext`.</span><span class="sxs-lookup"><span data-stu-id="64050-165">The new movie data is saved to the database by calling the `SaveChanges` method of `MovieDBContext`.</span></span> <span data-ttu-id="64050-166">Nach dem Speichern der Daten leitet der Code den Benutzer an die `Index`-Aktionsmethode der `MoviesController`-Klasse weiter, die die Filmsammlung einschließlich der gerade vorgenommenen Änderungen anzeigt.</span><span class="sxs-lookup"><span data-stu-id="64050-166">After saving the data, the code redirects the user to the `Index` action method of the `MoviesController` class, which displays the movie collection, including the changes just made.</span></span>

<span data-ttu-id="64050-167">Sobald die clientseitige Validierung, dass die Werte eines Felds nicht gültig sind feststellt, wird eine Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="64050-167">As soon as the client side validation determines the values of a field are not valid, an error message is displayed.</span></span> <span data-ttu-id="64050-168">Wenn Sie JavaScript deaktivieren, Sie erhalten keinen clientseitige Validierung, aber der Server erkennt die bereitgestellten Werte sind ungültig und werden wieder die Formularwerte mit Fehlermeldungen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="64050-168">If you disable JavaScript, you won't have client side validation but the server will detect the posted values are not valid, and the form values will be redisplayed with error messages.</span></span> <span data-ttu-id="64050-169">Später in diesem Lernprogramm untersuchen wir die Überprüfung im Detail.</span><span class="sxs-lookup"><span data-stu-id="64050-169">Later in the tutorial we examine validation in more detail.</span></span>

<span data-ttu-id="64050-170">Die `Html.ValidationMessageFor` Hilfen in den *Edit.cshtml* anzeigen, die Vorlage zum Anzeigen von entsprechenden Fehlermeldungen achten.</span><span class="sxs-lookup"><span data-stu-id="64050-170">The `Html.ValidationMessageFor` helpers in the *Edit.cshtml* view template take care of displaying appropriate error messages.</span></span>

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

<span data-ttu-id="64050-172">Alle der `HttpGet` Methoden einem ähnliches Muster folgen.</span><span class="sxs-lookup"><span data-stu-id="64050-172">All the `HttpGet` methods follow a similar pattern.</span></span> <span data-ttu-id="64050-173">Sie erhalten ein Movie-Objekt (oder eine Liste von Objekten, in der Groß-/Kleinschreibung `Index`), und übergeben Sie das Modell zur Ansicht.</span><span class="sxs-lookup"><span data-stu-id="64050-173">They get a movie object (or list of objects, in the case of `Index`), and pass the model to the view.</span></span> <span data-ttu-id="64050-174">Die `Create` Methode übergibt ein leeres Movie-Objekt an die Erstellungsansicht.</span><span class="sxs-lookup"><span data-stu-id="64050-174">The `Create` method passes an empty movie object to the Create view.</span></span> <span data-ttu-id="64050-175">Alle Methoden, die Daten erstellen, bearbeiten, löschen oder in beliebiger Weise ändern, nutzen dazu die `HttpPost`-Überladung der Methode.</span><span class="sxs-lookup"><span data-stu-id="64050-175">All the methods that create, edit, delete, or otherwise modify data do so in the `HttpPost` overload of the method.</span></span> <span data-ttu-id="64050-176">Ändern von Daten in einer HTTP GET-Methode ist ein Sicherheitsrisiko, gemäß der in der Post-Blogeintrag [ASP.NET MVC Tipp #46 – verwenden Sie keine Links zu löschen, da Sicherheitslücken entstehen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).</span><span class="sxs-lookup"><span data-stu-id="64050-176">Modifying data in an HTTP GET method is a security risk, as described in the blog post entry [ASP.NET MVC Tip #46 – Don't use Delete Links because they create Security Holes](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).</span></span> <span data-ttu-id="64050-177">Ändern von Daten in einer GET-Methode auch verletzt, bewährte Methoden für HTTP und das architektonische [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) Muster, der angibt, dass die GET-Anforderungen sollten nicht den Status Ihrer Anwendung ändern.</span><span class="sxs-lookup"><span data-stu-id="64050-177">Modifying data in a GET method also violates HTTP best practices and the architectural [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) pattern, which specifies that GET requests should not change the state of your application.</span></span> <span data-ttu-id="64050-178">Die Durchführung eines GET-Vorgangs sollte also eine sichere Operation sein, die keine Nebenwirkungen hat und die permanenten Daten nicht ändert.</span><span class="sxs-lookup"><span data-stu-id="64050-178">In other words, performing a GET operation should be a safe operation that has no side effects and doesn't modify your persisted data.</span></span>

<span data-ttu-id="64050-179">Wenn Sie einen US-Englisch-Computer verwenden, können Sie überspringen Sie diesen Abschnitt und wechseln Sie zum nächsten Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="64050-179">If you are using a US-English computer, you can skip this section and go to the next tutorial.</span></span> <span data-ttu-id="64050-180">Sie können die Globalize Version dieses Lernprogramms herunterladen [hier](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475).</span><span class="sxs-lookup"><span data-stu-id="64050-180">You can download the Globalize version of this tutorial [here](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475).</span></span> <span data-ttu-id="64050-181">Eine hervorragende auf Internationalisierung zweiteiligen-Lernprogramm, finden Sie unter [Nadeems ASP.NET MVC 5-Internationalisierung](http://afana.me/post/aspnet-mvc-internationalization.aspx).</span><span class="sxs-lookup"><span data-stu-id="64050-181">For an excellent two part tutorial on Internationalization, see [Nadeem's ASP.NET MVC 5 Internationalization](http://afana.me/post/aspnet-mvc-internationalization.aspx).</span></span>


> [!NOTE]
> <span data-ttu-id="64050-182">Darin jQuery-Validierung bei nicht englischen Gebietsschemas zu unterstützen, verwenden ein Komma (&quot;,&quot;) für ein Dezimaltrennzeichen und einem nicht US-englischen Datums-und Uhrzeitformate, enthalten Sie *globalize.js* und Ihre spezifischen  *Cultures/globalize.Cultures.js* Datei (aus [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) und JavaScript verwenden `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="64050-182">to support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="64050-183">Sie können die jQuery-Validierung nicht englischen von NuGet abrufen.</span><span class="sxs-lookup"><span data-stu-id="64050-183">You can get the jQuery non-English validation from NuGet.</span></span> <span data-ttu-id="64050-184">(Nicht installieren Globalize bei Verwendung von einem englischen Gebietsschema.)</span><span class="sxs-lookup"><span data-stu-id="64050-184">(Don't install Globalize if you are using a English locale.)</span></span>


1. <span data-ttu-id="64050-185">Aus der **Tools** klicken Sie im Menü **NuGetLibrary Package Manager**, und klicken Sie dann auf **NuGet-Pakete für Projektmappe verwalten**.</span><span class="sxs-lookup"><span data-stu-id="64050-185">From the **Tools** menu click **NuGetLibrary Package Manager**, and then click **Manage NuGet Packages for Solution**.</span></span>  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. <span data-ttu-id="64050-186">Wählen Sie im linken Bereich **Durchsuchen*. *** (siehe folgende Abbildung).</span><span class="sxs-lookup"><span data-stu-id="64050-186">On the left pane, select **Browse*.***(See the image below.)</span></span>
3. <span data-ttu-id="64050-187">Geben Sie in das Eingabefeld \* Globalize \*\*.</span><span class="sxs-lookup"><span data-stu-id="64050-187">In the input box, enter \*Globalize\*\*.</span></span>  
  
    <span data-ttu-id="64050-188">![](examining-the-edit-methods-and-edit-view/_static/image6.png)Wählen Sie `jQuery.Validation.Globalize`, wählen Sie `MvcMovie` , und klicken Sie auf **installieren**.</span><span class="sxs-lookup"><span data-stu-id="64050-188">![](examining-the-edit-methods-and-edit-view/_static/image6.png) Choose `jQuery.Validation.Globalize`, choose `MvcMovie` and click **Install**.</span></span> <span data-ttu-id="64050-189">Die *Scripts\jquery.globalize\globalize.js* Datei wird dem Projekt hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="64050-189">The *Scripts\jquery.globalize\globalize.js* file will be added to your project.</span></span> <span data-ttu-id="64050-190">Die *Scripts\jquery.globalize\cultures\* Ordner viele Kultur JavaScript-Dateien enthält.</span><span class="sxs-lookup"><span data-stu-id="64050-190">The *Scripts\jquery.globalize\cultures\* folder will contain many culture JavaScript files.</span></span> <span data-ttu-id="64050-191">Beachten Sie, dass es beim Installieren dieses Pakets fünf Minuten dauern.</span><span class="sxs-lookup"><span data-stu-id="64050-191">Note, it may take five minutes to install this package.</span></span>

 <span data-ttu-id="64050-192">Der folgende Code zeigt die Änderungen an der Datei Views\Movies\Edit.cshtml:</span><span class="sxs-lookup"><span data-stu-id="64050-192">The following code shows the modifications to the Views\Movies\Edit.cshtml file:</span></span> 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

<span data-ttu-id="64050-193">Um zu vermeiden, wiederholen diesen Code in jeder Ansicht bearbeiten, können Sie es auf die Layoutdatei verschieben.</span><span class="sxs-lookup"><span data-stu-id="64050-193">To avoid repeating this code in every Edit view, you can move it to the layout file.</span></span> <span data-ttu-id="64050-194">Um das Skript herunterladen zu optimieren, finden Sie unter meinem Lernprogramm [Bündelung und Minimierung](../../performance/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="64050-194">To optimize the script download, see my tutorial [Bundling and Minification](../../performance/bundling-and-minification.md).</span></span>

<span data-ttu-id="64050-195">Weitere Informationen finden Sie unter [ASP.NET MVC 3-Internationalisierung](http://afana.me/post/aspnet-mvc-internationalization.aspx) und [ASP.NET MVC 3-Internationalisierung – Teil 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).</span><span class="sxs-lookup"><span data-stu-id="64050-195">For more information see [ASP.NET MVC 3 Internationalization](http://afana.me/post/aspnet-mvc-internationalization.aspx) and [ASP.NET MVC 3 Internationalization - Part 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).</span></span>

<span data-ttu-id="64050-196">Als temporäres Problem behoben Wenn Überprüfung arbeiten in Ihrem Gebietsschema kann nicht angezeigt werden, Sie können erzwingen, dass Ihre Computer auf Englisch (USA) verwenden oder Deaktivieren von JavaScript in Ihrem Browser.</span><span class="sxs-lookup"><span data-stu-id="64050-196">As a temporary fix, if you can't get validation working in your locale, you can force your computer to use US English or you can disable JavaScript in your browser.</span></span> <span data-ttu-id="64050-197">Um Ihren Computer auf Englisch (USA) verwenden zu erzwingen, können Sie das Globalisierung-Element hinzufügen, in das Stammverzeichnis Projekte *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="64050-197">To force your computer to use US English, you can add the globalization element to the projects root *web.config* file.</span></span> <span data-ttu-id="64050-198">Der folgende Code zeigt die Globalisierung-Element mit der Kultur Englisch (USA) festgelegt.</span><span class="sxs-lookup"><span data-stu-id="64050-198">The following code shows the globalization element with the culture set to United States English.</span></span>

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><span data-ttu-id="64050-199"><a id="jQueryAjaxJSON"></a>In den nächsten Lernprogrammen implementieren wir Suchfunktion.</span><span class="sxs-lookup"><span data-stu-id="64050-199"><a id="jQueryAjaxJSON"></a> In the next tutorial, we'll implement search functionality.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="64050-200">[Zurück](accessing-your-models-data-from-a-controller.md)
[Weiter](adding-search.md)</span><span class="sxs-lookup"><span data-stu-id="64050-200">[Previous](accessing-your-models-data-from-a-controller.md)
[Next](adding-search.md)</span></span>