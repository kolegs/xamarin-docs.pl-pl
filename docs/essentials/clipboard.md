---
title: 'Xamarin.Essentials: Schowka'
description: W tym dokumencie opisano klasy Schowka w Xamarin.Essentials, które umożliwia kopiowanie i wklejanie tekstu do schowka systemowego między aplikacjami.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38842618"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: Schowka

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Schowka** klasa pozwala skopiować i wkleić tekst do schowka systemowego między aplikacjami.

## <a name="using-clipboard"></a>Korzystanie ze Schowka

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Aby sprawdzić, czy **Schowka** zawiera tekst jest obecnie gotowy do wklejenia:

```csharp
var hasText = Clipboard.HasText;
```

Aby ustawić tekst **Schowka**:

```csharp
Clipboard.SetText("Hello World");
```

Do odczytywania tekstu z **Schowka**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>interfejs API

- [Kod źródłowy Schowka](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Dokumentacja interfejsu API schowka](xref:Xamarin.Essentials.Clipboard)
