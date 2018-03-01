---
title: "Odwołania do natywnych bibliotek"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/28/2016
ms.openlocfilehash: 9299d2b37825298d3defa18a9f5137e11b29f6ce
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="referencing-native-libraries"></a>Odwołania do natywnych bibliotek

Xamarin.iOS obsługuje łączenie z natywnych bibliotek C i bibliotek języka Objective-C. Ten dokument omówiono sposób połączyć natywnych bibliotek C z projektu platformy Xamarin.iOS. Aby uzyskać informacje na ten sam bibliotek języka Objective-C, zobacz nasze [powiązanie typów języka Objective-C](~/ios/platform/binding-objective-c/index.md) dokumentu.

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>Tworzenie uniwersalnych bibliotek natywnego (i386 ARMv7 i ARM64)

Często jest to pożądane, aby tworzyć natywnych bibliotek dla każdej z obsługiwanych platform dla opracowywania aplikacji systemu iOS (i386 symulator i ARMv7/ARM64 dla z samymi urządzeniami). Jeśli masz już projekt Xcode biblioteki, jest to naprawdę proste zrobić.

Aby utworzyć wersję i386 natywnej biblioteki, uruchom następujące polecenie z terminala:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

Spowoduje to natywnej biblioteki statycznej w obszarze `MyProject.xcodeproj/build/Release-iphonesimulator/`. Kopiowanie (lub przenoszenie) pliku archiwum biblioteki (libMyLibrary.a) na innym bezpiecznym do późniejszego użycia nadanie mu nazwy unikatowe (takich jak **libMyLibrary i386.a**), aby go nie mogą powodować konfliktów z wersji arm64 i armv7 tę samą bibliotekę, która będzie utworzyć w następnej kolejności.

Aby utworzyć wersję ARM64 natywnej biblioteki, uruchom następujące polecenie:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

Teraz wbudowanych natywnej biblioteki będą znajdować się w `MyProject.xcodeproj/build/Release-iphoneos/`. Ponownie, skopiuj (lub Przenieś) ten plik w bezpiecznym miejscu, zmiana jego nazwy podobną **libMyLibrary arm64.a** tak, aby go nie mogą powodować konfliktów.

Teraz kompilacji wersji ARMv7 biblioteki:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

Kopiowanie (lub przenoszenie) wynikowy plik biblioteki do tej samej lokalizacji przeniesiono 2 wersje biblioteki, zmiana jego nazwy podobną **libMyLibrary armv7.a**.

Aby wprowadzić universal binary, można użyć `lipo` narzędzie w następujący sposób:

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

Spowoduje to utworzenie `libMyLibrary.a` który będzie uniwersalnych biblioteki (fat), która będzie odpowiednie do użycia dla wszystkich celów programowanie dla systemu iOS.


### <a name="missing-required-architecture-i386"></a>Brak wymaganych architektura i386

