---
title: Praca z typy natywne w wieloplatformowych aplikacji
description: W tym artykule omówiono korzystanie nowe typy natywnego interfejsu API Unified (nint, nuint, nfloat) z systemem iOS w aplikacji i platform, gdy kod jest współużytkowany z urządzeń z systemem innym niż z systemem iOS, takich jak Android i Windows Phone w systemach operacyjnych.
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: eb6979691eeef6dd7436d74fdfd501c747d9b3c6
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>Praca z typy natywne w wieloplatformowych aplikacji

_W tym artykule omówiono korzystanie nowe typy natywnego interfejsu API Unified (nint, nuint, nfloat) z systemem iOS w aplikacji i platform, gdy kod jest współużytkowany z urządzeń z systemem innym niż z systemem iOS, takich jak Android i Windows Phone w systemach operacyjnych._


Typy natywne typy 64 współpracować z systemów iOS i Mac interfejsów API. Jeśli piszesz udostępnionego kodu, który jest uruchamiany w systemie Android lub Windows również należy zarządzać konwersji typów ujednolicony w regularnych typów .NET, które można udostępniać.

W tym dokumencie omówiono sposoby na potrzeby współdziałania z interfejsu API Unified w kodzie udostępnionego wspólnego.

## <a name="when-to-use-the-native-types"></a>Kiedy należy używać w typach natywnych

Xamarin.Mac Unified API i Xamarin.iOS nadal zawierają `int`, `uint` i `float` typy danych, jak również `RectangleF`, `SizeF` i `PointF` typów. Te istniejących typów danych powinno być kontynuowane do użycia w dowolny kod udostępnionego, między platformami. Nowe typy danych natywnych należy używać tylko w przypadku, gdy wywołania API Mac lub iOS, gdzie pomocy technicznej dla typów obsługujących architektury są wymagane.

W zależności od charakteru kodu są udostępniane, niekiedy może być gdzie kod obsługujący wiele platform może być konieczne postępowania w przypadku `nint`, `nuint` i `nfloat` typy danych. Na przykład: biblioteki, która obsługuje przekształcenia na prostokątne danych, który korzystał wcześniej z `System.Drawing.RectangleF` udostępniania funkcji między wersjami aplikacji platformy Xamarin.iOS i Xamarin.Android musi zostać zaktualizowany do obsługi natywnych typów w systemie iOS.

Sposób obsługi tych zmian jest zależna od rozmiaru i złożoności aplikacji i formie kodu udostępniania, który został użyty, jak przedstawiono w sekcjach poniżej.

## <a name="code-sharing-considerations"></a>Zagadnienia dotyczące udostępnianie kodu

Jak już wspomniano w [opcje udostępniania kodu](~/cross-platform/app-fundamentals/code-sharing.md) dokumentu, istnieją dwa sposoby udostępniania kodu między projektami i platform: udostępnionych projektów i przenośnej biblioteki klas. Z dwóch typów została już użyta, ograniczy opcje, który mamy podczas obsługi natywnych typów w kodzie i platform.

### <a name="portable-class-library-projects"></a>Projekty bibliotek klas przenośnych

Przenośnej biblioteki klas (PCL) umożliwia wybieranie platform, które chcesz obsługiwać i używać interfejsów umożliwiają korzystanie z funkcji specyficznych dla platformy.

Ponieważ typ projektu PCL ma być kompilowana w dół do `.DLL` i nie ma on nie ma sensu interfejsu API Unified, będzie można wymusić nadal używać istniejących typów danych (`int`, `uint`, `float`) w PCL kodu źródłowego i typu rzutowania wywołań PCLs klasy i metody w aplikacji frontonu. Na przykład:

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>Udostępnionych projektów

Typ projektu udostępnionego zasobów umożliwia organizowanie kodu źródłowego w oddzielnych projektu, który następnie pobiera uwzględnione i skompilowany w aplikacji frontonu poszczególnych specyficzne dla platformy, a następnie użyj `#if` dyrektywy kompilatora jako wymagane do zarządzania wymagania dotyczące platformy.

Rozmiar i złożoność przodu kończyć aplikacji dla urządzeń przenośnych, które zużywają udostępnionego kodu oraz rozmiar i złożoność kodu są udostępniane, należy wziąć pod uwagę, gdy Wybieranie metody obsługi danych natywnych typów i platform z Typ projektu zasobów udostępnionych.

Na podstawie tych czynników, następujące rozwiązania można zaimplementować przy użyciu `if __UNIFIED__ ... #endif` dyrektywy kompilatora Unified API zmian w kodzie.

#### <a name="using-duplicate-methods"></a>Przy użyciu metod w zduplikowanych

