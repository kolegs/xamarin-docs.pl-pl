---
title: Za pomocą selektora daty
description: Zezwala użytkownikowi na wybranie daty widok platformy Xamarin.Forms
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/12/2018
ms.openlocfilehash: 0ab9d3c83b849e5ab5aac8bce9c581abd0312237
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="using-datepicker"></a>Za pomocą selektora daty

_Zezwala użytkownikowi na wybranie daty widok platformy Xamarin.Forms_

Platformy Xamarin.Forms [ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) wywołuje formant wyboru daty platform i pozwala użytkownikowi na wybranie daty. `DatePicker` Definiuje właściwości pięć:

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) typu [ `DateTime` ](https://developer.xamarin.com/api/type/System.DateTime/), jakie nie pierwszego dnia roku 1900 roku.
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) typu `DateTime`, którego wartość domyślna to ostatni dzień roku 2100.
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) typu `DateTime`, wybrana data, która domyślnie określa wartość [ `DateTime.Today` ](https://developer.xamarin.com/api/property/System.DateTime.Today/).
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) typu `string`, [standardowe](/dotnet/standard/base-types/standard-date-and-time-format-strings/) lub [niestandardowych](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET formatowania ciągu, która domyślnie określa "D", długiego Data wzorca.
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.TextColor/) typu [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/), kolor używany do wyświetlania wybranej daty, która domyślnie określa [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/).

`DatePicker` Uruchamiany [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) zdarzenie, gdy użytkownik zaznaczy datę.

> [!WARNING]
> Podczas ustawiania `MinimumDate` i `MaximumDate`, upewnij się, że `MinimumDate` zawsze jest mniejsza lub równa `MaximumDate`. W przeciwnym razie `DatePicker` zgłosi wyjątek.

Wewnętrznie `DatePicker` upewnia się, że `Date` między `MinimumDate` i `MaximumDate`włącznie. Jeśli `MinimumDate` lub `MaximumDate` jest ustawiona, aby `Date` nie jest między nimi `DatePicker` dopasuje wartości `Date`.

