---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Impedire attacchi di tipo Cross-Site Request Forgery (CSRF) in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: Viene descritto come implementare le misure di anti-CSRF in ASP.NET Web API e l'attacco forgery (CSRF) richiesta tra siti.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 1cd03f3b396cc2ece1d8dbe6820f6277c02d8e62
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a><span data-ttu-id="51074-103">Impedire attacchi di tipo Cross-Site Request Forgery (CSRF) in ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="51074-103">Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET Web API</span></span>
====================
<span data-ttu-id="51074-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="51074-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="51074-105">Cross-Site richiesta Forgery (CSRF) è un attacco in cui un sito dannoso invia una richiesta a un sito vulnerabile in cui l'utente è connesso</span><span class="sxs-lookup"><span data-stu-id="51074-105">Cross-Site Request Forgery (CSRF) is an attack where a malicious site sends a request to a vulnerable site where the user is currently logged in</span></span>

<span data-ttu-id="51074-106">Di seguito è riportato un esempio di un attacco di tipo CSRF:</span><span class="sxs-lookup"><span data-stu-id="51074-106">Here is an example of a CSRF attack:</span></span>

1. <span data-ttu-id="51074-107">Un utente accede www.example.com, con autenticazione basata su form.</span><span class="sxs-lookup"><span data-stu-id="51074-107">A user logs into www.example.com, using forms authentication.</span></span>
2. <span data-ttu-id="51074-108">Il server autentica l'utente.</span><span class="sxs-lookup"><span data-stu-id="51074-108">The server authenticates the user.</span></span> <span data-ttu-id="51074-109">La risposta dal server include un cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="51074-109">The response from the server includes an authentication cookie.</span></span>
3. <span data-ttu-id="51074-110">Senza la disconnessione, l'utente visita un sito web.</span><span class="sxs-lookup"><span data-stu-id="51074-110">Without logging out, the user visits a malicious web site.</span></span> <span data-ttu-id="51074-111">Questo sito contiene il modulo HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="51074-111">This malicious site contains the following HTML form:</span></span> 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    <span data-ttu-id="51074-112">Si noti che le azioni form invia al sito vulnerabile, non per il sito dannoso.</span><span class="sxs-lookup"><span data-stu-id="51074-112">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="51074-113">Questa è la parte "cross-site" di CSRF.</span><span class="sxs-lookup"><span data-stu-id="51074-113">This is the "cross-site" part of CSRF.</span></span>
4. <span data-ttu-id="51074-114">L'utente fa clic sul pulsante Invia.</span><span class="sxs-lookup"><span data-stu-id="51074-114">The user clicks the submit button.</span></span> <span data-ttu-id="51074-115">Il visualizzatore include il cookie di autenticazione con la richiesta.</span><span class="sxs-lookup"><span data-stu-id="51074-115">The browser includes the authentication cookie with the request.</span></span>
5. <span data-ttu-id="51074-116">La richiesta viene eseguita nel server con il contesto di autenticazione dell'utente e può eseguire qualsiasi operazione che è consentito un utente autenticato.</span><span class="sxs-lookup"><span data-stu-id="51074-116">The request runs on the server with the user's authentication context, and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="51074-117">Anche se in questo esempio richiede all'utente di fare clic sul pulsante del form, la pagina può solo eseguire facilmente uno script che invia automaticamente il form.</span><span class="sxs-lookup"><span data-stu-id="51074-117">Although this example requires the user to click the form button, the malicious page could just as easily run a script that submits the form automatically.</span></span> <span data-ttu-id="51074-118">Inoltre, tramite SSL non impedire un attacco di tipo CSRF poiché il sito dannoso può inviare una richiesta "https://".</span><span class="sxs-lookup"><span data-stu-id="51074-118">Moreover, using SSL does not prevent a CSRF attack, because the malicious site can send an "https://" request.</span></span>

<span data-ttu-id="51074-119">In genere, gli attacchi CSRF sono possibili contro i siti web che usano i cookie per l'autenticazione, perché il browser invia tutti i cookie pertinenti al sito web di destinazione.</span><span class="sxs-lookup"><span data-stu-id="51074-119">Typically, CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="51074-120">Tuttavia, gli attacchi CSRF non sono limitati per sfruttare i cookie.</span><span class="sxs-lookup"><span data-stu-id="51074-120">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="51074-121">Ad esempio, l'autenticazione di base e classificata sono anche vulnerabili.</span><span class="sxs-lookup"><span data-stu-id="51074-121">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="51074-122">Dopo un utente accede con l'autenticazione di base o Digest.</span><span class="sxs-lookup"><span data-stu-id="51074-122">After a user logs in with Basic or Digest authentication.</span></span> <span data-ttu-id="51074-123">il browser invia automaticamente le credenziali, fino al termine della sessione.</span><span class="sxs-lookup"><span data-stu-id="51074-123">the browser automatically sends the credentials until the session ends.</span></span>

