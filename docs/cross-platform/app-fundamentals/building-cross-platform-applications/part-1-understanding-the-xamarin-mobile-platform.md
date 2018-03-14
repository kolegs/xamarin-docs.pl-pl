---
title: "Część 1 — Opis platformy Xamarin Mobile"
ms.topic: article
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: d90cd8ae620f51d7ea9bac0945ccc7ec4b83bac4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>Część 1 — Opis platformy Xamarin Mobile

Platformy Xamarin składa się z liczbą elementów, które umożliwiają opracowywanie aplikacji dla systemów iOS i Android:

-   **W języku C#** — umożliwia przy użyciu zwykłego składni i zaawansowane funkcje, takie jak ogólne, LINQ i Biblioteka zadań równoległych.
-   **Mono .NET framework** — udostępnia implementację i platform szeroką gamę funkcji firmy Microsoft .NET framework.
-   **Kompilator** — w zależności od platformy, tworzy aplikacji natywnej (np.) System iOS) lub zintegrowanej aplikacji .NET, jak i środowiska uruchomieniowego (np.) Android). Kompilator wykonuje także wiele optymalizacje przenośnych wdrożenia, takie jak łączenie zadań używane bez kodu.
-   **Narzędzia IDE** — programu Visual Studio na Mac i systemu Windows pozwala na tworzenie, tworzenie i wdrażanie projektów Xamarin.


Ponadto ponieważ podstawowe języka C# w środowisku .NET Framework, udostępnianie kodu, który można także wdrożyć Windows Phone można struktury projektów.

 <a name="Under_the_Hood" />


## <a name="under-the-hood"></a>Kulisy

Mimo że Xamarin umożliwia pisanie aplikacji w języku C# oraz udostępniać ten sam kod na wielu platformach, różni się rzeczywistego wykonania w każdym systemie.

 <a name="Compilation" />


## <a name="compilation"></a>Kompilacja

Źródło C# sprawia, że drodze do natywnej aplikacji na bardzo różne sposoby na każdej platformie:

-   **iOS** — C# jest z wyprzedzeniem o czasu (drzewa obiektów aplikacji) do języka asemblera ARM. .NET framework jest dostępny, z klasami nieużywane trwa wycięte podczas konsolidacji w celu zmniejszenia rozmiaru aplikacji. Apple nie zezwala generowanie kodu w czasie wykonywania w systemie iOS, więc niektóre funkcje języka nie są dostępne (zobacz [ograniczenia Xamarin.iOS](~/ios/internals/limitations.md) ).
-   **Android** — C# są kompilowane do IL i wchodzących w skład MonoVM + JIT'ing. Nieużywane klas w ramach zostaną wycięte podczas konsolidacji. Aplikacji działa side-by-side ozdobione Java / (Android środowiska wykonawczego) i współdziała z natywnych typów za pomocą JNI (zobacz [ograniczenia Xamarin.Android](~/android/internals/limitations.md) ).
-   **Windows** — C# ma być kompilowana IL wykonywane przez wbudowane środowisko uruchomieniowe i nie wymaga narzędzia Xamarin. Projektowanie aplikacji systemu Windows następujące Xamarin wskazówki uproszczono do ponownego użycia kodu w systemach iOS i Android.
  Należy pamiętać, że platforma uniwersalna systemu Windows ma również **platformy .NET Native** opcję, która działa podobnie do kompilacji drzewa obiektów aplikacji platformy Xamarin.iOS.


W dokumentacji konsolidatora [Xamarin.iOS](~/ios/deploy-test/linker.md) i [Xamarin.Android](~/android/deploy-test/linker.md) zawiera więcej informacji na temat tego część procesu kompilacji.

Środowisko uruchomieniowe "kompilacja" — Generowanie kodu dynamicznie z `System.Reflection.Emit` — należy unikać.

Generowanie kodu dynamicznych na urządzeniach z systemem iOS uniemożliwia jądra firmy Apple, w związku z tym emitowanie kodu na bieżąco, nie będzie działać w platformy Xamarin.iOS. Podobnie funkcji środowiska Dynamic Language Runtime nie można używać z narzędziami Xamarin.

Niektóre funkcje odbicia działać (np.) MonoTouch.Dialog używa go do interfejsu API odbicia), po prostu nie generowanie kodu.

 <a name="Platform_SDK_Access" />


## <a name="platform-sdk-access"></a>Dostęp do zestawu SDK platformy

Xamarin sprawia, że funkcje oferowane przez SDK specyficzne dla platformy łatwo dostępne z zwykłego składni języka C#:

-   **iOS** — Xamarin.iOS udostępnia firmy Apple CocoaTouch zestawu SDK platformy jako przestrzenie nazw, które można odwoływać się w języku C#. Na przykład framework UIKit, który zawiera wszystkie kontrolki interfejsu użytkownika można dołączyć z prostą `using UIKit;` instrukcji.
-   **Android** — platformy Xamarin.Android udostępnia zestaw SDK systemu Android firmy Google jako przestrzenie nazw, więc możesz odwoływać się do dowolnej części obsługiwanym zestawem SDK przy użyciu instrukcji, takich jak `using Android.Views;` na dostęp do formantów interfejsu użytkownika.
-   **Windows** — aplikacje systemu Windows są tworzone za pomocą programu Visual Studio w systemie Windows. Typy projektów obejmują formularzy systemu Windows, WPF WinRT i platformy uniwersalnej systemu Windows (UWP).


 <a name="Seamless_Integration_for_Developers" />


## <a name="seamless-integration-for-developers"></a>Integracja dla deweloperów

Zaletą programu Xamarin jest, że mimo różnic pod maską Xamarin.iOS i Xamarin.Android (połączone z zestawów SDK systemu Windows firmy Microsoft) oferują bezproblemowe dotyczące pisania kodu C#, które mogą być ponownie używane na wszystkich platformach trzy.

Logiki biznesowej, użycia bazy danych, dostępu do sieci i innych typowych funkcji można zapisać raz i ponownie używany na każdej platformie podstaw dla interfejsów użytkownika specyficzne dla platformy, które wyglądają i przeprowadź natywnych aplikacji.

 <a name="Integrated_Development_Environment_(IDE)_Availability" />


## <a name="integrated-development-environment-ide-availability"></a>Programowanie zintegrowane środowisko (IDE) dostępności

Programowanie Xamarin może odbywać się w programie Visual Studio na Mac lub Windows. IDE wybranego będzie określana przez platformy, z którego mają być docelowe.

Ponieważ aplikacje systemu Windows mogą być opracowane tylko w systemie Windows, aby utworzyć dla systemu iOS, Android, _i_ system Windows wymaga programu Visual Studio dla systemu Windows. Jednak jest możliwe udostępnianie projektów i plików między komputerami z systemem Windows i Mac, iOS i Android aplikacje mogą być wbudowane na komputerze Mac i udostępnionych kodu później można było dodać do projektu systemu Windows.

Programowanie wymagania dotyczące każdej platformy omówiono bardziej szczegółowo w [wymaganie](~/cross-platform/get-started/requirements.md) przewodnik.


<a name="iOS" />

### <a name="ios"></a>iOS

Tworzenie aplikacji systemu iOS wymaga komputera Mac, z programem System macOS. Visual Studio umożliwia także zapisać i wdrożyć aplikacje dla systemu iOS za pomocą platformy Xamarin w programie Visual Studio. Jednak Mac są nadal potrzebne do celów licencjonowania i kompilacji.

Kompilator i symulatora do testowania musi być zainstalowany IDE Xcode firmy Apple. Możesz przetestować na własnych urządzeniach [bezpłatnie](~/ios/get-started/installation/device-provisioning/free-provisioning.md), ale aby tworzyć aplikacje dla dystrybucji (np. w sklepie App Store) należy dołączyć programie dla deweloperów firmy Apple ($99 USD na rok). Za każdym razem przesyłania lub aktualizację aplikacji, musi być sprawdzone i zatwierdzone przez firmę Apple, zanim staje się dostępna dla klientów do pobrania.

Kod jest napisany za pomocą środowiska IDE programu Visual Studio i może być skompilowany programowo lub edytować za pomocą platformy Xamarin dla systemu iOS projektanta w IDE albo układów ekranu głównego.

Zapoznaj się [Przewodnik instalacji Xamarin.iOS](~/ios/get-started/installation/index.md) szczegółowe informacje dotyczące pobierania Konfigurowanie.

<a name="Android" />

### <a name="android"></a>Android

Tworzenie aplikacji systemu android wymaga Java i zestawów Android SDK do zainstalowania. Określają one, kompilator, emulatora i inne narzędzia niezbędne do tworzenia, wdrażania i testowania. Narzędzia Java, zestaw SDK systemu Android firmy Google i na platformie Xamarin można wszystkie instalować i uruchamiać na następujące konfiguracje:

-  System Mac OS X El Capitan i powyżej (10.11 +) z programem Visual Studio dla komputerów Mac
-  Windows 7 & powyżej z programu Visual Studio 2015 lub 2017


Xamarin zapewnia ujednolicone Instalator, który będzie Skonfiguruj system z wstępnych języka Java, Android i Xamarin narzędzi (w tym wizualnego projektanta dla układów ekranu). Zapoznaj się [Przewodnik instalacji platformy Xamarin.Android](~/android/get-started/installation/index.md) szczegółowe informacje.

