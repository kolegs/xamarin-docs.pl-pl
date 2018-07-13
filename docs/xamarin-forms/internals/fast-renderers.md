---
title: Xamarin.Forms szybkie programy renderujące
description: W tym artykule przedstawiono szybkie programy renderujące, które zmniejszają inflacji i koszty renderowanie kontrolki zestawu narzędzi Xamarin.Forms w systemie Android spłaszczając wynikowy hierarchii kontroli natywnych.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: e4b060c703077e140e0f0d2f8c4c2b824c890e8d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997123"
---
# <a name="xamarinforms-fast-renderers"></a>Xamarin.Forms szybkie programy renderujące

_W tym artykule przedstawiono szybkie programy renderujące, które zmniejszają inflacji i koszty renderowanie kontrolki zestawu narzędzi Xamarin.Forms w systemie Android spłaszczając wynikowy hierarchii kontroli natywnych._

Tradycyjnie większość oryginalnego renderowania formantu w systemie Android składają się z dwóch widoków:

- Natywny kontrolować, takich jak `Button` lub `TextView`.
- Kontener `ViewGroup` obsługująca część pracy układu, obsługi gestu i innych zadań.

Jednak to podejście ma domniemanie wydajności, w tym dwa widoki są tworzone dla każdego formantu logicznej, co skutkuje bardziej złożone drzewa wizualnego, który wymaga więcej pamięci i więcej przetwarzania do renderowania na ekranie.

Szybkie programy renderujące skrócić inflacji i koszty renderowanie kontrolki zestawu narzędzi Xamarin.Forms w jednym widoku. W związku z tym zamiast tworzenia dwa widoki, a następnie dodanie ich do drzewa widoku, utworzono tylko jeden. Poprawia to wydajność, tworząc mniej obiektów, które z kolei oznacza mniej złożone drzewa widoku, a wykorzystanie pamięci, (który również powoduje mniej wstrzymuje kolekcji wyrzucania elementów).

Szybkie programy renderujące są dostępne w następujących kontrolkach w Xamarin.Forms 2.4 w systemie Android:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)

Funkcjonalnie te szybkie programy renderujące są nie różni się do oryginalnego programy renderujące. Jednak są obecnie eksperymentalne i można używać tylko, dodając następujący wiersz kodu, aby Twoje `MainActivity` klasy przed wywołaniem `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> Szybkie programy renderujące dotyczą tylko aplikacji zgodności dla systemu Android wewnętrznej bazy danych, więc tego ustawienia zostaną zignorowane w przypadku działań compat pre-app.

Ulepszenia wydajności będą się różnić dla poszczególnych aplikacji, w zależności od złożoności układu. Na przykład, udoskonalenia wydajności x2 są możliwe, podczas przewijania [ `ListView` ](xref:Xamarin.Forms.ListView) zawierający tysiące wierszy danych, w którym komórek w każdym wierszu składają się z formantów, które używają szybkie programy renderujące, co powoduje w sposób widoczny gładsze przewijania.


## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe programy renderujące](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
