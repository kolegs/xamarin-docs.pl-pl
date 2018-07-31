---
title: Znane problemy i ich rozwiązania
description: W tym dokumencie opisano znane problemy i rozwiązania dla środowiska Xamarin Workbooks. Omówiono w nim problemów CultureInfo, problemy z formatu JSON i nie tylko.
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: d362698d2844ae6d96bba4929d509f5373742578
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350998"
---
# <a name="known-issues--workarounds"></a>Znane problemy i ich rozwiązania

## <a name="persistence-of-cultureinfo-across-cells"></a>Stan trwały CultureInfo w komórkach

Ustawienie `System.Threading.CurrentThread.CurrentCulture` lub `System.Globalization.CultureInfo.CurrentCulture` nie ulegają zmianie komórek skoroszytu na cele platformy Mono na podstawie skoroszytów (Mac, iOS i Android) ze względu na [usterkę w rozwiązaniu w Mono `AppContext.SetSwitch` ] [ appcontext-bug] implementacji .

### <a name="workarounds"></a>Rozwiązania

* Ustawianie lokalnego domeny aplikacji `DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* Lub aktualizacji do skoroszytów 1.2.1 lub nowszej, który przepisze przypisania do `System.Threading.CurrentThread.CurrentCulture` i `System.Globalization.CultureInfo.CurrentCulture` zapewnienie żądane zachowanie (obejść Mono usterki).

## <a name="unable-to-use-newtonsoftjson"></a>Nie można użyć pakietu Newtonsoft.Json

### <a name="workaround"></a>Obejście

* Zaktualizuj skoroszyty 1.2.1, co spowoduje zainstalowanie pakietu Newtonsoft.Json 9.0.1.
  Skoroszyty 1.3, obecnie w kanał alfa, obsługuje wersje, 10 i nowszych.

### <a name="details"></a>Szczegóły

Newtonsoft.Json 10 został wydany który zaktualizowany zależność Microsoft.CSharp, który powoduje konflikt ze skoroszytami wersji, który jest dostarczany do obsługi `dynamic`. Ten problem jest rozwiązany w wersji zapoznawczej 1.3 skoroszyty, ale teraz ma współpracowaliśmy obejścia tego problemu przez przypinania Newtonsoft.Json specjalnie do wersji 9.0.1.

Pakiety NuGet jawnie w zależności od Newtonsoft.Json 10 lub nowszej są obsługiwane tylko w 1.3 skoroszyty obecnie w kanału alfa.

## <a name="code-tooltips-are-blank"></a>Kod etykietki narzędzi są puste

Brak [usterkę w edytorze Monaco] [ monaco-bug] w przeglądarce Safari/aparatu WebKit, który jest używany w aplikacji skoroszyty dla komputerów Mac, który powoduje renderowanie etykietki narzędzi kodu bez tekstu.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Obejście

* Kliknięcie etykietki narzędzia, po jego wyświetleniu wymusi tekstu do renderowania.

* Lub zaktualizuj ze skoroszytami 1.2.1 lub nowszej

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>Skiasharp — programy renderujące brakuje w 1.3 skoroszytów

Począwszy od 1.3 skoroszyty usunęliśmy SkiaSharp programy renderujące, które firma Microsoft dostarczane w skoroszytach 0.99.0, na rzecz SkiaSharp, zapewniając programy renderujące, samego, przy użyciu naszego [SDK](~/tools/workbooks/sdk/index.md).

### <a name="workaround"></a>Obejście

* Zaktualizuj SkiaSharp do najnowszej wersji w programie NuGet. W czasie pisania to 1.57.1.

## <a name="related-links"></a>Linki pokrewne

- [Raportowania błędów](~/tools/workbooks/install.md#reporting-bugs)