## <a name="anti-forgery-tokens"></a><span data-ttu-id="51074-124">Token antifalsificazione</span><span class="sxs-lookup"><span data-stu-id="51074-124">Anti-Forgery Tokens</span></span>

<span data-ttu-id="51074-125">Per impedire gli attacchi CSRF, MVC ASP.NET utilizza i token antifalsificazione, detti anche *richiedere token di verifica*.</span><span class="sxs-lookup"><span data-stu-id="51074-125">To help prevent CSRF attacks, ASP.NET MVC uses anti-forgery tokens, also called *request verification tokens*.</span></span>

1. <span data-ttu-id="51074-126">Il client richiede una pagina HTML contenente un modulo.</span><span class="sxs-lookup"><span data-stu-id="51074-126">The client requests an HTML page that contains a form.</span></span>
2. <span data-ttu-id="51074-127">Il server include due token nella risposta.</span><span class="sxs-lookup"><span data-stu-id="51074-127">The server includes two tokens in the response.</span></span> <span data-ttu-id="51074-128">Un token viene inviato come cookie.</span><span class="sxs-lookup"><span data-stu-id="51074-128">One token is sent as a cookie.</span></span> <span data-ttu-id="51074-129">L'altra viene inserita in un campo modulo nascosto.</span><span class="sxs-lookup"><span data-stu-id="51074-129">The other is placed in a hidden form field.</span></span> <span data-ttu-id="51074-130">I token vengono generati in modo casuale in modo da un avversario Impossibile dedurre i valori.</span><span class="sxs-lookup"><span data-stu-id="51074-130">The tokens are generated randomly so that an adversary cannot guess the values.</span></span>
3. <span data-ttu-id="51074-131">Quando il client invia il form, è necessario inviare entrambi i token al server.</span><span class="sxs-lookup"><span data-stu-id="51074-131">When the client submits the form, it must send both tokens back to the server.</span></span> <span data-ttu-id="51074-132">Il client invia il token del cookie in un cookie e invia il token del form all'interno dei dati del form.</span><span class="sxs-lookup"><span data-stu-id="51074-132">The client sends the cookie token as a cookie, and it sends the form token inside the form data.</span></span> <span data-ttu-id="51074-133">(Un client browser automaticamente avviene quando l'utente invia il form.)</span><span class="sxs-lookup"><span data-stu-id="51074-133">(A browser client automatically does this when the user submits the form.)</span></span>
4. <span data-ttu-id="51074-134">Se una richiesta non include entrambi i token, il server non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="51074-134">If a request does not include both tokens, the server disallows the request.</span></span>

<span data-ttu-id="51074-135">Di seguito è riportato un esempio di un form HTML con un token del form nascosto:</span><span class="sxs-lookup"><span data-stu-id="51074-135">Here is an example of an HTML form with a hidden form token:</span></span>

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

