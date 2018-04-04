---
title: Wprowadzenie do zachowania
description: Zachowania umożliwiają dodawanie funkcji do formantów interfejsu użytkownika bez konieczności podklasy je. Zamiast tego funkcji jest zaimplementowana w klasie zachowania i dołączonej do formantu, jakby było częścią samego formantu. Ten artykuł zawiera wprowadzenie do zachowania.
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: b5aa0d3de7092ac87d511ab8d59c329471fa6a28
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-behaviors"></a>Wprowadzenie do zachowania

_Zachowania umożliwiają dodawanie funkcji do formantów interfejsu użytkownika bez konieczności podklasy je. Zamiast tego funkcji jest zaimplementowana w klasie zachowania i dołączonej do formantu, jakby było częścią samego formantu. Ten artykuł zawiera wprowadzenie do zachowania._

Zachowania służą do implementowania kodu, który zwykle należy zapisać jako kodem, ponieważ bezpośrednio prowadzi interakcję z interfejsem API formantu w taki sposób, że można go zwięzłym dołączonej do formantu i pakiecie w celu ponownego wykorzystania przez więcej niż jedną aplikację. Mogą one służyć do zapewnienia pełnego zakresu funkcjonalności do formantów, takich jak:

- Dodanie modułu sprawdzania poczty e-mail do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/).
- Tworzenie formantu klasyfikacji za pomocą naciśnij aparat rozpoznawania gestów.
- Kontrolowanie animacji.
- Dodanie do formantu.

Zachowania również włączyć bardziej zaawansowanych scenariuszy. W kontekście *droższe*, zachowania są przydatne podejście do łączenia formantu do polecenia. Ponadto one może służyć do skojarzenia z formantami, które nie zostały zaprojektowane do interakcji z poleceniami poleceń. Na przykład one może służyć do wywołania polecenia w odpowiedzi na zdarzenie wywołujące.

Platformy Xamarin.Forms obsługuje dwóch różnych stylów zachowania:

- **Zachowania platformy Xamarin.Forms** — klas, które pochodzą z [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) lub [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) klasy, gdzie `T` jest typ kontroli, do którego zachowanie należy zastosować. Aby uzyskać więcej informacji na temat platformy Xamarin.Forms zachowania, zobacz [zachowania platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md) i [zachowania wielokrotnego użytku](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).
- **Dołączony zachowania** — `static` klas z co najmniej jednego z dołączonych właściwości. Aby uzyskać więcej informacji na temat zachowania dołączone zobacz [dołączony zachowania](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

Ten przewodnik koncentruje się na platformy Xamarin.Forms zachowania, ponieważ są to preferowane rozwiązanie do budowy zachowanie.



## <a name="related-links"></a>Linki pokrewne

- [Behavior](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Zachowanie<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
