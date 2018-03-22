---
title: "Przegląd ujednoliconego interfejsu API"
description: "Nowy styl interfejsu API ułatwia niż kiedykolwiek współużytkowanie kodu Mac i z systemem iOS, jak również umożliwiając obsługę 32- i 64 bitowych aplikacji o tej samej binarnego."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 03364b3fd91ffdb4debb3d58f8040d8ade9100b8
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="unified-api-overview"></a>Przegląd ujednoliconego interfejsu API

_Nowy styl interfejsu API ułatwia niż kiedykolwiek współużytkowanie kodu Mac i z systemem iOS, jak również umożliwiając obsługę 32- i 64 bitowych aplikacji o tej samej binarnego._

Do poprawy kodu udostępnianie między Mac i z systemem iOS i umożliwia deweloperom mają jeden kod podstawowej, które działa na 32- i 64 bitów, w wczesne 2015 wprowadzono nowy interfejs API w produktach zarówno Xamarin.Mac, jak i platformy Xamarin.iOS wywołać interfejsu API Unified.

> [!IMPORTANT]
> **Amortyzacja klasycznego profilu:** miarę dodawania nowych platform w Xamarin.iOS zostanie użyty stopniowo zastąpić funkcji z klasycznym profilu (monotouch.dll). Na przykład opcja z systemem innym niż NRC (liczba nowych ref) została usunięta. NRC zawsze została włączona dla wszystkich ujednoliconego aplikacji (tj. z systemem innym niż NRC nigdy nie był opcję) i ma nie znanych problemów. Przyszłych wydaniach usunie opcję użycia Boehm jako moduł garbage collector. Było również opcję Nigdy nie są dostępne do ujednoliconego aplikacji. Dalej znajduje się w wersji Xamarin.iOS 10.0 zaplanowano całkowite usunięcie klasycznego pomocy technicznej.

## <a name="ios"></a>iOS

`Xamarin.iOS.dll` Jest dołączany Xamarin.iOS 8.6 zestawu naszych **pierwszej wersji stabilny i obsługiwanych** ujednoliconego interfejsu API dla systemu iOS.
Poprzednie wersje Podgląd ujednolicony interfejs API są zamknięcia, ale nie w pełni zgodne.

## <a name="mac"></a>Mac

`Xamarin.Mac.dll` Zestawu w stabilna kanału Xamarin.Mac naszych **pierwszej wersji stabilny i obsługiwanych** interfejsu API Unified dla komputerów Mac.
Poprzednie wersje Podgląd ujednolicony interfejs API są zamknięcia, ale nie w pełni zgodne.

## <a name="runtime-defaults"></a>Domyślne ustawienia środowiska uruchomieniowego

Interfejsu API Unified domyślnie używa **SGen** modułu zbierającego elementy bezużyteczne i [liczenie odwołań w nowych](~/ios/internals/newrefcount.md) systemu śledzenia własności obiektu. Ta sama funkcja ma systemie Xamarin.Mac.

To rozwiązuje wiele problemów, które deweloperzy muszą ponosić w starym systemie i również ułatwić [zarządzania pamięcią](~/cross-platform/deploy-test/memory-perf-best-practices.md).

Zauważ, że można włączyć nowe licznikiem nawet w przypadku klasycznego interfejsu API, ale wartość domyślna jest zachowawcze i nie wymaga użytkowników wprowadzić zmiany. Przy użyciu interfejsu Unified API możemy miała możliwość zmianę domyślnych i dają deweloperom wszystkie ulepszenia w tym samym czasie Refaktoryzuj i ponownie testowania ich kodu.

<a name="namespace-changes" />

## <a name="library-split"></a>Biblioteka podziału

Od tego momentu naszych interfejsów API, zostaną wyświetlone na dwa sposoby:

-  **Klasycznego interfejsu API:** maksymalnie 32-bitowej (tylko) i uwidocznione w `monotouch.dll` i `XamMac.dll` zestawów.
-  **Ujednolicony interfejs API:** obsługuje zarówno 32- i 64 programowanie bitowego z jednego interfejsu API dostępne w `Xamarin.iOS.dll` i `Xamarin.Mac.dll` zestawów.

