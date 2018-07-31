---
title: Przeciągnij i upuść w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano sposób implementacji przeciągania i upuszczania w aplikacji platformy Xamarin.iOS przy użyciu interfejsów API, wprowadzona w systemie iOS 11. W szczególności omówiono Włączanie przeciągania i upuszczania w UITableView.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/05/2017
ms.openlocfilehash: bc58c866a4a754bccea8d851f79e73fe5a415eed
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351138"
---
# <a name="drag-and-drop-in-xamarinios"></a>Przeciągnij i upuść w rozszerzeniu Xamarin.iOS

_Implementowanie przeciągania i upuszczania dla systemu iOS 11_

System iOS 11 obejmuje przeciągania i upuszczania pomocy technicznej w celu skopiowania danych między aplikacjami na urządzeniu iPad. Użytkownicy mogą wybrać i przeciągnij wszystkich typów zawartości z aplikacji umieszczony side-by-side lub poprzez przeciąganie myszy na ikonie aplikacji, która spowoduje wyzwolenie aplikacji, aby otworzyć i umożliwić danych można usunąć:

![Przeciąganie i upuszczanie przykładu z aplikacji niestandardowej w informacje o aplikacji](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> Przeciąganie i upuszczanie jest dostępna tylko w ramach tej samej aplikacji na urządzeniu iPhone.

Należy rozważyć obsługę przeciągnij i upuść operacje dowolnym zawartości można utworzyć lub edytować:

- Kontrolki tekstu obsługi przeciągania i upuszczania dla wszystkich aplikacji skompilowana dla systemu iOS 11, bez konieczności wykonywania dodatkowych działań.
- Widoki tabel i widoków kolekcji zawiera ulepszeń w systemie iOS 11, które upraszczają Dodawanie przeciągnij i upuść działanie.
- Inne widoki, będzie możliwe do obsługi przeciągania i upuszczania za pomocą dodatkowych dostosowań.

W przypadku dodawania przeciągania i upuszczania pomocy technicznej dla aplikacji, można zapewnić różne poziomy jakości zawartości; na przykład można określić tekst sformatowany i zwykły tekst wersji danych tak, aby aplikacja odbierająca można wybrać, które najlepiej pasują do element docelowy przeciągania. Jest również możliwe do dostosowania wizualizacji przeciągania i umożliwia przeciąganie wielu elementów naraz.

## <a name="drag-and-drop-with-text-controls"></a>Przeciąganie i upuszczanie za pomocą kontrolek tekstu

`UITextView` i `UITextField` automatycznie obsługują przeciąganie zaznaczonego tekstu w poziomie i zawartość tekstową.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>Przeciąganie i upuszczanie za pomocą UITableView

`UITableView` ma wbudowaną obsługę przeciąganie i upuszczanie interakcji z wiersze tabeli, wymaga tylko kilka metod, aby włączyć to zachowanie domyślne.

Istnieją dwa interfejsy, zaangażowany:

- `IUITableViewDragDelegate` — Informacje pakietów po zainicjowaniu przeciągnij w widoku tabeli.
- `IUITableViewDropDelegate` — Przetwarza informacje gdy zrzutu jest nastąpiła i zakończony.

W [przykładowe DragAndDropTableView](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/) te dwa interfejsy są zarówno implementowane w `UITableViewController` klasy, wraz ze źródła danych i delegata. Są przydzielone w `ViewDidLoad` metody:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

Minimalnej ilości kodu wymagane dla tych dwóch interfejsów zostało wyjaśnione poniżej.

### <a name="table-view-drag-delegate"></a>Delegat przeciągnij widok tabeli

Jedyną metodą _wymagane_ do obsługi, przeciągając wiersz z tabeli jest `GetItemsForBeginningDragSession`. Jeśli użytkownik rozpoczyna przeciągnij wiersz, ta metoda zostanie wywołana.

Poniżej przedstawiono implementację. Jej pobiera dane skojarzone z przeciągniętego wiersz koduje go jako i konfiguruje `NSItemProvider` Określa, jak aplikacje będą obsługiwać "listy" część operacji (na przykład, czy ich może obsługiwać typ danych `PlainText`, w przykładzie):

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

Istnieje wiele metod opcjonalne w delegacie przeciągania, którą można implementować, aby dostosować zachowanie przeciągnij, jak na przykład wiele reprezentacji danych, które mogą być podejmowane zalet w aplikacjach docelowych (np. tekstu sformatowanego, jak również jako zwykły tekst lub wektor i Mapa bitowa wersje rysunku). Możesz też podać reprezentacji danych niestandardowych do użycia podczas przeciągania i upuszczania w ramach tej samej aplikacji.

### <a name="table-view-drop-delegate"></a>Delegat listy widok tabeli

Metody na delegata listy są wywoływane, gdy operacja przeciągania odbywa się za pośrednictwem widoku tabeli lub zakończeniu nad nim. Wymaganych metod ustalić, czy dane można go porzucić i jakie działania są podjęte, jeśli listy zostanie zakończona:

- `CanHandleDropSession` — Podczas przeciągania jest w toku i potencjalnie usuwane w aplikacji, ta metoda określa, czy dane przeciąganie może go porzucić.
- `DropSessionDidUpdate` — Podczas przeciągania jest w toku, ta metoda jest wywoływana, aby ustalić, jakie działanie ma. Informacji z widoku tabeli, jest przeciągany nad, przeciągnij sesji i ścieżki możliwe indeks można użyć do określenia zachowania i wizualną opinię udostępniany użytkownikowi.
- `PerformDrop` — Po użytkownik kończy listy (na przykład, przenosząc ich palca), ta metoda wyodrębnia dane przeciąganie i modyfikuje widoku tabeli, aby dodać dane w nowy wiersz (lub wierszy).

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` Wskazuje, czy widok tabeli może akceptować danych przeciągania. W poniższym przykładzie `CanLoadObjects` służy do potwierdzenia, że ten widok tabeli może akceptować dane ciągu.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate` Wywoływana jest metoda wielokrotnie podczas operacji przeciągania, aby zapewnić podpowiedzi wizualne dla użytkownika.

W poniższym kodzie `HasActiveDrag` służy do określania, czy operacja pochodzi w bieżącym widoku tabeli. Jeśli tak, tylko jednego wiersze są dozwolone do przeniesienia.
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

Można wykonać operacji usuwania, jeden z `Cancel`, `Move`, lub `Copy`.

Celem listy można wstawić nowy wiersz lub dodać/dołączanie danych do istniejącego wiersza.

#### <a name="performdrop"></a>PerformDrop

`PerformDrop` Metoda jest wywoływana, gdy użytkownik zakończy operację i modyfikuje źródło widoku, a dane tabeli, aby odzwierciedlić porzuconych dane.

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

Asynchroniczne ładowanie dużych obiektów danych można dodać dodatkowy kod.

### <a name="testing-drag-and-drop"></a>Testowanie przeciągania i upuszczania

IPad należy użyć, aby przetestować [przykładowe](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/).
Otwórz przykładowy wraz z innej aplikacji (np. informacje o), a następnie przeciągnij wierszy i tekst między nimi:

![Zrzut ekranu przedstawiający Trwa operacja przeciągania](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>Linki pokrewne

- [Przeciąganie i upuszczanie ludzi Interface Guidelines (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [Przeciąganie i upuszczanie przykładowy widok tabeli](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [Przeciąganie i upuszczanie przykładowy widok kolekcji](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [Wprowadzenie do przeciągania i upuszczania (WWDC) (wideo)](https://developer.apple.com/videos/play/wwdc2017/203/)
- [Przeciąganie i upuszczanie za pomocą kolekcji i widok tabeli (WWDC) (wideo)](https://developer.apple.com/videos/play/wwdc2017/223/)