W przypadku uzyskiwania `does not implement methodSignatureForSelector` lub `does not implement doesNotRecognizeSelector` wiadomości w danych wyjściowych środowiska uruchomieniowego podczas próby korzystać z biblioteki języka Objective-C w narzędziu iOS Simulator, biblioteki prawdopodobnie nie został skompilowany dla architektury i386 (zobacz [tworzenia uniwersalnych Natywnych bibliotek](#building_native) powyższej sekcji).

Aby sprawdzić architektur obsługiwanych przez danej biblioteki, wpisz następujące polecenie z poziomu terminala:

```bash
lipo -info /full/path/to/libraryname.a
```

Gdzie `/full/path/to/` jest pełną ścieżką do biblioteki są używane i `libraryname.a` jest zagrożona nazwę biblioteki.

Jeśli masz źródło do biblioteki należy skompilować i powiązać go z architekturą i386 również, aby przetestować aplikację w symulatorze systemu iOS.

### <a name="linking-your-library"></a>Łączenie biblioteki

Żadnej biblioteki innych firm, które zostaną zużyte musi być statycznie połączone z aplikacją. 

Jeśli chcesz statycznie Połącz bibliotekę "libMyLibrary.a" pochodzący z Internetu lub kompilacji w programie Xcode, należy wykonać kilka czynności:

-  Przełącz do projektu biblioteki
-  Konfigurowanie platformy Xamarin.iOS połączyć biblioteki
-  Metody dostępu z biblioteki.


Aby **doprowadzić do projektu biblioteki**, wybierz projekt w Eksploratorze rozwiązań i naciśnij klawisz **opcji + polecenia +**. Przejdź do libMyLibrary.a i dodaj go do projektu. Po wyświetleniu monitu, poinformuj programu Visual Studio for Mac lub Visual Studio skopiować go do projektu. Po dodaniu, Znajdź libFoo.a w projekcie, kliknij prawym przyciskiem myszy na nim i ustaw **Akcja kompilacji** do **Brak**.

Aby **Xamarin.iOS Konfiguruj, aby połączyć biblioteki**, opcje projektu dla użytkownika końcowego pliku wykonywalnego (nie biblioteki, ale program końcowego) należy dodać w **kompilacji systemu iOS**przez dodatkowy argument (są to część opcje projektu) "-gcc_flags" opcja następuje ciągu w cudzysłowie, który zawiera wszystkie dodatkowe biblioteki, które są wymagane dla programu, na przykład:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

Powyższy przykład połączy **libMyLibrary.a**

Można użyć `-gcc_flags` można określić dowolny zbiór argumenty wiersza polecenia do przekazania do kompilator GCC używane końcowe łącza z pliku wykonywalnego. Na przykład ten wiersz polecenia odwołujący się CFNetwork framework:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

Jeśli natywnej biblioteki zawiera kod w języku C++ należy również przekazać flagi - cxx w "Dodatkowych argumentów", aby Xamarin.iOS znała umożliwia poprawne kompilatora. Dla języka C++ poprzednie opcje wyglądałyby tak jak:

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>Podczas uzyskiwania dostępu do metody C & C# 35;

Dostępne są dwa rodzaje bibliotek macierzystego w systemie iOS:

-  Udostępnione bibliotek, które są częścią systemu operacyjnego.

-  Biblioteki statyczne, które są dostarczane z aplikacji.


Aby uzyskać dostęp do metody zdefiniowane w obydwu tych, należy użyć [funkcjonalności P/Invoke przez Mono](http://www.mono-project.com/Interop_with_Native_Libraries) czyli tej samej technologii, który ma zostać użyty w programie .NET, czyli około:

-  Określić, którą chcesz wywołać funkcję C
-  Określić jego podpisu
-  Określić biblioteki, który znajduje się w
-  Zapisu w odpowiedniej deklaracji P/Invoke


Korzystając z P/Invoke należy określić ścieżkę biblioteki, że połączenie jest ustanawiane z. Biblioteki udostępnione przy użyciu systemu iOS, można albo kodowania ścieżkę lub służy stałe wygody, które firma Microsoft zdefiniowane w naszym [klasy stałe](https://developer.xamarin.com/api/type/Constants/), stałe te powinny obejmować bibliotek udostępnionych z systemem iOS.

Na przykład, jeśli chcesz wywołać metody UIRectFrameUsingBlendMode z biblioteki UIKit firmy Apple, który ma podpis w C:

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

Twoje zgłoszenie P/Invoke będzie wyglądać następująco:

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Constants.UIKitLibrary jest jedynie stałą zdefiniowany jako "/ System/Library/Frameworks/UIKit.framework/UIKit", element EntryPoint umożliwia on opcjonalnie określić zewnętrzną nazwę (UIRectFramUsingBlendMode) podczas udostępnianie inną nazwę w języku C#, krótszego RectFrameUsingBlendMode.

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>Uzyskiwanie dostępu do C Dylibs

Jeśli masz trzeba korzystać z Dylib C, w aplikacji platformy Xamarin.iOS jest nieco dodatkowe ustawienia, która jest wymagana przed wywołaniem `DllImport` atrybutu.

Na przykład, jeśli mamy `Animal.dylib` z `Animal_Version` metodę, która zadzwonimy w naszej aplikacji, należy poinformować Xamarin.iOS lokalizację biblioteki przed próbą jej używać.

Aby to zrobić, Edytuj `Main.CS` pliku i zapewnić ich wyglądać następująco:

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

Gdzie `/full/path/to/` jest pełną ścieżką do Dylib są używane. Z tego kodu w miejscu, możemy można następnie połączyć `Animal_Version` metody w następujący sposób:

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>Biblioteki statyczne

Ponieważ biblioteki statyczne można używać tylko w systemie iOS, nie jest dostępna nie zewnętrznej biblioteki udostępnionej, aby połączyć się z tak parametr path w atrybucie DllImport wymaga specjalną nazwą `__Internal` (Zwróć uwagę na znaki podkreślenia podwójnego na początku nazwy) w przeciwieństwie do Nazwa ścieżki.

Dzięki temu DllImport do wyszukiwania symboli metody, która odwołuje się do bieżącego programu, zamiast próby załadowania go z udostępnionej biblioteki.

