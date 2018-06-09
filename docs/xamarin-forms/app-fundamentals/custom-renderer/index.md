---
title: Moduły renderowania niestandardowe platformy Xamarin.Forms
description: Niestandardowe moduły renderowania umożliwiają deweloperom zastąpienie renderowanie kontrolki natywne na każdej platformie, aby dostosować wygląd i zachowanie formantów platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a88462052906e68fd85a07161e8b5bb63a61e69d
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239893"
---
# <a name="xamarinforms-custom-renderers"></a>Moduły renderowania niestandardowe platformy Xamarin.Forms

_Interfejsy użytkownika platformy Xamarin.Forms są renderowane przy użyciu kontrolki natywne platformy docelowej, umożliwiając aplikacji platformy Xamarin.Forms zachować odpowiedni wyglądu i działania dotyczące każdej platformy. Niestandardowe moduły renderowania umożliwiają deweloperom zastąpienie tego procesu, aby dostosować wygląd i zachowanie platformy Xamarin.Forms kontrolek w każdej z platform._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[Wprowadzenie do renderowania niestandardowych](introduction.md)

Niestandardowe moduły renderowania Podaj podejście zaawansowane dostosowywanie wyglądu i zachowania formantów platformy Xamarin.Forms. Służy do zmiany małych stylów lub Zaawansowane układu specyficzne dla platformy i dostosowywania zachowania. Ten artykuł zawiera wprowadzenie do renderowania niestandardowych i opisano proces tworzenia niestandardowego modułu renderowania.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[Klasy podstawowe renderowania i kontrolki natywne](renderers.md)

Formant co platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu. W tym artykule wymieniono renderowania i klasy macierzystego formantu, które implementują każdej platformy Xamarin.Forms strony, układ widoku i komórki.

## <a name="customizing-an-entryentrymd"></a>[Dostosowywanie wpisu](entry.md)

Platformy Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kontroli umożliwia pojedynczy wiersz tekstu do edycji. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla `Entry` formantu umożliwiają deweloperom zastąpienie renderowania natywnego domyślne z własne dostosowania specyficzne dla platformy.

## <a name="customizing-a-contentpagecontentpagemd"></a>[Dostosowywanie obiektu ContentPage](contentpage.md)

A [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) jest element wizualny, która wyświetla pojedynczego widoku i zajmuje większość ekranu. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla `ContentPage` strony umożliwiają deweloperom zastąpienie renderowania natywnego domyślne z własne dostosowania specyficzne dla platformy.

## <a name="customizing-a-mapmapindexmd"></a>[Dostosowywanie mapy](map/index.md)

Xamarin.Forms.Maps zapewnia abstrakcji między platformami, wyświetlania mapy, które używają mapy natywnych interfejsów API na każdej platformie, aby zapewnić szybkie i znanych mapy środowisko dla użytkowników. W tym temacie przedstawiono sposób tworzenia niestandardowych renderowania dla `Map` formantu umożliwiają deweloperom zastąpienie renderowania natywnego domyślne z własne dostosowania specyficzne dla platformy.

## <a name="customizing-a-listviewlistviewmd"></a>[Dostosowywanie obiektu ListView](listview.md)

Platformy Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) jest widok, który będzie wyświetlany jako pionowy listy zbierania danych. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania hermetyzujący kontrolki listy specyficzne dla platformy i układy natywnego komórki, dzięki czemu większa kontrola nad macierzysty listy kontrolowania wydajności.

## <a name="customizing-a-viewcellviewcellmd"></a>[Dostosowywanie obiektu ViewCell](viewcell.md)

Platformy Xamarin.Forms [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) komórka mogą być dodawane do [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) lub [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/), zawierającą widoku zdefiniowane przez dewelopera. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla `ViewCell` który znajduje się wewnątrz platformy Xamarin.Forms `ListView` formantu. Powoduje to zatrzymanie obliczenia układ platformy Xamarin.Forms z wielokrotnie wywoływane podczas `ListView` przewijania.

## <a name="implementing-a-viewviewmd"></a>[Implementowanie kontrolki View](view.md)

Formanty interfejsów użytkownika niestandardowego platformy Xamarin.Forms powinien pochodzić od [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) klasy, która służy do umieszczania układy i kontroli na ekranie. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla platformy Xamarin.Forms kontrolki niestandardowej, która jest używana do wyświetlania podglądu strumienia wideo z aparatu fotograficznego urządzenia.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[Implementowanie kontrolki HybridWebView](hybridwebview.md)

W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla `HybridWebView` kontrolki niestandardowej, która pokazuje, jak poprawić formanty specyficzne dla platformy sieci web umożliwia kodu C# do wywołania z poziomu języka JavaScript.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[Implementowanie odtwarzacza wideo](video-player/index.md)

W tym artykule przedstawiono sposób zapisu renderowania do zaimplementowania niestandardowego `VideoPlayer` formant, który można odtwarzać filmy wideo z sieci web, klipów wideo osadzony jako zasoby aplikacji lub wideo przechowywanego w bibliotece wideo na urządzeniu użytkownika. Przedstawiono kilka technik, w tym implementacja metody i właściwości tylko do odczytu.


## <a name="related-links"></a>Linki pokrewne

- [Efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Niestandardowe moduły renderowania (Xamarin University wideo)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [Przykładowe niestandardowe moduły renderowania (Xamarin University wideo)](http://bit.ly/xf-customrenderer)
