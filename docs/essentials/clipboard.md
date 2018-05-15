---
title: Xamarin.Essentials Schowka
description: Klasa Schowka pozwala skopiować i wkleić tekst do schowka systemowego między aplikacjami.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 67a0218325918b57e5ed2618b57d52d3fe3ee820
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials Schowka

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Schowka** klasa umożliwia kopiowanie i wklejanie tekstu do schowka systemowego między aplikacjami.

## <a name="using-clipboard"></a>Przy użyciu Schowka

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Aby sprawdzić, czy **Schowka** ma obecnie gotowe można było ją wkleić tekst:

```csharp
var hasText = Clipboard.HasText;
```

Aby ustawić tekst **Schowka**:

```csharp
ClipBoard.SetText("Hello World");
```

Można odczytać tekstu z **Schowka**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>interfejs API

- [Kod źródłowy Schowka](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Dokumentacja interfejsu API schowka](xref:Xamarin.Essentials.Clipboard)
