---
title: Podgląd XAML dla platformy Xamarin.Forms
description: W tym artykule wyjaśniono, jak użyć podglądu plików XAML, aby zobaczyć układów platformy Xamarin.Forms renderowane podczas pisania. Podgląd XAML jest dostępna w Visual Studio 2017 i Visual Studio dla komputerów Mac.
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/31/2018
ms.openlocfilehash: 25c8e1a34f8be5ab2f8491e75fa5aac470d55bc8
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245862"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Podgląd XAML dla platformy Xamarin.Forms

_Zobacz układów platformy Xamarin.Forms renderowane podczas wpisywania!_

## <a name="requirements"></a>Wymagania

Projekt wymaga najnowszy pakiet NuGet platformy Xamarin.Forms dla podglądu plików XAML do pracy. Wyświetlanie podglądu aplikacji systemu Android wymaga [JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Więcej informacji można znaleźć w [informacje o wersji](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer).

## <a name="getting-started"></a>Wprowadzenie

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Użyj **Widok > inne okna > Podgląd platformy Xamarin.Forms** menu programu Visual Studio, aby otworzyć okna podglądu. Użyj **okna > nową grupę kart pionowy** menu w miejsce side-by-side.

[![W programie Visual Studio w wersji zapoznawczej formant ListView](xaml-previewer-images/xamlp-list-vs-sml.png "podgląd formularzy w programie Visual Studio")](xaml-previewer-images/xamlp-list-vs.png#lightbox "podgląd formularzy w programie Visual Studio")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**Podgląd** przycisk mogą być wyświetlane w edytorze prawym przyciskiem myszy plik XAML, a następnie wybierając **Otwórz za pomocą > Podgląd formularzy programu**. Okienko podglądu może następnie być widoczny, czy ukryty przez naciśnięcie przycisku **Podgląd** przycisk w prawym górnym rogu wszystkie okna dokumentu XAML:

[![Podgląd formantu ListView w programie Visual Studio dla komputerów Mac](xaml-previewer-images/xamlp-list-sml.png "podgląd formularzy w programie Visual Studio for Mac")](xaml-previewer-images/xamlp-list.png#lightbox "podgląd formularzy w programie Visual Studio dla komputerów Mac")

-----

## <a name="xaml-preview-options"></a>Opcje podglądu XAML

Dostępne są opcje wzdłuż górnej części okienka podglądu:

* **Telefon** — renderowania na ekranie rozmiar telefonu
* **Tablet** — renderowania na ekranie tabletu rozmiaru (należy pamiętać, w prawym dolnym rogu okienka istnieją kontrolki powiększania)
* **Android** — Pokaż Android wersję ekranu
* **iOS** — Pokaż wersję systemu iOS ekranu
* Portait (ikona) — Podgląd używa w orientacji pionowej
* Pozioma (ikona) — używa poziomą orientację w wersji zapoznawczej

## <a name="adding-design-time-data"></a>Dodawanie danych w czasie projektowania

Układy może być trudne do wizualizacji bez danych powiązanymi z formantami interfejsu użytkownika. Aby wprowadzić bardziej użyteczna w wersji zapoznawczej, przypisać niektóre dane statyczne formanty hardcoding kontekst powiązania (albo w CodeBehind lub przy użyciu kodu XAML).

Dotyczą Kuba Montemagno [wpis w blogu na temat dodawania danych czasu projektowania](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) na temat sposobu powiązać statycznych ViewModel w języku XAML.

## <a name="detecting-design-mode"></a>Wykrywanie trybu projektowania

Statycznych [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) właściwości można zbadać w celu określenia, czy aplikacja działa w podglądzie. Dzięki temu można określić kodu, które będą wykonywane tylko wtedy, gdy aplikacja jest uruchomiona w podglądzie:

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Sprawdzenie problemów poniżej, a [fora Xamarin](https://forums.xamarin.com/categories/xamarin-forms), jeśli wystąpią problemy.

### <a name="xaml-preview-isnt-showing"></a>Podgląd XAML nie jest wyświetlany.

Sprawdź następujące informacje:

* Projekt powinien skompilowany w (skompilowanych) przed podjęciem próby Podgląd plików XAML.
* Agent projektant musi być konfiguracji po raz pierwszy Podgląd pliku XAML — wskaźnik postępu będą wyświetlane w podglądzie, wraz z komunikaty o postępie, dopóki nie jest gotowy.
* Spróbuj zamknąć i ponownie otworzyć plik XAML.

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>Nieprawidłowy XAML: Projekt systemu Android musi wbudowane, aby można było utworzyć w wersji zapoznawczej

Podglądu plików XAML wymaga, aby przed renderowaniem strony można skompilować projekt.
Jeśli w górnej części okienka podglądu pojawi się poniższy błąd, ponownie skompilować aplikację i spróbuj ponownie.

![Komunikat o błędzie: projekt musi zostać skompilowane na początku](xaml-previewer-images/error-not-built-sml.png "komunikat o błędzie: Skompiluj ponownie projekt")
