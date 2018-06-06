---
title: Zmiany struktury dodatkowe systemu tvOS 10
description: W tym dokumencie opisano drobne zmiany i ulepszeń do istniejących struktur w systemie iOS 10. Zawarto informacje aktualizacje AVFoundation, AVKit, podstawowe dane, Core grafiki, Foundation, GameKit, GameplayKit i więcej.
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 326fb6a23048ba3d3e1d33f42c8da2fff8544c25
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788878"
---
# <a name="additional-tvos-10-frameworks-changes"></a>Zmiany struktury dodatkowe systemu tvOS 10

Oprócz najważniejszych zmian do systemu tvOS Apple wprowadził zmiany i usprawnienia wielu istniejących struktur w systemu tvOS 10.

<a name="AV-Foundation-Framework" />

## <a name="avfoundation-framework-additions"></a>Dodatki AVFoundation Framework

Struktura AVFoundation obejmuje następujące ulepszenia:

 - W systemu tvOS 10, aplikacja korzysta już inną [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) zachowania oparty na typie zawartości. Wystarczy ustawić `Rate` właściwości i AVFoundation określi, kiedy wystarczającej zawartości jest dostępny dla odtwarzania bez Gaśnięcie silnika.
 - Nowe `AVPlayerLooper` klasy ułatwia pętli dany element nośnika podczas odtwarzania.

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>Ulepszenia AVKit Framework

Struktura AVKit obejmuje następujące ulepszenia:

 - Aplikacja ma teraz kontrolę nad pomijanie zachowanie [AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller) , pomijanie gestu może przejść do następnego elementu listy odtwarzania lub wcześniejszym w ramach bieżącego elementu.

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>Rozszerzenia danych podstawowych

10 systemu tvOS obejmuje następujące rozszerzenia framework Core danych:

 - Główny [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) obiektów obsługuje równoczesnych powodujący błąd i pobieranie bez serializacji.
 - [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) klasy przechowuje pulę magazynów danych SQLite.
 - [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) obiektów z magazynów danych SQLite w obsługi trybu dziennika odnowy nowej generacji zapytania funkcji, których zarządzanego obiektu kontekstów (MOC) można przypiąć do wersji określonej bazy danych dla przyszłych pobierania i Błąd transakcji.
 - Przy użyciu wysokiego poziomu `NSPersistenceContainer` do odwołania `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) i innych danych podstawowych konfiguracji zasobów.
 - Dodano kilka nowych metod wygody `NSManagedObject` ułatwianie wykonywania pobrań i utworzyć podklasy.

Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania Framework danych podstawowych](https://developer.apple.com/reference/coredata).

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>Podstawowe rozszerzenia grafiki

10 systemu tvOS obejmuje następujące rozszerzenia Core framework grafiki:

 - Nowy [CGColorConverterRef](https://developer.apple.com/reference/coregraphics/cgcolorconverterref) klasa może być używana do wykonywania szereg konwersji kolorów.

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>Podstawowe rozszerzenia obrazu

10 systemu tvOS sprawia, że następujące rozszerzenia framework Core obrazu:

 - `ImageWithExtent` Metody [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) klasa może być używana do przetwarzania niestandardowego operacja wstawiania do filtru. Obraz Core będzie wywołania danego wywołania zwrotnego między filtry podczas przetwarzania obrazu dla danych wyjściowych lub wyświetlić.
 - Aplikacja teraz przetwarzać obrazów w przestrzeń kolorów poza przestrzeń kolorów pracy kontekstu obrazu Core konwertując i przestrzeń kolorów przed i po zakończeniu przetwarzania.
 - Wprowadzono kilka ulepszeń wydajności renderowania `UIImage` renderowania (jeśli jest obsługiwana przez obraz Core obrazu magazynów) w `UIImageView` obiektów. 
 - `UIImage` oznakowane przestrzeni całej obiekty spowoduje, że w całej gamy kolorów w `UIImageView` obiektów na urządzeniach z systemem iOS, które obsługuje szeroką koloru.
 - Kod jądra obrazu Core teraz mogą żądać formatów wyjściowych pikseli.

Ponadto dodano następujące nowe filtry Core obrazu:

 - `CINinePartTiled`
 - `CINinePartStretched`
 - `CIHueSaturationValueGradient`
 - `CIEdgePreserveUpsampleFilter`
 - `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Ulepszenia Foundation

