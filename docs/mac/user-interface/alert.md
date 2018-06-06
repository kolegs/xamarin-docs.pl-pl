---
title: Alerty w Xamarin.Mac
description: W tym artykule omówiono Praca z alertami w aplikacji Xamarin.Mac. Opisuje tworzenie i wyświetlanie alertów z kodu C# i odpowiada na żądania interakcji użytkownika.
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 1eb781fe02213de6a994f56e321316b93a128b60
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792645"
---
# <a name="alerts-in-xamarinmac"></a>Alerty w Xamarin.Mac

_W tym artykule omówiono Praca z alertami w aplikacji Xamarin.Mac. Opisuje tworzenie i wyświetlanie alertów z kodu C# i odpowiada na żądania interakcji użytkownika._

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tej samej alerty który używająca *Objective-C* i *Xcode* jest. 

Alert jest specjalnym rodzajem okna dialogowego, który jest wyświetlany, gdy występuje poważny problem (np. Błąd) lub jako ostrzeżenie (takich jak przygotowywanie do usunięcia pliku). Ponieważ alert jest okno dialogowe, wymagany jest również odpowiedzi użytkownika przed jego zamknięciem.

[![](alert-images/alert06.png "Przykład alertu")](alert-images/alert06.png#lightbox)

W tym artykule omówione zostaną następujące czynności podstawowe informacje dotyczące pracy z alertami w aplikacji Xamarin.Mac. 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>Wprowadzenie do alertów

Alert jest specjalnym rodzajem okna dialogowego, który jest wyświetlany, gdy występuje poważny problem (np. Błąd) lub jako ostrzeżenie (takich jak przygotowywanie do usunięcia pliku). Ponieważ alerty zakłócać użytkownika, ponieważ musi być ukryty, zanim użytkownik może kontynuować ich zadań, należy unikać wyświetlanie alert, jeśli jest to bezwzględnie konieczne.

Apple Sugeruj następujących wytycznych:

- Nie należy używać jedynie, aby udostępnić użytkownikom informacje alertu.
- Nie wyświetlaj alert w przypadku typowych działań można cofnąć. Nawet jeśli taka sytuacja może spowodować utratę danych.
- Jeśli sytuacja jest warta alert, należy unikać przy użyciu innego elementu interfejsu użytkownika lub metody, aby go wyświetlić.
- Ikonę ostrzeżenia powinny być używane rzadko.
- Opis alertu sytuacji wyraźnie i pełniej w komunikacie alertu.
- Nazwa przycisku domyślne powinny odpowiadać akcję, którą opisano w wiadomości alertu.

Aby uzyskać więcej informacji, zobacz [alerty](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1) sekcji firmy Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>Anatomia alertu

Jak już wspomniano, alerty powinny być wyświetlane w aplikacji użytkownika w przypadku wystąpienia poważnego problemu lub jako ostrzeżenie utracie danych (np. zamknięcie niezapisanym pliku). W Xamarin.Mac alert jest tworzony w kodzie języka C#, na przykład:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Critical,
    InformativeText = "We need to save the document here...",
    MessageText = "Save Document",
};
alert.RunModal ();
```

Powyższy kod wyświetla alert z ikoną aplikacji na ikonę ostrzeżenia, tytuł, ostrzeżenie i jeden **OK** przycisk:

[![](alert-images/alert01.png "Alert o przycisku OK")](alert-images/alert01.png#lightbox)

Apple zapewnia kilka właściwości, które mogą służyć do dostosowywania alertu:

- **AlertStyle** definiuje typ alertu jako jedną z następujących czynności:
    - **Ostrzeżenie** — używane w celu ostrzeżenia użytkownika bieżącego lub zbliżającym się zdarzenia, który nie jest krytyczna. Jest to domyślny styl.
    - **Informacyjny** — umożliwia ostrzega użytkownika o bieżących lub zbliżającym się zdarzeniu. Obecnie nie ma żadnej widocznej różnicy między **ostrzeżenie** i **informacyjny**
    - **Krytyczne** — używane, aby ostrzec użytkownika o poważne skutki o zbliżającym się zdarzeniu (np. usunięcie pliku). Ten typ alertu powinny być używane rzadko.
- **MessageText** -to jest komunikat głównego lub tytuł alertu i szybko należy zdefiniować sytuacji użytkownikowi.
- **InformativeText** -to jest treść alertu, w którym należy wyraźnie zdefiniować sytuacji i prezentowania opcji możliwego do użytkownika.
- **Ikona** — umożliwia ikon niestandardowych, który będzie wyświetlany użytkownikowi.
- **HelpAnchor** & **ShowsHelp** — umożliwia alert ograniczeni do aplikacji HelpBook i wyświetlić Pomoc dla alertu.
- **Przyciski** — domyślnie zawiera tylko alert **OK** przycisk ale **przyciski** kolekcji można dodawać więcej opcji zgodnie z potrzebami.
- **ShowsSuppressionButton** — Jeśli `true` Wyświetla pole wyboru, które użytkownik może użyć do pomijania alertu dla kolejnych wystąpień zdarzenie, które go.
- **AccessoryView** — umożliwia dołączanie do alertu, aby zapewnić dodatkowe informacje, takie jak dodanie inny widok podrzędny **pola tekstowego** do wprowadzania danych. Jeśli ustawisz nowy **AccessoryView** lub zmodyfikować istniejący, należy wywołać `Layout()` metodę, aby dopasować widoczne układ alertu.

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>Wyświetlanie alertu

Istnieją dwa różne sposoby, że alert może zostać wyświetlone, swobodnego lub jako arkusza. Poniższy kod wyświetla alert jako swobodnego:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.RunModal ();
```
Jeśli ten kod jest uruchamiana, wyświetlane są następujące:

