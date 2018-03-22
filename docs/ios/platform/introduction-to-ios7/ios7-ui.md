---
title: "System iOS 7 omówienie interfejsu użytkownika"
description: "System iOS 7 wprowadza nadmiar zmiany interfejsu użytkownika. W tym artykule omówiono niektóre zmiany większe, zarówno w wygląd formantów i interfejsów API, który obsługuje nowy projekt."
ms.topic: article
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3d70aff4df91120402e2987598b8973172b46245
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="ios-7-user-interface-overview"></a>System iOS 7 omówienie interfejsu użytkownika

_System iOS 7 wprowadza nadmiar zmiany interfejsu użytkownika. W tym artykule omówiono niektóre zmiany większe, zarówno w wygląd formantów i interfejsów API, który obsługuje nowy projekt._

System iOS 7 koncentruje się na zawartości za pośrednictwem programu chrome. Elementy interfejsu użytkownika w systemie iOS 7 uwypuklające chrome przez usunięcie atrybutów, takich jak nadmiarowe obramowania, pasków stanu i pasków nawigacji, które zmniejszenie ilości miejsca na ekranie używane przez widoki zawartości. W systemie iOS 7 zawartość jest przeznaczona do użycia cały ekran.

System iOS 7 wprowadza inne zmiany: kolor służy do odróżniania elementów interfejsu użytkownika, zamiast atrybutów, takich jak obramowania przycisku. Wiele elementów, takich jak pasków nawigacji i paski stanu są teraz rozmyte i przezroczyste lub przezroczyste z widoków zawartości biorąc obszaru poniżej. Widoki te zawartości renderowania za pośrednictwem rozmyte pasków przekazywania wrażenie hierarchii w interfejsie użytkownika.

W tym artykule opisano kilka zmian, aby elementy interfejsu użytkownika w systemie iOS 7, jak również różnych interfejsów API powiązane z nowego projektu interfejsu użytkownika.

## <a name="view-and-control-changes"></a>Wyświetlanie i kontroli zmiany

Wszystkie widoki w UIKit odpowiadają nowego wyglądu i działania systemu iOS 7. W tej sekcji opisano niektóre zmiany w tych widoków, a także powiązanych interfejsów API, które zostały zmienione w celu obsługi nowego interfejsu użytkownika.

### <a name="uibutton"></a>UIButton

Przyciski utworzone na podstawie `UIButton` klasy są teraz bez obramowania, nie tle domyślnie, jak pokazano poniżej:

 ![](ios7-ui-images/button.png "UIButton próbki")

`UIButtonType.RoundedRect` Styl jest przestarzała. Jeśli używane w systemie iOS 7, `UIButtonType.RoundedRect` spowoduje `UIButtonType.System` używany, która tworzy domyślny styl przycisku bez tło lub widoczna krawędzi, zgodnie z powyższym.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

Podobnie jak `UIButton`, pasek przyciski są również bez obramowania, domyślnie używany do nowego `UIBarButtonItemStyle.Plain` styl pokazano poniżej:

 ![](ios7-ui-images/barbuttonplain.png "UIBarButtonItem próbki")

Ponadto `UIBarButtonItemStyle.Bordered` styl jest przestarzała. Ustawienie `UIBarButtonItemStyle.Bordered` w systemie iOS 7 spowoduje `UIBarButtonItemStyle.Plain` styl używany.

`UIBarButtonItemStyle.Done` Stylu nie jest przestarzała. Jednak zostanie również utworzony bez obramowania przycisku tylko przy użyciu stylu pogrubioną, jak pokazano:

 ![](ios7-ui-images/barbuttondone.png "Przykładowe UIBarButtonItem w stylu gotowe")

### <a name="uialertview"></a>UIAlertView

Oprócz zmiany stylu dla nowych systemów iOS 7 i wygląd widoki alertów nie obsługuje dostosowywania za pomocą widok podrzędny. Mimo że `UIAlertView` dziedziczy `UIView`, wywoływania `AddSubview` na `UIAlertView` nie ma wpływu. Rozważmy na przykład następujący kod:

```csharp
UIBarButtonItem button = new UIBarButtonItem ("Bar Button", UIBarButtonItemStyle.Plain, (s,e) =>
{
    UIAlertView alert = new UIAlertView ("Title", "Message", null, "Cancel", "OK");

    alert.AddSubview (new UIView () {
        Frame = new CGRect(50, 50,100, 100),
        BackgroundColor = UIColor.Green
    });

    alert.Show ();
});
```

