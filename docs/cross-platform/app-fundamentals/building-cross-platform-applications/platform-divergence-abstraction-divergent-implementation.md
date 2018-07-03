---
title: Część 4 — Praca z wieloma platformami
description: W tym dokumencie opisano sposób obsługi rozbieżności aplikacji na podstawie platform lub funkcji. Omówiono w nim rozmiar ekranu, metafory nawigacji, Dotyk i gesty, powiadomienia wypychane i paradygmatów interfejsu, takie jak listy i karty.
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 4a60c99cbc9819f07b77bfe9abe046ea92a550a5
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403328"
---
# <a name="part-4---dealing-with-multiple-platforms"></a>Część 4 — Praca z wieloma platformami

## <a name="handling-platform-divergence-amp-features"></a>Obsługa Platform rozbieżności &amp; funkcji

Rozbieżności nie po prostu problemu 'cross-platform'; urządzenia na platformie "w tym samym" mają różne możliwości (szczególnie szerokiej gamy urządzeń z systemem Android, które są dostępne). Oczywistym i podstawowa jest rozmiar ekranu, ale inne atrybuty urządzenia mogą się różnić w i wymagają aplikacji można sprawdzić w przypadku niektórych funkcji i zachowywać się inaczej w zależności od ich obecność (lub brak).

Oznacza to, że wszystkie aplikacje muszą dotyczyć bezpiecznie obniżenie funkcjonalności; w przeciwnym razie prezentować zestawy nieatrakcyjnych funkcja najniższy uniwersalność. Głęboka Integracja platformy Xamarin przy użyciu natywnych zestawów SDK każdej z platform Zezwalaj aplikacjom z zalet funkcji specyficznych dla platformy, dlatego warto projektowanie aplikacji do używania tych funkcji.

Zobacz dokumentację możliwości platformy omówienie sposobu platformy różnią się w funkcji.

 <a name="Examples_of_Platform_Divergence" />


### <a name="examples-of-platform-divergence"></a>Przykłady rozbieżności platformy

 <a name="Fundamental_elements_that_exist_across_platforms" />


#### <a name="fundamental-elements-that-exist-across-platforms"></a>Podstawowe elementy, które istnieją między platformami

Istnieją pewne cechy aplikacji mobilnych, które są uniwersalne.
Poniżej przedstawiono wyższego poziomu pojęcia, które są zazwyczaj prawdziwe dla wszystkich urządzeń i w związku z tym może stanowić podstawę projektowania Twojej aplikacji:

-  Wybór funkcji za pośrednictwem karty lub menu
-  Wykaz danych i przewijanie
-  Pojedynczy widoki danych
-  Edytowanie pojedynczego widoki danych
-  Przechodzenia wstecz


Podczas projektowania przepływu ekranu wysokiego poziomu dotyczących tych pojęć można utworzyć wspólne środowisko użytkownika.

 <a name="platform-specific_attributes" />


#### <a name="platform-specific-attributes"></a>Atrybuty specyficzne dla platformy

Oprócz podstawowych elementów, które istnieją na wszystkich platformach konieczne będzie adres klucza platformy różnice w projekcie. Może być konieczne, należy wziąć pod uwagę (i pisanie kodu specjalnie w celu obsługi) te różnice:

-   **Rozmiary ekranu** — niektóre platformy (np. iOS i wcześniejszych wersji Windows Phone) mają znormalizowane rozmiary ekranu, które są stosunkowo prosta do docelowych. Urządzenia z systemem android mają szerokiej gamy ekranu wymiarów, które wymagają więcej nakładu pracy do obsługi w aplikacji.
-   **Nawigacja metafory** — różnią się między platformami (np.) przycisk "tył" sprzętu kontrolki interfejsu użytkownika Panorama) oraz w obrębie platform (Android 2 i 4 dla telefonu iPhone vs iPad).
-   **Klawiatury** — urządzenia z systemem Android — niektóre mają klawiatury fizycznych, a inne tylko klawiatury oprogramowania. Kod, który wykrywa klawiaturę nietrwałego jest przesłaniania część ekranu musi być wrażliwa na te różnice.
-   **Dotyk i gesty** — system operacyjny obsługuje rozpoznawania gestów zmienia się, szczególnie w starszych wersjach każdego systemu operacyjnego. Wcześniejszych wersjach systemu Android mają bardzo ograniczoną obsługę dotykową operacje, co oznacza, że obsługa starszych urządzeń mogą wymagać osobnego kodu
-   **Powiadomienia wypychane** — istnieją różne możliwości/implementacje na każdej z platform (np.) Kafelków dynamicznych na Windows).


 <a name="Device-specific_features" />


#### <a name="device-specific-features"></a>Funkcje specyficzne dla urządzenia

Określają, jakie musi być minimalne funkcje wymagane dla aplikacji; lub kiedy zdecyduj, jakie dodatkowe funkcje, aby móc korzystać z na każdej platformie. Kod będzie wymagane funkcje wykrywania i wyłączyć funkcje lub oferują alternatywne (np. alternatywa dla lokalizacji geograficznej może być pozostawiona użytkownikowi, wpisz lokalizację lub wybierz z mapy):

-   **Aparat** — funkcje różni się między urządzeniami: Niektóre urządzenia nie mają aparatu, inne osoby mają zarówno kamery skierowaną frontonu i tyłu. Niektóre aparaty fotograficzne są w stanie nagranie wideo.
-   **Lokalizacja geograficzna & mapy** — Obsługa lokalizacji GPS lub Wi-Fi nie jest dostępny w wszystkich urządzeń. Aplikacje muszą również obsługę dla różnych poziomów dokładności, który jest obsługiwany przez każdą z tych metod.
-   **Przyspieszeniomierz, Żyroskop i kompas** — te funkcje często znajdują się w tylko wybrane urządzenia na każdej platformie, dzięki czemu aplikacje prawie zawsze należy podać rezerwowe, gdy sprzęt nie jest obsługiwane.
-   **Twitter i Facebook** — tylko "wbudowane" iOS5 i iOS6 odpowiednio. Na wcześniejszych wersji i innych platform należy podać własne funkcje uwierzytelniania i współpracować bezpośrednio z każdego interfejsu API usług.
-   **W pobliżu komunikacji Zbliżeniowej (NFC)** — tylko (niektóre) na telefonach z systemem Android (w momencie pisania tego).


 <a name="Dealing_with_Platform_Divergence" />


### <a name="dealing-with-platform-divergence"></a>Obsługa Platform rozbieżności

Istnieją dwa różne podejścia do obsługi wielu platform z tego samego kodu — podstawy, każdy z swój własny zestaw zalety i wady.

-   **Abstrakcja platformy** — wzorzec fasady firm zapewnia ujednolicone dostęp między platformami i streszcza implementacji danej platformy w pojedynczą, jednolitą interfejs API.
-   **Implementacja nierównej** — wywołania określonej platformy funkcje za pośrednictwem nierównej implementacji przy użyciu architektury narzędzi, takich jak interfejsy i dziedziczenie lub kompilacji warunkowej.


 <a name="Platform_Abstraction" />


## <a name="platform-abstraction"></a>Abstrakcja platformy

 <a name="Class_Abstraction" />


### <a name="class-abstraction"></a>Klasa abstrakcji

Przy użyciu interfejsów lub klas bazowych zdefiniowane w udostępnionego kodu i zaimplementować lub rozszerzone w projektach specyficzne dla platformy. Pisanie i rozszerzanie udostępnionego kodu za pomocą klasy abstrakcje szczególnie jest odpowiedni dla biblioteki klas przenośnych, ponieważ mają ograniczonym podzbiorem dostępnej framework i nie może zawierać dyrektywy kompilatora do obsługi gałęzie kodu specyficznego dla platformy.

 <a name="Interfaces" />