Zająć przykład biblioteki, która wykonuje przekształcenia prostokątne dane podane powyżej. Biblioteka zawiera tylko jedną lub dwie metody bardzo proste, można utworzyć zduplikowane wersji tych metod dla platformy Xamarin.iOS i platformy Xamarin.Android. Na przykład:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #else
            public static float CalculateArea(RectangleF rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #endif
        #endregion
    }
}
```

W powyższym kodzie ponieważ `CalculateArea` procedura jest bardzo prosty, firma Microsoft ma używać kompilacja warunkowa i utworzyć oddzielne, wersja Unified API metody. Z drugiej strony Jeśli wiele procedur lub kilka złożonych procedury biblioteki, to rozwiązanie będzie nie być to możliwe, będzie stanowić problemu synchronizacja wszystkie metody do modyfikacji lub poprawki.

#### <a name="using-method-overloads"></a>Overloads-przy użyciu metody

W takim przypadku rozwiązanie może być do utworzenia wersji przeciążenia metody przy użyciu 32-bitowe typy danych tak, aby teraz wykonać `CGRect` jako parametr i/lub wartości zwracanej, przekonwertować tę wartość do `RectangleF` (wiedzy o konwersji z `nfloat` do `float` konwersja stratnej) i wywołań procedury do wykonują rzeczywistą pracę z oryginalną wersją. Na przykład:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

            // Call original routine to calculate area
            return (nfloat)CalculateArea((RectangleF)rect);

            }
        #endif

        public static float CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }

        #endregion
    }
}

```

Ponownie jest to dobre rozwiązanie, tak długo, jak utratę dokładności nie ma wpływu na wyniki do wymagań aplikacji.

#### <a name="using-alias-directives"></a>Alias dyrektyw Using

Obszarów, gdzie utratę dokładności to problemu, inne rozwiązanie możliwe jest użycie `using` dyrektywy w celu utworzenia aliasu dla danych typu macierzystego i CoreGraphics przy tym następujący kod do góry plików kodu źródłowego udostępnionego i konwertowanie żadnego wymagane `int`, `uint` lub `float` wartości do `nint`, `nuint` lub `nfloat`:

```csharp
#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif
```

Tak, aby następnie staje się naszego przykładowego kodu:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif

namespace NativeShared
{

    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        public static nfloat CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }
        #endregion
    }
}
```

Należy pamiętać, że w tym miejscu zmieniono `CalculateArea` metody zwracany `nfloat` zamiast standardowego `float`. Wykonano tak, aby firma Microsoft nie otrzyma błąd kompilacji w trakcie _niejawnie_ przekonwertować `nfloat` wynik naszych obliczeń (ponieważ są obie wartości mnożona `nfloat`) do `float` zwracają wartość.

Jeśli kod jest skompilowany i uruchom na urządzeniu z systemem innym niż interfejsu API Unified `using nfloat = global::System.Single;` mapy `nfloat` do `Single` która niejawnie przekonwertuje `float` co aplikacja odbierająca komunikaty frontonu do wywołania `CalculateArea` metody bez Zmiana.


#### <a name="using-type-conversions-in-the-front-end-app"></a>W aplikacji frontonu przy użyciu konwersje typów

W przypadku, gdy aplikacji frontonu wprowadzić tylko kilka wywołania do biblioteki udostępnionej kodu, inne rozwiązanie można pozostawić bez zmian biblioteki i wpisz rzutowanie w aplikacji platformy Xamarin.iOS lub Xamarin.Mac podczas wywoływania procedury istniejących. Na przykład:

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

Jeśli aplikacja odbierająca komunikaty dokonuje setki wywołania do biblioteki udostępnionej kodu, to ponownie, może nie być dobrym rozwiązaniem.

Oparty na architekturze naszej aplikacji, możemy przy użyciu co najmniej jednego z powyższych rozwiązań do obsługi danych natywnych typów (w razie potrzeby) w naszym kodzie między platformami.


## <a name="xamarinforms-applications"></a>Aplikacje platformy Xamarin.Forms

Poniżej jest wymagany do użycia platformy Xamarin.Forms dla UI między platformami, które również zostaną udostępnione z aplikacją Unified API:

- Całe rozwiązanie musi być używana wersja 1.3.1 (lub nowszego) platformy Xamarin.Forms pakietu NuGet.
- Dla dowolnego renderuje niestandardowych Xamarin.iOS użyć takich samych typach przedstawionych powyżej rozwiązania oparte na jak kodu interfejsu użytkownika został udostępniony (udostępnione projektu lub PCL).

Jak w przypadku standardowej aplikacji wieloplatformowych istniejące 32-bitowe typy danych używanego w dowolny kod i platform, współdzielonych w większości sytuacji wszystkie. Nowe typy danych natywnych należy używać tylko w przypadku, gdy wywołania API Mac lub iOS, gdzie pomocy technicznej dla typów obsługujących architektury są wymagane.

Aby uzyskać więcej informacji, zobacz nasze [aktualizowania istniejących aplikacji platformy Xamarin.Forms](http://developer.xamarin.com/guides/cross-platform/macios/updating-xamarin-forms-apps/) dokumentacji.

## <a name="summary"></a>Podsumowanie

W tym artykule mamy Zobacz podczas powinniśmy skorzystać w natywnych typów danych w aplikacji Unified API i ich wpływ na różnych platformach. Firma Microsoft prezentować kilka rozwiązań, których można użyć w sytuacjach, gdy nowe typy danych natywnych należy używać w bibliotek i platform. I firma Microsoft w tym samouczku przewodnika, aby obsługa Unified API w aplikacji platformy Xamarin.Forms dla wielu platform.



## <a name="related-links"></a>Linki pokrewne

- [Ujednolicony interfejs API](~/cross-platform/macios/unified/index.md)
- [Typy natywne](~/cross-platform/macios/nativetypes.md)
- [Opcje udostępniania kodu](~/cross-platform/app-fundamentals/code-sharing.md)
- [Udostępnianie kodu — przykład](https://developer.xamarin.com/samples/mobile/SharingCode/)
