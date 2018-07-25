---
title: System iOS 7 omówienie interfejsu użytkownika
description: System iOS 7 wprowadza mnóstwo zmiany w interfejsie użytkownika. W tym artykule opisano niektóre większe zmiany zarówno w wygląd kontrolek i interfejsów API, który obsługuje nowy projekt.
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7e7725418ed104bd9be014b80d20fd62de144ca5
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242293"
---
# <a name="ios-7-user-interface-overview"></a>System iOS 7 omówienie interfejsu użytkownika

_System iOS 7 wprowadza mnóstwo zmiany w interfejsie użytkownika. W tym artykule opisano niektóre większe zmiany zarówno w wygląd kontrolek i interfejsów API, który obsługuje nowy projekt._

System iOS 7 koncentruje się na zawartości za pośrednictwem przeglądarki chrome. Elementy interfejsu użytkownika w systemie iOS 7 uwypuklające chrome, usuwając atrybutów, takich jak nadmiarowe obramowania, paski stanu i pasków nawigacji, które zmniejszają ilość miejsca na ekranie, używany przy użyciu funkcji widoków zawartości. W systemie iOS 7 zawartość jest przeznaczony do stosowania cały ekran.

System iOS 7 wprowadzono kilka innych zmian: kolor jest używany do odróżniania elementów interfejsu użytkownika, audytów atrybutów, takich jak obramowania przycisku. Wiele elementów, takich jak pasków nawigacji i paski stanu są teraz rozmyte i przezroczyste lub przezroczyste z widoków zawartości, biorąc obszar poniżej. Widoki te zawartości renderowania za pośrednictwem rozmyte pasków, przekazywania czują się głębokość w interfejsie użytkownika.

W tym artykule omówiono kilka zmian do elementów interfejsu użytkownika w systemie iOS 7, jak również różne interfejsy API powiązane z nowego projektu interfejsu użytkownika.

## <a name="view-and-control-changes"></a>Wyświetlanie i kontroli zmiany

Wszystkie widoki w strukturze UIKit zgodne z nowego wyglądu i działania systemu iOS 7. W tej sekcji opisano niektóre zmiany w tych widokach, a także powiązanych interfejsów API, które zostały zmienione w celu obsługi nowego interfejsu użytkownika.

### <a name="uibutton"></a>Obiektu klasy UIButton

Przyciski tworzone na podstawie `UIButton` klasy są teraz bez obramowania, nie tła, domyślnie, jak pokazano poniżej:

 ![](ios7-ui-images/button.png "Przykład obiektu klasy UIButton")

`UIButtonType.RoundedRect` Stylu jest przestarzała. Jeśli używane w systemie iOS 7, `UIButtonType.RoundedRect` spowoduje `UIButtonType.System` używany, wytwarzająca domyślny styl przycisku bez tła i krawędzie widoczne, jak pokazano powyżej.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

Podobnie jak `UIButton`, pasek przyciski są również bez obramowania, ustawiając domyślnie do nowych `UIBarButtonItemStyle.Plain` styl pokazano poniżej:

 ![](ios7-ui-images/barbuttonplain.png "Przykładowe UIBarButtonItem")

Ponadto `UIBarButtonItemStyle.Bordered` stylu jest przestarzała. Ustawienie `UIBarButtonItemStyle.Bordered` w systemie iOS 7 będzie skutkować `UIBarButtonItemStyle.Plain` styl używany.

`UIBarButtonItemStyle.Done` Styl nie jest przestarzała. Jednakże utworzy również bez obramowania przycisku, tylko w przypadku styl pogrubiony, jak pokazano:

 ![](ios7-ui-images/barbuttondone.png "Przykładowe UIBarButtonItem w stylu gotowe")

### <a name="uialertview"></a>UIAlertView

Oprócz zmianę stylu dla nowego systemu iOS 7 wyglądu i działania widoki alertów nie obsługują już dostosowywania za pośrednictwem widok podrzędny. Mimo że `UIAlertView` dziedziczy `UIView`, wywoływania `AddSubview` na `UIAlertView` nie ma wpływu. Na przykład rozważmy następujący kod:

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

Tworzy to standardowy widok alertów z widok podrzędny, są ignorowane, jak pokazano poniżej:

 ![](ios7-ui-images/alert.png "Przykładowe UIAlertView")
 
 Uwaga: UIAlertView została zakończona w systemie iOS 8. Widok [alertu kontrolera](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller) przepis na temat korzystania z widoku alertów w systemie iOS 8 lub nowszym.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

