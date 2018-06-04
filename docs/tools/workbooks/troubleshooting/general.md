---
title: Znane problemy i rozwiązania
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: topgenorth
ms.author: toopge
ms.openlocfilehash: a424b37840bd944302235221f2e0c6a478d4bab3
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732963"
---
# <a name="known-issues--workarounds"></a>Znane problemy i rozwiązania

## <a name="persistence-of-cultureinfo-across-cells"></a>Trwałość CultureInfo w komórkach

Ustawienie `System.Threading.CurrentThread.CurrentCulture` lub `System.Globalization.CultureInfo.CurrentCulture` nie zachowany po komórek skoroszytu na cele skoroszyty na podstawie Mono (Mac, iOS i Android) z powodu [usterkę w jego Mono `AppContext.SetSwitch` ] [ appcontext-bug] implementacji .

### <a name="workarounds"></a>Rozwiązania

* Ustawianie lokalnego domeny aplikacji `DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* Lub aktualizacji do skoroszytów 1.2.1 lub nowszej, który będzie przepisywania przypisania do `System.Threading.CurrentThread.CurrentCulture` i `System.Globalization.CultureInfo.CurrentCulture` do zapewnienia żądanego zachowania (Praca wokół Mono usterki).

## <a name="unable-to-use-newtonsoftjson"></a>Nie można użyć Newtonsoft.Json

### <a name="workaround"></a>Obejście

* Aktualizacja ze skoroszytami 1.2.1, na których zostanie zainstalowany Newtonsoft.Json 9.0.1.
  Skoroszyty 1.3, obecnie w kanału alfa obsługuje wersje 10 i nowszych.

### <a name="details"></a>Szczegóły

Newtonsoft.Json 10 został zwolniony, który upadku zależność Microsoft.CSharp, który powoduje konflikt ze skoroszytami wersji jest dostarczany do obsługi `dynamic`. To jest opisany w wersji zapoznawczej 1.3 skoroszytów, ale teraz opracowaliśmy obejścia tego problemu przez przypinania Newtonsoft.Json specjalnie do wersji 9.0.1.

Pakiety NuGet jawnie w zależności od Newtonsoft.Json 10 lub nowszej są obsługiwane tylko w skoroszytach 1.3 obecnie w kanału alfa.

## <a name="code-tooltips-are-blank"></a>Etykietki narzędzi kodu są puste

Brak [usterkę w edytorze Monaco] [ monaco-bug] w Safari/WebKit, który jest używany w aplikacji skoroszyty Mac, który powoduje renderowanie etykietki narzędzi kodu bez tekstu.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Obejście

* Kliknięcie na elemencie tooltip po jego wyświetleniu wymusi tekstu do renderowania.

* Lub zaktualizuj ze skoroszytami 1.2.1 lub nowszej

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>Moduły renderowania SkiaSharp Brak w 1.3 skoroszytów

Począwszy od 1.3 skoroszyty usunęliśmy SkiaSharp moduły renderowania, które firma Microsoft dostarczone w skoroszytach 0.99.0, na rzecz SkiaSharp dostarczanie programy renderujące samego, przy użyciu naszych [SDK](~/tools/workbooks/sdk/index.md).

### <a name="workaround"></a>Obejście

* Zaktualizuj SkiaSharp do najnowszej wersji w NuGet. W czasie zapisywania to 1.57.1.

## <a name="related-links"></a>Linki pokrewne

- [Raportowanie błędów](~/tools/workbooks/install.md#reporting-bugs)
