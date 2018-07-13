---
title: DatePicker zestawu narzędzi Xamarin.Forms
description: DatePicker to widok zestawu narzędzi Xamarin.Forms, który umożliwia użytkownikowi wybranie daty. W tym artykule wyjaśniono, jak używać DatePicker w aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/04/2018
ms.openlocfilehash: 553957bfa06c7b7a9c5261e426ebee4190de5ebb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994929"
---
# <a name="xamarinforms-datepicker"></a>DatePicker zestawu narzędzi Xamarin.Forms

_Widok zestawu narzędzi Xamarin.Forms, który umożliwia użytkownikowi wybranie daty_

Xamarin.Forms [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) wywołuje kontrolce selektora daty platformy i umożliwia użytkownikowi wybranie daty. `DatePicker` Definiuje właściwości osiem:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) typu [ `DateTime` ](xref:System.DateTime), które domyślnie używa pierwszego dnia roku 1900.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) typu `DateTime`, która domyślnie ostatni dzień roku 2100.
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) typu `DateTime`, wybraną datą, wartością domyślną jest wartość [ `DateTime.Today` ](xref:System.DateTime.Today).
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) typu `string`, [standardowa](/dotnet/standard/base-types/standard-date-and-time-format-strings/) lub [niestandardowe](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET formatowanie ciągu, który jest wartością domyślną jest "D", wzorzec daty długiej.
- [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor) typu [ `Color` ](xref:Xamarin.Forms.Color), kolor używany do wyświetlania wybraną datą, która domyślnie [ `Color.Default` ](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) typu [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), które domyślnie używa [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) typu `string`, które domyślnie używa `null`.
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) typu `double`, którego wartość domyślna to od – 1,0.

`DatePicker` Generowane [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) zdarzenie, gdy użytkownik wybierze wartość typu date.

> [!WARNING]
> Podczas ustawiania `MinimumDate` i `MaximumDate`, upewnij się, że `MinimumDate` jest zawsze mniejsza lub równa `MaximumDate`. W przeciwnym razie `DatePicker` zgłosi wyjątek.

Wewnętrznie `DatePicker` zapewnia, że `Date` między `MinimumDate` i `MaximumDate`włącznie. Jeśli `MinimumDate` lub `MaximumDate` jest ustawiony tak, aby `Date` nie jest od nich `DatePicker` skoryguje wartość `Date`.

Wszystkie właściwości osiem są wspierane przez [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) obiektów, które oznacza, że mogą one być różne i właściwości mogą być celami powiązania danych. `Date` Właściwość ma domyślny tryb powiązania z [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), co oznacza, że może być elementem docelowym powiązanie danych w aplikacji, która używa [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektura.

## <a name="initializing-the-datetime-properties"></a>Inicjowanie właściwości daty/godziny

W kodzie, można inicjować `MinimumDate`, `MaximumDate`, i `Date` właściwości do wartości typu `DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Gdy `DateTime` wartość jest określona w XAML, używa analizatora XAML `DateTime.Parse` metody z `CultureInfo.InvariantCulture` argument do konwersji ciągu na `DateTime` wartość. Daty muszą być określone w dokładny format: dwucyfrowy miesięcy, dni dwucyfrowy i lata czterocyfrowe oddzielonych ukośnikami:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Jeśli `BindingContext` właściwość `DatePicker` jest ustawione na wystąpienie elementu ViewModel, zawierający właściwości typu `DateTime` o nazwie `MinDate`, `MaxDate`, i `SelectedDate` (na przykład), można utworzyć wystąpienie `DatePicker` następująco :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

W tym przykładzie wszystkie trzy właściwości są inicjowane na odpowiadające właściwości w ViewModel. Ponieważ `Date` właściwość ma tryb powiązania `TwoWay`, nową datę, która wybiera użytkownika jest automatycznie odzwierciedlana we ViewModel.

Jeśli `DatePicker` nie zawiera powiązanie na jego `Date` właściwości aplikacji należy dołączyć program obsługi do `DateSelected` zdarzenie, aby zostać poinformowany, gdy użytkownik wybierze nową datę.

Aby uzyskać informacji na temat właściwości czcionki ustawień, zobacz [czcionki](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="datepicker-and-layout"></a>DatePicker i układu

Można użyć opcji nieograniczone układ poziomy, takich jak `Center`, `Start`, lub `End` z `DatePicker`:

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

Jednak nie jest to zalecane. W zależności od ustawień `Format` właściwości wybrane daty mogą wymagać szerokości innego ekranu. Na przykład ciąg formatu "D" powoduje, że `DateTime` na potrzeby wyświetlania dat w formacie długim i "Środa 12 września, 2018 r." wymaga szerokości ekranu większą niż "Piątek, May 4 2018 r.". W zależności od platformy, może spowodować różnica ta `DateTime` widok, aby zmienić szerokość w układzie lub do wyświetlania się być obcięty.

> [!TIP]
> Najlepiej użyć domyślnej `HorizontalOptions` ustawienie `Fill` z `DatePicker`i nie należy używać szerokości `Auto` podczas przełączania `DatePicker` w `Grid` komórki.

## <a name="datepicker-in-an-application"></a>DatePicker w aplikacji

[ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) przykład obejmuje dwa `DatePicker` widoki na swojej stronie. Mogą one używane do wybierz dwie daty, a program oblicza liczbę dni między tymi datami. Program nie zmienia ustawienia `MinimumDate` i `MaximumDate` właściwości, więc dwiema datami musi należeć do zakresu od 1900 r. do 2100.

Poniżej przedstawiono plik XAML:

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

Każdy `DatePicker` przypisano `Format` właściwość "D" dla formatu daty długiej. Zauważ również, że `endDatePicker` obiekt ma powiązania, który jest przeznaczony dla jej `MinimumDate` właściwości. Źródło powiązania jest wybrane `Date` właściwość `startDatePicker` obiektu. Dzięki temu, że data zakończenia jest zawsze później niż lub równe dacie rozpoczęcia. Oprócz dwóch `DatePicker` obiektów `Switch` jest oznaczona etykietą "Include dni łącznie".

Dwa `DatePicker` widoki oferują procedurami obsługi dołączonymi do `DateSelected` zdarzenia i `Switch` ma program obsługi podłączony do jego `Toggled` zdarzeń. Te procedury obsługi zdarzeń w pliku związanym z kodem i wyzwalać nowe obliczenie dni między dwiema datami:

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

Gdy próbki pierwszego uruchomienia, zarówno `DatePicker` widoki są inicjowane do bieżącej daty. Poniższy zrzut ekranu przedstawia uruchomiony w systemach iOS, Android i platformy uniwersalnej Windows program:

[![Liczba dni między datami rozpoczęcia](datepicker-images/DaysBetweenDatesStart.png "liczba dni między datami rozpoczęcia")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "liczba dni od daty rozpoczęcia")

Naciskając jeden z `DatePicker` Wyświetla wywołuje selektor daty platformy. Trzech platformach implementacji tego selektora daty na różne sposoby, ale każde podejście jest dobrze znanym użytkownikom tej platformy:

[![Wybierz dni między datami](datepicker-images/DaysBetweenDatesSelect.png "wybierz dni między datami")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "wybierz dni między datami")

Po zaznaczeniu dwóch dat aplikacja wyświetla liczbę dni między tymi datami:

[![Dni między datami wynik](datepicker-images/DaysBetweenDatesResult.png "dni między datami wynik")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "dni między datami wyników")

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe DaysBetweenDates](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [DatePicker — interfejs API](xref:Xamarin.Forms.DatePicker)
