---
title: Zestawy dostępności
description: Ten dokument zawiera listę zestawów, które są dostępne do użycia w Xamarin.iOS, Xamarin.Android i Xamarin.Mac. Również linki do dokumentacji dotyczącej biblioteki .NET Standard oraz biblioteki klas przenośnych.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: d005d6c5e1dcfe7e9bcff44b308cea0ce7ab73e9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998657"
---
# <a name="available-assemblies"></a>Zestawy dostępności

Xamarin.iOS, Xamarin.Android i Xamarin.Mac wszystkie dostarczany z ponad tuzina zestawów. Tak, jak program Silverlight jest rozszerzonej podzbiór pulpitu zestawy .NET, platformy Xamarin jest także rozszerzone podzbiór kilka programów Silverlight i pulpitu zestawów platformy .NET.

Platformy Xamarin nie są ABI zgodny z istniejących zestawów skompilowane dla innego profilu. Należy ponownie skompilować kod źródłowy do generowania zestawów przeznaczonych dla poprawnego profilu (tak jak należy ponownie skompilować kod źródłowy, pod kątem programu Silverlight i .NET 3.5 oddzielnie).

Można kompilować aplikacje Xamarin.Mac w trzech trybów: użytkownika, który używa platformy Xamarin nadzorowana profilu Mobile, programu .NET Framework 4.5 Xamarin.Mac, co pozwala docelowe istniejących zestawów całego pulpitu i w system Mono znaleziono nieobsługiwany jedną, która używa interfejsu API platformy .NET Instalacja. Aby uzyskać więcej informacji, zobacz nasze [platform docelowych](~/mac/platform/target-framework.md) dokumentacji.


## <a name="net-standard-libraries"></a>Standardowych bibliotek platformy .NET

Oprócz systemów iOS, Android i Mac powiązań Xamarin projekty mogą wykorzystywać [biblioteki .NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Biblioteki klas przenośnych
 
Projekty Xamarin mogą również wykorzystywać [przenośnej biblioteki klas .NET](~/cross-platform/app-fundamentals/pcl.md), mimo że ta technologia jest wycofywany na rzecz .NET Standard.

## <a name="supported-assemblies"></a>Obsługiwane zestawy

> [!div class="mx-tdCol2BreakAll"]
> |Zestaw|Zgodność z interfejsu API|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|Obejmuje CJK, MidEast, inne, rzadkie, Zachodnia|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|Dostawca ADO.NET dla bazy danych SQLite; Zobacz ograniczenia.|✓|✓|✓|
> |Mono.Data.Tds.dll|Obsługa protokołu TDS; używany do [System.Data.SqlClient](xref:System.Data.SqlClient) pomocy technicznej w ramach [System.Data](xref:System.Data).|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|Interfejsy API usług kryptograficznych.|✓|✓|✓|
> |monotouch.dll|Ten zestaw zawiera powiązania C# CocoaTouch interfejsu API. To jest dostępne tylko w ramach klasyczne projekty systemu iOS.|✓| | |
> |MonoTouch.&#8203;Dialog-1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|OpenGL/OpenAL obiektowo interfejsów API, rozszerzony w celu zapewnienia obsługi urządzenia iPhone.|✓|✓|✓|
> |PLik System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), oraz typów z następujących przestrzeni nazw:<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[Program .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx) , za pomocą [niektóre funkcje usunięte](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|Klient pełną oData.|✓|✓|✓|
> |System.IO.&#8203;Compression| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System.&#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|Stos programu WCF jako obecne w [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), oraz typów z następujących przestrzeni nazw: <br />System<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;Transactions.dll|[Program .NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); wchodzi w skład [System.Data](~/ios/data-cloud/system.data.md) pomocy technicznej.|✓|✓|✓|
> |System.Web.&#8203;Services.dll|Podstawowe usługi sieci Web z profilu .NET 3.5, za pomocą funkcji serwera usunięte.|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Ten zestaw zawiera powiązania C# CocoaTouch interfejsu API. To jest używana tylko w ujednoliconej projektów systemu iOS.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|Aby uzyskać twórcom kompilatorów.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;Drawing.dll|System.Drawing interfejs API — tylko klasycznego interfejsu API. System.Drawing nie jest obsługiwana w ujednoliconego interfejsu API dla platformy Xamarin.Mac .NET w wersji 4.5 lub platform przenośnych. Obsługa System.Drawing mogą być dodawane do systemów iOS i OS X przy użyciu [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics) biblioteki|✓| |✓|
