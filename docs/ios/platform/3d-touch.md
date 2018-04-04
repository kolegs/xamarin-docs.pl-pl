---
title: Wprowadzenie do technologii 3D Touch
description: W tym artykule omówiono przy użyciu nowego telefonów iPhone 6s i telefonów iPhone 6s Plus 3D Touch gesty, przy użyciu Twojej aplikacji.
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a0d89315b82f4931538cdabe64aade7986b2a42e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-3d-touch"></a>Wprowadzenie do technologii 3D Touch

_W tym artykule omówiono przy użyciu nowego telefonów iPhone 6s i telefonów iPhone 6s Plus 3D Touch gesty, przy użyciu Twojej aplikacji._

[![](3d-touch-images/info01.png "Przykłady 3D Touch włączone aplikacji")](3d-touch-images/info01.png#lightbox)

W tym artykule będzie udostępniać i wprowadzenie do Dodawanie gestów poufnych wykorzystania do uruchomionych na nowe telefonów iPhone 6s i telefonów iPhone 6s aplikacji platformy Xamarin.iOS przy użyciu nowych 3D Touch interfejsów API oraz urządzeń.

Z technologii 3D Touch aplikację telefonów iPhone jest teraz można nie tylko stwierdzić, że użytkownik jest dotknięcie ekranu urządzenia, ale jest w stanie znaczeniu nacisku wywiera użytkownika i reagowanie na wykorzystanie różnych poziomów.

3D Touch zapewnia następujące funkcje do aplikacji:

- [Wykorzystania czułości](#Pressure-Sensitivity) — aplikacje można teraz miar jak twarde lub światła użytkownika zachodzi ekranu i podejmij korzystać z tych informacji.
  Na przykład aplikacja rysowania wprowadzić wierszu grubszy lub cieńsza oparte na jaką użytkownik zachodzi ekranu.
- [Podgląd i wyświetl](#Peek-and-Pop) -aplikację teraz można pozwolić użytkownikowi na interakcję z jego dane bez konieczności Przejdź poza ich bieżący kontekst. Naciskając twardych na ekranie ekranu, ich wglądu element, który są zainteresowani (takich jak Podgląd wiadomości). Naciskając trudniejsze, ich pop do elementu.
- [Szybkie akcje](#Quick-Actions) — Pomyśl o szybkie akcji, takich jak menu kontekstowe, które mogą zostać zdjęte ze stosu w górę po kliknięciu elementu w aplikacji komputerowej.
  Przy użyciu szybkie akcje, można dodawać skróty do funkcji w aplikacji bezpośrednio z ikonę aplikacji na ekranie głównym.
- [Testowanie 3D Touch w symulatorze](#Testing-3D-Touch-in-the-Simulator) — z odpowiedni sprzęt Mac można przetestować 3D aplikacji z obsługą dotyku w symulatorze systemu iOS.

<a name="Pressure-Sensitivity" />

## <a name="pressure-sensitivity"></a>Czułość nacisku

Jak wspomniano powyżej, przy użyciu nowych właściwości [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) klasy można pomiaru wykorzystania użytkownika jest stosowana do ekranu urządzenia z systemem iOS i użyć tych informacji w interfejsie użytkownika. Na przykład wprowadzenie pędzla bardziej przezroczyste lub nieprzezroczyste na podstawie ilości wykorzystania.

[![](3d-touch-images/pressure01.png "Pędzla, renderowane jako bardziej przezroczyste lub nieprzezroczyste na podstawie ilości wykorzystania")](3d-touch-images/pressure01.png#lightbox)

W wyniku 3D Touch, jeśli aplikacja działa w systemie iOS 9 (lub nowsza), a urządzenia z systemem iOS jest w stanie obsługi technologii 3D Touch w wykorzystania spowoduje `TouchesMoved` się zdarzenia.

Na przykład w przypadku monitorowania `TouchesMoved` zdarzenie [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/), można użyć poniższego kodu, można pobrać bieżącego wykorzystania, który użytkownik wprowadza do ekranu:

```csharp
public override void TouchesMoved (NSSet touches, UIEvent evt)
{
    base.TouchesMoved (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        // Get the pressure
        var force = touch.Force;
        var maxForce = touch.MaximumPossibleForce;

        // Do something with the touch and the pressure
        ...
    }
}
```

`MaximumPossibleForce` Właściwość zwraca najwyższą możliwą wartość dla `Force` właściwość [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) oparty na urządzeniu z systemem iOS, że aplikacja jest uruchomiona na.

> [!IMPORTANT]
> Zmiany w wykorzystania mogą spowodować `TouchesMoved` się zdarzenia, nawet jeśli X / Y współrzędne nie uległy zmianie. Z powodu tej zmiany w zachowaniu aplikacji systemu iOS powinna być przygotowana do `TouchesMoved` zdarzenia wywoływanej częściej i x / Y koordynuje być taki sam, jak ostatni `TouchesMoved` wywołania.




Aby uzyskać więcej informacji, zobacz firmy Apple [TouchCanvas: przy użyciu UITouch skutecznego](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/) przykładową aplikację i [odwołania do klasy UITouch](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/).

<a name="Peek-and-Pop" />

## <a name="peek-and-pop"></a>Podgląd i Wyświetl

3D Touch oferuje nowe możliwości, aby użytkownik mógł korzystać z danych w aplikacji szybciej niż kiedykolwiek, bez konieczności przechodzenia z ich bieżącej lokalizacji.

Na przykład jeśli aplikacja jest wyświetlana tabeli wiadomości, użytkownik może nacisnąć twardych na element, aby podgląd jego zawartość w widoku nakładki (czyli Apple jako *wgląd*).

[![](3d-touch-images/peekandpop01.png "Przykład wgląd w zawartość")](3d-touch-images/peekandpop01.png#lightbox)

Gdy użytkownik naciśnie trudniejsze, będą oni wprowadzać widoku regularne wiadomości (który jest określany jako *Pop*-ping do widoku).

### <a name="checking-for-3d-touch-availability"></a>Sprawdzanie dostępności technologii 3D Touch

Podczas pracy z [UIViewController]() można użyć poniższego kodu, aby sprawdzić, czy aplikacja jest uruchomiona na urządzenia z systemem iOS obsługuje 3D Touch:

```csharp
public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
{
    //Important: call the base function
    base.TraitCollectionDidChange(previousTraitCollection);

    //See if the new TraitCollection value includes force touch
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        //Do something with 3D touch, for instance...
        RegisterForPreviewingWithDelegate (this, View);
        ...
```

Ta metoda może zostać wywołana przed *lub po* `ViewDidLoad()`. 

### <a name="handling-peek-and-pop"></a>Obsługa podglądu i Pop

Na urządzeniu z systemem iOS, który może obsługiwać technologii 3D Touch, możemy użyć wystąpienia `UIViewControllerPreviewingDelegate` klasy do obsługi wyświetlanie **wgląd** i **Pop** elementu szczegóły. Na przykład, jeśli było kontrolera widoku tabeli o nazwie `MasterViewController ` firma Microsoft może użyć poniższego kodu do obsługi **wgląd** i **Pop**:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace DTouch
{
    public class PreviewingDelegate : UIViewControllerPreviewingDelegate
    {
        #region Computed Properties
        public MasterViewController MasterController { get; set; }
        #endregion

        #region Constructors
        public PreviewingDelegate (MasterViewController masterController)
        {
            // Initialize
            this.MasterController = masterController;
        }

        public PreviewingDelegate (NSObjectFlag t) : base(t)
        {
        }

        public PreviewingDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        /// Present the view controller for the "Pop" action.
        public override void CommitViewController (IUIViewControllerPreviewing previewingContext, UIViewController viewControllerToCommit)
        {
            // Reuse Peek view controller for details presentation
            MasterController.ShowViewController(viewControllerToCommit,this);
        }

        /// Create a previewing view controller to be shown at "Peek".
        public override UIViewController GetViewControllerForPreview (IUIViewControllerPreviewing previewingContext, CGPoint location)
        {
            // Grab the item to preview
            var indexPath = MasterController.TableView.IndexPathForRowAtPoint (location);
            var cell = MasterController.TableView.CellAt (indexPath);
            var item = MasterController.dataSource.Objects [indexPath.Row];

            // Grab a controller and set it to the default sizes
            var detailViewController = MasterController.Storyboard.InstantiateViewController ("DetailViewController") as DetailViewController;
            detailViewController.PreferredContentSize = new CGSize (0, 0);

            // Set the data for the display
            detailViewController.SetDetailItem (item);
            detailViewController.NavigationItem.LeftBarButtonItem = MasterController.SplitViewController.DisplayModeButtonItem;
            detailViewController.NavigationItem.LeftItemsSupplementBackButton = true;

            // Set the source rect to the cell frame, so everything else is blurred.
            previewingContext.SourceRect = cell.Frame;

            return detailViewController;
        }
        #endregion
    }
}
```

`GetViewControllerForPreview` Metoda jest używana do wykonywania **wgląd** operacji. Uzyskuje dostęp do danych komórek i tworzenia kopii tabeli, a następnie załadowanie `DetailViewController` z bieżącym scenorysu. Przez ustawienie `PreferredContentSize` do (0,0) prosimy dla domyślnej **wgląd** rozmiar widoku. Na koniec mamy rozmycia wszystko, ale komórki wyświetlane z `previewingContext.SourceRect = cell.Frame` i zostanie zwrócona nowy widok do wyświetlenia.

`CommitViewController` Ponownie używa widoku, utworzyliśmy w **wgląd** dla **Pop** widoku, gdy użytkownik naciśnie trudniejsze.

### <a name="registering-for-peek-and-pop"></a>Rejestrowanie na potrzeby wglądu i Pop

Z kontrolera widoku, który chcemy, aby zezwolić użytkownikowi na **wgląd** i **Pop** elementów, należy zarejestrować dla tej usługi. W przykładzie podanym wyżej kontrolera widoku tabeli (`MasterViewController`), czy możemy użyć poniższego kodu:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Check to see if 3D Touch is available
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        // Regiser for Peek and Pop
        RegisterForPreviewingWithDelegate(new PreviewingDelegate(this), View);
    }
    ...

}
```

W tym miejscu wywołania `RegisterForPreviewingWithDelegate` metody przy użyciu wystąpienia `PreviewingDelegate` utworzyliśmy powyżej. Na urządzeniach z systemem iOS, które obsługą technologii 3D Touch użytkownik może nacisnąć twardych element, aby wglądu w jej. Jeśli ich nacisku nawet, element zostanie Pop do niego standardowe wyświetlania widoku.

Aby uzyskać więcej informacji, zobacz nasze [systemu iOS 9 próbki ApplicationShortcuts](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) i firmy Apple [ViewControllerPreviews: przy użyciu UIViewController Podgląd interfejsów API](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html) Przykładowa aplikacja [ Odwołania do klasy UIPreviewAction](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/), [odwołania do klasy UIPreviewActionGroup](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/) i [referencyjne protokołu UIPreviewActionItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/).

<a name="Quick-Actions" />

## <a name="quick-actions"></a>Szybkie akcje

Przy użyciu 3D Touch i szybkie akcje, można dodać wspólne, szybkie i łatwe do skrótów dostęp do funkcji w aplikacji z głównej ikony ekranu na urządzeniu z systemem iOS.

Jak już wspomniano, możesz traktować szybkie akcje jak menu kontekstowe, które mogą zostać zdjęte ze stosu w górę po kliknięciu elementu w aplikacji komputerowej. Zapewnienie skróty do najbardziej typowych funkcji lub właściwości aplikacji należy używać szybkie akcje.

[![](3d-touch-images/quickactions01.png "Przykład menu Szybkie akcje")](3d-touch-images/quickactions01.png#lightbox)


### <a name="defining-static-quick-actions"></a>Definiowanie statycznego szybkie akcje

Jeśli co najmniej jeden szybkie akcje wymagane przez aplikację są statyczne i nie trzeba zmieniać, należy je zdefiniować w aplikacji `Info.plist` pliku. Edytuj ten plik w edytorze zewnętrznym i dodaj następujące klucze:

```xml
<key>UIApplicationShortcutItems</key>
<array>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeSearch</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will search for an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Search</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.000</string>
    </dict>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeShare</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will share an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Share</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.001</string>
    </dict>
</array>
```

W tym miejscu możemy definiowania dwa statycznego szybkie czynności do wykonania z następujących kluczy:

- `UIApplicationShortcutItemIconType` -Określenie ikony, która będzie wyświetlana przez element szybkiego działania jako jeden z następujących wartości:
  - `UIApplicationShortcutIconTypeAdd`
  - `UIApplicationShortcutIconTypeAlarm`
  - `UIApplicationShortcutIconTypeAudio`
  - `UIApplicationShortcutIconTypeBookmark`
  - `UIApplicationShortcutIconTypeCapturePhoto`
  - `UIApplicationShortcutIconTypeCaptureVideo`
  - `UIApplicationShortcutIconTypeCloud`
  - `UIApplicationShortcutIconTypeCompose`
  - `UIApplicationShortcutIconTypeConfirmation`
  - `UIApplicationShortcutIconTypeContact`
  - `UIApplicationShortcutIconTypeDate`
  - `UIApplicationShortcutIconTypeFavorite`
  - `UIApplicationShortcutIconTypeHome`
  - `UIApplicationShortcutIconTypeInvitation`
  - `UIApplicationShortcutIconTypeLocation`
  - `UIApplicationShortcutIconTypeLove`
  - `UIApplicationShortcutIconTypeMail`
  - `UIApplicationShortcutIconTypeMarkLocation`
  - `UIApplicationShortcutIconTypeMessage`
  - `UIApplicationShortcutIconTypePause`
  - `UIApplicationShortcutIconTypePlay`
  - `UIApplicationShortcutIconTypeProhibit`
  - `UIApplicationShortcutIconTypeSearch`
  - `UIApplicationShortcutIconTypeShare`
  - `UIApplicationShortcutIconTypeShuffle`
  - `UIApplicationShortcutIconTypeTask`
  - `UIApplicationShortcutIconTypeTaskCompleted`
  - `UIApplicationShortcutIconTypeTime`
  - `UIApplicationShortcutIconTypeUpdate`

        ![](3d-touch-images/uiapplicationshortcuticontype.png "UIApplicationShortcutIconType imagery")

* `UIApplicationShortcutItemSubtitle` -Definiuje pomocniczą dla elementu.
* `UIApplicationShortcutItemTitle` -Określa tytuł elementu.
* `UIApplicationShortcutItemType` -Jest wartość ciągu, który będzie używany do identyfikowania elementów w naszej aplikacji. Aby uzyskać więcej informacji, zobacz następującą sekcję.

> [!IMPORTANT]
> Szybkie elementy skrótów akcji, które są ustawiane w `Info.plist` nie można uzyskać dostępu do pliku z `Application.ShortcutItems` właściwości. Są one tylko przekazywane w celu `HandleShortcutItem` obsługi zdarzeń. 





### <a name="identifying-quick-action-items"></a>Identyfikowanie szybkie czynności do wykonania

Jak przedstawiono powyżej, gdy zdefiniowany określone elementy szybkiego akcji w Twojej aplikacji `Info.plist`, przypisane wartości ciągu na `UIApplicationShortcutItemType` klucz, aby zidentyfikować je.

Aby ułatwić tych identyfikatorów w kodzie, Dodaj klasę o nazwie `ShortcutIdentifier` aplikacji projektu i dołączyć je wyglądać następująco:

```csharp
using System;

namespace AppSearch
{
    public static class ShortcutIdentifier
    {
        public const string First = "com.company.appname.000";
        public const string Second = "com.company.appname.001";
        public const string Third = "com.company.appname.002";
        public const string Fourth = "com.company.appname.003";
    }
}
```

<a name="Handling-a-Quick-Action" />

### <a name="handling-a-quick-action"></a>Obsługa szybkiego akcji

Następnie należy zmodyfikować aplikacji `AppDelegate.cs` pliku do obsługi użytkownikowi wybranie elementu szybkich akcji z ikony aplikacji na ekranie głównym.

Wprowadź następujące zmiany:

```csharp
using System;
...

public UIApplicationShortcutItem LaunchedShortcutItem { get; set; }

public bool HandleShortcutItem(UIApplicationShortcutItem shortcutItem) {
    var handled = false;

    // Anything to process?
    if (shortcutItem == null) return false;

    // Take action based on the shortcut type
    switch (shortcutItem.Type) {
    case ShortcutIdentifier.First:
        Console.WriteLine ("First shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Second:
        Console.WriteLine ("Second shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Third:
        Console.WriteLine ("Third shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Fourth:
        Console.WriteLine ("Forth shortcut selected");
        handled = true;
        break;
    }

    // Return results
    return handled;
}

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    return shouldPerformAdditionalDelegateHandling;
}

public override void OnActivated (UIApplication application)
{
    // Handle any shortcut item being selected
    HandleShortcutItem(LaunchedShortcutItem);

    // Clear shortcut after it's been handled
    LaunchedShortcutItem = null;
}

public override void PerformActionForShortcutItem (UIApplication application, UIApplicationShortcutItem shortcutItem, UIOperationHandler completionHandler)
{
    // Perform action
    completionHandler(HandleShortcutItem(shortcutItem));
}
```

Najpierw należy zdefiniować publiczny `LaunchedShortcutItem` właściwość, aby śledzić ostatni element szybkie działanie wybranych przez użytkownika. Następnie możemy zastąpić `FinishedLaunching` — metoda i zobacz, jeśli `launchOptions` zostały przekazane i jeśli ich zawiera element szybkich akcji. Jeśli nie, przechowujemy szybkich akcji w `LaunchedShortcutItem` właściwości.

Następnie możemy zastąpić `OnActivated` — metoda i przebiegu żadnego wybrany element Szybkie uruchamianie `HandleShortcutItem` metodę, aby być przetwarzane. Obecnie możemy tylko pisania wiadomości **konsoli**. W rzeczywistym aplikacji będzie obsługiwać co ever akcji była wymagana. Po podjęciu działania, `LaunchedShortcutItem` wyczyścić właściwości.

Na koniec, jeśli aplikacja jest już uruchomiona, `PerformActionForShortcutItem` będzie wywoływana metoda obsługuje szybkiego czynności do wykonania, dlatego musimy ją zastąpić, a następnie wywołać naszych `HandleShortcutItem` metody tutaj również.


### <a name="creating-dynamic-quick-action-items"></a>Tworzenie elementów szybkie akcje dynamiczne

Oprócz definiowania statycznego szybkie działanie elementów w Twojej aplikacji `Info.plist` plików, można tworzyć dynamiczne na bieżąco szybkie akcje. Aby zdefiniować dwa nowe dynamiczne szybkie akcje, Edytuj Twojej `AppDelegate.cs` plik ponownie i zmodyfikuj `FinishedLaunching` metodę wyglądać podobnie do następującego:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    // Add dynamic shortcut items
    if (application.ShortcutItems.Length == 0) {
        var shortcut3 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Third, "Play") {
            LocalizedSubtitle = "Will play an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Play)
        };

        var shortcut4 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Fourth, "Pause") {
            LocalizedSubtitle = "Will pause an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Pause)
        };

        // Update the application providing the initial 'dynamic' shortcut items.
        application.ShortcutItems = new UIApplicationShortcutItem[]{shortcut3, shortcut4};
    }

    return shouldPerformAdditionalDelegateHandling;
}
```

Teraz sprawdzamy, czy `application` już zawiera zestaw tworzone dynamicznie `ShortcutItems`, w przeciwnym razie utwórz dwa nowe `UIMutableApplicationShortcutItem` obiekty do definiowania nowych elementów i dodać je do `ShortcutItems` tablicy.

Kod już dodana w [Obsługa szybkiego akcji](#Handling-a-Quick-Action) powyższej sekcji będzie obsługiwać te dynamiczne szybkie akcje, podobnie jak te statycznych.

Należy zauważyć, że można tworzyć kombinację statycznych i dynamicznych elementów szybkich akcji (zgodnie z tym tutaj), nie są ograniczone do jednego lub drugiego.

Aby uzyskać więcej informacji, skontaktuj się z naszym [systemu iOS 9 próbki ViewControllerPreview](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) i zobacz firmy Apple [ApplicationShortcuts: UIApplicationShortcutItem przy użyciu](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/) przykładowej aplikacji [ Odwołania do klasy UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/), [odwołania do klasy UIMutableApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/) i [odwołania do klasy UIApplicationShortcutIcon](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/).

<a name="Testing-3D-Touch-in-the-Simulator" />

## <a name="testing-3d-touch-in-the-simulator"></a>Testowanie 3D Touch w symulatorze

Przy użyciu najnowszej wersji Xcode i symulatora systemu iOS na komputerze Mac zgodne z życie Touch włączenia trackpad, można sprawdzić w symulatorze 3D Touch funkcji.

Aby włączyć tę funkcję, uruchom dowolną aplikację w symulowane iPhone sprzęt obsługuje 3D Touch (telefonów iPhone 6s lub nowszej). Następnie wybierz pozycję **sprzętu** menu w systemie iOS Simulator i Włącz **Użyj Trackpad życie dla technologii 3D touch** element menu:

[![](3d-touch-images/simulator01.png "Wybierz menu sprzętu w narzędziu iOS Simulator i Włącz Trackpad Force używana dla elementu menu 3D touch")](3d-touch-images/simulator01.png#lightbox)

Ta funkcja jest aktywny można nacisku na trackpad Mac umożliwiające 3D Touch, tak jak na sprzęcie rzeczywistych iPhone.

## <a name="summary"></a>Podsumowanie

W tym artykule wprowadziła nowych 3D Touch interfejsów API dostępne w systemie iOS 9 dla telefonów iPhone 6s i telefonów iPhone 6s Plus. Pasuje do dodawania czułości nacisku aplikacji; przy użyciu wglądu i Pop, aby szybko wyświetlić informacje w aplikacji z bieżącego kontekstu bez nawigacji; i zapewnienie skróty do aplikacji przy użyciu szybkie akcje na najczęściej używane funkcje.



## <a name="related-links"></a>Linki pokrewne

- [System iOS 9 ViewControllerPreview próbki](https://developer.xamarin.com/samples/monotouch/ios9/ViewControllerPreview/)
- [System iOS 9 ApplicationShortcuts próbki](https://developer.xamarin.com/samples/monotouch/ios9/ApplicationShortcuts/)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Przygotowanie aplikacji iPhone dla technologii 3D Touch](https://developer.apple.com/ios/3d-touch/)
