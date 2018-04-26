---
title: Dodawanie aplikacji Windows Phone
ms.prod: xamarin
ms.assetid: B598FA9D-6818-4CC9-8191-838C156DB9DA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 55bd4bdcfde4c91ad5c9b94bef486207466e135d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-windows-phone-app"></a>Dodawanie aplikacji Windows Phone


Po pierwsze, jeśli używasz szablonu PCL platformy Xamarin.Forms [zaktualizować profil](~/xamarin-forms/platform/windows/installation/index.md), postępuj zgodnie z instrukcjami poniżej:

1. **Kliknij prawym przyciskiem myszy na rozwiązanie > Dodaj > Nowy projekt...**  i Dodaj **pusta aplikacja (Windows Phone)**

  ![](phone-images/add-wp81.png "Dodaj okno dialogowe nowego projektu")

2. **Kliknij prawym przyciskiem myszy na nowo utworzonego projektu > Zarządzaj pakietami NuGet...**  i Dodaj **platformy Xamarin.Forms** pakietu.

3. **Kliknij prawym przyciskiem myszy na Projekt > Dodaj > odwołania** i utworzyć odwołanie projektu do projektu udostępnionego aplikacji platformy Xamarin.Forms.

  ![](phone-images/addref.png "Okno dialogowe menedżera odwołań")

4. Edytuj **App.xaml.cs** uwzględnienie `Init()` wywołanie metody, `OnLaunched` metody w pobliżu wiersza 67:

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . Edytuj **MainPage.xaml** — Zmień element główny `<Page` do `<forms:WindowsPhonePage` *i* zdefiniować `xmlns:forms` używa:

```xaml
<forms:WindowsPhonePage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPhonePage>
```

 6 . Edytuj **MainPage.xaml.cs** usunąć `: PhonePage` specyfikator dziedziczenia nazwy klasy.

```csharp
public sealed partial class MainPage  // REMOVE ": PhonePage"
```

 7 . Nadal **MainPage.xaml.cs**, Dodaj `LoadApplication` wywołanie w `MainPage` — Konstruktor (około wiersza 28) można uruchomić aplikacji platformy Xamarin.Forms:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . Kliknij dwukrotnie **Package.appxmanifest** do tych funkcji, które są często wymagane ustawienia:

  * Internet (klient i serwer)

9 . Na koniec należy dodać wszystkie zasoby lokalne (np.) pliki obrazów) istniejących projektach platformy, które są wymagane.

