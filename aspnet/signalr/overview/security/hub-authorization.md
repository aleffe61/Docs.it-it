---
uid: signalr/overview/security/hub-authorization
title: Autenticazione e autorizzazione per gli hub SignalR | Documenti Microsoft
author: pfletcher
description: In questo argomento viene descritto come limitare gli utenti o i ruoli possono accedere i metodi dell'hub. Versioni del software utilizzato in questo argomento ve di Visual Studio 2013 .NET 4.5 SignalR...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: f1538c933ff9e8e680d70ce1e63d24b189be47e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="00621-104">Autenticazione e autorizzazione per gli hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="00621-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="00621-105">da [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="00621-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="00621-106">In questo argomento viene descritto come limitare gli utenti o i ruoli possono accedere i metodi dell'hub.</span><span class="sxs-lookup"><span data-stu-id="00621-106">This topic describes how to restrict which users or roles can access hub methods.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="00621-107">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="00621-107">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="00621-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="00621-108">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="00621-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="00621-109">.NET 4.5</span></span>
> - <span data-ttu-id="00621-110">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="00621-110">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="00621-111">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="00621-111">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="00621-112">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="00621-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="00621-113">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="00621-113">Questions and comments</span></span>
> 
> <span data-ttu-id="00621-114">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="00621-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="00621-115">In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="00621-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="00621-116">Panoramica</span><span class="sxs-lookup"><span data-stu-id="00621-116">Overview</span></span>

<span data-ttu-id="00621-117">Di seguito sono elencate le diverse sezioni di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="00621-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="00621-118">Autorizzare l'attributo</span><span class="sxs-lookup"><span data-stu-id="00621-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="00621-119">Richiedere l'autenticazione per tutti gli hub</span><span class="sxs-lookup"><span data-stu-id="00621-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="00621-120">Autorizzazione personalizzata</span><span class="sxs-lookup"><span data-stu-id="00621-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="00621-121">Passare le informazioni di autenticazione client</span><span class="sxs-lookup"><span data-stu-id="00621-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="00621-122">Opzioni di autenticazione per i client .NET</span><span class="sxs-lookup"><span data-stu-id="00621-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="00621-123">Cookie di autenticazione basata su form</span><span class="sxs-lookup"><span data-stu-id="00621-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="00621-124">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="00621-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="00621-125">Intestazione di connessione</span><span class="sxs-lookup"><span data-stu-id="00621-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="00621-126">Certificato</span><span class="sxs-lookup"><span data-stu-id="00621-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="00621-127">Autorizzare l'attributo</span><span class="sxs-lookup"><span data-stu-id="00621-127">Authorize attribute</span></span>

