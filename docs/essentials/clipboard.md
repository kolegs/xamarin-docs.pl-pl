---
title: 'Xamarin.Essentials: Schowka'
description: W tym dokumencie opisano klasy Schowka w Xamarin.Essentials, która pozwala skopiować i wkleić tekst do schowka systemowego między aplikacjami.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782362"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: Schowka

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
Clipboard.SetText("Hello World");
```

Można odczytać tekstu z **Schowka**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>interfejs API

- [Kod źródłowy Schowka](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Dokumentacja interfejsu API schowka](xref:Xamarin.Essentials.Clipboard)
