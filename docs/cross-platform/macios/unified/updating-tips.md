---
title: Porady dotyczące aktualizowanie kodu do ujednoliconego interfejsu API
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
author: asb3993
ms.author: amburns
ms.openlocfilehash: 640f95e0083c73288cc8e1f183b06bd28a7b4e07
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>Porady dotyczące aktualizowanie kodu do ujednoliconego interfejsu API

Za pomocą narzędzia automatycznej migracji w programie Visual Studio dla przekonwertuje Mac, iOS i Mac projekty do odwoływania się do Unified API (Xamarin.iOS.dll lub Xamarin.Mac.dll) i wprowadzania wielu zmian w kodzie wymaganych dla Ciebie (zobacz [wersji Xamarin Studio Uwagi sekcja 'Klasycznym narzędzia migracji kodu Unified API'](http://developer.xamarin.com/releases/studio/xamarin.studio_5.7/xamarin.studio_5.7/) szczegółowe informacje).

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>Nie można odnaleźć NSInvalidArgumentException scenorysu błąd

Brak [usterki](https://bugzilla.xamarin.com/show_bug.cgi?id=25569) w bieżącej wersji programu Visual Studio for Mac, może wystąpić po konwersji projektu do interfejsów API Unified przy użyciu narzędzia automatycznej migracji. Po zainstalowaniu aktualizacji, jeśli zostanie wyświetlony komunikat o błędzie w postaci:

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...

```

Można wykonać następujące czynności, aby rozwiązać ten problem, znajdź następujący plik docelowy kompilacji:

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

W tym pliku, należy znaleźć następującego deklaracja obiekt docelowy:

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

I Dodaj `DependsOnTargets="_CollectBundleResources"` do niej atrybut. Jak to:

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Zapisz plik, uruchom ponownie program Visual Studio dla komputerów Mac i wykonaj czystej & odbudować projektu. Poprawkę dotyczącą tego problemu należy wydane przez Xamarin wkrótce.

## <a name="useful-tips"></a>Przydatne porady

Po użyciu narzędzi migracji, może nadal otrzymywać błędy kompilatora wymagają ręcznej interwencji.
Niektóre elementy, które może trzeba ręcznie naprawić obejmują:

* Porównywanie `enum`s może wymagać `(int)` rzutowania.

* `NSDictionary.IntValue` obecnie zwraca `nint`, Brak `Int32Value` której można użyć zamiast niego.

* `nfloat` i `nint` typów nie można oznaczyć `const`;   `static readonly nint` stosowne możliwości.

* Czynności, które kiedyś bezpośrednio w `MonoTouch.` przestrzeni nazw teraz znajdują się zwykle w `ObjCRuntime.` przestrzeni nazw (np.: `MonoTouch.Constants.Version` jest teraz `ObjCRuntime.Constants.Version`).

* Kod, który serializuje obiekty mogą być dzielone w trakcie próby serializować `nint` i `nfloat` typów. Należy sprawdzić kod serializacji działa zgodnie z oczekiwaniami po migracji.

* Niekiedy automatycznego narzędzia chybień kodu wewnątrz `#if #else` dyrektywy kompilatora warunkowego. W takim przypadku należy ręcznie wprowadzić poprawki (zobacz typowe błędy poniżej).

* Metody ręcznie wyeksportowanego za pomocą `[Export]` nie mogą być automatycznie rozwiązany przez narzędzie do migracji, na przykład w tym snippert kodu, należy ręcznie zaktualizować typu zwracanego na `nfloat`:

    ```csharp
    [Export("tableView:heightForRowAtIndexPath:")]
    public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
    ```

 * Interfejsu API Unified nie zapewnia niejawna konwersja między NSDate i .NET DateTime, ponieważ nie jest bezstratny konwersji. Aby uniknąć błędów związanych z `DateTimeKind.Unspecified` przekonwertować .NET `DateTime` do lokalnego lub UTC przed rzutowania `NSDate`.

 * Metody kategorii Objective-C teraz są generowane jako metody rozszerzenia interfejsu API Unified. Na przykład kodu, który korzystał wcześniej z `UIView.DrawString` może odwoływać się `NSString.DrawString` interfejsu API Unified.

 * Kodowanie przy użyciu klas AVFoundation z `VideoSettings` należy zmienić do użycia `WeakVideoSettings` właściwości. Wymaga to `Dictionary`, która jest dostępna jako właściwość klasy ustawień, na przykład:

    ```csharp
    vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
    ```

 * NSObject `.ctor(IntPtr)` konstruktor został zmieniony z publicznej do chronionego ([przed użyciem niewłaściwy](~/cross-platform/macios/unified/index.md#NSObject_ctor)).

 * `NSAction` został [zastąpione](~/cross-platform/macios/unified/index.md#NSAction) z starndard .NET `Action`. Niektóre obiekty delegowane proste (pojedynczy parametr) również zostały zastąpione `Action<T>`.

Na koniec można znaleźć [różnice Classic v Unified API](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) do odszukania zmiany do interfejsów API w kodzie. Wyszukiwanie [tej strony](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) pomoże znaleźć klasycznego interfejsów API i co one już została zaktualizowana w celu.

**Uwaga:** `MonoTouch.Dialog` przestrzeni nazw jest taka sama po migracji. Jeśli używa Twój kod **MonoTouch.Dialog** można nadal używać tej przestrzeni nazw — czy *nie* zmienić `MonoTouch.Dialog` do `Dialog`!

## <a name="common-compiler-errors"></a>Typowe błędy kompilatora

Inne przykłady typowych błędów są wymienione poniżej, wraz z rozwiązania:

**Błąd CS0012: Typ "MonoTouch.UIKit.UIView" jest zdefiniowany w zestawie, do którego nie odwołuje się.**

Poprawka: Zwykle oznacza, że projekt odwołuje się do składnika lub pakietu NuGet, który nie został utworzony przy użyciu interfejsu API Unified. Należy usunąć i ponownie dodać wszystkich składników i NuGet pakietów. Jeśli to nie rozwiąże błędu, biblioteki zewnętrznej może nie obsługuje jeszcze interfejsu API Unified.

**Błąd MT0034: Nie może zawierać zarówno "monotouch.dll" i "Xamarin.iOS.dll" w tym samym projekcie platformy Xamarin.iOS — "Xamarin.iOS.dll" odwołuje się do jawnie, gdy "monotouch.dll" odwołuje się do niego "Xamarin.Mobile, wersja = 0.6.3.0, Culture = neutral, PublicKeyToken = null ".**

Poprawka: Usuń składnik, który jest przyczyną tego błędu i ponownie dodać do projektu.

**Błąd CS0234: Nazwa typu lub przestrzeni nazw "Foundation" nie istnieje w przestrzeni nazw "MonoTouch". Czy nie brakuje odwołania do zestawu?**

Poprawka: Automatyczne narzędzie do migracji w programie Visual Studio for Mac *powinien* Aktualizuj wszystkie `MonoTouch.Foundation` odwołuje się do `Foundation`, jednak w niektórych przypadkach muszą one być aktualizowane ręcznie. Podobne błędy mogą występować dla innych przestrzeniach nazw wcześniej zawarte w `MonoTouch`, takich jak `UIKit`.

**Błąd CS0266: Nie można niejawnie przekonwertować typu "double" do "System.float"**

Poprawka: Zmień typ i rzutować `nfloat`. Ten błąd może również wystąpić w przypadku innych typów z 64-bitowe odpowiedniki (takich jak `nint`,)

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**Błąd CS0266: Nie można niejawnie przekonwertować typu "CoreGraphics.CGRect" do "System.Drawing.RectangleF". Istnieje konwersja jawna (czy nie brakuje rzutu?)**

Poprawka: Zmień wystąpień `RectangleF` do `CGRect`, `SizeF` do `CGSize`, i `PointF` do `CGPoint`. Przestrzeń nazw `using System.Drawing;` powinna zostać zastąpiona `using CoreGraphics;` (jeśli jest już obecny).

**Błąd CS1502: najlepiej dopasowana metoda przeciążona metody "CoreGraphics.CGContext.SetLineDash (System.nfloat, System.nfloat[])" ma nieprawidłowe argumenty.**

Poprawka: Zmień typ tablicy do `nfloat[]` i jawne rzutowanie `Math.PI`.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**Błąd CS0115: "WordsTableSource.RowsInSection (UIKit.UITableView, int)" jest oznaczony jako przesłonięcie, ale nie znaleziono do przesłonięcia odpowiedniej metody**

Poprawka: Zwracane typy wartości i parametr, aby zmienić `nint`. Występuje to powszechnie zamienników metod, takich jak te na `UITableViewSource`, takie jak `RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, `GetViewForHeader`itp.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**Błąd CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)': return type must be 'System.nint' to match overridden member `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView) "**

Poprawka: Typ zwracany jest zmiany na `nint`, rzutowania wartości zwracanej do `nint`.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**Błąd CS1061: Typ "CoreGraphics.CGPath" nie zawiera definicji dla "AddElipseInRect"**

Poprawka: Sprawdzanie pisowni do `AddEllipseInRect`. Inne zmiany nazwy obejmują:

* Zmień "Color.Black" `NSColor.Black`.
* Zmień MapKit "AddAnnotation" `AddAnnotations`.
* Zmień AVFoundation "DataUsingEncoding" `Encode`.
* Zmień AVFoundation "AVMetadataObject.TypeQRCode" `AVMetadataObjectType.QRCode`.
* Zmień AVFoundation "VideoSettings" `WeakVideoSettings`.
* Zmień PopViewControllerAnimated do `PopViewController`.
* Zmień CoreGraphics "CGBitmapContext.SetRGBFillColor" `SetFillColor`.

**Błąd CS0546: nie można przesłonić, ponieważ "MapKit.MKAnnotation.Coordinate" nie ma set z możliwością przesłonięcia (CS0546)**

Podczas tworzenia niestandardowego adnotacji przez tworzenie podklas MKAnnotation pola współrzędnych nie zdefiniowano metody ustawiającej, tylko metody pobierającej.

[Usuń](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

* Dodawanie pola do śledzenia Współrzędna
* Zwróć to pole metody pobierającej właściwości współrzędnych
* Zastąp metodę SetCoordinate i ustaw odpowiednie pole
* Wywołaj metodę SetCoordinate w Twojej ctor z parametrem współrzędnych przekazano

Powinien wyglądać podobnie do poniższej:

```csharp
class BasicPinAnnotation : MKAnnotation
{
    private CLLocationCoordinate2D _coordinate;

    public override CLLocationCoordinate2D Coordinate
    {
        get
        {
            return _coordinate;
        }
    }

    public override void SetCoordinate(CLLocationCoordinate2D value)
    {
        _coordinate = value;
    }

    public BasicPinAnnotation (CLLocationCoordinate2D coordinate)
    {
        SetCoordinate(coordinate);
    }
}
```

## <a name="related-links"></a>Linki pokrewne

- [Aktualizowanie aplikacji](~/cross-platform/macios/unified/updating-apps.md)
- [Aktualizowanie aplikacji dla systemu iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Aktualizowanie aplikacji dla komputerów Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualizowanie aplikacji platformy Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualizowanie powiązania](~/cross-platform/macios/unified/update-binding.md)
- [Praca z typami natywnymi w aplikacjach międzyplatformowych](~/cross-platform/macios/native-types-cross-platform.md)
- [Klasycznym vs różnice Unified API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