Kontrolki segmentowane w systemie iOS 7 są niewidoczne i obsługuje odcień koloru. Kolor odcienia jest używana do koloru tekstu i obramowanie. Po wybraniu segment kolor jest wymieniany między tła i tekstu, za pomocą kolor odcienia używany, aby wyróżnić wybranego segmentu, jak pokazano poniżej:

 ![](ios7-ui-images/segmentedcontrol.png "Przykładowe UISegmentedControl")

Ponadto `UISegmentedControlStyle` jest przestarzała w systemie iOS 7.

### <a name="picker-views"></a>Selektor widoków

Interfejs API selektor widoków jest w dużej mierze bez zmian; jednak wytyczne dotyczące projektowania 7 dla systemu iOS teraz stanu selektor widoków powinno zostać wyświetlone w tekście, a nie jako widoki wejściowe animowane od dolnej części ekranu lub za pośrednictwem nowego kontrolera wypychane na stosie kontrolera nawigacji, tak jak w poprzednim dla systemu iOS wersji. Można to zaobserwować w aplikacji systemu kalendarza:

 ![](ios7-ui-images/inlinepicker.png "Można to zaobserwować w aplikacji kalendarza systemu")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

Na pasku wyszukiwania są teraz wyświetlane w obszarze nawigacji po pasku `UISearchDisplayController.DisplaysSearchBarInNavigationBar` właściwość jest ustawiona na wartość true. Po ustawieniu na wartość false — wartość domyślna — pasek nawigacyjny ukryty w przypadku kontrolera wyszukiwania jest wyświetlana.