Wszystkie właściwości pięć bazują na [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) obiektów, które oznacza, że mogą one być stylem, a właściwości mogą być elementami docelowymi powiązania danych. `Date` Właściwość ma domyślny tryb powiązania z [ `BindingMode.TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/), co oznacza, że może być elementem docelowym powiązania danych w aplikacji, która używa [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektura.

## <a name="initializing-the-datetime-properties"></a>Inicjowanie właściwości daty i godziny

W kodzie, należy zainicjować `MinimumDate`, `MaximumDate`, i `Date` właściwości do wartości typu `DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Gdy `DateTime` wartość została określona w języku XAML, używa analizatora języka XAML `DateTime.Parse` metody z `CultureInfo.InvariantCulture` argument można przekonwertować ciągu na `DateTime` wartość. Należy określić daty w formacie dokładne: dwucyfrowe miesięcy, dni dwucyfrowe i lata czterocyfrowe oddzielone ukośniki:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Jeśli `BindingContext` właściwość `DatePicker` jest ustawione na wystąpienie elementu ViewModel, zawierający właściwości typu `DateTime` o nazwie `MinDate`, `MaxDate`, i `SelectedDate` (na przykład), można utworzyć wystąpienia `DatePicker` podobny :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

W tym przykładzie wszystkie trzy właściwości są inicjowane na odpowiadające właściwości w ViewModel. Ponieważ `Date` właściwość ma powiązanie tryb `TwoWay`, Nowa data, które użytkownik wybiera są automatycznie odzwierciedlane na ViewModel.

Jeśli `DatePicker` nie zawiera powiązanie na jego `Date` właściwości aplikacji należy dołączyć obsługi ma `DateSelected` zdarzeń za świadomość, gdy użytkownik wybierze nową datą.

## <a name="datepicker-and-layout"></a>Selektora daty i układu

Można użyć opcji nieograniczonego układ poziomy, takich jak `Center`, `Start`, lub `End` z `DatePicker`:

```xaml
<DatePicker ··· 
            HorizontalOptions="Center" 
            ··· />
```

Jednak nie jest to zalecane. W zależności od ustawienia `Format` właściwości wybrane daty mogą wymagać różnych wyświetlania szerokości. Na przykład ciąg formatu "D" powoduje, że `DateTime` na potrzeby wyświetlania dat w formacie długim i "Środa, 12 września 2018" wymaga szerokości ekranu większą niż "Piątek, 4 może 2018". W zależności od platformy, może spowodować tej różnicy `DateTime` widoku, aby zmienić szerokość w układzie lub wyświetlania do skrócenia.

> [!TIP]
> Najlepiej użyć domyślnego `HorizontalOptions` ustawienie `Fill` z `DatePicker`i nie należy używać szerokości `Auto` podczas umieszczania `DatePicker` w `Grid` komórki.

## <a name="datepicker-in-an-application"></a>Selektora daty w aplikacji

[ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) próbki zawiera dwie `DatePicker` widoków na stronie. Mogą być one używane do wybrania dwiema datami, a program oblicza liczbę dni między tymi datami. Program nie zmienia ustawienia `MinimumDate` i `MaximumDate` właściwości, więc datami musi należeć do zakresu od 1900 i 2100.

Oto pliku XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DaysBetweenDates"
             x:Class="DaysBetweenDates.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <StackLayout Margin="10">
        <Label Text="Days Between Dates"
               Style="{DynamicResource TitleStyle}"
               Margin="0, 20"
               HorizontalTextAlignment="Center" />

        <Label Text="Start Date:" />

        <DatePicker x:Name="startDatePicker"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <Label Text="End Date:" />

        <DatePicker x:Name="endDatePicker"
                    MinimumDate="{Binding Source={x:Reference startDatePicker},
                                          Path=Date}"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <StackLayout Orientation="Horizontal"
                     Margin="0, 0, 0, 30">
            <Label Text="Include both days in total: "
                   VerticalOptions="Center" />
            <Switch x:Name="includeSwitch"
                    Toggled="OnSwitchToggled" />
        </StackLayout>

        <Label x:Name="resultLabel"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Każdy `DatePicker` przypisano `Format` właściwości "D" w formacie daty długiej. Zauważ również, że `endDatePicker` obiekt ma powiązanie, którego celem jest jego `MinimumDate` właściwości. Źródło powiązania jest wybrane `Date` właściwość `startDatePicker` obiektu. Dzięki temu, że data zakończenia jest zawsze później lub taka sama jak Data rozpoczęcia. Oprócz dwóch `DatePicker` obiektów, `Switch` etykietą "Obejmują zarówno dni w sumie". 

Dwa `DatePicker` widoki mają procedurami obsługi dołączonymi do `DateSelected` zdarzenia i `Switch` dołączył obsługi do jego `Toggled` zdarzeń. Te programy obsługi zdarzeń znajdują się w pliku CodeBehind i wyzwolić nowego obliczenia dni między dwoma datami:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    void OnDateSelected(object sender, DateChangedEventArgs args)
    {
        Recalculate();
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        Recalculate();
    }

    void Recalculate()
    {
        TimeSpan timeSpan = endDatePicker.Date - startDatePicker.Date +
            (includeSwitch.IsToggled ? TimeSpan.FromDays(1) : TimeSpan.Zero);

        resultLabel.Text = String.Format("{0} day{1} between dates",
                                            timeSpan.Days, timeSpan.Days == 1 ? "" : "s");
    }
}
```

Próbki jest najpierw uruchamiania, zarówno `DatePicker` widoki są inicjowane do bieżącej daty. Poniższy zrzut ekranu przedstawia programu uruchomionego w systemach iOS, Android i platformy uniwersalnej systemu Windows:

[![Liczba dni między datami rozpoczęcia](datepicker-images/DaysBetweenDatesStart.png "liczba dni między datami rozpoczęcia")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "liczba dni między datami rozpoczęcia")

Wybranie jednej z `DatePicker` Wyświetla wywołuje wyboru daty platformy. Trzy platform zaimplementować ten formant wyboru daty w bardzo różne sposoby, ale każde podejście jest znana użytkowników tej platformy:

[![Wybierz dni między datami](datepicker-images/DaysBetweenDatesSelect.png "wybierz dni między datami")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "wybierz dni między datami")

Po zaznaczeniu dwiema datami, aplikacja wyświetla liczbę dni między tymi datami:

[![Dni między datami wynik](datepicker-images/DaysBetweenDatesResult.png "dni między datami wynik")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "dni między datami wyników")

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe DaysBetweenDates](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [Selektora daty interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)
