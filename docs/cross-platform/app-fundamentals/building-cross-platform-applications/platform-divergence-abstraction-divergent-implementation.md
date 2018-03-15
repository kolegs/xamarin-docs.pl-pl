---
title: "Część 4 - zajmujących się wielu platform"
ms.topic: article
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 21cd08ad2eb9c78ba1bcd6b31400a38266c65e51
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/13/2018
---
# <a name="part-4---dealing-with-multiple-platforms"></a>Część 4 - zajmujących się wielu platform

## <a name="handling-platform-divergence-amp-features"></a>Obsługa Platform rozbieżności &amp; funkcji

Rozbieżności danych nie jest po prostu problemu wielu platform. urządzenia na platformie "sam" mają różne możliwości (szczególnie szerokiej gamy urządzeń z systemem Android, które są dostępne). Najbardziej oczywistym i podstawowa jest rozmiaru ekranu, ale inne atrybuty urządzenia mogą różnić się i wymaga aplikacji sprawdzanie określonych funkcji i zachowywać się inaczej w zależności od ich obecności (lub jego braku).

Oznacza to, że wszystkie aplikacje muszą uwzględniać łagodne spadek funkcjonalności, w przeciwnym razie istnieje zestaw funkcji nieatrakcyjnych, najniższy denominator typowe. W programie Xamarin głęboką integrację z natywnych zestawów SDK dla każdej z platform zezwala aplikacjom korzystać z funkcji specyficznych dla platformy, dlatego warto projektowania aplikacji do używania tych funkcji.

Zobacz dokumentację możliwości platformy omówienie sposobu platformy różnią się w działaniu.

 <a name="Examples_of_Platform_Divergence" />


### <a name="examples-of-platform-divergence"></a>Przykłady rozbieżności platformy

 <a name="Fundamental_elements_that_exist_across_platforms" />


#### <a name="fundamental-elements-that-exist-across-platforms"></a>Podstawowe elementy, które istnieją na różnych platformach

Brak niektórych właściwości aplikacji mobilnych, które są uniwersalne.
Są to wyższego poziomu pojęcia, które są zazwyczaj true wszystkich urządzeń i w związku z tym mogą stanowić podstawę projektu aplikacji:

-  Wybór funkcji za pośrednictwem karty lub menu
-  Wykaz danych i przewijanie
-  Pojedynczy widoki danych
-  Edytowanie pojedynczego widoki danych
-  Przechodzenie wstecz


Podczas projektowania sieci przepływu ekranu wysokiego poziomu dotyczących tych pojęć można oprzeć wspólne środowisko użytkownika.

 <a name="platform-specific_attributes" />


#### <a name="platform-specific-attributes"></a>atrybuty specyficzne dla platformy

Oprócz podstawowych elementów, które istnieją na wszystkich platformach konieczne będzie adres klucza platformy różnice w projekcie. Należy wziąć pod uwagę (i pisania kodu, w szczególności do obsługi) tych różnic:

-   **Rozmiary ekranu** — niektóre platformy (na przykład iOS i Windows Phone starszych) ustalić rozmiaru ekranu, które są stosunkowo prosta do docelowego. Urządzenia z systemem android ma szerokiej gamy ekranu wymiarów, które wymagają więcej działań zmierzających do obsługi aplikacji.
-   **Metafory nawigacji** — różnią się w różnych platform (np.) przycisk "Wstecz" sprzętu formantu interfejsu użytkownika Panorama) i w obrębie platform (Android 2 i 4, iPhone vs iPad).
-   **Klawiatury** — niektóre Android urządzenia mają fizycznej klawiatury, podczas gdy inne zawierają tylko klawiatura programowa. Kod, który wykrywa soft klawiatury jest przesłaniania części ekranu musi być czułe na różnice te.
-   **Touch i gestów** — system operacyjny obsługuje rozpoznawania gestów różni się zwłaszcza w starszych wersjach każdego systemu operacyjnego. Wcześniejszych wersji systemu android mają bardzo ograniczoną obsługę operacji touch, co oznacza, że obsługa starszych urządzeń mogą wymagać oddzielne kodu
-   **Powiadomienia wypychane** — Brak możliwości różnych/implementacji na każdej z platform (np.) Kafelki na żywo z systemem Windows).


 <a name="Device-specific_features" />


