---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Typowe zadania porównania
description: Ten dokument zawiera porównanie sposób wykonywania różnych zadań na WPF i platformy Xamarin.Forms. Wygląda na przyciski, czasomierze, rozmiarze czcionki, otwierając identyfikatora URI i wyświetlanie arkusza akcji.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 2991e74ec59e3c43c665941706c2d9ac94bfb9ad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780479"
---
# <a name="common-tasks-comparison"></a>Typowe zadania porównania

| Zadanie | WPF | Xamarin.Forms |
|--- |--- |--- |
|Wyświetl komunikat na ekranie za pomocą przycisków|`MessageBox`|`Page.DisplayAlert`|
|Utwórz czasomierza|`DispatcherTimer` Klasy|`Device.StartTimer` Metoda statyczna|
|Pobierz domyślny rozmiar czcionki|`SystemFonts` Klasa statyczna|`Device.GetNamedSize` Metoda statyczna|
|Otwórz identyfikatora URI lub adres URL|`Process.Start`|`Device.OpenUri`|
|Wyświetl arkusz akcji (Lista przycisków)|n/d|`Page.DisplayActionSheet`|