Daje to standardowy widok alertów z widok podrzędny, są ignorowane, jak pokazano poniżej:

 ![](ios7-ui-images/alert.png "UIAlertView próbki")
 
 Uwaga: UIAlertView została uznana za przestarzałą w systemie iOS 8. Widok [alertu kontrolera](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/) przepisu przy użyciu widoku alertów w systemie iOS 8 i nowszych.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

Formanty segmentu w systemie iOS 7 są niewidoczne i obsługuje kolor odcienia. Kolor odcienia jest używany kolor tekstu i obramowania. Po wybraniu segment kolor jest zamieniona między tła i tekstu, na kolor odcienia używany do Wyróżnij wybranego segmentu, jak pokazano poniżej:

 ![](ios7-ui-images/segmentedcontrol.png "UISegmentedControl próbki")

Ponadto `UISegmentedControlStyle` jest przestarzała w systemie iOS 7.

### <a name="picker-views"></a>Selektor widoków

Interfejs API dla widoków selektora jest przede wszystkim bez zmian; Jednak system iOS 7 wskazówek teraz stanu widoków selektora powinny być prezentowane wbudowany, a nie jako animowany wejściowych widoków z dołu ekranu lub za pomocą nowego kontrolera wypychana na stos kontrolera nawigacji, jak w poprzednich iOS wersji. Można to zaobserwować w aplikacji kalendarza systemu:

 ![](ios7-ui-images/inlinepicker.png "Można to zaobserwować w aplikacji kalendarza systemu")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

Pasek wyszukiwania jest teraz wyświetlany w obszarze nawigacji pasek kiedy `UISearchDisplayController.DisplaysSearchBarInNavigationBar` właściwość jest ustawiona na true. Jeśli równa FAŁSZ — wartość domyślna — pasek nawigacyjny jest ukryty, gdy kontroler wyszukiwania jest wyświetlany.

