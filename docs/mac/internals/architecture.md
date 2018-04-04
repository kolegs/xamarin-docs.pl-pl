---
title: Architektura Xamarin.Mac
description: Ten przewodnik opisuje Xamarin.Mac i jej zależności do języka Objective-C na niskim poziomie. Wyjaśniono pojęcia, takie jak kompilacji, selektorów, rejestratorów, uruchamianie aplikacji i generatora.
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: d6d7557fed5ea0ca0719dcbddbda316340645320
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinmac-architecture"></a>Architektura Xamarin.Mac

_Ten przewodnik opisuje Xamarin.Mac i jej zależności do języka Objective-C na niskim poziomie. Wyjaśniono pojęcia, takie jak kompilacji, selektorów, rejestratorów, uruchamianie aplikacji i generatora._

## <a name="overview"></a>Omówienie

Xamarin.Mac aplikacje uruchomione w ramach środowiska wykonawczego Mono i skompilować dół języku pośrednim (IL), która jest następnie Just-in-Time (JIT) skompilowanych do natywnego kodu w czasie wykonywania za pomocą platformy Xamarin dla kompilatora. Ta funkcja jest uruchamiana side-by-side ze środowiskiem uruchomieniowym języka Objective-C. Zarówno środowiska wykonawcze działają jądra podobnymi do systemu UNIX, w szczególności XNU i ujawnia różnych interfejsach API do kodu użytkownika, co umożliwia deweloperom źródłowy system natywnego lub zarządzanego dostępu do.

Na poniższym diagramie przedstawiono ogólne omówienie tej architektury:

