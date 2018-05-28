---
title: Introduzione a Swashbuckle e ad ASP.NET Core
author: zuckerthoben
description: Informazioni su come aggiungere Swashbuckle al progetto dell'API Web ASP.NET Core per integrare l'interfaccia utente di Swagger.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 0eb9aa12419cc09899af6bc85dd32a85687dab62
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2018
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="d6ba7-103">Introduzione a Swashbuckle e ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6ba7-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="d6ba7-104">Di [Shayne Boyer](https://twitter.com/spboyer) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="d6ba7-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="d6ba7-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6ba7-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="d6ba7-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6ba7-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="d6ba7-107">Esistono tre componenti principali di Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-107">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="d6ba7-108">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): un modello a oggetti Swagger e il middleware per esporre oggetti `SwaggerDocument` come endpoint JSON.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-108">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="d6ba7-109">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): un generatore Swagger che compila oggetti `SwaggerDocument` direttamente da route, controller e modelli.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-109">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="d6ba7-110">È in genere combinato con il middleware di endpoint Swagger per esporre automaticamente il JSON Swagger.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-110">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="d6ba7-111">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): una versione incorporata dello strumento interfaccia utente di Swagger,</span><span class="sxs-lookup"><span data-stu-id="d6ba7-111">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="d6ba7-112">che interpreta il JSON di Swagger per descrivere in modo completo e personalizzabile la funzionalità dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-112">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="d6ba7-113">Include test harness predefiniti per i metodi pubblici.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-113">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="d6ba7-114">Installazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="d6ba7-114">Package installation</span></span>

<span data-ttu-id="d6ba7-115">È possibile aggiungere Swashbuckle con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-115">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6ba7-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6ba7-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d6ba7-117">Dalla finestra **Console di Gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-117">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="d6ba7-118">Passare a **Vista** > **Altre finestre** > **Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="d6ba7-118">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="d6ba7-119">Passare alla directory che contiene il file *TodoApi.csproj*</span><span class="sxs-lookup"><span data-stu-id="d6ba7-119">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="d6ba7-120">Eseguire il seguente comando:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-120">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="d6ba7-121">Dalla finestra di dialogo **Gestisci pacchetti NuGet**:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-121">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="d6ba7-122">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="d6ba7-122">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="d6ba7-123">Impostare **Origine pacchetto** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="d6ba7-123">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="d6ba7-124">Immettere "Swashbuckle.AspNetCore" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="d6ba7-124">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="d6ba7-125">Selezionare il pacchetto "Swashbuckle.AspNetCore" dalla scheda **Sfoglia** e fare clic su **Installa**</span><span class="sxs-lookup"><span data-stu-id="d6ba7-125">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d6ba7-126">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="d6ba7-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d6ba7-127">Fare clic su con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione) > **Aggiungi pacchetti**</span><span class="sxs-lookup"><span data-stu-id="d6ba7-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="d6ba7-128">Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="d6ba7-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="d6ba7-129">Immettere "Swashbuckle.AspNetCore" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="d6ba7-129">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="d6ba7-130">Selezionare il pacchetto "Swashbuckle.AspNetCore" dal riquadro dei risultati e fare clic su **Aggiungi pacchetto**</span><span class="sxs-lookup"><span data-stu-id="d6ba7-130">Select the "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d6ba7-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6ba7-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d6ba7-132">Eseguire il comando seguente da **Terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-132">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d6ba7-133">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="d6ba7-133">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d6ba7-134">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-134">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="d6ba7-135">Aggiungere e configurare il middleware di Swagger</span><span class="sxs-lookup"><span data-stu-id="d6ba7-135">Add and configure Swagger middleware</span></span>

<span data-ttu-id="d6ba7-136">Aggiungere il generatore di Swagger alla raccolta di servizi nel metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-136">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="d6ba7-137">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]</span><span class="sxs-lookup"><span data-stu-id="d6ba7-137">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="d6ba7-138">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]</span><span class="sxs-lookup"><span data-stu-id="d6ba7-138">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]</span></span>
::: moniker-end

