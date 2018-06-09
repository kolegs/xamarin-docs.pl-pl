---
title: Efekty platformy Xamarin.Forms
description: Efekty Zezwalaj kontrolki natywne na każdej z platform do dostosowania bez potrzeby odwołać się do wdrażania niestandardowego modułu renderowania.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: bc1c600aa6d5ede079fe0af83370393d4f0e298f
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239699"
---
# <a name="xamarinforms-effects"></a>Efekty platformy Xamarin.Forms

_Interfejsy użytkownika platformy Xamarin.Forms są renderowane przy użyciu kontrolki natywne platformy docelowej, umożliwiając aplikacji platformy Xamarin.Forms zachować odpowiedni wyglądu i działania dotyczące każdej platformy. Efekty Zezwalaj kontrolki natywne na każdej z platform do dostosowania bez potrzeby odwołać się do wdrażania niestandardowego modułu renderowania._

## <a name="introduction-to-effectsintroductionmd"></a>[Wprowadzenie do efekty](introduction.md)

Efekty Zezwalaj kontrolki natywne każdej platformy można dostosować i są zwykle używane do zmian małych style. Ten artykuł zawiera wprowadzenie do efekty, przedstawia granicę między efekty i niestandardowe moduły renderowania i opisuje `PlatformEffect` klasy.

## <a name="creating-an-effectcreatingmd"></a>[Tworzenie efektu](creating.md)

Efekty uprościć Dostosowywanie formantu. W tym artykule przedstawiono sposób tworzenia efekt, który zmienia kolor tła [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kontroli, gdy formant uzyska fokus.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[Przekazywanie parametrów w celu](passing-parameters/index.md)

Tworzenie efektu, które są konfigurowane za pomocą parametrów umożliwia efekt zostanie ponownie. Te artykuły prezentacja przy użyciu właściwości do przekazania parametrów w celu i zmianę parametru w czasie wykonywania.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[Wywoływanie zdarzeń z efektu](touch-tracking.md)

Skutki mogą wywoływać zdarzeń. W tym artykule przedstawiono sposób tworzenia zdarzeń, które implementuje śledzenia niskiego poziomu palca wielodotyku i zasygnalizuje aplikacji touch naciśnięcie, przenoszenie i wersje.
