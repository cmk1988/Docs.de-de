---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: "Deaktivieren Aktionen während der Animation (c#) | Microsoft Docs"
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Darüber hinaus wird die Aktion unterstützt..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 875c6be5e190c31fac030fc17ef040a934233f16
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="7ccf3-104">Deaktivieren Aktionen während der Animation (c#)</span><span class="sxs-lookup"><span data-stu-id="7ccf3-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="7ccf3-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7ccf3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7ccf3-106">[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7ccf3-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="7ccf3-107">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7ccf3-108">Darüber hinaus unterstützt Aktionen wie Mausklicks.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="7ccf3-109">Wenn ein Mausklick eine Animation gestartet wird, ist es jedoch wünschenswert, Mausklicks während der Animation deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="7ccf3-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="7ccf3-110">Overview</span></span>

<span data-ttu-id="7ccf3-111">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7ccf3-112">Darüber hinaus unterstützt Aktionen wie Mausklicks.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="7ccf3-113">Wenn ein Mausklick eine Animation gestartet wird, ist es jedoch wünschenswert, Mausklicks während der Animation deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="7ccf3-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="7ccf3-114">Steps</span></span>

<span data-ttu-id="7ccf3-115">Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="7ccf3-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="7ccf3-116">Die Animation wird in eine HTML-Schaltfläche wie folgt angewendet:</span><span class="sxs-lookup"><span data-stu-id="7ccf3-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="7ccf3-117">Beachten Sie, dass ein HTML-Steuerelement, statt ein Websteuerelement verwendet wird, da wir nicht, dass die Schaltfläche zum Erstellen eines Postbacks möchten. Es werden nur die Animation des clientseitigen für uns starten.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="7ccf3-118">Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="7ccf3-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="7ccf3-119">Innerhalb der `<Animations>` Knoten `<OnClick>` wird von der richtigen Elemente für die Maus zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="7ccf3-120">Allerdings kann der geklickt während der Animation.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="7ccf3-121">Die `<EnableAction>` Element dieser kümmern.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="7ccf3-122">Festlegen von `Enabled="false"` deaktiviert die Schaltfläche mit den im Rahmen der Animation.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="7ccf3-123">Da es mehrere einzelne Animationen (deaktivieren die Schaltfläche und die tatsächliche Animationen), verwenden die `<Parallel>` Element ist erforderlich, um Kleben Sie zusammen in einem einzelnen Animationen.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="7ccf3-124">Hier wird das vollständige Markup für `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="7ccf3-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="7ccf3-125">Es wäre auch möglich, auf die Schaltfläche zum nach der Animation, verwenden das folgende XML-Element am Ende der Liste erneut zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="7ccf3-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="7ccf3-126">Jedoch im vorliegenden Szenario nutzlos seit der Schaltfläche wäre dies ausgeblendet und nicht am Ende der Animation sichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="7ccf3-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="7ccf3-127">[![Die Schaltfläche ist deaktiviert, sobald die Animation ausgeführt wird.](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7ccf3-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="7ccf3-128">Die Schaltfläche ist deaktiviert, sobald die Animation ausgeführt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7ccf3-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7ccf3-129">[Zurück](animating-in-response-to-user-interaction-cs.md)
[Weiter](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7ccf3-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>