<span data-ttu-id="d6ba7-139">Importare gli spazi dei nomi seguenti per usare la classe `Info`:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-139">Import the following namespace to use the `Info` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="d6ba7-140">Nel metodo `Startup.Configure` abilitare il middleware per la gestione del documento JSON generato e l'interfaccia utente di Swagger:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-140">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="d6ba7-141">Avviare l'app e passare a `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-141">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="d6ba7-142">Il documento generato che descrive gli endpoint viene illustrato in [Specifica Swagger (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="d6ba7-142">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="d6ba7-143">L'interfaccia utente di Swagger è disponibile in `http://localhost:<port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-143">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="d6ba7-144">Esplorare l'API tramite l'interfaccia utente di Swagger e incorporarla in altri programmi.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-144">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="d6ba7-145">Per gestire l'interfaccia utente di Swagger nella radice dell'app (`http://localhost:<port>/`), impostare la proprietà `RoutePrefix` su una stringa vuota:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-145">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize--extend"></a><span data-ttu-id="d6ba7-146">Personalizzare ed estendere</span><span class="sxs-lookup"><span data-stu-id="d6ba7-146">Customize & extend</span></span>

<span data-ttu-id="d6ba7-147">Swagger offre opzioni per documentare il modello a oggetti e personalizzare l'interfaccia utente in modo che corrisponda al tema.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-147">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="d6ba7-148">Informazioni e descrizione API</span><span class="sxs-lookup"><span data-stu-id="d6ba7-148">API info and description</span></span>

<span data-ttu-id="d6ba7-149">L'azione di configurazione passata al metodo `AddSwaggerGen` aggiunge informazioni come ad esempio autore, licenza e descrizione:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-149">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="d6ba7-150">L'interfaccia utente di Swagger visualizza le informazioni sulla versione:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-150">The Swagger UI displays the version's information:</span></span>

