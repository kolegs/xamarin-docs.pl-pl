---
title: Zestawy
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 7d0ee27cfa2ae153ef481f943402f5fcfc5d04e4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="assemblies"></a>Zestawy

## <a name="supported-assemblies"></a>Obsługiwanych zestawów

To jest lista zestawów obsługiwane przez program Xamarin dla aplikacji Xamarin.tvOS. Poniżej przedstawiono szczegółową listę tych.  Niektóre ważne pominięć obejmują `System.EnterpriseServices`, stos ASP.NET i Windows.Forms.

|Zestaw|Dodane|Zgodność z interfejsu API|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|Autorzy kompilatora.|
|Mono.Data.Sqlite.dll|1.2|Dostawca ADO.NET dla bazy danych SQLite; zobacz [ograniczenia](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|Obsługa protokołu TDS; używany do [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) obsługuje w ramach [system.dane](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1.0|Interfejsy API usług kryptograficznych.|
|monotouch.dll|1.0|Ten zestaw zawiera [powiązanie C# do interfejsu API CocoaTouch](https://developer.xamarin.com/api/root/ios-unified/).|
|mscorlib.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|Interfejsy API, obiektowo OpenGL/OpenAL [rozszerzone, aby zapewnić obsługę urządzeń iPhone](https://developer.xamarin.com/api/namespace/OpenGLES/).|
|PLik System.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), oraz typy z następujących przestrzeni nazw: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[Program .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx), [z niektóre funkcje usunięte](~/ios/data-cloud/system.data.md).|
|System.Data.Service.Client.dll|3.x|Klient pełne oData.|
|System.Drawing|1.0|System.Drawing API - tylko klasycznego interfejsu API.<br />_System.Drawing nie jest obsługiwana w interfejsie API Unified Xamarin.Mac .NET 4.5 lub platform przenośnych._|
|System.Json.dll|1.1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[Usługi WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) stosu jako obecne w [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), oraz typy z następujących przestrzeni nazw: <ul><li>System</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[Program .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); część [system.dane](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) obsługuje.|
|System.Web.Services|1.1|[Podstawowe usługi sieci Web](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) z profilu .NET 3.5, przy użyciu funkcji serwera usunięte.|
|System.Xml.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>Biblioteki klas przenośnych

Oprócz powiązania Mac może wykorzystać Xamarin.tvOS [przenośnej biblioteki klas .NET](~/cross-platform/app-fundamentals/pcl.md).



## <a name="related-links"></a>Linki pokrewne

- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
