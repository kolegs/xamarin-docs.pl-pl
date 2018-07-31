---
title: Wprowadzenie do systemu iOS 11
description: Ten dokument prowadzi różne przewodniki z instrukcjami, które opisują funkcji systemu iOS 11, w tym ARKit, CoreML, strukturze MapKit, PDFKit, zestawu SiriKit, struktury Vision i innych.
ms.prod: xamarin
ms.assetid: 22C38EA6-6DA9-4B92-B41B-814E589033F6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2017
ms.openlocfilehash: e4e544679dbed2701c50d5596d4907451653090e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351060"
---
# <a name="introduction-to-ios-11"></a>Wprowadzenie do systemu iOS 11

_Wypróbuj nowy system iOS 11 interfejsów API za pomocą platformy Xamarin._

> [!NOTE]
> Przed rozpoczęciem należy przejrzeć [wprowadzenie](get-started.md) strony, aby uzyskać instrukcje dotyczące sposobu instalowania programu Xcode 9.

![Przykład ARKit](images/arkit.png) ![AR wprowadzenie do obiektów](images/arkit2.png) ![Przykład CoreML](images/coreml.png) ![Przykład strukturze MapKit](images/mapkit.png) ![Przykład prostokątów Vision](images/vision1.png) ![Przykład twarzy Vision](images/vision2.png) ![Przeciąganie i upuszczanie przykład](images/drag-drop.png) ![Przeciąganie i upuszczanie przykład](images/drag-drop2.png) ![Przykład zestawu SiriKit](images/sirikit.png)

System iOS 11 obejmuje wiele dobór funkcji i ulepszeń w różnych platform:

## <a name="preparing-your-app-for-ios-11updating-your-appindexmd"></a>[Przygotowywanie aplikacji dla systemu iOS 11](updating-your-app/index.md)

Apple wprowadziła architektury aktualizacje, nowe zmiany wizualne i procesem zaktualizowane iTunes Connect dla systemu iOS 11. Użyj tego przewodnika, aby upewnić się, że aplikacji platformy Xamarin.iOS jest gotowy do nowej wersji.

## <a name="arkitarkitindexmd"></a>[ARKit](arkit/index.md)

ARKit łączy rzeczywistości rozszerzonej z systemem iOS, pozwalając użytkownikom na interakcję z świata przy użyciu aparatu urządzenia.
Za pomocą platformy Xamarin, można również użyć [ARKit przy użyciu platformy UrhoSharp](arkit/urhosharp.md).

## <a name="coremlcoremlmd"></a>[CoreML](coreml.md)

Aplikacje systemu iOS 11 z CoreML można zintegrować modele uczenia maszynowego. Struktura CoreML udostępnia prosty interfejs API, aby dołączyć istniejących modeli do projektów aplikacji, aby umożliwić problemów do analizy bezpośrednio w aplikacji.

## <a name="corenfccorenfcmd"></a>[CoreNFC](corenfc.md)

iPhone 7 i nowszych urządzeniach może odczytywać tagów w pobliżu komunikacji Zbliżeniowej (NFC), włączenie aplikacji wykrywanie oznakowane produktów, miejsca lub rzeczy w świecie wokół nich.

## <a name="drag-and-dropdrag-and-dropmd"></a>[Przeciąganie i upuszczanie](drag-and-drop.md)

Przeciąganie i upuszczanie framework zapewniają obsługę całego systemu iOS do przenoszenia danych przez touch. Na urządzeniu iPad można przeciągnąć zarówno wewnątrz jednej, jak i między różnych aplikacji; znajduje się na telefonie iPhone, można przeciągnąć tylko w ramach tej samej aplikacji. Są obsługiwane dla wielu typów dostosowania, w tym rozbudowane typy danych, animacji i obsługa wielodotyku gestów.

## <a name="mapkitmapkitmd"></a>[MapKit](mapkit.md)

Strukturze MapKit zawiera szereg ulepszeń, w tym obsługę automatycznego znacznika, grupowania i dodawanie Kompas do widoku.

## <a name="pdfkit"></a>PDFKit

PDFKit jest teraz dostępna w systemie iOS 11, wprowadzenie do tworzenia plików PDF i edytowania możliwości do swoich aplikacji.

## <a name="sirikitsirikitmd"></a>[SiriKit](sirikit.md)

Używanie programu Siri obsługuje teraz jeszcze bardziej interakcji, w tym listy, uwagi i inne ulepszenia, takie jak nazwy alternatywnej aplikacji.

## <a name="visionvisionmd"></a>[Wizja](vision.md)

Udostępnia szereg obraz funkcje przetwarzania i analizy z systemami iOS, w tym wykrywanie twarzy i rozpoznawania, CoreML modeli, wykrywanie kodu kreskowego nowych interfejsów API, tekstu i horizon wykrywania i bardziej ogólne wykrywanie obiektów i śledzenia.

## <a name="samples"></a>Przykłady

Obejmuje kilka języka C# [przykłady](https://developer.xamarin.com/samples/ios/iOS11/) ułatwią Ci rozpoczęcie pracy:

* [Przykładowe ARKit](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
* [ARKit wprowadzenie do obiektów](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
* [ARKit i na platformie UrhoSharp](arkit/urhosharp.md)
* [Przykładowe CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreML)
* [Przykład rozpoznawanie obrazów CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition)
* [CoreML za pomocą platformy Azure utworzyć niestandardowy Model](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
* [Przykładowe czytnika CoreNFC tagu](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
* [Przeciąganie i upuszczanie widok tabeli](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView)
* [Przeciąganie i upuszczanie widok kolekcji](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
* [Przeciąganie i upuszczanie widok niestandardowy](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCustomView)
* [DragBoard przeciągania i upuszczania próbki](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropDragBoard)
* [Przykładowe strukturze MapKit](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample)
* [Przykładowe zestawu SiriKit](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
* [Zaktualizowanego przykładowego framework zdjęcia](https://developer.xamarin.com/samples/monotouch/ios11/SamplePhotoApp/)
* [Vision i CoreML próbki](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision)
* [Wizja prostokąty wykrywania próbki](https://developer.xamarin.com/samples/monotouch/ios11/VisionRects)
* [Przykład wykrywanie twarzy Vision](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces)
* [Przykładowe elementy widget PDKFit](https://developer.xamarin.com/samples/monotouch/ios11/PDFAnnotationWidgetsAdvanced)
* [Przykładowe PDFKit znaku wodnego](https://developer.xamarin.com/samples/monotouch/ios11/PDFDocumentWatermark)

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji na temat systemu iOS 11, odwiedź stronę firmy Apple [What's New in iOS 11](https://developer.apple.com/ios/) dokumentacji.


## <a name="related-links"></a>Linki pokrewne

- [What's New in iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Przykłady dla systemu iOS 11 środowiska Xamarin](https://developer.xamarin.com/samples/ios/iOS11/)
