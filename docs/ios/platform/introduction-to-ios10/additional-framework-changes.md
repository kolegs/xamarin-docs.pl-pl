---
title: Zmiany platformy iOS dodatkowe 10
description: W tym artykule omówiono dodatkowych, drobne zmiany lub ulepszenia istniejących struktur dla systemu iOS 10.
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 33852ef62bd00368ef6544d07e60dd6de4c3b7d3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="additional-ios-10-frameworks-changes"></a>Zmiany platformy iOS dodatkowe 10

_W tym artykule omówiono dodatkowych, drobne zmiany lub ulepszenia istniejących struktur dla systemu iOS 10._

## <a name="av-foundation-framework-additions"></a>Dodatki Framework AV Foundation

Struktura AVFoundation obejmuje następujące ulepszenia:

- W systemie iOS 10, deweloper nie ma już do implementacji innego [AVPlayerItem](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/) zachowania oparty na typie zawartości. Wystarczy ustawić `Rate` właściwości i AVFoundation określi, kiedy wystarczającej zawartości jest dostępny dla odtwarzania bez Gaśnięcie silnika.
- Nowy [AVCapturePhotoOutput](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureFileOutput/) klasy zastępuje przestarzałe `AVCaptureStillImageOutput` klasy i zapewnia ujednolicone metodę obsługi wszystkich przepływów pracy fotografii, zapewniając zaawansowane kontroli i monitorowania procesu przechwytywania i obsługuje nowe funkcje, takie jak zdjęcia na żywo i format RAW przechwytywania.
- Nowe `AVPlayerLooper` klasy ułatwia pętli dany element nośnika podczas odtwarzania.
- `AVAssetDownloadURLSession` Klasa umożliwia pobieranie i nowszym odtwarzanie FairPlay zaszyfrowanych strumienie HLS.
- Domyślnie [AVCaptureSession](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureSession/) klasy automatycznie obsługuje przechwytywania całej kolorów, całej przestrzeni w przypadku urządzeń sprzętowych go obsługuje. Zobacz firmy Apple [iOS informacje dotyczące zgodności urządzeń](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) więcej szczegółów.

## <a name="avkit-additions"></a>Dodatki AVKit

W ramach AVKit zawiera teraz nowe `UpdatesNowPlayingInfoCenter` właściwości, aby wskazać, kiedy powinny być zaktualizowane centrum informacyjnego gry teraz.

## <a name="core-data-enhancements"></a>Rozszerzenia danych podstawowych

iOS 10 obejmują następujące rozszerzenia framework Core danych:

- [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) obiektów z magazynów danych SQLite w obsługi trybu dziennika odnowy nowej generacji zapytania funkcji, których zarządzanego obiektu kontekstów (MOC) można przypiąć do wersji określonej bazy danych dla przyszłych pobierania i Błąd transakcji.
- Główny [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) obiektów obsługuje równoczesnych powodujący błąd i pobieranie bez serializacji.
- [NSPersistentStoreCoordinator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) klasy przechowuje pulę magazynów danych SQLite.
- Dodano kilka nowych metod wygody `NSManagedObject` ułatwianie wykonywania pobrań i utworzyć podklasy.
- Przy użyciu wysokiego poziomu `NSPersistenceContainer` do odwołania `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectModel/) i innych danych podstawowych konfiguracji zasobów.

Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania Framework danych podstawowych](https://developer.apple.com/reference/coredata).

## <a name="core-image-enhancements"></a>Podstawowe rozszerzenia obrazu

iOS 10 sprawia, że następujące rozszerzenia framework Core obrazu:

- Deweloper teraz przetwarzać obrazów w przestrzeń kolorów poza przestrzeń kolorów pracy kontekstu obrazu Core konwertując i przestrzeń kolorów przed i po zakończeniu przetwarzania.
- W przypadku urządzeń z systemem iOS używające A8 i A9 procesorów jest teraz obsługiwana formatu RAW. Obraz Core zapewnia wsparcie w dekodowania RAW obrazów z poziomu aparatu iSight wbudowanych lub z 3 aparatu strony. Użyj `FilterWithImageData` lub `FilterWithImageURL` metody [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) klasy do przetwarzania obrazów RAW.
- Wprowadzono kilka ulepszeń wydajności renderowania `UIImage` renderowania (jeśli jest obsługiwana przez obraz Core obrazu magazynów) w `UIImageView` obiektów. 
- `UIImage` oznakowane przestrzeni całej obiekty spowoduje, że w całej gamy kolorów w `UIImageView` obiektów na urządzeniach z systemem iOS, które obsługuje szeroką koloru.
- Kod jądra obrazu Core teraz mogą żądać formatów wyjściowych pikseli.
- `ImageWithExtent` Metody [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) klasa może być używana do przetwarzania niestandardowego operacja wstawiania do filtru. Obraz Core będzie wywołania danego wywołania zwrotnego między filtry podczas przetwarzania obrazu dla danych wyjściowych lub wyświetlić.

Ponadto dodano następujące nowe filtry Core obrazu:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>Dodatki ruchu Core

Nowość w systemach iOS 10, struktura ruchu Core obejmuje pedometer zdarzenia, które umożliwiają aplikacji na odbieranie szybkiego, w czasie rzeczywistym powiadomienia użytkownika wstrzymywanie i wznawianie śledzenia podczas pracy. Użyj [CMPedometer](https://developer.xamarin.com/api/type/CoreMotion.CMPedometer/) do rejestrowania zdarzeń pedometer pierwszym planie lub w tle.

## <a name="foundation-enhancements"></a>Ulepszenia Foundation

Struktury Foundation dla systemu iOS 10 wprowadzono następujące ulepszenia:

- Użyj nowych [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) klasy do formatowania zlokalizowanych pomiarów do wyświetlania dla użytkownika końcowego.
- Użyj nowych [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) klasie, aby obliczeń Interwał daty i czasu, takie jak czas trwania, porównanie odstępach czasu i testowania dla interwału przecięcia.
- Użyj nowych [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) klasy do konwersji między różne jednostki z miary (jednostka miary) lub wykonywania obliczeń na wartościach w różnych UOMs.

- Użyj nowych [NSUnit](https://developer.apple.com/reference/foundation/nsunit) i [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) klasy reprezentujący określonego UOMs.
- Dodano kilka nowych właściwości do [NSLocal](https://developer.apple.com/reference/foundation/nslocale) klasę, aby uzyskać informacje dotyczące lokalnej i format wyświetlania dostępne.

## <a name="gamekit-enhancements"></a>Ulepszenia GameKit

Struktura GameKit w systemie iOS 10 wprowadzono następujące ulepszenia:

- **Aplikacji Centrum gier** przestarzałe i usunięty z systemu iOS. Jeśli aplikacja korzysta z GameKit, jego _musi_ przedstawić własny interfejs wyświetlanie GameKit funkcji, np. tablice wyników. 
- Nowy typ konta tylko do usługi iCloud została zaimplementowana przez [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) klasy.
- Nowy [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) klasy zawiera rozwiązania uogólnionego zarządzania magazynu danych w aplikacji Game Center. `GKGameSession` przechowuje listę odtwarzaczy i aplikacja jest odpowiedzialny za wdrażanie jak i kiedy uczestnika daty jest przechowywane, pobrać lub wymieniane między uczestników. W wielu przypadkach gier sesji można zastąpić istniejących na podstawie Włącz dopasowań, dopasowań w czasie rzeczywistym lub trwałe gry Zapisz metody.

## <a name="gameplaykit-enhancements"></a>Ulepszenia GameplayKit

Struktura GameplayKit w systemie iOS 10 wprowadzono następujące ulepszenia:

- Użyj nowych [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) klasy zapewnienie wysokiej wydajności, wyszukiwanie naturalnego ścieżki.
- Generowanie procedurach szumu został dodany i może służyć do zwiększenia wzrostu w wyszukiwania naturalnego tekstury, Dodaj wzrostu do aparatu przepływu i zapewnić sformatowanego względem gier.
- Użyj przestrzennych partycjonowania do partycjonowania danych world gier dla wydajne wyszukiwanie.
- Nowe strategist Monte Carlo ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) został dodany do pełnego możliwe przeniesienie obliczeń.
- Dodano obsługę 3D do istniejącego agenta i zachowania znajdowanie ścieżki przy użyciu nowego [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) i [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) klasy.
- Nowy [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) i [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) upewnij klasy połączenie GameplayKit i SpriteKit łatwiejsze niż kiedykolwiek wcześniej.
- Dodano nowy interfejs API drzewa decyzyjnego ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) i [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) w celu zwiększenia AI tworzenia gier.

## <a name="healthkit-enhancements"></a>Ulepszenia HealthKit

Struktura HealthKit w systemie iOS 10 wprowadzono następujące ulepszenia:

- Dodano nowe klucze metadanych dla typów pogody (takich jak `HKWeatherConditionClear` i `HKWeatherConditionCloudy`) i typy ćwiczeń (takich jak `HKWorkoutActivityTypeFlexibility` i `HKWorkoutActivityTypeWheelchairRunPace`) zostały dodane.
- Nowe `HKCDADocument` klasy został dodany do reprezentowania klinicznych dokumentu architektura (dysk CD) sformatowany dokumentu.
- Użyj nowych [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) klasę, aby określić `ActivityType` i `LocationType` z ćwiczeń.
- Nowy [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) i `WheelchairUse` metody [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) klasy zostały dodane do pracy z dla niepełnosprawnych powiązane dane kondycji.

## <a name="homekit-enhancements"></a>Ulepszenia HomeKit

Struktura HomeKit w systemie iOS 10 wprowadzono następujące ulepszenia:

- Dodano nowe usługi oraz właściwości.
- IPad można skonfigurować do działania jako koncentrator HomeKit do zapewnienia zdalnego dostępu akcesoriów, uruchamiania wyzwalaczy automatyzacji i włączyć udostępniony uprawnienia użytkownika.
- Dodano obsługę dla aparatu i dzwonka Akcesoria.
- Dla Akcesoria podano więcej kontekstu i konfiguracji.

Zobacz nasze [wprowadzenie do HomeKit](~/ios/platform/homekit.md) dokumentacji, aby uzyskać więcej informacji.

## <a name="metal-enhancements"></a>Ulepszenia kompletnego stanu

Wprowadzono następujące ulepszenia metali framework w systemie iOS 10:

- Teraz można używać aplikacjach i grach 3W _tworzenia mozaiki_ wydajnie renderowanie geometrii za pośrednictwem procesora GPU i złożone sceny.
- Zapewnia precyzyjną kontrolę alokacji zasobów w celu zoptymalizowania wydajności systemu operacyjnego na podstawie aplikacji przy użyciu zasobów sterty i Memoryless celów renderowania.
- Specjalizacja funkcji umożliwia utworzenie kolekcji stopniu zoptymalizowany i funkcje kombinacji światła do sceny.

Aby dowiedzieć się więcej, zobacz firmy Apple [przewodnik programowania w języku systemu operacyjnego](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

## <a name="modelio-enhancements"></a>Ulepszenia ModelIO

Struktura ModelIO w systemie iOS 10 wprowadzono następujące ulepszenia:

 - Format pliku USD jest obecnie obsługiwany.
 - Podpisany dodano obsługę do pola odległość [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) klasy.
 - Użyj nowych `MDLLightProbeIrradianceDataSource` klasę, aby pomóc w jasny sondowania umieszczania.
 - Użyj nowych `MDLMaterialPropertyGraph` klasa umożliwia łatwą obsługę środowiska uruchomieniowego zmienia się do modeli.

## <a name="photos-enhancements"></a>Ulepszenia zdjęć

Framework zdjęć w systemie iOS 10 wprowadzono następujące ulepszenia:

- Użyj [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) i [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) klasy, aby skorzystać z nowych funkcji procesor obrazu Core przeprowadzenie edycji.
- Edycja fotografii na żywo jest teraz dostępna dla aplikacji, które obsługują framework zdjęć i rozszerzenia do edycji fotografii (wewnątrz zdjęć i aparatu do użytku aplikacji).
- Użyj nowych [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) klasę do edycji dotyczą zarówno wideo i nadal zawartości na żywo zdjęcia.

## <a name="replaykit-enhancements"></a>Ulepszenia ReplayKit

Struktura ReplayKit w systemie iOS 10 wprowadzono następujące ulepszenia:

- Użyj [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) i [RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) klasy do obsługi emisji z zarejestrowanych nośnika przez zewnętrzną 3 witryny.
- Rozszerzenia interfejsu użytkownika emisji i przekaż emisji są wymagane do obsługi emisji usług firm ReplayKit 3rd w aplikacji.

## <a name="scenekit-enhancements"></a>Ulepszenia SceneKit

Struktura SceneKit w systemie iOS 10 wprowadzono następujące ulepszenia:

- [SCNCamera](https://developer.xamarin.com/api/type/SceneKit.SCNCamera/) klasy zapewniają większą wzrostu przy użyciu funkcji HDR i efektów. Adaptacyjną narażenia umożliwia utworzenie automatyczne efekty lub użyj winietowanie, odpowiedniemu kolorów i kolor klasyfikacji, aby dodać efekty fillmatic do gry.
- SceneKit zawiera teraz nowy system fizycznie na podstawie renderowania (PBR) bardziej realistyczne wyniki z prostszych tworzenia zasobów.
- Użyj nowych [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) cieniowanie modelu do produktu szeroką gamę efektów cienia realistyczne podczas wymagające tylko trzech podstawowych właściwości (`Diffuse`, `Metalness` i `Roughness`).
- Ponieważ PBR cieniowania działa najlepiej oświetlenie oparte na środowisku, należy użyć `LightingEnvironment` właściwość do przypisania na podstawie obrazu oświetlenia do całej sceny.
- Użyj `IESProfileURL` właściwości do zaimportowania rzeczywistych osprzętu światła definiujące oświetlenia na podstawie rzeczywistych wartości takich jak natężenie (w lumenów) i kolor temperatury (w stopniach kelvin —).
- Zarówno PBR i HDR funkcje aparatu zapewniają lepsze wyniki niż techniki tradycyjnych renderowania i w związku z tym SceneKit wykonuje obecnie wszystkich obliczeń kolorów w liniowej przestrzeni kolorów (przy użyciu P3 przestrzeń kolorów na urządzenie kolor całej wyświetlana).
- Kolor teraz SceneKit dopasowuje wszystkie kolory przeczytaj informacje o profilu kolorów.
- SceneKit interpretuje wartości składnika kolorów w liniowej przestrzeni kolorów RGB dla wszystkich typów programów do cieniowania.
- Kolor zarówno liniowej renderowania miejsca i kolor całego można wyłączyć, określając `SCNDisableLinearSpaceRendering` i `SCNDisableWideGamut` kluczy w aplikacji `Info.plist`.
- Tworzenie Naczelne dowolnego wielokąta, (lub ładowane z pliki generowane programistycznie) do określenia geometrii z nowym [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) klasy.
- Ponieważ SceneKit odczytuje oraz dostosować informacje o profilu kolorów w obrazach tekstury, należy użyć katalogi zasobów dla wszystkich obrazów do upewnij się, że te informacje.

## <a name="spritekit-enhancements"></a>Ulepszenia SpriteKit

Struktura SpriteKit w systemie iOS 10 wprowadzono następujące ulepszenia:

- Niestandardowe programów do cieniowania może zapewnić atrybutów (`SKAttribute`) które mogą być konfigurowane niezależnie w każdym węźle, który używa programu do cieniowania, podając wartość atrybutu (`SKAttributeValue`).
- Tilemaps obsługują teraz kształtów kafelka kwadratowych, gniazdo sześciokątne i izometryczny 2D, 2,5 D i gry przewijanie strony za pomocą `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` i `SKTileSet` klasy.
- Użyj nowych `SKWarpGeometry` klasy stretch lub zakłócać [SKSpriteNode](https://developer.xamarin.com/api/type/SpriteKit.SKSpriteNode/) lub [SKEffectNode](https://developer.xamarin.com/api/type/SpriteKit.SKEffectNode/) renderowania. Nowy [SKAction](https://developer.xamarin.com/api/type/SpriteKit.SKAction/) klasa może być używana do animowania przejścia między efekty warp.
- [SKView](https://developer.xamarin.com/api/type/SpriteKit.SKView/) klasa udostępnia kilka nowych metod umożliwiają precyzyjną kontrolę nad warunkami i sposobami sceny jest renderowany.

## <a name="scrollview-enhancements"></a>Ulepszenia ScrollView

Formant ScrollView w systemie iOS 10.3 wprowadzono następujące ulepszenia:

- `UIScrollView` zawierają teraz `IndexDisplayMode` właściwość, aby kontrolować sposób indeks jest wyświetlany, gdy użytkownik jest przewijanie jako `UIScrollViewIndexDisplayMode` z:
    - `Automatic` -Wyświetlanie indeksu jest kontrolowany przez system operacyjny.
    - `AlwaysHidden` -Wyświetlanie indeksu zawsze jest ukryty.

Zobacz [iOSTenThree próbki](https://developer.xamarin.com/samples/monotouch/iOS10/iOSTenThree) do użycia.

## <a name="uikit-enhancements"></a>Ulepszenia UIKit

Struktura UIKit w systemie iOS 10 wprowadzono następujące ulepszenia:

- Nowy [UIPasteboard](https://developer.xamarin.com/api/type/UIKit.UIPasteboard/) interfejs API udostępnia nowe opcje (np. okres istnienia ograniczenia) i zostanie automatycznie zadeklarować zgodne typy zawartości dla typowych klasy.
- Nowa funkcja obsługi całkowicie interakcyjny, opartej na obiektach, przerywania animacji został dodany i mogą być połączone z gestów. Pleas Zobacz firmy Apple w [UIViewAnimating Protocol Reference](https://developer.apple.com/reference/uikit/uiviewanimating), [odwołania do klasy UIViewPropertyAnimator](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protocol Reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [Odwołania do klasy UICubicTimingParameters](https://developer.apple.com/reference/uikit/uicubictimingparameters) i [odwołania do klasy UISpringTimingParameter](https://developer.apple.com/reference/uikit/uispringtimingparameters) Aby uzyskać więcej informacji.
- Nowy `UIPreviewInteraction` i `UIPreviewInteractionDelegate` Zezwalaj aplikacji developer zapewnia niestandardowy interfejs dla operacji wglądu i pop.
- Nowe `UIAccessibilityCustomRotor` klasa umożliwia aplikacji udostępnia technologie pomocnicze, takie jak głos za pośrednictwem funkcji niestandardowych, specyficznej dla kontekstu.
- Użyj `UIAccessibilityIsAssistiveTouchRunning` i `UIAccessibilityAssistiveTouchStatusDidChangeNotification` symbole, aby określić, czy AssistiveTouch jest włączony.
- Użyj `UIAccessibilityHearingDevicePairedEar` i `UIAccessibilityHearingDevicePairedEarDidChangeNotification` symbole, aby pobrać stan dowolnego łączyć MIF aparaty słuchowe.
- Do obsługi typu dynamicznego w etykietach, korzystać z nowych pól tekstowych i pól tekstowych `PreferredFontForTextStyle` metody `UIFont` klasy.
- Podjęcie decyzji, należy zaktualizować element czcionki podczas urządzenia `UIContentSizeCategory` zmiany, użyj `AdjustsFontForContentSizeCategory` właściwość `UIContentSizeCategoryAdjusting` delegowanie.
- `OpenURL` Metody `UIApplication` klasy jest wywoływana asynchronicznie i obsługuje teraz obsługi zakończenia, która jest wywoływana po wykonaniu akcji Otwórz.
- Inicjowania udostępniania CloudKit i zmodyfikuj jego właściwości za pomocą nowej `UICloudSharingController` i `UICloudSharingControllerDelegate` klasy.
- Zalety prefetched komórek w celu poprawy wydajności przewijania `UICollectionViews` z nowym `UICollectionViewDataSourcePrefetching` delegowanie.
- Deweloper może teraz sterować wyglądem wskaźnika dla elementów paska kartę (takich jak kolor tła i tekstu).
- Odśwież kontrolki jest teraz obsługiwana we wszystkich przewijania widoku i przewijania widoku podklasy (takie jak `UICollectionView`).

## <a name="webkit-enhancements"></a>Ulepszenia WebKit

Framework WebKit w systemie iOS 10 wprowadzono następujące ulepszenia:

- Peek i obsługa pop został dodany do `WKWebView` klasy. Użyj `ShouldPreviewElement` metodę, aby określić, czy podgląd powinien być wyświetlany widok danej sieci web.


## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Nowości w systemie iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
