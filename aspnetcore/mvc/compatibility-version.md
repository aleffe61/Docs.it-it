---
title: Versione di compatibilità per ASP.NET Core MVC
author: rick-anderson
description: Informazioni su come la classe Startup in ASP.NET Core configura i servizi e la pipeline delle richieste dell'app.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/20/2018
uid: mvc/compatibility-version
ms.openlocfilehash: bedeeba07dcca19176e779f3541c445e94efcd07
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2018
ms.locfileid: "41910073"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>Versione di compatibilità per ASP.NET Core MVC

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Il metodo <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> consente a un'app di acconsentire o rifiutare esplicitamente modifiche potenzialmente importanti del comportamento introdotte in ASP.NET Core MVC 2.1 o versioni successive. Queste modifiche potenzialmente importanti del comportamento riguardano in genere il funzionamento del sottosistema MVC e la modalità con cui il **codice dell'utente** viene chiamato dal runtime. Se si acconsente esplicitamente, si ottiene il comportamento più recente e il comportamento a lungo termine di ASP.NET Core.

Il codice seguente imposta la modalità di compatibilità su ASP.NET Core 2.1:

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

È consigliabile testare l'app usando la versione più recente (`CompatibilityVersion.Version_2_1`). Microsoft prevede che la maggior parte delle app non riscontrerà modifiche importanti del comportamento quando viene usata la versione più recente.

Le app che chiamano `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` sono protette dalle modifiche potenzialmente importanti del comportamento introdotte in ASP.NET Core 2.1 MVC e versioni 2.x successive. La protezione:

* Non è valida per tutte le modifiche 2.1 e successive, ma è destinata alle modifiche potenzialmente importanti del comportamento del runtime ASP.NET Core nel sottosistema MVC.
* Non viene estesa alla versione principale successiva.

La compatibilità predefinita per le app ASP.NET Core 2.1 e versioni 2.x successive che **non** chiamano `SetCompatibilityVersion` è la compatibilità 2.0. In altri termini il fatto di non chiamare `SetCompatibilityVersion` equivale a chiamare `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

Il codice seguente imposta la modalità di compatibilità su ASP.NET Core 2.1, con l'eccezione dei comportamenti seguenti:

* [AllowCombiningAuthorizeFilters](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [InputFormatterExceptionPolicy](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

Per le app che riscontrano modifiche potenzialmente importanti del comportamento, l'uso delle opzioni di compatibilità appropriate:

* Consente di usare la versione più recente e di rifiutare esplicitamente modifiche importanti del comportamento specifiche.
* Garantisce il tempo necessario per l'aggiornamento dell'app, che in tal modo potrà funzionare con le modifiche più recenti.

I commenti relativi alla classe di origine [MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) offrono una spiegazione chiara delle modifiche e dei motivi per cui queste rappresentano un miglioramento per la maggior parte degli utenti.

In una data futura verrà rilasciata la [versione ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap). I comportamenti precedenti supportati dalle opzioni di compatibilità verranno rimossi nella versione 3.0. Queste modifiche positive andranno a vantaggio della maggior parte degli utenti. Se si introducono ora queste modifiche, la maggior parte delle app può trarne vantaggio da subito e sarà disponibile tempo sufficiente per aggiornare le altre applicazioni.
