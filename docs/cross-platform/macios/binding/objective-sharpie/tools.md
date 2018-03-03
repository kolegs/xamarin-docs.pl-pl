---
title: "Narzędzia i polecenia"
description: "Omówienie narzędzi dostępnych w Sharpie cel i argumenty wiersza polecenia, które z nich korzystać."
ms.topic: article
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 0d6e953a6a45b78d470c7ff73e1d6faa7444a683
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="tools--commands"></a>Narzędzia i polecenia

_Omówienie narzędzi dostępnych w Sharpie cel i argumenty wiersza polecenia, które z nich korzystać._

<style type="text/css"> niebieski .terminal {kolorów: rgb(10,96,254);} .terminal zielony {kolorów: rgb(12,156,26);} amarantowy .terminal {kolorów: rgb(152,12,103);} </style>


Po pomyślnym Sharpie cel [zainstalowane](~/cross-platform/macios/binding/objective-sharpie/get-started.md), otwórz terminal i zapoznać się z <em>polecenia</em> Sharpie cel ma oferować:

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

Sharpie celu zawiera następujące narzędzia:

<table>
  <thead>
    <tr><td>Narzędzie</td><td>Opis</td>
  </thead>
  <tbody>
    <tr><td><b>Xcode</b></td><td>Zawiera informacje o bieżącej instalacji Xcode i wersje systemu iOS i Mac zestawów SDK, które są dostępne. Użyjemy tych informacji później gdy firma Microsoft generuje naszych powiązania.</td></tr>
    <tr><td><b>pod</b></td><td>Wyszukuje, konfiguruje instaluje (w katalogu lokalnym) i wiąże Objective-C <a href="https://cocoapods.org">CocoaPod</a> bibliotek, które są dostępne w głównym repozytorium specyfikacji. To narzędzie ocenia zainstalowanych CocoaPod ustalenie automatycznie poprawne dane wejściowe do przekazania do <code>bind</code> narzędzie poniżej. <em><strong>Nowość w wersji 3.0!</strong></em></td></tr>
    <tr><td><b>BIND</b></td><td>Analizuje pliki nagłówkowe (<code>*.h</code>) w bibliotece języka Objective-C do <a href="~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md">początkowej <i>ApiDefinition.cs</i> i <i>StructsAndEnums.cs</i> pliki</a>.</td></tr>
    <tr><td><b>update</b></td><td>Sprawdza, czy są dostępne nowsze wersje Sharpie cel i pobiera i uruchamia Instalator, jeśli jest dostępny.</td></tr>
    <tr><td><b>Sprawdź dokumentów</b></td><td>Przedstawia szczegółowe informacje o <code>[Verify]</code> atrybutów.</td></tr>
    <tr><td><b>Dokumentacja</b></td><td>Przechodzi do tego dokumentu w domyślnej przeglądarce sieci web.</td></tr>
  </tbody>
</table>

Aby uzyskać pomoc dotyczącą konkretnego narzędzia Sharpie cel, wprowadź nazwę, narzędzia i `-help` opcji. Na przykład `sharpie xcode -help` zwraca następujące dane wyjściowe:

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

Przed możemy rozpocząć proces wiązania, potrzebujemy uzyskać informacje o naszych bieżącego zainstalowanych zestawów SDK, wprowadzając następujące polecenie w terminalu `sharpie xcode -sdks`. Dane wyjściowe mogą się różnić w zależności od tego, które wersje Xcode został zainstalowany. Sharpie celu szuka zestawów SDK zainstalowany w żadnym `Xcode*.app` w obszarze `/Applications` katalogu:

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

Znajdującą się powyżej, zobaczysz, że mamy `iphoneos9.1` zainstalowany zestaw SDK na naszych maszyny i ma `arm64` wsparcie dla architektury. Firma Microsoft będzie używać tej wartości dla wszystkich próbek w tej sekcji. Te informacje w miejscu, możemy przystąpić do analizy pliki nagłówkowe biblioteka języka Objective-C do początkowej `ApiDefinition.cs` i `StructsAndEnums.cs` dla powiązania projektu.

