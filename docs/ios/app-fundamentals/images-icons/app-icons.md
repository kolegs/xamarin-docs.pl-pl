---
title: Ikony aplikacji w rozszerzeniu Xamarin.iOS
description: 'W tym dokumencie opisano sposób pracy z różnymi ikony aplikacji w rozszerzeniu Xamarin.iOS: ikony aplikacji, samego, ikony w centrum uwagi, ikony ustawień i kompozycji iTunes.'
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: cd67c564461721ade6f3eb269b461ddea5e2d2c4
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276005"
---
# <a name="application-icons-in-xamarinios"></a>Ikony aplikacji w rozszerzeniu Xamarin.iOS

Poniższe tematy zostanie omówiona szczegółowo:

* [Aplikacja, uwagi i ikony ustawień](#icon-types) — różne typy ikon wymagane dla aplikacji systemu iOS.
* [Zarządzanie ikony z wykazy zasobów](#managing) — Zarządzanie ikon aplikacji przy użyciu wykazy zasobów.
* [iTunes kompozycji](#itunes) — podając wymagane iTunes kompozycji Ad-Hoc metody dostarczania aplikacji.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>Aplikacji, uwagi i ikony ustawień

W ten sam sposób, że aplikacji platformy Xamarin.iOS zasoby obrazów dla kontrolki interfejsu użytkownika i służy jako ikony dokumentu zasoby obrazów może służyć do zapewnienia ikon aplikacji. Poniższe zrzuty ekranu z tabletu iPad przedstawiono trzy korzysta z ikon w systemie iOS:

- **Ikona aplikacji** — każda aplikacja dla systemu iOS należy zdefiniować ikonę aplikacji. Jest to ikona, który użytkownik naciśnie z ekranu głównego dla systemu iOS, aby uruchomić aplikację. Ponadto ta ikona jest używana przez aplikację Game Center, jeśli ma to zastosowanie. Przykład: 

    [![](app-icons-images/000.png "Ikona aplikacji")](app-icons-images/000-full.png#lightbox)
- **Ikona w centrum uwagi** — zawsze wtedy, gdy użytkownik wprowadza nazwę aplikacji w polu wyszukiwania funkcji Spotlight, jest ona wyświetlana jako ikona. Przykład: 

    [![](app-icons-images/000a.png "Ikona funkcji w centrum uwagi")](app-icons-images/000a-full.png#lightbox)
- **Ikona ustawienia** — Jeśli użytkownik wprowadzi **ustawienia** aplikacji na urządzeniu z systemem iOS, ta ikona pojawi się na końcu **ustawienia** listy aplikacji. Przykład: 

    [![](app-icons-images/000b.png "Ikona ustawienia")](app-icons-images/000b-full.png#lightbox)

Następujących rozmiarów zasobów obrazu i rozwiązania będzie potrzebnych do obsługują wszystkie typy ikon wymagane przez aplikację platformy Xamarin.iOS przeznaczonych dla systemu iOS 5 za pomocą systemu iOS 9 (lub nowszego):

### <a name="iphone-icon-sizes"></a>Rozmiary ikonę telefonu iPhone

- **dla telefonu iPhone: system iOS 9 i 10 (iPhone 6 i 7 oraz)**

    ||3x|
    |---|---|
    |Ikona aplikacji|180x180|
    |Funkcja w centrum uwagi|120x120|
    |Ustawienia|87x87|

- **dla telefonu iPhone: systemów iOS 7 i 8**

    ||1x|2x|
    |---|---|---|
    |Ikona aplikacji|60x60<sup>1</sup>|120x120|
    |Funkcja w centrum uwagi|40x40<sup>2</sup>|80x80|
    |Ustawienia|-|-|

- **dla telefonu iPhone: system iOS 5 i 6**

    ||1x|2x|
    |---|---|---|
    |Ikona aplikacji|57x57|114x114|
    |Funkcja w centrum uwagi|29x29|58x58|
    |Ustawienia|29x29<sup>3, 4</sup>|58x58<sup>3, 4</sup>|

### <a name="ipad-icon-sizes"></a>Rozmiary ikonę urządzenia iPad

- **iPad: system iOS 9 i 10**

    ||2 x (iPad Pro)|
    |---|---|
    |Ikona aplikacji|167x167<sup>6</sup>|
    |Funkcja w centrum uwagi|120x120<sup>6</sup>|
    |Ustawienia|58x58<sup>5</sup>|

- **iPad: systemów iOS 7 i 8**

    ||1x|2x|
    |---|---|---|
    |Ikona aplikacji|76x76|152x152|
    |Funkcja w centrum uwagi|40x40|80x80|
    |Ustawienia|-|-|

- **iPad: system iOS 5 i 6**

    ||1x|2x|
    |---|---|---|
    |Ikona aplikacji|72x72|144x144|
    |Funkcja w centrum uwagi|50x50|100x100|
    |Ustawienia|29x29<sup>3, 5</sup>|58x58<sup>3, 5</sup>|

 1. Zarówno Visual Studio dla komputerów Mac i Xcode nie obsługują już ustawienie 1 obrazu x dla systemu iOS 7.
 2. Ustawienie obrazu 1 x dla systemu iOS 7 nie jest obsługiwana podczas korzystania z zawartości katalogów.
 3. dla systemu iOS 7 i 8 pełnić te same rozmiary obrazu z systemem iOS 5 i 6.
 4. Używa tego samego obrazów i rozmiary jako ikona funkcji Spotlight.
 5. Używa tego samego rozmiaru ikony jako telefon iPhone.
 6. Obsługiwane tylko za pomocą zestawów obrazu katalogu zasobów.
 
 Aby uzyskać więcej informacji na temat ikony Zobacz firmy Apple [ikonę i rozmiary obrazów](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) dokumentacji.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>Zarządzanie ikony z wykazy zasobów

Dla ikon, specjalny `AppIcon` zestaw obrazów mogą być dodawane do `Assets.xcassets` pliku w projekcie aplikacji. Wszystkie wersje obrazu wymagane do obsługi wszystkich rozdzielczościach są uwzględnione w _elementu xcasset_ i zgrupowane. Specjalnego edytora w programie Visual Studio dla komputerów Mac umożliwia deweloperowi do uwzględnienia, a następnie skonfigurować te obrazy graficzne.

Aby użyć katalog zasobów, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji.
2. Przewiń w dół do **ikony aplikacji** sekcji.
3. Z **źródła** listy rozwijanej liście, upewnij się, **AppIcon** wybrano: 

    ![](app-icons-images/migrate01.png "Upewnij się, że jest zaznaczona opcja AppIcon")
4. Z **Eksploratora rozwiązań**, kliknij dwukrotnie `Assets.xcassets` plik, aby otworzyć do edycji: 

    ![](app-icons-images/asset01.png "Plik Assets.xcassets w Eksploratorze rozwiązań")
5. Wybierz `AppIcon` z listy zasobów, aby wyświetlić `Icon Editor`:

    ![](app-icons-images/asset02.png "Edytor AppIcon")
6. Albo kliknij danego typu ikonę i wybierz plik obrazu na wymagany typ/rozmiar lub przeciągnij w obrazie z folderu i upuść je na żądany rozmiar.
7. Kliknij przycisk **Otwórz** przycisk, aby uwzględnić obrazu w projekcie i ustaw go w elementu xcasset.
8. Powtórz te czynności dla wszystkich obrazów wymagane.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij dwukrotnie **Info.plist** w pliku **Eksploratora rozwiązań**:

    ![](app-icons-images/icon01w.png "Wybierz plik Info.plist")
2. Kliknij pozycję **wizualne elementy zawartości** kartę, a następnie kliknij polecenie **użyj katalogu zasobów** przycisku w obszarze **ikony aplikacji**: 

    ![](app-icons-images/icon02w.png "Wybierz kartę wizualnych elementów zawartości")
4. Z **Eksploratora rozwiązań**, rozwiń węzeł **wykaz zasobów** folderu: 

    ![](app-icons-images/image009.png "Rozwiń folder katalogu zasobów")
5. Kliknij dwukrotnie **Media** plik, aby otworzyć go w edytorze: 

    ![](app-icons-images/image010.png "Otwórz plik multimedialny w edytorze")
6. W obszarze **Eksplorator właściwości** Deweloper można wybrać różne typy i rozmiary ikon wymagane.
7. Kliknij danego typu ikonę i wybierz wymagany typ/rozmiar pliku obrazu.
8. Kliknij przycisk **Otwórz** przycisk, aby uwzględnić obrazu w projekcie i ustaw go w elementu xcasset.
9. Powtórz te czynności dla wszystkich obrazów wymagane.

-----

Jest to preferowana metoda tym oraz zarządzaniem nią zasoby obrazów, które będą służyć do zapewnienia ikony aplikacji, uwagi i ustawień dla aplikacji.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Migrowanie z pliku Info.plist wykazów zasobów

Dla istniejących Xamarin.iOS aplikacji za pomocą `Info.plist` plików do zarządzania jej ikony, wysoce zalecane jest, że deweloper przełączyć się go do użycia `AppIcons` zasób obrazu wewnątrz `Assets.xcassets`.

Wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji.
2. Przewiń w dół do **ikony aplikacji** sekcji.
3. Z **źródła** listy rozwijanej wybierz **migracji wykazów zasobów**: 

    ![](app-icons-images/migrate02.png "Wybierz opcję migracji wykazów zasobów")
4. Wszelkie istniejące ikony zdefiniowane w `Info.plist` pliku zostaną zmigrowane do `AppIcons` obrazu zestawie dodane do `Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "Ustaw obraz AppIcons w Assets.xcassets")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji.
2. Kliknij na telefonie iPhone sekcji ikon: 

    ![](app-icons-images/image007.png "Edytor ikon iPhone Rhe")
3. Przewiń w dół do **ikony** sekcji.
4. Z **wykaz zasobów** listy rozwijanej wybierz **Użyj wykazów zasobów**.
5. Wszelkie istniejące ikony zdefiniowane w `Info.plist` pliku zostaną zmigrowane do `Images` dodane do zestawu `Assets.xcassets`.
6. Czy zapisać zmiany `Info.plist` pliku.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes kompozycji

Jeśli przy użyciu zapytań Ad-Hoc metody dostarczania aplikacji (zarówno dla użytkowników firmowych i testowanie wersji beta na rzeczywistych urządzeniach), deweloper musi też to 512 x 512 i obraz 1024 x 1024, która będzie służyć do reprezentowania aplikacji w programie iTunes.

Aby określić iTunes kompozycji, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji.
2. Przewiń do **iTunes kompozycji** części edytora: 

    ![](app-icons-images/itunes01.png "Przewiń do sekcji kompozycji edytora w programie iTunes")
3. Wszystkie brakujące obrazu, kliknij miniaturę w edytorze, wybierz plik obrazu kompozycji żądaną iTunes w oknie dialogowym Otwórz plik i kliknij przycisk **OK** przycisku.
4. Powtórz ten krok, aż wszystkie niezbędne obrazy zostały określone dla aplikacji.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji.

2. Kliknij pozycję **wizualne elementy zawartości** kartę, a następnie rozwiń węzeł **iTunes kompozycji**: 

    ![](app-icons-images/itunes01w.png "Edytowanie iTunes kompozycji w programie Visual Studio")
4. Wszystkie brakujące obrazu, kliknij miniaturę w edytorze, wybierz plik obrazu kompozycji żądaną iTunes w oknie dialogowym Otwórz plik i kliknij przycisk **Otwórz** przycisku.
5. Powtórz ten krok, aż wszystkie niezbędne obrazy zostały określone dla aplikacji.

-----

## <a name="related-links"></a>Linki pokrewne

- [Praca z obrazami (przykład)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Witaj, telefonu iPhone](~/ios/get-started/hello-ios/index.md)
- [Niestandardowa ikona i wytyczne dotyczące tworzenia obrazu](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
