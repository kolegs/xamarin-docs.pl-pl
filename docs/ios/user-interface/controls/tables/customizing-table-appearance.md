---
title: "Dostosowywanie wyglądu tabeli"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 4a3f8ca8f4502b9585536815aef81f66cacd214f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="customizing-a-tables-appearance"></a>Dostosowywanie wyglądu tabeli

Najprostszym sposobem, aby zmienić wygląd tabeli jest stylu inną komórkę. Można zmienić stylu komórki, który jest używany podczas tworzenia każdej komórki w `UITableViewSource`w `GetCell` metody.

## <a name="cell-styles"></a>Style komórek

Istnieją cztery wbudowane style:

-  **Domyślna** — obsługuje `UIImageView`.
-  **Podtytuł** — obsługuje `UIImageView` i pomocniczą.
-  **Wartość1** — prawa obsługuje wyrównany podtytuł `UIImageView`.
-  **Wartość2** — tytuł jest wyrównany do prawej i pomocniczą jest wyrównany (ale nie obrazu).


Te zrzuty ekranu pokazują, jak każdy styl pojawi się:

 [![](customizing-table-appearance-images/image7.png "Te zrzuty ekranu Pokaż sposobu wyświetlania każdego stylu")](customizing-table-appearance-images/image7.png#lightbox)

Przykład **CellDefaultTable** zawiera kod, aby utworzyć te ekranów. Styl komórki jest ustawiona `UITableViewCell` konstruktora w następujący sposób:

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[Obsługiwane właściwości](http://developer.xamarin.com/api/type/UIKit.UITableViewCell/) komórki można następnie ustawić stylu:

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Akcesoria

Komórki może mieć następujące Akcesoria, dodawane po prawej stronie w widoku:

-   **Znacznik wyboru** — można wskazać wielokrotnego wyboru w tabeli.
-   **DetailButton** — odpowiada na touch niezależnie od pozostałej części komórki, dzięki któremu do wykonywania różnych funkcji do tej samej komórki dotknięcie (takich jak otwieranie menu podręczne lub nowego okna, które nie jest częścią `UINavigationController` stosu).
-   **DisclosureIndicator** — zwykle używane w celu wskazania, że dotknięcie komórki zostanie otwarty inny widok.
-   **DetailDisclosureButton** — kombinację `DetailButton` i `DisclosureIndicator`.


Oto jak wyglądają:

 [![](customizing-table-appearance-images/image8.png "Akcesoria próbki")](customizing-table-appearance-images/image8.png#lightbox)

Aby wyświetlić jeden z tych Akcesoria można ustawić `Accessory` właściwości w `GetCell` metody:

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

Gdy `DetailButton` lub `DetailDisclosureButton` są wyświetlane, należy również przesłonić `AccessoryButtonTapped` na wykonanie akcji, gdy jest on dotknięciu.

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

Przykład **CellAccessoryTable** przedstawiono przykład przy użyciu Akcesoria.

## <a name="cell-separators"></a>Komórki separatorów

Separatory komórki są komórek tabeli używany do rozdzielania w tabeli. Właściwości są ustawiane w tabeli.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

Istnieje również możliwość dodać efekt rozmycia lub popularność do separator:

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

Separator ma także wstawki:

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>Tworzenie układów komórki niestandardowych

Aby zmienić stylu wizualnego. należy podać go, aby wyświetlić niestandardowe komórki tabeli. Niestandardowe komórka może mieć różne układy kolorów i kontroli.

W przykładzie CellCustomTable implementuje `UITableViewCell` podklasy, który definiuje niestandardowe układ `UILabel`s i `UIImage` z różnych czcionek i kolorów. Wynikowa komórek wyglądać następująco:

 [![](customizing-table-appearance-images/image9.png "Układy niestandardowe komórki")](customizing-table-appearance-images/image9.png#lightbox)

Klasy niestandardowej komórki obejmuje tylko trzy metody:

-   **Konstruktor** — tworzy kontrolek interfejsu użytkownika i ustawia właściwości style niestandardowe (np.) krój czcionki, rozmiaru i kolory).
-   **UpdateCell** — metoda `UITableView.GetCell` służące do ustawiania właściwości komórki.
-   **LayoutSubviews** — Ustaw lokalizację kontrolek interfejsu użytkownika. W przykładzie każdej komórki ma ten sam układ, ale komórki bardziej złożonych (zwłaszcza o różnych rozmiarach) może być konieczne inny układ pozycji w zależności od zawartości będzie wyświetlany.


Zakończenie przykładowy kod **CellCustomTable > CustomVegeCell.cs** następuje:

```csharp
public class CustomVegeCell : UITableViewCell  {
    UILabel headingLabel, subheadingLabel;
    UIImageView imageView;
    public CustomVegeCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
    {
        SelectionStyle = UITableViewCellSelectionStyle.Gray;
        ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);
        imageView = new UIImageView();
        headingLabel = new UILabel () {
            Font = UIFont.FromName("Cochin-BoldItalic", 22f),
            TextColor = UIColor.FromRGB (127, 51, 0),
            BackgroundColor = UIColor.Clear
        };
        subheadingLabel = new UILabel () {
            Font = UIFont.FromName("AmericanTypewriter", 12f),
            TextColor = UIColor.FromRGB (38, 127, 0),
            TextAlignment = UITextAlignment.Center,
            BackgroundColor = UIColor.Clear
        };
        ContentView.AddSubviews(new UIView[] {headingLabel, subheadingLabel, imageView});

    }
    public void UpdateCell (string caption, string subtitle, UIImage image)
    {
        imageView.Image = image;
        headingLabel.Text = caption;
        subheadingLabel.Text = subtitle;
    }
    public override void LayoutSubviews ()
    {
        base.LayoutSubviews ();
        imageView.Frame = new CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
        headingLabel.Frame = new CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
        subheadingLabel.Frame = new CGRect (100, 18, 100, 20);
    }
}
```

`GetCell` Metoda `UITableViewSource` musi zostać zmodyfikowane w celu utworzenia niestandardowych komórki:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (cellIdentifier) as CustomVegeCell;
    if (cell == null)
        cell = new CustomVegeCell (cellIdentifier);
    cell.UpdateCell (tableItems[indexPath.Row].Heading
            , tableItems[indexPath.Row].SubHeading
            , UIImage.FromFile ("Images/" + tableItems[indexPath.Row].ImageName) );
    return cell;
}
```



## <a name="related-links"></a>Linki pokrewne

- [WorkingWithTables (przykład)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