![Interfaccia utente di Swagger con informazioni sulla versione: descrizione, autore e altri collegamenti](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="d6ba7-152">XML (commenti)</span><span class="sxs-lookup"><span data-stu-id="d6ba7-152">XML comments</span></span>

<span data-ttu-id="d6ba7-153">I commenti XML possono essere abilitati con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-153">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="d6ba7-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6ba7-154">Visual Studio</span></span>](#tab/visual-studio-xml/)

* <span data-ttu-id="d6ba7-155">Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e selezionare **Proprietà**</span><span class="sxs-lookup"><span data-stu-id="d6ba7-155">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="d6ba7-156">Selezionare la casella di controllo **File di documentazione XML** nella sezione **Output** della scheda **Compilazione**</span><span class="sxs-lookup"><span data-stu-id="d6ba7-156">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="d6ba7-157">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="d6ba7-157">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

* <span data-ttu-id="d6ba7-158">Aprire la finestra di dialogo **Opzioni progetto** > **Compila** > **Compilatore**</span><span class="sxs-lookup"><span data-stu-id="d6ba7-158">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="d6ba7-159">Selezionare la casella di controllo **Genera la documentazione XML** nella sezione **Opzioni generali**</span><span class="sxs-lookup"><span data-stu-id="d6ba7-159">Check the **Generate xml documentation** box under the **General Options** section</span></span>

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="d6ba7-160">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6ba7-160">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="d6ba7-161">Aggiungere manualmente il frammento di codice seguente al file *csproj*:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-161">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

---

<span data-ttu-id="d6ba7-162">Abilitando i commenti XML, viene eseguito il debug delle informazioni per membri e tipi pubblici non documentati.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-162">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="d6ba7-163">I membri e tipi non documentati sono indicati nel messaggio di avviso.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-163">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="d6ba7-164">Ad esempio, il messaggio seguente indica una violazione del codice di avviso 1591:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-164">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="d6ba7-165">Eliminare gli avvisi definendo un elenco delimitato da punto e virgola di codici di avviso da ignorare nel file *csproj*:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-165">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="d6ba7-166">Configurare Swagger in modo che usi il file XML generato.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-166">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="d6ba7-167">Per Linux o sistemi operativi diversi da Windows, i percorsi e i nomi di file possono fare distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-167">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="d6ba7-168">Ad esempio, un file *ToDoApi.XML* è valido in Windows, ma non in CentOS.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-168">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="d6ba7-169">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]</span><span class="sxs-lookup"><span data-stu-id="d6ba7-169">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="d6ba7-170">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]</span><span class="sxs-lookup"><span data-stu-id="d6ba7-170">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]</span></span>
::: moniker-end

<span data-ttu-id="d6ba7-171">Nel codice precedente la [reflection](/dotnet/csharp/programming-guide/concepts/reflection) consente di compilare un nome file XML che corrisponde a quello del progetto API Web.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-171">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="d6ba7-172">Questo approccio assicura che il nome file XML generato corrisponda al nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-172">This approach ensures that the generated XML file name matches the project name.</span></span> <span data-ttu-id="d6ba7-173">La proprietà [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) viene usata per costruire un percorso del file XML.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-173">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="d6ba7-174">L'aggiunta a un'azione di commenti con tripla barra migliora l'interfaccia utente di Swagger poiché viene aggiunta la descrizione all'intestazione della sezione.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-174">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="d6ba7-175">Aggiungere un elemento [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) sopra l'azione `Delete`:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-175">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="d6ba7-176">L'interfaccia utente di Swagger visualizza il testo interno dell'elemento `<summary>` del codice precedente:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-176">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Interfaccia utente di Swagger che visualizza il commento XML "Deletes a specific TodoItem."](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="d6ba7-179">L'interfaccia utente è determinata dallo schema JSON generato:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-179">The UI is driven by the generated JSON schema:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="d6ba7-180">Aggiungere un elemento [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) alla documentazione del metodo di azione `Create`.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-180">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="d6ba7-181">In questo modo vengono integrate le informazioni specificate nell'elemento `<summary>` e l'interfaccia utente di Swagger risulta più affidabile.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-181">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="d6ba7-182">Il contenuto dell'elemento `<remarks>` può essere costituito da testo, JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-182">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="d6ba7-183">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span><span class="sxs-lookup"><span data-stu-id="d6ba7-183">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="d6ba7-184">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span><span class="sxs-lookup"><span data-stu-id="d6ba7-184">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]</span></span>
::: moniker-end

<span data-ttu-id="d6ba7-185">Si notino i miglioramenti dell'interfaccia utente con questi commenti aggiuntivi:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-185">Notice the UI enhancements with these additional comments:</span></span>