Struktury Foundation dla systemu tvOS 10 wprowadzono następujące ulepszenia:

 - Użyj nowych [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) klasie, aby obliczeń Interwał daty i czasu, takie jak czas trwania, porównanie odstępach czasu i testowania dla interwału przecięcia.
 - Dodano kilka nowych właściwości do [NSLocal](https://developer.apple.com/reference/foundation/nslocale) klasę, aby uzyskać informacje dotyczące lokalnej i format wyświetlania dostępne.
 - Użyj nowych [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) klasy do konwersji między różne jednostki z miary (jednostka miary) lub wykonywania obliczeń na wartościach w różnych UOMs.
 - Użyj nowych [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) klasy do formatowania zlokalizowanych pomiarów do wyświetlania dla użytkownika końcowego.
 - Użyj nowych [NSUnit](https://developer.apple.com/reference/foundation/nsunit) i [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) klasy reprezentujący określonego UOMs.

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>Ulepszenia GameKit

Struktura GameKit w systemu tvOS 10 wprowadzono następujące ulepszenia:

 - Nowy typ konta tylko do usługi iCloud została zaimplementowana przez [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) klasy.
 - Nowy [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) klasy zawiera rozwiązania uogólnionego zarządzania magazynu danych w aplikacji Game Center. `GKGameSession` przechowuje listę odtwarzaczy i aplikacja jest odpowiedzialny formularza wykonania, kiedy jest przechowywana data uczestnika, pobrać lub wymieniane między graczy. W wielu przypadkach gier sesji można zastąpić istniejących na podstawie Włącz dopasowań, dopasowań w czasie rzeczywistym lub trwałe gry Zapisz metody.

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>Ulepszenia GameplayKit

Struktura GameplayKit w systemu tvOS 10 wprowadzono następujące ulepszenia:

 - Generowanie procedurach szumu został dodany i może służyć do zwiększenia wzrostu w wyszukiwania naturalnego tekstury, Dodaj wzrostu do aparatu przepływu i zapewnić sformatowanego względem gier.
 - Użyj przestrzennych partycjonowania do partycjonowania danych world gier dla wydajne wyszukiwanie.
 - Nowe strategist Monte Carlo ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) został dodany do pełnego możliwe przeniesienie obliczeń.
 - Dodano nowy interfejs API drzewa decyzyjnego ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) i [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) w celu zwiększenia AI tworzenia gier.
 - Dodano obsługę 3D do istniejącego agenta i zachowania znajdowanie ścieżki przy użyciu nowego [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) i [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) klasy.
 - Użyj nowych [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) klasy zapewnienie wysokiej wydajności, wyszukiwanie naturalnego ścieżki.
 - Nowy [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) i [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) upewnij klasy połączenie GameplayKit i SpriteKit łatwiejsze niż kiedykolwiek wcześniej.

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>Ulepszenia kompletnego stanu

Wprowadzono następujące ulepszenia metali framework w systemu tvOS 10:

 - Teraz można używać aplikacjach i grach 3W _tworzenia mozaiki_ wydajnie renderowanie geometrii za pośrednictwem procesora GPU i złożone sceny.
 - Specjalizacja funkcji umożliwia utworzenie kolekcji stopniu zoptymalizowany i funkcje kombinacji światła do sceny.
 - Zapewnia precyzyjną kontrolę alokacji zasobów w celu zoptymalizowania wydajności systemu operacyjnego na podstawie aplikacji przy użyciu zasobów sterty i Memoryless celów renderowania.

