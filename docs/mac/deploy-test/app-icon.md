---
title: Ikona aplikacji
description: W tym artykule opisano tworzenie obrazów wymaganych dla ikony aplikacji Xamarin.Mac, tworzenie pakietów obrazów do pliku .icns i, w tym ikonę w projekcie Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 675b9405-d9a7-49f0-94ad-417f10a71d11
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3603e43b4b98d1387c718d0a6010d38aa01440c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="application-icon"></a>Ikona aplikacji

_W tym artykule opisano tworzenie obrazów wymaganych dla ikony aplikacji Xamarin.Mac, tworzenie pakietów obrazów do pliku .icns i, w tym ikonę w projekcie Xamarin.Mac._


## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, deweloper ma dostęp do tego samego obrazu i Ikona narzędzi dewelopera, pracy w *Objective-C* i *Xcode* jest.

Ikona dużą należy przekazać głównym celem Xamarin.Mac aplikacji i wskazówkę dotyczącą sposobu obsługi, jaki użytkownik powinien oczekiwać podczas korzystania z aplikacji. W tym artykule opisano kroki niezbędne do utworzenia obrazu zasoby wymagane do ikony, pakowanie tych zasobów w `AppIcons.appiconset` pliku i korzystanie z tego pliku w aplikacji Xamarin.Mac.

![Edytor AppIcons.appiconset](app-icon-images/intro01.png "AppIcons.appiconset edytora")


## <a name="application-icon"></a>Ikona aplikacji

Ikona dużą należy przekazać głównym celem Xamarin.Mac aplikacji i wskazówkę dotyczącą sposobu obsługi, jaki użytkownik powinien oczekiwać podczas korzystania z aplikacji. Każda aplikacja macOS musi zawierać kilku rozmiarów ikona do wyświetlenia w wyszukiwarce, dokowania Launchpad i innych miejscach komputera.


## <a name="designing-the-icon"></a>Projektowanie ikony

Apple sugeruje Poniższe porady podczas projektowania ikona aplikacji:

- Należy wziąć pod uwagę, podając ikonę kształtu realistyczne i unikatowe.
- Jeśli aplikacja macOS ma duplikat systemu iOS, nie ponownie użyć ikony aplikacji dla systemu iOS.
- Użycie obrazów uniwersalnych łatwo rozpoznać osoby.
- Dokładamy wszelkich starań, dla uproszczenia.
- Użyj koloru i tle oszczędnie aby ikona Poinformuj wątku aplikacji.
- Unikaj mieszanie tekstu z _zamiast_ tekst lub wiersze, aby zasugerować tekstu.
- Utwórz wersję idealny ikony podmiotu zamiast rzeczywistej zdjęcie.
- Unikaj używania macOS elementy interfejsu użytkownika w ikon.
- Nie należy używać repliki ikony Apple ikon.

