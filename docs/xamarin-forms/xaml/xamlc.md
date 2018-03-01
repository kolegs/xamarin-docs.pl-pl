---
title: Kompilacja XAML
description: "XAML mogą być opcjonalnie kompilowane bezpośrednio na język pośredni (IL) z kompilatora XAML (XAMLC)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/21/2016
ms.openlocfilehash: 3afb7608838a2c34f143d0563b50f03ad7f6ecf4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-compilation"></a>Kompilacja XAML

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
> **Uwaga**: `XamlCompilation` atrybutu i `XamlCompilationOptions` wyliczenie znajdują się w `Xamarin.Forms.Xaml0` przestrzeni nazw, który musi być importowany z nich korzystać.


## <a name="related-links"></a>Linki pokrewne

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
