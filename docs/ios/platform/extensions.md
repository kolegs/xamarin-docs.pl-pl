---
title: Rozszerzenia systemu iOS
description: "Wprowadzone w systemie iOS 8, rozszerzenia są elementy widget, które są przedstawiane przez systemu iOS w kontekstach standardowe, takie jak w Centrum powiadomień, gdy użytkownik zażąda niestandardowych klawiatury lub są one fotografii edycji. Wszystkie rozszerzenia są instalowane w połączeniu z aplikacji kontenera i są aktywowani z określonego punktu rozszerzenia w aplikacji hosta."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f6e80b21c76089c0f3f7ac655584b7e18400307e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="ios-extensions"></a>Rozszerzenia systemu iOS

_Wprowadzone w systemie iOS 8, rozszerzenia są elementy widget, które są przedstawiane przez systemu iOS w kontekstach standardowe, takie jak w Centrum powiadomień, gdy użytkownik zażąda niestandardowych klawiatury lub są one fotografii edycji. Wszystkie rozszerzenia są instalowane w połączeniu z aplikacji kontenera i są aktywowani z określonego punktu rozszerzenia w aplikacji hosta._

Rozszerzenia, wprowadzoną w systemie iOS 8, są przystosowane w szczególności `UIViewControllers` które są przedstawiane przez iOS wewnątrz kontekstów standardowe takie jak w **Centrum powiadomień**, jak specjalizowany niestandardowych typów żądanej przez użytkownika do wykonania dane wejściowe lub innych kontekstach, takich jak edytowanie fotografii, w którym rozszerzenia można podać efekt specjalny filtry.

Wszystkie rozszerzenia są instalowane w połączeniu z aplikacji kontenera (z obu elementów napisane przy użyciu interfejsów API Unified 64-bitowego) i są aktywowani z określonego punktu rozszerzenia w aplikacji hosta. I ponieważ będą one używane jako dodatki do istniejących funkcji systemu, muszą być wysoka wydajność, krótki i niezawodne. 

W tym artykule omówiono następujące tematy:

- [Punkty rozszerzenia](#Extension-Points) -Wyświetla typ dostępnych punktów rozszerzenia i typ rozszerzenia, które mogą być tworzone dla każdego punktu.
- [Ograniczenia](#Limitations) -rozszerzenia mają różne ograniczenia, niektóre z nich uniwersalnych dla wszystkich typów, gdy inne rodzaje rozszerzenia może mieć pewne ograniczenia na ich użycie.
- [Przesyłanie, instalowania i uruchamiania rozszerzenia](#Distributing-Installing-and-Running-Extensions) -rozszerzenia są dystrybuowane z poziomu aplikacji kontenera, który z kolei jest przesłane i dystrybuowane za pośrednictwem sklepu z aplikacjami. Rozszerzenia rozpowszechnianej za pomocą aplikacji są zainstalowane w tym momencie, ale użytkownik musi jawnie włączyć każdego rozszerzenia. Różne typy rozszerzeń są włączone na różne sposoby.
- [Cykl życia rozszerzenia](#Extension-Lifecycle) — rozszerzenia `UIViewController` będzie można utworzyć wystąpienia i rozpocząć normalnego cyklu życia kontrolera widoku. Jednak w przeciwieństwie do normalnej aplikacji, rozszerzenia są załadowane, wykonywane, a następnie przerwane wielokrotnie.
- [Tworzenie rozszerzenia](#Creating-an-Extension) — podczas opracowywania rozszerzeń, rozwiązań będzie zawierać co najmniej dwa projekty: kontener aplikacji i jeden projektu dla każdego rozszerzenia kontener zawiera.
- [Wskazówki](#Walkthrough) — obejmuje tworzenie prostego **dzisiaj** rozszerzenia, która zapewnia interfejs użytkownika scenorysu przy użyciu elementu widget lub w kodzie, instalowania rozszerzenia i testowanie go w narzędziu iOS Simulator.
- [Podczas komunikacji z aplikacją hosta](#Communicating-with-the-Host-App) -krótko opisano komunikacji z aplikacją hosta z rozszerzeniem.
- [Podczas komunikacji z aplikacją nadrzędnej](#Communicating-with-the-Parent-App) -krótko opisano komunikacji z aplikacją nadrzędnego z rozszerzeniem.
- [Środki ostrożności i zagadnienia dotyczące](#Precautions-and-Considerations) -listy niektóre znać środki ostrożności i kwestii, które powinny brane pod uwagę podczas projektowania i wdrażania rozszerzenia.
 

<a name="Extension-Points" />

## <a name="extension-points"></a>Punkty rozszerzenia

Istnieje kilka typów rozszerzenia, które można utworzyć w systemie iOS 8 (i nowsze):

<table>
<colgroup>
<col />
<col />
<col />
</colgroup>

<thead>
<tr>
    <th >Typ</th>
    <th >Opis</th>
    <th >Punkt rozszerzenia</th>
    <th >Host aplikacji</th>
</tr>
</thead>

<tbody>
<tr>
    <td >Akcja</td>
    <td >Edytor specjalnych, lub podglądu dla określonego typu nośnika</td>
    <td ><code>com.apple.ui-services</code></td>
    <td >wszystkie</td>
</tr>
<tr>
    <td >Dostawca dokumentu</td>
    <td >Umożliwia aplikacji korzystanie z magazynu zdalnego dokumentu</td>
    <td ><code>com.apple.fileprovider-ui</code></td>
    <td >Aplikacje używające <a href="https://developer.xamarin.com/api/type/UIKit.UIDocumentPickerViewController/">UIDocumentPickerViewController</a></td>
</tr>
<tr>
    <td >Klawiatury</td>
    <td >Alternatywne klawiatury</td>
    <td ><code>com.apple.keyboard-service</code></td>
    <td >wszystkie</td>
</tr>
<tr>
    <td >Edytowanie fotografii</td>
    <td >Manipulowanie zdjęć i edytowania</td>
    <td ><code>com.apple.photo-editing</code></td>
    <td >Edytor Photos.App</td>
</tr>
<tr>
    <td >Udostępnij</td>
    <td >Udostępnia dane z sieciami społecznościowymi, wiadomości usługi itd.</td>
    <td ><code>com.apple.share-services</code></td>
    <td >wszystkie</td>
</tr>
<tr>
    <td >Dzisiaj</td>
    <td >"Elementy widget" wyświetlanych na ekranie dzisiaj lub Centrum powiadomień</td>
    <td ><code>com.apple.widget-extensions</code></td>
    <td >Centrum powiadomień i dziś</td>
</tr>
</tbody>
</table>

[Dodatkowe rozszerzenia punktów](~/ios/platform/introduction-to-ios10/index.md#app-extensions) zostały dodane w systemie iOS 10.

<a name="Limitations" />

## <a name="limitations"></a>Ograniczenia

Rozszerzenia mają różne ograniczenia, niektóre z nich uniwersalnych dla wszystkich typów (dla wystąpienia, a nie typu rozszerzenia może dostępu do kamery lub mikrofonów) podczas innych typów rozszerzenia może mieć pewne ograniczenia na ich użycia (na przykład niestandardowe klawiatury Nie można używać dla pól wpis zabezpieczyć dane takie jak hasła). 

Uniwersalny ograniczenia są:

- [Kit kondycji](~/ios/platform/healthkit.md) i [interfejsu użytkownika zestaw zdarzeń](~/ios/platform/eventkit.md) struktur nie są dostępne
- Nie można użyć rozszerzenia [rozszerzony tryby tła](http://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- Rozszerzenia nie może uzyskać dostępu kamer lub mikrofonów urządzenia (chociaż są one dostęp do istniejących plików multimedialnych)
- Rozszerzenia nie może odebrać porzucić lotniczego danych (chociaż są one przesyłania danych za pośrednictwem porzucić lotniczego)
- [UIActionSheet](https://developer.xamarin.com/api/type/UIKit.UIActionSheet/) i [UIAlertView](https://developer.xamarin.com/api/type/UIKit.UIAlertView/) są niedostępne; należy użyć rozszerzenia [UIAlertController](https://developer.xamarin.com/api/type/UIKit.UIAlertController/)
- Kilka elementów członkowskich z [UIApplication](https://developer.xamarin.com/api/type/UIKit.UIApplication/) są niedostępne: [UIApplication.SharedApplication](https://developer.xamarin.com/api/property/UIKit.UIApplication.SharedApplication/), `UIApplication.OpenURL`, `UIApplication.BeginIgnoringInteractionEvents` i `UIApplication.EndIgnoringInteractionEvents`
- iOS wymusza limit użycia pamięci 16 MB na współczesnych rozszerzenia.
- Domyślnie rozszerzenia klawiatury nie mają dostępu do sieci. Ma to wpływ na debugowanie na urządzeniu (ograniczenia nie jest wymuszana w symulatorze), ponieważ Xamarin.iOS wymaga dostępu do sieci w celu debugowania do pracy. Istnieje możliwość żądania dostępu do sieci przez ustawienie `Requests Open Access` wartości w pliku Info.plist projektu do `Yes`. Zobacz firmy Apple [przewodnik klawiatury niestandardowe](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) uzyskać więcej informacji o ograniczeniach rozszerzenia klawiatury.

Ograniczenia pojedynczych Zobacz firmy Apple [przewodnik programowania w języku rozszerzenie aplikacji](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/).

<a name="Distributing-Installing-and-Running-Extensions" />

## <a name="distributing-installing-and-running-extensions"></a>Dystrybucja, instalowania i uruchamiania rozszerzenia

Rozszerzenia są dystrybuowane z poziomu aplikacji kontenera, który z kolei jest przesłane i dystrybuowane za pośrednictwem sklepu z aplikacjami. Rozszerzenia rozpowszechnianej za pomocą aplikacji są zainstalowane w tym momencie, ale użytkownik musi jawnie włączyć każdego rozszerzenia. Różne typy rozszerzeń są włączone na różne sposoby; kilka wymagają od użytkownika przejść do **ustawienia** aplikacji i włączyć je stamtąd. Podczas gdy inne są włączone w punkcie użytkowania, np. włączenie rozszerzenie udostępnianie podczas wysyłania zdjęcie. 

Aplikacja, w którym rozszerzenia jest używana (w którym użytkownik napotka punkt rozszerzenia) jest określany jako **aplikacji hosta**, ponieważ jest ona aplikacji, który obsługuje rozszerzenia podczas wykonywania. Aplikacja, która instaluje rozszerzenie jest **aplikacji kontenera**, ponieważ jest aplikacja, która zawiera rozszerzenia, podczas instalacji.  

Zwykle aplikacja kontenera opisuje rozszerzenia i przeprowadzi użytkownika przez proces włączania go.

<a name="Extension-Lifecycle" />

## <a name="extension-lifecycle"></a>Cykl życia rozszerzenia

Rozszerzenie może być tak proste, jak pojedynczy [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) lub bardziej złożonych rozszerzeń, które stanowią kilku ekranach interfejsu użytkownika. Gdy użytkownik napotka _punktów rozszerzenia_ (takie jak kiedy udostępnianie obraz), mają one możliwość wyboru z rozszerzenia zarejestrowane dla tego punktu rozszerzenia. 

Po jednej z aplikacji przez rozszerzenia, jego `UIViewController` będzie można utworzyć wystąpienia i rozpocząć normalnego cyklu życia kontrolera widoku. Jednak w przeciwieństwie do normalnej aplikacji, które są wstrzymane, ale zazwyczaj zakończony, gdy użytkownik zakończy interakcji z nimi, rozszerzenia są załadowane, wykonywane, a następnie przerwane wielokrotnie.

Rozszerzenia mogą komunikować się z ich aplikacji hosta za pośrednictwem [NSExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) obiektu. Niektóre rozszerzenia zostały operacji otrzymujących asynchronicznego wywołania zwrotne z wynikami. Tych wywołań zwrotnych będą wykonywane na wątki w tle i rozszerzenia muszą to uwzględniać; na przykład za pomocą [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/member/Foundation.NSObject.InvokeOnMainThread/) Jeśli chcą, aby zaktualizować interfejsu użytkownika. Zobacz [komunikacji z aplikacją hosta](#Communicating-with-the-Host-App) sekcji poniżej, aby uzyskać więcej informacji.

Domyślnie rozszerzenia i ich aplikacji kontenera można nie komunikują się, niezależnie od instalowanego razem. W niektórych przypadkach aplikacja kontenera jest zasadniczo pusty kontener "wysyłki" których celem jest obsługiwana po zainstalowaniu rozszerzenia. Jednak jeśli jest wymagane przez okoliczności, aplikacji kontenera i rozszerzenia może udostępniać zasoby z obszaru wspólnej. Ponadto **dzisiaj rozszerzenia** może żądać jej aplikacji kontenera, aby otworzyć adresu URL. To zachowanie jest wyświetlany w obszarze [rozwijać Widget odliczania](http://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo).

<a name="Creating-an-Extension" />

## <a name="creating-an-extension"></a>Tworzenie rozszerzenia

Rozszerzenia (i ich aplikacji kontenera) muszą być 64-bitowe pliki binarne i utworzone przy użyciu platformy Xamarin.iOS [Unified API](http://developer.xamarin.com/guides/cross-platform/macios/unified). Podczas tworzenia rozszerzenia, rozwiązań będzie zawierać co najmniej dwa projekty: kontener aplikacji i jeden projektu dla każdego rozszerzenia kontener zawiera. 

<a name="Container-App-Project-Requirements" />

### <a name="container-app-project-requirements"></a>Wymagania dotyczące projektu aplikacji kontenera

Aplikacja kontenera używane do instalowania rozszerzenia ma następujące wymagania:

- Musi on Obsługa odwołanie do projektu rozszerzenia.   
- Musi to być pełnej aplikacji (musi mieć możliwość uruchamiania, a następnie uruchom pomyślnie) nawet wtedy, gdy go nie robi nic więcej niż umożliwiają instalowanie rozszerzenia. 
- Musi mieć identyfikator pakietu, który stanowi podstawę do identyfikator pakietu rozszerzenia projektu (zobacz poniższą sekcję, aby uzyskać więcej informacji).

<a name="Extension-Project-Requirements" />

### <a name="extension-project-requirements"></a>Wymagania dotyczące projektu rozszerzenia

Ponadto projekt rozszerzenia ma następujące wymagania:

- Musi mieć identyfikator pakietu, który rozpoczyna się od identyfikator pakietu aplikacji jego kontenera. Na przykład jeśli identyfikator pakietu aplikacji kontenera `com.myCompany.ContainerApp`, identyfikator rozszerzenia może być `com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- Należy zdefiniować klucz `NSExtensionPointIdentifier`, odpowiednią wartość (takich jak `com.apple.widget-extension` dla **dzisiaj** widget Centrum powiadomień), w jego `Info.plist` pliku.
- Musi także definiować *albo* `NSExtensionMainStoryboard` klucza lub `NSPrincipalClass` klucza w jego `Info.plist` plik z odpowiednią wartość:
    - Użyj `NSExtensionMainStoryboard` klawisz, aby określić nazwę scenorysu, który przedstawia główne interfejsu użytkownika dla rozszerzenia (minus `.storyboard`). Na przykład `Main` dla `Main.storyboard` pliku.
    - Użyj `NSPrincipalClass` klucza w celu określenia klasy, która zostanie zainicjowana po uruchomieniu rozszerzenia. Wartość musi odpowiadać **zarejestrować** wartości z `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

Określone typy rozszerzeń mogą mieć dodatkowe wymagania. Na przykład **dzisiaj** lub **Centrum powiadomień** główna klasa rozszerzenia musi implementować [INCWidgetProviding](https://developer.xamarin.com/api/type/NotificationCenter.INCWidgetProviding/).

> [!IMPORTANT]
> **Uwaga:** po uruchomieniu projektu przy użyciu jednej szablony rozszerzenia udostępniane przez program Visual Studio for Mac większości (Jeśli to nie wszystko) tych wymagań zostanie podana i spełnia zostanie automatycznie przez szablon.

<a name="Walkthrough" />

## <a name="walkthrough"></a>Wskazówki 

Poniższe wskazówki spowoduje utworzenie przykład **dzisiaj** widżetu oblicza dnia i liczbę dni w roku:

[ ![](extensions-images/carpediemscreenshot-sm.png "Przykład widżetu dzisiaj oblicza dnia i liczbę dni pozostałych w roku.")](extensions-images/carpediemscreenshot.png)

<a name="Creating-the-Solution" />

### <a name="creating-the-solution"></a>Tworzenie rozwiązania

Aby utworzyć wymagane rozwiązanie, wykonaj następujące czynności:

1. Najpierw utwórz nowy iOS, **jednej aplikacji widoku** projekt i kliknij przycisk **dalej** przycisk: 

    [ ![](extensions-images/today01.png "Najpierw należy utworzyć nowe iOS, jednej aplikacji w widoku projektu i kliknij przycisk Dalej")](extensions-images/today01.png)
2. Wywołaj projektu `TodayContainer` i kliknij przycisk **dalej** przycisk: 

    [ ![](extensions-images/today02.png "Wywołaj projektu TodayContainer i kliknij przycisk Dalej")](extensions-images/today02.png)
3. Sprawdź **Nazwa projektu** i **Nazwa rozwiązania** i kliknij przycisk **Utwórz** przycisk, aby utworzyć rozwiązania: 

    [ ![](extensions-images/today03.png "Sprawdź nazwę projektu i nazwa rozwiązania, a następnie kliknij przycisk Utwórz, aby utworzyć rozwiązanie")](extensions-images/today03.png)
4. Następnie w **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy w ramach rozwiązania i Dodaj nową **iOS rozszerzenia** projekt z **dzisiaj rozszerzenia** szablonu: 

    [ ![](extensions-images/today04.png "Następnie w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie i dodać nowy projekt rozszerzenia systemu iOS z szablonu dzisiaj rozszerzenia")](extensions-images/today04.png)
5. Wywołaj projektu `DaysRemaining` i kliknij przycisk **dalej** przycisk: 

    [ ![](extensions-images/today05.png "Wywołaj projektu DaysRemaining i kliknij przycisk Dalej")](extensions-images/today05.png)
6. Przejrzyj projekt i kliknij przycisk **Utwórz** przycisk, aby go utworzyć: 

    [ ![](extensions-images/today06.png "Przejrzyj projekt i kliknij przycisk Utwórz, aby go utworzyć")](extensions-images/today06.png)

Wynikowa rozwiązania teraz powinny mieć dwa projekty, jak pokazano poniżej:

[ ![](extensions-images/today07.png "Wynikowa rozwiązanie teraz musi mieć dwa projekty, jak pokazano poniżej")](extensions-images/today07.png)

<a name="Creating-the-Extension-User-Interface" />

### <a name="creating-the-extension-user-interface"></a>Tworzenie rozszerzenia interfejsu użytkownika

Następnie należy do projektowania dla interfejsu sieci **dzisiaj** elementu widget. Albo można to zrobić za pomocą scenorysu lub przez tworzenie interfejsu użytkownika w kodzie. Obie metody zostanie omówiona szczegółowo poniżej.

<a name="Using-Storyboards" />

#### <a name="using-storyboards"></a>Przy użyciu Scenorys

Aby utworzyć interfejsu użytkownika z scenorysu, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie projekt rozszerzenia `Main.storyboard` plik, aby otworzyć do edycji: 

    [ ![](extensions-images/today08.png "Kliknij dwukrotnie plik Main.storyboard projektów rozszerzenia, aby otworzyć do edycji")](extensions-images/today08.png)
2. Wybierz etykietę, która została automatycznie dodana do interfejsu użytkownika za pomocą szablonu i nadaj mu **nazwa** `TodayMessage` w **elementu Widget** karcie **Explorer właściwości**: 

    [ ![](extensions-images/today09.png "Wybierz etykietę, która została automatycznie dodana do interfejsu użytkownika za pomocą szablonu i nadaj mu TodayMessage nazwa na karcie Widget Eksploratora właściwości")](extensions-images/today09.png)
3. Zapisz zmiany do scenorysu.

<a name="Using-Code" />

#### <a name="using-code"></a>Przy użyciu kodu

Aby utworzyć interfejsu użytkownika w kodzie, wykonaj następujące czynności: 

1. W **Eksploratora rozwiązań**, wybierz pozycję **DaysRemaining** projekt, Dodaj nową klasę i nadaj mu `CodeBasedViewController`: 

    [ ![](extensions-images/code01.png "Aelect DaysRemaining projekt, Dodaj nową klasę i wywołać go CodeBasedViewController")](extensions-images/code01.png)
2. Ponownie, **Eksploratora rozwiązań**, kliknij dwukrotnie rozszerzenia `Info.plist` plik, aby otworzyć do edycji: 

    [ ![](extensions-images/code02.png "Kliknij dwukrotnie plik Info.plist rozszerzenia, aby otworzyć do edycji")](extensions-images/code02.png)
3. Wybierz **widoku źródła** (od dołu ekranu), a następnie otwórz `NSExtension` węzła: 

    [ ![](extensions-images/code03.png "Wybierz widok źródła w dolnej części ekranu i otwórz węzeł NSExtension")](extensions-images/code03.png)
4. Usuń `NSExtensionMainStoryboard` klucza i Dodaj `NSPrincipalClass` z wartością `CodeBasedViewController`: 

    [ ![](extensions-images/code04.png "Usuń klucz NSExtensionMainStoryboard i dodać NSPrincipalClass o wartości CodeBasedViewController")](extensions-images/code04.png)
5. Zapisz zmiany.

Następnie Edytuj `CodeBasedViewController.cs` pliku i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
    [Register("CodeBasedViewContoller")]
    public class CodeBasedViewController : UIViewController, INCWidgetProviding
    {
        public CodeBasedViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Add label to view
            var TodayMessage = new UILabel (new CGRect (0, 0, View.Frame.Width, View.Frame.Height)) {
                TextAlignment = UITextAlignment.Center
            };

            View.AddSubview (TodayMessage);
            
            // Insert code to power extension here...

        }
    }
}
```

Należy pamiętać, że `[Register("CodeBasedViewContoller")]` pasuje do wartości określonej dla `NSPrincipalClass` powyżej.

<a name="Coding-the-Extension" />

### <a name="coding-the-extension"></a>Kodowanie rozszerzenia

Przy użyciu interfejsu użytkownika utworzone, otwórz `TodayViewController.cs` lub `CodeBasedViewController.cs` pliku (na podstawie metody używanej do tworzenia interfejsu użytkownika powyżej), zmień **ViewDidLoad** — metoda i zapewnić ich wyglądać następująco:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Calculate the values
    var dayOfYear = DateTime.Now.DayOfYear;
    var leapYearExtra = DateTime.IsLeapYear (DateTime.Now.Year) ? 1 : 0;
    var daysRemaining = 365 + leapYearExtra - dayOfYear;

    // Display the message
    if (daysRemaining == 1) {
        TodayMessage.Text = String.Format ("Today is day {0}. There is one day remaining in the year.", dayOfYear);
    } else {
        TodayMessage.Text = String.Format ("Today is day {0}. There are {1} days remaining in the year.", dayOfYear, daysRemaining);
    }
}
```

Jeśli przy użyciu kodu na podstawie metody interfejsu użytkownika, Zastąp `// Insert code to power extension here...` komentarz o nowy kod z powyżej. Po wywołanie implementacji podstawowej (i wstawianie etykietę dla wersji kod na podstawie), ten kod jest proste obliczenia uzyskanie dzień roku, a pozostałe są liczby dni. A następnie wyświetla komunikat w etykiecie (`TodayMessage`) utworzonego w projekcie interfejsu użytkownika.

Należy zwrócić uwagę, jak podobne tego procesu jest normalny proces zapisywania aplikacji. Rozszerzenie `UIViewController` ma taki sam cykl życia jako kontrolera widoku w aplikacji, z wyjątkiem rozszerzenia nie ma trybów tła i nie są wstrzymywane, po zakończeniu korzystania z nich. Zamiast tego rozszerzenia są wielokrotnie zainicjowany i zwalnia przydzielone zgodnie z wymaganiami.

<a name="Creating-the-Container-App-User-Interface" />

### <a name="creating-the-container-app-user-interface"></a>Tworzenie interfejsu użytkownika aplikacji kontenera

W ramach tego przewodnika aplikacji kontenera jest używane jako metoda wysyłki i zainstaluj rozszerzenie i nie funkcje własnych. Edytuj TodayContainer `Main.storyboard` i Dodaj tekst definiowania funkcji rozszerzenia i sposobu jego instalacji:

[ ![](extensions-images/today10.png "Edytuj plik TodayContainers Main.storyboard i Dodaj tekst definiujący funkcję rozszerzeń i sposobu jego instalacji")](extensions-images/today10.png)

Zapisz zmiany do scenorysu.

<a name="Testing-the-Extension" />

### <a name="testing-the-extension"></a>Rozszerzenie testowania

Aby przetestować rozszerzenia w narzędziu iOS Simulator, uruchom **TodayContainer** aplikacji. Zostanie wyświetlony widok główny kontenera:

[ ![](extensions-images/run01.png "Zostanie wyświetlony widok główny kontenerów")](extensions-images/run01.png)

Następnie kliknij przycisk **Home** przycisk w symulatorze, przejdź w dół od górnej krawędzi ekranu, aby otworzyć **Centrum powiadomień**, wybierz pozycję **dzisiaj** i kliknij polecenie **Edytować** przycisk:

[ ![](extensions-images/run02.png "Kliknij przycisk Strona główna w symulatorze, przejdź w dół od górnej krawędzi ekranu, aby otworzyć Centrum powiadomień, wybierz kartę dzisiaj i kliknij przycisk Edytuj")](extensions-images/run02.png)

Dodaj **DaysRemaining** rozszerzenia **dzisiaj** wyświetlić, a następnie kliknij przycisk **gotowe** przycisk:

[ ![](extensions-images/run03.png "Dodaj rozszerzenie DaysRemaining do widoku dziś, a następnie kliknij przycisk Gotowe")](extensions-images/run03.png)

Nowy element widget zostaną dodane do **dzisiaj** widoku i wyniki będą wyświetlane:

[ ![](extensions-images/run04.png "Nowy element widget zostanie dodany do widoku dziś i wyświetli wyniki")](extensions-images/run04.png)

<a name="Communicating-with-the-Host-App" />

## <a name="communicating-with-the-host-app"></a>Podczas komunikacji z aplikacją hosta

Przykład dziś rozszerzenia utworzone powyżej nie komunikuje się z jej hosta aplikacji ( **dzisiaj** ekranu). Jeśli została ona, czy użyć [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) właściwość `TodayViewController` lub `CodeBasedViewController` klasy. 

Dla rozszerzeń, które będzie odbierać dane z aplikacji hosta, dane są w formie tablicy [NSExtensionItem](https://developer.xamarin.com/api/type/Foundation.NSExtensionItem/) obiekty przechowywane w [InputItems](https://developer.xamarin.com/api/property/Foundation.NSExtensionContext.InputItems/) właściwość [ExtensionContext ](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) rozszerzenia `UIViewController`.

Inne rozszerzenie, na przykład zdjęcie Edytowanie rozszerzenia może odróżnić użytkownika Kończenie lub anulowanie użycia. To są sygnalizowane powrót do aplikacji hosta za pomocą [CompleteRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CompleteRequest/) i [CancelRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CancelRequest/) metody [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) właściwości.

Aby uzyskać więcej informacji, zobacz firmy Apple [przewodnik programowania w języku rozszerzenie aplikacji](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1).

<a name="Communicating-with-the-Parent-App" />

## <a name="communicating-with-the-parent-app"></a>Podczas komunikacji z aplikacją nadrzędnego

Grupa aplikacji umożliwia różnych aplikacji (lub aplikacji i jej rozszerzenia) dostęp do lokalizacji magazynu udostępnionego pliku. Grupy aplikacji mogą być używane dla danych, takich jak:

- [Ustawienia Apple Watch](~/ios/watchos/app-fundamentals/settings.md).
- [Udostępnione NSUserDefaults](~/ios/app-fundamentals/user-defaults.md).
- [Udostępnione pliki](~/ios/watchos/app-fundamentals/parent-app.md#files).

Aby uzyskać więcej informacji, zobacz [grupy aplikacji](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) sekcji naszych **Praca z funkcjami** dokumentacji.

<a name="MobileCoreServices" />

## <a name="mobilecoreservices"></a>MobileCoreServices

Podczas pracy z rozszerzenia, należy użyć jednolitego identyfikatora typu (UTI), aby utworzyć i przetwarzanie danych, które są wymieniane między aplikacji, a inne aplikacje i/lub usług.

`MobileCoreServices.UTType` Klasy statycznej definiuje następujące właściwości pomocnika, które odnoszą się do firmy Apple `kUTType...` definicje:

- `kUTTypeAlembic` - `Alembic`
- `kUTTypeAliasFile` - `AliasFile`
- `kUTTypeAliasRecord` - `AliasRecord`
- `kUTTypeAppleICNS` - `AppleICNS`
- `kUTTypeAppleProtectedMPEG4Audio` - `AppleProtectedMPEG4Audio`
- `kUTTypeAppleProtectedMPEG4Video` - `AppleProtectedMPEG4Video`
- `kUTTypeAppleScript` - `AppleScript`
- `kUTTypeApplication` - `Application`
- `kUTTypeApplicationBundle` - `ApplicationBundle`
- `kUTTypeApplicationFile` - `ApplicationFile`
- `kUTTypeArchive` - `Archive`
- `kUTTypeAssemblyLanguageSource` - `AssemblyLanguageSource`
- `kUTTypeAudio` - `Audio`
- `kUTTypeAudioInterchangeFileFormat` - `AudioInterchangeFileFormat`
- `kUTTypeAudiovisualContent` - `AudiovisualContent`
- `kUTTypeAVIMovie` - `AVIMovie`
- `kUTTypeBinaryPropertyList` - `BinaryPropertyList`
- `kUTTypeBMP` - `BMP`
- `kUTTypeBookmark` - `Bookmark`
- `kUTTypeBundle` - `Bundle`
- `kUTTypeBzip2Archive` - `Bzip2Archive`
- `kUTTypeCalendarEvent` - `CalendarEvent`
- `kUTTypeCHeader` - `CHeader`
- `kUTTypeCommaSeparatedText` - `CommaSeparatedText`
- `kUTTypeCompositeContent` - `CompositeContent`
- `kUTTypeConformsToKey` - `ConformsToKey`
- `kUTTypeContact` - `Contact`
- `kUTTypeContent` - `Content`
- `kUTTypeCPlusPlusHeader` - `CPlusPlusHeader`
- `kUTTypeCPlusPlusSource` - `CPlusPlusSource`
- `kUTTypeCSource` - `CSource`
- `kUTTypeData` - `Database`
- `kUTTypeDelimitedText` - `DelimitedText`
- `kUTTypeDescriptionKey` - `DescriptionKey`
- `kUTTypeDirectory` - `Directory`
- `kUTTypeDiskImage` - `DiskImage`
- `kUTTypeElectronicPublication` - `ElectronicPublication`
- `kUTTypeEmailMessage` - `EmailMessage`
- `kUTTypeExecutable` - `Executable`
- `kUTExportedTypeDeclarationsKey` - `ExportedTypeDeclarationsKey`
- `kUTTypeFileURL` - `FileURL`
- `kUTTypeFlatRTFD` - `FlatRTFD`
- `kUTTypeFolder` - `Folder`
- `kUTTypeFont` - `Font`
- `kUTTypeFramework` - `Framework`
- `kUTTypeGIF` - `GIF`
- `kUTTypeGNUZipArchive` - `GNUZipArchive` 
- `kUTTypeHTML` - `HTML`
- `kUTTypeICO` - `ICO`
- `kUTTypeIconFileKey` - `IconFileKey`
- `kUTTypeIdentifierKey` - `IdentifierKey`
- `kUTTypeImage` - `Image`
- `kUTImportedTypeDeclarationsKey` - `ImportedTypeDeclarationsKey`
- `kUTTypeInkText` - `InkText`
- `kUTTypeInternetLocation` - `InternetLocation`
- `kUTTypeItem` - `Item`
- `kUTTypeJavaArchive` - `JavaArchive`
- `kUTTypeJavaClass` - `JavaClass`
- `kUTTypeJavaScript` - `JavaScript`
- `kUTTypeJavaSource` - `JavaSource`
- `kUTTypeJPEG` - `JPEG`
- `kUTTypeJPEG2000` - `JPEG2000`
- `kUTTypeJSON` - `JSON`
- `kUTType3dObject` - `k3dObject`
- `kUTTypeLivePhoto` - `LivePhoto`
- `kUTTypeLog` - `Log` 
- `kUTTypeM3UPlaylist` - `M3UPlaylist`
- `kUTTypeMessage` - `Message`
- `kUTTypeMIDIAudio` - `MIDIAudio`
- `kUTTypeMountPoint` - `MountPoint`
- `kUTTypeMovie` - `Movie`
- `kUTTypeMP3` - `MP3`
- `kUTTypeMPEG` - `MPEG`
- `kUTTypeMPEG2TransportStream` - `MPEG2TransportStream`
- `kUTTypeMPEG2Video` - `MPEG2Video`
- `kUTTypeMPEG4` - `MPEG4`
- `kUTTypeMPEG4Audio` - `MPEG4Audio`
- `kUTTypeObjectiveCPlusPlusSource` - `ObjectiveCPlusPlusSource`
- `kUTTypeObjectiveCSource` - `ObjectiveCSource`
- `kUTTypeOSAScript` - `OSAScript`
- `kUTTypeOSAScriptBundle` - `OSAScriptBundle`
- `kUTTypePackage` - `Package`
- `kUTTypePDF` - `PDF`
- `kUTTypePerlScript` - `PerlScript`
- `kUTTypePHPScript` - `PHPScript`
- `kUTTypePICT` - `PICT`
- `kUTTypePKCS12` - `PKCS12`
- `kUTTypePlainText` - `PlainText`
- `kUTTypePlaylist` - `Playlist`
- `kUTTypePluginBundle` - `PluginBundle`
- `kUTTypePNG` - `PNG`
- `kUTTypePolygon` - `Polygon`
- `kUTTypePresentation` - `Presentation`
- `kUTTypePropertyList` - `PropertyList`
- `kUTTypePythonScript` - `PythonScript`
- `kUTTypeQuickLookGenerator` - `QuickLookGenerator`
- `kUTTypeQuickTimeImage` - `QuickTimeImage`
- `kUTTypeQuickTimeMovie` - `QuickTimeMovie` 
- `kUTTypeRawImage` - `RawImage`
- `kUTTypeReferenceURLKey` - `ReferenceURLKey`
- `kUTTypeResolvable` - `Resolvable`
- `kUTTypeRTF` - `RTF`
- `kUTTypeRTFD` - `RTFD`
- `kUTTypeRubyScript` - `RubyScript`
- `kUTTypeScalableVectorGraphics` - `ScalableVectorGraphics`
- `kUTTypeScript` - `Script`
- `kUTTypeShellScript` - `ShellScript`
- `kUTTypeSourceCode` - `SourceCode`
- `kUTTypeSpotlightImporter` - `SpotlightImporter`
- `kUTTypeSpreadsheet` - `Spreadsheet`
- `kUTTypeStereolithography` - `Stereolithography`
- `kUTTypeSwiftSource` - `SwiftSource`
- `kUTTypeSymLink` - `SymLink`
- `kUTTypeSystemPreferencesPane` - `SystemPreferencesPane`
- `kUTTypeTabSeparatedText` - `TabSeparatedText`
- `kUTTagClassFilenameExtension` - `TagClassFilenameExtension`
- `kUTTagClassMIMEType` - `TagClassMIMEType`
- `kUTTypeTagSpecificationKey` - `TagSpecificationKey`
- `kUTTypeText` - `Text`
- `kUTType3DContent` - `ThreeDContent`
- `kUTTypeTIFF` - `TIFF`
- `kUTTypeToDoItem` - `ToDoItem`
- `kUTTypeTXNTextAndMultimediaData` - `TXNTextAndMultimediaData`
- `kUTTypeUniversalSceneDescription` - `UniversalSceneDescription`
- `kUTTypeUnixExecutable` - `UnixExecutable`
- `kUTTypeURL` - `URL` 
- `kUTTypeURLBookmarkData` - `URLBookmarkData`
- `kUTTypeUTF16ExternalPlainText` - `UTF16ExternalPlainText`
- `kUTTypeUTF16PlainText` - `UTF16PlainText`
- `kUTTypeUTF8PlainText` - `UTF8PlainText`
- `kUTTypeUTF8TabSeparatedText` - `UTF8TabSeparatedText`
- `kUTTypeVCard` - `VCard`
- `kUTTypeVersionKey` - `VersionKey` 
- `kUTTypeVideo` - `Video` 
- `kUTTypeVolume` - `Volume` 
- `kUTTypeWaveformAudio` - `WaveformAudio`
- `kUTTypeWebArchive` - `WebArchive`
- `kUTTypeWindowsExecutable` - `WindowsExecutable`
- `kUTTypeX509Certificate` - `X509Certificate`
- `kUTTypeXML` - `XML`
- `kUTTypeXMLPropertyList` - `XMLPropertyList`
- `kUTTypeXPCService` - `XPCService`
- `kUTTypeZipArchive` - `ZipArchive`

Zobacz poniższy przykład:

```csharp
using MobileCoreServices;
...

NSItemProvider itemProvider = new NSItemProvider ();
itemProvider.LoadItem(UTType.PropertyList ,null, (item, err) => {
    if (err == null) {
        NSDictionary results = (NSDictionary )item;
        NSString baseURI =
results.ObjectForKey("NSExtensionJavaScriptPreprocessingResultsKey");
    }
});
```

Aby uzyskać więcej informacji, zobacz [grupy aplikacji](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) sekcji naszych **Praca z funkcjami** dokumentacji.


<a name="Precautions-and-Considerations" />

## <a name="precautions-and-considerations"></a>Środki ostrożności i zagadnienia

Rozszerzenia mają znacznie mniej pamięci, aby je niż aplikacje. Oczekiwano wykonania szybko i z minimalnym nieautoryzowanego dostępu dla użytkownika i aplikacji, które znajdują się one w. Rozszerzenia należy również zapewnia jednak charakterystyczne, przydatna funkcja odbierającą aplikację z oznaczeniami firmowymi interfejsu użytkownika, który umożliwia użytkownikowi do identyfikowania developer rozszerzenia lub kontenera aplikacji, które należą do.

Biorąc pod uwagę te wymagania ścisłej, należy tylko wdrożyć rozszerzeń, które zostały dokładnie przetestowane i zoptymalizowany pod kątem wydajności i zmniejszenie zużycia pamięci. 

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

Ten dokument został objęty rozszerzeń, co to są, typ punktów rozszerzenia i znane ograniczenia narzucone na rozszerzenia przez system iOS. Omówione jego tworzenia, dystrybuowania, instalowania i uruchamiania rozszerzenia i cyklem życia rozszerzenia. Zapewniała Przewodnik tworzenia prostej **dzisiaj** przedstawiający dwa sposoby tworzenia widżetu interfejs użytkownika za pomocą Scenorys lub kod elementu widget. Go pokazano, jak przetestować rozszerzenia w symulatorze systemu iOS. Na koniec go krótko omówione komunikacji z hosta aplikacji i kilka środki ostrożności i zagadnienia, które należy podjąć w przypadku tworzenia rozszerzenia. 


## <a name="related-links"></a>Linki pokrewne

- [ContainerApp (przykład)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Tworzenie rozszerzeń w Xamarin.iOS (klip wideo)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