Poniższy zrzut ekranu przedstawia pasek wyszukiwania w ramach `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "Przykładowe UISearchDisplayController")

### <a name="uitableview"></a>UITableView

Interfejsy API wokół `UITableView` to głównie bez zmian; jednak styl zmienił znacząco do nowego projektu interfejsu użytkownika. Hierarchia wewnętrzny widok również jest nieco inny. Ta zmiana nie wpłynie na większość aplikacji, ale jest to element wiedzieć.

#### <a name="grouped-table-style"></a>Styl pogrupowanej tabeli

Styl pogrupowanych zmienione został zaktualizowany o zawartości teraz rozszerzenie do krawędzi ekranu, jak pokazano poniżej:

 ![](ios7-ui-images/table1.png "Przykład pogrupowanej tabeli stylu")

#### <a name="separatorinset"></a>SeparatorInset

Separatory wierszy można teraz zastosowane przez ustawienie `UITableVIewCell.SeparatorInset` właściwości. Na przykład poniższy kod będzie służyć do wcięcie komórki od lewej krawędzi:

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

To daje w widoku tabeli z komórek z wcięciami, jak pokazano poniżej:

 ![](ios7-ui-images/separatorinset.png "Przykładowy UITableView SeparatorInset")

#### <a name="table-button-styles"></a>Style przycisku tabeli

Różne przyciski, używanych w widokach tabeli wszystkie zmieniły się. Poniższy zrzut ekranu przedstawia widok tabeli w trybie edycji:

 ![](ios7-ui-images/table2.png "Ten zrzut ekranu przedstawia widok tabeli w trybie edycji")

### <a name="additional-control-changes"></a>Dodatkową kontrolę zmian

Innych kontrolek UIKit uległy zmianie, w tym suwaki, przełączniki i steppery. Te zmiany są wyłącznie visual. Aby uzyskać więcej informacji można znaleźć firmy Apple [przewodnik dotyczący przejścia interfejsu użytkownika systemu iOS 7](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html).

## <a name="general-user-interface-changes"></a>Zmiany interfejsu użytkownika ogólne

Oprócz zmian w strukturze UIKit systemu iOS 7 wprowadzono szereg zmiany wizualne w interfejsie użytkownika, w tym:

-  Zawartość pełny ekran
-  Wygląd paska
-  Kolor odcienia

<a name="fullscreen" />

### <a name="full-screen-content"></a>Zawartość pełnego ekranu

System iOS 7 jest przeznaczona do umożliwiają aplikacji wykorzystać cały ekran. Kontrolerów widoku zostaną wyświetlone nakładające się pasek stanu i pasek nawigacyjny — Jeśli istnieje — w przeciwieństwie znajdujących się poniżej pasków stanu i nawigacji.

Podczas przygotowywania aplikacji dla systemu iOS 7 można wyrównać wizualnie za pomocą widoków podrzędnych *programu Interface Builder* lub *platformy Xamarin iOS Designer*. Umożliwia także jedną z nowych interfejsów API ułatwią obsługę zawartości w trybie pełnoekranowym programowo. Te interfejsy API są wprowadzane poniżej.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide i BottomLayoutGuide

 `TopLayoutGuide` i `BottomLayoutGuide` służyć jako odwołanie dla których widoki powinna rozpoczynać się ani kończyć, tak aby zawartość nie nakłada się przezroczyste `UIKit` paska, jak w poniższym przykładzie:

 [![](ios7-ui-images/clipped.png "Przykłady nie nakłada się przezroczyste pasek UIKit")](ios7-ui-images/clipped.png#lightbox)

Te interfejsy API mogą być używane do obliczania Przesunięcie widoku z góry lub u dołu ekranu i odpowiednio dostosować umieszczania zawartości:

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

Możemy użyć wartość obliczona powyżej, ustawić naszych `ImageView`przez przesunięcie od górnej krawędzi ekranu, aby cały obraz jest widoczny:

 [![](ios7-ui-images/good2.png "Przykład ImageViews przemieszczenia w górnej części ekranu")](ios7-ui-images/good2.png#lightbox)

Zapoznaj się [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) przykład roboczy.

Wartość przemieszczenia jest generowany dynamicznie po dodano Widok hierarchii, więc próby odczytania `TopLayoutGuide` i `BottomLayoutGuide` wartości w `ViewDidLoad` zwróci wartość 0. Oblicz wartość po widok został załadowany — na przykład w `ViewDidLayoutSubviews`.

> [!IMPORTANT]
> `TopLayoutGuide` i `BottomLayoutGuide` są przestarzałe w systemie iOS 11 na rzecz nowy układ obszaru bezpiecznego. Apple mają stwierdził, że za pomocą obszaru bezpiecznego jest zgodny z wersji dla systemu iOS w starszej niż iOS 11. Aby uzyskać więcej informacji, zobacz [aktualizowania aplikacji dla systemu iOS 11](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) przewodnik.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

Ten interfejs API Określa, które krawędzi widoku wydłużony do pełnego ekranu, niezależnie od tego paska przezroczystości. W systemie iOS 7 pasków nawigacji i paski narzędzi są wyświetlane na warstwę ponad kontrolera widoku — w przeciwieństwie do w systemie iOS w poprzednich wersjach, gdzie nie zajmowały tej samej przestrzeni. System iOS 7 aplikacji zdjęcia ilustruje domyślną `UIViewController.EdgesForExtendedLayout` wartość `UIRectEdge.All`. To ustawienie wypełnia wszystkich czterech krawędzi w widoku z zawartością, tworząc efekt nakładających się i pełnego ekranu:

 [![](ios7-ui-images/photos.png "Przykładowe EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

Naciskając obraz, który usuwa słupków i pokazuje obraz pełnoekranowy:

 [![](ios7-ui-images/photos2.png "EdgesForExtendedLayout z paskami usunięte")](ios7-ui-images/photos2.png#lightbox)

Ponieważ pełnoekranowym zawartość jest wartość domyślna, aplikacje skonfigurowane dla systemu iOS 6, będzie miał część widoku przycinana, tak jak w poniższym zrzucie ekranu:

 [![](ios7-ui-images/clipped.png "Aplikacje skonfigurowane dla systemu iOS 6, będzie miał część widoku przycinana, tak jak w tym zrzucie ekranu")](ios7-ui-images/clipped.png#lightbox)

Modyfikowanie `UIViewController.EdgesForExtendedLayout` właściwość dostosowuje tego zachowania. Firma Microsoft można określić, czy widok nie wypełniać żadnych krawędzi, dzięki naszym widoku pozwoli uniknąć wyświetlania zawartości w miejsce zajmowane przez nawigacji lub pasków narzędzi (na każdą orientację):

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

W aplikacji zobaczymy widoku zostaje przeniesiony, ponownie, aby cały obraz jest widoczny:

 [![](ios7-ui-images/good.png "Przykład: widoczne całego obrazu")](ios7-ui-images/good.png#lightbox)

Należy pamiętać, że podczas skutki `TopLayoutGuide/BottomLayoutGuide` i `EdgesForExtendedLayout` interfejsy API są podobne, są przeznaczone do wypełnienia inne cele. Zmiana `EdgesForExtendedLayout` ustawienia domyślnego może naprawić obciętych widoków w aplikacji przeznaczonych dla systemu iOS 6, ale projekt dobre dla systemu iOS 7 powinny przestrzegać estetycznych pełnego ekranu i podaj pełnego ekranu przeglądania środowisko, opierając się na `TopLayoutGuide` i `BottomLayoutGuide`aby prawidłowo umieść zawartość, która ma być zmieniane w wygodnym miejscu dla użytkownika.

Zapoznaj się [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) przykład roboczy.

### <a name="status-and-navigation-bars"></a>Stan i pasków nawigacji

Pasek stanu i pasków nawigacji są renderowane przy użyciu przezroczystości. Paski stanu są przezroczyste, a pasków narzędzi i pasków nawigacji półprzezroczyste i rozmyty przekazuj czują się głębokość w interfejsie użytkownika. Poniższy zrzut ekranu przedstawia ten Rozmycie i przejrzystości, gdzie kolor tła niebieski widok kolekcji pokazuje za pośrednictwem pasków stanu i nawigacji, co daje im światła wygląd niebieski:

 ![](ios7-ui-images/transparent-navbar.png "Stan próbki i rozmywając — pasek nawigacyjny")

#### <a name="status-bar-styles"></a>Style paska stanu

Oprócz Rozmycie i przejrzystości pierwszy plan pasek stanu może być jasny i ciemny (ciemny jest wartość domyślna). Styl paska stanu, można ustawić z kontrolera widoku. Kontroler widoku można również ustawić, czy pasek stanu jest ukrywane lub wyświetlane.

Na przykład, poniższy kod zastąpienia `PreferredStatusBarStyle` metody kontrolera widoku, aby wyświetlić światła pierwszego planu pasek stanu:

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

Powoduje to, że są wyświetlane na pasku stanu poniżej:

 ![](ios7-ui-images/light-status-bar.png "Przykładowy pasek stanu")

Aby ukryć pasek stanu z kontrolera widoku kodu, należy zastąpić `PrefersStatusBarHidden`, jak pokazano poniżej:

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

Spowoduje to ukrycie paska stanu:

 ![](ios7-ui-images/status-bar-hidden.png "Pasek stanu ukryte")

### <a name="tint-color"></a>Kolor odcienia

Przyciski są teraz wyświetlane jako tekst bez przeglądarki chrome. Kolor tekstu może być kontrolowana za pomocą nowego `TintColor` właściwość `UIView`. Ustawienie `TintColor` stosuje do hierarchii całego widoku dla widoku, który ustawia jego kolor. Aby zastosować `TintColor`w całej aplikacji, ustaw ją na `Window`. Możesz także wykryć, kiedy zmienia kolor odcienia za pośrednictwem `UIView.TintColorDidChange` metody.

Na przykład, poniższy zrzut ekranu przedstawia efekt zmiany kolor odcienia, w widoku z kontrolera nawigacji do purpurowy:

 ![](ios7-ui-images/tint-color.png "Kolor odcienia purpurowy w widoku kontrolery nawigacji")

Kolor odcienia mogą być stosowane do obrazów, a także gdy `RenderingMode` ustawiono `UIImageRenderingMode.AlwaysTemplate`.

> [!IMPORTANT]
> Kolor odcienia, nie można ustawić za pomocą `UIAppearance`.


### <a name="dynamic-type"></a>Typ dynamiczny

W systemie iOS 7 użytkownik może określić rozmiar tekstu w ustawieniach systemowych. O typie dynamicznym czcionka jest dostosowywany dynamicznie do wyglądają dobrze niezależnie od rozmiaru. `UIFont.PreferredFontForTextStyle` należy używać można uzyskać czcionki, które jest zoptymalizowane pod kątem rozmiar kontrolowanej przez użytkownika.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano zmiany w elementach interfejsu użytkownika w systemie iOS 7. Jego sprawdza, czy niektóre zmiany wprowadzone do widoków i kontrolek w strukturze UIKit, wyróżnianie zmiany wizualne także zmienia się na powiązanych interfejsów API. Na koniec wprowadza nowe interfejsy API do pracy z zawartością pełny ekran, nowa funkcja obsługi kolor odcienia i typu dynamicznego.

## <a name="related-links"></a>Linki pokrewne

- [ImageViewer (przykład)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