#### <a name="interfaces"></a>Interfejsy

Przy użyciu interfejsów pozwala na Implementowanie klas specyficznych dla platformy, które nadal mogą być przekazywane do bibliotek udostępnionych z zalet wspólny kod.

Interfejs jest definiowana w kodzie udostępnionych i przekazany do biblioteki udostępnionej, jako parametr lub właściwość.

Aplikacje specyficzne dla platformy można następnie zaimplementuj interfejs i nadal korzystać z udostępnionego kodu ' "go przetworzyć.

 **Zalety**

Implementacja może zawierać kod specyficzny dla platformy i nawet odwołania do bibliotek zewnętrznych specyficzne dla platformy.

 **Wady**

O utworzyć i przekazać implementacji do udostępnionego kodu. Jeśli ten interfejs jest używany głębokiego w ramach udostępnionego kodu następnie kończy się ono są przekazywane za pomocą wielu parametrów metody lub w przeciwnym razie przekazana z łańcuchem wywołań. Jeśli wiele różnych interfejsów korzysta z udostępnionego kodu następnie one muszą wszystkie być tworzone i ustawić w jakimś miejscu udostępnionego kodu.

 <a name="Inheritance" />


#### <a name="inheritance"></a>Dziedziczenie

Udostępniony kod implementacji klasy abstrakcyjne lub wirtualne, które mogłyby zostać rozszerzone w jeden lub więcej projektów specyficznych dla platformy. Ta opcja działa podobnie do korzystania z interfejsów, ale niektóre zachowania już wdrożone. Istnieją różne punkty widzenia na to, czy interfejsy lub dziedziczenia są lepsze uzasadnienie wyboru tych elementów: w szczególności ponieważ języka C# dopuszcza tylko pojedyncze dziedziczenie go dyktowanie sposób swoje interfejsy API mogą być projektowane w przyszłości. Dziedziczenie należy używać ostrożnie.

Zalety i wady interfejsów stosuje się jednakowo do dziedziczenia z dodatkową zaletę, że klasa bazowa może zawierać implementację kodu (być może platformy niezależny od implementacji można opcjonalnie rozszerzać).

<a name="Xamarin.Forms" />

### <a name="xamarinforms"></a>Xamarin.Forms

Zobacz [Xamarin.Forms](~/xamarin-forms/get-started/index.md) dokumentacji.


### <a name="plug-in-cross-platform-functionality"></a>Wtyczka funkcji dla wielu Platform

Można także rozszerzyć aplikacje dla wielu platform w spójny sposób za pomocą wtyczki.

