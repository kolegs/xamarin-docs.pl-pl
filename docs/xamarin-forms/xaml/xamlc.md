---
title: Kompilacja XAML w interfejsie Xamarin.Forms
description: W tym artykule wyjaśniono, jak XAML może być opcjonalnie kompilowane bezpośrednio do języka pośredniego (IL) za pomocą kompilatora XAML zestawu narzędzi Xamarin.Forms (XAMLC).
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/02/2018
ms.openlocfilehash: b828e62ef1037bf47a2ae5fb303fbf8fcace6549
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997136"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Kompilacja XAML w interfejsie Xamarin.Forms

_XAML może być opcjonalnie kompilowane bezpośrednio do języka pośredniego (IL) za pomocą kompilatora XAML (XAMLC)._

XAMLC oferuje wiele korzyści:

- Wykonuje sprawdzanie kompilacji XAML, powiadamiania użytkownika o błędach.
- Usuwa pewien czas ładowania i wystąpienia elementów XAML.
- Pomaga zmniejszyć rozmiar pliku ostateczny zestaw przy nie jest już w tym pliki .xaml.

XAMLC jest domyślnie wyłączona, aby zapewnić zgodność wstecz. Można ją włączyć na poziomie zestawu, jak i klasy, dodając `XamlCompilation` atrybutu.

Poniższy przykład kodu pokazuje, umożliwiając XAMLC na poziomie zestawu:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

W tym przykładzie kompilacji sprawdzanie wszystkich XAML, które są zawarte w zestawie odbędzie się z błędami XAML zgłaszane w czasie kompilacji, a nie czasu wykonywania. W związku z tym `assembly` prefiks `XamlCompilation` atrybut określa, że ten atrybut ma zastosowanie do całego zestawu.

Poniższy przykład kodu pokazuje, umożliwiając XAMLC na poziomie klasy:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

W tym przykładzie, w czasie kompilacji sprawdzania XAML dla `HomePage` klasy zostanie wykonany, a błędy raportowane jako część procesu kompilacji.

> [!NOTE]
> `XamlCompilation` Atrybutu i `XamlCompilationOptions` wyliczenie znajdują się w `Xamarin.Forms.Xaml` przestrzeni nazw, które muszą być importowane z nich korzystać.


## <a name="related-links"></a>Linki pokrewne

- [XamlCompilation](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [XamlCompilationOptions](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