Oznacza to, że dla przedsiębiorstwa Deweloperzy (nie wskazuje sklepu App Store), możesz kontynuować korzystanie z istniejących interfejsów API klasycznych, jak firma Microsoft będzie przechowywać utrzymania je w nieskończoność, lub przeprowadzić uaktualnienie do nowych interfejsów API.

### <a name="namespace-changes"></a>Namespace zmiany

Aby zmniejszyć tarcia udostępnianie kodu przez naszych produktów Mac i iOS, zmieniamy przestrzenie nazw dla interfejsów API w produktach.

Firma Microsoft usuwanych prefiks "MonoTouch" z naszych produktów z systemem iOS i "MonoMac" z produktu Mac typów danych.

Uproszczono współużytkowanie kodu Mac i iOS platform bez konieczności kompilacja warunkowa i zmniejsza szumu w górnej części plików kodu źródłowego.

-  **Klasycznego interfejsu API:** Użyj przestrzeni nazw `MonoTouch.` lub `MonoMac.` prefiks.
-  **Ujednolicony interfejs API:** bez prefiksu przestrzeni nazw

### <a name="api-changes"></a>API Changes

Interfejsu API Unified usuwa przestarzałe metody i kilka wystąpień w przypadku, gdy błędów pisowni w nazwach interfejsu API podczas były one były powiązane z oryginalnym MonoTouch i MonoMac przestrzeni nazw w klasycznym interfejsów API. Wystąpienia te zostały poprawione w nowych interfejsów API Unified i będzie muszą zostać zaktualizowane składnika, iOS i Mac aplikacji. Poniżej przedstawiono listę typowych te, które możesz napotkać:

|Nazwa metody interfejsu API klasycznego|Nazwa metody interfejsu API Unified|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

Aby zapoznać się z pełną listą zmiany podczas przełączania z klasycznego interfejsu API Unified, zobacz nasze [vs klasyczny (monotouch.dll) interfejsu API Unified (Xamarin.iOS.dll) różnice](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) dokumentacji.

### <a name="updating-to-unified"></a>Aktualizowanie do ujednoliconego

Kilka stary/uszkodzony/przestarzałe interfejs API w **klasycznego** nie są dostępne w **Unified** interfejsu API. Można łatwiej rozwiązać `CS0616` uaktualnienia ostrzeżenia przed uruchomieniem programu (ręczne lub automatyczne), ponieważ będziesz mieć `[Obsolete]` atrybutu komunikatu (część ostrzeżenia), prowadzące do prawej interfejsu API.

Należy pamiętać, że firma Microsoft publikują [ *różnicowego* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) z klasycznym vs unified zmiany interfejsu API, które mogą być używane przed lub po aktualizacji projektu. Nadal ustalania zastępuje nieaktualny już dokument wywołań w klasycznym będzie często można oszczędność czasu (mniej wyszukiwań dokumentacji).

Wykonaj te instrukcje, aby [zaktualizować istniejące aplikacje dla systemu iOS](~/cross-platform/macios/unified/updating-ios-apps.md), lub [aplikacji Mac](~/cross-platform/macios/unified/updating-mac-apps.md) do interfejsu API Unified.
Przejrzyj pozostałą część tej strony i [te wskazówki](~/cross-platform/macios/unified/updating-tips.md) Aby uzyskać dodatkowe informacje na temat migracji kodu.

## <a name="components-and-nuget"></a>Składniki i NuGet

Większość istniejących składników i pakiety NuGet zostaną muszą zostać zaktualizowane do obsługi interfejsu API Unified.
Nie można przywołać składniki utworzono przy użyciu klasycznego interfejsu API z projektu Unified API.

Istniejące składniki, które nie ma żadnych odwołań do `monotouch.dll` (lub `XamMac.dll`) nie powinna wymagać aktualizacji.

### <a name="components"></a>Składniki