Przeczytaj [galerii ikona aplikacji](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Gallery.html#//apple_ref/doc/uid/20000957-CH88-SW1) i [projektowania ikony aplikacji](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Designing.html#//apple_ref/doc/uid/20000957-CH87-SW1) sekcje firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) przed rozpoczęciem projektowania aplikacji Xamarin.Mac ikony.


## <a name="required-image-sizes-and-filenames"></a>Rozmiary obrazów wymaganych i nazwy plików

Podobnie jak wszystkie inne zasobu obrazu dewelopera ma użyć w aplikacji Xamarin.Mac aplikacji ikona musi podać zarówno Standard, jak i rozpoznawania siatkówki wersji. Ponownie, podobnie jak każdy inny obraz, należy użyć `@2x` formatowania w nazwach plików ikony:

- **Standard rozpoznawania**  - _Nazwa_obrazu_**.** _rozszerzenie nazwy pliku_ (przykład: **icon_512x512.png**)
- **O wysokiej rozdzielczości**  - _Nazwa_obrazu_**@2x.** _rozszerzenie nazwy pliku_ (przykład: **icon_512x512@2x.png**)

Na przykład aby przekazać wersji 512 x 512 ikony aplikacji, plik będzie nosić **icon_512x512.png** i **icon_512x512@2x.png**.

Aby upewnić się, że ikony wygląda bardzo we wszystkich miejscach, że użytkownicy będą widzieć go, należy podać zasobów do rozmiarów wymienionych poniżej:

|Nazwa pliku|Rozmiar w pikselach|
|---|---|
|icon_512x512@2x.png|1024 x 1024|
|icon_512x512.png|512 x 512|
|icon_256x256@2x.png|512 x 512|
|icon_256x256.png|256 x 256|
|icon_128x128@2x.png|256 x 256|
|icon_128x128.png|128 x 128|
|icon_32x32@2x.png|64 x 64|
|icon_32x32.png|32 x 32|
|icon_16x16@2x.png|32 x 32|
|icon_16x16.png|16 x 16|

Aby uzyskać więcej informacji, zobacz firmy Apple [Podaj rozdzielczości wersji z wszystkich aplikacji zasobów graficznych](https://developer.apple.com/library/mac/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Optimizing/Optimizing.html#//apple_ref/doc/uid/TP40012302-CH7-SW3) dokumentacji.


## <a name="packaging-the-icon-resources"></a>Tworzenie pakietów zasobów ikony

Ikona zaprojektowany i zapisywane do nazwy i rozmiary wymaganego pliku Visual Studio for Mac ułatwia przydzielić zasoby obrazów do wykorzystania w Xamarin.Mac.

Wykonaj następujące czynności:

1. W **konsoli rozwiązania**, otwórz **Assets.xcassets** > **AppIcons.appiconset**: 

    ![Edytowanie AppIcons.appiconset](app-icon-images/intro01.png "edycji AppIcons.appiconset")
2. Dla każdego wymaganego rozmiaru ikony kliknij ikonę, a następnie wybierz odpowiedni plik obrazu, które zostały utworzone powyżej: 

    [![Wybieranie obrazu ikony](app-icon-images/intro02.png "Wybieranie obrazu ikony")](app-icon-images/intro02-large.png#lightbox)
3. Zapisz zmiany.


## <a name="using-the-icon"></a>Za pomocą ikony

Raz `AppIcons.appiconset` plik został utworzony, trzeba będzie przypisać je do projektu Xamarin.Mac w programie Visual Studio dla komputerów Mac.

Wykonaj następujące czynności:

1. Kliknij dwukrotnie **Info.plist** w **konsoli rozwiązania** otworzyć **opcje projektu**.
2. W **Mac OS X aplikacji docelowej** sekcji, a następnie kliknij przycisk **ikony aplikacji** wybierz `AppIcons.appiconset` pliku: 

    [![Ustawienie zestawu ikona](app-icon-images/icon01.png "ustawienie zestawu ikon")](app-icon-images/icon01-large.png#lightbox)
3. Zapisz zmiany.

Gdy aplikacja jest uruchamiana, zostanie wyświetlony nową ikonę w doku:

![Przykład ikony aplikacji w macOS dock](app-icon-images/icon04.png "przykładem ikonę aplikacji macOS dock")


## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowy widok w pracy z obrazami wymagane do utworzenia aplikacji macOS ikony, pakowanie ikonę i tym ikonę w projekcie Xamarin.Mac.


## <a name="related-links"></a>Linki pokrewne

- [MacImages (przykład)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Praca z obrazami](~/mac/app-fundamentals/image.md)
- [System macOS Human Interface Guidelines - obrazów i ikon](https://developer.apple.com/macos/human-interface-guidelines/icons-and-images/image-size-and-resolution/)
- [Informacje o wysokiej rozdzielczości dla systemu OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
- [Konstruktor Icns](https://itunes.apple.com/us/app/icns-builder/id554660130?mt=12)
