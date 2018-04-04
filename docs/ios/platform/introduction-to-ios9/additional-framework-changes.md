---
title: Zmiany struktury dodatkowe systemu iOS 9
description: W tym artykule omówiono dodatkowych, drobne zmiany lub ulepszenia istniejących struktur dla systemu iOS 9.
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 5561cccfd0968c309526aae1e5dc90831ca681b4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="additional-ios-9-frameworks-changes"></a>Zmiany struktury dodatkowe systemu iOS 9

_W tym artykule omówiono dodatkowych, drobne zmiany lub ulepszenia istniejących struktur dla systemu iOS 9._

[![](additional-framework-changes-images/ios9-sml.png "System iOS 9 Logo")](additional-framework-changes-images/ios9.png#lightbox)

Oprócz najważniejszych zmian w systemach iOS firmy Apple wprowadził zmiany i usprawnienia wielu istniejących struktur w systemie iOS 9.

## <a name="av-foundation-framework-additions"></a>Dodatki Framework AV Foundation

W ramach AV Foundation [AVSpeechSynthesisVoice](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechSynthesisVoice/) klasy można teraz określić głosu przez identyfikator, oprócz języka.

Na przykład następujący kod pobiera listę wszystkich dostępnych głosów:

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

Następnie można użyć jednego głosy z listy przez ustawienie go jako `Voice` właściwości wystąpienia [AVSpeachUtterance](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechUtterance/) klasy.

[AVQueuePlayer](https://developer.xamarin.com/api/type/AVFoundation.AVQueuePlayer/) klasa obsługuje teraz kombinację internet nośników przesyłania strumieniowego i opartych na plikach w kolejce. Poprzednie wersje można tylko kolejki nośnika tego samego typu.

Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania AVSpeechSynthesisVoice](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice).

## <a name="avkit-framework-additions"></a>Dodatki AVKit Framework

Aby pracować z nową funkcją obraz w obrazie (PIP), struktura AVKit obejmuje nowy `AVPictureInPictureController` i [AVPlayerViewController](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) klasy:

- **AVPictureInPictureController** — ta klasa umożliwia aplikacji systemu iOS 9 na odpowiedź użytkownika uruchamianie odtwarzania wideo w oknie narzędzia PIP przestawne, o zmiennym rozmiarze na urządzeniu iPad.
- **AVPlayerViewController** -zarządza `AVPlayer` kontrolera używany do wyświetlania zawartości wideo w oknie narzędzia PIP przestawne, o zmiennym rozmiarze na urządzeniu iPad.

Aby uzyskać więcej informacji, zobacz nasze [wielozadaniowości dla tabletu iPad](~/ios/platform/introduction-to-ios9/index.md#multitasking) dokumentacji i firmy Apple [odwołania AVPictureInPictureController](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) i [AVPlayerViewController odwołania](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>Wprowadzenie do usługi sieci Web CloudKit

CloudKit framework upraszcza tworzenie aplikacji tego dostępu do usługi iCloud. Obejmuje to pobieranie danych aplikacji i zasobów prawa, jak również mogą bezpiecznie przechowywać informacji o aplikacji. Ten zestaw zapewnia warstwę anonimowość zezwalając na dostęp do aplikacji przy użyciu ich identyfikatorów iCloud bez udostępniania informacji osobistych.

Nowy _usług sieci Web CloudKit_ framework zapewnia biblioteki JavaScript (CloudKit JS), która może być zawarte w witrynie sieci Web w celu zapewnienia dostępu do tych samych danych na podstawie CloudKit i zawartość jako aplikację platformy Xamarin.iOS.

> [!IMPORTANT]
> Przed dostęp, stanowią lub zaktualizować zawartość z CloudKit bazy danych przy użyciu CloudKit JS, należy wcześniej zdefiniowane schemat tej bazy danych.




Aby uzyskać więcej informacji można znaleźć w następujących dokumentach:

- [Wprowadzenie do CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) — nasze wprowadzenie do korzystania z CloudKit w aplikacji platformy Xamarin.iOS.
- [Szybki Start CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) — wprowadzenie CloudKit firmy Apple.
- [Odwołanie JS CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) — dokumentacja CloudKit JS firmy Apple.
- [Dokumentacja usług sieci Web CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240) — odwołanie firmy Apple, który opisuje interfejs HTTP do CloudKit.
- [Katalog CloudKit: Wprowadzenie do CloudKit (Cocoa i JavaScript)](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) -firmy Apple przykładowej aplikacji przy użyciu CloudKit i CloudKit JS.

## <a name="foundation-framework-additions"></a>Dodatki Framework Foundation

Apple obejmuje następujące zmiany w ramach Foundation z systemem iOS 9:

### <a name="changes-to-nsbundle"></a>Zmiany NSBundle

Wprowadzono następujące zmiany [NSBundle](https://developer.xamarin.com/api/type/Foundation.NSBundle/) klasy dla systemu iOS 9:

* `GetPreservationPriorityForTag (NSString tag)` -Pobiera bieżący priorytet zachowania zasobów z danym znacznikiem. Prawidłowe wartości należą do zakresu od `0.0` do `1.0`, zasobów o najniższym priorytecie, które ma zostać przeczyszczony najpierw.
* `SetPreservationPriorityForTag (double priority, NSSet tags)` — Ustawia bieżący priorytet zachowania zasobów przy użyciu tagów danym. Prawidłowe wartości należą do zakresu od `0.0` do `1.0`, zasobów o najniższym priorytecie, które ma zostać przeczyszczony najpierw.

Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania NSBundle](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle).

### <a name="changes-to-nsprocessinfo"></a>Zmiany NSProcessInfo

Wszystkich procesów działających na urządzeniu z systemem iOS ma jeden _procesu agenta informacji_ (PIA). Użyj [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) klasy, aby podać informacje o bieżącym zasilania PIA i kontroli i cieplna zarządzania dla danego procesu.

Na przykład aby kontrolować automatycznego zakończenia procesu można użyć poniższego kodu:

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania NSProcessInfo](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo).

### <a name="reacting-to-low-power-mode"></a>Reagowanie na tryb niskiego poboru energii

Użyj `LowPowerModeEnabled` właściwość [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) klasę, aby określić, czy na urządzeniu z systemem iOS, że aplikacja jest uruchomiona na włączono tryb niskiego poboru energii. Na przykład:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>HealthKit Framework zmiany

Apple uwzględnione następujące zmiany [HealthKit](https://developer.xamarin.com/api/namespace/HealthKit/) framework w systemie iOS 9:

- Obsługa śledzenia usunięcia wpisów w bazie danych HealthKit i zbiorczego usuwania. Zobacz firmy Apple [HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject), [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) i [odwołania do klasy HKHealthStore](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) Aby uzyskać więcej informacji.
- Nowe śledzenie kategorie i właściwości zostały dodane do `HKQuantityTypeIdentifier` klasy (takich jak `UVExposure`) i `HKCategoryTypeIdentifier` klasy (takie jak `OvulationTestResult`). Zobacz firmy Apple [odwołania stałe HealthKit](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html#//apple_ref/doc/uid/TP40014710) Aby uzyskać więcej informacji.

Zobacz nasze [wprowadzenie do HealthKit](~/ios/platform/healthkit.md) dokumentacji, aby uzyskać więcej informacji na temat pracy z HealthKit w Xamarin.iOS.

## <a name="local-authentication-framework-changes"></a>Zmiana Framework uwierzytelniania lokalnego

Apple uwzględnione następujące zmiany [uwierzytelniania lokalnych](https://developer.xamarin.com/api/namespace/LocalAuthentication/) framework w systemie iOS 9:

- Przy użyciu `EvaluateAccessControl` i `EvaluatePolicy` metody [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) klasy, można teraz prób ponownego użycia funkcji Touch ID jest zgodny z poprzednim odblokowanie powiodło się.
- Możliwość uzyskać listę aktualnie zarejestrowanych palców.
- Obsługa śledzenia palca zostanie dodany lub usunięty z uwierzytelniania.
- Możliwość używania _kontekst uwierzytelniania_ wywołania łańcucha kluczy i pomocy technicznej do oceny kontroli dostępu łańcucha kluczy na liście.
- Możliwość anulowania monit z kodu użytkownika.

Zobacz nasze [wprowadzenie do funkcji Touch ID](~/ios/platform/touchid.md) dokumentacji, aby uzyskać więcej informacji na temat pracy z funkcji Touch ID w platformy Xamarin.iOS.

### <a name="lacontext-changes"></a>LAContext zmiany

Wprowadzono następujące zmiany [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) klasy dla systemu iOS 9:

- **TouchIdAuthenticationMaximumAllowableReuseDuration** — zwraca maksymalną ilość czasu, który można ponownie użyć uwierzytelniania identyfikator touch.
- **EvaluatedPolicyDomainState** — pobiera lub ustawia stan obliczane zasad.
- **MaxBiometryFailures** -zostały amortyzacji w systemie iOS 9.
- **TouchIdAuthenticationAllowableReuseDuration** pobiera lub ustawia czas, który można ponownie użyć uwierzytelniania identyfikator touch.
- **EvaluateAccessControl** — asynchronicznie ocenia zasady uwierzytelniania.
- **Unieważnienie** — unieważnia uwierzytelniania identyfikator danego touch.
- **IsCredentialSet** — zwraca `true` Jeśli poświadczenia są obecnie skonfigurowane.
- **SetCredentialType** ustawia podany typ poświadczeń.

Zobacz firmy Apple [LAContext odwołania](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) więcej szczegółów.

## <a name="mapkit-framework-changes"></a>MapKit Framework zmiany

Apple uwzględnione następujące zmiany [MapKit](https://developer.xamarin.com/api/namespace/MapKit/) framework w systemie iOS 9:

- MapKit zapewnia wsparcie dla uruchamiania aplikacji mapy bezpośrednio do przesyłania instrukcjami i do wykonywania zapytań przesyłanych szacowany czas przybyciu (EAT) przy użyciu [MKLaunchOptions](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) i [MKDirections](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) klasy.
- Wyniki wyszukiwania zwrócony przez MapKit i [CLGeocoder](https://developer.xamarin.com/api/type/CoreLocation.CLGeocoder/) klasy można też podać wynik strefy czasowej.
- Teraz można całkowicie dostosować mapy adnotacji przedstawione za pomocą aplikacji systemu iOS `DetailCalloutAccessoryView` właściwość [MKAnnotationView](https://developer.xamarin.com/api/type/MapKit.MKAnnotationView/) klasy.

Można znaleźć pod adresem naszych [mapy iOS](~/ios/user-interface/controls/ios-maps/index.md) i [wskazówki - eksploracji adnotacje i nakładki na MapKit](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) dokumentacji, aby uzyskać więcej informacji na temat pracy z mapy i adnotacje w Xamarin.iOS i Apple [CLGeocoder odwołania](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) Aby uzyskać więcej informacji.

## <a name="passkit-framework-additions"></a>Dodatki PassKit Framework

Apple uwzględnione następujące zmiany [PassKit](https://developer.xamarin.com/api/namespace/PassKit/) framework w systemie iOS 9:

- Apple Pay teraz obsługuje zarówno debetowa magazynu i kart kredytowych, oraz kart odnajdowania. Zobacz **sieci płatności** sekcji firmy Apple [odwołania do klasy PKPaymentRequest](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) Aby uzyskać więcej informacji.
- Z bezpośrednio z poziomu aplikacji platformy Xamarin.iOS, można teraz dodawać sieci płatności i wystawców kart zapłacenia firmy Apple. Zobacz firmy Apple [odwołania do klasy PKAddPaymentPassViewController](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) więcej szczegółów.

Zobacz nasze [wprowadzenie do PassKit](~/ios/platform/passkit.md) dokumentacji, aby uzyskać więcej informacji na temat pracy z PassKit w Xamarin.iOS.

## <a name="safari-services-framework-additions"></a>Safari usług dodatków Framework

Apple uwzględnione następujące zmiany [usług Safari](https://developer.xamarin.com/api/namespace/SafariServices/) framework w systemie iOS 9:

- Teraz można użyć nowej [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) klasy do wyświetlenia zawartości sieci web w aplikacji platformy Xamarin.iOS. Umożliwia udostępnianie danych witryny sieci Web i pliki cookie z aplikacją Safari, a zawiera kilka funkcji w programie Safari (np. czytnik i automatyczne uzupełnianie). [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) funkcje **gotowe** przycisku, który zostanie zwrócona użytkowników do aplikacji, po zakończeniu przeglądania zawartości sieci web.

Ponieważ [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) klasy jest dostosowane do wyświetlania pojedynczej strony zawartości sieci web, należy rozważyć użycie go zamienić wszelkie [WKWebKit](https://developer.xamarin.com/api/type/WebKit.WKWebView/) lub [UIWebView](https://developer.xamarin.com/api/type/UIKit.UIWebView/)formantów w istniejącej aplikacji platformy Xamarin.iOS.

### <a name="displaying-a-website"></a>Wyświetlanie witryny sieci Web

Poniższy kod jest przykładem wywoływania [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) z wewnątrz innego kontrolera widoku:

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>UIKit Framework zmiany

Apple dołączył wiele udoskonaleń do kilku elementów [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) dla systemu iOS 9. W poniższych sekcjach zostaną opisano te zmiany.

### <a name="3d-touch-events"></a>Zdarzenia 3D Touch

Nowy system iOS 9 i telefonów iPhone 6s i telefonów iPhone 6s Plus, 3D Touch dodaje gestów poufnych wykorzystania aplikacji systemu iOS. W związku z tym, jeśli aplikacja działa w systemie iOS 9 (lub nowsza), a urządzenia z systemem iOS jest w stanie obsługi technologii 3D Touch, zmiany w wykorzystania spowoduje, że `TouchesMoved` się zdarzenia. 

Z powodu tej zmiany w zachowaniu aplikacji systemu iOS powinna być przygotowana do `TouchesMoved` zdarzenia wywoływanej częściej, nawet jeśli X / Y współrzędne nie uległy zmianie. 

Aby uzyskać więcej informacji, zobacz nasze [wprowadzenie do technologii 3D Touch](~/ios/platform/3d-touch.md) przewodnik.

### <a name="document-open-in-place-functionality"></a>Dokument otwarty w miejscu funkcji

Za pomocą `FinishedLaunching (application, launchOptions)` lub `WillFinishLaunching (Application, launchOptions)` metody [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) klasy, można teraz otworzyć dokument i zmodyfikować go w miejscu (a nie praca kopii).

Aby umożliwić obsługę nowych funkcji Otwórz w miejscu, należy dodać `LSSupportsOpeningDocumentsInPlace` klucza do aplikacji platformy Xamarin.iOS **Info.plist** pliku o wartości `YES`.

Zobacz firmy Apple [UIApplicationDelegate odwołania](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) więcej szczegółów.

### <a name="enhanced-touch-events"></a>Zdarzenia rozszerzonego Touch

Apple udostępnił kilka ulepszeń Touch zdarzeń w systemie iOS 9. Obejmują one możliwość użycia funkcji Touch prognozowania oraz uzyskać dostęp do pośredniego poprawki między odświeżeniami wyświetlania.

Zobacz firmy Apple [Przewodnik obsługi zdarzeń dla systemu iOS](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) więcej szczegółów.

### <a name="fetching-tailored-content"></a>Pobieranie zawartości dopasowane

Nowe `NSDataAsset` klasa pozwala na pobieranie zawartości dostosowane do pamięci i graficzne możliwości urządzenia z systemem iOS, które jest obecnie uruchomiony w aplikacji platformy Xamarin.iOS.

### <a name="new-layout-anchors"></a>Nowych kotwic układu

Nowy `NSLayoutAnchor` i `NSLayoutDimension` układ klasy zakotwiczenia pracować z nowych właściwości zakotwiczenia [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) klasy (takich jak `LeadingAnchor` i `WidthAnchor`) ułatwiają układu w systemie iOS 9.

Zobacz nasze [wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) dokumentacji, aby uzyskać więcej informacji na temat pracy z klasy rozmiar i AutoLayout w aplikacji platformy Xamarin.iOS i firmy Apple [NSLayoutAnchor odwołania](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor), [ Odwołanie NSLayoutDimension](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) i [odwołania UIView](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) Aby uzyskać więcej informacji.

### <a name="new-readable-content-margins"></a>Nowe marginesy zawartości do odczytu

Nowe `UILayoutGuide` klasa może być używana do udostępniania można odczytać zawartości marginesy i zdefiniuj regionów rysowania zawartości wewnątrz widoku. Zobacz firmy Apple [odwołania UILayoutGuide](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) Aby uzyskać więcej informacji.

### <a name="text-input-in-notifications-modifications"></a>Wprowadzanie tekstu w modyfikacje powiadomienia

[UIUserNotificationAction](https://developer.xamarin.com/api/type/UIKit.UIUserNotificationAction/) klasa ma nową `Behavior` właściwość, która może służyć do obsługi danych wejściowych tekstu z powiadomień.

### <a name="uiapplicationdelegate-changes"></a>UIApplicationDelegate zmiany

Gdy formalnie przestarzałe przez firmę Apple, zasugerować zastępuje wszystkie wywołania `FinishedLaunching (UIApplication application)` metody [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) klasy przy użyciu jednej `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` lub `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` metody.

Zobacz firmy Apple [UIApplicationDelegate odwołania](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) więcej szczegółów.

### <a name="uikit-dynamics-changes"></a>Zmiany Dynamics UIKit

Apple obejmuje następujące zmiany UIKit Dynamics systemu iOS 9:

- Dynamics teraz obsługuje kolizji z systemem innym niż prostokątne granice.
- Nowe, można dostosowywać `UIFieldBehavior` klasa jest używana do obsługi różnych typów pól.
- Załącznik dodatkowe typy zostały dodane do `UIAttachmentBehavior` klasy.

Zobacz firmy Apple [UIAttachment odwołania](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) więcej szczegółów.

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView i UIDatePicker zmiany

Przed systemu iOS 9 [UIPickerView](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) i [UIDatePicker](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/) formanty zostały bez zmienny rozmiar i będzie automatycznie zmieniać rozmiar aby wypełnić ich kontenera (zazwyczaj szerokość aplikacja była urządzenia z systemem iOS uruchomiona).

W systemie iOS 9 to automatyczną zmianę rozmiaru już nie występuje i formanty będzie odtwarzany na szerokość 320 punktu na wszystkich urządzeniach z systemem iOS, niezależnie od rozmiaru i orientacji ekranu.

Aby naprawić tę sytuację, umożliwia automatyczny układ i rozmiar klasy przypiąć szerokość formantu z krawędziami kontenera nadrzędnego (Widok), a następnie określ wymaganej wysokości. Zobacz nasze [wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) dokumentacji, aby uzyskać więcej informacji na temat pracy z klasy rozmiar i automatycznie Rozmieść elementy w aplikacji platformy Xamarin.iOS.

### <a name="new-uitextinputassistantitem-class"></a>Nowa klasa UITextInputAssistantItem

Użyj nowych `UITextInputAssistantItem` klasy do grup przycisków paska układu w _pasek skrótów_. Pasek skrótów jest nowy obszar dostępnej w klawiatura programowa zapewnienie skróty pisania.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Wprowadzenie do systemu iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [Nowości w systemie iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