[![](alert-images/alert02.png "Proste alertu")](alert-images/alert02.png#lightbox)

Poniższy kod wyświetla ten sam alert jako arkusz:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

Jeśli ten kod jest uruchamiana, poniżej zostanie wyświetlony:

[![](alert-images/alert03.png "Alert wyświetlany jako arkusz")](alert-images/alert03.png#lightbox)


<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>Praca z przycisków alertu

Domyślnie alertu są wyświetlane tylko **OK** przycisku. Jednak możesz nie są ograniczone do, możesz utworzyć dodatkowe przyciski przez dodanie ich do **przyciski** kolekcji. Poniższy kod tworzy swobodnego alertu o **OK**, **anulować** i **może być** przycisk:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
var result = alert.RunModal ();
```

Przycisk pierwszego dodane będzie _przycisk domyślny_ który zostanie aktywowany, gdy użytkownik naciśnie klawisz Enter. Zwrócona wartość będzie całkowitą reprezentującą, który przycisk zostanie naciśnięty. W tym przypadku zostaną zwrócone następujące wartości:

- **OK** — 1000.
- **Anuluj** — 1001.
- **Może być** — 1002.

Jeśli firma Microsoft wykonywania kodu poniżej będą wyświetlane:

[![](alert-images/alert04.png "Alert z trzech opcji przycisku")](alert-images/alert04.png#lightbox)

Oto kod dla tego samego alertu jako arkusz:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}", result);
});
```
Jeśli ten kod jest uruchamiana, poniżej zostanie wyświetlony:

[![](alert-images/alert05.png "W przypadku wystąpienia alertu trzy przycisk wyświetlany jako arkusz")](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> Nigdy nie należy dodawać więcej niż trzy przyciski do alertu.

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>Przedstawiający przycisk Pomiń

Jeśli Alert `ShowSuppressButton` właściwość jest `true`, pole wyboru, które użytkownik może użyć do pomijania alertu dla kolejnych wystąpień zdarzenie, które on wyświetla alert. Poniższy kod przedstawia swobodnego alert z przyciskiem Pomiń:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Jeśli wartość `alert.SuppressionButton.State` jest `NSCellStateValue.On`, użytkownik ma zaznaczone pole wyboru Pomiń, #else nie ma.

Jeśli kod jest uruchamiana, poniżej zostanie wyświetlony:

[![](alert-images/alert06.png "Alert o przycisk Pomiń")](alert-images/alert06.png#lightbox)

Oto kod dla tego samego alertu jako arkusz:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Jeśli ten kod jest uruchamiana, poniżej zostanie wyświetlony:

[![](alert-images/alert07.png "Wyświetl alert z przyciskiem Pomiń jako arkusz")](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>Dodawanie niestandardowych widok podrzędny

Alerty mają `AccessoryView` właściwość, która może służyć do dostosowywania dalsze alert i dodać takie operacje, jak **pola tekstowego** dla danych wejściowych użytkownika. Poniższy kod tworzy swobodnego alert z polem tekstowym dodany:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
AlertStyle = NSAlertStyle.Informational,
InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

W tym miejscu klucza wiersze są `var input = new NSTextField (new CGRect (0, 0, 300, 20));` która tworzy nowy **pola tekstowego** czy dodamy alertu. `alert.AccessoryView = input;` które dołącza **pola tekstowego** alert i wywołanie `Layout()` metodę, która jest wymagana, aby zmienić rozmiar alertu, aby zmieścić ją w nowy widok podrzędny.

Jeśli firma Microsoft wykonywania kodu poniżej będą wyświetlane:

[![](alert-images/alert08.png "Jeśli przeprowadzana kod, poniżej zostanie wyświetlony")](alert-images/alert08.png#lightbox)

W tym miejscu jest ten sam alert jako arkusz:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Jeśli firma Microsoft uruchomić ten kod, poniżej zostanie wyświetlony:

[![](alert-images/alert09.png "Alert o widok niestandardowy")](alert-images/alert09.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się pracy z alertami w aplikacji Xamarin.Mac. Widzieliśmy różne typy i używa alertów, jak utworzyć i dostosować alerty i sposobu pracy z alertami w kodzie języka C#.

## <a name="related-links"></a>Linki pokrewne

- [MacWindows (przykład)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Praca z systemu Windows](~/mac/user-interface/window.md)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do systemu Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [NSAlert](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
