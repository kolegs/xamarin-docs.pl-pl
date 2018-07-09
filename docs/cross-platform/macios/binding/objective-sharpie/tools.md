---
title: Narzędzie Objective Sharpie narzędzia i polecenia
description: Ten dokument zawiera omówienie narzędzi dostępnych w programie Objective Sharpie i argumenty wiersza polecenia, aby korzystać z nich.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 718b5104ddc4593d080b88b062c42d371d9e8e2e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855071"
---
# <a name="objective-sharpie-tools--commands"></a>Narzędzie Objective Sharpie narzędzia i polecenia

_Omówienie narzędzi dostępnych w programie Objective Sharpie i argumenty wiersza polecenia, aby umożliwić ich używanie._

<style type="text/css"> niebieski .terminal {kolorów: rgb(10,96,254);} .terminal zielony {kolorów: rgb(12,156,26);} amarantowy .terminal {kolorów: rgb(152,12,103);} </style>


Po pomyślnym Objective Sharpie [zainstalowane](~/cross-platform/macios/binding/objective-sharpie/get-started.md), otwórz terminal i zapoznaj się z <em>polecenia</em> Objective Sharpie ma do zaoferowania:

<pre>$ <b>sharpie -help</b>
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, -help                Show detailed help
  -v, -version             Show version information

Telemetry Options:
  -tlm-about               Show a detailed overview of what usage and binding
                             information will be submitted to Xamarin by
                             default when using Objective Sharpie.
  -tlm-do-not-submit       Do not submit any usage or binding information to
                             Xamarin. Run 'sharpie -tml-about' for more
                             information.
  -tlm-do-not-identify     Do not submit Xamarin account information when
                             submitting usage or binding information to Xamarin
                             for analysis. Binding attempts and usage data will
                             be submitted anonymously if this option is
                             specified.

Available Tools:
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation</pre>

Narzędzie Objective Sharpie zawiera następujące narzędzia:

|Narzędzie|Opis|
|--- |--- |
|**środowisko xcode**|Zawiera informacje o bieżącej instalacji programu Xcode i wersje dla systemów iOS i Mac zestawów SDK, które są dostępne. Firma Microsoft będzie używać tych informacji w dalszej części gdy firma Microsoft generuje naszych powiązania.|
|**Zasobnik**|Wyszukuje, konfiguruje, instaluje (w katalogu lokalnym) i wiąże języka Objective-C [CocoaPod](https://cocoapods.org/) bibliotek, które są dostępne z głównym repozytorium specyfikacji. To narzędzie oblicza CocoaPod zainstalowane, aby automatycznie wywnioskować poprawne dane wejściowe do przekazania do `bind` narzędzie poniżej. Nowość w wersji 3.0!|
|**powiązania**|Analizuje pliki nagłówkowe (`*.h`) w bibliotece języka Objective-C do początkowej [ApiDefinition.cs i StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) plików.|
|**update**|Sprawdza, czy są dostępne nowsze wersje Objective Sharpie i pobiera i uruchamia Instalatora, jeśli jest dostępny.|
|**Sprawdź docs**|Przedstawia szczegółowe informacje na temat `[Verify]` atrybutów.|
|**Dokumentacja**|Powoduje przejście do tego dokumentu w domyślnej przeglądarce internetowej.|

Aby uzyskać pomoc dotyczącą określonego narzędzie Objective Sharpie, wprowadź nazwę narzędzia i `-help` opcji. Na przykład `sharpie xcode -help` zwraca następujące wyniki:

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

Zanim firma będzie mogła rozpocząć proces wiązania, potrzebujemy uzyskać informacje o naszej bieżącej zainstalowanych zestawów SDK, wprowadzając następujące polecenie w terminalu `sharpie xcode -sdks`. Dane wyjściowe mogą się różnić w zależności od tego, które wersje środowiska Xcode został zainstalowany. Narzędzie Objective Sharpie wyszukuje zestawy SDK zainstalować w dowolnej `Xcode*.app` w obszarze `/Applications` katalogu:

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

Znajdującą się powyżej, okaże się, że mamy `iphoneos9.1` zestaw SDK zainstalowany na naszą maszynę i ma `arm64` Obsługa architektury. Firma Microsoft będzie używać tej wartości we wszystkich przykładach w tej sekcji. Dzięki tym informacjom w miejscu, możemy przystąpić do analizy pliki nagłówkowe biblioteki języka Objective-C do początkowej `ApiDefinition.cs` i `StructsAndEnums.cs` powiązania projektu.

## <a name="related-links"></a>Linki pokrewne

- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)