#### <a name="device-specific-features"></a>Funkcje specyficzne dla urządzenia

Określają, jakie musi być minimalne funkcje wymagane dla aplikacji; lub jeśli zdecyduj, jakie dodatkowe funkcje, aby móc korzystać z na każdej z platform. Kod będzie wymagane funkcje wykrywania i wyłączenie funkcji lub ofert alternatyw (np.) zamiast lokalizacja geograficzna może być użytkownikowi należy wpisać lokalizację lub wybierz ją z mapy):

-   **Aparat fotograficzny** — funkcja różni się między urządzeniami: Niektóre urządzenia nie ma aparatu fotograficznego, inne osoby mają zarówno kamery przodu i skierowanym. Niektóre kamery są w stanie nagrań wideo.
-   **Lokalizacja geograficzna & mapy** — Obsługa GPS lub Wi-Fi w lokalizacji nie jest obecna na wszystkich urządzeniach. Aplikacje muszą również automatycznie dostosowują się do różnych poziomów dokładności obsługiwaną przez każdą z tych metod.
-   **Przyspieszeniomierza, Żyroskop i kompas** — te funkcje często znajdują się w tylko wybrane urządzenia na każdej platformie, więc aplikacje prawie zawsze wymaga podania rezerwowe, gdy sprzęt nie jest obsługiwany.
-   **Facebook i Twitter** — tylko "wbudowanych" iOS5 i iOS6 odpowiednio. W starszych wersjach i innych platform należy podać funkcji uwierzytelniania i interfejsu bezpośrednio z każdym usług interfejsu API.
-   **Pobliżu komunikacji Zbliżeniowej (NFC)** — tylko w przypadku (niektórych) systemu Android, telefony (w czasie zapisywania).


 <a name="Dealing_with_Platform_Divergence" />


### <a name="dealing-with-platform-divergence"></a>Zajmujących się rozbieżności platformy

Istnieją dwa różne podejścia do obsługi wielu platform z tego samego kodu — podstawy, każde z nich własny zestaw zalety i wady.

-   **Platforma abstrakcji** — wzorzec fasad firm zapewnia ujednoliconą dostęp do różnych platform i abstracts implementacji danej platformy w jednej, ujednoliconej interfejs API.
-   **Implementacja rozbieżności** — wywołania określonej platformy funkcji za pomocą implementacji rozbieżności za pomocą architektury narzędzi, takich jak interfejsów i dziedziczenia lub kompilacji warunkowej.


 <a name="Platform_Abstraction" />


## <a name="platform-abstraction"></a>Abstrakcja platformy

 <a name="Class_Abstraction" />


### <a name="class-abstraction"></a>Abstrakcja — klasa

Za pomocą interfejsów lub klas podstawowych zdefiniowanych w kodzie udostępnionego i zaimplementowana lub rozszerzone w projektach specyficzne dla platformy. Zapisywanie i rozszerzanie udostępnionego kodu za pomocą obiektów klas abstrakcyjnych szczególnie jest przeznaczone do przenośnej biblioteki klas, ponieważ ma ograniczonym podzbiorem dostępnej framework i nie może zawierać dyrektywy kompilatora do obsługi gałęzie kodu specyficzne dla platformy.

 <a name="Interfaces" />


#### <a name="interfaces"></a>Interfejsy

Za pomocą interfejsów umożliwia Implementowanie klasy specyficzne dla platformy, które nadal mogą zostać przekazane do bibliotek udostępnionych przeprowadzać typowy kod.

Interfejs jest zdefiniowana w kodzie udostępnionego i przekazać do biblioteki udostępnionej jako parametr lub właściwości.