<span data-ttu-id="51074-136">I token antifalsificazione funzionano perché la pagina non è possibile leggere i token dell'utente, a causa di criteri di origine stesso.</span><span class="sxs-lookup"><span data-stu-id="51074-136">Anti-forgery tokens work because the malicious page cannot read the user's tokens, due to same-origin policies.</span></span> <span data-ttu-id="51074-137">([Criteri stessa origine](http://www.w3.org/Security/wiki/Same_Origin_Policy) impedire che i documenti ospitati in due diversi siti di accedere a contenuto di un'altra.</span><span class="sxs-lookup"><span data-stu-id="51074-137">([Same-origin policies](http://www.w3.org/Security/wiki/Same_Origin_Policy) prevent documents hosted on two different sites from accessing each other's content.</span></span> <span data-ttu-id="51074-138">Nell'esempio precedente, la pagina può inviare richieste all'example.com, ma che non è possibile leggere la risposta).</span><span class="sxs-lookup"><span data-stu-id="51074-138">So in the earlier example, the malicious page can send requests to example.com, but it cannot read the response.)</span></span>

<span data-ttu-id="51074-139">Per impedire attacchi di tipo CSRF, utilizzare i token antifalsificazione con qualsiasi protocollo di autenticazione in cui il browser invia automaticamente le credenziali dopo che l'utente accede.</span><span class="sxs-lookup"><span data-stu-id="51074-139">To prevent CSRF attacks, use anti-forgery tokens with any authentication protocol where the browser silently sends credentials after the user logs in.</span></span> <span data-ttu-id="51074-140">Questo include i protocolli di autenticazione basato su cookie, ad esempio autenticazione basata su form, nonché i protocolli, ad esempio autenticazione di base e Digest.</span><span class="sxs-lookup"><span data-stu-id="51074-140">This includes cookie-based authentication protocols, such as forms authentication, as well as protocols such as Basic and Digest authentication.</span></span>

<span data-ttu-id="51074-141">È necessario che i token antifalsificazione per qualsiasi metodo nonsafe (POST, PUT, DELETE).</span><span class="sxs-lookup"><span data-stu-id="51074-141">You should require anti-forgery tokens for any nonsafe methods (POST, PUT, DELETE).</span></span> <span data-ttu-id="51074-142">Inoltre, assicurarsi che provvisoria metodi (GET, HEAD) non dispone di tutti gli effetti collaterali.</span><span class="sxs-lookup"><span data-stu-id="51074-142">Also, make sure that safe methods (GET, HEAD) do not have any side effects.</span></span> <span data-ttu-id="51074-143">Inoltre, se si abilita il supporto di domini, ad esempio CORS o JSONP, sicuri anche metodi quali GET sono potenzialmente vulnerabili ad attacchi CSRF, consentendo all'utente malintenzionato di leggere i dati potenzialmente riservati.</span><span class="sxs-lookup"><span data-stu-id="51074-143">Moreover, if you enable cross-domain support, such as CORS or JSONP, then even safe methods like GET are potentially vulnerable to CSRF attacks, allowing the attacker to read potentially sensitive data.</span></span>

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a><span data-ttu-id="51074-144">Token antifalsificazione in ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="51074-144">Anti-Forgery Tokens in ASP.NET MVC</span></span>

<span data-ttu-id="51074-145">Per aggiungere i token antifalsificazione a una pagina Razor, utilizzare il **HtmlHelper.AntiForgeryToken** metodo di supporto:</span><span class="sxs-lookup"><span data-stu-id="51074-145">To add the anti-forgery tokens to a Razor page, use the **HtmlHelper.AntiForgeryToken** helper method:</span></span>

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

<span data-ttu-id="51074-146">Questo metodo aggiunge il campo nascosto del modulo e imposta inoltre il token del cookie.</span><span class="sxs-lookup"><span data-stu-id="51074-146">This method adds the hidden form field and also sets the cookie token.</span></span>

## <a name="anti-csrf-and-ajax"></a><span data-ttu-id="51074-147">Anti-CSRF e AJAX</span><span class="sxs-lookup"><span data-stu-id="51074-147">Anti-CSRF and AJAX</span></span>

<span data-ttu-id="51074-148">Il token del form può essere un problema per le richieste AJAX, perché una richiesta AJAX potrebbe inviare i dati JSON, non i dati di form HTML.</span><span class="sxs-lookup"><span data-stu-id="51074-148">The form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="51074-149">Una soluzione consiste nell'inviare i token in un'intestazione HTTP personalizzata.</span><span class="sxs-lookup"><span data-stu-id="51074-149">One solution is to send the tokens in a custom HTTP header.</span></span> <span data-ttu-id="51074-150">Il codice seguente usa la sintassi Razor per generare i token e quindi aggiunge i token a una richiesta AJAX.</span><span class="sxs-lookup"><span data-stu-id="51074-150">The following code uses Razor syntax to generate the tokens, and then adds the tokens to an AJAX request.</span></span> <span data-ttu-id="51074-151">I token vengono generati nel server chiamando **AntiForgery.GetTokens**.</span><span class="sxs-lookup"><span data-stu-id="51074-151">The tokens are generated at the server by calling **AntiForgery.GetTokens**.</span></span>

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

<span data-ttu-id="51074-152">Quando si elabora la richiesta, estrarre il token dall'intestazione di richiesta.</span><span class="sxs-lookup"><span data-stu-id="51074-152">When you process the request, extract the tokens from the request header.</span></span> <span data-ttu-id="51074-153">Chiamare quindi il **AntiForgery.Validate** metodo per convalidare i token.</span><span class="sxs-lookup"><span data-stu-id="51074-153">Then call the **AntiForgery.Validate** method to validate the tokens.</span></span> <span data-ttu-id="51074-154">Il **convalida** metodo genera un'eccezione se il token non sono validi.</span><span class="sxs-lookup"><span data-stu-id="51074-154">The **Validate** method throws an exception if the tokens are not valid.</span></span>

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]