Poniższy zrzut ekranu przedstawia pasek wyszukiwania w `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "UISearchDisplayController próbki")

### <a name="uitableview"></a>UITableView

Interfejsy API wokół `UITableView` są głównie bez zmian; jednak styl zmienił znacząco są zgodne z nowego projektu interfejsu użytkownika. Wyświetlanie wewnętrznej hierarchii jest również nieco inny. Ta zmiana nie wpływa na większości aplikacji, ale jest to element znać.

#### <a name="grouped-table-style"></a>Styl tabeli grupowanych

Styl grupowanych zmienić został zaktualizowany o zawartości teraz rozszerzenia na krawędzi ekranu, jak pokazano poniżej:

 ![](ios7-ui-images/table1.png "Przykładowa tabela grupowanych stylu")

#### <a name="separatorinset"></a>SeparatorInset

Separatory wierszy może być teraz wcięty przez ustawienie `UITableVIewCell.SeparatorInset` właściwości. Na przykład następujący kod będzie służyć do wcięcie komórek od lewej krawędzi:

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

Daje to w widoku tabeli, z wcięciami komórek w sposób przedstawiony poniżej:

 ![](ios7-ui-images/separatorinset.png "Przykładowe UITableView SeparatorInset")

#### <a name="table-button-styles"></a>Style przycisku tabeli

Przyciski różnych używanych w widokach tabeli wszystkie zmieniły się. Poniższy zrzut ekranu przedstawia widok tabeli w trybie edycji:

 ![](ios7-ui-images/table2.png "Ten zrzut ekranu przedstawia widok tabeli w trybie edycji")

### <a name="additional-control-changes"></a>Dodatkową kontrolę zmian

Inne formanty UIKit zostały zmienione także, w tym suwaki, przełączniki i steppery. Te zmiany są wyłącznie visual. Aby uzyskać więcej informacji, zapoznaj się firmy Apple [przewodnik dotyczący przejścia interfejsu użytkownika dla systemu iOS 7](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html).

## <a name="general-user-interface-changes"></a>Interfejs użytkownika ogólne zmiany

Oprócz zmian w UIKit, system iOS 7 wprowadzono szereg zmiany wizualne do interfejsu użytkownika, w tym:

-  Zawartość pełny ekran
-  Wygląd paska
-  Kolor odcienia

<a name="fullscreen" />

### <a name="full-screen-content"></a>Zawartość pełnego ekranu

System iOS 7 jest przeznaczona do pozwolić wykorzystać cały ekran aplikacji. Widok kontrolery są teraz wyświetlane nachodzące pasek stanu i pasek nawigacyjny — Jeśli istnieje - znajdujące się poniżej pasków stanu i nawigacja w przeciwieństwie.

Podczas przygotowywania aplikacji dla systemu iOS 7 można wyrównać widoków podrzędnych w sposób wizualny przy użyciu *konstruktora interfejsu* lub *Xamarin iOS projektanta*. Umożliwia także jedną z nowych interfejsów API które dodatkowo ułatwią obsługę zawartości w trybie pełnoekranowym programowo. Te interfejsy API są wprowadzane poniżej.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide i BottomLayoutGuide

 `TopLayoutGuide` i `BottomLayoutGuide` służyć jako odwołanie dla których widoków powinny rozpoczynać się ani kończyć, tak, aby zawartość nie nakłada się półprzezroczyste `UIKit` pasek, jak w poniższym przykładzie:

 [![](ios7-ui-images/clipped.png "Przykładowe zawartości nie nakłada się pasek UIKit półprzezroczyste")](ios7-ui-images/clipped.png#lightbox)

Te interfejsy API mogą być używane do obliczania widoku przesunięcie od góry lub u dołu ekranu, a następnie odpowiednio umieszczania zawartości:

```csharp
public override void ViewDidLayoutSubviews ()
{
    base.ViewDidLayoutSubviews ();

    if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
        nfloat displacement_y = this.TopLayoutGuide.Length;

        //load subviews with displacement
    }

}
```

Możemy użyć obliczonego powyżej, aby ustawić wartość naszych `ImageView`przez przesunięcie od górnej krawędzi ekranu, tak aby widoczne całego obrazu:

 [![](ios7-ui-images/good2.png "Przykład ImageViews przesunięcie z góry ekranu")](ios7-ui-images/good2.png#lightbox)

Zapoznaj się [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) dla przykładu pracy.

Wartość przesunięcia jest generowany dynamicznie po widok został dodany do hierarchii, dlatego próby odczytu `TopLayoutGuide` i `BottomLayoutGuide` wartości w `ViewDidLoad` zwraca wartość 0. Obliczanie wartości po widok został załadowany — na przykład w `ViewDidLayoutSubviews`.

> [!IMPORTANT]
> `TopLayoutGuide` i `BottomLayoutGuide` zostały uznane za przestarzałe w systemie iOS 11 na rzecz nowy układ obszaru bezpieczne. Apple ma stwierdził, że użycie bezpiecznego miejsca jest zgodny z systemem iOS w wersji wcześniejszej niż iOS 11. Aby uzyskać więcej informacji, zobacz [aktualizowanie aplikacji dla systemu iOS 11](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) przewodnik.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

Ten interfejs API Określa, które krawędzi widoku wydłużony do pełnego ekranu, niezależnie od tego paska przejrzystości. W systemie iOS 7 pasków nawigacji i paski narzędzi są wyświetlane warstwę ponad kontrolera widoku — w odróżnieniu od w systemie iOS poprzedniej wersji, której nie zajmują tą samą przestrzenią. Aplikacje systemu iOS 7 zdjęć przedstawiono domyślne `UIViewController.EdgesForExtendedLayout` wartość `UIRectEdge.All`. To ustawienie wypełnia wszystkich czterech krawędzi w widoku z zawartością, tworzenie efektu nakładających się i pełnego ekranu:

 [![](ios7-ui-images/photos.png "EdgesForExtendedLayout próbki")](ios7-ui-images/photos.png#lightbox)

Naciskając obrazu usuwa pasków i przedstawia pełnoekranowym obrazu:

 [![](ios7-ui-images/photos2.png "EdgesForExtendedLayout z paskami usunięte")](ios7-ui-images/photos2.png#lightbox)

Ponieważ zawartość pełnego ekranu jest wartość domyślna, aplikacje skonfigurowane pod kątem iOS 6 mają część widoku przycinana, tak jak na poniższym zrzucie ekranu:

 [![](ios7-ui-images/clipped.png "Aplikacje skonfigurowane dla systemu iOS 6 będzie mieć część widoku przycinana, tak jak tego zrzutu ekranu")](ios7-ui-images/clipped.png#lightbox)

Modyfikowanie `UIViewController.EdgesForExtendedLayout` ustawia właściwości tego zachowania. Firma Microsoft można określić, czy widok nie wypełnia żadnych krawędzi, więc naszych widoku pozwoli uniknąć wyświetlania zawartości w miejsce zajmowane przez nawigacji lub paski narzędzi (w każdym orientacja):

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

W naszej aplikacji możemy wyświetlony zostanie ponownie zmienić ich pozycji widok, aby cały obraz jest widoczne:

 [![](ios7-ui-images/good.png "Przykład: widoczne całego obrazu")](ios7-ui-images/good.png#lightbox)

Należy pamiętać, że podczas skutków `TopLayoutGuide/BottomLayoutGuide` i `EdgesForExtendedLayout` interfejsy API są podobne, są przeznaczone do wypełniania różnych celów. Zmiana `EdgesForExtendedLayout` ustawienie domyślnego mogą ustalić przyciętą widoków w aplikacji dla systemu iOS 6, ale projektowanie dobrej systemów iOS 7 należy przestrzegać estetycznych pełnego ekranu i podaj pełnego ekranu przeglądania środowisko, zależne `TopLayoutGuide` i `BottomLayoutGuide`prawidłowo umieścić zawartość, która ma operować na wygodne miejsce dla użytkownika.

Zapoznaj się [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) dla przykładu pracy.

### <a name="status-and-navigation-bars"></a>Stan i pasków nawigacji

Pasek stanu i pasków nawigacji są renderowane z przezroczystości. Paski stanu są niewidoczne, pasków narzędzi i pasków nawigacji są przezroczyste i rozmyty w celu przedstawienia wrażenie hierarchii w interfejsie użytkownika. Poniższy zrzut ekranu przedstawia ten rozmycia i przejrzystości, w którym wyświetlana kolor tła niebieski widok kolekcji za pomocą pasków stanu i nawigacji, zapewniając im światła wygląd niebieski:

 ![](ios7-ui-images/transparent-navbar.png "Przykładowe stanu i rozmycia — pasek nawigacyjny")

#### <a name="status-bar-styles"></a>Style paska stanu

Wraz z rozmycia i przejrzystości na pierwszym planie pasek stanu może być jasny i ciemny (ciemny jest wartość domyślna). Style paska stanu można ustawić kontroler widoku. Kontroler widoku można również określić, czy pasek stanu jest ukryte lub wyświetlane.

Na przykład poniższy kod zastąpienia `PreferredStatusBarStyle` metody kontrolera widoku, aby wyświetlić światła pierwszego planu pasek stanu:

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

Powoduje to, że są wyświetlane na pasku stanu jak poniżej:

 ![](ios7-ui-images/light-status-bar.png "Przykładowy pasek stanu")

Aby ukryć pasek stanu z kontrolera widoku kodu, należy zastąpić `PrefersStatusBarHidden`, jak pokazano poniżej:

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

To ukrywa pasek stanu:

 ![](ios7-ui-images/status-bar-hidden.png "Ukryte paska stanu")

### <a name="tint-color"></a>Kolor odcienia

Przyciski są teraz wyświetlane jako tekst bez chrome. Kolor tekstu można sterować za pomocą nowej `TintColor` właściwość `UIView`. Ustawienie `TintColor` dotyczy kolor hierarchii widok, który ustawia ją dla całego widoku. Aby zastosować `TintColor`w całej aplikacji, ustaw go na `Window`. Można także wykryć gdy zmienia kolor odcienia za pośrednictwem `UIView.TintColorDidChange` metody.

Na przykład poniższy zrzut ekranu przedstawia wynik zmiana koloru odcień w widoku kontrolera nawigacji na purpurowy:

 ![](ios7-ui-images/tint-color.png "Kolor odcienia purpurowy w widoku kontrolerów nawigacji")

Kolor odcienia może odnosić się do obrazów, a także gdy `RenderingMode` ma ustawioną wartość `UIImageRenderingMode.AlwaysTemplate`.

> [!IMPORTANT]
> Kolor odcienia, nie można ustawić za pomocą `UIAppearance`.


### <a name="dynamic-type"></a>Typ dynamiczny

W systemie iOS 7 użytkownik może określić rozmiar tekstu w ustawieniach systemu. Z typu dynamicznego czcionki jest dynamicznie dostosowywana do wyglądać niezależnie od rozmiaru. `UIFont.PreferredFontForTextStyle` należy pobrać czcionki, która jest zoptymalizowana dla rozmiaru kontrolowane przez użytkownika.

## <a name="summary"></a>Podsumowanie

W tym artykule omówiono zmiany do elementów interfejsu użytkownika w systemie iOS 7. Go sprawdza, czy niektóre zmiany wprowadzone do widoków i formantów w UIKit wyróżnianie zmiany wizualne podobnie jak zmiany do powiązanego interfejsów API. Ponadto podaj nowe interfejsy API do pracy z zawartością pełny ekran, nowa funkcja obsługi kolor odcienia i typu dynamicznego.

## <a name="related-links"></a>Linki pokrewne

- [ImageViewer (przykład)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