![Interfaccia utente di Swagger con commenti aggiuntivi visualizzati](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="d6ba7-187">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="d6ba7-187">Data annotations</span></span>

<span data-ttu-id="d6ba7-188">Decorare il modello con gli attributi inclusi nello spazio dei nomi [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) per gestire i componenti dell'interfaccia utente di Swagger.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-188">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="d6ba7-189">Aggiungere l'attributo `[Required]` alla proprietà `Name` della classe `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-189">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="d6ba7-190">La presenza di questo attributo modifica il comportamento dell'interfaccia utente e lo schema JSON sottostante:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-190">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="d6ba7-191">Aggiungere l'attributo `[Produces("application/json")]` al controller API.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-191">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="d6ba7-192">Il suo scopo consiste nel dichiarare che le azioni del controller supportano un tipo di contenuto della risposta di *application/json*:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-192">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="d6ba7-193">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="d6ba7-193">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="d6ba7-194">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="d6ba7-194">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]</span></span>
::: moniker-end

<span data-ttu-id="d6ba7-195">L'elenco a discesa **Response Content Type** (Tipo di contenuto risposta) include questo tipo di contenuto come selezione predefinita per le azioni GET del controller:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-195">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfaccia utente di Swagger con tipo di contenuto di risposta predefinito](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="d6ba7-197">Con l'aumento dell'uso delle annotazioni dei dati nell'API Web, le pagine della Guida dell'API e dell'interfaccia utente diventano più descrittive e utili.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-197">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="d6ba7-198">Descrivere i tipi di risposta</span><span class="sxs-lookup"><span data-stu-id="d6ba7-198">Describe response types</span></span>

<span data-ttu-id="d6ba7-199">La principale preoccupazione degli sviluppatori delle applicazioni è ciò che viene restituito, in particolare i tipi di risposta e i codici di errore, se diversi da quelli standard.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-199">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="d6ba7-200">I tipi di risposta e i codici di errore vengono indicati nei commenti XML e nelle annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-200">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="d6ba7-201">L'azione `Create` restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-201">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="d6ba7-202">Quando il corpo della richiesta inviata è Null, viene restituito un codice di stato HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-202">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="d6ba7-203">Senza la documentazione appropriata nell'interfaccia utente di Swagger, il consumer non riconosce tali risultati previsti.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-203">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="d6ba7-204">Risolvere il problema aggiungendo le righe evidenziate nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-204">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="d6ba7-205">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span><span class="sxs-lookup"><span data-stu-id="d6ba7-205">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="d6ba7-206">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span><span class="sxs-lookup"><span data-stu-id="d6ba7-206">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]</span></span>
::: moniker-end

<span data-ttu-id="d6ba7-207">L'interfaccia utente di Swagger ora documenta chiaramente i codici di risposta HTTP previsti:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-207">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Interfaccia utente di Swagger che visualizza la descrizione della classe risposta POST "Returns the newly created Todo item" e "400 - If the item is null" per il codice di stato e il motivo nei messaggi di risposta](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="d6ba7-209">Personalizzare l'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="d6ba7-209">Customize the UI</span></span>

<span data-ttu-id="d6ba7-210">L'interfaccia utente è funzionale e presentabile,</span><span class="sxs-lookup"><span data-stu-id="d6ba7-210">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="d6ba7-211">ma è necessario che le pagine della documentazione API rappresentino il proprio marchio o tema.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-211">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="d6ba7-212">Per personalizzare i componenti Swashbuckle è necessario aggiungere le risorse per gestire i file statici e quindi compilare la struttura di cartelle per ospitare i file.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-212">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="d6ba7-213">Se si usa .NET Framework o .NET Core 1.x, aggiungere il pacchetto NuGet [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) al progetto:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-213">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="d6ba7-214">Il pacchetto NuGet precedente è già installato se si usano .NET Core 2.x e il [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="d6ba7-214">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="d6ba7-215">Abilitare il middleware dei file statici:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-215">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="d6ba7-216">Acquisire il contenuto della cartella *dist* dall'[archivio dell'interfaccia utente di Swagger di GitHub](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="d6ba7-216">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="d6ba7-217">La cartella contiene le risorse necessarie per la pagina dell'interfaccia utente di Swagger.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-217">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="d6ba7-218">Creare una cartella *wwwroot/swagger/ui* e copiarvi il contenuto della cartella *dist*.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-218">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="d6ba7-219">Creare un file *custom.css* in *wwwroot/swagger/ui*, con il CSS riportato di seguito per personalizzare l'intestazione della pagina:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-219">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="d6ba7-220">Creare il riferimento a *custom.css* nel file *index.html* dopo tutti gli altri file CSS:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-220">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="d6ba7-221">Passare alla pagina *index.html* all'indirizzo `http://localhost:<port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-221">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="d6ba7-222">Immettere `http://localhost:<port>/swagger/v1/swagger.json` nella casella di testo dell'intestazione e scegliere il pulsante **Explore** (Esplora).</span><span class="sxs-lookup"><span data-stu-id="d6ba7-222">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="d6ba7-223">La pagina risultante è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="d6ba7-223">The resulting page looks as follows:</span></span>

![Interfaccia utente di Swagger con il titolo dell'intestazione personalizzato](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="d6ba7-225">Si può fare molto altro con la pagina.</span><span class="sxs-lookup"><span data-stu-id="d6ba7-225">There's much more you can do with the page.</span></span> <span data-ttu-id="d6ba7-226">Per vedere le funzionalità complete per le risorse dell'interfaccia utente, accedere all'[archivio dell'interfaccia utente di Swagger di GitHub](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="d6ba7-226">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>