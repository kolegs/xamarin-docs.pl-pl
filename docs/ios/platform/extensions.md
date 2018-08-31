---
title: Rozszerzenia w rozszerzeniu Xamarin.iOS systemu iOS
description: W tym dokumencie opisano rozszerzenia, które są elementy widget przedstawione przez system iOS w kontekście standardowych, takich jak w Centrum powiadomień. Omówiono w nim sposób tworzenia rozszerzenie i komunikować się z nią z poziomu aplikacji nadrzędnej.
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 831625c88bb3c0f8403d3b330054050488956cb6
ms.sourcegitcommit: f9fb316344fc4dce2fdce0dde3c5f4c2e4a42d08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43257901"
---
# <a name="ios-extensions-in-xamarinios"></a>rozszerzeń systemu iOS w rozszerzeniu Xamarin.iOS

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**Tworzenie rozszerzeń w systemie iOS, przez [Xamarin University](https://university.xamarin.com/)**

Rozszerzenia, jak wprowadzona w systemie iOS 8, są przeznaczone `UIViewControllers` , są uporządkowane według iOS wewnątrz kontekstów standardowego takie jak poziomu **Centrum powiadomień**, jak wyspecjalizowane typy klawiatury niestandardowej żądanej przez użytkownika do wykonania dane wejściowe lub w innych kontekstach, takich jak edytowanie fotografii, w którym rozszerzenia mogą udostępniać efekty specjalne filtry.

Wszystkie rozszerzenia są instalowane razem z aplikacją kontenera (za pomocą oba te elementy napisane przy użyciu interfejsów API Unified 64-bitowy) i są aktywowane z określonego punktu rozszerzenia w aplikacji hosta. A ponieważ będą one używane jako dodatki do istniejących funkcji systemu, muszą one być o wysokiej wydajności i zwarte i niezawodne. 

## <a name="extension-points"></a>Punkty rozszerzenia

|Typ|Opis|Punkt rozszerzenia|Host aplikacji|
|--- |--- |--- |--- |
|Akcja|Specjalnego edytora lub przeglądarka dla określonego typu nośnika|`com.apple.ui-services`|Wszystkie|
|Dostawca dokumentu|Zezwala aplikacji na korzystanie z magazynu zdalnego dokumentu|`com.apple.fileprovider-ui`|Aplikacje korzystające z [UIDocumentPickerViewController](https://developer.xamarin.com/api/type/UIKit.UIDocumentPickerViewController/)|
|Klawiatury|Alternatywne klawiatury|`com.apple.keyboard-service`|Wszystkie|
|Edytowania zdjęć|Manipulowanie zdjęć i edytowanie|`com.apple.photo-editing`|Edytor Photos.App|
|Udostępnij|Udostępnia dane z sieciami społecznościowymi, obsługi komunikatów usługi itd.|`com.apple.share-services`|Wszystkie|
|Dzisiaj|"Widżetami" wyświetlanych na ekranie dzisiaj lub Centrum powiadomień|`com.apple.widget-extensions`|Dnia dzisiejszego do Centrum powiadomień|

[Punkty rozszerzenia dodatkowe](~/ios/platform/introduction-to-ios10/index.md#app-extensions) zostały dodane w systemie iOS 10.

## <a name="limitations"></a>Ograniczenia

Rozszerzenia mają kilka ograniczeń, niektóre z nich są uniwersalne dla wszystkich typów (wystąpienia, a nie typu rozszerzenia dostępu do kamery lub mikrofon) podczas innych rodzajów rozszerzenia mogą mieć określone ograniczenia od ich użycia (na przykład niestandardowe klawiatury Nie można użyć do zabezpieczonych danych wpis pola, takie jak hasło). 

Uniwersalne ograniczenia są:

- [Kit kondycji](~/ios/platform/healthkit.md) i [interfejsu użytkownika zestawu zdarzeń](~/ios/platform/eventkit.md) platform nie są dostępne
- Nie można użyć rozszerzeń [rozszerzone tryby w tle](http://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- Rozszerzenia nie dostępu do urządzenia kamery lub mikrofon (chociaż mogą one uzyskiwać dostęp do istniejących plików multimedialnych)
- Rozszerzenia nie może odbierać porzucić Air danych (mimo że są one mogą przesyłać dane za pośrednictwem porzucić Air)
- [UIActionSheet](https://developer.xamarin.com/api/type/UIKit.UIActionSheet/) i [UIAlertView](https://developer.xamarin.com/api/type/UIKit.UIAlertView/) są niedostępne; należy użyć rozszerzenia [UIAlertController](https://developer.xamarin.com/api/type/UIKit.UIAlertController/)
- Kilku członków [UIApplication](https://developer.xamarin.com/api/type/UIKit.UIApplication/) są niedostępne: [UIApplication.SharedApplication](https://developer.xamarin.com/api/property/UIKit.UIApplication.SharedApplication/), `UIApplication.OpenURL`, `UIApplication.BeginIgnoringInteractionEvents` i `UIApplication.EndIgnoringInteractionEvents`
- iOS wymusza 16 MB limit użycia pamięci współczesnych rozszerzeń.
- Domyślnie rozszerzenia klawiatury nie mają dostępu do sieci. Ma to wpływ na debugowanie na urządzeniu (ograniczenia nie jest wymuszana w symulatorze), ponieważ platforma Xamarin.iOS wymaga dostępu do sieci dla debugowania do pracy. Istnieje możliwość dostępu do sieci żądania przez ustawienie `Requests Open Access` wartości w pliku Info.plist projektu do `Yes`. Zobacz firmy Apple [przewodnik klawiatury niestandardowe](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) Aby uzyskać więcej informacji o ograniczeniach rozszerzenie klawiatury.

Poszczególne ograniczeń, zobacz firmy Apple [Podręcznik programowania rozszerzenia aplikacji](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/).

## <a name="distributing-installing-and-running-extensions"></a>Dystrybucja, instalowania i uruchamiania rozszerzenia

Rozszerzenia są dystrybuowane za pomocą aplikacji kontenera, który z kolei jest przesłane i udostępnianych za pośrednictwem Store aplikacji. Rozszerzenia rozpowszechniać za pomocą aplikacji są zainstalowane w tym momencie, ale użytkownik musi jawnie włączyć każdego rozszerzenia. Różne typy rozszerzeń są włączone na różne sposoby; kilka wymaga od użytkownika przejść do **ustawienia** aplikację i włączyć je stamtąd. Podczas gdy inne są włączone w punkcie użycia, takich jak umożliwianie rozszerzenie udostępniania, wysyłając zdjęcie. 

Aplikacja, w którym jest używane rozszerzenie (gdy użytkownik napotka punktu rozszerzenia) jest określany jako **aplikacji hosta**, ponieważ jest aplikacja, który obsługuje rozszerzenia podczas wykonywania. Aplikacja, która instaluje rozszerzenie jest **aplikacji kontenera**, ponieważ jest aplikacja która zawiera rozszerzenie, podczas instalacji.  

Zazwyczaj aplikacji kontenera w tym artykule opisano rozszerzenie, a przeprowadzi użytkownika przez proces włączania go.

## <a name="extension-lifecycle"></a>Cykl życia rozszerzenia

Rozszerzenie może być bardzo proste, jako pojedynczy [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) lub bardziej złożonych rozszerzeń, które są dostępne na wielu ekranach interfejsu użytkownika. Gdy użytkownik napotka _punkty rozszerzenia_ (takie jak czas udostępniania obraz), będą mieć możliwość wyboru z rozszerzeń zarejestrowany dla tego punktu rozszerzenia. 

Jeśli zdecydują jednej z aplikacji przez rozszerzenia, jego `UIViewController` zostanie utworzone wystąpienie i rozpocząć normalne cyklu życia kontroler widoku. Jednak w przeciwieństwie do zwykła aplikacja, która jest wstrzymana, ale nie jest ogólnie zakończony, kiedy użytkownik kończy interakcji z nimi, rozszerzenia są załadowane, wykonywane i następnie przerwana wielokrotnie.

Rozszerzenia mogą komunikować się wraz ze swoimi aplikacjami hosta za pośrednictwem [NSExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) obiektu. Niektóre rozszerzenia mają operacje, które odbierają asynchronicznymi wywołaniami zwrotnymi z wynikami. Te wywołania zwrotne, które zostaną wykonane na wątkach w tle, a rozszerzenia należy wziąć pod uwagę; na przykład za pomocą [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/member/Foundation.NSObject.InvokeOnMainThread/) zaktualizować interfejs użytkownika. Zobacz [komunikacji z aplikacją hosta](#Communicating-with-the-Host-App) sekcji poniżej, aby uzyskać więcej informacji.

Domyślnie rozszerzenia i ich aplikacje kontenerów można komunikują się, pomimo instalowany razem. W niektórych przypadkach aplikacja kontenera jest zasadniczo pustego kontenera "wysyłki" którego cel jest obsługiwany po zainstalowaniu rozszerzenia. Jednak jeśli dyktują to okoliczności, aplikacji kontenera i rozszerzenia mogą udostępniać zasoby przy użyciu typowych obszaru. Ponadto **rozszerzenie już dzisiaj** może zażądać jej aplikacji kontenera, aby otworzyć adresu URL. To zachowanie jest wyświetlany w [ewolucji widżet odliczania](http://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo).

## <a name="creating-an-extension"></a>Tworzenie rozszerzenia

Rozszerzenia (i ich aplikacje kontenera) muszą być 64-bitowych plików binarnych a utworzone przy użyciu rozszerzenia Xamarin.iOS [Unified API](http://developer.xamarin.com/guides/cross-platform/macios/unified). Podczas tworzenia rozszerzenia, rozwiązanie będzie zawierać co najmniej dwa projekty: aplikacja kontenera i jednego projektu dla każdego kontenera rozszerzenie udostępnia. 

### <a name="container-app-project-requirements"></a>Wymagania dotyczące projektu aplikacji kontenera

Aplikacja kontenera, użyty do zainstalowania rozszerzenia ma następujące wymagania:

- Musisz go utrzymywać odwołania do projektu rozszerzenia.   
- Musi to być pełna aplikacja (musi być możliwe do uruchomienia i uruchomione pomyślnie) nawet wtedy, gdy go nie robi nic więcej niż zapewnia metodę, aby zainstalować rozszerzenie. 
- Musi mieć identyfikator pakietu, który stanowi podstawę do identyfikatora pakietu rozszerzenia projektu (zobacz sekcję poniżej, aby uzyskać więcej informacji).

### <a name="extension-project-requirements"></a>Wymagania dotyczące projektu rozszerzenia

Ponadto projekt rozszerzenia ma następujące wymagania:

- Musi zawierać identyfikator pakietu, który rozpoczyna się od identyfikatora pakietu aplikacji jego kontenera. Na przykład, jeśli identyfikator pakietu aplikacji kontenera `com.myCompany.ContainerApp`, identyfikator rozszerzenia może być `com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- Należy zdefiniować, klucz `NSExtensionPointIdentifier`, z odpowiednią wartością (takie jak `com.apple.widget-extension` dla **już dziś** widżet Centrum powiadomień), w jego `Info.plist` pliku.
- Musisz również zdefiniować *albo* `NSExtensionMainStoryboard` klucza lub `NSExtensionPrincipalClass` w jego `Info.plist` pliku odpowiednią wartość:
    - Użyj `NSExtensionMainStoryboard` klawisz, aby określić nazwę scenorysu, która przedstawia głównego interfejsu użytkownika dla rozszerzenia (minus `.storyboard`). Na przykład `Main` dla `Main.storyboard` pliku.
    - Użyj `NSExtensionPrincipalClass` klawisz, aby określić klasę, która będzie inicjowana, gdy rozszerzenie zostanie uruchomiony. Wartość musi odpowiadać **zarejestrować** wartość Twojej `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

Określone typy rozszerzeń mogą mieć dodatkowe wymagania. Na przykład **już dziś** lub **Centrum powiadomień** rozszerzenia główna klasa musi implementować [INCWidgetProviding](https://developer.xamarin.com/api/type/NotificationCenter.INCWidgetProviding/).

> [!IMPORTANT]
> W przypadku uruchomienia projektu przy użyciu jednej szablony rozszerzenia udostępniane przez program Visual Studio dla komputerów Mac, większość (Jeśli to nie wszystkie) te wymagania będą udostępniane i spełnione dla Ciebie automatycznie przez szablon.

## <a name="walkthrough"></a>Przewodnik 

W następującym przewodniku utworzysz przykładową **już dziś** widżet, który oblicza dzień i liczby dni pozostałej w tym roku:

[![](extensions-images/carpediemscreenshot-sm.png "Element widget dzisiaj przykładu, który oblicza dzień i liczby dni pozostałej w tym roku")](extensions-images/carpediemscreenshot.png#lightbox)

### <a name="creating-the-solution"></a>Tworzenie rozwiązania

Aby utworzyć wymagane rozwiązanie, wykonaj następujące czynności:

1. Najpierw utwórz nowe z systemami iOS, **aplikacja pojedynczego widoku** projektu, a następnie kliknij przycisk **dalej** przycisku: 

    [![](extensions-images/today01.png "Najpierw utwórz nowe z systemem iOS, aplikacja pojedynczego widoku projektu, a następnie kliknij przycisk Dalej")](extensions-images/today01.png#lightbox)
2. Wywołaj projektu `TodayContainer` i kliknij przycisk **dalej** przycisku: 

    [![](extensions-images/today02.png "Wywoływanie TodayContainer projektu i kliknij przycisk Dalej")](extensions-images/today02.png#lightbox)
3. Sprawdź **Nazwa projektu** i **SolutionName** i kliknij przycisk **Utwórz** przycisk, aby utworzyć rozwiązanie: 

    [![](extensions-images/today03.png "Sprawdź nazwę projektu i SolutionName, a następnie kliknij przycisk Utwórz, aby utworzyć rozwiązanie")](extensions-images/today03.png#lightbox)
4. Następnie w **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie i Dodaj nową **rozszerzenia systemu iOS** projekt z **rozszerzenie już dzisiaj** szablonu: 

    [![](extensions-images/today04.png "Następnie w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie i dodawanie nowego projektu rozszerzenia systemu iOS za pomocą szablonu rozszerzenie już dzisiaj")](extensions-images/today04.png#lightbox)
5. Wywołaj projektu `DaysRemaining` i kliknij przycisk **dalej** przycisku: 

    [![](extensions-images/today05.png "Wywoływanie DaysRemaining projektu i kliknij przycisk Dalej")](extensions-images/today05.png#lightbox)
6. Przejrzyj projekt i kliknij przycisk **Utwórz** przycisk, aby go utworzyć: 

    [![](extensions-images/today06.png "Przejrzyj projekt i kliknij przycisk Utwórz, aby go utworzyć")](extensions-images/today06.png#lightbox)

Wynikowy rozwiązania teraz powinien mieć dwa projekty, jak pokazano poniżej:

[![](extensions-images/today07.png "Wynikowy rozwiązanie powinno zostać udostępnionych dwa projekty, jak pokazano poniżej")](extensions-images/today07.png#lightbox)

### <a name="creating-the-extension-user-interface"></a>Tworzenie rozszerzenia interfejsu użytkownika

Następnie należy projektować interfejs dla Twojego **już dziś** elementu widget. Albo można to zrobić za pomocą scenorysu lub przez tworzenie interfejsu użytkownika w kodzie. Obie metody zostały omówione poniżej szczegółowo.

#### <a name="using-storyboards"></a>Za pomocą scenorysów

Tworzenie interfejsu użytkownika ze Scenorysem, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie projektu rozszerzenia `Main.storyboard` plik, aby otworzyć do edycji: 

    [![](extensions-images/today08.png "Kliknij dwukrotnie plik Main.storyboard projektów rozszerzenia, aby go otworzyć do edycji")](extensions-images/today08.png#lightbox)
2. Wybierz etykietę, która została automatycznie dodana do interfejsu użytkownika za pomocą szablonu i nadaj mu **nazwa** `TodayMessage` w **widżet** karcie **Eksplorator właściwości**: 

    [![](extensions-images/today09.png "Wybierz etykietę, która została automatycznie dodana do interfejsu użytkownika za pomocą szablonu i nadaj TodayMessage nazwę elementu Widget na karcie Eksplorator właściwości")](extensions-images/today09.png#lightbox)
3. Zapisz zmiany do scenorysu.

#### <a name="using-code"></a>Przy użyciu kodu

Aby utworzyć interfejs użytkownika w kodzie, wykonaj następujące czynności: 

1. W **Eksploratora rozwiązań**, wybierz opcję **DaysRemaining** projektu, Dodaj nową klasę i wywołać go `CodeBasedViewController`: 

    [![](extensions-images/code01.png "Aelect DaysRemaining projektu, Dodaj nową klasę i wywołać go CodeBasedViewController")](extensions-images/code01.png#lightbox)
2. Ponownie, **Eksploratora rozwiązań**, kliknij dwukrotnie ikonę rozszerzenia `Info.plist` plik, aby otworzyć do edycji: 

    [![](extensions-images/code02.png "Kliknij dwukrotnie plik Info.plist rozszerzenia, aby go otworzyć do edycji")](extensions-images/code02.png#lightbox)
3. Wybierz **widok źródła** (u dołu ekranu), a następnie otwórz `NSExtension` węzła: 

    [![](extensions-images/code03.png "Wybierz widok źródła w dolnej części ekranu, a następnie otwórz węzeł NSExtension")](extensions-images/code03.png#lightbox)
4. Usuń `NSExtensionMainStoryboard` klucza i Dodaj `NSExtensionPrincipalClass` wartością `CodeBasedViewController`: 

    [![](extensions-images/code04.png "Usuń klucz NSExtensionMainStoryboard i Dodaj NSExtensionPrincipalClass wartością CodeBasedViewController")](extensions-images/code04.png#lightbox)
5. Zapisz zmiany.

Następnie Edytuj `CodeBasedViewController.cs` plików i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
    [Register("CodeBasedViewController")]
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

Należy pamiętać, że `[Register("CodeBasedViewController")]` pasuje do wartości określonej dla `NSExtensionPrincipalClass` powyżej.

### <a name="coding-the-extension"></a>Kodowanie rozszerzenia

Za pomocą interfejsu użytkownika tworzone, Otwórz okno dialogowe `TodayViewController.cs` lub `CodeBasedViewController.cs` pliku (w oparciu o metodę używaną do tworzenia interfejsu użytkownika powyżej), zmień **ViewDidLoad** metody i przypisz ją wyglądać podobnie do następującego:

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

Jeśli przy użyciu kodu na podstawie metody interfejsu użytkownika, należy zastąpić `// Insert code to power extension here...` komentarz z nowego kodu z góry. Po wywoływania implementację podstawową i wstawianie etykietę dla wersji kodu na podstawie, ten kod wykonuje prostego obliczenia można pobrać dzień roku, a pozostałe są liczby dni. Następnie wyświetla komunikat w etykiecie (`TodayMessage`) utworzonego w projekcie interfejsu użytkownika.

Należy pamiętać, podobnie jak ten proces jest aby normalny proces pisania aplikacji. Rozszerzenie `UIViewController` ma taki sam cykl życia jako kontrolera widoku w aplikacji, z wyjątkiem rozszerzeń nie mają tryby w tle i nie zostają zawieszone po zakończeniu korzystania z nich. Zamiast tego rozszerzenia są wielokrotnie zainicjowany i ponownie alokowane zgodnie z potrzebami.

### <a name="creating-the-container-app-user-interface"></a>Tworzenie interfejsu użytkownika aplikacji kontenera

W ramach tego przewodnika aplikacji kontenera służy po prostu jako metodę dostarczania i zainstalować rozszerzenie i zawiera żadnych funkcji własne. Edytuj TodayContainer `Main.storyboard` pliku i Dodaj tekst definiowania funkcji rozszerzenia i sposobach jego instalacji:

[![](extensions-images/today10.png "Edytuj plik TodayContainers Main.storyboard i Dodaj tekst definiowania funkcji rozszerzeń i sposobach jego instalacji")](extensions-images/today10.png#lightbox)

Zapisz zmiany do scenorysu.

### <a name="testing-the-extension"></a>Testowanie rozszerzeń

Aby przetestować rozszerzenie w narzędziu iOS Simulator, uruchom **TodayContainer** aplikacji. Zostanie wyświetlony widok główny kontenera:

[![](extensions-images/run01.png "Zostanie wyświetlony widok główny kontenerów")](extensions-images/run01.png#lightbox)

Następnie trafień **Home** przycisku w symulatorze, przesuń palcem w dół od górnej krawędzi ekranu, aby otworzyć **Centrum powiadomień**, wybierz opcję **już dziś** kartę, a następnie kliknij przycisk **Edytuj** przycisku:

[![](extensions-images/run02.png "Przycisk Strona główna w symulatorze, przesuń palcem w dół od górnej krawędzi ekranu, aby otworzyć Centrum powiadomień, wybierz kartę dzisiaj i kliknij przycisk Edytuj")](extensions-images/run02.png#lightbox)

Dodaj **DaysRemaining** rozszerzenie **już dziś** wyświetlić, a następnie kliknij przycisk **gotowe** przycisku:

[![](extensions-images/run03.png "Dodaj rozszerzenie DaysRemaining do widoku dziś i kliknięcie przycisku Gotowe")](extensions-images/run03.png#lightbox)

Nowy widżet zostaną dodane do **już dziś** widoku i wyniki będą wyświetlane:

[![](extensions-images/run04.png "Nowy widżet zostanie dodany do widoku dziś, a wyniki zostaną wyświetlone")](extensions-images/run04.png#lightbox)

## <a name="communicating-with-the-host-app"></a>Podczas komunikacji z aplikacją hosta

Przykład dzisiaj te rozszerzenia, które zostały utworzone powyżej komunikują się ze swojej aplikacji hosta ( **już dziś** ekranu). Jeśli został on, użyje [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) właściwość `TodayViewController` lub `CodeBasedViewController` klasy. 

Dla rozszerzenia, które będą odbierać dane z aplikacji hosta, dane są w formie tablicy [NSExtensionItem](https://developer.xamarin.com/api/type/Foundation.NSExtensionItem/) obiektów przechowywanych w [InputItems](https://developer.xamarin.com/api/property/Foundation.NSExtensionContext.InputItems/) właściwość [ExtensionContext ](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) rozszerzenia `UIViewController`.

Inne rozszerzenie, np. rozszerzenia edytowania zdjęć, może rozróżnić użytkownika ukończenie lub anulowanie użycia. To są sygnalizowane wróć do aplikacji hosta za pośrednictwem [CompleteRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CompleteRequest/) i [CancelRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CancelRequest/) metody [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) właściwości.

Aby uzyskać więcej informacji, zobacz firmy Apple [Podręcznik programowania rozszerzenia aplikacji](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1).

## <a name="communicating-with-the-parent-app"></a>Podczas komunikacji z aplikacją nadrzędną

Grupa aplikacji umożliwia różnych aplikacji (lub aplikację i jego rozszerzenia) uzyskiwać dostęp do lokalizacji magazynu udostępnionego pliku. Grupy aplikacji mogą być używane dla danych, takich jak:

- [Apple Watch ustawienia](~/ios/watchos/app-fundamentals/settings.md).
- [Udostępnione NSUserDefaults](~/ios/app-fundamentals/user-defaults.md).
- [Pliki udostępnione](~/ios/watchos/app-fundamentals/parent-app.md#files).

Aby uzyskać więcej informacji, zobacz [grupy aplikacji](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) części naszych **Praca z funkcjami** dokumentacji.

## <a name="mobilecoreservices"></a>MobileCoreServices

Podczas pracy z rozszerzenia, użyj jednolitego typ identyfikator (identyfikator UTI) do tworzenia i modyfikowania danych, które są wymieniane między aplikacji, a inne aplikacje i/lub usługi.

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

Aby uzyskać więcej informacji, zobacz [grupy aplikacji](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) części naszych **Praca z funkcjami** dokumentacji.

## <a name="precautions-and-considerations"></a>Środki ostrożności i zagadnienia

Rozszerzenia mają znacznie mniej pamięci, aby je niż aplikacje. Należy się spodziewać, szybko i przy minimalnym nieautoryzowanego dostępu dla użytkownika i aplikacji, które są hostowane na platformie. Rozszerzenie powinny również zapewniają jednak funkcja charakterystyczne, przydatne do aplikacji za pomocą interfejsu użytkownika, marki, dzięki czemu użytkownik może zidentyfikować rozszerzenia dla deweloperów lub aplikacji kontenera, do których należą.

Biorąc pod uwagę te ściśle wymagane, należy wdrożyć tylko rozszerzeń, które została dokładnie przetestowana i zoptymalizowane pod kątem wydajności i użycia pamięci. 

## <a name="summary"></a>Podsumowanie

Ten dokument ma objęte rozszerzenia, czym są, typ punkty rozszerzenia i znane ograniczenia nałożone na rozszerzeniu przez system iOS. Omówiono jej tworzenia, dystrybucja, instalowania i uruchamiania rozszerzenia i cyklem życia rozszerzenia. Go podano instrukcje dotyczące tworzenia prostego **już dziś** widżet przedstawiający dwa sposoby tworzenia widżetu interfejsu użytkownika za pomocą scenorysów lub kodu. Jego pokazano, jak przetestować rozszerzenia w narzędziu iOS Simulator. Na koniec go pokrótce omówione podczas komunikowania się z aplikacji hosta i pewne środki ostrożności i zagadnienia, które należy podjąć podczas tworzenia rozszerzenia. 

## <a name="related-links"></a>Linki pokrewne

- [ContainerApp (przykład)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Tworzenie rozszerzenia w rozszerzeniu Xamarin.iOS (wideo)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