[![Diagram przedstawiający ogólne omówienie architektury](architecture-images/mac-arch.png "Diagram przedstawiający ogólne omówienie architektury")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>Kodu natywnego i zarządzanego

Wdrażając aplikacje dla platformy Xamarin, warunki *natywnego* i *zarządzane* kodu są często używane. Kod zarządzany jest kod, który ma jego wykonanie zarządzane przez .NET Framework środowisko uruchomieniowe języka wspólnego lub w przypadku jego Xamarin: Mono środowiska uruchomieniowego.

Kod natywny jest kod, który będzie uruchamiany natywnie na danej platformie (na przykład Objective-C lub nawet drzewa obiektów aplikacji skompilowany kod, w układzie ARM). Ten przewodnik opisuje sposób jest skompilowana do kodu natywnego kodu zarządzanego i wyjaśniono sposób działania aplikacji Xamarin.Mac pełnego wykorzystania interfejsów API Mac firmy Apple przy użyciu powiązań, a jednocześnie również ma dostęp do. BCL w sieci i zaawansowane języka, takich jak C#.

## <a name="requirements"></a>Wymagania

Poniżej jest wymagany do opracowywania aplikacji macOS z Xamarin.Mac:

- Mac macOS uruchomionych Sierra (10.12) lub nowszej.
- Najnowszą wersję Xcode (zainstalowane z [sklepu z aplikacjami](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- Najnowszą wersję Xamarin.Mac i Visual Studio dla komputerów Mac

Uruchamianie aplikacji Mac utworzone za pomocą Xamarin.Mac mają następujące wymagania systemowe:

- Mac z systemem Mac OS X 10,7 lub nowszej.

## <a name="compilation"></a>Kompilacja

Podczas kompilowania aplikacji platformy Xamarin kompilatora Mono C# (lub F #) zostanie uruchomiony i będzie kompilowania kodu C# i F # na język pośredni firmy Microsoft (MSIL lub IL). Następnie używa Xamarin.Mac *tylko w czasie (JIT)* kompilatora w czasie wykonywania, aby skompilować kodu natywnego, dzięki czemu wykonanie na komputerze o prawidłowej architekturze zgodnie z potrzebami.

Dzięki temu nie trzeba Xamarin.iOS, który używa kompilacji drzewa obiektów aplikacji. Korzystając z drzewa obiektów aplikacji kompilatora, wszystkie zestawy i wszystkie metody w nich są kompilowane w czasie kompilacji. Z JIT kompilacja odbywa się na żądanie tylko dla metod, które są wykonywane.

Z aplikacjami Xamarin.Mac Mono jest zwykle osadzone w pakiecie aplikacji (i nazywany **osadzone Mono**). Korzystając z klasycznego interfejsu API Xamarin.Mac, zamiast tego można użyć aplikacji **Mono systemu**, jednak to nie jest obsługiwana w interfejsie API Unified. System Mono odwołuje się do Mono, który został zainstalowany w systemie operacyjnym. Po uruchomieniu aplikacji Xamarin.Mac app zostanie ona użyta.

## <a name="selectors"></a>Selektorów.

Za pomocą platformy Xamarin, będziemy mieć dwa oddzielne ekosystemami .NET i Apple, czego potrzebujemy, aby przenieść ze sobą w celu wydaje się jako prostsze, jak to możliwe upewnić się, że jej celem jest smooth użytkowników. Firma Microsoft już wspomniano w sekcji powyżej sposób komunikacji dwóch środowisk uruchomieniowych i bardzo dobrze znany terminu "powiązania", dzięki czemu macierzystych interfejsów API Mac do użycia w programie Xamarin. Powiązania są objaśnione szczegółowo w [dokumentacji powiązanie Objective-C](~/mac/platform/binding.md), więc na razie Przyjrzyjmy się, jak działa Xamarin.Mac kulisy.

Po pierwsze musi istnieć sposób ujawniać Objective-C, aby C#, który odbywa się za pośrednictwem selektorów. Selektora jest komunikat, który jest wysyłany do obiektu lub klasy. Z języka Objective-C odbywa się za pośrednictwem [objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html) funkcji. Aby uzyskać więcej informacji na temat używania selektorów dotyczą systemu iOS [selektorów Objective-C](~/ios/internals/objective-c-selectors.md) przewodnik. Istnieje również musi być sposób ujawniać zarządzanego kodu języka Objective-C, który jest bardziej skomplikowany z faktu, że Objective-C nie może ustalić niczego o kodu zarządzanego. Aby uzyskać obejścia tego problemu, możemy użyć [rejestratora](~/mac/internals/registrar.md). To co omówiono bardziej szczegółowo w następnej sekcji.

## <a name="registrar"></a>Rejestrator

Jak wspomniano powyżej, rejestratora jest kodem ujawnia kod zarządzany do Objective-C. Dzieje się tak, tworząc listę co zarządzanej klasy, która jest pochodną NSObject:

- Dla wszystkich klas, które nie są zawijania istniejącej klasy Objective-C, tworzy nową klasę języka Objective-C z elementami członkowskimi Objective-C dublowania wszystkich zarządzanych elementów członkowskich, które mają `[Export]` atrybutu.
- W implementacji dla każdego elementu członkowskiego w języku Objective-C kod jest automatycznie dodawany do wywołania dublowany zarządzanego elementu członkowskiego.

Pseudo-poniższy kod przedstawia przykład jak to zrobić:

**C# (zarządzany kod):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**Objective-C (kod natywny):**

```objc
@interface MyViewController : UIViewController
 - (void)myFunc;
@end 

@implementation MyViewController
- (void)myFunc {
    // Code to call the managed C# MyFunc method in MyViewController
}
@end
```

Zarządzany kod może zawierać atrybutów, `[Register]` i `[Export]`, używającej rejestratora, aby dowiedzieć się, że obiekt musi zostać uwidoczniona dla Objective-C. Atrybut [rejestru] jest używany do określenia nazwy wygenerowanej klasy Objective-C, w przypadku, gdy nazwa generowany domyślnie nie jest odpowiedni. Wszystkie klasy wywodzące się z NSObject zostaną automatycznie zarejestrowane Objective-C. Wymagany atrybut [eksportu] zawiera ciąg, czyli selektora używane w wygenerowanej klasy Objective-C.

Istnieją dwa typy rejestratorów używane w Xamarin.Mac — dynamiczną i statyczną:

- Dynamiczne rejestratorów — to rejestratora domyślnego dla wszystkich Xamarin.Mac kompilacji. Dynamiczne rejestratora wykonuje rejestrację wszystkich typów w zestawie użytkownika w czasie wykonywania. Robi to przy użyciu funkcje udostępniane przez Objective-C runtime interfejsu API. Dynamiczne Rejestrator ma w związku z tym uruchomienia wolniej, ale szybszą czas kompilacji. Funkcje natywne (zwykle w C) o nazwie trampolines, są używane jako implementacje metod, używając rejestratorów dynamicznych. Różnią się od różnych architektur.
- Statyczne rejestratorów — statyczne rejestratora generuje kod języka Objective-C podczas kompilacji, które następnie są kompilowane do biblioteki statycznej i połączonych w pliku wykonywalnego. Umożliwia szybsze uruchamianie, ale trwa dłużej, w czasie kompilacji.

## <a name="application-launch"></a>Uruchamianie aplikacji

Logika uruchamiania Xamarin.Mac będą się różnić w zależności od czy osadzone lub system Mono jest używany. Aby wyświetlić kod i kroki Xamarin.Mac uruchamiania aplikacji, zapoznaj się [nagłówka uruchamiania](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) pliku w repozytorium publicznych xamarin macios.

## <a name="generator"></a>Generator

Xamarin.Mac zawiera definicje dla komputerów Mac każdego interfejsu API. Można przeglądać żadnego z tych adresów na [repozytorium github MaciOS](https://github.com/xamarin/xamarin-macios/tree/master/src). Te definicje zawierać interfejsów z atrybutami, a także niezbędne metody i właściwości. Na przykład następujący kod jest służy do definiowania NSBox w [przestrzeni nazw AppKit](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526). Zwróć uwagę, że jest to interfejs z wielu metod i właściwości:

```csharp
[BaseType (typeof (NSView))]
public interface NSBox {

        …

        [Export ("borderRect")]
        CGRect BorderRect { get; }

        [Export ("titleRect")]
        CGRect TitleRect { get; }

        [Export ("titleCell")]
        NSObject TitleCell { get; }

        [Export ("sizeToFit")]
        void SizeToFit ();

        [Export ("contentViewMargins")]
        CGSize ContentViewMargins { get; set; }

        [Export ("setFrameFromContentFrame:")]
        void SetFrameFromContentFrame (CGRect contentFrame);

        …

}
```

Generator o nazwie `bmac` w Xamarin.Mac, wymaga tych plików definicji i używa .NET narzędzia do kompilowania ich do tymczasowego zestawu. Jednak ten zestaw tymczasowy nie jest niemożliwe, aby wywoływać kod języka Objective-C. Generator następnie odczytuje tymczasowego zestawu i generuje kod C#, który może być używany w czasie wykonywania. Jest to, dlaczego, na przykład jeśli dodasz losowe atrybut do pliku definicji .cs go nie będą widoczne w outputted kodu. Generator nie wiesz i w związku z tym `bmac` nie może ustalić, aby wyszukać w zestawie tymczasowych, należy przeprowadzić.

Po utworzeniu Xamarin.Mac.dll Pakowarka `mmp`, będzie powiązać ze sobą wszystkie składniki.

Na wysokim poziomie powoduje to osiągnięcie, wykonując następujące zadania:

- Utwórz strukturę pakietu aplikacji.
- Kopiowanie z zarządzanych zestawów.
- Jeśli włączono łączenie Uruchom zarządzanych konsolidator, aby zoptymalizować ponownie zestawy przez usunięcie części nieużywane.
- Utwórz aplikację uruchamiania, łączenie w kodzie uruchamiania omówiono wraz z kodem registar, jeśli w trybie statycznym.

Jest to, a następnie uruchom jako część użytkownika proces, która kompiluje kod użytkownika do zestawu kompilacji, Xamarin.Mac.dll i działa tego odwołania `mmp` dokonanie pakietu

Aby uzyskać szczegółowe informacje o konsolidator i sposobie ich użycia, dotyczą systemu iOS [konsolidatora](~/ios/deploy-test/linker.md) przewodnik.

## <a name="summary"></a>Podsumowanie

W tym przewodniku przeglądał kompilacji aplikacji Xamarin.Mac i eksplorowanych Xamarin.Mac i ich związek z Objective-C.
