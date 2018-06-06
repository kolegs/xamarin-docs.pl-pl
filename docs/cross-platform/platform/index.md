---
title: Programowanie obsługi języków w programie Xamarin
description: 'Ten dokument zawiera opis różnych języków programowania, obsługiwane przez program Xamarin. Zawarto informacje C#, F #, przenośne języku Visual i szablony Razor.'
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 715f63a0be54ba3342bd63c1c76d89656313359a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781679"
---
# <a name="programming-language-support-in-xamarin"></a>Programowanie obsługi języków w programie Xamarin

## <a name="c"></a>C# 

###  <a name="async-support-overviewcross-platformplatformasyncmd"></a>[Asynchroniczna pomoc techniczna — omówienie](~/cross-platform/platform/async.md)

C# w wersji 5 wprowadzone dwóch nowych słów kluczowych express operacji asynchronicznych: async i await. Słowa kluczowe umożliwiają pisania prostych kodu, który korzysta z biblioteki równoległych zadań do wykonania długotrwałe operacje (np. dostępu do sieci) w innym wątku i łatwo uzyskiwać dostęp do wyników po zakończeniu. Najnowsze wersje platformy Xamarin.iOS i platformy Xamarin.Android obsługuje async i await — ten dokument zawiera wyjaśnienia dotyczące i przykład za pomocą nowej składni za pomocą platformy Xamarin.

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[Funkcje języka C# 6](~/cross-platform/platform/csharp-six.md)

Najnowsza wersja języka C# — w wersji 6 — w dalszym ciągu rozwijany języka mniej standardowej, zwiększenia przejrzystości i spójności więcej. Składnia inicjalizacji czyszczący, możliwość używania `await` w `catch/finally` bloków i warunkowe null `?` operator są szczególnie użyteczne.

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

Tworzenie aplikacji mobilnych z języka F # i Xamarin.

##  <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[Portable Visual Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Program Visual Studio obsługuje tworzenie przenośnej biblioteki klas przy użyciu Visual języku, który następnie może być włączone w aplikacji platformy Xamarin. W tym artykule przedstawiono sposób tworzenia nowego PCL Visual Basic, a następnie użyć go w przykładowej aplikacji platformy Xamarin.iOS, Xamarin.Android i Windows Phone.

##  <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Tworzenie widoków HTML za pomocą szablonów Razor](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin umożliwia deweloperom korzystać z aparatu Razor tworzenia szablonów, pierwotnie została wprowadzona z platformą ASP.NET MVC, wraz z języka C# do łatwo połączyć dane z kodu HTML, Javascript i CSS bez proste ręcznego tworzenia ciągów HTML w kodzie.
W tym artykule przedstawiono sposób użycia szablonów Razor za pomocą platformy Xamarin dla systemów Android i iOS.
