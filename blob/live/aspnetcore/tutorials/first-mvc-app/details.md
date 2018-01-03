---
title: Analisi dei dettagli e dei metodi di eliminazione
author: rick-anderson
description: Metodo e vista del controller Details in un'app ASP.NET Core MVC di base.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 43394106c9074f9487e1065a37a88eb017833bae
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="8c5a3-104">Analisi dei dettagli e dei metodi di eliminazione</span><span class="sxs-lookup"><span data-stu-id="8c5a3-104">Examining the Details and Delete methods</span></span>

<span data-ttu-id="8c5a3-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8c5a3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8c5a3-106">Aprire il controller di film ed esaminare il metodo `Details`:</span><span class="sxs-lookup"><span data-stu-id="8c5a3-106">Open the Movie controller and examine the `Details` method:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

<span data-ttu-id="8c5a3-107">Il motore di scaffolding MVC che ha creato questo metodo di azione aggiunge un commento che mostra una richiesta HTTP che richiama il metodo.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-107">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="8c5a3-108">In questo caso si tratta di una richiesta GET con tre segmenti di URL, il controller `Movies`, il metodo `Details` e un valore `id`.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-108">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="8c5a3-109">Tenere presente che questi segmenti vengono definiti nel file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-109">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="8c5a3-110">Entity Framework semplifica la ricerca di dati usando il metodo `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-110">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="8c5a3-111">Un'importante funzionalità di sicurezza integrata nel metodo è che il codice verifica che il metodo di ricerca abbia trovato un film prima di tentare di eseguire una qualsiasi operazione su di esso.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-111">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="8c5a3-112">Ad esempio, un hacker potrebbe introdurre errori nel sito modificando l'URL creato dai collegamenti di `http://localhost:xxxx/Movies/Details/1` in qualcosa di simile a `http://localhost:xxxx/Movies/Details/12345` o usando altri valori che non rappresentano un film effettivo.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-112">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="8c5a3-113">Se non è stato cercato un film con valore null, l'app genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-113">If you did not check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="8c5a3-114">Esaminare i metodi `Delete` e `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-114">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

<span data-ttu-id="8c5a3-115">Si noti che il metodo `HTTP GET Delete` non elimina il film specificato, ma restituisce una vista del film in cui è possibile inviare (HttpPost) la richiesta di eliminazione.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-115">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="8c5a3-116">L'esecuzione di un'operazione di eliminazione in risposta a una richiesta GET (o l'esecuzione di un'operazione di modifica, di creazione o di qualsiasi altra azione che modifica i dati) introduce un problema di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-116">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="8c5a3-117">Il metodo `[HttpPost]` che elimina i dati è denominato `DeleteConfirmed` per fornire al metodo HTTP POST un nome o una firma univoca.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-117">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="8c5a3-118">Le firme dei due metodi sono illustrate di seguito:</span><span class="sxs-lookup"><span data-stu-id="8c5a3-118">The two method signatures are shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="8c5a3-119">Il Common Language Runtime (CLR) richiede metodi in rapporto di overload per disporre di una firma di parametro univoca, ovvero lo stesso nome di metodo ma un elenco diverso di parametri.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-119">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="8c5a3-120">Tuttavia, qui è necessario usare due `Delete` metodi: uno per GET e uno per POST, entrambi con la stessa firma di parametro.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-120">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="8c5a3-121">Entrambi devono accettare un singolo intero come parametro.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-121">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="8c5a3-122">Per risolvere questo problema esistono due approcci. Una possibilità è assegnare nomi diversi ai metodi.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-122">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="8c5a3-123">Questa operazione è stata eseguita dal meccanismo di scaffolding nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-123">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="8c5a3-124">Tuttavia in questo modo si introduce un piccolo problema: ASP.NET esegue il mapping dei segmenti di un URL ai metodi di azione in base al nome e, se si rinomina un metodo, generalmente il routing non è in grado di trovare questo metodo.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-124">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="8c5a3-125">La soluzione è mostrata nell'esempio e consiste nell'aggiungere l'attributo `ActionName("Delete")` al metodo `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-125">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="8c5a3-126">Questo attributo esegue il mapping per il sistema di routing in modo che un URL che includa /Delete/ per una richiesta POST troverà il metodo `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-126">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="8c5a3-127">Un'altra alternativa comune per i metodi con nomi e firme identici consiste nel modificare artificialmente la firma del metodo POST in modo da includere un parametro aggiuntivo (non usato).</span><span class="sxs-lookup"><span data-stu-id="8c5a3-127">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="8c5a3-128">Questa azione è stata eseguita in un post precedente quando è stato aggiunto il parametro `notUsed`.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-128">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="8c5a3-129">È possibile eseguire la stessa operazione qui per il metodo `[HttpPost] Delete`:</span><span class="sxs-lookup"><span data-stu-id="8c5a3-129">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="8c5a3-130">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="8c5a3-130">Publish to Azure</span></span>

<span data-ttu-id="8c5a3-131">Per istruzioni su come pubblicare l'app in Azure usando Visual Studio, vedere [Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="8c5a3-131">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="8c5a3-132">L'app può essere pubblicata anche dalla [riga di comando](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="8c5a3-132">The app can also be published from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="8c5a3-133">Grazie aver completato questa introduzione ad ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-133">Thanks for completing this introduction to ASP.NET Core MVC.</span></span> <span data-ttu-id="8c5a3-134">Si apprezza l'invio di commenti.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-134">We appreciate any comments you leave.</span></span> <span data-ttu-id="8c5a3-135">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) (Introduzione a MVC ed Entity Framework Core) è un ottimo completamento per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="8c5a3-135">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="8c5a3-136">Precedente</span><span class="sxs-lookup"><span data-stu-id="8c5a3-136">Previous</span></span>](validation.md)