Specyficzne dla platformy aplikacji można następnie implementować interfejs i nadal korzystać z udostępnionego kodu ' "go przetworzyć.

 **Zalety**

Implementacja może zawierać kod specyficzne dla platformy i nawet odwołuje się biblioteki zewnętrznej specyficzne dla platformy.

 **Wady**

Konieczności tworzenia, a następnie przekaż implementacje do udostępnionego kodu. Jeśli używany jest interfejs głębokość z udostępnionym kodem następnie kończy się ono są przekazywane wiele parametrów metody lub w przeciwnym razie przesuwana łańcuchu wywołań. Jeśli udostępniony kod używa wielu różnych interfejsach następnie one musi wszystkie utworzona i ustawiona w innym kodzie udostępnionego.

 <a name="Inheritance" />


#### <a name="inheritance"></a>Dziedziczenie

Udostępniony kod można zaimplementować klasy abstrakcyjnej ani wirtualnej, które może zostać rozszerzony w co najmniej jeden projekt specyficzne dla platformy. Ten proces jest podobny do korzystania z interfejsów, ale niektóre zachowania już wdrożone. Istnieją różne widoki na czy lepszym rozwiązaniem projektowania interfejsów lub dziedziczenia: w szczególności ponieważ C# zezwala na tylko pojedyncze dziedziczenie go dyktowania sposób swoje interfejsy API może być zaprojektowana w przyszłości. Dziedziczenie należy używać ostrożnie.

Zalety i wady interfejsów stosowane jednakowo do dziedziczenia z dodatkową korzyścią klasy podstawowej mogący zawierać implementację kodu (prawdopodobnie platformy o niesprecyzowanym implementację, która może być opcjonalnie rozszerzony).

<a name="Xamarin.Forms" />

### <a name="xamarinforms"></a>Xamarin.Forms

Zobacz [platformy Xamarin.Forms](~/xamarin-forms/get-started/index.md) dokumentacji.


### <a name="plug-in-cross-platform-functionality"></a>Wtyczka funkcji i Platform

Można również rozszerzyć zasięg wieloplatformowych aplikacji w spójny sposób za pomocą wtyczki.

