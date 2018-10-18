---
title: Kontrolka selektora w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano, jak projektować i pracować z selektora formantów w aplikacji platformy Xamarin.iOS. Omówiono w nim sposób implementacji próbnik w kodzie i w narzędziu iOS Designer.
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/14/2018
ms.openlocfilehash: 0ef33c2036b1ff2d5a7e2035ca5fa8af58672867
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "34789915"
---
# <a name="picker-control-in-xamarinios"></a>Kontrolka selektora w rozszerzeniu Xamarin.iOS

A [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) sprawia, że można wybrać wartość z listy przez poszczególne składniki interfejsu przypominającego kółka przewijania.

Formanty wyboru są często używane do wybierz datę i godzinę; Udostępnia firmy Apple [`UIDatePicker`](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/)
Klasa w tym celu.

W artykule opisano sposób wdrażania i użycia `UIPickerView` i `UIDatePicker` kontrolki.

## <a name="uipickerview"></a>UIPickerView

### <a name="implementing-a-picker"></a>Implementowanie selektora

Implementowanie selektora przy uruchamianiu nową `UIPickerView`:

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width, 
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
    )
);
```

### <a name="pickers-and-storyboards"></a>Magazynowi oraz scenorysów

Aby utworzyć selektor w **narzędzia iOS Designer**, przeciągnij **selektora widoku** z **przybornika** do powierzchni projektowej.

![Przeciągnij widoku selektora powierzchnię projektową](picker-images/image1.png "przeciągnij selektora widoku na powierzchni projektowej")

### <a name="working-with-a-picker-control"></a>Praca z formantem selektora

Selektor używa _modelu_ interakcję z danymi:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    var pickerModel = new PeopleModel(personLabel);
    personPicker.Model = pickerModel;
}
```

[ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) Klasa bazowa implementuje dwa interfejsy [`IUIPickerDataSource`](https://developer.xamarin.com/api/type/UIKit.IUIPickerViewDataSource/)
i [ `IUIPickerViewDelegate` ](https://developer.xamarin.com/api/type/UIKit.IUIPickerViewDelegate/), który zadeklarować różnych metod, które określają danych selektora i sposób obsługi interakcji:

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

Selektor może mieć wiele kolumn, lub _składniki_. Składniki podzielić się z selektora na wiele sekcji, co umożliwia wybór łatwiejsze i bardziej szczegółowych danych:

![Selektor z dwóch składników](picker-images/image3.png "selektora z dwa składniki:")

Aby określić wiele składników w selektorze, użyj [`GetComponentCount`](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.GetComponentCount/p/UIKit.UIPickerView/) 
Metoda.

### <a name="customizing-a-pickers-appearance"></a>Dostosowywanie wyglądu formantu wyboru

Aby dostosować wygląd próbnik, należy użyć [`UIPickerView.UIPickerViewAppearance`](https://developer.xamarin.com/api/type/UIKit.UIPickerView+UIPickerViewAppearance/)
klasy lub zastąpienie [ `GetView` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.GetView/p/UIKit.UIPickerView/System.nint/System.nint/UIKit.UIView/) i [ `GetRowHeight` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.GetRowHeight/p/UIKit.UIPickerView/System.nint/) metody `UIPickerViewModel`.

## <a name="uidatepicker"></a>UIDatePicker

### <a name="implementing-a-date-picker"></a>Implementowanie formantu wyboru daty

Implementowanie selektor daty przez utworzenie wystąpienia `UIDatePicker`:

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width,
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
     )
);
```

### <a name="date-pickers-and-storyboards"></a>Wyboru daty i scenorysów

Aby utworzyć selektor daty, w **iOS Designer**, przeciągnij **selektora daty** z **przybornika** do powierzchni projektowej.

![Przeciągnij selektora daty do powierzchni projektowej](picker-images/image2.png "przeciągnij selektora daty do powierzchni projektowej")

### <a name="date-picker-properties"></a>Właściwości selektora daty

#### <a name="minimum-and-maximum-date"></a>Minimalna i maksymalna data

[`MinimumDate`](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.MinimumDate/) i [ `MaximumDate` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.MaximumDate/) ograniczyć zakres dat, które są dostępne w selektora daty. Na przykład poniższy kod ogranicza wybór daty do 60 roku prowadzących do chwili obecne:

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();
components.Year = -60;
NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);
datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

> [!TIP]
> Można jawnie rzutowane `DateTime` do `NSDate`:
> ```csharp
> DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
> DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
> ```

#### <a name="minute-interval"></a>-Minutowego interwału

[ `MinuteInterval` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.MinuteInterval/) Właściwość ustawia interwał, jaką selektora wyświetli minut:

```csharp
datePickerView.MinuteInterval = 10;
```

#### <a name="mode"></a>Tryb

Wyboru daty obsługuje cztery [tryby](https://developer.xamarin.com/api/type/UIKit.UIDatePickerMode/), które zostały opisane poniżej:

##### <a name="uidatepickermodetime"></a>UIDatePickerMode.Time

`UIDatePickerMode.Time` Wyświetla czas z selektorem godzinowe i minutowe i opcjonalnie oznaczenia AM lub PM:

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

![UIDatePickerMode.Time](picker-images/image8.png "UIDatePickerMode.Time")

##### <a name="uidatepickermodedate"></a>UIDatePickerMode.Date

`UIDatePickerMode.Date` Wyświetla datę przy użyciu miesiąca, dnia i roku selektor:

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

![UIDatePickerMode.Date](picker-images/image7.png "UIDatePickerMode.Date")

Kolejność selektory zależy od ustawień regionalnych selektor daty, która domyślnie używa ustawień regionalnych systemu. Powyższy obraz pokazuje układ selektory w `en_US` ustawień regionalnych, ale następujące zmiany kolejności dzień | Miesiąc | Rok:

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![Dzień | Miesiąc | Rok](picker-images/image9.png "dzień | Miesiąc | Rok")

##### <a name="uidatepickermodedateandtime"></a>UIDatePickerMode.DateAndTime

`UIDatePickerMode.DateAndTime` Wyświetla skróconą widoku daty, godziny, w godzinach i minutach i opcjonalnie oznaczenia AM lub PM (w zależności od tego, czy używany jest zegar 12 lub 24 godziny):

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

![UIDatePickerMode.DateAndTime](picker-images/image6.png "UIDatePickerMode.DateAndTime")

Podobnie jak w przypadku [ `UIDatePickerMode.Date` ](#uidatepickermodedate), kolejność selektory i użytkowania zegar 12 lub 24 godziny zależy od ustawień regionalnych selektora daty.

> [!TIP]
> Użyj `Date` właściwości, aby przechwycić wartość wybór daty w trybie `UIDatePickerMode.Time`, `UIDatePickerMode.Date`, lub `UIDatePickerMode.DateAndTime`. Ta wartość jest przechowywana jako `NSDate`.

##### <a name="uidatepickermodecountdowntimer"></a>UIDatePickerMode.CountDownTimer

`UIDatePickerMode.CountDownTimer` Wyświetla wartości godzinowe i minutowe:

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

!["UIDatePickerMode.CountDownTimer"](picker-images/image5.png "UIDatePickerMode.CountDownTimer")

`CountDownDuration` Właściwość przechwytuje wartość wybór daty w `UIDatePickerMode.CountDownTimer` trybu. Na przykład, aby dodać wartość odliczania w dół do bieżącej daty:

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="nsdateformatter"></a>NSDateFormatter

Aby sformatować `NSDate`, użyj [ `NSDateFormatter` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatter/).

Aby użyć `NSDateFormatter`, wywołać jej [ `ToString` ](https://developer.xamarin.com/api/member/Foundation.NSDateFormatter.ToString/p/Foundation.NSDate/) metody. Na przykład:

```csharp
var date = NSDate.Now;
var formatter = new NSDateFormatter();
formatter.DateStyle = NSDateFormatterStyle.Full;
formatter.TimeStyle = NSDateFormatterStyle.Full;
var formattedDate = formatter.ToString(d);
// Tuesday, August 14, 2018 at 11:20:42 PM Mountain Daylight Time
```

##### <a name="dateformat"></a>DateFormat

[ `DateFormat` ](https://developer.xamarin.com/api/property/Foundation.NSDateFormatter.DateFormat/) Właściwość (ciąg) `NSDateFormatter` umożliwia specyfikacji formatu daty można dostosowywać:

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

##### <a name="timestyle"></a>TimeStyle

[ `TimeStyle` ](https://developer.xamarin.com/api/property/Foundation.NSDateFormatter.TimeStyle/) Właściwości ( [ `NSDateFormatterStyle` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatterStyle/)) z `NSDateFormatter` Określa formatowanie godziny na podstawie wstępnie zdefiniowanych stylów:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

Różne `NSDateFormatterStyle` wartości godziny będą wyświetlane w następujący sposób:

- `NSDateFormatterStyle.Full`: 19:46:00: 00 czasu wschodniego
- `NSDateFormatterStyle.Long`: 7:47:00 PM EDT
- `NSDateFormatterStyle.Medium`: 19:47:00: 00
- `NSDateFormatterSytle.Short`: 7:47 PM

##### <a name="datestyle"></a>DateStyle

[ `DateStyle` ](https://developer.xamarin.com/api/property/Foundation.NSDateFormatter.DateStyle/) Właściwości ( `NSDateFormatterStyle`) z `NSDateFormatter` Określa formatowanie daty na podstawie wstępnie zdefiniowanych stylów:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

Różne `NSDateFormatterStyle` wartości wyświetlanie dat w następujący sposób:

- `NSDateFormatterStyle.Full`: Środa, sierpnia 2 maja 2017 r na 19:48:00
- `NSDateFormatterStyle.Long`: 2 sierpnia 2017, 7:49 PM
- `NSDateFormatterStyle.Medium`: 2 sierpnia 2017 r. 7:49 PM
- `NSDateFormatterStyle.Short`: 8/2/17, 19:50:00

> [!NOTE]
> `DateFormat` i `DateStyle` / `TimeStyle` zapewnia różne sposoby określania formatowania daty i godziny. Ostatnio ustawione właściwości stwierdzić, że danych wyjściowych programu formatującego daty.

## <a name="related-links"></a>Linki pokrewne

- [PickerControl (przykład)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
