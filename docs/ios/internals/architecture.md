---
title: Architektura systemu iOS
description: Eksploracja Xamarin.iOS na niskim poziomie
ms.topic: article
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: bf9d292acf43bbbe3e4ba76b5a264a11288b7225
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="ios-architecture"></a>Architektura systemu iOS

Aplikacji platformy Xamarin.iOS wykonane w środowisku wykonywania Mono i użyj pełnej kompilacji z wyprzedzeniem o czasie (drzewa obiektów aplikacji) do kompilowania kodu C# na język asemblera ARM. Ta funkcja jest uruchamiana side-by-side z [Objective-C Runtime](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/). Zarówno środowiska wykonawcze działają jądra podobnymi do systemu UNIX, w szczególności [XNU](https://en.wikipedia.org/wiki/XNU), uwidaczniać różnych interfejsach API do kodu użytkownika, co umożliwia deweloperom możliwość dostępu do podstawowego systemu natywnego lub zarządzanego.

Na poniższym diagramie przedstawiono ogólne omówienie tej architektury:

[ ![](architecture-images/ios-arch-small.png "Ten diagram przedstawia ogólne omówienie architektury kompilacji wyprzedzeniem o czasie (drzewa obiektów aplikacji)")](architecture-images/ios-arch.png)

## <a name="native-and-managed-code-an-explanation"></a>Natywnego i kodu zarządzanego: wyjaśnienie

