---
title: Przeciągnij i upuść w Xamarin.iOS
description: Ten dokument zawiera opis sposobu implementacji przeciągania i upuszczania w aplikacji platformy Xamarin.iOS przy użyciu interfejsów API, wprowadzone w systemie iOS 11. W szczególności omówiono włączanie w UITableView przeciągania i upuszczania.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/05/2016
ms.openlocfilehash: 7c41f96dae88047e64ec1e74838e3efab55958cc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786967"
---
# <a name="drag-and-drop-in-xamarinios"></a>Przeciągnij i upuść w Xamarin.iOS

_Implementowanie przeciągania i upuszczania dla systemu iOS 11_

iOS 11 obejmuje przeciągania i upuszczania pomocy technicznej, aby skopiować dane między aplikacjami na tabletów iPad. Użytkownicy mogą wybrać i przeciągnij wszystkie typy zawartości z aplikacji znajduje się obok siebie lub przeciągając ikonie aplikacji, które wyzwoli aplikacji, aby otworzyć i umożliwić danych można usunąć:

![Przeciągnij i upuść przykład z niestandardowych aplikacji na informacje o aplikacji](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> Przeciąganie i upuszczanie jest dostępna tylko w tej samej aplikacji na telefonie iPhone.

Należy wziąć pod uwagę Obsługa przeciągania i upuszczania operacji dowolnej lokalizacji zawartości można utworzyć lub edytować:

- Tekst kontrolki przeciągania i upuszczania obsługę wszystkie aplikacje dla systemu iOS 11, bez konieczności wykonywania dodatkowych działań.
- Widoki tabel i widoków kolekcji obejmują ulepszenia w systemie iOS 11, który uprościć dodawanie przeciągania i upuszczania zachowanie.
- Inne widoki może się do obsługi przeciągania i upuszczania z dodatkowe dostosowanie.

Podczas dodawania przeciąganie i upuszczanie obsługuje do aplikacji, zapewniają różne poziomy zawartości wierności; na przykład można określić zarówno tekst sformatowany i zwykły tekst wersji danych tak, aby aplikacja odbierająca można wybrać, który najlepiej jest dopasowywana do docelowy przeciągania. Istnieje również możliwość, aby dostosować wizualizacji przeciągania i Włącz przeciąganie wielu elementów naraz.

## <a name="drag-and-drop-with-text-controls"></a>Przeciągnij i upuść z formantami

`UITextView` i `UITextField` automatycznie obsługują przeciąganie zaznaczonego tekstu wychodzących, a zawartość tekstową.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>Przeciągnij i upuść z UITableView

`UITableView` zawiera wbudowane obsługę przeciągnij i upuść interakcji z wierszami tabeli, wymagających tylko kilka metod umożliwiających zachowanie domyślne.

Obejmuje dwa interfejsy:

- `IUITableViewDragDelegate` — Pakiety informacje po zainicjowaniu przeciągnij w widoku tabeli.
- `IUITableViewDropDelegate` — Przetwarza informacje podczas upuszczania jest w trakcie próby oraz zostać zakończona.

W [próbki DragAndDropTableView](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/) te dwa interfejsy są oba zaimplementowane na `UITableViewController` klasy wraz ze źródła danych i delegata. Przydzieleni w `ViewDidLoad` metody:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

Minimalny wymagany kod dla tych dwóch interfejsów opisanej poniżej.

### <a name="table-view-drag-delegate"></a>Delegat przeciągania widoku tabeli

Jedyną metodą _wymagane_ do obsługi Przeciąganie wiersza z tabeli widoku jest `GetItemsForBeginningDragSession`. Jeśli użytkownik rozpocznie przeciąganie wiersza, ta metoda zostanie wywołana.

Poniżej przedstawiono implementację. Go pobiera dane skojarzone z przeciąganego wiersza koduje go i konfiguruje `NSItemProvider` Określa, jak aplikacje obsłuży "Usuń" część operacji (na przykład, czy można obsługują typu danych `PlainText`, w tym przykładzie):

```csharp
public UIDragItem[] GetItemsForBeginningDragSession (UITableView tableView,
  IUIDragSession session, NSIndexPath indexPath)
{
  // gets the 'information' to be dragged
  var placeName = model.PlaceNames[indexPath.Row];
  // convert to NSData representation
  var data = NSData.FromString(placeName, NSStringEncoding.UTF8);
  // create an NSItemProvider to describe the data
  var itemProvider = new NSItemProvider();
  itemProvider.RegisterDataRepresentation(UTType.PlainText,
                                NSItemProviderRepresentationVisibility.All,
                                (completion) =>
  {
    completion(data, null);
    return null;
  });
  // wrap in a UIDragItem
  return new UIDragItem[] { new UIDragItem(itemProvider) };
}
```

Istnieje wiele metod opcjonalne w delegacie przeciągania, który można zaimplementować, aby dostosować zachowanie przeciągania, takie jak dostarczanie wielu reprezentacje danych, które można podjąć zaletą w aplikacji docelowej (np. tekst sformatowany jako także jako zwykły tekst lub wektora i wersje mapy bitowej rysunku). Można też podać oświadczenia niestandardowe dane do użycia podczas przeciągania i upuszczania w ramach tej samej aplikacji.

### <a name="table-view-drop-delegate"></a>Delegat Porzuć widok tabeli

Metody obiektu delegowanego listy są wywoływane podczas operacji przeciągania odbywa się za pośrednictwem widoku tabeli lub zakończeniu powyżej. Wymaganych metod można określić tego, czy dane może zostać porzucony i jakie akcje są pobierane, jeśli zostało ukończone upuszczania:

- `CanHandleDropSession` — Podczas przeciągania jest w toku, a potencjalnie usuwane w aplikacji, ta metoda określa, czy dane przeciągane może go porzucić.
- `DropSessionDidUpdate` — Podczas przeciągania jest w toku, ta metoda jest wywoływana w celu określenia, jakie działanie ma. Informacje z widoku tabeli przeciągany nad, przeciągnij sesji i ścieżkę możliwe indeksu można używany do określenia zachowania i wizualne podane dla użytkownika.
- `PerformDrop` — Gdy użytkownik kończy listy (podnoszenia ich palca), ta metoda wyodrębnia dane przeciągane i modyfikuje widok tabeli, aby dodać dane w nowym wierszu (lub wierszy).

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` Wskazuje, czy widok tabeli może akceptować dane przeciągane. W poniższym przykładzie `CanLoadObjects` służy do potwierdzenia, że ten widok tabeli może akceptować dane ciągu.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate` Metoda jest wywoływana wielokrotnie podczas operacji przeciągania jest w toku, aby zapewnić podpowiedzi wizualne dla użytkownika.

W kodzie poniżej `HasActiveDrag` służy do określania, czy operacja pochodzi z aktualnego widoku tabeli. Jeśli tak, tylko jeden wiersze są dozwolone do przeniesienia.
W przypadku przeciągania z innego źródła, zostanie wskazany operacji kopiowania:

```csharp
public UITableViewDropProposal DropSessionDidUpdate(UITableView tableView, IUIDropSession session, NSIndexPath destinationIndexPath)
{
  // The UIDropOperation.Move operation is available only for dragging within a single app.
  if (tableView.HasActiveDrag)
  {
    if (session.Items.Length > 1)
    {
        return new UITableViewDropProposal(UIDropOperation.Cancel);
    } else {
        return new UITableViewDropProposal(UIDropOperation.Move, UITableViewDropIntent.InsertAtDestinationIndexPath);
    }
  } else {
    return new UITableViewDropProposal(UIDropOperation.Copy, UITableViewDropIntent.InsertAtDestinationIndexPath);
  }
}
```

Operacja jej porzucenia może być jednym z `Cancel`, `Move`, lub `Copy`.

Próba porzucenia można wstawić nowy wiersz lub Dodaj/dołączanie danych do istniejącego wiersza.

#### <a name="performdrop"></a>PerformDrop

`PerformDrop` Metoda jest wywoływana, gdy użytkownik wykonuje tę operację i modyfikuje źródło widoku, a dane tabeli, aby odzwierciedlić porzuconych danych.

```csharp
public void PerformDrop(UITableView tableView, IUITableViewDropCoordinator coordinator)
{
  NSIndexPath indexPath, destinationIndexPath;
  if (coordinator.DestinationIndexPath != null)
  {
    indexPath = coordinator.DestinationIndexPath;
    destinationIndexPath = indexPath;
  }
  else
  {
    // Get last index path of table view
    var section = tableView.NumberOfSections() - 1;
    var row = tableView.NumberOfRowsInSection(section);
    destinationIndexPath = NSIndexPath.FromRowSection(row, section);
  }
  coordinator.Session.LoadObjects(typeof(NSString), (items) =>
  {
    // Consume drag items
    List<string> stringItems = new List<string>();
    foreach (var i in items)
    {
      var q = NSString.FromHandle(i.Handle);
      stringItems.Add(q.ToString());
    }
    var indexPaths = new List<NSIndexPath>();
    for (var j = 0; j < stringItems.Count; j++)
    {
      var indexPath1 = NSIndexPath.FromRowSection(destinationIndexPath.Row + j, destinationIndexPath.Section);
      model.AddItem(stringItems[j], indexPath1.Row);
      indexPaths.Add(indexPath1);
    }
    tableView.InsertRows(indexPaths.ToArray(), UITableViewRowAnimation.Automatic);
  });
}
```

Można dodać dodatkowy kod asynchronicznie załadować danych dużych obiektów.

### <a name="testing-drag-and-drop"></a>Testowanie przeciąganie i upuszczanie

Aby przetestować, należy użyć iPad [próbki](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/).
Otwórz przykładowe równolegle z innej aplikacji (na przykład informacje) i przeciągnij wierszy i tekst między nimi:

![Zrzut ekranu przedstawiający trwających operacji przeciągania](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>Linki pokrewne

- [Przeciągnij i upuść Human Interface Guidelines (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [Przeciągnij i upuść przykład widoku tabeli](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [Przeciągnij i upuść przykładowy widok kolekcji](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [Wprowadzenie do przeciągania i upuszczania (WWDC) (klip wideo)](https://developer.apple.com/videos/play/wwdc2017/203/)
- [Przeciągnij i upuść z kolekcji i widok tabeli (WWDC) (klip wideo)](https://developer.apple.com/videos/play/wwdc2017/223/)