Możesz skompilować i testowania aplikacji na urządzeniu rzeczywistych bez licencji Google, jednak do dystrybucji aplikacji za pośrednictwem sklepu (takich jak Google Play, Amazon lub Barnes &amp; Noble) Opłata rejestracji mogą być należne dla operatora. Google Play zostaną natychmiast, publikowanie aplikacji, chociaż w innych magazynach proces zatwierdzania podobne do firmy Apple.

 <a name="Windows_Phone" />


### <a name="windows"></a>Windows

Aplikacje systemu Windows (WinForms WPF i platformy uniwersalnej systemu Windows) są tworzone za pomocą programu Visual Studio. Nie używają one Xamarin bezpośrednio. Jednak kod w języku C# mogą być współużytkowane przez system Windows, iOS i Android.
Odwiedź firmy Microsoft [Centrum deweloperów](https://developer.microsoft.com/) Aby dowiedzieć się więcej na temat narzędzi wymaganych dla systemu Windows dla deweloperów.

 <a name="Creating_the_User_Interface_(UI)" />


## <a name="creating-the-user-interface-ui"></a>Tworzenie interfejsu użytkownika (UI)

Najważniejszą korzyścią z używania Xamarin jest, że interfejs użytkownika aplikacji korzysta natywnych kontrolek w każdej z platform, tworzenie aplikacji, które są nierozróżnialne z aplikacji napisanych w języku Objective-C lub Java (dla systemów iOS i Android odpowiednio).

Podczas kompilowania ekranów w aplikacji, można ułożyć formantów w kodzie lub tworzenie ekranów pełną przy użyciu narzędzi do projektowania dostępnych dla każdej platformy.

 <a name="Programmatically_Create_Controls" />


### <a name="create-controls-programmatically"></a>Programowe tworzenie formantów

Każdej z platform umożliwia użytkownikowi formantów interfejsu do dodania do ekranu przy użyciu kodu. Może to być bardzo czasochłonne, ponieważ może być trudne do wizualizacji gotowego projektu po kodować pikseli współrzędne położenia kontrolki i rozmiary.

Programowe tworzenie formantów przynosi korzyści, szczególnie w systemie iOS tworzenia widoków, których rozmiar lub inaczej renderowane między rozmiaru ekranu urządzenia iPhone i iPad.

 <a name="Visual_Designer" />


### <a name="visual-designer"></a>Projektant graficzny

Dotyczy wszystkich platform inną metodę wizualnie układania ekrany:

-   **iOS** — na platformie Xamarin iOS projektanta ułatwia tworzenie widoków przy użyciu przeciągnij i upuść pola funkcjonalność i właściwości. Zbiorczo te widoki tworzą scenorysu i jest dostępny w **. Scenorysu** pliku, który znajduje się w projekcie.
-   **Android** — Xamarin zapewnia Android projektanta przeciągania i upuszczania w interfejsu użytkownika dla programu Visual Studio. Układów ekranu android są zapisywane jako **. AXML** pliki, korzystając z narzędzia Xamarin.
-   **Windows** — firma Microsoft udostępnia interfejs użytkownika projektanta przeciągania i upuszczania w Visual Studio i Blend. Układów ekranu głównego są przechowywane jako. Pliki XAML.


Te zrzuty ekranu przedstawiają dostępnych projektantów visual ekranu na każdej platformie:

 [ ![](part-1-understanding-the-xamarin-mobile-platform-images/designer-all1.png "Te zrzuty ekranu Pokaż dostępnych projektantów visual ekranu na każdej platformie")](part-1-understanding-the-xamarin-mobile-platform-images/designer-all1.png#lightbox)

We wszystkich przypadkach elementy wizualne utworzonych może być przywoływany w kodzie.

 <a name="User_Interface_Considerations" />


### <a name="user-interface-considerations"></a>Uwagi dotyczące interfejsu użytkownika

Najważniejszą korzyścią z używania Xamarin do tworzenia aplikacji międzyplatformowego jest, aby można było korzystać z natywnych narzędzi interfejsu użytkownika do przedstawienia znanego interfejsu użytkownika. Interfejs użytkownika przeprowadzenia tak szybko, jak innych aplikacji natywnej.

Niektóre metafory interfejsu użytkownika działa na wielu platformach (na przykład wszystkich platform trzech użyć podobne kontrolki przewijania listy) ale w kolejności dla aplikacji "uznać" prawo interfejsu użytkownika należy korzystać z użytkownika specyficzne dla platformy elementów interfejsu, gdy jest to konieczne. Przykłady specyficzne dla platformy metafory interfejsu użytkownika:

-   **iOS** — hierarchiczna nawigacja z użyciem nietrwałego przycisku Wstecz, tabulatorów w dolnej części ekranu.
-   **Android** — / system — oprogramowanie kopii przycisku, w menu Akcja, kartę w górnej części ekranu.
-   **Windows** — aplikacje systemu Windows można uruchamiać na komputery stacjonarne, tabletów (takich jak Microsoft Surface) i telefonów. Urządzenia z systemem Windows 10 mogą mieć sprzętu przyciski Wstecz i dynamiczne Kafelki, na przykład.


Zaleca się przeczytanie odpowiednich dla platform docelowych zaleceń dotyczących projektowania:

-   **iOS** — [firmy Apple Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
-   **Android** — [dotyczące interfejsu użytkownika firmy Google](http://developer.android.com/guide/practices/ui_guidelines/index.html)
-   **Windows** — [wskazówki dotyczące projektowania czynności użytkownika dla systemu Windows](https://developer.microsoft.com/en-us/windows/design)


 <a name="Library_and_Code_Re-use" />


## <a name="library-and-code-re-use"></a>Biblioteki i ponowne wykorzystywanie kodu

Platformy Xamarin umożliwia ponowne wykorzystanie istniejącego kodu C# dla wszystkich platform, a także integrację bibliotek natywnie napisane dla każdej platformy.

 <a name="C#_Source_and_Libraries" />


### <a name="c-source-and-libraries"></a>C# źródła i biblioteki

Ponieważ C# i .NET framework za pomocą platformy Xamarin produktów, partii istniejącego kodu źródłowego (zarówno Otwórz źródła i wewnętrznych projektów) mogą być ponownie używane w projektach Xamarin.iOS lub platformy Xamarin.Android. Często źródło może po prostu dodane do rozwiązania Xamarin i będzie ona działać natychmiast. Użycie nieobsługiwanej funkcji .NET framework może być wymagane niektórych ulepszeń.

Przykłady źródła C#, które mogą być używane w Xamarin.iOS lub Xamarin.Android: SQLite NET, NewtonSoft.JSON i SharpZipLib.

 <a name="Objective-C_Bindings_+_Binding_Projects" />


### <a name="objective-c-bindings--binding-projects"></a>Powiązania Objective-C + powiązania projektów

Xamarin zawiera narzędzie *btouch* , pomaga utworzyć powiązania umożliwiających bibliotek języka Objective-C do użycia w projektach platformy Xamarin.iOS. Zapoznaj się [dokumentacji powiązanie typów języka Objective-C](~/cross-platform/macios/binding/binding-types-reference.md) szczegółowe informacje na temat jak to zrobić.

Przykłady biblioteki języka Objective-C, które mogą być używane w Xamarin.iOS: kod kreskowy RedLaser skanowanie integracji usługi Google Analytics i PayPal. Open source platformy Xamarin.iOS powiązania są dostępne na [github](https://github.com/mono/monotouch-bindings).

 <a name=".jar_Bindings_+_Binding_Projects" />


### <a name="jar-bindings--binding-projects"></a>powiązania JAR + powiązania projektów

Program Xamarin obsługuje korzystanie z istniejących bibliotek języka Java w platformy Xamarin.Android. Zapoznaj się [powiązanie dokumentacji biblioteka języka Java](~/android/platform/binding-java-library/index.md) szczegółowe informacje dotyczące sposobu używania. Plik JAR z platformy Xamarin.Android.

Open source platformy Xamarin.Android powiązania są dostępne na [github](https://github.com/mono/monodroid-bindings).

 <a name="C_via_PInvoke" />


### <a name="c-via-pinvoke"></a>C za pomocą funkcji PInvoke

Technologia "Platformy Invoke" (P/Invoke) umożliwia kodu zarządzanego (C#) można wywoływać metod w bibliotekach natywnego, a także obsługę natywnych bibliotek do wywołania do kodu zarządzanego.

Na przykład [SQLite NET](https://github.com/praeclarum/sqlite-net) biblioteka używa instrukcji w następujący sposób:

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

To wiąże się z implementacji native SQLite języka C w systemach iOS i Android.
Deweloperzy zapoznać się z istniejącą API C można konstruować zestaw klas C# do mapowania do natywnego interfejsu API i korzystać z istniejącego kodu platformy. Brak dokumentacji [łączenie natywnych bibliotek](~/ios/platform/native-interop.md) dotyczą w Xamarin.iOS, Xamarin.Android podobne zasady.

 <a name="C++_via_Cxxi" />


### <a name="c-via-cppsharp"></a>C++ via CppSharp

Miguel wyjaśniono CXXI (nazywane teraz [CppSharp](https://github.com/mono/CppSharp)) na jego [blogu](http://tirania.org/blog/archive/2011/Dec-19.html). Zamiast powiązanie bezpośrednio do biblioteki C++ jest utworzenie otoki C i powiązać który przy użyciu elementu P/Invoke.

