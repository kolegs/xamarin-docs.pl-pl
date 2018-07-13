---
title: Wprowadzenie do zachowania
description: Zachowania umożliwiają dodawanie funkcji do kontrolek interfejsu użytkownika bez konieczności podklasy je. Zamiast tego funkcje jest zaimplementowana w klasie zachowanie i dołączone do formantu, tak, jakby były one częścią sama kontrolka. Ten artykuł zawiera wprowadzenie do zachowania.
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 176f41d4b7349af2cf7cc49de8ba0789ad2f8c11
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995817"
---
# <a name="introduction-to-behaviors"></a>Wprowadzenie do zachowania

_Zachowania umożliwiają dodawanie funkcji do kontrolek interfejsu użytkownika bez konieczności podklasy je. Zamiast tego funkcje jest zaimplementowana w klasie zachowanie i dołączone do formantu, tak, jakby były one częścią sama kontrolka. Ten artykuł zawiera wprowadzenie do zachowania._

Zachowania umożliwiają wdrożenie kodu, który będzie zazwyczaj trzeba napisać jako związany z kodem, ponieważ współpracuje bezpośrednio z interfejsem API formantu w taki sposób, że można go zwięźle dołączonej do formantu i spakowane do ponownego wykorzystania w więcej niż jedną aplikację. Mogą one służyć do zapewniają pełną gamę funkcji do kontrolek, takich jak:

- Dodawanie weryfikacji wiadomości e-mail [ `Entry` ](xref:Xamarin.Forms.Entry).
- Tworzenie kontrolki rating przy użyciu wzorca tap aparat rozpoznawania gestów.
- Kontrolowanie animacji.
- Dodawanie efektu do kontrolki.

Zachowania również włączyć bardziej zaawansowanych scenariuszy. W kontekście *polecenia*, zachowania są przydatne do łączenia z kontrolki do polecenia. Ponadto one może służyć do skojarzenia poleceń z kontrolkami, które nie zostały zaprojektowane do interakcji z poleceniami. Na przykład ich może służyć do wywołania polecenia w odpowiedzi na zdarzenie.

Zestaw narzędzi Xamarin.Forms obsługuje dwa różne style zachowania:

- **Zachowania zestawu narzędzi Xamarin.Forms** — klas, które wynikają z [ `Behavior` ](xref:Xamarin.Forms.Behavior) lub [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) klasy, gdzie `T` typu formantu, do którego jest zachowanie należy zastosować. Aby uzyskać więcej informacji na temat zachowania zestawu narzędzi Xamarin.Forms, zobacz [zachowania zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md) i [zachowania wielokrotnego użytku, do](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).
- **Dołączone zachowania** — `static` klas z jedną lub więcej właściwości dołączone. Aby uzyskać więcej informacji na temat dołączone zachowania, zobacz [dołączone zachowania](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

Ten przewodnik koncentruje się na zachowania zestawu narzędzi Xamarin.Forms, ponieważ są one jest preferowanym podejściem do budowy zachowanie.



## <a name="related-links"></a>Linki pokrewne

- [Behavior](xref:Xamarin.Forms.Behavior)
- [Zachowanie&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
