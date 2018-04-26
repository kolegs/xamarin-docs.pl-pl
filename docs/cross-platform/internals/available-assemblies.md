---
title: Zestawy dostępne
description: Zestawy dostępne w Xamarin.iOS, Xamarin.Android i Xamarin.Mac
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: a80a23e8c3a41b7a06e1bbcb33d171ed2dc30fd2
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
# <a name="available-assemblies"></a>Zestawy dostępne

_Zestawy dostępne w Xamarin.iOS, Xamarin.Android i Xamarin.Mac_

Xamarin.iOS, Xamarin.Android i Xamarin.Mac wszystkie dostarczany z ponad dwanaście zestawów. Tak samo, jak Silverlight jest rozszerzona podzbiór pulpitu zestawów platformy .NET, platform Xamarin jest również rozszerzonej podzbiór kilka Silverlight i pulpitu zestawów platformy .NET.

Platformy Xamarin nie są zgodne z istniejących zestawów skompilowany dla innego profilu ABI. Należy ponownie skompilować kod źródłowy na generowanie zestawów elementów docelowych poprawnego profilu (tak samo jak konieczne ponowne skompilowanie kodu źródłowego na docelowy Silverlight i .NET 3.5 oddzielnie).

Aplikacje Xamarin.Mac może zostać skompilowany w trzech trybów: korzystającą Xamarin obiektu wyselekcjonowanych profilu Mobile, programu .NET Framework 4.5 Xamarin.Mac, dzięki czemu można docelowego istniejące zestawy całego pulpitu i znaleziono nieobsługiwany jedną, która korzysta z interfejsu API programu .NET w systemie Mono Instalacja. Aby uzyskać więcej informacji, zobacz nasze [docelowych platform](~/mac/platform/target-framework.md) dokumentacji.


## <a name="net-standard-libraries"></a>Standardowych bibliotek .NET

Oprócz systemu iOS, Android i Mac powiązań, Xamarin projektów, jaką może wykorzystać [bibliotek .NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Biblioteki klas przenośnych
 
Można również korzystać z projektów Xamarin [przenośnej biblioteki klas .NET](~/cross-platform/app-fundamentals/pcl.md), mimo że ta technologia jest przestarzałe na rzecz .NET Standard.

## <a name="supported-assemblies"></a>Obsługiwanych zestawów

> [!div class="mx-tdCol2BreakAll"]
> |Zestaw|Zgodność z interfejsu API|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|Obejmuje CJK, MidEast, inne, rzadko, zachód|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|Dostawca ADO.NET dla bazy danych SQLite; Zobacz ograniczenia.|✓|✓|✓|
> |Mono.Data.Tds.dll|Obsługa protokołu TDS; używany do [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) obsługuje w ramach [system.dane](https://developer.xamarin.com/api/namespace/System.Data/).|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|Interfejsy API usług kryptograficznych.|✓|✓|✓|
> |monotouch.dll|Ten zestaw zawiera powiązanie C# CocoaTouch interfejsu API. To jest dostępna tylko w obrębie Classic projektów dla systemu iOS.|✓| | |
> |MonoTouch.&#8203;Dialog-1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|OpenGL/OpenAL obiektowo interfejsów API, rozszerzony w celu zapewnienia obsługi urządzenia iPhone.|✓|✓|✓|
> |PLik System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), oraz typy z następujących przestrzeni nazw:<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[Program .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx) , z [usunięte niektóre funkcje](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|Klient pełne oData.|✓|✓|✓|
> |System.IO.&#8203;Compression| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System.&#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|Stos WCF jako obecne w [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), oraz typy z następujących przestrzeni nazw: <br />System<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;Transactions.dll|[Program .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); część [system.dane](~/ios/data-cloud/system.data.md) obsługuje.|✓|✓|✓|
> |System.Web.&#8203;Services.dll|Podstawowe usługi sieci Web z profilu .NET 3.5, przy użyciu funkcji serwera usunięte.|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Ten zestaw zawiera powiązanie C# CocoaTouch interfejsu API. To jest używana tylko w projektach systemu iOS Unified.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|Autorzy kompilatora.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;Drawing.dll|System.Drawing API - tylko klasycznego interfejsu API. System.Drawing nie jest obsługiwana w interfejsie API Unified Xamarin.Mac .NET 4.5 lub platform przenośnych. Można dodać obsługę System.Drawing z systemami iOS i OS X przy użyciu [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics) biblioteki|✓| |✓|
