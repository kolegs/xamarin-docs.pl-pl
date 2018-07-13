---
title: Zestaw narzędzi Xamarin.Forms niestandardowe programy renderujące
description: Niestandardowe programy renderujące umożliwiają deweloperom zastąpienia renderowanie kontrolki natywne na każdej platformie, aby dostosować wygląd i zachowanie kontrolki zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: c7ae25688b2f8635a9a89318e0b307e58add7a5a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998748"
---
# <a name="xamarinforms-custom-renderers"></a>Zestaw narzędzi Xamarin.Forms niestandardowe programy renderujące

_Interfejsy użytkownika Xamarin.Forms są renderowane przy użyciu natywnych kontrolek platformę docelową, umożliwiając aplikacjach Xamarin.Forms zachować odpowiedni wygląd i działanie dla każdej platformy. Niestandardowe programy renderujące umożliwiają deweloperom przesłonić ten proces, aby dostosować wygląd i zachowanie kontrolki zestawu narzędzi Xamarin.Forms na każdej platformie._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[Wprowadzenie do niestandardowe programy renderujące](introduction.md)

Niestandardowe programy renderujące zapewniają zaawansowanych sposobów dostosowywania wygląd i zachowanie kontrolki zestawu narzędzi Xamarin.Forms. One może służyć do zmiany stylów małych lub zaawansowanych układ specyficzne dla platformy i dostosowywania zachowania. Ten artykuł zawiera wprowadzenie do niestandardowe programy renderujące oraz opisano proces tworzenia niestandardowego modułu renderowania.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[Natywne kontrolki i klasy bazowe programu renderującego](renderers.md)

Każdy formant zestawu narzędzi Xamarin.Forms ma towarzyszący modułu renderowania dla każdej platformy, która tworzy wystąpienie kontrolki natywne. W tym artykule wymieniono renderowania i klasy natywne kontrolki, które implementują strony, układ, widok i komórki każdego zestawu narzędzi Xamarin.Forms.

## <a name="customizing-an-entryentrymd"></a>[Dostosowywanie wpisu](entry.md)

Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) kontrolka zezwala na pojedynczy wiersz tekstu do edycji. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla `Entry` kontrolki, dzięki czemu deweloperzy mogą zastąpić domyślne renderowanie natywnych, dostosowując swoje własne specyficzne dla platformy.

## <a name="customizing-a-contentpagecontentpagemd"></a>[Dostosowywanie obiektu ContentPage](contentpage.md)

A [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) to element graficzny, która wyświetla pojedynczy widok i zajmuje większą część ekranu. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla `ContentPage` strony, dzięki czemu deweloperzy mogą zastąpić domyślne renderowanie natywnych, dostosowując swoje własne specyficzne dla platformy.

## <a name="customizing-a-mapmapindexmd"></a>[Dostosowywanie mapy](map/index.md)

Projekt xamarin.Forms.Maps dla zapewnia abstrakcję dla wielu platform, które wyświetlanie map, korzystających z mapy natywnych interfejsów API na każdej platformie zapewnienie mapę szybkie i dobrze znane środowisko dla użytkowników. W tym temacie pokazano, jak utworzyć niestandardowe programy renderujące dla `Map` kontrolki, dzięki czemu deweloperzy mogą zastąpić domyślne renderowanie natywnych, dostosowując swoje własne specyficzne dla platformy.

## <a name="customizing-a-listviewlistviewmd"></a>[Dostosowywanie obiektu ListView](listview.md)

Rozwiązanie Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) jest widok, który wyświetla zbiór danych jako pionowy listy. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania hermetyzujący kontrolki listy specyficzne dla platformy i układy natywnych komórki, dzięki czemu większa kontrola nad natywnych listy kontrolowania wydajności.

## <a name="customizing-a-viewcellviewcellmd"></a>[Dostosowywanie obiektu ViewCell](viewcell.md)

Rozwiązanie Xamarin.Forms [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) jest komórki, które mogą być dodawane do [ `ListView` ](xref:Xamarin.Forms.ListView) lub [ `TableView` ](xref:Xamarin.Forms.TableView), która zawiera widok zdefiniowany dla deweloperów. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla `ViewCell` hostowaną wewnątrz zestawu narzędzi Xamarin.Forms `ListView` kontroli. Spowoduje to zatrzymanie obliczenia układ interfejsu Xamarin.Forms z wielokrotnie wywoływana podczas `ListView` przewijania.

## <a name="implementing-a-viewviewmd"></a>[Implementowanie kontrolki View](view.md)

Formanty interfejsów użytkownika niestandardowego zestawu narzędzi Xamarin.Forms powinien pochodzić od [ `View` ](xref:Xamarin.Forms.View) klasy, która służy do umieszczania, układy i formanty na ekranie. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla zestawu narzędzi Xamarin.Forms formantu niestandardowego, który służy do wyświetlania podglądu strumienia wideo z aparatu fotograficznego urządzenia.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[Implementowanie kontrolki HybridWebView](hybridwebview.md)

W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla `HybridWebView` formant niestandardowy, który pokazuje, jak poprawić formantów web specyficzne dla platformy umożliwiający kodu C# do wywołania języka JavaScript.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[Implementowanie odtwarzacza wideo](video-player/index.md)

W tym artykule pokazano, jak pisać programy renderujące, aby zaimplementować niestandardowy `VideoPlayer` formant, który można odtworzyć wideo z sieci web, filmów wideo osadzonych jako zasobów aplikacji lub filmy wideo, przechowywane w bibliotece wideo na urządzeniu użytkownika. Kilka technik zostały przedstawione w tym implementacja metody i właściwości, które można powiązać tylko do odczytu.


## <a name="related-links"></a>Linki pokrewne

- [Efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Niestandardowe programy renderujące (Xamarin University wideo)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [Przykładowe niestandardowe programy renderujące (Xamarin University wideo)](http://bit.ly/xf-customrenderer)