Połączone z naszych [github wtyczek](https://github.com/xamarin/plugins), większość dodatków plug-in są open source projektów (zwykle dotyczy instalacji za pośrednictwem pakietu Nuget), które ułatwiają implementacji różnych funkcji specyficzne dla platformy z baterii stan ustawienia z programu wspólnego interfejsu API, który ułatwia korzystanie w aplikacji platformy Xamarin i platformy Xamarin.Forms.


<a name="Other_Cross-Platform_Libraries" />

### <a name="other-cross-platform-libraries"></a>Innych bibliotek i Platform

Istnieje wiele 3 bibliotek strony dostępne, które zapewniają funkcje i platform:

-   **MvvmCross** -  [https://github.com/slodge/MvvmCross/](https://github.com/slodge/MvvmCross/)
-   **Vernacular** (dla lokalizacji -)  [https://github.com/rdio/vernacular/](https://github.com/rdio/vernacular/)
-   **MonoGame** (dla platformy XNA gry) -  [http://monogame.codeplex.com/](http://monogame.codeplex.com/)
-   **NGraphics** - [NGraphics](https://github.com/praeclarum/NGraphics) i jego macierzystych [https://github.com/praeclarum/CrossGraphics](https://github.com/praeclarum/CrossGraphics)


 <a name="Divergent_Implementation" />


### <a name="divergent-implementation"></a>Implementacja rozbieżności

 <a name="Conditional_Compilation" />


#### <a name="conditional-compilation"></a>Kompilacja warunkowa

Istnieje kilka sytuacji, gdy udostępniony kod nadal będzie konieczne działają inaczej na każdej platformie, prawdopodobnie podczas uzyskiwania dostępu do klasy lub funkcje, które zachowują się inaczej. Kompilacja warunkowa działa najlepiej z udostępnionych projektów zasobów, których w wielu projektach, które mają różne symbole zdefiniowane odwołuje się tego samego pliku źródłowego.

Projekty Xamarin zawsze Zdefiniuj `__MOBILE__` który ma wartość true dla systemu iOS i projekty aplikacji systemu Android (należy pamiętać, podkreślenie o podwójnej precyzji, przed i po awarii w tych symboli).

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

<a name="iOS" />

##### <a name="ios"></a>iOS

Definiuje Xamarin.iOS `__IOS__` służącego do wykrywania urządzeń z systemem iOS.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

Dostępne są również specyficznych dla czujki i TV symboli:

```csharp
#if __TVOS__
// tv-specific stuff
#endif

#if __WATCHOS__
// watch-specific stuff
#endif
```

<a name="Android" />

##### <a name="android"></a>Android

Kod, który ma być kompilowana tylko do aplikacji platformy Xamarin.Android można użyć następujących czynności

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

Każda wersja interfejsu API definiuje również nowe dyrektywy kompilatora, tak umożliwi kodu w następujący sposób dodawania funkcji, jeśli są przeznaczone dla nowszej interfejsów API. Każdy poziom interfejsu API zawiera wszystkie symbole poziomu "niższe". Ta funkcja nie jest naprawdę przydatny do obsługi wielu platform; zwykle `__ANDROID__` symbol będą wystarczające.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

##### <a name="mac"></a>Mac

Nie ma obecnie to symbol wbudowany dla Xamarin.Mac, ale można dodać własne w Mac projekt aplikacji **Opcje > kompilacji > kompilatora** w **zdefiniować symbole** dialogowe lub edytowanie **.csproj**  i Dodaj (na przykład `__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

<a name="Windows_Phone" />

##### <a name="windows-phone"></a>Windows Phone

Aplikacje Windows Phone definiuje dwie symbole — `WINDOWS_PHONE` i `SILVERLIGHT` — można kod, aby platforma docelowa. Nie mają one podkreślenia otaczającego je podobnie jak symbole platformy Xamarin, czy.


<a name="Using_Conditional_Compilation" />

##### <a name="using-conditional-compilation"></a>Przy użyciu kompilacja warunkowa

Prosty przykład analizę przypadku kompilacja warunkowa jest ustawienie Lokalizacja pliku dla pliku bazy danych SQLite. Trzy platformy są nieco inne wymagania dotyczące określający lokalizację pliku:

-   **iOS** — Apple preferuje danych niezwiązanych z użytkownikiem w określonej lokalizacji (katalog biblioteki), ale nie ma stałej systemu dla tego katalogu. Specyficzne dla platformy kodu jest wymagane do utworzenia poprawną ścieżkę.
-   **Android** — ścieżka systemu zwróconych przez `Environment.SpecialFolder.Personal` jest dopuszczalne lokalizację do przechowywania plików bazy danych.
-   **Windows Phone** — mechanizm izolowanych magazynów nie zezwala na pełną ścieżkę należy określić tylko względną ścieżkę i nazwę pliku.
-   **Platforma uniwersalna systemu Windows** — używa `Windows.Storage` interfejsów API.

W poniższym kodzie użyto kompilacja warunkowa zapewnienie `DatabaseFilePath` jest poprawna dla każdej platformy:

```csharp
public static string DatabaseFilePath {
        get {
    var filename = "TodoDatabase.db3";
#if SILVERLIGHT
    // Windows Phone 8
    var path = filename;
#else

#if __ANDROID__
    string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
#if __IOS__
        // we need to put in /Library/ on iOS5.1 to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library");
#else
        // UWP
        string libraryPath = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
#endif
#endif
        var path = Path.Combine (libraryPath, filename);
#endif
        return path;
}
```

Wynik jest klasa, która może być utworzony i używany na wszystkich platformach umieszczenia pliku bazy danych SQLite w innej lokalizacji na każdej z platform.

