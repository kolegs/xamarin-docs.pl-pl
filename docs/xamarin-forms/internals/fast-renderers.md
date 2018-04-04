---
title: Szybkie renderowania
description: W tym artykule przedstawiono szybki moduły renderowania, które zmniejszają inflacji i kosztów renderowania formantu platformy Xamarin.Forms w systemie Android przez spłaszczanie wynikowy hierarchii macierzystego formantu.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 16807d890f12810ccc1d50cb8e506e104ec8e6a3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="fast-renderers"></a>Szybkie renderowania

_W tym artykule przedstawiono szybki moduły renderowania, które zmniejszają inflacji i kosztów renderowania formantu platformy Xamarin.Forms w systemie Android przez spłaszczanie wynikowy hierarchii macierzystego formantu._

Zazwyczaj większość oryginalnego renderowania formantu w systemie Android składają się z dwóch widoków:

- Natywny kontrolować, takich jak `Button` lub `TextView`.
- Kontener `ViewGroup` obsługująca część pracy układu, Obsługa gestu i innych zadań.

Jednak takie podejście ma wpływ na wydajność w dwóch widoków są tworzone dla każdej kontrolki logicznego, co prowadzi do bardziej złożonych drzewa wizualnego, która wymaga więcej pamięci i inne przetwarzania do renderowania na ekranie.

Szybkie renderowania zmniejszyć inflacji i kosztów renderowania formantu platformy Xamarin.Forms w jednym widoku. Dlatego zamiast tworzenia dwóch widoków, a następnie dodanie ich do drzewa widoku, tylko jeden z nich jest tworzony. Poprawia to wydajność, tworząc mniejszą liczbę obiektów, które z kolei oznacza mniej złożona drzewa widoku, a mniej wykorzystanie pamięci, (która także powoduje mniej pamięci kolekcji pauzy).

Szybkie renderowania są dostępne w następujących kontrolkach w 2.4 platformy Xamarin.Forms w systemie Android:

- [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)
- [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)
- [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)

Funkcjonalnie te szybkiego renderowania są nie różni się od oryginalnej renderowania. Jednak są obecnie eksperymentalne i mogą służyć tylko przez dodanie poniższego kodu do Twojej `MainActivity` klasy przed wywołaniem `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> Szybkie renderowania dotyczą tylko zaplecza z systemem Android zgodność aplikacji, to ustawienie jest ignorowana w wersji pre-app compat działań.

Ulepszenia wydajności będą się różnić dla poszczególnych aplikacji, w zależności od złożoności układu. Na przykład poprawy wydajności x2 to możliwe, gdy przewijanie [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) zawierające tysiące wiersze danych, gdzie komórek w każdym wierszu składają się z formantów, które używają szybkiego moduły renderowania, które powoduje widoczny płynniejszy przewijania.


## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe programy renderujące](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
