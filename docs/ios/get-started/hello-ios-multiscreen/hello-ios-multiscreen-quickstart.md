---
title: Witaj, iOS Multiscreen — Szybki Start
description: Ten dokument przedstawia sposób rozwiń Phoneword przykładowej aplikacji, aby dodać drugi ekran, zawierająca model-view-controller, nawigacji dla systemu iOS oraz inne podstawowe koncepcje programowanie dla systemu iOS na bieżąco.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: d72e6230-c9ee-4bee-90ec-877d256821aa
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: 469032dc7caa46c6a89b350dc37bc9a93366066a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785674"
---
# <a name="hello-ios-multiscreen--quickstart"></a>Witaj, iOS Multiscreen — Szybki Start

Ta część przewodnika doda drugi ekranu aplikacji Phoneword, która spowoduje wyświetlenie historii numerów telefonów, które zostały wywołane z aplikacją. Końcowe aplikacji będą mieć drugi ekranu, który wyświetla historię wywołanie, jak pokazano na poniższym zrzucie ekranu:

 [![](hello-ios-multiscreen-quickstart-images/00.png "Końcowe aplikacji będzie mieć drugi ekranu, który wyświetla historię wywołanie, jak pokazano na tym zrzucie ekranu pokazano")](hello-ios-multiscreen-quickstart-images/00.png#lightbox)

[Towarzyszące nowości](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md), aplikacji, która jest zbudowany kompilacji i wkrótce omówiono architekturę, nawigacji i innych nowych pojęć iOS napotykane wzdłuż sposób.

 <a name="Requirements" />

## <a name="requirements"></a>Wymagania

Ten wznawia przewodnik, gdzie wyłączone Hello, lewo dokumentu z systemem iOS i wymaga zakończenia [Hello, iOS szybkiego startu](~/ios/get-started/hello-ios/index.md). Ukończono wersji aplikacji Phoneword można pobrać z [Hello, przykładowe iOS](https://developer.xamarin.com/samples/monotouch/Hello_iOS/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="walkthrough"></a>Wskazówki

W tym przewodniku doda ekran Historia wywołań do naszej **Phoneword** aplikacji.


1. Otwórz **Phoneword** aplikacji w programie Visual Studio dla komputerów Mac. W razie potrzeby ukończona aplikacja Phoneword z [Hello, wskazówki iOS](~/ios/get-started/hello-ios/index.md) przewodnik można pobrać z [tutaj](https://developer.xamarin.com/samples/monotouch/Hello_iOS/).


2. Otwórz **Main.storyboard** plik z **konsoli rozwiązania**:

  ![](hello-ios-multiscreen-quickstart-images/02new.png "Main.storyboard w Projektancie systemu iOS")


3. Przeciągnij **kontrolera nawigacji** z **przybornika** na powierzchnię projektu (konieczne może być pomniejszyć do tych wszystkich na powierzchni projektowej!):

  ![](hello-ios-multiscreen-quickstart-images/03new.png "Przeciągnij z przybornika na powierzchnię projektu kontrolera nawigacji")


4. Przeciągnij **Sourceless Segue** (który jest Szara strzałka w lewo pojedynczy kontroler widoku) do **kontrolera nawigacji** zmienić punkt początkowy aplikacji:

  ![](hello-ios-multiscreen-quickstart-images/04new.png "Przeciągnij Sourceless Segue do kontrolera nawigacji, aby zmienić punkt początkowy aplikacji")


5. Wybierz istniejącą **kontrolera widoku głównego** , klikając na dolnym pasku, a następnie naciśnij klawisz **usunąć** Aby usunąć go z powierzchni projektu.
Następnie przenieś **Phoneword** sceny dalej, aby **kontrolera nawigacji**:

  ![](hello-ios-multiscreen-quickstart-images/05new.png "Przenieś sceny Phoneword obok kontrolera nawigacji")


6. Ustaw **ViewController** jako kontroler nawigacji **główny kontroler widoku**. Naciśnij klawisz **Ctrl** klucza, a następnie kliknij wewnątrz **kontrolera nawigacji**. Niebieski linii powinny być wyświetlane. Następnie, przytrzymując naciśnięty **Ctrl** klucza, przeciągnij z **kontrolera nawigacji** do **Phoneword** sceny i wersji. Ta metoda jest wywoływana _przeciąganie Ctrl_:

 ![](hello-ios-multiscreen-quickstart-images/06.png "Przeciągnij z kontrolera nawigacji do sceny Phoneword i wersji")


7. Z popover ustawioną relacji **głównego**:

  ![](hello-ios-multiscreen-quickstart-images/07new.png "Ustawienie relacji do elementu głównego")

  **ViewController** jest teraz **kontrolera widoku głównego kontrolera nawigacji:**

  ![](hello-ios-multiscreen-quickstart-images/08.png "ViewController jest teraz kontroler widoku głównego kontrolerów nawigacji")


8. Kliknij dwukrotnie **Phoneword** ekranu **tytuł** pasek i zmień **tytuł** do **Phoneword**:

  ![](hello-ios-multiscreen-quickstart-images/09.png "Zmień tytuł do \"Phoneword\"")


9. Przeciągnij **przycisk** z **przybornika** i umieść go w obszarze **przycisk Wywołaj**. Przeciągnij uchwyty, aby utworzyć nowy **przycisk** szerokość **przycisk Wywołaj**:

  ![](hello-ios-multiscreen-quickstart-images/10new.png "Utwórz nowy przycisk szerokość przycisku wywołania")


10. W **konsoli właściwości**, zmień **nazwa** przycisku **CallHistoryButton** i zmienić **tytuł** do **wywołania Historia**:

  ![](hello-ios-multiscreen-quickstart-images/11new.png "Zmień nazwę przycisku CallHistoryButton i Zmień tytuł, aby wywołać historii")


11. Utwórz **Historia wywołań** ekranu. Z **przybornika**, przeciągnij **kontrolera widoku tabeli** na powierzchnię projektu:

 ![](hello-ios-multiscreen-quickstart-images/12new.png "Przeciągnij kontrolera widoku tabeli na powierzchnię projektu")


12. Następnie wybierz pozycję **kontrolera widoku tabeli** , klikając na pasku czarny u dołu sceny. W **konsoli właściwości**, zmień **kontrolera widoku tabeli** klasy do `CallHistoryController` i naciśnij klawisz **Enter**:

  ![](hello-ios-multiscreen-quickstart-images/13new.png "Zmień klasę kontrolerów widok tabeli CallHistoryController")

  IOS projektanta wygeneruje klasy niestandardowej zapasowy o nazwie `CallHistoryController` do zarządzania tym ekranie do hierarchii widok zawartości.
  **CallHistoryController.cs** pliku będą wyświetlane w **konsoli rozwiązania**:

  ![](hello-ios-multiscreen-quickstart-images/14new.png "Plik CallHistoryController.cs w konsoli rozwiązania")


13. Kliknij dwukrotnie **CallHistoryController.cs** plik, aby go otworzyć i Zastąp zawartość następującym kodem:

  ```csharp
  using System;
  using Foundation;
  using UIKit;
  using System.Collections.Generic;

  namespace Phoneword_iOS
  {
      public partial class CallHistoryController : UITableViewController
      {
          public List<string> PhoneNumbers { get; set; }

          static NSString callHistoryCellId = new NSString ("CallHistoryCell");

          public CallHistoryController (IntPtr handle) : base (handle)
          {
              TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
              TableView.Source = new CallHistoryDataSource (this);
              PhoneNumbers = new List<string> ();
          }

          class CallHistoryDataSource : UITableViewSource
          {
              CallHistoryController controller;

              public CallHistoryDataSource (CallHistoryController controller)
              {
                  this.controller = controller;
              }

              public override nint RowsInSection (UITableView tableView, nint section)
              {
                  return controller.PhoneNumbers.Count;
              }

              public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
              {
                  var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                  int row = indexPath.Row;
                  cell.TextLabel.Text = controller.PhoneNumbers [row];
                  return cell;
              }
          }
      }
  }
  ```

  Zapisz tę aplikację (**⌘ + s**) i skompiluj go (**⌘ + b**) aby upewnić się, nie ma żadnych błędów.


14. Utwórz _Segue_ (przejście) między **Phoneword** sceny i **Historia wywołań** sceny.
  W **sceny Phoneword**, wybierz pozycję **przycisk Historia wywołań** i Ctrl i przeciągnij od **przycisk** do **Historia wywołań** sceny:

  ![](hello-ios-multiscreen-quickstart-images/15.png "CTRL i przeciągnij po kliknięciu przycisku do sceny Historia wywołań")

  Z **Segue akcji** popover, wybierz opcję **Pokaż**

  IOS projektanta doda Segue między dwoma sceny:

  ![](hello-ios-multiscreen-quickstart-images/17new.png "Segue między dwoma sceny")


15. Dodaj **tytuł** do **kontrolera widoku tabeli** wybierając czarny pasek u dołu sceny i zmiana **tytuł kontrolera widoku** do **wywołania Historia** w **konsoli właściwości**:

  ![](hello-ios-multiscreen-quickstart-images/18new.png "Zmień tytuł kontrolera widoku, aby Historia rozmów w konsoli właściwości")

16. Gdy aplikacja jest uruchamiana, **przycisk Historia wywołań** spowoduje otwarcie **Historia wywołań** ekranu, ale tabela widok będzie pusty, ponieważ nie jest wykonywany kod w celu śledzenia i wyświetlania numerów telefonów.

  Ta aplikacja będzie przechowywać numery telefonów jako lista ciągów.

  Dodaj `using` dyrektywy dla `System.Collections.Generic` w górnej części **ViewController**:

  ```csharp
  using System.Collections.Generic;
  ```



17. Modyfikowanie `ViewController` klasy następującym kodem:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Foundation;
    using UIKit;

    namespace Phoneword_iOS
    {
      public partial class ViewController : UIViewController
      {
        string translatedNumber = "";

        public List<string> PhoneNumbers { get; set; }

        protected ViewController(IntPtr handle) : base(handle)
        {
          //initialize list of phone numbers called for Call History screen
          PhoneNumbers = new List<string>();
        }

        public override void ViewDidLoad()
        {
          base.ViewDidLoad();
          // Perform any additional setup after loading the view, typically from a nib.

          TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
            // Convert the phone number with text to a number
            // using PhoneTranslator.cs
            translatedNumber = PhoneTranslator.ToNumber(
              PhoneNumberText.Text);

            // Dismiss the keyboard if text field was tapped
            PhoneNumberText.ResignFirstResponder();

            if (translatedNumber == "")
            {
              CallButton.SetTitle("Call ", UIControlState.Normal);
              CallButton.Enabled = false;
            }
            else
            {
              CallButton.SetTitle("Call " + translatedNumber,
                UIControlState.Normal);
              CallButton.Enabled = true;
            }
          };

          CallButton.TouchUpInside += (object sender, EventArgs e) => {

            //Store the phone number that we're dialing in PhoneNumbers
            PhoneNumbers.Add(translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl("tel:" + translatedNumber);

            // otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl(url))
            {
              var alert = UIAlertController.Create("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
              alert.AddAction(UIAlertAction.Create("Ok", UIAlertActionStyle.Default, null));
              PresentViewController(alert, true, null);
            }
          };
        }

        public override void PrepareForSegue(UIStoryboardSegue segue, NSObject sender)
        {
          base.PrepareForSegue(segue, sender);

          // set the View Controller that’s powering the screen we’re
          // transitioning to

          var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

          //set the Table View Controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryContoller != null)
          {
            callHistoryContoller.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```


Istnieje kilka czynności wykonywane w tym miejscu
  * Zmienna `translatedNumber` został przeniesiony z `ViewDidLoad` metodę _zmiennej na poziomie klasy_.
  * **CallButton** kod został zmodyfikowany w celu dodania wybierane numery do listy numerów telefonów przez wywołanie metody `PhoneNumbers.Add(translatedNumber)`
  * `PrepareForSegue` Dodano — metoda

  Zapisz i tworzenia aplikacji, aby upewnić się, że nie ma żadnych błędów.

20. Naciśnij klawisz **Start** przycisk, aby uruchomić aplikację w **symulatora systemu iOS**:

  ![](hello-ios-multiscreen-quickstart-images/19.png "Naciśnij przycisk Start, aby uruchomić aplikację w symulatorze systemu iOS")


Gratulujemy Kończenie pierwszej aplikacji platformy Xamarin.iOS wielu ekranu!

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="walkthrough"></a>Wskazówki

W tym przewodniku doda ekran Historia wywołań do naszej **Phoneword** aplikacji.


1. Otwórz **Phoneword** aplikacji w programie Visual Studio. W razie potrzeby Pobierz [ukończona aplikacja Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) z [Hello, wskazówki iOS](~/ios/get-started/hello-ios/index.md) pobieranie przewodnika. Odwołanie jest wymagane do nawiązania [Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) do użycia z systemem iOS projektanta i symulatora systemu iOS.


2. Rozpocznij od edycji interfejsu użytkownika. Otwórz **Main.storyboard** plik z **Eksploratora rozwiązań**, upewniając się, że **widoku jako** ustawiono _iPhone 6_:

  ![](hello-ios-multiscreen-quickstart-images/image1.png "Main.storyboard w Projektancie systemu iOS")


3. Przeciągnij **kontrolera nawigacji** z **przybornika** na powierzchnię projektu:

  ![](hello-ios-multiscreen-quickstart-images/image2.png "Przeciągnij z przybornika na powierzchnię projektu kontrolera nawigacji")


4. Przeciągnij **Sourceless Segue** (to Szara strzałka w lewo z **Phoneword** sceny) z **Phoneword** sceny **kontrolera nawigacji** zmienić punkt początkowy aplikacji:

  ![](hello-ios-multiscreen-quickstart-images/image3.png "Przeciągnij Sourceless Segue do kontrolera nawigacji, aby zmienić punkt początkowy aplikacji")


5. Wybierz **kontrolera widoku głównego** , klikając pasek czarny, a następnie naciśnij klawisz **usunąć** Aby usunąć go z powierzchni projektu.
  Następnie przenieś **Phoneword** sceny dalej, aby **kontrolera nawigacji**:

  ![](hello-ios-multiscreen-quickstart-images/image4.png "Przenieś sceny Phoneword obok kontrolera nawigacji")


6. Ustaw **ViewController** jako kontroler nawigacji `Root View Controller`. Naciśnij klawisz **Ctrl** klucza, a następnie kliknij wewnątrz **kontrolera nawigacji**. Niebieski linii powinny być wyświetlane. Następnie, przytrzymując naciśnięty **Ctrl** klucza, przeciągnij z **kontrolera nawigacji** do **Phoneword** sceny i wersji. Ta metoda jest wywoływana _przeciąganie Ctrl_:

  ![](hello-ios-multiscreen-quickstart-images/image5.png "Przeciągnij z kontrolera nawigacji do sceny Phoneword i wersji")


7. Z popover ustawioną relacji **głównego**:

  ![](hello-ios-multiscreen-quickstart-images/image6.png "Ustaw relacji do elementu głównego")

  **ViewController** jest teraz naszych **kontrolera widoku głównego kontrolera nawigacji.**


8. Kliknij dwukrotnie **Phoneword** ekranu **tytuł** pasek i zmień **tytuł** do **Phoneword**:

  ![](hello-ios-multiscreen-quickstart-images/image7.png "Zmień tytuł, aby Phoneword")


9. Przeciągnij **przycisk** z **przybornika** i umieść go w obszarze **przycisk Wywołaj**. Przeciągnij uchwyty, aby utworzyć nowy **przycisk** szerokość **przycisk Wywołaj**:

  ![](hello-ios-multiscreen-quickstart-images/image8.png "Utwórz nowy przycisk szerokość przycisku wywołania")


10. W **Explorer właściwości**, zmień **nazwa** z **przycisk** do `CallHistoryButton` i zmienić **tytuł** do  **Historia rozmów**:

  ![](hello-ios-multiscreen-quickstart-images/image9.png "Zmień nazwę przycisku \"CallHistoryButton\" i tytuł \"Historia rozmów\"")


11. Utwórz **Historia wywołań** ekranu. Z **przybornika**, przeciągnij **kontrolera widoku tabeli** na powierzchnię projektu:

  ![](hello-ios-multiscreen-quickstart-images/image10.png "Przeciągnij kontrolera widoku tabeli na powierzchnię projektu")


12. Wybierz **kontrolera widoku tabeli** , klikając na pasku czarny u dołu sceny. W **Explorer właściwości**, zmień **kontrolera widoku tabeli** klasy do `CallHistoryController` i naciśnij klawisz **Enter**:

  ![](hello-ios-multiscreen-quickstart-images/image11.png "Zmień klasę kontrolerów widok tabeli CallHistoryController")

  IOS projektanta wygeneruje klasy niestandardowej zapasowy o nazwie `CallHistoryController` do zarządzania tym ekranie do hierarchii widok zawartości.
  **CallHistoryController.cs** pliku będą wyświetlane w **Eksploratora rozwiązań**:

  ![](hello-ios-multiscreen-quickstart-images/image12.png "Plik CallHistoryController.cs w Eksploratorze rozwiązań")


13. Kliknij dwukrotnie **CallHistoryController.cs** plik, aby go otworzyć i Zastąp zawartość następującym kodem:

        using System;
        using Foundation;
        using UIKit;
        using System.Collections.Generic;

        namespace Phoneword
        {
            public partial class CallHistoryController : UITableViewController
            {
                public List<String> PhoneNumbers { get; set; }

                static NSString callHistoryCellId = new NSString ("CallHistoryCell");

                public CallHistoryController (IntPtr handle) : base (handle)
                {
                    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
                    TableView.Source = new CallHistoryDataSource (this);
                    PhoneNumbers = new List<string> ();
                }

                class CallHistoryDataSource : UITableViewSource
                {
                    CallHistoryController controller;

                    public CallHistoryDataSource (CallHistoryController controller)
                    {
                        this.controller = controller;
                    }

                    // Returns the number of rows in each section of the table
                    public override nint RowsInSection (UITableView tableView, nint section)
                    {
                        return controller.PhoneNumbers.Count;
                    }

                    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
                    {
                        var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                        int row = indexPath.Row;
                        cell.TextLabel.Text = controller.PhoneNumbers [row];
                        return cell;
                    }
                }
            }
        }

  Zapisz aplikację i skompiluj go, aby upewnić się, że nie ma żadnych błędów. Jest można zignorować ostrzeżenia kompilacji teraz.


14. Utwórz _Segue_ (przejście) między **Phoneword** sceny i **Historia wywołań** sceny.
  W **sceny Phoneword**, wybierz pozycję **przycisk Historia wywołań** i **Ctrl i przeciągnij** z **przycisk** do **Historia wywołań**  Sceny:

  ![](hello-ios-multiscreen-quickstart-images/image13.png "CTRL i przeciągnij po kliknięciu przycisku do sceny Historia wywołań")

  Z **Segue akcji** popover, wybierz opcję **Pokaż**:

  ![](hello-ios-multiscreen-quickstart-images/image14.png "Zaznacz opcję Pokaż jako typ segue")

  IOS projektanta doda Segue między dwoma sceny:

  ![](hello-ios-multiscreen-quickstart-images/image15.png "Segue między dwoma sceny")


15. Dodaj **tytuł** do **kontrolera widoku tabeli** wybierając czarny pasek u dołu sceny i zmiana **kontrolera widoku > Tytuł** do **wywołania Historia** w **Explorer właściwości**:

  ![](hello-ios-multiscreen-quickstart-images/image16.png "Zmień tytuł kontrolera widoku, aby Historia rozmów")


16. Gdy aplikacja jest uruchamiana, **przycisk Historia wywołań** spowoduje otwarcie **Historia wywołań** ekranu, ale tabela widok będzie pusty, ponieważ nie jest wykonywany kod w celu śledzenia i wyświetlania numerów telefonów.

  Ta aplikacja będzie przechowywać numery telefonów jako lista ciągów.

  Dodaj `using` dyrektywy dla `System.Collections.Generic` w górnej części **ViewController**:

        using System.Collections.Generic;

17. Modyfikowanie `ViewController` klasy następującym kodem:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Foundation;
    using UIKit;

    namespace Phoneword_iOS
    {
      public partial class ViewController : UIViewController
      {
        string translatedNumber = "";

        public List<string> PhoneNumbers { get; set; }

        protected ViewController(IntPtr handle) : base(handle)
        {
          //initialize list of phone numbers called for Call History screen
          PhoneNumbers = new List<string>();
        }

        public override void ViewDidLoad()
        {
          base.ViewDidLoad();
          // Perform any additional setup after loading the view, typically from a nib.

          TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
            // Convert the phone number with text to a number
            // using PhoneTranslator.cs
            translatedNumber = PhoneTranslator.ToNumber(
              PhoneNumberText.Text);

            // Dismiss the keyboard if text field was tapped
            PhoneNumberText.ResignFirstResponder();

            if (translatedNumber == "")
            {
              CallButton.SetTitle("Call ", UIControlState.Normal);
              CallButton.Enabled = false;
            }
            else
            {
              CallButton.SetTitle("Call " + translatedNumber,
                UIControlState.Normal);
              CallButton.Enabled = true;
            }
          };

          CallButton.TouchUpInside += (object sender, EventArgs e) => {

            //Store the phone number that we're dialing in PhoneNumbers
            PhoneNumbers.Add(translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl("tel:" + translatedNumber);

            // otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl(url))
            {
              var alert = UIAlertController.Create("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
              alert.AddAction(UIAlertAction.Create("Ok", UIAlertActionStyle.Default, null));
              PresentViewController(alert, true, null);
            }
          };
        }

        public override void PrepareForSegue(UIStoryboardSegue segue, NSObject sender)
        {
          base.PrepareForSegue(segue, sender);

          // set the View Controller that’s powering the screen we’re
          // transitioning to

          var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

          //set the Table View Controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryContoller != null)
          {
            callHistoryContoller.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```

Istnieje kilka czynności wykonywane w tym miejscu
  * Zmienna `translatedNumber` został przeniesiony z `ViewDidLoad` metodę _zmiennej na poziomie klasy_.
  * **CallButton** kod został zmodyfikowany w celu dodania wybierane numery do listy numerów telefonów przez wywołanie metody `PhoneNumbers.Add(translatedNumber)`
  * `PrepareForSegue` Dodano — metoda

  Zapisz i tworzenia aplikacji, aby upewnić się, że nie ma żadnych błędów.

  Zapisz i tworzenia aplikacji, aby upewnić się, że nie ma żadnych błędów.


20. Naciśnij klawisz **Start** przycisk, aby uruchomić naszej aplikacji wewnątrz **symulatora systemu iOS**:

  ![](hello-ios-multiscreen-quickstart-images/19.png "Na pierwszym ekranie przykładowej aplikacji")


Gratulujemy Kończenie pierwszej aplikacji platformy Xamarin.iOS wielu ekranu!


-----

Aplikacji może teraz obsłużyć nawigacji za pomocą obu Segues scenorysu w kodzie. Teraz nadszedł czas na dissect narzędzia i umiejętności właśnie dowiedzieliśmy się w [Hello, iOS Wieloekranowy nowości](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md).


## <a name="related-links"></a>Linki pokrewne

- [Witaj, iOS (przykład)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [System iOS w portalu inicjowania obsługi](https://developer.apple.com/ios/manage/overview/index.action)
