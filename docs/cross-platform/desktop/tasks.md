---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Typowe zadania porównania
ms.technology: xamarin-crossplatform
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 84b3edc1a8cbc9ad642d94b457321786fae5fe70
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="common-tasks-comparison"></a>Typowe zadania porównania

| Zadanie | WPF | Xamarin.Forms |
|--- |--- |--- |
|Wyświetl komunikat na ekranie za pomocą przycisków|`MessageBox`|`Page.DisplayAlert`|
|Utwórz czasomierza|`DispatcherTimer` Klasy|`Device.StartTimer` Metoda statyczna|
|Pobierz domyślny rozmiar czcionki|`SystemFonts` Klasa statyczna|`Device.GetNamedSize` Metoda statyczna|
|Otwórz identyfikatora URI lub adres URL|`Process.Start`|`Device.OpenUri`|
|Wyświetl arkusz akcji (Lista przycisków)|n/d|`Page.DisplayActionSheet`|
