---
title: Modyfikatorów pól XAML w platformy Xamarin.Forms
description: 'X: fieldmodifier — atrybut namespace określa poziom dostępu dla wygenerowanego pól nazwanych elementów XAML.'
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 8be56524ec1c5331f30418fcc29a4bd2c26ccde1
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209495"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Modyfikatorów pól XAML w platformy Xamarin.Forms

_`x:FieldModifier` Atrybutu namespace określa poziom dostępu dla wygenerowanego pól nazwanych elementów XAML._

## <a name="overview"></a>Omówienie

Prawidłowe wartości atrybutu to:

- `Public` — Określa, czy pola wygenerowane dla elementu XAML jest `public`.
- `NotPublic` — Określa, czy pola wygenerowane dla elementu XAML jest `internal` do zestawu.

Wartość atrybutu nie jest ustawiona, zostanie wygenerowane pole elementu `private`.

Wymaga spełnienia następujących warunków `x:FieldModifier` atrybut do przetworzenia:

- Element najwyższego poziomu XAML musi być prawidłowym `x:Class`.
- Bieżący element XAML ma `x:Name` określony.

Następujące XAML znajdują się przykładowe ustawienie atrybutu:

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> `x:FieldModifier` Nie można użyć atrybutu, aby określić poziom dostępu do klasy XAML.
