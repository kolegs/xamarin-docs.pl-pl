---
title: Kompilacja XAML w platformy Xamarin.Forms
description: W tym artykule opisano, jak XAML mogą być opcjonalnie kompilowane bezpośrednio na język pośredni (IL) z kompilatora XAML platformy Xamarin.Forms (XAMLC).
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/21/2016
ms.openlocfilehash: 0128fecebe9f6ba8f55e965a8fa65787d03d9ded
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245748"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Kompilacja XAML w platformy Xamarin.Forms

_XAML mogą być opcjonalnie kompilowane bezpośrednio na język pośredni (IL) z kompilatora XAML (XAMLC)._

XAMLC oferuje wiele korzyści:

- Wykonuje sprawdzanie kompilacji XAML powiadomienia użytkownika o błędach.
- Usuwa niektóre godziny obciążenia i konkretyzacji elementów XAML.
- Pomaga zmniejszyć rozmiar pliku w zestawie końcowym poprzez nie jest już w tym pliki .xaml.

XAMLC jest domyślnie wyłączona, aby zapewnić zgodność wstecz. Można go włączyć na poziomie zestawu i klasy, dodając `XamlCompilation` atrybutu.

Poniższy przykład kodu pokazuje, włączanie XAMLC na poziomie zestawu:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

W tym przykładzie kompilacji sprawdzanie wszystkich XAML zawartych w `PhotoApp` przestrzeni nazw zostanie wykonane XAML błędów zgłaszane w kompilacji, a nie w czasie wykonywania.
`assembly` Prefiks `XamlCompilation` atrybut określa, że ten atrybut ma zastosowanie do całego zestawu.

Poniższy przykład kodu pokazuje, włączanie XAMLC na poziomie klasy:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

W tym przykładzie kompilacji XAML dla kontroli `HomePage` klasa zostanie wykonane i błędy raportowane jako część procesu kompilacji.

> [!NOTE]
> `XamlCompilation` Atrybutu i `XamlCompilationOptions` wyliczenie znajdują się w `Xamarin.Forms.Xaml` przestrzeni nazw, który musi być importowany z nich korzystać.


## <a name="related-links"></a>Linki pokrewne

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