Wdrażając aplikacje dla platformy Xamarin warunki *natywnych i zarządzanych* kodu są często używane. [Kod zarządzany](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/) jest kod, który ma ona zarządzana przez [środowisko uruchomieniowe języka wspólnego .NET Framework](https://msdn.microsoft.com/en-us/library/8bs2ecf4(v=vs.110).aspx), lub w przypadku jego Xamarin: Mono środowiska uruchomieniowego. Jest to określane mianem język pośredni.

Kod natywny jest kod, który będzie uruchamiany natywnie na danej platformie (na przykład Objective-C lub nawet drzewa obiektów aplikacji skompilowany kod, w układzie ARM). Ten przewodnik opisuje sposób drzewa obiektów aplikacji kompiluje kodu zarządzanego do kodu natywnego i wyjaśniono sposób działania aplikacji platformy Xamarin.iOS pełnego wykorzystania firmy Apple iOS interfejsów API przy użyciu powiązań, a jednocześnie ma również dostęp do. BCL w sieci i zaawansowane języka, takich jak C#.


## <a name="aot"></a>AOT

Podczas kompilowania aplikacji platformy Xamarin kompilatora Mono C# (lub F #) zostanie uruchomiony i kompilowania kodu C# i F # do firmy Microsoft pośredniego Language (MSIL). Jeśli używasz platformy Xamarin.Android, aplikacja Xamarin.Mac lub nawet aplikacji platformy Xamarin.iOS w symulatorze, [.NET środowiska uruchomieniowego języka wspólnego (CLR)](https://msdn.microsoft.com/en-us/library/8bs2ecf4(v=vs.110).aspx) kompiluje MSIL przy użyciu tylko w kompilatorze czas (JIT). W czasie wykonywania, który to jest kompilowany do kodu natywnego, które można uruchamiać na architektury aplikacji.

Istnieje jednak ograniczenia zabezpieczeń w systemach iOS, ustaw przez firmę Apple, która nie zezwala na wykonywanie kodu dynamicznie generowanym na urządzeniu.
Aby upewnić się, że firma Microsoft stosować się do tych protokołów bezpieczeństwa, Xamarin.iOS użyje kompilatora wyprzedzeniem o czasie (drzewa obiektów aplikacji) do kompilowania kodu zarządzanego. Tworzy natywnego systemu iOS binarny, opcjonalnie zoptymalizowane z LLVM dla urządzeń, które można wdrożyć na procesor oparty na architekturze ARM firmy Apple. Poniżej przedstawiono diagram nierównej jak to dopasowuje razem:

[ ![](architecture-images/aot.png "Jak to dopasowuje razem nierównej diagram")](architecture-images/aot-large.png)

Przy użyciu drzewa obiektów aplikacji zawiera szereg ograniczeń, które opisano szczegółowo w [ograniczenia](~/ios/internals/limitations.md) przewodnik. Zapewnia także szereg ulepszeń za pośrednictwem JIT poprzez skrócenie czasu uruchomienia i różnych optymalizacji wydajności

Teraz, gdy będziemy mieć zbadane, sposobu kompilowania kodu ze źródła do kodu natywnego, Spójrzmy pod maską, aby zobaczyć, jak Xamarin.iOS pozwala pisać aplikacje pełni natywną systemu iOS

## <a name="selectors"></a>Selektorów.

Za pomocą platformy Xamarin, będziemy mieć dwa oddzielne ekosystemami .NET i Apple, czego potrzebujemy, aby przenieść ze sobą w celu wydaje się jako prostsze, jak to możliwe upewnić się, że jej celem jest smooth użytkowników. Firma Microsoft już wspomniano w sekcji powyżej sposób komunikacji dwóch środowisk uruchomieniowych i bardzo dobrze znany terminu "powiązania", dzięki czemu natywnych interfejsów API do użycia w programie Xamarin dla systemu iOS. Powiązania są objaśnione szczegółowo w naszym [powiązania Objective-C](~/cross-platform/macios/binding/overview.md) dokumentacji, więc na razie Przyjrzyjmy się jak iOS działa pod maską.

Po pierwsze musi istnieć sposób ujawniać Objective-C, aby C#, który odbywa się za pośrednictwem selektorów. Selektora jest komunikat, który jest wysyłany do obiektu lub klasy. Z języka Objective-C odbywa się za pośrednictwem [objc_msgSend](~/cross-platform/macios/binding/overview.md) funkcji.
Aby uzyskać więcej informacji na temat używania selektorów dotyczą [selektorów Objective-C](~/ios/internals/objective-c-selectors.md) przewodnik. Istnieje również musi być sposób ujawniać zarządzanego kodu języka Objective-C, który jest bardziej skomplikowany z faktu, że Objective-C nie może ustalić niczego o kodu zarządzanego. Aby uzyskać obejścia tego problemu, możemy użyć *rejestratorów*. Te są co omówiono bardziej szczegółowo w następnej sekcji.

## <a name="registrars"></a>Rejestratorów

Jak wspomniano powyżej, rejestratora jest kodem ujawnia kod zarządzany do Objective-C. Dzieje się tak, tworząc listę co zarządzanej klasy, która jest pochodną NSObject:

- Dla wszystkich klas, które nie są zawijania istniejącej klasy Objective-C, tworzy nową klasę języka Objective-C z elementami członkowskimi Objective-C dublowania wszystkich zarządzanych elementów członkowskich, które mają [`Export`] atrybutu.

- W implementacji dla każdego elementu członkowskiego w języku Objective-C kod jest automatycznie dodawany do wywołania dublowany zarządzanego elementu członkowskiego.

Pseudo-poniższy kod przedstawia przykład jak to zrobić:

**C# (zarządzany kod)**

```

 class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }


```

**Objective-C:**

```csharp
@interface MyViewController : UIViewController { }

    -(void)myFunc;
@end @implementation

    MyViewController {}
    -(void) myFunc
    {
    /* code to call the managed MyViewController.MyFunc method */
    }
@end

```

Zarządzany kod może zawierać atrybutów, `[Register]` i `[Export]`, używającej rejestratora, aby dowiedzieć się, że obiekt musi zostać uwidoczniona dla Objective-C.
`[Register]` Atrybut służy do określenia nazwy wygenerowanej klasy Objective-C, w przypadku, gdy nazwa generowany domyślnie nie jest odpowiedni. Wszystkie klasy wywodzące się z NSObject zostaną automatycznie zarejestrowane Objective-C.
Wymagane `[Export]` atrybut zawiera ciąg, który jest selektora używane w wygenerowanej klasy Objective-C.

Istnieją dwa typy rejestratorów używane w Xamarin.iOS — dynamiczną i statyczną:


- **Dynamiczne rejestratorów** — dynamiczne rejestratora jest rejestrację wszystkich typów w zestawie użytkownika w czasie wykonywania. Robi to przy użyciu funkcji dostarczonych przez [Objective-C runtime interfejsu API](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/). Dynamiczne Rejestrator ma w związku z tym uruchomienia wolniej, ale szybszą czas kompilacji. Jest to domyślna symulatora systemu iOS. Funkcje natywne (zwykle w C) o nazwie trampolines, są używane jako implementacje metod, używając rejestratorów dynamicznych. Różnią się od różnych architektur.

- **Statyczne rejestratorów** — statyczne rejestratora generuje kod języka Objective-C podczas kompilacji, które następnie są kompilowane do biblioteki statycznej i połączonych w pliku wykonywalnego. Umożliwia szybsze uruchamianie, ale trwa dłużej, w czasie kompilacji. To jest używany domyślnie dla kompilacji urządzenia. Statyczne rejestratora można również używać razem symulatora systemu iOS przez przekazanie `--registrar:static` jako `mtouch` atrybutu w opcjach kompilacji projektu, jak pokazano poniżej:

    [ ![](architecture-images/image1.png "Ustawienie mtouch dodatkowe argumenty")](architecture-images/image1.png)

Aby uzyskać więcej informacji na temat systemu IOS, system rejestracji typ używany przez Xamarin.iOS dotyczą [rejestratora typu](~/ios/internals/registrar.md) przewodnik.

## <a name="application-launch"></a>Uruchamianie aplikacji

Punkt wejścia wszystkich plików wykonywalnych Xamarin.iOS jest udostępniany przez funkcję o nazwie `xamarin_main`, która inicjuje mono.

W zależności od typu projektu poniżej można to zrobić:

- Regularne iOS i aplikacji systemu tvOS nosi nazwę zarządzanego metodę Main, dostarczone przez aplikację systemu Xamarin. Zarządzane to metoda Main wywołuje `UIApplication.Main`, która jest punkt wejścia dla Objective-C. Powiązanie dla Objective C jest UIApplication.Main `UIApplicationMain` metody.
- Dla rozszerzeń, funkcji macierzystej — `NSExtensionMain` lub (`NSExtensionmain` dla rozszerzeń WatchOS) — dostarczony przez firmę Apple nosi nazwę biblioteki. Ponieważ te projekty bibliotek klas i projektów nie pliku wykonywalnego, nie są żadne zarządzanych formy Main, do wykonania.

Wszystkie ta sekwencja uruchamiania jest kompilowany do biblioteki statycznej, która jest następnie łączony w końcowym pliku wykonywalnego, więc aplikacji wie, jak uzyskać poza podstaw.

W tym momencie ma uruchomienia aplikacji, Mono jest uruchomiona, możemy w kodzie zarządzanym i wiemy jak wywołania kodu natywnego i wywołanie zwrotne. Dalej operacją, której konieczne jest rzeczywiście Rozpocznij dodawanie formantów i aplikacji interaktywnego.


## <a name="generator"></a>Generator

Xamarin.iOS zawiera definicje dla każdego pojedynczego systemu iOS interfejsu API. Można przeglądać żadnego z tych adresów na [repozytorium github MaciOS](https://github.com/xamarin/xamarin-macios/tree/master/src). Te definicje zawierać interfejsów z atrybutami, a także niezbędne metody i właściwości. Na przykład następujący kod jest służy do definiowania UIToolbar w UIKit [przestrzeni nazw](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327). Zwróć uwagę, że jest to interfejs z wielu metod i właściwości:

```csharp
[BaseType (typeof (UIView))]
public interface UIToolbar : UIBarPositioning {
    [Export ("initWithFrame:")]
    IntPtr Constructor (CGRect frame);

    [Export ("barStyle")]
    UIBarStyle BarStyle { get; set; }

    [Export ("items", ArgumentSemantic.Copy)][NullAllowed]
    UIBarButtonItem [] Items { get; set; }

    [Export ("translucent", ArgumentSemantic.Assign)]
    bool Translucent { [Bind ("isTranslucent")] get; set; }

    // done manually so we can keep this "in sync" with 'Items' property
    //[Export ("setItems:animated:")][PostGet ("Items")]
    //void SetItems (UIBarButtonItem [] items, bool animated);

    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage ([NullAllowed] UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);

    ...
}
```

Generator o nazwie [ `btouch` ](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) w Xamarin.iOS, wymaga tych plików definicji i używa narzędzia .NET do [kompilowania ich do tymczasowego zestawu](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318). Jednak ten zestaw tymczasowy nie jest niemożliwe, aby wywoływać kod języka Objective-C. Generator następnie odczytuje tymczasowego zestawu i generuje kod C#, który może być używany w czasie wykonywania.
Jest to, dlaczego, na przykład jeśli dodasz losowe atrybut do pliku definicji .cs go nie będą widoczne w outputted kodu. Generator nie może ustalić informacji na ten temat i w związku z tym `btouch` nie może ustalić, aby wyszukać w zestawie tymczasowych, należy przeprowadzić.

Po utworzeniu Xamarin.iOS.dll mtouch będą ze sobą pakietu wszystkie składniki.

Na wysokim poziomie powoduje to osiągnięcie, wykonując następujące zadania:

-   Utwórz strukturę pakietu aplikacji.
-   Kopiowanie z zarządzanych zestawów.
-   Jeśli włączono łączenie Uruchom zarządzanych konsolidator, aby zoptymalizować ponownie zestawy zgrywania nieużywane części wychodzących.
-   Kompilacja drzewa obiektów aplikacji.
-   Utwórz natywny plik wykonywalny, który generuje serii statycznych bibliotek (po jednym dla każdego zestawu) połączonych do natywnego pliku wykonywalnego, dzięki czemu natywnego pliku wykonywalnego, który składa się z kodu uruchamiania, kod rejestratora (jeśli jest statyczny) i wszystkie dane wyjściowe z drzewa obiektów aplikacji kompilatora


Aby uzyskać szczegółowe informacje o konsolidator i sposobie ich użycia, zobacz [konsolidatora](~/ios/deploy-test/linker.md) przewodnik.

## <a name="summary"></a>Podsumowanie

W tym przewodniku przeglądał kompilacji drzewa obiektów aplikacji w aplikacji platformy Xamarin.iOS i eksplorowanych Xamarin.iOS i ich związek z języka Objective-C szczegółowo.

## <a name="related-links"></a>Linki pokrewne

- [Ograniczenia](~/ios/internals/limitations.md)
- [Powiązanie Objective-C](~/cross-platform/macios/binding/overview.md)
- [Objective-C Selectors](~/ios/internals/objective-c-selectors.md)
- [Typ rejestratora](~/ios/internals/registrar.md)
- [Linker](~/ios/deploy-test/linker.md)
