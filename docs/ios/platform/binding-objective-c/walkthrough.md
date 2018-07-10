---
title: 'Wskazówki: Powiązywanie biblioteki języka Objective-C dla systemu iOS'
description: Ten artykuł zawiera praktyczne wskazówki dotyczące tworzenia powiązanie interfejsu Xamarin.iOS dla istniejącej biblioteki języka Objective-C, InfColorPicker. Poruszono w nim tematy, takie jak kompilowanie statycznej biblioteki języka Objective-C, powiązanie i za pomocą tego powiązania w aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: 8285a82920f0d95a88855c5257535048c6de41d5
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854860"
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>Wskazówki: Powiązywanie biblioteki języka Objective-C dla systemu iOS

_Ten artykuł zawiera praktyczne wskazówki dotyczące tworzenia powiązanie interfejsu Xamarin.iOS dla istniejącej biblioteki języka Objective-C, InfColorPicker. Poruszono w nim tematy, takie jak kompilowanie statycznej biblioteki języka Objective-C, powiązanie i za pomocą tego powiązania w aplikacji platformy Xamarin.iOS._

Podczas pracy w systemie iOS, mogą wystąpić przypadki, w której chcesz używać innej biblioteki języka Objective-C. W takiej sytuacji można użyć rozszerzenia Xamarin.iOS _powiązania projektu_ utworzyć [powiązania C#](~/cross-platform/macios/binding/overview.md) który umożliwi korzystanie z biblioteki w aplikacji platformy Xamarin.iOS.

Ogólnie należący do ekosystemu z systemem iOS można znaleźć biblioteki w 3 wersjach:

* Jako plik wstępnie skompilowanym biblioteki statycznej o `.a` rozszerzenia wraz z jego nagłówków (.h plików). Na przykład [biblioteki analizy firmy Google](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* Jako platforma wstępnie skompilowane. Jest to folder zawierający bibliotekę statyczną, nagłówki i niektóre dodatkowe zasoby z `.framework` rozszerzenia. Na przykład [biblioteki AdMob firmy Google](https://developers.google.com/admob/ios/download).
* Jako pliki kodu źródłowego. Na przykład, biblioteka zawierająca tylko `.m` i `.h` pliki języka Objective C.

W tym scenariuszu pierwszego i drugiego już będzie wstępnie skompilowanym CocoaTouch biblioteki statycznej, w tym artykule skupimy się na trzeci scenariusz. Należy pamiętać, że przed rozpoczęciem Utwórz powiązanie, Zawsze sprawdzaj licencję określoną za pomocą biblioteki, aby upewnić się, że można powiązać go bezpłatnie.

Ten artykuł zawiera instrukcje krok po kroku przewodnik tworzenia powiązań projektu za pomocą "open source" [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) języka Objective-C projektu jako przykład, jednak wszystkie informacje w tym przewodniku może być dostosowane do użytku z dowolnymi biblioteka języka Objective-C innych firm. Biblioteka InfColorPicker zawiera kontroler widoku wielokrotnego użytku, który umożliwia użytkownikowi wybranie kolor oparty na jego reprezentację HSB, dzięki czemu wybór koloru jest bardziej przyjazny dla użytkownika.

[![](walkthrough-images/run01.png "Przykład biblioteki InfColorPicker uruchomionej w systemie iOS")](walkthrough-images/run01.png#lightbox)

Omówimy wszystkie niezbędne kroki w celu korzystania z tego konkretnego interfejsu API języka Objective-C w rozszerzeniu Xamarin.iOS:

- Najpierw utworzymy biblioteki statycznej języka Objective-C za pomocą środowiska Xcode.
- Firma Microsoft będzie następnie powiązać to biblioteka statyczna z rozszerzeniem Xamarin.iOS.
- Następnie pokazują, jak zmniejszyć obciążenie przez automatyczne generowanie niektóre (ale nie wszystkich) Objective Sharpie niezbędne definicje interfejsu API wymagane przez powiązanie interfejsu Xamarin.iOS.
- Na koniec utworzymy aplikację platformy Xamarin.iOS, która używa powiązania.

Przykładowa aplikacja pokażemy, jak używać silnego delegata do komunikacji między InfColorPicker interfejsu API i kodu języka C#. Po pokazaliśmy już, jak używać silnego delegata, omówimy sposób używania słabe delegatów do wykonania tych samych czynności.

## <a name="requirements"></a>Wymagania

W tym artykule przyjęto założenie, że masz pewną znajomość programu Xcode i języka Objective-C i zapoznaniu się z naszym [powiązań języka Objective-C](~/cross-platform/macios/binding/index.md) dokumentacji. Ponadto wymagane jest spełnienie następujących wykonać czynności przedstawione:

-  **Środowisko Xcode i zestawu iOS SDK** -Xcode firmy Apple i iOS najnowsze interfejsu API muszą zostać zainstalowane i skonfigurowane na komputerze dewelopera.
-  **[Narzędzia wiersza polecenia narzędzia Xcode](#Installing_the_Xcode_Command_Line_Tools)**  -narzędzi Xcode w wierszu polecenia musi być zainstalowany dla obecnie zainstalowanej wersji programu Xcode (poniżej podano szczegóły instalacji).
-  **Program Visual Studio for Mac lub Visual Studio** — najnowszej wersji programu Visual Studio dla komputerów Mac lub Visual Studio powinien być zainstalowany i skonfigurowany na komputerze deweloperskim. Apple Mac jest wymagane do tworzenia aplikacji platformy Xamarin.iOS i przy użyciu programu Visual Studio muszą być podłączone do [hosta kompilacji rozszerzenia Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
-  **Najnowszą wersję Objective Sharpie** — bieżąca kopia narzędzie Objective Sharpie pobrane z [tutaj](~/cross-platform/macios/binding/objective-sharpie/get-started.md). Jeśli masz już Objective Sharpie zainstalowane, można zaktualizować ją do najnowszej wersji przy użyciu `sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Instalowanie narzędzi wiersza polecenia programu Xcode

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Jak wspomniano powyżej, będziemy używać narzędzi wiersza polecenia narzędzia Xcode (w szczególności `make` i `lipo`) w tym przewodniku. `make` Polecenie jest bardzo popularne narzędzia Unix, służący do kompilowania programów wykonywalnych i bibliotek za pomocą _pliku reguł programu make_ określający, jak program powinny zostać skompilowane. `lipo` Polecenia to narzędzie wiersza polecenia OS X do tworzenia wielu architektury plików; łączy wiele `.a` pliki w jednym pliku, który może być używany przez wszystkie architektury sprzętu.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Jak wspomniano powyżej, będziemy używać narzędzi wiersza polecenia narzędzia Xcode na **hosta kompilacji Mac.** (w szczególności `make` i `lipo`) w tym przewodniku. `make` Polecenie jest bardzo popularne narzędzia Unix, służący do kompilowania programów wykonywalnych i bibliotek za pomocą _pliku reguł programu make_ do określa sposób tworzenia programu. `lipo` Polecenia to narzędzie wiersza polecenia OS X do tworzenia wielu architektury plików; łączy wiele `.a` pliki w jednym pliku, który może być używany przez wszystkie architektury sprzętu.


-----

Zgodnie z firmy Apple [tworzenie z wiersza polecenia za pomocą środowiska Xcode — często zadawane pytania](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) dokumentacji, w systemie OS X 10.9 i nowszych, **pliki do pobrania** okienka programu Xcode **preferencje** okna dialogowego nie obsługuje już pobierania narzędzia wiersza polecenia.

Należy użyć jednej z następujących metod do zainstalowania narzędzi:

- **Zainstaluj program Xcode** — po zainstalowaniu środowiska Xcode, oferujemy z Twoimi narzędziami wiersza polecenia. W systemie OS X 10.9 podkładek (zainstalowane w `/usr/bin`), można mapować dowolne narzędzie zawarte w `/usr/bin` do odpowiednich narzędzi w programie Xcode. Na przykład `xcrun` polecenia, dzięki czemu można znaleźć lub uruchomić dowolne narzędzie wewnątrz środowiska Xcode z wiersza polecenia.
- **Aplikacja terminalu** — z poziomu terminalu aplikacji, można zainstalować narzędzi wiersza polecenia, uruchamiając `xcode-select --install` polecenia:
    - Uruchom aplikację Terminal.
    - Typ `xcode-select --install` i naciśnij klawisz **Enter**, na przykład:

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - Użytkownik zostanie zapytany, aby zainstalować narzędzia wiersza polecenia, kliknij przycisk **zainstalować** przycisku: [ ![ ] (walkthrough-images/xcode01.png "instalowania narzędzi wiersza polecenia")](walkthrough-images/xcode01.png#lightbox)

    - Narzędzia zostanie pobrany i zainstalowany z serwerów firmy Apple: [ ![ ] (walkthrough-images/xcode02.png "pobieranie narzędzi")](walkthrough-images/xcode02.png#lightbox)

- **Do pobrania dla deweloperów firmy Apple** — dostępny jest pakiet narzędzi wiersza polecenia [do pobrania dla deweloperów firmy Apple]() strony sieci web. Zaloguj się przy użyciu identyfikatora Apple ID, a następnie wyszukaj i Pobierz narzędzia wiersza polecenia: [ ![ ] (walkthrough-images/xcode03.png "znajdowanie narzędzi wiersza polecenia")](walkthrough-images/xcode03.png#lightbox)

Za pomocą narzędzi wiersza polecenia, zainstalowane możemy przystąpić do, wykonaj instrukcje przedstawione.

## <a name="walkthrough"></a>Przewodnik

W tym przewodniku omówiono następujące czynności:

- **[Utworzyć bibliotekę statycznych](#Creating_A_Static_Library)**  — ten krok obejmuje tworzenia biblioteki statycznej **InfColorPicker** kod języka Objective-C. Biblioteka statyczna będzie miał `.a` rozszerzenie pliku, a zostaną osadzone w zestawie .NET projektu biblioteki.
- **[Utwórz projekt powiązanie interfejsu Xamarin.iOS](#Create_a_Xamarin.iOS_Binding_Project)**  — gdy będziemy już mieć bibliotekę statyczną, użyjemy go do utworzenia projektu powiązanie interfejsu Xamarin.iOS. Projekt powiązania składa się z biblioteki statycznej, którą właśnie utworzyliśmy i metadane w postaci kodu C#, który objaśnia, jak można używać interfejsu API języka Objective-C. Te metadane jest często nazywany definicji interfejsu API. Użyjemy **[Objective Sharpie](#Using_Objective_Sharpie)** aby pomóc nam o Tworzenie definicji interfejsu API.
- **[Normalizuj definicji interfejsu API](#Normalize_the_API_Definitions)**  — Objective Sharpie jest świetnie pomagającym nam, ale nie wszystko. Omówimy pewne zmiany, które należy wprowadzić do definicji interfejsu API, zanim będzie można ich użyć.
- **[Korzystanie z biblioteki powiązania](#Using_the_Binding)**  — na koniec utworzymy aplikację platformy Xamarin.iOS i pokazuje, jak użyć naszego projektu nowo utworzone wiązanie.

Teraz, gdy wiemy, jakie kroki są zaangażowane, przejdźmy do pozostałej części przewodnika.

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>Tworzenie biblioteki statycznej

Jeśli możemy sprawdzić kod dla InfColorPicker w usłudze Github:

[![](walkthrough-images/image02.png "Sprawdź kod dla InfColorPicker w usłudze Github")](walkthrough-images/image02.png#lightbox)

Widzimy trzy następujące katalogi w projekcie:

- **InfColorPicker** -ten katalog zawiera kod języka Objective-C dla projektu.
- **PickerSamplePad** -ten katalog zawiera przykładowy projekt dla urządzenia iPad.
- **PickerSamplePhone** -ten katalog zawiera przykładowy projekt dla telefonu iPhone.

Teraz pobrać projekt InfColorPicker z [GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) i Rozpakuj go w katalogu naszych wyboru. Otwierając cel Xcode `PickerSamplePhone` projektu, widzimy następującą strukturę projektu w oknie Nawigator Xcode:

[![](walkthrough-images/image03.png "Struktura projektu w oknie Nawigator Xcode")](walkthrough-images/image03.png#lightbox)

Ten projekt realizuje ponownego użycia kodu przez bezpośrednie dodanie kodu źródłowego InfColorPicker (czerwonym prostokątem) na każdy projekt przykładu. Kod przykładowy projekt jest w niebieskim polu. Ponieważ tego konkretnego projektu nie zapewnia nam z biblioteką statyczną, niezbędne jest, aby nas utworzyć projekt Xcode do kompilowania biblioteki statycznej.

Pierwszym krokiem jest dla nas Dodawanie kodu źródłowego InfoColorPicker do biblioteki statycznej. Teraz można to osiągnąć, wykonaj następujące czynności:

1. Uruchom program Xcode.
2. Z **pliku** menu wybierz opcję **New** > **projektu...** :

    [![](walkthrough-images/image04.png "Uruchamianie nowego projektu")](walkthrough-images/image04.png#lightbox)
3. Wybierz **Framework bib & Liotekę**, **Cocoa Touch biblioteki statycznej** szablon i kliknij przycisk **dalej** przycisku:

    [![](walkthrough-images/image05.png "Wybierz szablon, Cocoa Touch biblioteki statycznej")](walkthrough-images/image05.png#lightbox)

4. Wprowadź `InfColorPicker` dla **Nazwa projektu** i kliknij przycisk **dalej** przycisku:

    [![](walkthrough-images/image06.png "Wprowadź nazwę projektu InfColorPicker")](walkthrough-images/image06.png#lightbox)
5. Wybierz lokalizację do zapisania projektu, a następnie kliknij przycisk **OK** przycisku.
6. Teraz należy dodać źródła z projektu InfColorPicker do naszego projektu biblioteki statycznej. Ponieważ **InfColorPicker.h** plik już istnieje w naszym biblioteka statyczna (domyślnie), programu Xcode nie umożliwi nam go zastąpić. Z **wyszukiwania**, nawigowanie do kodu źródłowego InfColorPicker w oryginalnego projektu, który możemy rozpakowano z serwisu GitHub, skopiuj wszystkie pliki InfColorPicker i wklej je do naszego nowego projektu biblioteki statycznej:

    [![](walkthrough-images/image12.png "Skopiuj wszystkie pliki InfColorPicker")](walkthrough-images/image12.png#lightbox)

7. Wróć do programu Xcode, kliknij prawym przyciskiem myszy **InfColorPicker** i wybierz polecenie **Dodaj pliki do "InfColorPicker..."**:

    [![](walkthrough-images/image08.png "Trwa dodawanie plików")](walkthrough-images/image08.png#lightbox)

8. W oknie dialogowym Dodaj pliki, przejdź do plików kodu źródłowego InfColorPicker, które właśnie został skopiowany, zaznacz je i kliknij **Dodaj** przycisku:

    [![](walkthrough-images/image09.png "Zaznacz wszystko, a następnie kliknij przycisk Dodaj")](walkthrough-images/image09.png#lightbox)

9. Kod źródłowy zostaną skopiowane do naszego projektu:

    [![](walkthrough-images/image10.png "Kod źródłowy zostanie skopiowany do projektu")](walkthrough-images/image10.png#lightbox)

10. Wybierz z Nawigatora projektu Xcode **InfColorPicker.m** pliku i Skomentuj ostatnie dwa wiersze (ze względu na sposób tej biblioteki zostały zapisane, ten plik nie jest używany):

    [![](walkthrough-images/image14.png "Edytowanie pliku InfColorPicker.m")](walkthrough-images/image14.png#lightbox)

11. Teraz należy sprawdzić, czy wszystkie struktury wymagane przez bibliotekę. Te informacje można znaleźć w pliku README lub otwierając jedną z przykładowych projektów, pod warunkiem. W tym przykładzie użyto `Foundation.framework`, `UIKit.framework`, i `CoreGraphics.framework` Dodajmy je.

12. Wybierz **InfColorPicker docelowy > fazy kompilacji** i rozwiń **binarny z bibliotekami** sekcji:

    [![](walkthrough-images/image16b.png "Rozwiń sekcję binarny z bibliotekami")](walkthrough-images/image16b.png#lightbox)

13. Użyj **+** przycisk, aby otworzyć okno dialogowe, umożliwiające dodanie struktury wymagane ramek wymienionych powyżej:

    [![](walkthrough-images/image16c.png "Dodaj struktur wymagane ramek wymienionych powyżej")](walkthrough-images/image16c.png#lightbox)

14. **Binarny z bibliotekami** sekcja powinna teraz wyglądać podobnie jak na poniższej ilustracji:

    [![](walkthrough-images/image16d.png "W sekcji binarny z bibliotekami")](walkthrough-images/image16d.png#lightbox)

W tym momencie możemy Zamknij, ale firma Microsoft nie są jeszcze gotowe. Biblioteka statyczna została utworzona, ale musimy skompiluj go, aby utworzyć systemu plików Fat binarny, który zawiera wszystkie wymagane architektury symulatora systemu iOS i urządzenia z systemem iOS.

### <a name="creating-a-fat-binary"></a>Tworzenie Fat pliku binarnego

Wszystkie urządzenia z systemem iOS procesorami obsługiwane przez architekturę ARM, które opracowali wraz z upływem czasu. Każdej nowej architektury dodano nowych instrukcji i inne ulepszenia przy jednoczesnym zachowaniu wstecznej zgodności. Na urządzeniach z systemem iOS mamy armv6, armv7, armv7s, zestawy instrukcji arm64 — mimo że [firma Microsoft nie są już używane armv6](~/ios/deploy-test/compiling-for-different-devices.md). Symulator systemu iOS nie jest obsługiwana przez ARM i istead x86 oraz x86_64 obsługiwane symulatora. Firma Microsoft, które oznacza, że dla nas jest, że firma Microsoft zapewnia bibliotek na każdą instrukcję ustawia.

Biblioteka Fat jest `.a` plik zawierający wszystkie obsługiwane architektury.

Tworzenie binarnego systemu plików fat jest procesem trzech etapów:

- Kompilowanie biblioteki statycznej w wersji ARM 7 i ARM64.
- Skompiluj x86 i x84_64 wersję biblioteki statycznej.
- Użyj `lipo` narzędzia wiersza polecenia, aby połączyć dwie biblioteki statycznej w jeden.

Chociaż te trzy kroki są dość proste i może być konieczne powtarzanie ich w przyszłości, gdy biblioteka języka Objective-C odbiera aktualizacje lub jeśli firma Microsoft wymaga poprawki. Jeśli zdecydujesz zautomatyzować te kroki, uprości przyszłych konserwacji i obsługi projekt powiązania dla systemu iOS.

Istnieje wiele narzędzi służących do automatyzowania takiego zadania — skrypt powłoki, [nachylenia](http://rake.rubyforge.org/), [xbuild](http://www.mono-project.com/docs/tools+libraries/tools/xbuild/), i [wprowadzić](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html). Zainstalowanej narzędzi wiersza polecenia programu Xcode, możemy również instalowany upewnij, wtedy system kompilacji, który będzie używany w ramach tego przewodnika. Oto **pliku reguł programu make** używanego do utworzenia wielu architektura biblioteki udostępnionej, który będzie działać na urządzeniu z systemem iOS i symulatora dla dowolnej biblioteki:

```bash
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=./YOUR-PROJECT-NAME
PROJECT=$(PROJECT_ROOT)/YOUR-PROJECT-NAME.xcodeproj
TARGET=YOUR-PROJECT-NAME

all: lib$(TARGET).a

lib$(TARGET)-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

lib$(TARGET)-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET)-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET).a: lib$(TARGET)-i386.a lib$(TARGET)-armv7.a lib$(TARGET)-arm64.a
    xcrun -sdk iphoneos lipo -create -output $@ $^

clean:
    -rm -f *.a *.dll
```

Wprowadź **pliku reguł programu make** poleceń w edytorze zwykły tekst z wybraniu i zaktualizuj sekcje z **YOUR PROJECT nazwy** nazwą projektu. Jest również ważne, aby upewnić się, że firma Microsoft Wklej instrukcje powyżej, kartach na instrukcje zostały zachowane.

Zapisz plik o nazwie **pliku reguł programu make** do tej samej lokalizacji co biblioteka statyczna środowiska Xcode InfColorPicker utworzone powyżej:

[![](walkthrough-images/lib00.png "Zapisz plik o nazwie pliku reguł programu make")](walkthrough-images/lib00.png#lightbox)

Otwórz aplikację Terminal na komputerze Mac, a następnie przejdź do lokalizacji Twojego pliku reguł programu make. Typ `make` w terminalu, naciśnij klawisz **Enter** i **pliku reguł programu make** zostaną wykonane:

[![](walkthrough-images/lib01.png "Przykładowe dane wyjściowe z pliku reguł programu make")](walkthrough-images/lib01.png#lightbox)

Po uruchomieniu upewnij, będzie widocznych wiele przewijanie przez tekstu. Jeśli wszystko działało poprawnie, zostanie wyświetlone zostaną słowa **kompilacji zakończyło się pomyślnie** i `libInfColorPicker-armv7.a`, `libInfColorPicker-i386.a` i `libInfColorPickerSDK.a` pliki zostaną skopiowane do tej samej lokalizacji co **pliku reguł programu make**:

[![](walkthrough-images/lib02.png "Pliki libInfColorPicker armv7.a, libInfColorPicker i386.a i libInfColorPickerSDK.a, generowane przez pliku reguł programu make")](walkthrough-images/lib02.png#lightbox)

Architektury można sprawdzić w ramach Fat plik binarny, za pomocą następującego polecenia:

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

Powinien być wyświetlany poniżej:

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

W tym momencie możemy ukończono pierwszym krokiem w naszej powiązania z systemem iOS, tworząc bibliotekę statyczną, za pomocą środowiska Xcode i narzędzia wiersza polecenia narzędzia Xcode `make` i `lipo`. Teraz przejdź do następnego kroku i użyj **Objective Sharpie** do zautomatyzowania tworzenia powiązań interfejsu API dla nas.

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>Tworzenie rozszerzenia Xamarin.iOS projekt powiązania

Firma Microsoft może korzystać z **Objective Sharpie** Aby zautomatyzować proces wiązania, musimy utworzyć projekt powiązanie interfejsu Xamarin.iOS do przechowywania definicji interfejsu API (który będziemy używać **Objective Sharpie** nam pomóc Kompilacja) i utworzyć powiązanie C# dla nas.

Teraz należy wykonać następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom program Visual Studio dla komputerów Mac.
1. Z **pliku** menu, wybierz opcję **New** > **rozwiązania...** :

    ![](walkthrough-images/bind01.png "Uruchamianie nowego rozwiązania")

1. W oknie dialogowym nowe rozwiązanie, wybierz **biblioteki** > **powiązań projektu systemu iOS**:

    ![](walkthrough-images/bind02.png "Wybierz powiązanie projektu systemu iOS")

1. Kliknij przycisk **dalej** przycisku.

1. Wprowadź "InfColorPickerBinding" jako **Nazwa projektu** i kliknij przycisk **Utwórz** przycisk, aby utworzyć rozwiązanie:

    ![](walkthrough-images/bind02a.png "Wprowadź InfColorPickerBinding jako nazwę projektu")

Rozwiązanie zostanie utworzony i dwa domyślne pliki zostaną dołączone:

![](walkthrough-images/bind03.png "Struktura rozwiązania w Eksploratorze rozwiązań")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. Uruchom program Visual Studio.

1. Z **pliku** menu, wybierz opcję **New** > **projektu...** :

    ![Uruchamianie nowego projektu](walkthrough-images/bind01vs.png "rozpoczynaniu nowego projektu")

1. W oknie dialogowym Nowy projekt, wybierz **Visual C# > iPhone & iPad > Biblioteka powiązań (Xamarin) systemu iOS**:

    [![Wybierz Biblioteka powiązań systemu iOS](walkthrough-images/bind02.w157-sml.png)](walkthrough-images/bind02.w157.png#lightbox)

1. Wprowadź "InfColorPickerBinding" jako **nazwa** i kliknij przycisk **OK** przycisk, aby utworzyć rozwiązanie.

Rozwiązanie zostanie utworzony i dwa domyślne pliki zostaną dołączone:

![](walkthrough-images/bind03vs.png "Struktura rozwiązania w Eksploratorze rozwiązań")

-----

- **ApiDefinition.cs** — ten plik będzie zawierać kontrakty, które definiują, jak API języka Objective-C zostaną opakowane w języku C#.
- **Structs.cs** — ten plik będzie zawierać wszystkie struktury lub wartości wyliczenia, które są wymagane przez interfejsy i delegaty.

Firma Microsoft będzie działać przy użyciu tych plików w dalszej części przewodnika. Najpierw należy dodać biblioteki InfColorPicker do powiązania projektu.

### <a name="including-the-static-library-in-the-binding-project"></a>Łącznie z biblioteką statyczną w projekcie powiązania

Teraz mamy nasz powiązań projektu podstawowego gotowe, należy dodać biblioteki Fat binarne utworzone powyżej dla **InfColorPicker** biblioteki.

Wykonaj następujące kroki, aby dodać bibliotekę:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij prawym przyciskiem myszy **odwołania natywne** folderu w konsoli rozwiązania, a następnie wybierz pozycję **Dodaj odwołania natywne**:

    ![](walkthrough-images/bind04a.png "Dodawanie odwołania natywne")

1. Przejdź do Fat Binary wprowadziliśmy wcześniej (`libInfColorPickerSDK.a`) i naciśnij klawisz **Otwórz** przycisku:

    ![](walkthrough-images/bind05.png "Wybierz plik libInfColorPickerSDK.a")
1. Plik zostanie uwzględniony w projekcie:

    ![](walkthrough-images/bind04.png "W tym pliku")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kopiowanie `libInfColorPickerSDK.a` z Twojej **hosta kompilacji Mac.** i wkleić go do projektu powiązania.

1. Kliknij prawym przyciskiem myszy nad projektem i wybierz polecenie **Dodaj > istniejący element...** :

    ![](walkthrough-images/bind04vs.png "Dodawanie istniejącego pliku")

1. Przejdź do `libInfColorPickerSDK.a` i naciśnij klawisz **Dodaj** przycisku:

    ![](walkthrough-images/bind05vs.png "Dodawanie libInfColorPickerSDK.a")

1. Plik zostanie uwzględniony w projekcie.

-----

Gdy **.a** plik zostanie dodany do projektu, automatycznie ustawi Xamarin.iOS **Akcja kompilacji** pliku do **ObjcBindingNativeLibrary**i Utwórz plik specjalny wywołuje się `libInfColorPickerSDK.linkwith.cs`.


Ten plik zawiera `LinkWith` atrybut, który informuje Xamarin.iOS jak dojście do biblioteki statycznej, który możemy właśnie dodane. Zawartość tego pliku są wyświetlane w poniższy fragment kodu:

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

`LinkWith` Atrybut identyfikuje biblioteki statycznej dla projektu i niektórych flag konsolidatora ważne.


Kolejną rzeczą, jakiej musimy się tworzenie definicji interfejsu API dla projektu InfColorPicker. Dla celów tego instruktażu użyjemy Objective Sharpie, aby wygenerować plik **ApiDefinition.cs**.

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>Za pomocą narzędzie Objective Sharpie

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Narzędzie Objective Sharpie znajduje się w wierszu polecenia narzędzia (udostępnione przez: Xamarin), która może pomóc w utworzeniu definicjami wymaganej do powiązania 3 biblioteki języka Objective-C innej firmy do języka C#. W tej sekcji użyjemy Objective Sharpie, aby utworzyć początkowy **ApiDefinition.cs** InfColorPicker projektu.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Narzędzie Objective Sharpie znajduje się w wierszu polecenia narzędzia (udostępnione przez: Xamarin), która może pomóc w utworzeniu definicjami wymaganej do powiązania 3 biblioteki języka Objective-C innej firmy do języka C#. W tej sekcji użyjemy Objective Sharpie na naszych **hosta kompilacji Mac.** do utworzenia początkowego **ApiDefinition.cs** InfColorPicker projektu.


-----

Aby rozpocząć pracę, Pobierz teraz plik Instalatora Objective Sharpie zgodnie z opisem w tym [przewodnik](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing). Uruchom Instalatora i wykonaj wszystkie polecenia na ekranie Kreatora instalacji, aby zainstalować Objective Sharpie na nasz komputer deweloperski.

Po pomyślnym mamy Objective Sharpie teraz zainstalowany, uruchom aplikację Terminal i wprowadź następujące polecenie, aby uzyskać pomoc dotyczącą wszystkie narzędzia, które zapewnia do pomocy w powiązaniu:

```bash
sharpie -help
```

Jeśli firma Microsoft wykonane powyższe polecenie, zostanie wygenerowany następujące dane wyjściowe:

```bash
Europa:Resources kmullins$ sharpie -help
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, --helpShow detailed help
  -v, --versionShow version information

Available Tools:

  xcode    Get information about Xcode installations and available SDKs.

  bind     Create a Xamarin C# binding to Objective-C APIs
Europa:Resources kmullins$
```

Na potrzeby tego przewodnika firma Microsoft będzie można korzystać z następujących narzędzi Objective Sharpie:

- **środowisko xcode** — to narzędzia daje nam informacje o naszej bieżącej instalacji programu Xcode i wersje dla systemów iOS i Mac interfejsów API, które firma Microsoft została zainstalowana. Firma Microsoft będzie używać tych informacji w dalszej części gdy firma Microsoft generuje naszych powiązania.
- **Powiąż** -użyjemy tego narzędzia można przeanalizować **.h** pliki w projekcie InfColorPicker do początkowej **ApiDefinition.cs** i **StructsAndEnums.cs** plików.

Aby uzyskać pomoc dotyczącą określonego narzędzie Objective Sharpie, wprowadź nazwę narzędzia i `-help` opcji. Na przykład `sharpie xcode -help` zwraca następujące wyniki:

```bash
Europa:Resources kmullins$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]+

Options:
  -h, --help                 Show detailed help
  -v, --verbose              Be verbose with output
      --sdks                 List all available Xcode SDKs. Pass -verbose for
                               more details.
Europa:Resources kmullins$
```

Zanim firma będzie mogła rozpocząć proces wiązania, potrzebujemy uzyskać informacje o naszej bieżącej zainstalowanych zestawów SDK, wprowadzając następujące polecenie w terminalu `sharpie xcode -sdks`:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

Znajdującą się powyżej, okaże się, że mamy `iphoneos9.3` zestaw SDK zainstalowany na naszą maszynę. Dzięki tym informacjom w miejscu, możemy przystąpić do analizowania projektów InfColorPicker `.h` pliki do początkowej **ApiDefinition.cs** i `StructsAndEnums.cs` InfColorPicker projektu.

Wprowadź następujące polecenie w terminalu aplikacji:

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

Gdzie `[full-path-to-project]` jest pełną ścieżką do katalogu, gdzie **InfColorPicker** plik projektu Xcode znajduje się na nasz komputer, a [system operacyjny telefonu iphone] jest zestawu iOS SDK, które zainstalowały firma Microsoft, jak wspomniano `sharpie xcode -sdks` polecenia. Należy pamiętać, że w tym przykładzie firma Microsoft zrealizowaniu  **\*.h** jako parametru, która obejmuje *wszystkich* pliki nagłówkowe, w tym katalogu — zwykle użytkownik powinien nie to zrobić, ale zamiast tego należy dokładnie zapoznaj się z artykułem pliki nagłówkowe, aby znaleźć najwyższego poziomu **.h** pliku, który odwołuje się wszystkie inne odpowiednie pliki i po prostu przekazać ją do Objective Sharpie.

Następujące [dane wyjściowe](walkthrough-images/os05.png) zostanie wygenerowany w terminalu:

```bash
Europa:Resources kmullins$ sharpie bind -output InfColorPicker -namespace InfColorPicker -sdk iphoneos8.1 /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h -unified
Compiler configuration:
    -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk -miphoneos-version-min=8.1 -resource-dir /Library/Frameworks/ObjectiveSharpie.framework/Versions/1.1.1/clang-resources -arch armv7 -ObjC

[  0%] parsing /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h
In file included from /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h:60:
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* sourceColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* resultColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
4 warnings generated.
[100%] parsing complete
[bind] InfColorPicker.cs
Europa:Resources kmullins$
```

I **InfColorPicker.enums.cs** i **InfColorPicker.cs** pliki zostaną utworzone w naszym katalogu:

[![](walkthrough-images/os06.png "Pliki InfColorPicker.enums.cs i InfColorPicker.cs")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Otwórz oba te pliki w projekcie powiązania, które utworzyliśmy powyżej. Skopiuj zawartość **InfColorPicker.cs** pliku i wklej go do **ApiDefinition.cs** pliku, zastępując istniejące `namespace ...` bloku kodu z zawartością  **InfColorPicker.cs** pliku (pozostawiając `using` instrukcji nienaruszony):

![](walkthrough-images/os07.png "Plik InfColorPickerControllerDelegate")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Otwórz oba te pliki w projekcie powiązania, które utworzyliśmy powyżej. Skopiuj zawartość **InfColorPicker.cs** pliku (z **hosta kompilacji Mac.**) i wklej go w **ApiDefinition.cs** pliku, zastępując istniejące `namespace ...` blok kodu z zawartością **InfColorPicker.cs** pliku (pozostawiając `using` instrukcji bez zmian).


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>Normalizuj definicji interfejsu API

Narzędzie Objective Sharpie czasami ma problem tłumaczenia `Delegates`, dlatego firma Microsoft będzie należy zmodyfikować definicję `InfColorPickerControllerDelegate` interfejsu i Zastąp `[Protocol, Model]` wiersz poniższym kodem:

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
Tak, aby definicji wygląda następująco:

[![](walkthrough-images/os11.png "Definicja")](walkthrough-images/os11.png#lightbox)

Następnie możemy zrobić to samo z zawartością `InfColorPicker.enums.cs` plik, kopiując i wklejając je w `StructsAndEnums.cs` pliku, pozostawiając `using` instrukcji bez zmian:

[![](walkthrough-images/os09.png "Zawartość StructsAndEnums.cs pliku ")](walkthrough-images/os09.png#lightbox)

Możesz również stwierdzić, że Objective Sharpie ma adnotację powiązanie o `[Verify]` atrybutów. Te atrybuty wskazują, że należy sprawdzić, czy Objective Sharpie czy poprawny element porównując powiązania z oryginalnej deklaracji Objective-C-C (który będzie świadczona w komentarzu powyżej deklaracji powiązanej). Po upewnieniu się powiązań, należy usunąć atrybut Sprawdź. Aby uzyskać więcej informacji, zobacz [Sprawdź](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) przewodnik.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


W tym momencie nasze powiązania projektu należy gotowy do kompilacji. Teraz naszych rojekt powiązania i upewnij się, że firma Microsoft zakończenie bez błędów:

[Skompiluj projekt, powiązania i upewnij się, że nie ma żadnych błędów](walkthrough-images/os12.png)


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


W tym momencie nasze powiązania projektu należy gotowy do kompilacji. Umożliwia naszych rojekt powiązania i upewnij się, że firma Microsoft zakończenie bez błędów.


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>Za pomocą tego powiązania

Wykonaj te kroki tworzenia przykładowej aplikacji dla telefonu iPhone, do użycia z systemem iOS, powiązanie biblioteki utworzony powyżej:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Tworzenie projektu platformy Xamarin.iOS** — dodać nowego projektu platformy Xamarin.iOS o nazwie **InfColorPickerSample** do rozwiązania, jak pokazano na poniższych zrzutach ekranu:

    ![](walkthrough-images/use01.png "Dodawanie aplikacji pojedynczego widoku")

    ![](walkthrough-images/use01a.png "Ustawienie identyfikatora")

1. **Dodaj odwołanie do projektu powiązanie** — aktualizacja **InfColorPickerSample** projektu, dzięki czemu ma odwołanie do **InfColorPickerBinding** projektu:

    ![](walkthrough-images/use02.png "Dodawanie odwołania do projektu powiązania")

1. **Tworzenie interfejsu użytkownika dla telefonu iPhone** — dwukrotne kliknięcie **MainStoryboard.storyboard** w pliku **InfColorPickerSample** projekt, aby edytować go w narzędziu iOS Designer. Dodaj **przycisk** do widoku i nadać mu `ChangeColorButton`, jak pokazano poniżej:

    ![](walkthrough-images/use03.png "Dodawanie przycisku do widoku")

1. **Dodaj InfColorPickerView.xib** — Biblioteka InfColorPicker Objective-C obejmuje **.pliki** pliku. Rozszerzenie Xamarin.iOS nie obejmuje to **.pliki** w projekcie powiązania, co spowoduje błędy czasu wykonywania w naszej przykładowej aplikacji. Obejść ten problem jest dodanie **.pliki** pliku do naszego projektu Xamarin.iOS. Wybierz projekt interfejsu Xamarin.iOS, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Dodaj pliki**i Dodaj **.pliki** pliku, jak pokazano na poniższym zrzucie ekranu:

    ![](walkthrough-images/use04.png "Dodaj InfColorPickerView.xib")

1. Po wyświetleniu monitu, skopiuj **.pliki** plik do projektu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **Tworzenie projektu platformy Xamarin.iOS** — dodać nowego projektu platformy Xamarin.iOS o nazwie **InfColorPickerSample** przy użyciu **aplikacja pojedynczego widoku** szablonu:

    [![Projekt aplikacji (Xamarin) dla systemu iOS](walkthrough-images/use01.w157-sml.png)](walkthrough-images/use01.w157.png#lightbox)

    [![Wybierz szablon](walkthrough-images/use01-2.w157-sml.png)](walkthrough-images/use01-2.w157.png#lightbox)

1. **Dodaj odwołanie do projektu powiązanie** — aktualizacja **InfColorPickerSample** projektu, dzięki czemu ma odwołanie do **InfColorPickerBinding** projektu:

    ![](walkthrough-images/use02vs.png "Dodaj odwołanie do projektu powiązania")

1. **Tworzenie interfejsu użytkownika dla telefonu iPhone** — dwukrotne kliknięcie **MainStoryboard.storyboard** w pliku **InfColorPickerSample** projekt, aby edytować go w narzędziu iOS Designer. Dodaj **przycisk** do widoku i nadać mu `ChangeColorButton`, jak pokazano poniżej:

    ![](walkthrough-images/use03vs.png "Tworzenie interfejsu użytkownika dla telefonu iPhone")

1. **Dodaj InfColorPickerView.xib** — Biblioteka InfColorPicker Objective-C obejmuje **.pliki** pliku. Rozszerzenie Xamarin.iOS nie obejmuje to **.pliki** w projekcie powiązania, co spowoduje błędy czasu wykonywania w naszej przykładowej aplikacji. Obejść ten problem jest dodanie **.pliki** pliku do naszego projektu interfejsu Xamarin.iOS z naszych **hosta kompilacji Mac**. Wybierz projekt interfejsu Xamarin.iOS, kliknij prawym przyciskiem myszy i wybierz **Dodaj** > **istniejący element...** i Dodaj **.pliki** pliku.

-----

Następnie rzućmy rzut oka na protokoły języka Objective-C i jak chronimy je w powiązania i kodu C#.

### <a name="protocols-and-xamarinios"></a>Protokoły i rozszerzenia Xamarin.iOS

W języku Objective-C, to protokół definiuje metody (lub wiadomości), mogą być używane w pewnych okolicznościach. Model są bardzo podobne do interfejsów w języku C#. Jedną główną różnica pomiędzy protokołu języka Objective-C i interfejs C# jest protokołów można opcjonalnie metody - metod, których nie ma klasy do zaimplementowania. Używa języka Objective-C @optional — słowo kluczowe jest używany do wskazania, które metody są opcjonalne. Aby uzyskać więcej informacji na temat protokołów zobacz [zdarzenia, protokoły i delegaci](~/ios/app-fundamentals/delegates-protocols-and-events.md).

**InfColorPickerController** ma jeden taki protokół, pokazano we fragmencie kodu poniżej:

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

Protokół ten jest używany przez **InfColorPickerController** informowanie klientów, że użytkownik pobrał nowy kolor i że **InfColorPickerController** zostało zakończone. Narzędzie Objective Sharpie zamapowane tego protokołu, jak pokazano w poniższym fragmencie kodu:

```csharp
[BaseType(typeof(NSObject))]
[Model]
public partial interface InfColorPickerControllerDelegate {

    [Export ("colorPickerControllerDidFinish:")]
    void ColorPickerControllerDidFinish (InfColorPickerController controller);

    [Export ("colorPickerControllerDidChangeColor:")]
    void ColorPickerControllerDidChangeColor (InfColorPickerController controller);
}

```

Podczas kompilowania biblioteki powiązanie interfejsu Xamarin.iOS utworzy abstrakcyjna klasa bazowa o nazwie `InfColorPickerControllerDelegate`, który implementuje ten interfejs za pomocą metod wirtualnych.

Istnieją dwa sposoby, że w aplikacji platformy Xamarin.iOS można zaimplementować ten interfejs:

- **Delegowanie silne** — przy użyciu silnego delegata związane z utworzeniem klasy C# podklasy `InfColorPickerControllerDelegate` i zastępuje właściwe metody. **InfColorPickerController** użyje wystąpienia tej klasy do komunikowania się z jej klientów.
- **Słabe delegata** -słabe delegat jest nieco inna technika, która obejmuje utworzenie publiczną metodę w klasie niektóre (takie jak `InfColorPickerSampleViewController`) i następnie udostępnianie tej metody do `InfColorPickerDelegate` protokołu za pomocą `Export` atrybutu.

Silne delegaty zapewniają funkcji Intellisense, bezpieczeństwo typów i lepiej hermetyzacji. Z tego względu należy używać silnych delegatów gdzie to możliwe, zamiast słabe delegata.

W tym przewodniku omówimy obu tych technik: najpierw Implementowanie delegata silne i wyjaśniający, jak zaimplementować delegata słabe.

### <a name="implementing-a-strong-delegate"></a>Implementowanie delegata silne

Zakończ aplikację platformy Xamarin.iOS przy użyciu silnego delegata odpowiedzieć na `colorPickerControllerDidFinish:` komunikat:

**Podklasy InfColorPickerControllerDelegate** — Dodaj nową klasę do projektu o nazwie `ColorSelectedDelegate`. Edytuj klasy tak, aby miało następujący kod:

```csharp
using InfColorPickerBinding;
using UIKit;

namespace InfColorPickerSample
{
    public class ColorSelectedDelegate:InfColorPickerControllerDelegate
    {
        readonly UIViewController parent;

        public ColorSelectedDelegate (UIViewController parent)
        {
            this.parent = parent;
        }

        public override void ColorPickerControllerDidFinish (InfColorPickerController controller)
        {
            parent.View.BackgroundColor = controller.ResultColor;
            parent.DismissViewController (false, null);
        }
    }
}
```

Xamarin.iOS powiąże delegata języka Objective-C, tworząc abstrakcyjna klasa bazowa o nazwie `InfColorPickerControllerDelegate`. Podklasy danego typu i zastąpienie `ColorPickerControllerDidFinish` metodę, aby uzyskać dostęp do wartości `ResultColor` właściwość `InfColorPickerController`.

**Utwórz wystąpienie ColorSelectedDelegate** -nasz program obsługi zdarzeń musi wystąpienie `ColorSelectedDelegate` typu, które zostały utworzone w poprzednim kroku. Edytuj klasy `InfColorPickerSampleViewController` i dodaj następującą zmienną wystąpienia klasy:

```csharp
ColorSelectedDelegate selector;
```

**Inicjowanie zmiennej ColorSelectedDelegate** — aby upewnić się, że `selector` jest prawidłowym wystąpienia, zaktualizuj metodę `ViewDidLoad` w `ViewController` aby dopasować poniższy fragment kodu:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
    selector = new ColorSelectedDelegate (this);
}
```
**Zaimplementuj metodę HandleTouchUpInsideWithStrongDelegate** — obok implementacji programu obsługi zdarzeń po użytkownik dotyka **ColorChangeButton**. Edytuj `ViewController`i dodaj następującą metodę:

```csharp
using InfColorPicker;
...

private void HandleTouchUpInsideWithStrongDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.Delegate = selector;
    picker.PresentModallyOverViewController (this);
}

```

Firma Microsoft najpierw uzyskać wystąpienie `InfColorPickerController` za pośrednictwem metody statycznej i czy wystąpienia świadomość naszych silne delegata za pomocą właściwości `InfColorPickerController.Delegate`. Ta właściwość została wygenerowana automatycznie dla nas przez Objective Sharpie. Na koniec nazywamy `PresentModallyOverViewController` można wyświetlić widoku `InfColorPickerSampleViewController.xib` tak, aby użytkownik może wybrać kolor.

**Uruchom aplikację** — na tym etapie, wszystko jest gotowe ze wszystkimi naszego kodu. Jeśli aplikacja zostanie uruchomiona, powinno być możliwe zmienić kolor tła `InfColorColorPickerSampleView` jak pokazano na poniższych zrzutach ekranu:

[![](walkthrough-images/run01.png "Uruchamianie aplikacji")](walkthrough-images/run01.png#lightbox)

Gratulacje! W tym momencie została pomyślnie utworzone i powiązane biblioteki języka Objective-C do użycia w aplikacji platformy Xamarin.iOS. Następnie Przyjrzyjmy się przy użyciu delegatów słabe.

### <a name="implementing-a-weak-delegate"></a>Implementacja słabego delegata

Zamiast Tworzenie podklasy klasy powiązane z protokołu języka Objective-C w przypadku określonych pełnomocników, Xamarin.iOS pozwala także implementować metody protokołu w każdej klasy, która pochodzi od klasy `NSObject`, urządzanie metody z `ExportAttribute`, a następnie podanie odpowiednich selektorów. Podczas takiego podejścia można przypisać wystąpienie klasy do `WeakDelegate` zamiast do właściwości `Delegate` właściwości. Słabe delegata oferuje elastyczność wykonaj klasy delegata w dół hierarchii dziedziczenia różnych. Zobaczmy, jak zaimplementować i użycie delegowania słabe w naszej aplikacji platformy Xamarin.iOS.

**Utwórz procedurę obsługi zdarzeń dla TouchUpInside** — Utwórzmy nowy program obsługi zdarzeń dla `TouchUpInside` zdarzeń przycisk Zmień kolor tła. Ten program obsługi wypełni rolę tej samej `HandleTouchUpInsideWithStrongDelegate` obsługi możemy utworzony w poprzedniej sekcji, ale użyje słabe delegata zamiast silne delegata. Edytuj klasy `ViewController`i dodaj następującą metodę:

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**Aktualizuj ViewDidLoad** — firma Microsoft musi zmienić `ViewDidLoad` korzystającą z programu obsługi zdarzeń, który właśnie utworzyliśmy. Edytuj `ViewController` i zmień `ViewDidLoad` przypominają poniższy fragment kodu:


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**Obsługa colorPickerControllerDidFinish: komunikat** — po `ViewController` jest zakończone, dla systemu iOS będzie wysyłać wiadomość `colorPickerControllerDidFinish:` do `WeakDelegate`. Musimy utworzyć metodę języka C#, która może obsłużyć tego komunikatu. Aby to zrobić, utworzymy metodę języka C# i następnie ją za pomocą adorn `ExportAttribute`. Edytuj `ViewController`i dodaj następującą metodę do klasy:

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

Uruchom aplikację. Należy ją teraz zachowywać się dokładnie tak, jak wcześniej, ale używa słabe delegata zamiast silne delegata. W tym momencie zakończyła się pomyślnie w tym przewodniku. Teraz masz opis sposobu tworzenia i wykorzystywania projekt powiązanie interfejsu Xamarin.iOS.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono proces tworzenia i używania projekt powiązanie interfejsu Xamarin.iOS. Najpierw Omówiliśmy sposób kompilowania istniejącej biblioteki języka Objective-C do biblioteki statycznej. Następnie omówiono sposób tworzenia projektu powiązanie interfejsu Xamarin.iOS i sposobu używania Objective Sharpie do generowania definicji interfejsu API biblioteki języka Objective-C. Omówiliśmy sposób aktualizacji i dostosować wygenerowanej definicji interfejsu API w celu dostosowania ich do użytku publicznego. Po projekt powiązanie interfejsu Xamarin.iOS zostało ukończone, przeszliśmy do używania tego powiązania w aplikacji platformy Xamarin.iOS, ze szczególnym uwzględnieniem przy użyciu silnego delegatów i słabych delegatów.

## <a name="related-links"></a>Linki pokrewne

- [Przykład powiązania (przykład)](https://developer.xamarin.com/samples/monotouch/InfColorPicker/)
- [Tworzenie powiązań bibliotek języka Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Szczegóły powiązania](~/cross-platform/macios/binding/overview.md)
- [Podręcznik informacyjny typów powiązań](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin dla deweloperów języka Objective-C](~/ios/get-started/objective-c-developers/index.md)
- [Struktura — zalecenia dotyczące projektowania](http://msdn.microsoft.com/library/ms229042.aspx)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
