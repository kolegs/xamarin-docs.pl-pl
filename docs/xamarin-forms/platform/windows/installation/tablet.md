---
title: Dodawanie aplikacji dla systemu Windows
ms.topic: article
ms.prod: xamarin
ms.assetid: ADF99B78-F1BC-48DF-9128-01B93C4411C1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 369e8005c1d398c90b14a95ac36fc8659e462fbf
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="adding-a-windows-app"></a>Dodawanie aplikacji dla systemu Windows


1. **Kliknij prawym przyciskiem myszy na rozwiązanie > Dodaj > Nowy projekt...**  i Dodaj **pusta aplikacja (Windows)**

 ![](tablet-images/add-wu.png "")

2. **Kliknij prawym przyciskiem myszy na Projekt > Zarządzaj pakietami NuGet...**  i Dodaj **platformy Xamarin.Forms** pakietu.

3. **Kliknij prawym przyciskiem myszy na Projekt > Dodaj > odwołania** i utworzyć odwołanie projektu do projektu udostępnionego aplikacji platformy Xamarin.Forms.

  ![](tablet-images/addref.png "")

4. Edytuj **App.xaml.cs** uwzględnienie `Init()` wywołanie metody wewnątrz `OnLaunched` metody w pobliżu wiersza 65

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . Edytuj **MainPage.xaml** — Zmień element główny `<Page` do `<forms:WindowsPage` *i* zdefiniować `xmlns:forms` używa:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPage>
```


 6 . Edytuj **MainPage.xaml.cs** usunąć `: Page` specyfikator dziedziczenia nazwy klasy.

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 7 . Nadal **MainPage.xaml.cs**, Dodaj `LoadApplication` wywołanie w `MainPage` — Konstruktor (około wiersza 28) można uruchomić aplikacji platformy Xamarin.Forms:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . Kliknij dwukrotnie **Package.appxmanifest** do tych funkcji, które są często wymagane ustawienia:

  Zestaw możliwości:

  * Internet (klient)
  * Lokalizacja

9 . Na koniec należy dodać wszystkie zasoby lokalne (np.) pliki obrazów) istniejących projektach platformy, które są wymagane.

