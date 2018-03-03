---
title: Dodatkowe macOS Sierra Framework zmiany
description: "W tym artykule omówiono dodatkowych, drobne zmiany lub ulepszenia istniejących struktur dla macOS Sierra."
ms.topic: article
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d49276d6367a52a05d486cd5cab198c666965bf7
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="additional-macos-sierra-framework-changes"></a>Dodatkowe macOS Sierra Framework zmiany

_W tym artykule omówiono dodatkowych, drobne zmiany lub ulepszenia istniejących struktur dla macOS Sierra._

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>Przyspieszenie ulepszenia Framework

Przyspieszenie ramach macOS Sierra zostały wprowadzone następujące rozszerzenie:

- Dodano wykorzystujące technikę (calculus całkowitych).
- Dodano funkcje podstawowe dotyczące konstruowania sieci neuronowej.
- Dodano geometryczną funkcji predykatu do testowania czynności, takich jak część wspólną dwóch obiektów geometrycznej.

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>Ulepszenia Appkit Framework

Ramach AppKit macOS Sierra zostały wprowadzone następujące rozszerzenie:

- Kilka ulepszeń `NSCollectionView` takich jak:
    - **Zwijane sekcje** — zezwala użytkownikowi na zwinąć sekcję widok kolekcji w pojedynczy wiersz poziomej.
    - **Liczby zmiennoprzecinkowe nagłówki** -nagłówków i stopek można teraz przestawione (w układzie blokowym) przy użyciu tego samego interfejsu API jako [UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) w systemie iOS.
    - **Przewijany widoków tła** — teraz można ustawić tła widoki kolekcji przewijanie wraz z zawartością.
- Przebieg układu odroczonego widok został zoptymalizowany i rozszerzony.
- Przeciągnij i upuść interfejsu API zawiera teraz nowe `NSFilePromiseProvider` i `NSFilePromiseReceiver` klasy do obsługi przeciągnij flocking.
- Kilka konstruktorów wygody zostały dodane do istniejących formantów:
    -  `NSButton` zawiera nowe konstruktory Tworzenie przycisków, przyciski pola wyboru i opcji.
    -  `NSTextField` zawiera nowe konstruktorów tworzenia etykiety zawijania i bez zawijania, oparte na atrybutach etykiety i pola edycji.
    -  `NSSegmentedControl` zawiera nowe konstruktorów przy tworzeniu formantów segmentu z grupy etykiety lub obrazów.
    -  `NSSlider` zawiera nowe konstruktorów tworzenia poziome suwaki liniowej.
    -  `NSImageView` zawiera nowe konstruktorów do tworzenia widoków nieedytowalnego obrazu z podanego `NSImage`.
-  Nowe `NSGridView` został dodany do kolekcji widoków sub do siatki o zmiennej wielkości wierszy i kolumn, które można dynamicznie ukryte lub pokazane układu automatycznego.

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>Ulepszenia AVFoundation Framework

Ramach AVFoundation macOS Sierra zostały wprowadzone następujące rozszerzenie:

- W macOS, aplikacja nie ma już do implementacji innego [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) zachowania oparty na typie zawartości. Wystarczy ustawić `Rate` właściwości i AVFoundation określi, kiedy wystarczającej zawartości jest dostępny dla odtwarzania bez Gaśnięcie silnika.
- Nowe `AVPlayerLooper` klasy ułatwia pętli dany element nośnika podczas odtwarzania.
- `AVAssetDownloadURLSession` Klasa umożliwia pobieranie i nowszym odtwarzanie FairPlay zaszyfrowanych strumienie HLS.

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>Rozszerzenia Framework danych podstawowych

Do struktury danych podstawowych dla macOS Sierra zostały wprowadzone następujące rozszerzenie:

- Główny [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) obiektów obsługuje równoczesnych powodujący błąd i pobieranie bez serializacji.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) klasy przechowuje pulę magazynów danych SQLite.
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) obiektów z magazynów danych SQLite w obsługi trybu dziennika odnowy nowej generacji zapytania funkcji, których zarządzanego obiektu kontekstów (MOC) można przypiąć do wersji określonej bazy danych dla przyszłych pobierania i Błąd transakcji.
- Przy użyciu wysokiego poziomu `NSPersistenceContainer` do odwołania `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) i innych danych podstawowych konfiguracji zasobów.
- Dodano kilka nowych metod wygody `NSManagedObject` ułatwianie wykonywania pobrań i utworzyć podklasy.

Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania Framework danych podstawowych](https://developer.apple.com/reference/coredata).

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>Ulepszenia Framework obrazu Core

Core Framework obrazu dla macOS Sierra zostały wprowadzone następujące rozszerzenie:

- `ImageWithExtent` Metody [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) klasa może być używana do przetwarzania niestandardowego operacja wstawiania do filtru. Obraz Core będzie wywołania danego wywołania zwrotnego między filtry podczas przetwarzania obrazu dla danych wyjściowych lub wyświetlić.
- Aplikacja teraz przetwarzać obrazów w przestrzeń kolorów poza przestrzeń kolorów pracy kontekstu obrazu Core konwertując i przestrzeń kolorów przed i po zakończeniu przetwarzania.
- Obraz Core jądra można teraz zażądać format danych wyjściowych pikseli.
- Dodano następujące nowe filtry obrazów: `CINinePartTitled`, `CINinePartStretched`, `CIHueSaturationValueGradient`, `CIEdgePreserveUpsampleFilter` i `CIClamp`.

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Ulepszenia Framework Foundation

Do struktury Foundation dla macOS Sierra zostały wprowadzone następujące rozszerzenie:

- Użyj [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) interfejsu API reprezentujący, konwersji i wyświetlania wielu typowych jednostek fizycznych takich jak masa, długość, szybkość, czas trwania i temperatury.
- Użyj [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) daty sformatowane klasę do analizowania i generowania ISO 8601.
- Użyj nowych [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) klasie, aby obliczeń Interwał daty i czasu, takie jak czas trwania, porównanie odstępach czasu i testowania dla interwału przecięcia.
- Użyj [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) klasę do analizowania elementów nazwisko osoby z ciągu.
- Użyj nowych [NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) klasy w celu uzyskania metryki dla danego adresu URL sieci sesji.

Aby uzyskać więcej informacji, zobacz firmy Apple [Foundation informacje o wersji dla v10.12 OS X i iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html).

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>Ulepszenia GameKit Framework

Ramach GameKit macOS Sierra zostały wprowadzone następujące rozszerzenie:

- **Aplikacji Centrum gier** przestarzałe i usunąć z macOS. Jeśli aplikacja korzysta z GameKit, jego _musi_ przedstawić własny interfejs wyświetlanie GameKit funkcji, np. tablice wyników. 
- Nowy typ konta tylko do usługi iCloud została zaimplementowana przez [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) klasy.
- Nowy [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) klasy zawiera rozwiązania uogólnionego zarządzania magazynu danych w aplikacji Game Center. `GKGameSession` przechowuje listę odtwarzaczy i aplikacja jest odpowiedzialny formularza wykonania, kiedy jest przechowywana data uczestnika, pobrać lub wymieniane między graczy. W wielu przypadkach gier sesji można zastąpić istniejących na podstawie Włącz dopasowań, dopasowań w czasie rzeczywistym lub trwałe gry Zapisz metody.

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>Ulepszenia GamePlayKit Framework

Ramach GamePlayKit macOS Sierra zostały wprowadzone następujące rozszerzenie:

- Generowanie procedurach szumu został dodany i może służyć do zwiększenia wzrostu w wyszukiwania naturalnego tekstury, Dodaj wzrostu do aparatu przepływu i zapewnić sformatowanego względem gier.
- Użyj przestrzennych partycjonowania do partycjonowania danych world gier dla wydajne wyszukiwanie.
- Nowe strategist Monte Carlo ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) został dodany do pełnego możliwe przeniesienie obliczeń.
- Dodano nowy interfejs API drzewa decyzyjnego ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) i [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) w celu zwiększenia AI tworzenia gier.
- Dodano obsługę 3D do istniejącego agenta i zachowania znajdowanie ścieżki przy użyciu nowego [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) i [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) klasy.
- Użyj nowych [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) klasy zapewnienie wysokiej wydajności, wyszukiwanie naturalnego ścieżki.
- Nowy [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) i [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) upewnij klasy połączenie GameplayKit i SpriteKit łatwiejsze niż kiedykolwiek wcześniej.

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>Ulepszenia Framework kompletnego stanu

Platforma systemu operacyjnego dla macOS Sierra zostały wprowadzone następujące rozszerzenie:

- Teraz można używać aplikacjach i grach 3W _tworzenia mozaiki_ wydajnie renderowanie geometrii za pośrednictwem procesora GPU i złożone sceny.
- Specjalizacja funkcji umożliwia utworzenie kolekcji stopniu zoptymalizowany i funkcje kombinacji światła do sceny.
- Zapewnia precyzyjną kontrolę alokacji zasobów w celu zoptymalizowania wydajności systemu operacyjnego na podstawie aplikacji przy użyciu zasobów sterty i Memoryless celów renderowania.

Aby dowiedzieć się więcej, zobacz firmy Apple [przewodnik programowania w języku systemu operacyjnego](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>Ulepszenia Framework we/wy modelu

Strukturę modelu we/wy dla macOS Sierra zostały wprowadzone następujące rozszerzenie:

- Format pliku USD jest obecnie obsługiwany.
- Użyj nowych `MDLMaterialPropertyGraph` klasa umożliwia łatwą obsługę środowiska uruchomieniowego zmienia się do modeli.
- Podpisany dodano obsługę do pola odległość [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) klasy.
- Użyj nowych `MDLLightProbeIrradianceDataSource` klasę, aby pomóc w jasny sondowania umieszczania.

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>Ulepszenia Framework zdjęć

Ramach zdjęć macOS Sierra zostały wprowadzone następujące rozszerzenie:

- Edycja fotografii na żywo jest teraz dostępna dla aplikacji, które obsługują framework zdjęć i rozszerzenia do edycji fotografii (wewnątrz zdjęć i aparatu do użytku aplikacji).
- Użyj nowych [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) klasę do edycji dotyczą zarówno wideo i nadal zawartości na żywo zdjęcia.
- Użyj [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) i [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) klasy, aby skorzystać z nowych funkcji procesor obrazu Core przeprowadzenie edycji.
- Do obsługi na żywo zdjęć [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) i [PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) klasy systemie z systemem iOS macOS.

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>Ulepszenia SceneKit Framework

Ramach SceneKit macOS Sierra zostały wprowadzone następujące rozszerzenie:

- Zawiera teraz nowy system fizycznie na podstawie renderowania (PBR) bardziej realistyczne wyniki z prostszych tworzenia zasobów.
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

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>Ulepszone zabezpieczenia Framework

Następujące rozszerzenia zostały wykonane przez strukturę zabezpieczeń dla macOS Sierra:

- `SecKey` Interfejs został modernizowana i unified na wszystkich platformach (z systemem iOS, systemu tvOS, watchOS i macOS).

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>Ulepszenia SpriteKit Framework

Ramach SpriteKit macOS Sierra zostały wprowadzone następujące rozszerzenie:

- Tilemaps obsługują teraz kształtów kafelka kwadratowych, gniazdo sześciokątne i izometryczny 2D, 2,5 D i gry przewijanie strony za pomocą `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` i `SKTileSet` klasy.
- Użyj nowych `SKWarpGeometry` klasy stretch lub zakłócać [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) lub [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) renderowania. Nowy [SKAction](https://developer.apple.com/reference/spritekit/skaction) klasa może być używana do animowania przejścia między efekty warp.
- Niestandardowe programów do cieniowania może zapewnić atrybutów (`SKAttribute`) które mogą być konfigurowane niezależnie w każdym węźle, który używa programu do cieniowania, podając wartość atrybutu (`SKAttributeValue`).
- [SKView](https://developer.apple.com/reference/spritekit/skview) klasa udostępnia kilka nowych metod umożliwiają precyzyjną kontrolę nad warunkami i sposobami sceny jest renderowany.

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>Nowe struktury

System macOS Sierra dodano następujące struktury:

- **Framework intencje** — ta struktura Zezwalaj aplikacji na Sprawdź interakcji (takie jak akcje lokalizacji lub użytkownik) i podejmij działania na podstawie tych informacji.
- **SafariServices Framework** — ta struktura Zezwalaj aplikacji na tworzenie rozszerzeń aplikacji dla programu Safari (na przykład blockers zawartości) macOS i iOS.

## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla komputerów Mac](https://developer.xamarin.com/samples/mac/)
- [What's new in 10.12 X systemu operacyjnego](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