Aby dowiedzieć się więcej, zobacz firmy Apple [przewodnik programowania w języku systemu operacyjnego](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>Ulepszenia programów do cieniowania kompletnego stanu wydajności

Struktura programów do cieniowania wydajności systemu operacyjnego w systemu tvOS 10 wprowadzono następujące ulepszenia:

 - Wiele nowych jądra zostały dodane do framework wydajności systemu operacyjnego programów do cieniowania umożliwi aplikacji przeprowadzać obliczenia zoptymalizowanych pod kątem wysokiej, równoległe dane takie jak sieci neuronowej operacji i konwersji miejsca kolorów.

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>Ulepszenia ModelIO

Struktura ModelIO w systemu tvOS 10 wprowadzono następujące ulepszenia:

 - Format pliku USD jest obecnie obsługiwany.
 - Użyj nowych `MDLMaterialPropertyGraph` klasa umożliwia łatwą obsługę środowiska uruchomieniowego zmienia się do modeli.
 - Podpisany dodano obsługę do pola odległość [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) klasy.
 - Użyj nowych `MDLLightProbeIrradianceDataSource` klasę, aby pomóc w jasny sondowania umieszczania.

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>Ulepszenia SceneKit

Struktura SceneKit w systemu tvOS 10 wprowadzono następujące ulepszenia:

 - SceneKit zawiera teraz nowy system fizycznie na podstawie renderowania (PBR) bardziej realistyczne wyniki z prostszych tworzenia zasobów.
 - Użyj nowych [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) cieniowanie modelu do produktu szeroką gamę efektów cienia realistyczne podczas wymagające tylko trzech podstawowych właściwości (`Diffuse`, `Metalness` i `Roughness`).
 - Ponieważ PBR cieniowania działa najlepiej oświetlenie oparte na środowisku, należy użyć `LightingEnvironment` właściwość do przypisania oświetlenia na podstawie obrazu tan całej sceny.
 - Użyj `IESProfileURL` właściwości do zaimportowania rzeczywistych osprzętu światła definiujących oświetlenia podstawowej na wartości rzeczywistych, takie jak natężenie (w lumenów) i kolor temperatury (w stopniach kelvin —).
 - [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera) klasy zapewniają większą wzrostu przy użyciu funkcji HDR i efektów. Adaptacyjną narażenia umożliwia utworzenie automatyczne efekty lub użyj winietowanie, odpowiedniemu kolorów i kolor klasyfikacji, aby dodać filmatic efekty do gry.
 - Zarówno PBR i HDR funkcje aparatu zapewniają lepsze wyniki niż techniki tradycyjnych renderowania i w związku z tym SceneKit wykonuje obecnie wszystkich obliczeń kolorów w liniowej przestrzeni kolorów (przy użyciu P3 przestrzeń kolorów na urządzenie kolor całej wyświetlana).
 - Kolor teraz SceneKit dopasowuje wszystkie kolory przeczytaj informacje o profilu kolorów.
 - SceneKit interpretuje wartości składnika kolorów w liniowej przestrzeni kolorów RGB dla wszystkich typów programów do cieniowania.
 - Ponieważ SceneKit odczytuje oraz dostosować informacje o profilu kolorów w obrazach tekstury, należy użyć katalogi zasobów dla wszystkich obrazów do upewnij się, że te informacje.
 - Kolor zarówno liniowej renderowania miejsca i kolor całego można wyłączyć, określając `SCNDisableLinearSpaceRendering` i `SCNDisableWideGamut` kluczy w aplikacji `Info.plist`.
 - Tworzenie Naczelne dowolnego wielokąta, (lub ładowane z pliki generowane programistycznie) do określenia geometrii z nowym [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) klasy.

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>Ulepszenia SpriteKit

Struktura SpriteKit w systemu tvOS 10 wprowadzono następujące ulepszenia:

 - Tilemaps obsługują teraz kształtów kafelka kwadratowych, gniazdo sześciokątne i izometryczny 2D, 2,5 D i gry przewijanie strony za pomocą `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` i `SKTileSet` klasy.
 - Użyj nowych `SKWarpGeometry` klasy stretch lub zakłócać [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) lub [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) renderowania. Nowy [SKAction](https://developer.apple.com/reference/spritekit/skaction) klasa może być używana do animowania przejścia między efekty warp.
 - Niestandardowe programów do cieniowania może zapewnić atrybutów (`SKAttribute`) które mogą być konfigurowane niezależnie w każdym węźle, który używa programu do cieniowania, podając wartość atrybutu (`SKAttributeValue`).
 - [SKView](https://developer.apple.com/reference/spritekit/skview) klasa udostępnia kilka nowych metod umożliwiają precyzyjną kontrolę nad warunkami i sposobami sceny jest renderowany.

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>Ulepszenia UIKit

Struktura UIKit w systemu tvOS 10 wprowadzono następujące ulepszenia:

 - Ulepszono fokus interfejsu API do obsługi fokus elementu widok-dodatkowo do `UIViews`. Elementy, które obsługują fokus _musi_ zaimplementować `IUIFocusItem` interfejsu.
 - Nowy `UIGraphicsRender` klasy udostępnia zorientowane obiektowo metoda tworzenia mapy bitowe lub plików PDF z renderowaniem UIKit lub Core grafiki i zastępuje przestarzałe `UIGraphicsBeginImageContext` metody.
 - `UIUserInterfaceStyle` Klasy został dodany do określenia, które motywu interfejsu użytkownika (ciemny lub światła) jest obecnie aktywny.
 - Dodano nowe obsługę całkowicie interakcyjny, opartej na obiektach, przerywania animacji i van można połączyć z gestów. Pleas Zobacz firmy Apple w [UIViewAnimating Protocol Reference](https://developer.apple.com/reference/uikit/uiviewanimating), [odwołania do klasy UIViewPropertyAnimator](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protocol Reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [Odwołania do klasy UICubicTimingParameters](https://developer.apple.com/reference/uikit/uicubictimingparameters) i [odwołania do klasy UISpringTimingParameter](https://developer.apple.com/reference/uikit/uispringtimingparameters) Aby uzyskać więcej informacji.
 - Nowy `UIPreviewInteraction` i `UIPreviewInteractionDelegate` Zezwalaj aplikacji aplikacji do zapewnienia niestandardowy interfejs operacje wglądu i pop.
 - Nowe `UIAccessibilityCustomRotor` klasa umożliwia aplikacji udostępnia technologie pomocnicze, takie jak głos za pośrednictwem funkcji niestandardowych, specyficznej dla kontekstu.
 - Użyj `UIAccessibilityIsAssistiveTouchRunning` i `UIAccessibilityAssistiveTouchStatusDidChangeNotification` symbole, aby określić, czy AssistiveTouch jest włączony.
 - Użyj `UIAccessibilityHearingDevicePairedEar` i `UIAccessibilityHearingDevicePairedEarDidChangeNotification` symbole, aby pobrać stan dowolnego łączyć MIF aparaty słuchowe.
 - Nowy [UIPasteboard](https://developer.apple.com/reference/uikit/uipasteboard) interfejs API udostępnia nowe opcje (np. okres istnienia ograniczenia) i zostanie automatycznie zadeklarować zgodne typy zawartości dla typowych klasy.
 - Do obsługi typu dynamicznego w etykietach, korzystać z nowych pól tekstowych i pól tekstowych `PreferredFontForTextStyle` metody `UIFont` klasy.
 - Podjęcie decyzji, element należy zaktualizować jej czcionki podczas urządzenia `UIContentSizeCategory` zmiany, użyj `AdjustsFontForContentSizeCategory` właściwość `UIContentSizeCategoryAdjusting` delegowanie.
 - Aplikację można teraz sterować wyglądem BADGE dla elementów paska kartę, takich jak kolor tła i tekstu.
 - Kontrolki Odśwież teraz obsługiwane we wszystkich przewijania widoku oraz widoku przewijania podklasy (takie jak `UICollectionView`).
 - `OpenURL` Metody `UIApplication` klasy jest wywoływana asynchronicznie obsługuje teraz obsługi zakończenia, która jest wywoływana po wykonaniu Otwórz.
 - Inicjowania udostępniania CloudKit i zmodyfikuj jego właściwości za pomocą nowej `UICloudSharingController` i `UICloudSharingControllerDelegate` klasy.
 - Zalety prefetched komórek w celu poprawy wydajności przewijania `UICollectionViews` z nowym `UICollectionViewDataSourcePrefetching` delegowanie.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [What's new in systemu tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
