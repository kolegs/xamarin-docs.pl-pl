---
title: Formanty
description: Xamarin.iOS przedstawia wszystkie obiekty interfejsu użytkownika natywnego dostarczonymi przez firmę Apple. Łatwo jest ona dodawana do aplikacji platformy Xamarin.iOS przy użyciu systemu iOS projektanta, konstruktora interfejsu w środowisku Xcode lub programowo. Niezależnie od wybranej metody Xamarin.iOS przedstawia wszystkie właściwości obiektu interfejsu użytkownika i metody w języku C#.
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 82b2998319d4e78ee4f58a6d024032a509724537
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="controls"></a>Formanty

_Xamarin.iOS przedstawia wszystkie obiekty interfejsu użytkownika natywnego dostarczonymi przez firmę Apple. Łatwo jest ona dodawana do aplikacji platformy Xamarin.iOS przy użyciu systemu iOS projektanta, konstruktora interfejsu w środowisku Xcode lub programowo. Niezależnie od wybranej metody Xamarin.iOS przedstawia wszystkie właściwości obiektu interfejsu użytkownika i metody w języku C#._

Ten dokument przedstawiono niektóre z najczęściej formantów interfejsu użytkownika dla systemu iOS i sposób ich użycia.

## <a name="alertsalertsmd"></a>[Alerty](alerts.md)

Począwszy od systemu iOS 8, UIAlertController ma UIActionSheet zastąpionego ukończone i UIAlertView zarówno których obecnie są przestarzałe.

## <a name="buttonsbuttonsmd"></a>[Przyciski](buttons.md)

Klasa UIButton jest używana do reprezentowania różnych inny styl przycisku na ekranach systemu iOS. W tej sekcji przedstawiono różne opcje do pracy z przycisków w systemie iOS.

## <a name="collection-viewsuicollectionviewmd"></a>[Widoki kolekcji](uicollectionview.md)

Kolekcja widoków, dostępne w `UICollectionView` klasy, są nowe pojęcie w systemie iOS 6, który wprowadzenie przedstawiające wielu elementów za pomocą układów. Wzorce udostępniania danych na `UICollectionView` do tworzenia elementów i interakcji z tych wykonaj elementów tego samego delegowania i danych źródłowych wzorce często używane w opracowywania aplikacji systemu iOS.

## <a name="imagesimagemd"></a>[Obrazy](image.md)

Dodawanie obrazów do aplikacji wymaga wykonania dwóch kroków: najpierw dodaj go do projektu. następnie dodaj formanty i kod umożliwiający ich wyświetlenie na ekranie. Zapoznaj się [Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md) artykuł bardziej szczegółowe zakres obsługi w Xamarin.iOS obrazu.

## <a name="manual-camera-controlsintro-to-manual-camera-controlsmd"></a>[Ręczne kontrolki aparatu](intro-to-manual-camera-controls.md)

Formanty aparatu ręczny, dostarczone przez `AVFoundation Framework` w systemie iOS 8, należy zezwolić aplikacji mobilnej wykonać pełną kontrolę nad aparatu fotograficznego urządzenia z systemem iOS. Ten poziom szczegółowych kontroli może służyć do tworzenia profesjonalnych kamer poziomu aplikacji i dostarczenie kompozycji wykonawcy przy Dostosowywanie parametry aparatu podczas wykonywania nadal obrazu lub wideo.

## <a name="mapsios-mapsindexmd"></a>[Mapy](ios-maps/index.md)

Mapy są typową funkcją w nowoczesnych przenośnych systemów operacyjnych. iOS oferuje obsługę mapowania natywnie przez strukturę zestawu mapy. Z zestawem mapy aplikacje można łatwo dodać rozbudowanych, interakcyjnych mapy. Mapy te można dostosować na różne sposoby, takie jak dodawanie adnotacji do oznaczania lokalizacji na mapie i nakładanie grafiki dowolnych kształtów. Zestaw mapy nawet ma wbudowaną obsługę przedstawiający bieżącej lokalizacji urządzenia.

## <a name="labelslabelsmd"></a>[Etykiety](labels.md)

`UILabel` Formant jest używany do wyświetlania jednego oraz wiele wierszy, odczytać tylko tekst.