<span data-ttu-id="00621-128">SignalR fornisce il [Authorize](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attributo per specificare quali utenti o i ruoli hanno accesso a un hub o un metodo.</span><span class="sxs-lookup"><span data-stu-id="00621-128">SignalR provides the [Authorize](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="00621-129">Questo attributo si trova nel `Microsoft.AspNet.SignalR` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="00621-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="00621-130">Si applica il `Authorize` attributo a un hub o metodi particolari in un hub.</span><span class="sxs-lookup"><span data-stu-id="00621-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="00621-131">Quando si applica il `Authorize` attributo a una classe di hub, i requisiti di autorizzazione specificato viene applicato a tutti i metodi dell'hub.</span><span class="sxs-lookup"><span data-stu-id="00621-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="00621-132">In questo argomento vengono forniti esempi dei diversi tipi di requisiti di autorizzazione che è possibile applicare.</span><span class="sxs-lookup"><span data-stu-id="00621-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="00621-133">Senza il `Authorize` attributo, un oggetto connesso client può accedere a qualsiasi metodo pubblico dell'hub.</span><span class="sxs-lookup"><span data-stu-id="00621-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="00621-134">Se è stato definito un ruolo denominato "Admin" nell'applicazione web, è possibile specificare che solo gli utenti in tale ruolo possono accedere a un hub con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="00621-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="00621-135">In alternativa, è possibile specificare che un hub contenga un metodo disponibile per tutti gli utenti e un secondo metodo è disponibile solo per gli utenti autenticati, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="00621-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="00621-136">Negli esempi seguenti scenari di autorizzazione diversi:</span><span class="sxs-lookup"><span data-stu-id="00621-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="00621-137">`[Authorize]`: solo gli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="00621-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="00621-138">`[Authorize(Roles = "Admin,Manager")]`: solo gli utenti nei ruoli specificati autenticati</span><span class="sxs-lookup"><span data-stu-id="00621-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="00621-139">`[Authorize(Users = "user1,user2")]`: solo gli utenti con i nomi utente specificati autenticati</span><span class="sxs-lookup"><span data-stu-id="00621-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="00621-140">`[Authorize(RequireOutgoing=false)]`: solo gli utenti autenticati possono richiamare l'hub, ma le chiamate dal server ai client non sono limitate per l'autorizzazione, ad esempio, quando solo determinati utenti possono inviare un messaggio ma non tutti gli altri utenti possono ricevere il messaggio.</span><span class="sxs-lookup"><span data-stu-id="00621-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="00621-141">La proprietà RequireOutgoing può essere applicata solo all'intero hub, non sui metodi di persone all'interno dell'hub.</span><span class="sxs-lookup"><span data-stu-id="00621-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="00621-142">Quando RequireOutgoing non è impostata su false, solo gli utenti che soddisfano il requisito di autorizzazione vengono chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="00621-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="00621-143">Richiedere l'autenticazione per tutti gli hub</span><span class="sxs-lookup"><span data-stu-id="00621-143">Require authentication for all hubs</span></span>

<span data-ttu-id="00621-144">È possibile richiedere l'autenticazione per tutti gli hub e i metodi dell'hub nell'applicazione chiamando il [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) metodo all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="00621-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="00621-145">È possibile utilizzare questo metodo quando si dispone di più hub e imporre un requisito di autenticazione per tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="00621-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="00621-146">Con questo metodo, è possibile specificare i requisiti per l'autorizzazione in uscita, un utente o ruolo.</span><span class="sxs-lookup"><span data-stu-id="00621-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="00621-147">È possibile specificare solo che l'accesso ai metodi hub è limitato agli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="00621-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="00621-148">Tuttavia, è comunque possibile applicare l'attributo di Authorize hub o i metodi per specificare i requisiti aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="00621-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="00621-149">Qualsiasi requisito che è specificare un attributo viene aggiunto per il requisito di autenticazione di base.</span><span class="sxs-lookup"><span data-stu-id="00621-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="00621-150">Nell'esempio seguente viene illustrato un file di avvio che consente di limitare tutti i metodi dell'hub per gli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="00621-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="00621-151">Se si chiama il `RequireAuthentication()` metodo dopo l'elaborazione di una richiesta SignalR, SignalR genererà un `InvalidOperationException` eccezione.</span><span class="sxs-lookup"><span data-stu-id="00621-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="00621-152">SignalR genera questa eccezione, poiché è possibile aggiungere un modulo a HubPipeline dopo che la pipeline è stata richiamata.</span><span class="sxs-lookup"><span data-stu-id="00621-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="00621-153">Nell'esempio precedente viene chiamata la `RequireAuthentication` metodo nel `Configuration` metodo che viene eseguito una volta prima di gestire la prima richiesta.</span><span class="sxs-lookup"><span data-stu-id="00621-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="00621-154">Autorizzazione personalizzata</span><span class="sxs-lookup"><span data-stu-id="00621-154">Customized authorization</span></span>

<span data-ttu-id="00621-155">Se si desidera personalizzare la modalità di determinazione di autorizzazione, è possibile creare una classe che deriva da `AuthorizeAttribute` ed eseguire l'override di [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) metodo.</span><span class="sxs-lookup"><span data-stu-id="00621-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="00621-156">Per ogni richiesta, SignalR richiama questo metodo per determinare se l'utente è autorizzato a completare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="00621-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="00621-157">Nel metodo sottoposto a override, fornire la logica necessaria per lo scenario di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="00621-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="00621-158">Nell'esempio seguente viene illustrato come applicare l'autorizzazione tramite l'identità basata sulle attestazioni.</span><span class="sxs-lookup"><span data-stu-id="00621-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="00621-159">Passare le informazioni di autenticazione client</span><span class="sxs-lookup"><span data-stu-id="00621-159">Pass authentication information to clients</span></span>

<span data-ttu-id="00621-160">Potrebbe essere necessario utilizzare le informazioni di autenticazione nel codice che viene eseguito sul client.</span><span class="sxs-lookup"><span data-stu-id="00621-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="00621-161">Passare le informazioni necessarie quando si chiama i metodi nel client.</span><span class="sxs-lookup"><span data-stu-id="00621-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="00621-162">Ad esempio, un metodo di applicazione di chat può passare come parametro il nome utente della persona che la registrazione di un messaggio, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="00621-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="00621-163">In alternativa, è possibile creare un oggetto per rappresentare le informazioni di autenticazione e passare tale oggetto come un parametro, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="00621-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="00621-164">Non passare mai id connessione del client per altri client, come un utente malintenzionato potrebbe usare per simulare una richiesta dal client.</span><span class="sxs-lookup"><span data-stu-id="00621-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="00621-165">Opzioni di autenticazione per i client .NET</span><span class="sxs-lookup"><span data-stu-id="00621-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="00621-166">Quando si dispone di un client .NET, ad esempio un'applicazione console, che interagisce con un hub di cui è limitato agli utenti autenticati, è possibile passare le credenziali di autenticazione in un cookie, l'intestazione di connessione o un certificato.</span><span class="sxs-lookup"><span data-stu-id="00621-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="00621-167">Negli esempi di questa sezione viene illustrato come utilizzare tali metodi diversi per autenticare un utente.</span><span class="sxs-lookup"><span data-stu-id="00621-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="00621-168">Non sono completamente funzionali App SignalR.</span><span class="sxs-lookup"><span data-stu-id="00621-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="00621-169">Per ulteriori informazioni sui client .NET con SignalR, vedere [hub API Guida - Client .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="00621-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="00621-170">Cookie</span><span class="sxs-lookup"><span data-stu-id="00621-170">Cookie</span></span>

<span data-ttu-id="00621-171">Quando il client .NET interagisce con un hub che utilizza l'autenticazione basata su form ASP.NET, è necessario impostare manualmente il cookie di autenticazione per la connessione.</span><span class="sxs-lookup"><span data-stu-id="00621-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="00621-172">Il cookie viene aggiunto il `CookieContainer` proprietà il [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) oggetto.</span><span class="sxs-lookup"><span data-stu-id="00621-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="00621-173">Nell'esempio seguente mostra un'app console che recupera un cookie di autenticazione da una pagina web e aggiunge tale cookie per la connessione.</span><span class="sxs-lookup"><span data-stu-id="00621-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="00621-174">L'applicazione console invia le credenziali da **www.contoso.com/RemoteLogin** che potrebbe fare riferimento a una pagina vuota che contiene il file code-behind seguente.</span><span class="sxs-lookup"><span data-stu-id="00621-174">The console app posts the credentials to **www.contoso.com/RemoteLogin** which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="00621-175">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="00621-175">Windows authentication</span></span>

<span data-ttu-id="00621-176">Quando si utilizza l'autenticazione di Windows, è possibile passare le credenziali dell'utente corrente utilizzando il [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) proprietà.</span><span class="sxs-lookup"><span data-stu-id="00621-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="00621-177">Le credenziali per la connessione viene impostato il valore di DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="00621-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="00621-178">Intestazione di connessione</span><span class="sxs-lookup"><span data-stu-id="00621-178">Connection header</span></span>

<span data-ttu-id="00621-179">Se l'applicazione non utilizza i cookie, è possibile passare le informazioni utente nell'intestazione di connessione.</span><span class="sxs-lookup"><span data-stu-id="00621-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="00621-180">Ad esempio, è possibile passare un token nell'intestazione di connessione.</span><span class="sxs-lookup"><span data-stu-id="00621-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="00621-181">Quindi, nell'hub, verificare il token dell'utente.</span><span class="sxs-lookup"><span data-stu-id="00621-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="00621-182">Certificato</span><span class="sxs-lookup"><span data-stu-id="00621-182">Certificate</span></span>

<span data-ttu-id="00621-183">È possibile passare un certificato client per verificare che l'utente.</span><span class="sxs-lookup"><span data-stu-id="00621-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="00621-184">Aggiungere il certificato quando si crea la connessione.</span><span class="sxs-lookup"><span data-stu-id="00621-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="00621-185">Nell'esempio seguente viene illustrato come aggiungere un certificato client per la connessione; non è visibile l'applicazione console completa.</span><span class="sxs-lookup"><span data-stu-id="00621-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="00621-186">Usa il [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) classe che fornisce diversi modi per creare il certificato.</span><span class="sxs-lookup"><span data-stu-id="00621-186">It uses the [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]