Połączone z naszych [wtyczki github](https://github.com/xamarin/plugins), większość wtyczki są typu open-source projektów (zwykle dostępne do zainstalowania za pomocą narzędzia Nuget), które ułatwiają implementacji różnych specyficznych dla platformy funkcje stav baterie ustawień z wspólny interfejs API, który można łatwo korzystać w aplikacjach platformy Xamarin i Xamarin.Forms.


<a name="Other_Cross-Platform_Libraries" />

### <a name="other-cross-platform-libraries"></a>Inne biblioteki dla wielu Platform

Istnieją różne dostępne, które zapewniają funkcjonalność międzyplatformowa 3rd bibliotek innych firm:

-   **MvvmCross** -  [https://github.com/slodge/MvvmCross/](https://github.com/slodge/MvvmCross/)
-   **Vernacular** (dla lokalizacji) —  [https://github.com/rdio/vernacular/](https://github.com/rdio/vernacular/)
-   **MonoGame** (dla gry XNA) —  [http://www.monogame.net](http://www.monogame.net)
-   **NGraphics** - [NGraphics](https://github.com/praeclarum/NGraphics) i jego prekursorów [https://github.com/praeclarum/CrossGraphics](https://github.com/praeclarum/CrossGraphics)


 <a name="Divergent_Implementation" />


### <a name="divergent-implementation"></a>Implementacja nierównej

 <a name="Conditional_Compilation" />


#### <a name="conditional-compilation"></a>Kompilacja warunkowa

Istnieją sytuacje, w którym ze współużytkowanym kodem nadal obowiązują działają inaczej na każdej platformie, prawdopodobnie uzyskiwania dostępu do klasy lub funkcje, które zachowują się inaczej. Kompilacja warunkowa działa najlepiej z udostępnionych projektów zasobów, gdzie w wielu projektach, które mają różne symboli zdefiniowanych odwołuje się tym samym pliku źródłowym.

Projekty Xamarin zawsze zdefiniować `__MOBILE__` czyli wartość true dla systemów iOS, jak i projektów aplikacji dla systemu Android (Uwaga podkreślenia podwójnej precyzji, przed i po awarii na te symbole).

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

<a name="iOS" />

##### <a name="ios"></a>iOS

Definiuje rozszerzenia Xamarin.iOS `__IOS__` używane do wykrywania urządzeń z systemem iOS.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

Dostępne są również symbole specyficznych dla wyrażenie kontrolne i TV:

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

Użyć następującego kodu, które mają być łączone tylko w aplikacji platformy Xamarin.Android

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

Każda wersja interfejsu API definiuje również nowej dyrektywy kompilatora, więc kod będzie dodawanie funkcji, jeśli są stosowane dla nowszych interfejsów API. Każdy poziom interfejsu API zawiera wszystkie symbole poziomu "niższe". Ta funkcja nie jest bardzo przydatne do obsługi wielu platform; Zazwyczaj `__ANDROID__` symbol okażą się wystarczające.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

##### <a name="mac"></a>Mac

Nie ma obecnie to symbol wbudowany dla platformy Xamarin.Mac, ale można dodać własne w Mac projektu aplikacji **Opcje > kompilacji > kompilatora** w **zdefiniować symbole** pole lub edytować **csproj**  pliku i Dodaj istnieje (na przykład `__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

<a name="Windows_Phone" />

##### <a name="windows-phone"></a>Windows Phone

Aplikacje Windows Phone definiuje dwa symbole — `WINDOWS_PHONE` i `SILVERLIGHT` — który może służyć do kodu do platformy docelowej. Te nie mają podkreślenia, w przypadku otaczających je podobnie jak symbole platformy Xamarin, aby zrobić.


<a name="Using_Conditional_Compilation" />

##### <a name="using-conditional-compilation"></a>Przy użyciu kompilacji warunkowej

Prosty przykład analiza przypadku kompilacja warunkowa jest ustawienie lokalizacji pliku dla pliku bazy danych SQLite. Trzy platformy mają nieco inne wymagania dotyczące określania lokalizacji pliku:

-   **iOS** — Apple preferuje danych niezwiązanych z użytkownikiem w określonej lokalizacji (katalog biblioteki), ale nie ma stałej systemu dla tego katalogu. Kod specyficzny dla platformy jest wymagany do kompilowania prawidłowej ścieżki.
-   **Android** — ścieżka systemu zwróconych przez `Environment.SpecialFolder.Personal` jest dopuszczalne lokalizacja pliku bazy danych.
-   **Windows Phone** — mechanizm wydzielonej pamięci masowej nie zezwala na pełną ścieżkę, należy określić tylko względną ścieżkę i nazwę pliku.
-   **Platforma Universal Windows** — używa `Windows.Storage` interfejsów API.

W poniższym kodzie użyto kompilacji warunkowej, aby upewnić się, `DatabaseFilePath` jest poprawna dla każdej platformy:

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

Wynik jest klasa, która może zostać utworzony i używane na wszystkich platformach, umieszczając plik bazy danych SQLite w innej lokalizacji na każdej platformie.

