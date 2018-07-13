---
title: Efekty zestawu narzędzi Xamarin.Forms
description: Efekty umożliwiają natywne kontrolki na każdej z platform można dostosować bez konieczności uciekania się do wdrażania niestandardowego modułu renderowania.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 9d859377c40c6fca07e140c50da46d8f30aaae04
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994461"
---
# <a name="xamarinforms-effects"></a>Efekty zestawu narzędzi Xamarin.Forms

_Interfejsy użytkownika Xamarin.Forms są renderowane przy użyciu natywnych kontrolek platformę docelową, umożliwiając aplikacjach Xamarin.Forms zachować odpowiedni wygląd i działanie dla każdej platformy. Efekty umożliwiają natywne kontrolki na każdej z platform można dostosować bez konieczności uciekania się do wdrażania niestandardowego modułu renderowania._

## <a name="introduction-to-effectsintroductionmd"></a>[Wprowadzenie do efektów](introduction.md)

Efekty Zezwalaj natywne kontrolki na każdej z platform można dostosować i są zwykle używane do zmiany stylów małe. Ten artykuł zawiera wprowadzenie do efektów, przedstawia granic między efekty i niestandardowe programy renderujące i opisano `PlatformEffect` klasy.

## <a name="creating-an-effectcreatingmd"></a>[Tworzenie efektu](creating.md)

Efekty uprościć Dostosowywanie formantu. W tym artykule pokazano, jak utworzyć efekt, który zmienia kolor tła [ `Entry` ](xref:Xamarin.Forms.Entry) kontroli, gdy formant uzyskuje fokus.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[Przekazywanie parametrów do efektu](passing-parameters/index.md)

Tworzenie efektu, który jest skonfigurowany za pomocą parametrów umożliwia efekt ma być ponownie używane. Następujące artykuły pokazują korzystanie z właściwości w celu przekazania parametrów do efektu i zmieniając parametr w czasie wykonywania.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[Wywoływanie zdarzeń od efektu](touch-tracking.md)

Efekty może wywoływać zdarzenia. W tym artykule pokazano, jak utworzyć zdarzenia, który implementuje śledzenia niskiego poziomu finger wielodotyku i sygnały aplikację touch naciśnięcia, przenoszone i wersji.
