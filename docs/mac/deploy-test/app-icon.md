---
title: Ikona aplikacji dla aplikacji platformy Xamarin.Mac
description: W tym artykule opisano tworzenie obrazów wymagane dla aplikacji platformy Xamarin.Mac ikony, tworzenie pakietów obrazy w pliku .icns i tym ikony w projekcie platformy Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 675b9405-d9a7-49f0-94ad-417f10a71d11
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9c81138aea57c0027ad0f53e3116878ffb800eae
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276044"
---
# <a name="application-icon-for-xamarinmac-apps"></a>Ikona aplikacji dla aplikacji platformy Xamarin.Mac

_W tym artykule opisano tworzenie obrazów wymagane dla aplikacji platformy Xamarin.Mac ikony, tworzenie pakietów obrazy w pliku .icns i tym ikony w projekcie platformy Xamarin.Mac._


## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, deweloper ma dostęp do tego samego obrazu i Ikona narzędzi dla deweloperów pracujących w *języka Objective-C* i *Xcode* jest.

Doskonałe ikona powinna obejmują głównym celem Xamarin.Mac aplikacji i wskazówkę dotyczącą środowiska użytkownika, należy się spodziewać podczas korzystania z aplikacji. W tym artykule opisano wszystkie kroki niezbędne do utworzenia obrazu zasoby wymagane dla ikony, pakowanie, te zasoby do `AppIcon.appiconset` plików i korzystanie z tego pliku w aplikacji platformy Xamarin.Mac.

![Edytor AppIcon.appiconset](app-icon-images/intro01.png "AppIcon.appiconset edytora")


## <a name="application-icon"></a>Ikona aplikacji

Doskonałe ikona powinna obejmują głównym celem Xamarin.Mac aplikacji i wskazówkę dotyczącą środowiska użytkownika, należy się spodziewać podczas korzystania z aplikacji. Każda aplikacja macOS musi zawierać kilku rozmiarów jego ikonę do wyświetlenia w wyszukiwarce, dokowania, Launchpad i innych miejscach w komputerze.


## <a name="designing-the-icon"></a>Projektowanie ikony

Apple sugeruje Poniższe porady, projektując ikony aplikacji:

- Należy rozważyć nadanie Ikona kształtu realistycznego i unikatowy.
- Jeśli aplikację dla systemu macOS ma odpowiednika dla systemu iOS, nie używaj ponownie plików ikony aplikacji dla systemu iOS.
- Użyj aplikacje uniwersalne osób łatwą do rozpoznania.
- Cel: dla uproszczenia.
- Umożliwia koloru i w tle oszczędnie ikona Opowiedz historię aplikacji pomocy.
- Unikaj łączenia operacji faktycznego tekstu za pomocą _zamiast_ tekstu lub wierszy w celu sugerowanie tekstu.
- Utwórz idealny wersję ikony podmiotu, a nie przy użyciu rzeczywistego zdjęć.
- Należy unikać używania elementy interfejsu użytkownika z systemem macOS w ikony.
- Nie należy używać repliki ikony Apple ikon.

Przeczytaj [Galeria ikon aplikacji](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Gallery.html#//apple_ref/doc/uid/20000957-CH88-SW1) i [projektowania ikony aplikacji](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Designing.html#//apple_ref/doc/uid/20000957-CH87-SW1) części firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) przed rozpoczęciem projektowania ikonę aplikacji rozszerzenia Xamarin.Mac.


## <a name="required-image-sizes-and-filenames"></a>Rozmiary obrazów wymagane i nazwy plików

Podobnie jak każdy inny obraz zasób, deweloper ma użyć w aplikacji platformy Xamarin.Mac aplikacja ikonę musi podać wersji Standard i Siatkówka rozwiązania. Ponownie, podobnie jak każdy inny obraz, należy użyć `@2x` formatowania podczas nadawania nazw pliki ikon:

- **Rozdzielczość standardowa**  - _ImageName_**.** _rozszerzenie nazwy pliku_ (przykład: **icon_512x512.png**)
- **O wysokiej rozdzielczości**  - _ImageName_**@2x.** _rozszerzenie nazwy pliku_ (przykład: **icon_512x512@2x.png**)

Na przykład podać 512 x 512 wersję ikona aplikacji, plik będzie nosić **icon_512x512.png** i **icon_512x512@2x.png**.

Aby upewnić się, że ikona wygląda doskonałe we wszystkich miejscach, że użytkownicy będą widzieć je, należy podać zasobów do rozmiarów wymienionych poniżej:

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

Aby uzyskać więcej informacji, zobacz firmy Apple [zapewniają rozdzielczości wersje wszystkich aplikacji graficznych zasobów](https://developer.apple.com/library/mac/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Optimizing/Optimizing.html#//apple_ref/doc/uid/TP40012302-CH7-SW3) dokumentacji.


## <a name="packaging-the-icon-resources"></a>Pakowanie zasobów ikony

Ikona zaprojektowane i zapisane do rozmiaru wymaganego pliku i nazwy Visual Studio dla komputerów Mac ułatwia przydzielić zasoby obrazów do wykorzystania w platformy Xamarin.Mac.

Wykonaj następujące czynności:

1. W **konsoli rozwiązania**, otwórz **Assets.xcassets** > **AppIcons.appiconset**: 

    ![Edytowanie AppIcon.appiconset](app-icon-images/intro01.png "AppIcon.appiconset do edycji")
2. Dla każdego wymaganego rozmiaru ikony kliknij ikonę, a następnie wybierz odpowiedni plik obrazu, które zostały utworzone powyżej: 

    [![Wybieranie obrazu ikony](app-icon-images/intro02.png "Wybieranie obrazu ikony")](app-icon-images/intro02-large.png#lightbox)
3. Zapisz zmiany.


## <a name="using-the-icon"></a>Za pomocą ikony

Po `AppIcon.appiconset` pliku została skompilowana, trzeba będzie ją przypisać go do projektu rozszerzenia Xamarin.Mac w programie Visual Studio dla komputerów Mac.

Wykonaj następujące czynności:

1. Kliknij dwukrotnie **Info.plist** w **konsoli rozwiązania** otworzyć **opcje projektu**.
2. W **cel aplikacji systemu Mac OS X** sekcji, a następnie kliknij przycisk **ikony aplikacji** wybrać `AppIcon.appiconset` pliku: 

    [![Ustawienie zestaw ikon](app-icon-images/icon01.png "ustawienie zestaw ikon")](app-icon-images/icon01-large.png#lightbox)
3. Zapisz zmiany.

Po uruchomieniu aplikacji zostanie wyświetlony nową ikonę w obszarze docka:

![Przykład ikony aplikacji w systemie macOS zadokować](app-icon-images/icon04.png "zadokować przykład ikony aplikacji w systemie macOS")


## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe przyjrzeć się pracy z obrazami, wymagane do utworzenia aplikacji z systemem macOS ikony, pakowanie, ikona i tym ikony w projekcie platformy Xamarin.Mac.


## <a name="related-links"></a>Linki pokrewne

- [MacImages (przykład)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Praca z obrazami](~/mac/app-fundamentals/image.md)
- [System macOS Human Interface Guidelines - obrazów i ikon](https://developer.apple.com/macos/human-interface-guidelines/icons-and-images/image-size-and-resolution/)
- [Informacje o wysokiej rozdzielczości dla systemu OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
- [Konstruktor Icns](https://itunes.apple.com/us/app/icns-builder/id554660130?mt=12)
