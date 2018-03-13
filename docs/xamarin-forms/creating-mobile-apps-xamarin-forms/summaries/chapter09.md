---
title: "Podsumowanie rozdziału 9. Wywołania API specyficzne dla platformy"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 84650c930445172d27520129123d493253851642
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Podsumowanie rozdziału 9. Wywołania API specyficzne dla platformy

Czasami jest niezbędne do uruchomienia niektórych kodu, który jest różny od platformy. W tym rozdziale Eksploruje technik.

## <a name="preprocessing-in-the-shared-asset-project"></a>Przetwarzanie wstępne projektu współużytkowanych zasobów

Projekt platformy Xamarin.Forms udostępnionych zasobów może zostać uruchomiony inny kod dla każdej platformy przy użyciu dyrektywy preprocesora C# `#if`, `#elif`, i `endif`. To jest przedstawiona w [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Potrójna zrzut ekranu przedstawiający zmiennej sformatowany akapit](images/ch09fg01-small.png "Model urządzenia i systemu operacyjnego")](images/ch09fg01-large.png#lightbox "Model urządzenia i systemu operacyjnego")

Jednak wynikowy kod może być ugly i trudne do odczytania.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Równoległe klas w projekcie zasobów udostępnionych

Bardziej ustrukturyzowanymi podejście do wykonywania kodu specyficzne dla platformy w SAP jest przedstawiona w [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) próbki. Każdy z projektów platformy o identycznej nazwie klasy i metody, ale zaimplementowany dla tej konkretnej platformy. SAP następnie po prostu tworzy wystąpienie klasy i wywołuje metodę.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService i biblioteki klas przenośnych

Zwykle biblioteki nie może uzyskać dostępu klas w projektach aplikacji. To ograniczenie jest prawdopodobnie zapobiec technika pokazano **PlatInfoSap2** użycia w PCL. Jednak platformy Xamarin.Forms zawiera klasę o nazwie [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) używającą odbicia .NET dostęp do publicznych klas w projekcie aplikacji z PCL.

Zdefiniuj PCL `interface` z elementami członkowskimi, należy go używać w każdej z platform. Następnie w każdej z platform zawiera implementację tego interfejsu. Klasa, która implementuje interfejs musi być identyfikowany z [DependencyAttribute](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyAttribute/) na poziomie zestawu.

PCL, a następnie używa ogólnego [ `Get` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DependencyService.Get{T}/p/Xamarin.Forms.DependencyFetchTarget/) metody `DependencyService` uzyskanie wystąpienia klasy platformy, która implementuje interfejs.

To jest przedstawiona w [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) próbki.

## <a name="platform-specific-sound-generation"></a>Generowanie dźwięku specyficzne dla platformy

[ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) próbki dodaje sygnały do **MonkeyTap** program uzyskując dostęp do urządzeń generacji dźwięk w każdej z platform.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 9 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Przykłady rozdział 9](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Zależność usługi](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