## <a name="pickers-and-date-pickerspickermd"></a>[Selektorami i wyboru daty](picker.md)

Formant wyboru Wyświetla formantu "przypominającej koło", który zawiera listę o wartości z zaznaczonej wartości są wyróżnione. Użytkownicy Obróć koło z opcją, które mają.

Dla selektorami jej do ustawienia daty i / lub godzina sprawy jednego określonego użytkownika. Do zapewnienia tego Apple został utworzony niestandardowy podklasą klasy UIPickerView o nazwie UIDatePicker.

## <a name="progress-and-activity-indicatorsprogress-activity-indicatormd"></a>[Wskaźniki postępu i aktywności](progress-activity-indicator.md)

iOS udostępnia dwa sposoby wskazać w aplikacji: wskaźniki działania (łącznie z konkretną _sieci_ wskaźnika działania) i paski postępu.

## <a name="search-barssearchbarmd"></a>[Paski wyszukiwania](searchbar.md)

UISearchBar służy do wyszukiwania za pomocą listy wartości. 

## <a name="sliders-steppers-and-segmented-controlsslider-switch-segmented-controlsmd"></a>[Suwaki, steppery i Segmentowanych formantów](slider-switch-segmented-controls.md)

Kontrolka suwaka umożliwia proste wybór wartość liczbową w zakresie. iOS używa `UISwitch` zgodnie z wartością logiczną wejściowych, które mogą być reprezentowane przez przycisk radiowy na innych platformach. Formant segmentu jest zorganizowany sposób Zezwalaj użytkownikom na interakcję z mniejszą liczbą opcji.

## <a name="stack-viewuistackviewmd"></a>[Widok stosu](uistackview.md)

Formant widoku stosu (`UIStackView`) korzysta z możliwości automatycznego układu oraz klasy wielkości Zarządzanie stos widoków podrzędnych, poziomej lub pionowej, który dynamicznie odpowiada na ekran i orientację rozmiar urządzenia z systemem iOS.

## <a name="tables-and-cellstablesindexmd"></a>[Tabele i komórki](tables/index.md)

jego sekcji przedstawiono klasy służące do tworzenia i wyświetlania w tabelach, a następnie przykłady sposobu ich używania w platformy Xamarin.iOS. Obejmuje on przy użyciu domyślnego wyglądu w przypadku tabel, dostosowywanie układu Implementowanie edycji i zaprojektować wizualnie tabeli za pomocą platformy Xamarin iOS projektanta. Czasami ekran jest oczywiście listę wierszy (na przykład aplikacja utworów muzycznych) i innych wystąpień jest trudne do rozpoznania formantu table (na przykład edytowanie w aplikacji kontaktów lub konwersacji w aplikacji Messages).

## <a name="text-inputtext-inputmd"></a>[Wprowadzanie tekstu](text-input.md)

Akceptowanie danych wejściowych użytkownika tekst jest realizowane za pomocą `UITextField` na jeden wiersz danych wejściowych i UITextView edycji tekstu wielowierszowego. Można przeciągnięcie dowolnej z tych kontrolek do ekranu i kliknij dwukrotnie, aby ustawić tekst początkowej.

## <a name="tab-bars-and-tab-bar-controllerscreating-tabbed-applicationsmd"></a>[Paski kart i kontrolery paska kart](creating-tabbed-applications.md)

aplikacje systemu iOS przy użyciu interfejsu użytkownika karty nawigacji są tworzone za pomocą klasy UITabBarController. W tym artykule zostanie omówiony sposób z kartami aplikacji, która zawiera kilka widoków i kontrolerów. Następnie zajmiemy się jak załadować UITabBarController, gdy nie jest ona głównego kontrolera, takie jak po ekranie logowania.

## <a name="web-viewsuiwebviewmd"></a>[Widoki sieci Web](uiwebview.md)

W tym artykule, przeanalizujemy każdego z trzech widoków sieci Web dostarczonymi przez firmę Apple: `UIWebView`, `WKWebview`, i `SFSafariViewController`, ich podobieństwa i różnic oraz sposobu ich użycia.

## <a name="related-links"></a>Linki pokrewne

- [Formanty (przykład)](https://developer.xamarin.com/samples/Controls/)
