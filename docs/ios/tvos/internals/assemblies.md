---
title: Zestawy obsługiwane przez program Xamarin dla systemu tvOS
description: Aby pomóc w wyjaśnianiu funkcje dostępne dla aplikacji systemu tvOS, ten dokument zawiera listę zestawów, obsługiwane przez program Xamarin do tworzenia aplikacji dla systemu tvOS.
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 89f2d4b1a4b58f49ab859d3603433427d05c7393
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996564"
---
# <a name="assemblies-supported-by-xamarin-for-tvos"></a>Zestawy obsługiwane przez program Xamarin dla systemu tvOS

## <a name="supported-assemblies"></a>Obsługiwane zestawy

To jest lista zestawów obsługiwane przez program Xamarin dla aplikacji Xamarin.tvOS. Poniżej przedstawiono szczegółową listę z nich.  Uwzględnić pewne istotne pominięć `System.EnterpriseServices`, stos ASP.NET i Windows.Forms.

|Zestaw|Dodano|Zgodność z interfejsu API|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|Aby uzyskać twórcom kompilatorów.|
|Mono.Data.Sqlite.dll|1.2|Dostawca ADO.NET dla bazy danych SQLite; zobacz [ograniczenia](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|Obsługa protokołu TDS; używany do [System.Data.SqlClient](xref:System.Data.SqlClient) pomocy technicznej w ramach [System.Data](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1.0|Interfejsy API usług kryptograficznych.|
|monotouch.dll|1.0|Ten zestaw zawiera [powiązania C# do interfejsu API CocoaTouch](https://docs.microsoft.com/dotnet/api/?view=xamarinios-10.8).|
|mscorlib.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|Interfejsów API, obiektowo OpenGL/OpenAL [rozszerzone, aby zapewnić obsługę urządzenia iPhone](https://developer.xamarin.com/api/namespace/OpenGLES/).|
|PLik System.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), oraz typów z następujących przestrzeni nazw: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[Program .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx), [z niektórych funkcji](~/ios/data-cloud/system.data.md).|
|System.Data.Service.Client.dll|3.x|Klient pełną oData.|
|System.Drawing|1.0|System.Drawing interfejs API — tylko klasycznego interfejsu API.<br />_System.Drawing nie jest obsługiwana w ujednoliconego interfejsu API dla platformy Xamarin.Mac .NET w wersji 4.5 lub platform przenośnych._|
|System.Json.dll|1.1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[Usługi WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) stosu się w [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), oraz typów z następujących przestrzeni nazw: <ul><li>System</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[Program .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); wchodzi w skład [System.Data](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) pomocy technicznej.|
|System.Web.Services|1.1|[Podstawowe usługi sieci Web](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) z profilu .NET 3.5, za pomocą funkcji serwera usunięte.|
|System.Xml.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>Biblioteki klas przenośnych

Oprócz powiązania Mac może zużywać Xamarin.tvOS [przenośnej biblioteki klas .NET](~/cross-platform/app-fundamentals/pcl.md).

## <a name="related-links"></a>Linki pokrewne

- [tvOS](https://developer.apple.com/tvos/)
- [Ludzi przewodniki interfejsu systemu tvOS](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Podręcznik programowania aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
