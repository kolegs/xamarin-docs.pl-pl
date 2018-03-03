---
title: "Wskazówki: Powiązanie z systemem iOS biblioteka języka Objective C"
description: "Ten artykuł zawiera praktyczne wskazówki tworzenia powiązania Xamarin.iOS dla istniejącej biblioteki języka Objective-C, InfColorPicker. Obejmuje ona tematy, takie jak kompilowanie biblioteki statycznej Objective-C, powiązania jej i za pomocą tego powiązania w aplikacji platformy Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 210a6b45c144de3a0663658d8b33132e39c75ae7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>Wskazówki: Powiązanie z systemem iOS biblioteka języka Objective C

_Ten artykuł zawiera praktyczne wskazówki tworzenia powiązania Xamarin.iOS dla istniejącej biblioteki języka Objective-C, InfColorPicker. Obejmuje ona tematy, takie jak kompilowanie biblioteki statycznej Objective-C, powiązania jej i za pomocą tego powiązania w aplikacji platformy Xamarin.iOS._

Podczas pracy w systemie iOS, mogą występować w przypadkach, w której chcesz korzystać z biblioteki języka Objective-C innych firm. W takiej sytuacji można użyć Xamarin.iOS _powiązania projektu_ utworzyć [powiązanie C#](~/cross-platform/macios/binding/overview.md) który będzie można korzystać z biblioteki w aplikacji platformy Xamarin.iOS.

Zazwyczaj w ekosystemie iOS bibliotek można znaleźć w odmian 3:

* Jako plik wstępnie skompilowanym biblioteki statycznej `.a` rozszerzenia wraz z jego nagłówków (.h plików). Na przykład [firmy Google Analytics biblioteki](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* Jako platforma wstępnie skompilowana. Jest to folder zawierający biblioteki statycznej, nagłówki i czasami dodatkowe zasoby z `.framework` rozszerzenia. Na przykład [firmy Google AdMob biblioteki](https://developers.google.com/admob/ios/download).
* Jako pliki kodu źródłowego. Na przykład biblioteką właśnie `.m` i `.h` pliki języka Objective C.

W scenariuszu pierwszego i drugiego już będzie wstępnie skompilowanym biblioteki statycznej CocoaTouch, w tym artykule dotyczą możemy na trzeci scenariusz. Pamiętaj, że przed rozpoczęciem należy utworzyć powiązanie, sprawdzać licencję określoną z biblioteki, aby upewnić się, jest wolne powiązać go.

Ten artykuł zawiera przewodnik krok po kroku, tworzenia powiązania projektu przy użyciu typu open source [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective-C projektu na przykład, jednak wszystkie informacje w tym przewodniku mogą być dostosowywane do użycia ze wszystkimi biblioteka języka Objective-C innych firm. Biblioteka InfColorPicker udostępnia kontroler widoku wielokrotnego użytku, który umożliwia użytkownikowi wybranie koloru oparta na jej reprezentacji HSB zaznaczeniu kolor bardziej przyjazny dla użytkownika.

[ ![](walkthrough-images/run01.png "Przykład biblioteki InfColorPicker uruchomiony w systemie iOS")](walkthrough-images/run01.png)

Wszystkie czynności niezbędnych do korzystania z tego konkretnego interfejsu API języka Objective-C w Xamarin.iOS omówione zostaną następujące czynności:

- Najpierw utworzymy biblioteki statycznej Objective-C za pomocą środowiska Xcode.
- Następnie firma Microsoft będzie powiązać tej biblioteki statyczne z platformy Xamarin.iOS.
- Następnie Pokaż jak Sharpie cel można zmniejszyć obciążenie przez automatyczne generowanie niektóre (ale nie wszystkie) wymagane przez powiązanie Xamarin.iOS niezbędne definicje interfejsu API.
- Na koniec utworzymy aplikację platformy Xamarin.iOS, która używa powiązania.

Przykładowej aplikacji pokazują, jak używać silnych delegata do komunikacji między InfColorPicker interfejsu API i naszego kodu C#. Po możemy przedstawiono sposób użycia silne delegata, jak używać delegatów słabe do wykonania tych samych zadań omówione zostaną następujące czynności.

## <a name="requirements"></a>Wymagania

W tym artykule przyjęto założenie, że masz pewną znajomość Xcode i języka Objective-C i zapoznaniu się z naszym [powiązanie Objective-C](~/cross-platform/macios/binding/index.md) dokumentacji. Ponadto następujące jest wymagany do wykonania czynności przedstawionych:

-  **Xcode i iOS SDK** -Xcode firmy Apple i najnowszych interfejsu API dla systemu iOS muszą być zainstalowana i skonfigurowana na komputerze dewelopera.
-  **[Narzędzia wiersza polecenia Xcode](#Installing_the_Xcode_Command_Line_Tools)**  — narzędzia wiersza polecenia Xcode musi być zainstalowana obecnie zainstalowanej wersji Xcode (poniżej podano szczegółowe informacje dotyczące instalacji).
-  **Programu Visual Studio for Mac lub Visual Studio** — najnowszej wersji programu Visual Studio for Mac lub Visual Studio powinna być zainstalowana i skonfigurowana na komputerze dewelopera. Jest wymagany do tworzenia aplikacji platformy Xamarin.iOS Mac firmy Apple i przy użyciu programu Visual Studio musi być podłączony do [Xamarin.iOS kompilacji hosta](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
-  **Najnowszą wersję Sharpie cel** -kopię bieżącego narzędzia Sharpie cel pobrane z [tutaj](~/cross-platform/macios/binding/objective-sharpie/get-started.md). Jeśli masz już zainstalowany Sharpie cel, można zaktualizować ją do najnowszej wersji przy użyciu `sharpie update`

## <a name="installing-the-xcode-command-line-tools"></a>Instalowanie narzędzi wiersza polecenia Xcode

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Jak już wspomniano, będziemy używać narzędzi wiersza polecenia Xcode (w szczególności `make` i `lipo`) w tym przewodniku. `make` Polecenie to bardzo powszechne narzędzie systemu Unix, służący do kompilowania programów wykonywalnych i bibliotek przy użyciu _pliku reguł programu make_ Określa, jak program powinny zostać skompilowane. `lipo` Polecenie jest narzędzie wiersza polecenia OS X do tworzenia plików wielu architektura; będzie łączyć wiele `.a` pliki w jednym pliku, które mogą być używane przez wszystkie architektury sprzętu.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Jak już wspomniano, będziemy używać narzędzi wiersza polecenia Xcode na **hosta kompilacji Mac** (w szczególności `make` i `lipo`) w tym przewodniku. `make` Polecenie to bardzo powszechne narzędzie systemu Unix, służący do kompilowania programów wykonywalnych i bibliotek przy użyciu _pliku reguł programu make_ do określa sposób tworzenia programu. `lipo` Polecenie jest narzędzie wiersza polecenia OS X do tworzenia plików wielu architektura; będzie łączyć wiele `.a` pliki w jednym pliku, które mogą być używane przez wszystkie architektury sprzętu.


-----

Zgodnie z firmy Apple [tworzenie z wiersza polecenia z Xcode — często zadawane pytania](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) dokumentacji w OS X 10.9 lub nowszej, **pobiera** okienko Xcode **preferencje** okna dialogowego nie obsługuje już pobieranie narzędzia wiersza polecenia.

Należy użyć jednej z następujących metod zainstalowanie narzędzi:

- **Zainstaluj program Xcode** — po zainstalowaniu Xcode, jest dostarczany wraz z wszystkie narzędzia wiersza polecenia. W OS X 10.9 podkładek (zainstalowane w `/usr/bin`), można mapować dowolnego narzędzia zawarte w `/usr/bin` do narzędzia w programie Xcode. Na przykład `xcrun` polecenia, dzięki czemu można znaleźć lub uruchomić z wiersza polecenia dowolnego narzędzia w programie Xcode.
- **Aplikacja Terminal** — z terminala aplikacji można zainstalować narzędzi wiersza polecenia, uruchamiając `xcode-select --install` polecenia:
    - Uruchom aplikację terminala.
    - Typ `xcode-select --install` i naciśnij klawisz **Enter**, na przykład:

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - Użytkownik zostanie zapytany instalacji narzędzi wiersza polecenia, kliknij przycisk **zainstalować** przycisku: [ ![ ] (walkthrough-images/xcode01.png "Instalowanie narzędzi wiersza polecenia")](walkthrough-images/xcode01.png)

    - Narzędzia zostanie pobrany i zainstalowany z serwerami firmy Apple: [ ![ ] (walkthrough-images/xcode02.png "pobieranie narzędzi")](walkthrough-images/xcode02.png)

- **Pliki do pobrania dla deweloperów firmy Apple** -pakiet narzędzi wiersza polecenia jest dostępny [pliki do pobrania dla deweloperów firmy Apple]() strony sieci web. Zaloguj się za pomocą Identyfikatora firmy Apple, a następnie wyszukaj i Pobierz narzędzia wiersza polecenia: [ ![ ] (walkthrough-images/xcode03.png "Znajdowanie narzędzia wiersza polecenia")](walkthrough-images/xcode03.png)

Z zainstalowanymi narzędziami wiersza polecenia jest już kontynuować przewodnika.

## <a name="walkthrough"></a>Wskazówki

W tym przewodniku firma Microsoft będzie obejmuje następujące kroki:

- **[Utwórz bibliotekę statyczną](#Creating_A_Static_Library)**  — ten krok obejmuje tworzenia biblioteki statycznej **InfColorPicker** kod języka Objective-C. Biblioteka statyczna będzie mieć `.a` rozszerzenie pliku i zostanie osadzony w zestawie .NET projektu biblioteki.
- **[Tworzenie projektu platformy Xamarin.iOS powiązania](#Create_a_Xamarin.iOS_Binding_Project)**  — gdy mamy biblioteki statycznej użyjemy jej do utworzenia projektu platformy Xamarin.iOS powiązania. Projekt powiązanie składa się z biblioteką statyczną, którą właśnie utworzyliśmy i metadanych w postaci kodu C#, który objaśnia, jak można użyć interfejsu API języka Objective-C. Te metadane jest często określana jako definicje interfejsu API. Użyjemy  **[Sharpie cel](#Using_Objective_Sharpie)**  aby pomóc nam o utworzenie definicji interfejsu API.
- **[Normalizuj definicji interfejsu API](#Normalize_the_API_Definitions)**  — Sharpie celem jest Dobra robota pomagającym nam, ale nie wszystko. Omówiono pewne zmiany, które są konieczne do definicji interfejsu API przed ich użyciem.
- **[Korzystanie z biblioteki powiązania](#Using_the_Binding)**  -Finally, utworzymy aplikację platformy Xamarin.iOS pokazanie sposobu korzystania z naszych projektu nowo utworzone wiązanie.

Teraz, gdy firma Microsoft zrozumieć, jakie kroki są wykonywane, Przyjrzyjmy się rest tego przewodnika.

## <a name="creating-a-static-library"></a>Tworzenia biblioteki statycznej

Jeśli kod możemy sprawdzenia pod kątem InfColorPicker w witrynie Github:

[ ![](walkthrough-images/image02.png "Sprawdź kod dla InfColorPicker w serwisie Github")](walkthrough-images/image02.png)

Widzimy następujące trzy katalogi w projekcie:

- **InfColorPicker** -ten katalog zawiera kod języka Objective C dla projektu.
- **PickerSamplePad** -ten katalog zawiera przykładowy projekt iPad.
- **PickerSamplePhone** -ten katalog zawiera przykładowy projekt iPhone.

Umożliwia pobieranie projektu InfColorPicker z [GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) i Rozpakuj go w katalogu naszych wybór. Otwarcie Xcode celu `PickerSamplePhone` projektu, widzimy następujące struktury projektu w Nawigatorze Xcode:

[ ![](walkthrough-images/image03.png "W Nawigatorze Xcode struktury projektu")](walkthrough-images/image03.png)

Ten projekt osiąga ponowne użycie kodu dodając bezpośrednio InfColorPicker kodu źródłowego (w czerwonym prostokątem) do każdego projektu próbki. Kod przykładowy projekt jest wewnątrz pola niebieski. Ponieważ konkretnego projektu nie uzyskaliśmy bibliotekę statyczną, konieczne jest dla nas Tworzenie projektu Xcode do skompilowania z biblioteką statyczną.

Pierwszym krokiem jest dla nas do dodania do biblioteki statycznej InfoColorPicker kodu źródłowego. Umożliwia to osiągnąć wykonaj następujące czynności:

1. Uruchom środowisko Xcode.
2. Z **pliku** menu wybierz opcję **nowy** > **projektu...** :

    [ ![](walkthrough-images/image04.png "Uruchamianie nowego projektu")](walkthrough-images/image04.png)
3. Wybierz **Framework & biblioteki**, **Cocoa Touch biblioteki statycznej** szablon i kliknij przycisk **dalej** przycisk:

    [ ![](walkthrough-images/image05.png "Wybierz szablon Cocoa Touch biblioteki statycznej")](walkthrough-images/image05.png)
4. Wprowadź `InfColorPicker` dla **Nazwa projektu** i kliknij przycisk **dalej** przycisk:

    [ ![](walkthrough-images/image06.png "Wprowadź nazwę projektu InfColorPicker")](walkthrough-images/image06.png)
5. Wybierz lokalizację, aby zapisać projekt i kliknij przycisk **OK** przycisku.
6. Teraz należy dodać źródła z projektu InfColorPicker do naszej projektu biblioteki statycznej. Ponieważ **InfColorPicker.h** plik już istnieje w naszym biblioteka statyczna (domyślnie), środowisko Xcode nie pozwoli nam go zastąpić. Z **wyszukiwania**, przejdź do kodu źródłowego InfColorPicker oryginalnego projektu, który mamy rozpakować z serwisu GitHub, skopiuj wszystkie pliki InfColorPicker i wklej je do naszego nowego projektu biblioteki statycznej:

    [ ![](walkthrough-images/image12.png "Skopiuj wszystkie pliki InfColorPicker")](walkthrough-images/image12.png)

7. Wróć do Xcode, kliknij prawym przyciskiem myszy **InfColorPicker** i wybierz polecenie **Dodaj pliki do "InfColorPicker..."**:

    [ ![](walkthrough-images/image08.png "Dodawanie plików")](walkthrough-images/image08.png)

8. W oknie dialogowym Dodaj pliki, przejdź do plików kodu źródłowego InfColorPicker, które właśnie skopiowane, wybierz je wszystkie i kliknij **Dodaj** przycisk:

    [ ![](walkthrough-images/image09.png "Wybierz wszystkie, a następnie kliknij przycisk Dodaj")](walkthrough-images/image09.png)

9. Kod źródłowy zostaną skopiowane do naszej projektu:

    [ ![](walkthrough-images/image10.png "Kod źródłowy zostaną skopiowane do projektu")](walkthrough-images/image10.png)

10. Wybierz z Nawigatora projektu Xcode **InfColorPicker.m** plików oraz komentarz ostatnie dwa wiersze (ze względu na sposób, w tej bibliotece zostało zapisane, ten plik nie jest używany):

    [ ![](walkthrough-images/image14.png "Edytowanie pliku InfColorPicker.m")](walkthrough-images/image14.png)

11. Teraz musimy sprawdzić, czy są żadnych platform wymagane przez bibliotekę. Te informacje można znaleźć w pliku README lub przez otwarcie jednego z przykładowych projektów pod warunkiem. W tym przykładzie użyto `Foundation.framework`, `UIKit.framework`, i `CoreGraphics.framework` tak Dodajmy je.

12. Wybierz **docelowej InfColorPicker > fazy kompilacji** i rozwiń **binarny z bibliotekami** sekcji:

    [ ![](walkthrough-images/image16b.png "Rozwiń sekcję binarny z bibliotekami")](walkthrough-images/image16b.png)

13. Użyj  **+**  przycisk, aby otworzyć okno dialogowe umożliwiające dodanie struktury wymagane ramki wymienione powyżej:

    [ ![](walkthrough-images/image16c.png "Dodaj struktury wymagane ramki wymienionych powyżej")](walkthrough-images/image16c.png)

14. **Binarny z bibliotekami** sekcji powinna wyglądać tak jak obraz poniżej:

    [ ![](walkthrough-images/image16d.png "W sekcji binarny z bibliotekami")](walkthrough-images/image16d.png)

W tym momencie mamy zamknięcia, ale firma Microsoft nie jest jeszcze gotowe. Biblioteka statyczna został utworzony, ale musimy skompiluj go, aby utworzyć Fat binarny, który zawiera wszystkie wymagane architektury dla urządzeń z systemem iOS i symulatora systemu iOS.

### <a name="creating-a-fat-binary"></a>Tworzenie pliku binarnego Fat

Wszystkie urządzenia z systemem iOS mają procesory z architektury ARM zostały opracowane w czasie. Każda nowa architektura dodane nowe instrukcje i inne ulepszenia zachowując zapewnienia zgodności. Na urządzeniach z systemem iOS mamy armv6, armv7, armv7s, zbiór instrukcji arm64 — mimo że [używamy już armv6](~/ios/deploy-test/compiling-for-different-devices.md). Symulatora systemu iOS nie jest obsługiwany przez ARM i jest istead x86 i x86_64 zasilane symulatora. Firma Microsoft, które oznacza, że dla instytucji jest, że firma Microsoft udostępnić bibliotek na każdą instrukcję ustawia.

Biblioteka Fat jest `.a` plik zawierający wszystkie obsługiwane architektury.

Tworzenie binarnego fat jest procesem trzech fazach:

- Kompilowanie biblioteki statycznej w wersji ARM 7 i ARM64.
- Kompiluj x86 i x84_64 wersji biblioteki statycznej.
- Użyj `lipo` narzędzie wiersza polecenia do łączenia dwóch bibliotek statycznych w jeden.

Gdy te trzy kroki są raczej proste, a może być konieczne powtarzanie ich w przyszłości, gdy biblioteka języka Objective-C odbiera aktualizacje, lub jeśli wymagane poprawki. Jeśli zdecydujesz się zautomatyzować następujące kroki, uprości przyszłych konserwacji i pomocy technicznej dla projektu iOS powiązania.

Istnieje wiele narzędzi dostępne w celu automatyzacji takiego zadania — skrypt powłoki, [nachylenia](http://rake.rubyforge.org/), [xbuild](http://www.mono-project.com/Microsoft.Build), i [upewnij](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html). Zainstalowano narzędzia wiersza polecenia Xcode, będziemy również zainstalować upewnij, wtedy system kompilacji, który będzie używany w ramach tego przewodnika. Oto **pliku reguł programu make** używanej do utworzenia wielu architektura biblioteki udostępnionej, który będzie działać na urządzeniu z systemem iOS i symulatora dla żadnej biblioteki:

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

Wprowadź **pliku reguł programu make** poleceń w edytorze zwykłego tekstu, wybieranie i zaktualizuj sekcji z **YOUR PROJECT nazwy** z nazwą projektu. Należy również upewnić się, że firma Microsoft Wklej instrukcje powyżej, zachowywanie kartach w instrukcji.

Zapisz plik o nazwie **pliku reguł programu make** do tej samej lokalizacji co biblioteki statycznej Xcode InfColorPicker, które zostały utworzone powyżej:


[ ![](walkthrough-images/lib00.png "Zapisz plik o tej nazwie pliku reguł programu make")](walkthrough-images/lib00.png)

Otwórz aplikację terminalu na komputerze Mac, a następnie przejdź do lokalizacji pliku reguł programu make. Typ `make` w terminalu, naciśnij klawisz **Enter** i **pliku reguł programu make** zostaną wykonane:

[ ![](walkthrough-images/lib01.png "Przykładowe dane wyjściowe pliku reguł programu make")](walkthrough-images/lib01.png)

Po uruchomieniu upewnij, zobaczysz dużą ilość tekstu przewijanie przez. Jeśli wszystko działało poprawnie, zobaczysz wyrazy **kompilacji zakończyło się pomyślnie** i `libInfColorPicker-armv7.a`, `libInfColorPicker-i386.a` i `libInfColorPickerSDK.a` pliki zostaną skopiowane do tej samej lokalizacji co **pliku reguł programu make**:

[ ![](walkthrough-images/lib02.png "LibInfColorPicker armv7.a, libInfColorPicker i386.a i libInfColorPickerSDK.a pliki wygenerowane przez pliku reguł programu make")](walkthrough-images/lib02.png)

Można potwierdzić architektur w ramach Twojej Fat danych binarnych za pomocą następującego polecenia:

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

To powinien być wyświetlany poniżej:

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

W tym momencie została ukończona pierwsze naszych powiązania z systemem iOS przez tworzenia biblioteki statycznej przy użyciu narzędzia wiersza polecenia Xcode i Xcode `make` i `lipo`. Teraz przejdź do następnego kroku i użyj **Sharpie cel** można zautomatyzować tworzenie powiązań interfejsu API w firmie Microsoft.

## <a name="create-a-xamarinios-binding-project"></a>Utwórz Xamarin.iOS powiązania projektu

Przed możemy użyć **Sharpie cel** Aby zautomatyzować proces wiązania, należy utworzyć powiązanie projektu platformy Xamarin.iOS do przechowywania definicji interfejsu API (która będzie używana **Sharpie cel** aby pomóc nam Kompilacja) i utworzyć powiązanie C# firmie Microsoft.

Teraz należy wykonać następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom program Visual Studio dla komputerów Mac.
1. Z **pliku** menu, wybierz opcję **nowy** > **rozwiązania...** :

    ![](walkthrough-images/bind01.png "Uruchamianie nowego rozwiązania")

1. W oknie dialogowym nowe rozwiązanie, wybierz **biblioteki** > **iOS powiązania projektu**:

    ![](walkthrough-images/bind02.png "Wybierz iOS powiązania projektu")

1. Kliknij przycisk **dalej** przycisku.

1. Wprowadź "InfColorPickerBinding" jako **Nazwa projektu** i kliknij przycisk **Utwórz** przycisk, aby utworzyć rozwiązania:

    ![](walkthrough-images/bind02a.png "Wprowadź InfColorPickerBinding jako nazwy projektu")

Rozwiązanie zostanie utworzony i dwa domyślne pliki będą dołączone:

![](walkthrough-images/bind03.png "Struktura rozwiązania w Eksploratorze rozwiązań")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. Uruchom program Visual Studio.

1. Z **pliku** menu, wybierz opcję **nowy** > **projektu...** :

    ![](walkthrough-images/bind01vs.png "Uruchamianie nowego projektu")

1. W oknie dialogowym Nowy projekt, wybierz **iOS** > **biblioteki powiązania**:

    ![](walkthrough-images/bind02vs.png "Wybierz powiązania biblioteki z systemem iOS")

1. Wprowadź "InfColorPickerBinding" jako **nazwa** i kliknij przycisk **OK** przycisk, aby utworzyć rozwiązanie.

Rozwiązanie zostanie utworzony i dwa domyślne pliki będą dołączone:

![](walkthrough-images/bind03vs.png "Struktura rozwiązania w Eksploratorze rozwiązań")

-----



- **ApiDefinition.cs** — ten plik będzie zawierać umów, które definiują sposób API języka Objective-C zostaną opakowane w języku C#.
- **Structs.cs** — ten plik będzie zawierać wszystkie struktury lub wartości wyliczenia, które są wymagane przez interfejsów i delegatów.

Firma Microsoft będzie działać z tych plików w dalszej części przewodnika. Najpierw należy dodać biblioteki InfColorPicker do powiązania projektu.

### <a name="including-the-static-library-in-the-binding-project"></a>Łącznie z biblioteką statyczną w projekcie powiązania

Teraz mamy naszych podstawowej powiązania projektu gotowy należy dodać biblioteki Fat binarne, które zostały utworzone powyżej dla **InfColorPicker** biblioteki.

Wykonaj następujące kroki, aby dodać biblioteki:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij prawym przyciskiem myszy **odwołania natywnego** folderu w konsoli rozwiązania i wybierz **Dodaj odwołania natywnego**:

    ![](walkthrough-images/bind04a.png "Dodać odwołanie do natywnego")

1. Przejdź do Fat Binary wprowadziliśmy wcześniej (`libInfColorPickerSDK.a`) i naciśnij klawisz **Otwórz** przycisk:

    ![](walkthrough-images/bind05.png "Wybierz plik libInfColorPickerSDK.a")
1. Plik zostanie dołączony do projektu:

    ![](walkthrough-images/bind04.png "W tym pliku")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kopiuj `libInfColorPickerSDK.a` z Twojej **hosta kompilacji Mac** i wklej go do projektu powiązania.

1. Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj > istniejący element...** :

    ![](walkthrough-images/bind04vs.png "Dodawanie istniejącego pliku")

1. Przejdź do `libInfColorPickerSDK.a` i naciśnij klawisz **Dodaj** przycisk:

    ![](walkthrough-images/bind05vs.png "Dodawanie libInfColorPickerSDK.a")

1. Plik zostanie dołączony do projektu.

-----


Gdy plik .a zostanie dodany do projektu, automatycznie ustawi Xamarin.iOS **Akcja kompilacji** pliku do **ObjcBindingNativeLibrary**i utwórz specjalny plik o nazwie `libInfColorPickerSDK.linkwith.cs`.


Ten plik zawiera `LinkWith` atrybut, który informuje Xamarin.iOS sposobu dodawania dojście biblioteki statycznej, która po prostu firma Microsoft. Zawartość tego pliku są wyświetlane w poniższy fragment kodu:

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

`LinkWith` Atrybut identyfikuje biblioteki statycznej dla projektu oraz niektórych flag konsolidatora ważne.


Następny operacją, której konieczne jest utworzenie definicji interfejsu API dla projektu InfColorPicker. Na potrzeby tego przewodnika użyjemy Sharpie cel, aby wygenerować plik **ApiDefinition.cs**.

## <a name="using-objective-sharpie"></a>Przy użyciu celu Sharpie

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Wiersz polecenia jest Sharpie celu narzędzie (udostępnione przez Xamarin), które mogą pomóc w tworzeniu definicji wymaganej do powiązania 3 biblioteka języka Objective-C strony na język C#. W tej sekcji użyjemy Sharpie cel, aby utworzyć pierwszy **ApiDefinition.cs** InfColorPicker projektu.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Wiersz polecenia jest Sharpie celu narzędzie (udostępnione przez Xamarin), które mogą pomóc w tworzeniu definicji wymaganej do powiązania 3 biblioteka języka Objective-C strony na język C#. W tej sekcji użyjemy Sharpie cel na naszych **hosta kompilacji Mac** do utworzenia początkowej **ApiDefinition.cs** InfColorPicker projektu.


-----

Aby rozpocząć, teraz pobrać plik Instalatora Sharpie cel zgodnie z opisem w tym [przewodnik](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing). Uruchom Instalatora i wykonaj wszystkie monity na ekranie z Kreatora instalacji, aby zainstalować Sharpie cel na naszych komputerze dewelopera.

Po pomyślnie zostały Sharpie cel zainstalowany, teraz uruchomić aplikację terminala i wprowadź następujące polecenie, aby uzyskać pomoc dotyczącą wszystkich narzędzi dostępnych w powiązaniu:

```bash
sharpie -help
```

Jeśli firma Microsoft wykonane powyższe polecenie, zostanie wygenerowana następujące dane wyjściowe:

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

Na potrzeby tego przewodnika użyjemy następujących narzędzi Sharpie cel:

- **xcode** — to narzędzia daje informacji na temat naszych bieżąca instalacja Xcode i wersje systemu iOS i Mac interfejsów API, który firma Microsoft jest zainstalowany. Użyjemy tych informacji później gdy firma Microsoft generuje naszych powiązania.
- **Powiąż** -użyjemy tego narzędzia do analizy **.h** pliki w projekcie InfColorPicker do początkowej **ApiDefinition.cs** i **StructsAndEnums.cs** plików.

Aby uzyskać pomoc dotyczącą konkretnego narzędzia Sharpie cel, wprowadź nazwę, narzędzia i `-help` opcji. Na przykład `sharpie xcode -help` zwraca następujące dane wyjściowe:

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

Przed możemy rozpocząć proces wiązania, potrzebujemy uzyskać informacje o naszych bieżącego zainstalowanych zestawów SDK, wprowadzając następujące polecenie w terminalu `sharpie xcode -sdks`:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

Znajdującą się powyżej, zobaczysz, że mamy `iphoneos8.1` zainstalowany zestaw SDK na naszych maszyny. Te informacje w miejscu, możemy przystąpić do analizy projektu InfColorPicker `.h` plików do początkowej **ApiDefinition.cs** i `StructsAndEnums.cs` InfColorPicker projektu.

Wprowadź następujące polecenie w terminalu aplikacji:

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

Gdzie `[full-path-to-project]` jest pełną ścieżką do katalogu, gdzie **InfColorPicker** pliku projektu Xcode znajduje się na komputerze naszych i [systemu operacyjnego iphone] jest iOS SDK, który firma Microsoft jest zainstalowany, jak podano przez `sharpie xcode -sdks` polecenia. Należy pamiętać, że w tym przykładzie mamy zrealizowaniu  **\*.h** jako parametru, która obejmuje *wszystkie* pliki nagłówka w tym katalogu — zwykle użytkownik powinien nie, ale zamiast tego należy dokładnie zapoznać się za pośrednictwem pliki nagłówkowe, aby znaleźć najwyższego poziomu **.h** pliku, który odwołuje się do wszystkich innych odpowiednich plików po prostu Przekaż który Sharpie cel.

Następujące [dane wyjściowe](walkthrough-images/os05.png) zostanie wygenerowany w terminala:

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

[ ![](walkthrough-images/os06.png "Pliki InfColorPicker.enums.cs i InfColorPicker.cs")](walkthrough-images/os06.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Otwórz pliki w projekcie powiązania, które utworzono powyżej. Skopiuj zawartość **InfColorPicker.cs** pliku i wklej ją do **ApiDefinition.cs** pliku, zastępując istniejące `namespace ...` blok kodu z zawartością  **InfColorPicker.cs** pliku (pozostawiając `using` instrukcje nienaruszony):

![](walkthrough-images/os07.png "Plik InfColorPickerControllerDelegate")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Otwórz pliki w projekcie powiązania, które utworzono powyżej. Skopiuj zawartość **InfColorPicker.cs** pliku (z **hosta kompilacji Mac**) i wklej ją do **ApiDefinition.cs** pliku, zastępując istniejące `namespace ...` blok kodu z zawartością **InfColorPicker.cs** pliku (pozostawiając `using` instrukcje nienaruszony).


-----

## <a name="normalize-the-api-definitions"></a>Normalizuj definicji interfejsu API

Czasami ma Sharpie celu tłumaczenia problem `Delegates`, więc musimy zmodyfikować definicję `InfColorPickerControllerDelegate` interfejsu i Zastąp `[Protocol, Model]` wiersz z następujących czynności:

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
Tak, że definicja wygląda następująco:

[ ![](walkthrough-images/os11.png "Definicja")](walkthrough-images/os11.png)

Następnie możemy tak samo postąpić z zawartością `InfColorPicker.enums.cs` plik, kopiując i wklejając je w `StructsAndEnums.cs` pliku, pozostawiając `using` instrukcje bez zmian:

[ ![](walkthrough-images/os09.png "Zawartość StructsAndEnums.cs pliku ")](walkthrough-images/os09.png)

Można również znaleźć czy Sharpie cel ma adnotację powiązania o `[Verify]` atrybutów. Te atrybuty wskazują, że należy sprawdzić, czy Sharpie cel została poprawny element porównując powiązania z oryginalnej deklaracji Objective-C-C (która będzie dostępna w komentarz powyżej deklaracji powiązanej). Po upewnieniu się powiązania, należy usunąć atrybut weryfikacji. Aby uzyskać więcej informacji, zapoznaj się [Sprawdź](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) przewodnik.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


W tym momencie nasze powiązania projektu należy gotowy do kompilacji. Umożliwia tworzenie projektu naszych powiązania i upewnij się, że firma Microsoft zakończył bez błędów:

[Skompiluj projekt powiązania i upewnij się, że nie ma żadnych błędów](walkthrough-images/os12.png)


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


W tym momencie nasze powiązania projektu należy gotowy do kompilacji. Umożliwia tworzenie projektu naszych powiązania i upewnij się, że firma Microsoft zakończył bez błędów.


-----

## <a name="using-the-binding"></a>Za pomocą tego powiązania

Wykonaj następujące kroki, aby utworzyć iPhone przykładowej aplikacji do użycia z systemem iOS, powiązanie biblioteki utworzone powyżej:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Tworzenie projektu platformy Xamarin.iOS** — Dodawanie nowego projektu platformy Xamarin.iOS o nazwie **InfColorPickerSample** do rozwiązania, jak pokazano na poniższych zrzutach ekranu:

    ![](walkthrough-images/use01.png "Dodawanie aplikacji pojedynczego widoku")

    ![](walkthrough-images/use01a.png "Ustawienie identyfikator")

1. **Dodaj odwołanie do projektu powiązanie** — aktualizacja **InfColorPickerSample** projekt tak, aby miał on odwołania do **InfColorPickerBinding** projektu:

    ![](walkthrough-images/use02.png "Dodawanie odwołania do projektu powiązania")

1. **Utwórz iPhone interfejsu użytkownika** -kliknij dwukrotnie **MainStoryboard.storyboard** w pliku **InfColorPickerSample** projektu, aby go edytować w systemie iOS projektanta. Dodaj **przycisk** do widoku i nadaj mu `ChangeColorButton`, jak pokazano poniżej:

    ![](walkthrough-images/use03.png "Dodawanie przycisku do widoku")
1. **Dodaj InfColorPickerView.xib** — zawiera biblioteki InfColorPicker Objective-C **.xib** pliku. Xamarin.iOS nie będzie zawierać to **.xib** w projekcie powiązanie, co spowoduje ignorowanie błędów czasu wykonywania w naszym przykładowej aplikacji. Obejście tego jest dodanie **.xib** pliku do naszej projektu platformy Xamarin.iOS. Wybierz platformy Xamarin.iOS projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Dodaj pliki**i Dodaj **.xib** plików, jak pokazano na poniższym zrzucie ekranu:

    ![](walkthrough-images/use04.png "Dodaj InfColorPickerView.xib")

1. Gdy pojawi się pytanie, skopiuj **.xib** plik do projektu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. **Tworzenie projektu platformy Xamarin.iOS** — Dodawanie nowego projektu platformy Xamarin.iOS o nazwie **InfColorPickerSample** do rozwiązania, jak pokazano na poniższym zrzucie ekranu:

    ![](walkthrough-images/use01vs.png "Tworzenie projektu platformy Xamarin.iOS")

1. **Dodaj odwołanie do projektu powiązanie** — aktualizacja **InfColorPickerSample** projekt tak, aby miał on odwołania do **InfColorPickerBinding** projektu:

    ![](walkthrough-images/use02vs.png "Dodaj odwołanie do projektu powiązania")

1. **Utwórz iPhone interfejsu użytkownika** -kliknij dwukrotnie **MainStoryboard.storyboard** w pliku **InfColorPickerSample** projektu, aby go edytować w systemie iOS projektanta. Dodaj **przycisk** do widoku i nadaj mu `ChangeColorButton`, jak pokazano poniżej:

    ![](walkthrough-images/use03vs.png "Utwórz iPhone interfejsu użytkownika")

1. **Dodaj InfColorPickerView.xib** — zawiera biblioteki InfColorPicker Objective-C **.xib** pliku. Xamarin.iOS nie będzie zawierać to **.xib** w projekcie powiązanie, co spowoduje ignorowanie błędów czasu wykonywania w naszym przykładowej aplikacji. Obejście tego jest dodanie **.xib** pliku do naszej projektu platformy Xamarin.iOS z naszych **hosta kompilacji Mac**. Wybierz platformy Xamarin.iOS projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj** > **istniejący element...** i Dodaj **.xib** pliku.


-----



Następnie wytłumaczone krótki przegląd protokołów w języku Objective C i jak możemy ich obsługę powiązania i kod w języku C#.

### <a name="protocols-and-xamarinios"></a>Protokoły i platformy Xamarin.iOS

W języku Objective-C, protokół definiuje metody (lub wiadomości) które mogą być używane w pewnych okolicznościach. Koncepcyjnie są bardzo podobne do interfejsów w języku C#. Jeden główna różnica między protokół Objective-C i interfejs języka C# jest protokołów można opcjonalnie metody - metody, które nie ma klasy do zaimplementowania. Używa języka Objective-C @optional — słowo kluczowe jest używany do określania, które metody są opcjonalne. Aby uzyskać więcej informacji na temat protokołów zobacz [zdarzeń, protokołów i delegatów](~/ios/app-fundamentals/delegates-protocols-and-events.md).

**InfColorPickerController** ma jeden taki protokół, przedstawiono we fragmencie kodu poniżej:

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

Protokół ten jest używany przez **InfColorPickerController** informują klientów o tym, że użytkownik pobrał nowy kolor, a **InfColorPickerController** zostało zakończone. Sharpie celu zamapowane tego protokołu, jak pokazano w poniższy fragment kodu:

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

Podczas kompilowania biblioteki powiązania Xamarin.iOS utworzy abstrakcyjna klasa podstawowa o nazwie `InfColorPickerControllerDelegate`, który implementuje ten interfejs, za pomocą metod wirtualnych.

Istnieją dwa sposoby, że firma Microsoft implementuje ten interfejs w aplikacji platformy Xamarin.iOS:

- **Delegowanie silne** — przy użyciu silnego delegata związane z utworzeniem klasy C# tego podklasy `InfColorPickerControllerDelegate` i zastępuje odpowiednie metody. **InfColorPickerController** użyje wystąpienia tej klasy do komunikowania się z jej klientów.
- **Delegat słabe** -słabe delegat jest nieco inne metody, która obejmuje utworzenie publicznej metody w klasie niektórych (takich jak `InfColorPickerSampleViewController`), a następnie udostępnianie tej metody do `InfColorPickerDelegate` protokołu za pomocą `Export` atrybutu.

Silne delegatów Podaj Intellisense, zabezpieczeń i lepsze hermetyzacji. Z tego względu należy używać silnych delegatów gdzie to możliwe, zamiast słabe delegata.

W tym przewodniku omówimy obie techniki: najpierw implementacja silne delegata i następnie wyjaśnienie sposobu implementacji słabe delegata.

### <a name="implementing-a-strong-delegate"></a>Wdrożenie silnych delegata

Zakończ aplikacji platformy Xamarin.iOS przy użyciu silnego delegata odpowiedzieć `colorPickerControllerDidFinish:` komunikat:

**Podklasy InfColorPickerControllerDelegate** — Dodawanie nowych klas do projektu o nazwie `ColorSelectedDelegate`. Edytuj klasy, aby miała następujący kod:

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

Xamarin.iOS będzie powiązać delegata Objective-C, tworząc abstrakcyjna klasa podstawowa o nazwie `InfColorPickerControllerDelegate`. Podklasy danego typu i zastąpienie `ColorPickerControllerDidFinish` metodę, aby uzyskać dostęp do wartości `ResultColor` właściwość `InfColorPickerController`.

**Utwórz wystąpienie ColorSelectedDelegate** — nasze obsługi zdarzeń należy wystąpienie `ColorSelectedDelegate` typ utworzonej w poprzednim kroku. Edytuj klasy `InfColorPickerSampleViewController` i dodaj następujące zmienna wystąpienia klasy:

```csharp
ColorSelectedDelegate selector;
```

**Inicjowanie zmiennej ColorSelectedDelegate** — aby upewnić się, że `selector` jest prawidłową wystąpienia, zaktualizuj metodę `ViewDidLoad` w `ViewController` odpowiadające następujący fragment kodu:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
    selector = new ColorSelectedDelegate (this);
}
```
**Zaimplementuj metodę HandleTouchUpInsideWithStrongDelegate** -obok implementacji programu obsługi zdarzeń dla gdy użytkownik dotyka **ColorChangeButton**. Edytuj `ViewController`i dodaj następującą metodę:

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

Firma Microsoft najpierw uzyskać wystąpienia `InfColorPickerController` za pomocą metody statycznej i ustawić go jako wystąpienie pamiętać o naszych silne delegata za pomocą właściwości `InfColorPickerController.Delegate`. Ta właściwość została wygenerowana automatycznie firmie Microsoft przez Sharpie cel. Na koniec nazywamy `PresentModallyOverViewController` do wyświetlenia widoku `InfColorPickerSampleViewController.xib` , dzięki czemu użytkownik może wybrać kolor.

**Uruchom aplikację** — w tym punkcie skończymy ze wszystkimi naszego kodu. Po uruchomieniu aplikacji powinno być możliwe zmienić kolor tła `InfColorColorPickerSampleView` jak pokazano na poniższych zrzutach ekranu:

[ ![](walkthrough-images/run01.png "Uruchamianie aplikacji")](walkthrough-images/run01.png)

Gratulacje! W tym momencie został pomyślnie utworzony i powiązane biblioteki języka Objective-C do użycia w aplikacji platformy Xamarin.iOS. Następnie Przyjrzyjmy się przy użyciu słabe delegatów.

### <a name="implementing-a-weak-delegate"></a>Implementowanie słabe delegata

Zamiast tworzenie podklas klasy powiązane z protokołu Objective-C w przypadku określonych pełnomocników, Xamarin.iOS umożliwia także implementować metody protokołu w dowolnej klasy, która jest pochodną `NSObject`, dekoracji metody z `ExportAttribute`, a następnie dostarczanie odpowiednich selektorów. Po zastosowaniu takiego podejścia, przypisz wystąpienie klasy do `WeakDelegate` zamiast do właściwości `Delegate` właściwości. Słabe delegata zapewnia elastyczność podjęcie klasa delegata w dół hierarchii dziedziczenia różnych. Zobaczmy, jak wdrożyć i użycie delegowania słabe w naszej aplikacji platformy Xamarin.iOS.

**Tworzenie obsługi zdarzeń dla TouchUpInside** — Utwórzmy nowy program obsługi zdarzeń dla `TouchUpInside` zdarzeń przycisku Zmień kolor tła. Ten program obsługi spowoduje wypełnienie tego samego elementu role jako `HandleTouchUpInsideWithStrongDelegate` obsługi został utworzony w poprzedniej sekcji, ale użyje słabe delegata zamiast silne delegata. Edytuj klasy `ViewController`i dodaj następującą metodę:

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**Zaktualizuj ViewDidLoad** — należy zmienić `ViewDidLoad` korzystającą z obsługi zdarzeń, który właśnie utworzony. Edytuj `ViewController` i zmienić `ViewDidLoad` sposób przypominający poniższy fragment kodu:


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**Obsługa colorPickerControllerDidFinish: komunikat** — po `ViewController` jest zakończone, iOS wyśle wiadomość `colorPickerControllerDidFinish:` do `WeakDelegate`. Należy utworzyć metody C#, który może obsługiwać ten komunikat. Aby to zrobić, możemy utworzyć metodę C# i następnie adorn go przy użyciu `ExportAttribute`. Edytuj `ViewController`i dodaj następującą metodę do klasy:

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

Uruchom aplikację. Teraz zachowania dokładnie tak jak poprzednio, ale używa słabe delegata zamiast silne delegata. W tym momencie zakończyła się pomyślnie w tym przewodniku. Teraz wymaga zrozumienia sposobu tworzenia i korzystać z projektu platformy Xamarin.iOS powiązania.

## <a name="summary"></a>Podsumowanie

W tym artykule udał przez proces tworzenia i używania powiązania projektu platformy Xamarin.iOS. Najpierw omówiono sposób kompilowania istniejącej biblioteki języka Objective-C do biblioteki statycznej. Następnie możemy opisano sposób tworzenia projektu platformy Xamarin.iOS powiązania i sposobu użycia Sharpie cel do wygenerowania definicji interfejsu API dla biblioteki języka Objective-C. Omówiono aktualizacji i dostosować wygenerowanego definicje interfejsu API, aby były nadające się do publicznych zużycia. Po projektu platformy Xamarin.iOS powiązania zostało zakończone, możemy przeniesione do korzystanie z tego wiązania w aplikacji platformy Xamarin.iOS nacisk na przy użyciu delegatów silne i słabe delegatów.

## <a name="related-links"></a>Linki pokrewne

- [Przykład powiązania (przykład)](https://developer.xamarin.com/samples/monotouch/InfColorPicker/)
- [Tworzenie powiązań bibliotek języka Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Szczegóły powiązania](~/cross-platform/macios/binding/overview.md)
- [Powiązanie typy Podręcznik](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin dla deweloperów języka Objective C](~/ios/get-started/objective-c-developers/index.md)
- [Struktura — zalecenia dotyczące projektowania](http://msdn.microsoft.com/en-us/library/ms229042.aspx)
