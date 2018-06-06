---
title: Wprowadzenie do systemu iOS 11
description: Ten dokument łącza do różnych prowadnic opisano funkcje systemu IOS, 11, w tym ARKit, CoreML MapKit, PDFKit, SiriKit, framework wizja i więcej.
ms.prod: xamarin
ms.assetid: 22C38EA6-6DA9-4B92-B41B-814E589033F6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: dca4d9889d90f10626840dfc722e439fd60c5756
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787510"
---
# <a name="introduction-to-ios-11"></a>Wprowadzenie do systemu iOS 11

_Spróbuj nowe iOS 11 interfejsów API za pomocą platformy Xamarin._

> [!NOTE]
> Przed rozpoczęciem należy przejrzeć [wprowadzenie](get-started.md) strony, aby uzyskać instrukcje dotyczące sposobu instalowania Xcode 9.

![Przykład ARKit](images/arkit.png) ![AR wprowadzenie do obiektów](images/arkit2.png) ![Przykład CoreML](images/coreml.png) ![Przykład MapKit](images/mapkit.png) ![Przykład prostokąty wizji](images/vision1.png) ![Wizja krojów przykład](images/vision2.png) ![Przeciągnij i upuść przykład](images/drag-drop.png) ![Przeciągnij i upuść przykład](images/drag-drop2.png) ![Przykład SiriKit](images/sirikit.png)

iOS 11 zawiera wiele całkowicie nowy funkcji i ulepszeń w różnych platform:

## <a name="preparing-your-app-for-ios-11updating-your-appindexmd"></a>[Przygotowywanie aplikacji dla systemu iOS 11](updating-your-app/index.md)

Apple wprowadziła architektura aktualizacje, nowe zmiany wizualne i procesem Connect zaktualizowane iTunes dla systemu iOS 11. Użyj tego przewodnika, aby upewnić się, że aplikacji platformy Xamarin.iOS jest gotowy do nowej wersji.

## <a name="arkitarkitindexmd"></a>[ARKit](arkit/index.md)

ARKit łączy rzeczywistości rozszerzony w systemach iOS, co pozwala użytkownikom na interakcję z na świecie za pośrednictwem aparatu fotograficznego urządzenia.
Za pomocą platformy Xamarin, można również użyć [ARKit z UrhoSharp](arkit/urhosharp.md).

## <a name="coremlcoremlmd"></a>[CoreML](coreml.md)

Aplikacje systemu iOS 11 z CoreML można zintegrować Machine learning modeli. W ramach CoreML udostępnia prosty interfejs API włączenie istniejących modeli do projektów aplikacji umożliwia problemów do analizy bezpośrednio w aplikacji.

## <a name="corenfccorenfcmd"></a>[CoreNFC](corenfc.md)

iPhone 7 i nowsze urządzenia mogą odczytywać tagów w pobliżu komunikacji Zbliżeniowej (NFC), włączanie aplikacji do wykrywania oznakowanych produktów, miejscami lub elementów w świecie wokół nich.

## <a name="drag-and-dropdrag-and-dropmd"></a>[Przeciąganie i upuszczanie](drag-and-drop.md)

Przeciąganie i upuszczanie framework zapewnia obsługę systemu iOS na poziomie przenoszenia danych przez touch. Na urządzeniu iPad możesz przeciągać w obrębie i między różnych aplikacji. gdy na telefonie iPhone, możesz przeciągnąć tylko w obrębie tej samej aplikacji. Brak obsługi wielu typów dostosowania, w tym typy danych sformatowanego, animacji i gestów multitouch obsługi.

## <a name="mapkitmapkitmd"></a>[MapKit](mapkit.md)

MapKit ma wiele ulepszeń, w tym obsługi automatycznego znacznika, grupowanie i dodawanie Kompas do widoku.

## <a name="pdfkit"></a>PDFKit

PDFKit jest teraz dostępna w systemie iOS 11, przynoszą do tworzenia plików PDF i edytowania do aplikacji.

## <a name="sirikitsirikitmd"></a>[SiriKit](sirikit.md)

Używanie programu Siri obsługuje teraz jeszcze więcej interakcji, łącznie z listy i uwagi i inne rozszerzenia, takich jak nazwy alternatywnej aplikacji.

## <a name="visionvisionmd"></a>[Wizja](vision.md)

Wprowadzono szereg obrazu funkcje przetwarzania i analizy w systemach iOS, w tym wykrywania twarzy na obrazie i rozpoznawania, CoreML modeli nowy kod kreskowy wykrywania interfejsów API, tekst i typu horizon wykrywania i bardziej ogólne wykrywania obiektu i śledzenia.

## <a name="samples"></a>Przykłady

Mamy wiele C# [przykłady](https://developer.xamarin.com/samples/ios/iOS11/) ułatwiających rozpoczęcie pracy:

* [Przykładowe ARKit](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
* [ARKit wprowadzenie do obiektów](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
* [ARKit i UrhoSharp](arkit/urhosharp.md)
* [Przykładowe CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreML)
* [Przykład rozpoznawania CoreML obrazu](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition)
* [CoreML z modelem niestandardowych Azure](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
* [CoreNFC Tag czytnika próbki](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
* [Przeciągnij i upuść widoku tabeli](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView)
* [Przeciągnij i upuść widok kolekcji](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
* [Przeciągnij i upuść widok niestandardowy](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCustomView)
* [DragBoard przeciągania i upuszczania próbki](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropDragBoard)
* [Przykładowe MapKit](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample)
* [Przykładowe SiriKit](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
* [Przykładowy framework zdjęć](https://developer.xamarin.com/samples/monotouch/ios11/SamplePhotoApp/)
* [Wizja & CoreML próbki](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision)
* [Wizja prostokąty wykrywania próbki](https://developer.xamarin.com/samples/monotouch/ios11/VisionRects)
* [Wizja kroje wykrywania próbki](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces)
* [Przykładowe elementy widget PDKFit](https://developer.xamarin.com/samples/monotouch/ios11/PDFAnnotationWidgetsAdvanced)
* [Znak wodny PDFKit próbki](https://developer.xamarin.com/samples/monotouch/ios11/PDFDocumentWatermark)

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji w systemie iOS 11, odwiedź stronę firmy Apple [nowości w systemie iOS 11](https://developer.apple.com/ios/) dokumentacji.


## <a name="related-links"></a>Linki pokrewne

- [Nowości w systemie iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Xamarin iOS 11 próbek](https://developer.xamarin.com/samples/ios/iOS11/)
