---
title: Selektor
description: Ten przewodnik obejmuje projektowanie i pracy z selektorami w aplikacji platformy Xamarin.iOS.
ms.topic: article
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/02/2017
ms.openlocfilehash: 402b17ddbb28fb8896ad0f158fe8dbcd17689f40
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="picker"></a>Selektor

Formant wyboru Wyświetla formantu "przypominającej koło", który zawiera listę o wartości z zaznaczonej wartości są wyróżnione. Użytkownicy Obróć koło z opcją, które mają.

Dla selektorami jej do ustawienia daty i / lub godzina sprawy jednego określonego użytkownika. Do zapewnienia tego Apple został utworzony niestandardowy podklasą klasy UIPickerView o nazwie UIDatePicker.

Artykuł obejmuje wdrażanie i używanie [selektora](#picker) i [wyboru daty](#datepicker) kontrolki.

<a name="picker">

## <a name="picker"></a>Selektor

### <a name="implementing-a-picker"></a>Implementowanie selektora

Formant wyboru jest wdrażany przy uruchamianiu nową [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="pickers-and-storyboards"></a>Selektorami i planów

Jeśli używasz systemu iOS Designer do tworzenia interfejsu użytkownika, selektor mogą być dodawane do układu z przybornika:

![](picker-images/image1.png)


### <a name="working-with-picker"></a>Praca z selektora

Po utworzeniu selektora, czy w kodzie, lub za pośrednictwem scenorys, musisz przypisać _modelu_ do niego, aby mogli przekazywać i interakcji z danymi;

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    var pickerModel = new PeopleModel(personLabel);

    personPicker.Model = pickerModel;
}
```

Poniższy kod przedstawia przykład modelu:

```csharp
public class PeopleModel : UIPickerViewModel
{
    public string[] names = new string[] {
            "Amy Burns",
            "Kevin Mullins",
            "Craig Dunn",
            "Joel Martinez",
            "Charles Petzold",
            "David Britch",
            "Mark McLemore",
            "Tom Opegenorth",
            "Joseph Hill",
            "Miguel De Icaza"
        };

    private UILabel personLabel;

    public PeopleModel(UILabel personLabel)
    {
        this.personLabel = personLabel;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 2;
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return names.Length;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        if (component == 0)
            return names[row];
        else
            return row.ToString();
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {   
        personLabel.Text = $"This person is: {names[pickerView.SelectedRowInComponent(0)]},\n they are number {pickerView.SelectedRowInComponent(1)}";
    }

    public override nfloat GetComponentWidth(UIPickerView picker, nint component)
    {
        if (component == 0)
            return 240f;
        else
            return 40f;
    }

    public override nfloat GetRowHeight(UIPickerView picker, nint component)
    {
        return 40f;
    }
```

Najpierw należy przekazać w niektórych danych, aby zapewnić użytkownikowi na wybranie opcji. Gdy możliwe starać się zachować tej listy możliwie krótki, lub jeśli konieczne spróbuj użyć więcej niż jeden "Wybierz" (nazywane *składniki*):

![Selektor z dwa składniki:](picker-images/image3.png)

Aby ustawić liczbę składników, należy zastąpić `GetComponentCount` metody: 

```csharp
public override nint GetComponentCount(UIPickerView pickerView)
{
    return 2;
}
```

Zwracana wartość oznacza liczbę wybierania, które użytkownika selektora.

### <a name="customizing-appearance"></a>Dostosowywanie wyglądu
 
Wygląd `UIPickerView` można dostosować za pomocą `UIPickerView.UIPickerViewAppearance` klasy lub poprzez przesłonięcie `UIPickerViewModel.GetView` i `UIPickerViewModel.GetRowHeight` metod w `UIPickerViewModel`.


<a name="datepicker">

## <a name="date-picker"></a>Wybór daty

### <a name="implementing-a-date-picker"></a>Implementowanie wyboru daty

Wybór daty jest wdrażany przy uruchamianiu nową [ `UIDatePickerView` ](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="date-pickers-and-storyboards"></a>Wyboru daty i planów

Jeśli używasz iOS Designer do tworzenia interfejsu użytkownika, **wyboru daty** można dodać do układu z przybornika. Następujące właściwości można dostosować z konsoli właściwości:

![](picker-images/image2.png)

* **Tryb** — tryb daty i godziny. Może to być data, czas, daty i godziny lub czasomierza odliczania. 
* **Ustawienia regionalne** — z ustawieniami regionalnymi wyboru daty. Wybierz **domyślne** ustawioną wartość domyślną systemu lub ustawić żadnych ustawień regionalnych.
* **Interwał** — przedstawia przyrostu, w którym będą wyświetlane opcje zegara.
* **Data, Minimalna data, maksymalna data** — ustawia selektor będzie wyświetlana data początkowa i ograniczenia na wybranie daty.

### <a name="configuring-the-datepicker"></a>Konfigurowanie selektora daty

Można ograniczyć zakres dat, które użytkownik może wybrać z za pomocą `MinimumDate` i `MaximumDate` właściwości. Poniższy fragment kodu przedstawia przykład sposobu ustawiania z zakresu od 60 lat temu i dzisiaj:

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();

components.Year = -60;

NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);

datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

Formanty .NET można też użyć również można ustawić zakresu dat minimalną i maksymalną. Na przykład:

```csharp
DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
```

Można również ustawić `MinuteInterval` właściwości, aby ustawić interwał, jaką selektora spowoduje wyświetlenie minut. Poniższy fragment kodu można ustawić selektor minut, aby można ustawić w odstępach 10.

```csharp
datePickerView.MinuteInterval = 10;
```

Istnieją cztery tryby, które można ustawić formant wyboru daty przy użyciu [ `UIDatePicker.Mode` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.Mode/) właściwości. Na poniższej liście przedstawiono przykład każdego z nich i sposobu wdrażania go:

#### <a name="time"></a>Godzina

Tryb czasu wyświetla czas selektora godzinowe i minutowe oraz opcjonalnie oznaczeniem AM lub PM. Została skonfigurowana z `UIDatePickerMode.Time` właściwości. Na przykład:

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

Na poniższym obrazie przedstawiono przykład ten tryb selektora daty:

![](picker-images/image8.png)



#### <a name="date"></a>Data

Tryb danych wyświetla datę z dzień, miesiąc i rok selektora. Została skonfigurowana z `UIDatePickerMode.Date` właściwości. Na przykład:

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

Na poniższym obrazie przedstawiono przykład tego selektora daty:

![](picker-images/image7.png)

Kolejność selektory zależy od ustawień regionalnych `UIDatePicker`. Domyślnie ta zostanie ustawiona na domyślne systemu. Powyższy obraz pokazuje układ selektorów w `en_US` ustawień regionalnych, ale można go zmienić na dzień | Miesiąc | Układ roku, można użyć poniższego kodu można ustawić ustawienia regionalne:

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![](picker-images/image9.png)


#### <a name="date-and-time"></a>Data i godzina

Tryb Data i godzina Wyświetla widok shortend dat, czasu w godzinach i minutach i opcjonalnie AM lub PM dependings oznaczenia na, jeśli jest używany zegar 12 lub 24 godziny. Została skonfigurowana z `UIDatePickerMode.DateAndTime` właściwości. Na przykład:

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

Na poniższym obrazie przedstawiono przykład tego selektora daty:

![](picker-images/image6.png)

Jak [data](#Date), kolejność selektory i stosowania 12 lub 24 godziny zegara zależy od ustawień regionalnych `UIDatePicker`.

#### <a name="countdown-timer"></a>Czasomierza odliczania

Tryb czasomierza odliczania Wyświetla godzinowe i minutowe wartości. Została skonfigurowana z `UIDatePickerMode.CountDownTimer` właściwości. Na przykład:

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

Na poniższym obrazie przedstawiono przykład tego selektora daty:

![](picker-images/image5.png)

Można użyć `CountDownDuration` właściwości do przechwytywania dispayed wartości przez formant wyboru daty odliczania. Na przykład aby dodać wartość odliczania w dół do bieżącej daty, można użyć poniższego kodu:

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="formatting"></a>Formatowanie 

Wartości trybów czasu i daty oraz DateAndTime można przechwycić za pomocą `Date` właściwość UIDatePicker (na przykład: `datePickerView.Date`), czyli typu NSDate. Aby sformatować tę datę na coś więcej do odczytu, użyj [ `NSDateFormatter` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatter/). Poniższe przykłady pokazują, jak korzystać z niektórych właściwości dostępne dla tej klasy.

`DateFormat` Jest ustawiony jako ciąg do reprezentowania wyświetlanie daty:

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

`TimeStyle` Zestawy właściwości `NSDateFormatterStyle`:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

Pola `NSDateFormatterStyle` wyświetlić w następujący sposób:

![](picker-images/timestyle.png)

`DateStyle` Zestawy właściwości `NSDateFormatterStyle`:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

Pola `NSDateFormatterStyle` wyświetlić w następujący sposób:

![](picker-images/datestyle.png)

Można następnie output sformatowany NSDate na ciąg przy użyciu następującego kodu:

```csharp
dateLabel.Text = dateTimeformat.ToString(datePickerView.Date);
```

## <a name="related-links"></a>Linki pokrewne

- [PickerControl (przykład)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
