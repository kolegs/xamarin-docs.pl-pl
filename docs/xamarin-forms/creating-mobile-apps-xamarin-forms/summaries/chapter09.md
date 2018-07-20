---
title: Podsumowanie rozdziałów 9. Wywołania interfejsu API specyficzne dla platformy
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziału 9. Wywołania interfejsu API specyficzne dla platformy'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: e7b2eea22758155db7d79fa26f3376e16cf16a45
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39157019"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Podsumowanie rozdziałów 9. Wywołania interfejsu API specyficzne dla platformy

> [!NOTE] 
> Uwagi na tej stronie wskazać obszary, w którym Xamarin.Forms podzielił z materiału znajdujące się w książce.

Czasami jest niezbędne do uruchomienia kodu, który jest różny od platformy. W tym rozdziale przedstawiono technik.

## <a name="preprocessing-in-the-shared-asset-project"></a>Przetwarzanie wstępne w do projektu zasobów udostępnionych

Xamarin.Forms projektu udostępnionego zasobu może wykonać różny kod dla każdej z platform za pomocą dyrektywy preprocesora C# `#if`, `#elif`, i `endif`. Jest to zaprezentowane w [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Potrójna zrzut ekranu przedstawiający zmiennej sformatowane akapitu](images/ch09fg01-small.png "Model urządzenia i systemu operacyjnego")](images/ch09fg01-large.png#lightbox "Model urządzenia i systemu operacyjnego")

Jednak wynikowy kod może być nieładnego i trudne do odczytania.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Równoległe klas do projektu zasobów udostępnionych

Bardziej ustrukturyzowane podejście do wykonywania kodu specyficznego dla platformy w systemie SAP jest przedstawiona w [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) próbki. Każdy z projektów platformy o identycznej nazwie klasy i metody, ale zaimplementowane dla tej konkretnej platformy. SAP następnie po prostu tworzy wystąpienie klasy i wywołuje metodę.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService i biblioteki klas przenośnych

> [!NOTE] 
> Biblioteki klas przenośnych zostały zastąpione przez biblioteki .NET Standard. Cały kod przykładowy z książki został przekonwertowany przy użyciu standardowych bibliotek platformy .NET.

Biblioteki zwykle nie może uzyskać dostęp do klas w projektach aplikacji. To ograniczenie wydaje się, aby zapobiec techniki przedstawione w **PlatInfoSap2** używany w bibliotece. Jednak zestaw narzędzi Xamarin.Forms zawiera klasę o nazwie [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) używającej odbicie .NET dostęp do publicznych klas w projekcie aplikacji z biblioteki.

Należy zdefiniować biblioteki `interface` z elementami członkowskimi, należy go używać w każdej z platform. Następnie w każdej z platform zawiera implementację tego interfejsu. Klasa, która implementuje interfejs musi być identyfikowany za pomocą [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute) na poziomie zestawu.

Biblioteka następnie używa ogólnego [ `Get` ](xref:Xamarin.Forms.DependencyService.Get*) metody `DependencyService` do uzyskania wystąpienie klasy platforma, która implementuje interfejs.

Jest to zaprezentowane w [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) próbki.

## <a name="platform-specific-sound-generation"></a>Generowanie dźwięku specyficzne dla platformy

[ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) Przykładowa aplikacja dodaje dźwięków do **MonkeyTap** program, uzyskując dostęp do urządzenia dźwiękowe generacji w każdej z platform.

## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 9 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Przykłady rozdział 9](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Zależność usługi](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
