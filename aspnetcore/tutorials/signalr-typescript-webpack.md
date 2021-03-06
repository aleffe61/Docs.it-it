---
title: Usare ASP.NET Core SignalR con TypeScript e Webpack
author: ssougnez
description: In questa esercitazione verrà configurato Webpack per creare un bundle e compilare un'app Web ASP.NET Core SignalR il cui client è scritto in TypeScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: a7b39bbf657244db83e9d60014a5759000eb5f14
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206952"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a>Usare ASP.NET Core SignalR con TypeScript e Webpack

Di [Sébastien Sougnez](https://twitter.com/ssougnez) e [Scott Addie](https://twitter.com/Scott_Addie)

[Webpack](https://webpack.js.org/) consente agli sviluppatori di creare un bundle delle risorse di un'app Web sul lato client-e di compilarle. Questa esercitazione illustra l'uso di Webpack in un'app Web ASP.NET Core SignalR il cui client è scritto in [TypeScript](https://www.typescriptlang.org/).

In questa esercitazione si imparerà a:

> [!div class="checklist"]
> * Eseguire lo scaffolding di un'app ASP.NET Core SignalR di base
> * Configurare il client TypeScript di SignalR
> * Configurare una pipeline di compilazione tramite Webpack
> * Configurare il server SignalR
> * Abilitare la comunicazione tra client e server

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisiti

Installare il software seguente:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2.1 o versione successiva](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.7.3 o successive con il carico di lavoro **Sviluppo ASP.NET e Web**

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

* [.NET Core SDK 2.1 o versione successiva](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)

---

## <a name="create-the-aspnet-core-web-app"></a>Creare l'app Web ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Configurare Visual Studio in modo che cerchi npm nella variabile di ambiente *PATH*. Per impostazione predefinita, Visual Studio usa la versione di npm trovata nella directory di installazione. Seguire queste istruzioni in Visual Studio:

1. Passare a **Strumenti** > **Opzioni** > **Progetti e soluzioni** > **Gestione pacchetti Web** > **Strumenti Web esterni**.
1. Selezionare la voce *$(PATH)* dall'elenco. Fare clic sulla freccia in su per spostare la voce nella seconda posizione nell'elenco. La prima voce si riferisce ai pacchetti locali del progetto.

    ![Configurazione di Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

La configurazione di Visual Studio è completata. A questo punto è possibile creare il progetto

1. Selezionare l'opzione di menu **File** > **Nuovo** > **Progetto** e scegliere il modello **Applicazione Web ASP.NET Core**.
1. Assegnare al progetto il nome *SignalRWebPack* e fare clic su **OK**.
1. Selezionare *.NET Core* nell'elenco a discesa del framework di destinazione e quindi selezionare *ASP.NET Core 2.1* nell'elenco a discesa del selettore del framework. Selezionare il modello **Vuoto** e fare clic su **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Eseguire il comando seguente nel **Terminale integrato**:

```console
dotnet new web -o SignalRWebPack
```

Nella directory *SignalRWebPack* viene creata un'app Web ASP.NET Core vuota destinata a .NET Core.

---

## <a name="configure-webpack-and-typescript"></a>Configurare Webpack e TypeScript

I passaggi seguenti consentono di configurare la conversione da TypeScript a JavaScript e di creare un bundle delle risorse sul lato client.

1. Eseguire il comando seguente nella radice del progetto per creare un file *package.json*:

    ```console
    npm init -y
    ```

1. Aggiungere la proprietà evidenziata al file *package.json*:

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    Impostando la proprietà `private` su `true` si evita la visualizzazione di avvisi di installazione del pacchetto nel passaggio successivo.

1. Installare i pacchetti npm necessari. Eseguire il comando seguente dalla radice del progetto:

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    Ecco alcuni dettagli del comando da tenere in considerazione:

    * Un numero di versione segue il segno `@` per ogni nome di pacchetto. npm installa queste versioni del pacchetto specifiche.
    * L'opzione `-E` disabilita il comportamento predefinito di npm, in base al quale gli operatori intervallo di [Versionamento Semantico](https://semver.org/) vengono scritti in *package.json*. Ad esempio, viene usato `"webpack": "4.12.0"` invece di `"webpack": "^4.12.0"`. Questa opzione impedisce che vengano eseguiti aggiornamenti non desiderati a versioni più recenti del pacchetto.

    Per informazioni dettagliate, vedere la documentazione ufficiale di [npm-install](https://docs.npmjs.com/cli/install).

1. Sostituire la proprietà `scripts` del file *package.json* con il frammento di codice seguente:

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Ecco una spiegazione degli script:

    * `build`: crea un bundle delle risorse sul lato client in modalità di sviluppo e verifica la presenza di modifiche al file. Questo controllo fa sì che il bundle venga rigenerato a ogni modifica del file di progetto. L'opzione `mode` disabilita le ottimizzazioni di produzione, come l'eliminazione del codice non utilizzato e la minimizzazione. Usare `build` solo in modalità di sviluppo.
    * `release`: crea un bundle delle risorse sul lato client in modalità di produzione.
    * `publish`: esegue lo script `release` per creare il bundle delle risorse sul lato client in modalità di produzione. Chiama il comando [publish](/dotnet/core/tools/dotnet-publish) dell'interfaccia della riga di comando di .NET Core per pubblicare l'app.

1. Creare un file denominato *webpack.config.js* nella radice del progetto con il contenuto seguente:

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    Il file precedente configura la compilazione di Webpack. Ecco alcuni dettagli di configurazione del server da tenere in considerazione:

    * La proprietà `output` esegue l'override del valore predefinito di *dist*. Il bundle viene invece creato nella directory *wwwroot*.
    * La matrice `resolve.extensions` include *.js* per importare il codice JavaScript del client SignalR.

1. Creare una nuova directory *src* nella radice del progetto. La sua funzione è quella di archiviare le risorse lato client del progetto.

1. Creare *src/index.html* con il contenuto seguente.

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    Il codice HTML precedente definisce il markup del boilerplate della home page.

1. Creare una nuova directory *src/css*. La sua funzione è quella di archiviare i file *.css* del progetto.

1. Creare *src/css/main.css* con il contenuto seguente:

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    Il file *main.css* precedente definisce lo stile dell'app.

1. Creare *src/tsconfig.json* con il contenuto seguente:

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    Il codice precedente configura il compilatore TypeScript in modo da produrre codice JavaScript compatibile con [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.

1. Creare *src/index.ts* con il contenuto seguente:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    Il compilatore TypeScript precedente recupera i riferimenti agli elementi DOM e collega due gestori eventi:

    * `keyup`: questo evento viene generato quando l'utente digita qualcosa nella casella di testo identificata come `tbMessage`. La funzione `send` viene chiamata quando l'utente preme **INVIO**.
    * `click`: questo evento viene generato quando l'utente fa clic sul pulsante **Invia**. Viene chiamata la funzione `send`.

## <a name="configure-the-aspnet-core-app"></a>Configurare l'app ASP.NET Core

1. Il codice fornito nel metodo `Startup.Configure` visualizza *Hello World!*. Sostituire la chiamata del metodo `app.Run` con chiamate a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    Il codice precedente consente al server di individuare e servire il file *index.html*, sia che l'utente immetta l'URL completo o l'URL radice dell'app Web.

1. Chiamare [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) nel metodo `Startup.ConfigureServices`. Aggiunge i servizi SignalR al progetto.

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. Eseguire il mapping di una route */hub* all'hub `ChatHub`. Aggiungere le righe seguenti alla fine del metodo `Startup.Configure`:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. Creare una nuova directory denominata *Hubs* nella radice del progetto. La sua funzione è quella di archiviare l'hub SignalR, che verrà creato nel passaggio successivo.

1. Creare l'hub *Hubs/ChatHub.cs* con il codice seguente:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. Aggiungere il codice seguente all'inizio del file *Startup.cs* per risolvere il riferimento a `ChatHub`:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>Abilitare la comunicazione tra client e server

L'app attualmente visualizza un semplice form per l'invio di messaggi. Quando si prova a eseguire questa operazione, non accade nulla. Il server è in ascolto di una route specifica, ma non esegue alcuna operazione con i messaggi inviati.

1. Eseguire il comando seguente nella radice del progetto:

    ```console
    npm install @aspnet/signalr
    ```

    Il comando precedente installa il [client TypeScript di SignalR](https://www.npmjs.com/package/@aspnet/signalr), che consente al client di inviare messaggi al server.

1. Aggiungere il codice evidenziato al file *src/index.ts*:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Il codice precedente supporta la ricezione di messaggi dal server. La classe `HubConnectionBuilder` crea un nuovo compilatore per la configurazione della connessione al server. La funzione `withUrl` configura l'URL dell'hub.

    SignalR consente lo scambio di messaggi tra un client e un server. Ogni messaggio ha un nome specifico. Ad esempio, i messaggi con nome `messageReceived` eseguono la logica responsabile della visualizzazione del nuovo messaggio nell'area dei messaggi. L'ascolto di un messaggio specifico può essere eseguito tramite la funzione `on`. È possibile restare in ascolto di un numero indefinito di nomi di messaggio. Si può anche passare parametri al messaggio, come il nome dell'autore e il contenuto del messaggio ricevuto. Quando il client riceve un messaggio, viene creato un nuovo elemento `div` con il nome dell'autore e il contenuto del messaggio nell'attributo `innerHTML`. Viene aggiunto all'elemento `div` principale che visualizza i messaggi.

1. Ora che il client può ricevere messaggi, configurarlo anche per l'invio. Aggiungere il codice evidenziato al file *src/index.ts*:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    Per inviare un messaggio attraverso la connessione WebSockets è necessario chiamare il metodo `send`. Il primo parametro del metodo è il nome del messaggio. I dati del messaggio sono rappresentati dagli altri parametri. In questo esempio un messaggio identificato come `newMessage` viene inviato al server. Il messaggio è costituito dal nome utente e dall'input dell'utente in una casella di testo. Se l'invio riesce, il valore della casella di testo viene cancellato.

1. Aggiungere il metodo evidenziato alla classe `ChatHub`:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    Il codice precedente trasmette i messaggi ricevuti a tutti gli utenti connessi dopo che il server li riceve. Non è necessario avere un metodo generico `on` per ricevere tutti i messaggi. È sufficiente un metodo denominato dopo il nome del messaggio.

    In questo esempio il client TypeScript invia un messaggio identificato come `newMessage`. Il metodo C# `NewMessage` si aspetta i dati inviati dal client. Viene eseguita una chiamata al metodo [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) su [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all). I messaggi ricevuti vengono inviati a tutti i client connessi all'hub.

## <a name="test-the-app"></a>Eseguire il test dell'app

Per verificare che l'app funzioni, eseguire la procedura seguente.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Eseguire Webpack in modalità *versione*. Nella finestra **Console di gestione pacchetti** eseguire il comando seguente nella radice del progetto:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Selezionare **Debug** > **Avvia senza eseguire il debug** per avviare l'app in un browser senza collegare il debugger. Il file *wwwroot/index.html* viene servito all'indirizzo `http://localhost:<port_number>`.

1. Aprire un'altra istanza del browser (qualsiasi browser). Incollare l'URL nella barra degli indirizzi.

1. Scegliere un browser, digitare qualcosa nella casella di testo **Messaggio** e fare clic sul pulsante **Invia**. Il nome utente e il messaggio univoci vengono visualizzati immediatamente in entrambe le pagine.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

1. Eseguire Webpack in modalità *versione* eseguendo il comando seguente nella radice del progetto:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Compilare ed eseguire l'app eseguendo il comando seguente nella radice del progetto:

    ```console
    dotnet run
    ```

    Il server Web avvia l'app e la rende disponibile su localhost.

1. Aprire un browser all'indirizzo `http://localhost:<port_number>`. Il file *wwwroot/index.html* viene servito. Copiare l'URL dalla barra dell'indirizzo.

1. Aprire un'altra istanza del browser (qualsiasi browser). Incollare l'URL nella barra degli indirizzi.

1. Scegliere un browser, digitare qualcosa nella casella di testo **Messaggio** e fare clic sul pulsante **Invia**. Il nome utente e il messaggio univoci vengono visualizzati immediatamente in entrambe le pagine.

---

![Messaggio visualizzato in entrambe le finestre del browser](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