składniki systemu iOS w [Magazyn składników Xamarin](https://components.xamarin.com/) należy zaktualizować, aby pracować z projektami Unified API. Xamarin działa to aktualizację składników możemy publikowania i zachęca innym osobom na tym samym.

### <a name="nuget"></a>NuGet

Pakiety NuGet, które wcześniej obsługiwane platformy Xamarin.iOS przy użyciu klasycznego interfejsu API opublikowane ich zestawów przy użyciu **Monotouch10** moniker platformy.

Interfejsu API Unified wprowadzono nowy identyfikator platformy zgodne pakietów - **Xamarin.iOS10**. Istniejące pakiety NuGet zostaną muszą zostać zaktualizowane dodanie obsługi dla tej platformy, od kompilowania aplikacji interfejsu API Unified.

> [!IMPORTANT]
> Jeśli masz wystąpił błąd w formie _"błąd 3 nie może zawierać zarówno"monotouch.dll"i"Xamarin.iOS.dll"w tym samym projekcie platformy Xamarin.iOS —"Xamarin.iOS.dll"odwołuje się do jawnie, gdy"monotouch.dll"odwołuje się do niego" xxx, wersja = 0.0.000, Culture = neutral, PublicKeyToken = null ""_ po przekonwertowaniu aplikacji do interfejsów API Unified, jest zazwyczaj z powodu konieczności składnika lub pakietu NuGet w projekcie, który nie został jeszcze zaktualizowany do interfejsu API Unified. Należy usunąć istniejący składnik/NuGet, aktualizacja do wersji, która obsługuje interfejsy API Unified i wykonać czystą kompilację.

<a name="deprecated-apis" />

## <a name="arrays-and-systemcollectionsgeneric"></a>System.Collections.Generic — i tablic

Ponieważ C# indeksatory oczekuje typu `int`, musisz jawnie rzutowania `nint` wartości do `int` do uzyskania dostępu do elementów w kolekcji lub tablicy. Na przykład:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Jest to zachowanie oczekiwane, ponieważ rzutowania z `int` do `nint` jest stratna na 64-bitowym, niejawna konwersja nie została wykonana.

## <a name="converting-datetime-to-nsdate"></a>Konwertowanie daty/godziny na NSDate

Korzystając z Unified API niejawnej konwersji wartości `DateTime` do `NSDate` wartości jest już wykonywane. Te wartości, należy jawnie można przekonwertować typu na inny. Następujące metody rozszerzenia może służyć do automatyzowania ten proces:

```csharp
public static DateTime NSDateToDateTime(this NSDate date)
{
    // NSDate has a wider range than DateTime, so clip
    // the converted date to DateTime.Min|MaxValue.
    double secs = date.SecondsSinceReferenceDate;
    if (secs < -63113904000)
        return DateTime.MinValue;
    if (secs > 252423993599)
        return DateTime.MaxValue;
    return (DateTime) date;
}

public static NSDate DateTimeToNSDate(this DateTime date)
{
    if (date.Kind == DateTimeKind.Unspecified)
        date = DateTime.SpecifyKind (date, /* DateTimeKind.Local or DateTimeKind.Utc, this depends on each app */)
    return (NSDate) date;
}

```

## <a name="deprecated-apis-and-typos"></a>Przestarzałe interfejsy API i błędów

Wewnątrz Xamarin.iOS klasycznego interfejsu API (monotouch.dll) `[Obsolete]` atrybut był używany na dwa sposoby:

-  **Przestarzałe iOS interfejsu API:** jest to w przypadku Apple wskazówek w celu zatrzymania przy użyciu interfejsu API, ponieważ został on zastąpiony przez nowszej. Klasycznego interfejsu API jest nadal cienkie i często wymagane (jeśli są używane starszą wersją systemu iOS).
 Takie interfejsu API (i `[Obsolete]` atrybut) są umieszczone w nowej zestawów platformy Xamarin.iOS.
-  **Niepoprawny interfejs API** niektóre interfejsu API, które miał literówki na ich nazwy.

Dla oryginalnego zestawów (monotouch.dll i XamMac.dll) firma Microsoft zachowana poprzedni kod dostępne zgodność, ale zostały usunięte z zestawów Unified API (Xamarin.iOS.dll i Xamarin.Mac)

<a name="NSObject_ctor" />

## <a name="nsobject-subclasses-ctorintptr"></a>.Ctor(IntPtr) podklasy NSObject

Każdy `NSObject` podklasy ma konstruktora akceptującego `IntPtr`. Jest to, jak firma Microsoft wystąpienia nowe wystąpienie zarządzanego z uchwyt macierzysty ObjC.

W klasycznym to `public` konstruktora. Jednak było łatwe niewłaściwym użyciem tej funkcji w kodzie użytkownika, np. utworzenie kilku zarządzane wystąpienia dla pojedynczego wystąpienia ObjC *lub* tworzenie zarządzanych wystąpienia, który będzie braku oczekiwanym stanem zarządzany (dla podklasy).

Aby uniknąć problemów tymi rodzaj `IntPtr` konstruktory są teraz `protected` w **ujednoliconego** interfejsu API, można użyć tylko w przypadku podklasy. Daje to pewność, że Popraw/safe interfejsu API jest używany do tworzenia zarządzanego wystąpienia z dojść, tj.

    var label = Runtime.GetNSObject<UILabel> (handle);

Ten interfejs API zwróci istniejącego wystąpienia zarządzanego (Jeśli już istnieje) lub utworzy nową (jeśli jest to wymagane). Jest już dostępna zarówno klasycznego i ujednoliconego interfejsu API.

Należy pamiętać, że `.ctor(NSObjectFlag)` jest teraz również `protected` , ale ten zestaw został rzadko używane poza podklasy.

<a name="NSAction" />

## <a name="nsaction-replaced-with-action"></a>NSAction zastąpione akcji

Z API Unified `NSAction` zostały usunięte na rzecz standard .NET `Action`. Jest to duży poprawy jakości, ponieważ `Action` jest wspólny typ .NET, podczas gdy `NSAction` został specyficzne dla platformy Xamarin.iOS. Robią dokładnie tak samo, ale były unikatowe i niezgodne typy i spowodowało więcej kodu o są zapisywane w ten sam efekt.

Na przykład, jeśli istniejąca aplikacja Xamarin zawiera następujący kod:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```
Go teraz można zastąpić proste lambda:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

Wcześniej wyniesie wystąpi błąd kompilatora ponieważ `Action` nie można przypisać do `NSAction`, ale ponieważ `UITapGestureRecognizer` teraz przyjmuje `Action` zamiast `NSAction` jest nieprawidłowy w Unified API.

## <a name="custom-delegates-replaced-with-actiont"></a>Niestandardowych delegatów zastąpione akcji<T>

W **ujednoliconego** prostych (np. jeden parametr) zostały zastąpione delegatów .net `Action<T>`. Np.

    public delegate void NSNotificationHandler (NSNotification notification);

można teraz używać jako `Action<NSNotification>`. Ten kod Podnieś poziom ponownego wykorzystania i zmniejszyć zduplikowania kodu wewnątrz zarówno Xamarin.iOS własnych aplikacji.

## <a name="taskbool-replaced-with-taskbooleannserror"></a>Zadanie<bool> zastąpione zadań < wartość logiczna, NSError >>

W **klasycznego** wystąpiły pewne async interfejsów API zwracanie `Task<bool>`. Jednak niektóre z nich gdzie ma być używany, gdy `NSError` było częścią podpisu, tj. `bool` został już `true` musiały catch wyjątku, aby uzyskać `NSError`.

Ponieważ błędy są często i wartości zwracanej nie jest przydatne w tym wzorcu został zmieniony w **ujednoliconego** do zwrócenia `Task<Tuple<Boolean,NSError>>`. Dzięki temu można sprawdzić sukcesów i wszelkie błąd, który może się to zdarzyć podczas wywołania asynchronicznego.

## <a name="nsstring-vs-string"></a>Ciąg wersji programu vs NSString

W niektórych przypadkach niektóre stałe musiała zostać zmieniony z `string` do `NSString`, np. `UITableViewCell`

**Classic**

    public virtual string ReuseIdentifier { get; }

**Ujednolicone**

    public virtual NSString ReuseIdentifier { get; }

Generalnie firma Microsoft preferowane .NET `System.String` typu. Jednak mimo z wytycznymi firmy Apple, niektóre natywnego interfejsu API są porównywania wskaźniki stałe (nie ciąg, sam), to prawidłowe tylko gdy uwidaczniamy stałe jako `NSString`.

 <a name="protocols" />

## <a name="objective-c-protocols"></a>Protokoły języka Objective C

Oryginalnego MonoTouch nie miał pełną obsługę dla protokołów ObjC i niektóre,-optymalne, interfejsu API zostały dodane do obsługi najbardziej typowym scenariuszem. To ograniczenie nie istnieje, ale dla zgodności z poprzednimi wersjami kilka interfejsów API są przechowywane wokół wewnątrz `monotouch.dll` i `XamMac.dll`.

Te ograniczenia zostały usunięte i wyczyścić na Unified API. Większość zmian będzie wyglądać następująco:

**Classic**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**Ujednolicone**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

`I` Prefiksu oznacza **ujednoliconego** uwidacznia interfejsu zamiast określonego typu dla protokołu ObjC. Ułatwi to przypadków, w których nie chcesz podklasy określonego typu, który Xamarin.iOS dostarczony.

Może on również niektóre API być bardziej dokładne i łatwy w użyciu, np.:

**Classic**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**Ujednolicone**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

Takie interfejsu API są teraz łatwiejsze do nas, bez odwołuje się z dokumentacją i uzupełniania kodu użytkownika IDE zapewnia bardziej użyteczne sugestie oparte na protokole/interfejsu.

### <a name="nscoding-protocol"></a>NSCoding Protocol

Nasze powiązania pierwotnej uwzględnione .ctor(NSCoder) dla każdego typu — nawet wtedy, gdy nie obsługuje `NSCoding` protokołu.  Pojedynczy `Encode(NSCoder)` metody był obecny w `NSObject` do kodowania obiektu.
Jednak ta metoda będzie działać tylko, jeśli wystąpienie były zgodne z protokołem NSCoding.

Interfejs API Unified Naprawiono to.  Nowe zestawy będą mieć tylko `.ctor(NSCoder)` Jeśli typ odpowiada `NSCoding`. Takie typy teraz istnieć `Encode(NSCoder)` metodę, która odpowiada `INSCoding` interfejsu.

Niski wpływ: W większości przypadków ta zmiana nie mają wpływu na aplikacje jako nie można używać konstruktorów stare, zostały usunięte.

## <a name="further-tips"></a>Dalsze wskazówki

Należy pamiętać o dodatkowych zmian są wymienione w [porady dotyczące aktualizowania aplikacji interfejsu API Unified](~/cross-platform/macios/unified/updating-tips.md).

## <a name="sample-code"></a>Przykładowy kod

Począwszy od 31 lipca możemy opublikowano porty próbek iOS ten nowy interfejs API na `magic-types` gałęzi w [monotouch przykłady](https://github.com/xamarin/monotouch-samples/commits/magic-types).

Dla komputerów Mac, sprawdzamy próbek w obu [przykłady mac](https://github.com/xamarin/mac-samples) repozytorium (przedstawiający nowych interfejsów API w Mavericks/Yosemite), a także przykłady 32/x 64 w gałęzi typy magic [przykłady mac](https://github.com/xamarin/monotouch-samples/commits/magic-types).

## <a name="related-links"></a>Linki pokrewne

- [Aktualizowanie aplikacji dla systemu iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Aktualizowanie aplikacji dla komputerów Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualizowanie aplikacji platformy Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualizowanie powiązania](~/cross-platform/macios/unified/update-binding.md)
- [Praca z typami natywnymi w aplikacjach międzyplatformowych](~/cross-platform/macios/native-types-cross-platform.md)
- [Aktualizowanie porady](~/cross-platform/macios/unified/updating-tips.md)
- [Klasycznym vs różnice Unified API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
