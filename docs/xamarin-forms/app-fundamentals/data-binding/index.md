---
title: Powiązanie danych
description: Powiązanie danych to technika łączenia właściwości dwa obiekty, dzięki czemu zmiany w jednej właściwości są automatycznie odzwierciedlane w innych właściwości. Powiązanie danych jest integralną częścią architektury Model-View-ViewModel (MVVM) aplikacji.
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: ee8481696b0ef85aec949c6def7767e57eb99e17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="data-binding"></a>Powiązanie danych

_Powiązanie danych to technika łączenia właściwości dwa obiekty, dzięki czemu zmiany w jednej właściwości są automatycznie odzwierciedlane w innych właściwości. Powiązanie danych jest integralną częścią architektury Model-View-ViewModel (MVVM) aplikacji._

## <a name="the-data-linking-problem"></a>Problem łączenie danych

Aplikacja platformy Xamarin.Forms składa się z co najmniej jedna strona, z których każdy zawiera wiele obiektów interfejsu użytkownika o nazwie zazwyczaj *widoków*. Jest jeden z podstawowych zadań programu, aby zachować te widoki zsynchronizowane i do śledzenia różne wartości lub wybrane elementy, które reprezentują. Często widoki reprezentują wartości ze źródła danych, a użytkownik modyfikuje tych widoków, aby zmienić te dane. Po zmianie widoku danych muszą odzwierciedlenia tej zmiany i podobnie zmiany danych, taka zmiana musi mieć odzwierciedlenie w widoku.

Aby pomyślnie obsługiwać tego zadania, program podaje zmiany w tych widokach lub dane źródłowe. Typowe rozwiązania jest określenie zdarzenia, które sygnalizują po zmianie. Program obsługi zdarzeń mógł zostać zainstalowany powiadomiona tych zmian. Odpowiada za przesyłania danych z jednego obiektu. Jednak w przypadku wielu widoków musi również istnieć wiele programów obsługi zdarzeń i pobiera związane z dużą ilością kodu.

## <a name="the-data-binding-solution"></a>Rozwiązania powiązania danych

Powiązanie danych automatyzuje tego zadania, a następnie renderuje niepotrzebnych obsługi zdarzeń. (Zdarzenia są nadal wymagane, jednak ponieważ infrastruktura wiązania danych używa ich). Powiązania danych można zaimplementować w kodzie lub w języku XAML, ale są one znacznie częściej w języku XAML, gdzie pomagają zmniejszyć rozmiar pliku CodeBehind. Zastępując kod procedury w obsłudze zdarzenia deklaratywne kod lub znacznik, aplikacja jest prostszy i wyjaśniono.

Jeden z dwóch obiektów zaangażowane w powiązaniu danych jest prawie zawsze element, który jest pochodną `View` i stanowi część wizualny interfejs strony. Drugi obiekt jest:

- Inny `View` pochodnych, zwykle na tej samej stronie.
- Obiekt w pliku kodu.

W programach demonstracyjnych, takich jak te w [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) przykładowe powiązania danych między dwiema `View` pochodne często są wyświetlane dla celów jasności i prostota. Jednak te same zasady można zastosować do powiązania danych między `View` i innych obiektów. Po utworzeniu aplikacji przy użyciu architektury Model-View-ViewModel (MVVM) klasy z danych źródłowych jest często nazywana ViewModel.

Powiązania danych są przedstawione w serii następujące artykuły:

## <a name="basic-bindingsbasic-bindingsmd"></a>[Podstawowe powiązania](basic-bindings.md)

Różnice między wiązania danych źródłowe i docelowe i zobacz proste danych powiązania w kodzie i pliku XAML.

## <a name="binding-modebinding-modemd"></a>[Tryb powiązania](binding-mode.md)

Wykryj, metody kontrolowania przepływ danych między tymi dwoma obiektami z trybu wiązania.

## <a name="string-formattingstring-formattingmd"></a>[Formatowanie ciągu](string-formatting.md)

Powiązanie danych umożliwia sformatowanie i Wyświetl obiekty jako ciągi.

## <a name="binding-pathbinding-pathmd"></a>[Ścieżka powiązania](binding-path.md)

Lepsze zapoznanie się `Path` właściwości powiązania danych dostęp do właściwości podrzędnych i członków kolekcji.

## <a name="binding-value-convertersconvertersmd"></a>[Konwertery wartości powiązania](converters.md)

Konwertery wartości powiązania umożliwia alter wartości w ciągu powiązania danych.

## <a name="the-command-interfacecommandingmd"></a>[Interfejs polecenia](commanding.md)

Implementowanie `Command` właściwości powiązania danych.



## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/markup-extensions/index.md)
