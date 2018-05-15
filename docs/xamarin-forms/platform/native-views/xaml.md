---
title: Natywny widoków w języku XAML
description: Natywny widoków z systemem iOS, Android i platformy uniwersalnej systemu Windows można odwoływać się bezpośrednio z plików XAML platformy Xamarin.Forms. W widokach natywny można ustawić właściwości i procedury obsługi zdarzeń i współdziałają z widokami platformy Xamarin.Forms. W tym artykule pokazano, jak korzystać z natywnych widoków z plików XAML platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: 24e7f29e42607d4a2c957cf85dad15f659d3618e
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="native-views-in-xaml"></a>Natywny widoków w języku XAML

_Natywny widoków z systemem iOS, Android i platformy uniwersalnej systemu Windows można odwoływać się bezpośrednio z plików XAML platformy Xamarin.Forms. W widokach natywny można ustawić właściwości i procedury obsługi zdarzeń i współdziałają z widokami platformy Xamarin.Forms. W tym artykule pokazano, jak korzystać z natywnych widoków z plików XAML platformy Xamarin.Forms._

W tym artykule omówiono następujące zagadnienia:

- [Korzystanie z widoków natywnego](#consuming) — proces służący do konsumowania natywnego widoku z XAML.
- [Przy użyciu natywnych powiązań](#native_bindings) — wiązanie do i z właściwości natywnego widoków danych.
- [Przekazywanie argumentów do natywnej widoków](#passing_arguments) — przekazywanie argumentów konstruktorów natywnego widoku i wywoływanie natywnych widoku metody fabryki.
- [Odwołanie do widoków natywnych z kodu](#native_view_code) — pobieranie wystąpienia widoku natywnego zadeklarowany w pliku XAML z pliku CodeBehind.
- [Tworzenie podklas natywnego widoków](#subclassing) — tworzenie podklas natywnego widoków do definiowania API przyjaznych dla języka XAML.  

<a name="overview" />

## <a name="overview"></a>Omówienie

Aby osadzić natywnego widoku do pliku XAML platformy Xamarin.Forms:

1. Dodaj `xmlns` deklaracji przestrzeni nazw w pliku XAML dla przestrzeni nazw, która zawiera natywny widoku.
1. Tworzenie wystąpienia widoku macierzystego w pliku XAML.

> [!NOTE]
> XAMLC musi być wyłączona dla wszystkich stron XAML, które za pomocą natywnego widoków.

Aby odwołać natywnego widoku z pliku CodeBehind, należy użyć udostępnionego projektu zasobów (SAP) i zawijać specyficzne dla platformy kod dyrektywy kompilacji warunkowej. Aby uzyskać więcej informacji, zobacz [odwołujących się do widoków natywnych z kodu](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Korzystanie z widoków natywnego

Poniższy przykład kodu pokazuje, korzystanie z widoków macierzystego dla poszczególnych platform do platformy Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static androidLocal:MainActivity.Instance}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

A także określenie `clr-namespace` i `assembly` w przypadku przestrzeni nazw natywnego widoku `targetPlatform` musi być także określona. Powinna to być ustawione na jedną wartości [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) wyliczenia i zwykle jest ustawiana na `iOS`, `Android`, lub `Windows`. W czasie wykonywania, analizator XAML zignoruje wszystkie prefiksy przestrzeni nazw XML, które mają `targetPlatform` platformy, na którym działa aplikacja nie jest zgodna.

Każda deklaracja przestrzeni nazw może służyć do odwołania z określonego obszaru nazw klasy lub struktury. Na przykład `ios` deklaracji przestrzeni nazw może służyć do odwołania klasy lub struktury ze sklepu iOS `UIKit` przestrzeni nazw. Właściwości widoku natywny można ustawić za pomocą języka XAML, ale typy właściwości oraz obiekt muszą być zgodne. Na przykład `UILabel.TextColor` właściwość jest ustawiona na `UIColor.Red` przy użyciu `x:Static` — rozszerzenie znaczników i `ios` przestrzeni nazw.

Właściwości i dołączonych właściwości można również ustawić na natywnego widoków przy użyciu `Class.BindableProperty="value"` składni. Każdy widok natywnych jest ujęte w poszczególnych platform `NativeViewWrapper` wystąpienia, która jest pochodną [ `Xamarin.Forms.View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) klasy. Ustawienie właściwości możliwej do wiązania lub dołączona właściwość powiązania w widoku natywnego przenosi wartość właściwości otoki. Na przykład można określić przez ustawienie układzie poziomym wyśrodkowany `View.HorizontalOptions="Center"` w macierzystym widoku.

> [!NOTE]
> Pamiętaj, że style nie można używać z widokami natywnego, ponieważ style docelowymi mogą być tylko właściwości, które bazują na `BindableProperty` obiektów.

Konstruktory android widget wymagają Android `Context` obiekt jako argument, a to może zostać udostępniona za pośrednictwem właściwości statycznych w `MainActivity` klasy. W związku z tym, podczas tworzenia widżetu systemu Android w języku XAML, `Context` obiekt zazwyczaj musi zostać przekazany do konstruktora widżetu przy użyciu `x:Arguments` atrybutem `x:Static` — rozszerzenie znaczników. Aby uzyskać więcej informacji, zobacz [przekazywanie argumentów do natywnej widoków](#passing_arguments).

> [!NOTE]
> Należy zwrócić uwagę nazewnictwa natywnego widoku z `x:Name` w .NET Standard projektu biblioteki lub udostępnionego projektu zasobów (SAP) nie jest możliwe. W ten sposób spowoduje wygenerowanie zmiennej typu macierzystego, co spowoduje błąd kompilacji. Jednak natywnego widoki mogą być ujęte w `ContentView` wystąpień i pobierane w pliku związanym z kodem, pod warunkiem, że jest on używany SAP. Aby uzyskać więcej informacji, zobacz [odwołujących się do widoku natywnych z kodu](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Natywny powiązania

Powiązanie danych służy do synchronizowania interfejsu użytkownika ze źródłem danych i upraszcza aplikacji platformy Xamarin.Forms Wyświetla i interakcje z jego dane. Pod warunkiem, że obiekt źródłowy implementuje `INotifyPropertyChanged` interfejsu, zmiany w *źródła* obiektu są automatycznie przypisany do *docelowej* obiektu powiązania framework, a zmiany w *docelowej* obiektu opcjonalnie może zostać przeniesiony do *źródła* obiektu.

Właściwości widoków natywny można również użyć wiązania z danymi. Poniższy przykład kodu pokazuje powiązania danych przy użyciu właściwości natywnego widoków:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeSwitch"
        x:Class="NativeSwitch.NativeSwitchPage">
    <StackLayout Margin="20">
        <Label Text="Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <Entry Placeholder="This Entry is bound to the native switch" IsEnabled="{Binding IsSwitchOn}" />
        <ios:UISwitch On="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=ValueChanged}"
            OnTintColor="{x:Static ios:UIColor.Red}"
            ThumbTintColor="{x:Static ios:UIColor.Blue}" />
        <androidWidget:Switch x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

Strona zawiera [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) których [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) wiąże właściwości `NativeSwitchPageViewModel.IsSwitchOn` właściwości. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Strony ustawiono nowe wystąpienie klasy `NativeSwitchPageViewModel` klasy w pliku kodu powiązanego z wdrożeniem klasy ViewModel `INotifyPropertyChanged` interfejsu.

Strona zawiera również macierzystego przełącznika dla każdej platformy. Używa każdego przełącznika natywnego [ `TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) powiązania do aktualizacji wartość `NativeSwitchPageViewModel.IsSwitchOn` właściwości. W związku z tym, gdy przełącznik jest wyłączony, `Entry` jest wyłączone, a po włączeniu tego przełącznika `Entry` jest włączona. Poniższe zrzuty ekranu przedstawiają tę funkcję na każdej platformie:

![](xaml-images/native-switch-disabled.png "Wyłączone z macierzystego przełącznika")
![](xaml-images/native-switch-enabled.png "macierzystego przełącznika włączone")

Automatycznie są obsługiwane dwukierunkowego powiązania, pod warunkiem, że właściwości macierzystej implementuje `INotifyPropertyChanged`, obsługuje obserwowania klucz-wartość (KVO) w systemie iOS lub jest `DependencyProperty` na platformy uniwersalnej systemu Windows. Wiele widoków macierzystym nie obsługuje jednak powiadomienia o zmianie właściwości. Widoki te można określić [ `UpdateSourceEventName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.UpdateSourceEventName/) wartości właściwości jako część wyrażenia powiązania. Tej właściwości należy ustawić na nazwę zdarzenia w widoku natywnego, która sygnalizuje, gdy zmieniono właściwość target. Następnie, gdy wartość macierzystego przełącznika zostanie zmieniona, `Binding` klasa zostanie powiadomiony, że użytkownik zmienił wartość przełącznika i `NativeSwitchPageViewModel.IsSwitchOn` aktualizacji wartości właściwości.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Przekazywanie argumentów do natywnej widoków

Argumenty konstruktora mogą zostać przekazane do natywnej widoków przy użyciu `x:Arguments` atrybutem `x:Static` — rozszerzenie znaczników. Ponadto metodami factory widoku natywnego (`public static` metody, które zwracają wartości tego samego typu jako klasy lub struktury, która definiuje metody lub obiektów) może być wywoływany przez określenie metody nazwy przy użyciu `x:FactoryMethod` atrybut i argumenty przy użyciu `x:Arguments` atrybutu.

Poniższy przykład kodu pokazuje obie techniki:

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winMedia="clr-namespace:Windows.UI.Xaml.Media;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winText="clr-namespace:Windows.UI.Text;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winui="clr-namespace:Windows.UI;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows">
        ...
        <ios:UILabel Text="Simple Native Color Picker" View.HorizontalOptions="Center">
            <ios:UILabel.Font>
                <ios:UIFont x:FactoryMethod="FromName">
                    <x:Arguments>
                        <x:String>Papyrus</x:String>
                        <x:Single>24</x:Single>
                    </x:Arguments>
                </ios:UIFont>
            </ios:UILabel.Font>
        </ios:UILabel>
        <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                    Text="Simple Native Color Picker"
                    TextSize="24"
                    View.HorizontalOptions="Center">
            <androidWidget:TextView.Typeface>
                <androidGraphics:Typeface x:FactoryMethod="Create">
                    <x:Arguments>
                        <x:String>cursive</x:String>
                        <androidGraphics:TypefaceStyle>Normal</androidGraphics:TypefaceStyle>
                    </x:Arguments>
                </androidGraphics:Typeface>
            </androidWidget:TextView.Typeface>
        </androidWidget:TextView>
        <winControls:TextBlock Text="Simple Native Color Picker"
                    FontSize="20"
                    FontStyle="{x:Static winText:FontStyle.Italic}"
                    View.HorizontalOptions="Center">
            <winControls:TextBlock.FontFamily>
                <winMedia:FontFamily>
                    <x:Arguments>
                        <x:String>Georgia</x:String>
                    </x:Arguments>
                </winMedia:FontFamily>
            </winControls:TextBlock.FontFamily>
        </winControls:TextBlock>
        ...
</ContentPage>
```

[ `UIFont.FromName` ](https://developer.xamarin.com/api/member/UIKit.UIFont.FromName/) Metoda fabryki służy do ustawiania [ `UILabel.Font` ](https://developer.xamarin.com/api/property/UIKit.UILabel.Font/) właściwości na nowy [ `UIFont` ](https://developer.xamarin.com/api/type/UIKit.UIFont/) w systemie iOS. `UIFont` Nazwie i rozmiarze są określane przez argumenty metody, które są elementami podrzędnymi `x:Arguments` atrybutu.

[ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/) Metoda fabryki służy do ustawiania [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/) właściwości na nowy [ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) w systemie Android. `Typeface` Nazwy rodziny i style są określane przez argumenty metody, które są elementami podrzędnymi `x:Arguments` atrybutu.

[ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) Konstruktor jest używany do ustawiania [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) właściwości na nowy `FontFamily` na uniwersalnych platformy systemu Windows (UWP). `FontFamily` Nazwa jest określona przez argument metody, która jest elementem podrzędnym `x:Arguments` atrybutu.

> [!NOTE]
> Argumenty muszą odpowiadać typów wymaganych przez konstruktora lub metody fabryki.

Poniższe zrzuty ekranu pokazują wynik określania argumentów metody i konstruktora fabryki do ustawienia czcionki na różne widoki native:

![](xaml-images/passing-arguments.png "Ustawienia czcionki w widokach natywnego")

Aby uzyskać więcej informacji na temat przekazywanie argumentów w języku XAML, zobacz [przekazywanie argumentów w języku XAML](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Odwołanie do widoków natywnych z kodu

Chociaż nie jest możliwe, nazwę widoku macierzystego z `x:Name` atrybutu, prawdopodobnie można pobrać wystąpienia widoku natywnego, zadeklarowanego w pliku XAML z pliku CodeBehind w projekcie udostępnionym dostępu, pod warunkiem, że natywnego widoku jest elementem podrzędnym [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) , który określa `x:Name` wartość atrybutu. Następnie w kompilacji warunkowej dyrektywy w pliku związanym z kodem wykonaj następujące czynności:

1. Pobrać [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) właściwości wartości i rzutować go na poszczególnych platform `NativeViewWrapper` typu.
1. Pobrać `NativeViewWrapper.NativeElement` właściwości i rzutować go do typu natywnego widoku.

Następnie można wywołać natywnego interfejsu API w macierzystym widoku w celu wykonania żądanej operacji. To rozwiązanie oferuje również korzyści że wiele widoków natywnego XAML dla różnych platform można elementy podrzędne tego samego [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Poniższy przykład kodu pokazuje tej techniki:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Text in a TextView" />
            <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>
```

W powyższym przykładzie natywnego widoków dla każdej platformy są elementami podrzędnymi [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) formantów z `x:Name` używany do pobierania wartości atrybutu `ContentView` w kodem:

```csharp
public partial class NativeViewInsideContentViewPage : ContentPage
{
    public NativeViewInsideContentViewPage()
    {
        InitializeComponent();

#if __IOS__
        var wrapper = (Xamarin.Forms.Platform.iOS.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (UIKit.UIButton)wrapper.NativeView;
        button.SetTitle("Scale and Rotate Text", UIKit.UIControlState.Normal);
        button.SetTitleColor(UIKit.UIColor.Black, UIKit.UIControlState.Normal);
#endif
#if __ANDROID__
        var wrapper = (Xamarin.Forms.Platform.Android.NativeViewWrapper)contentViewTextParent.Content;
        var textView = (Android.Widget.TextView)wrapper.NativeView;
        textView.SetTextColor(Android.Graphics.Color.Red);
#endif
#if WINDOWS_UWP
        var textWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewTextParent.Content;
        var textBlock = (Windows.UI.Xaml.Controls.TextBlock)textWrapper.NativeElement;
        textBlock.Foreground = new Windows.UI.Xaml.Media.SolidColorBrush(Windows.UI.Colors.Red);
        var buttonWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (Windows.UI.Xaml.Controls.Button)buttonWrapper.NativeElement;
        button.Click += (sender, args) => OnButtonTap(sender, EventArgs.Empty);
#endif
    }

    async void OnButtonTap(object sender, EventArgs e)
    {
        contentViewButtonParent.Content.IsEnabled = false;
        contentViewTextParent.Content.ScaleTo(2, 2000);
        await contentViewTextParent.Content.RotateTo(360, 2000);
        contentViewTextParent.Content.ScaleTo(1, 2000);
        await contentViewTextParent.Content.RelRotateTo(360, 2000);
        contentViewButtonParent.Content.IsEnabled = true;
    }
}
```

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Dostępu do właściwości można pobrać otoką natywnego widoku jako specyficzne dla platformy `NativeViewWrapper` wystąpienia. `NativeViewWrapper.NativeElement` Następnie dostępu do właściwości można pobrać natywnego widoku jako jego typu macierzystego. Widok natywnego interfejsu API jest następnie wywoływane w celu wykonania żądanej operacji.

IOS i Android przyciski natywne udostępnianie takie same `OnButtonTap` program obsługi zdarzeń, ponieważ zużywa każdego przycisku natywnego `EventHandler` delegowanie w odpowiedzi na zdarzenie touch. Jednak platformy uniwersalnej systemu Windows (UWP) używa oddzielnej `RoutedEventHandler`, co z kolei zużywa `OnButtonTap` obsługi zdarzeń w tym przykładzie. W związku z tym po kliknięciu przycisku macierzystego, `OnButtonTap` wykonuje program obsługi zdarzeń, które skaluje i obraca macierzystego formantu zawartych w [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) o nazwie `contentViewTextParent`. Poniższe zrzuty ekranu pokazują takiej sytuacji na każdej platformie:

![](xaml-images/contentview.png "ContentView zawierający macierzystego formantu")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>Tworzenie podklas natywnego widoków

Wiele systemów iOS i Android native widoków nie są odpowiednie dla wystąpienia w języku XAML, ponieważ korzystają z metody zamiast właściwości, aby skonfigurować kontrolki. Rozwiązanie tego problemu jest podklasą natywnego widoki otoki, które definiują więcej API XAML przyjaznego używający właściwości można skonfigurować kontrolki, i który korzysta z zdarzenia niezależne od platformy. Opakowana widoków natywnego następnie można umieszczone w udostępnionych zasobów projektu (SAP) i ujęty w dyrektywy warunkowej kompilacji, lub umieszczone w projektach specyficzne dla platformy i do których odwołuje się z XAML .NET Standard projektu biblioteki.

Poniższy przykład kodu pokazuje, że strony platformy Xamarin.Forms, który wykorzystuje podklasy natywnego widoków:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SubclassedNativeControls.Droid;assembly=SubclassedNativeControls.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:SubclassedNativeControls"
        x:Class="SubclassedNativeControls.SubclassedNativeControlsPage">
    <StackLayout Margin="20">
        <Label Text="Subclassed Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <StackLayout Orientation="Horizontal">
          <Label Text="You have chosen:" />
          <Label Text="{Binding SelectedFruit}" />      
        </StackLayout>
        <iosLocal:MyUIPickerView ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectedItemChanged}" />
        <androidLocal:MySpinner x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

Strona zawiera [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wyświetlający owoców wybierany przez użytkownika z macierzystego formantu. `Label` Wiąże `SubclassedNativeControlsPageViewModel.SelectedFruit` właściwości. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Strony ustawiono nowe wystąpienie klasy `SubclassedNativeControlsPageViewModel` klasy w pliku kodu powiązanego z wdrożeniem klasy ViewModel `INotifyPropertyChanged` interfejsu.

Strona zawiera również natywnego selektora widoku dla każdej platformy. Każdy widok natywnego Wyświetla kolekcji owoców przez powiązanie jego `ItemSource` właściwości `SubclassedNativeControlsPageViewModel.Fruits` kolekcji. Umożliwia użytkownikowi pobranie owoców, jak pokazano na poniższych zrzutach ekranu:

![](xaml-images/sub-classed.png "Podklasy natywnego widoków")

W systemach iOS i Android native selektorami użyć metod można skonfigurować kontrolki. W związku z tym te selektorami musi podklasy do udostępnienia właściwości, aby je przyjaznych dla języka XAML. W systemie Windows platformy Uniwersalnej, `ComboBox` jest już przyjaznych dla języka XAML i nie wymagają podklasy.

### <a name="ios"></a>iOS

Podklasy wdrożenia iOS [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) widoku i udostępnia właściwości i zdarzenia, które mogą być łatwo używane z XAML:

```csharp
public class MyUIPickerView : UIPickerView
{
    public event EventHandler<EventArgs> SelectedItemChanged;

    public MyUIPickerView()
    {
        var model = new PickerModel();
        model.ItemChanged += (sender, e) =>
        {
            if (SelectedItemChanged != null)
            {
                SelectedItemChanged.Invoke(this, e);
            }
        };
        Model = model;
    }

    public IList<string> ItemsSource
    {
        get
        {
            var pickerModel = Model as PickerModel;
            return (pickerModel != null) ? pickerModel.Items : null;
        }
        set
        {
            var model = Model as PickerModel;
            if (model != null)
            {
                model.Items = value;
            }
        }
    }

    public string SelectedItem
    {
        get { return (Model as PickerModel).SelectedItem; }
        set { }
    }
}
```

`MyUIPickerView` Klasy ujawnia `ItemsSource` i `SelectedItem` właściwości oraz `SelectedItemChanged` zdarzeń. A [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) wymaga podstawowej [ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) modelu danych, który jest dostępny przez `MyUIPickerView` właściwości i zdarzeń. `UIPickerViewModel` Modelu danych jest zapewniana przez `PickerModel` klasy:

```csharp
class PickerModel : UIPickerViewModel
{
    int selectedIndex = 0;
    public event EventHandler<EventArgs> ItemChanged;
    public IList<string> Items { get; set; }

    public string SelectedItem
    {
        get
        {
            return Items != null && selectedIndex >= 0 && selectedIndex < Items.Count ? Items[selectedIndex] : null;
        }
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return Items != null ? Items.Count : 0;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        return Items != null && Items.Count > row ? Items[(int)row] : null;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 1;
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        selectedIndex = (int)row;
        if (ItemChanged != null)
        {
            ItemChanged.Invoke(this, new EventArgs());
        }
    }
}
```

`PickerModel` Klasa udostępnia powiązany magazyn `MyUIPickerView` klasy, za pomocą `Items` właściwości. Zawsze, gdy wybrany element w `MyUIPickerView` zmiany, [ `Selected` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.Selected/) wykonać metody, która aktualizuje wybranego indeksu i uruchamiany `ItemChanged` zdarzeń. Gwarantuje to, że `SelectedItem` właściwość zawsze zwraca ostatni element pobrane przez użytkownika. Ponadto `PickerModel` klasy zastępuje metody, które są używane do instalacji `MyUIPickerView` wystąpienia.

### <a name="android"></a>Android

Podklasy implementacji systemu Android [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) widoku i udostępnia właściwości i zdarzenia, które mogą być łatwo używane z XAML:

```csharp
class MySpinner : Spinner
{
    ArrayAdapter adapter;
    IList<string> items;

    public IList<string> ItemsSource
    {
        get { return items; }
        set
        {
            if (items != value)
            {
                items = value;
                adapter.Clear();

                foreach (string str in items)
                {
                    adapter.Add(str);
                }
            }
        }
    }

    public string SelectedObject
    {
        get { return (string)GetItemAtPosition(SelectedItemPosition); }
        set
        {
            if (items != null)
            {
                int index = items.IndexOf(value);
                if (index != -1)
                {
                    SetSelection(index);
                }
            }
        }
    }

    public MySpinner(Context context) : base(context)
    {
        ItemSelected += OnBindableSpinnerItemSelected;

        adapter = new ArrayAdapter(context, Android.Resource.Layout.SimpleSpinnerItem);
        adapter.SetDropDownViewResource(Android.Resource.Layout.SimpleSpinnerDropDownItem);
        Adapter = adapter;
    }

    void OnBindableSpinnerItemSelected(object sender, ItemSelectedEventArgs args)
    {
        SelectedObject = (string)GetItemAtPosition(args.Position);
    }
}
```

`MySpinner` Klasy ujawnia `ItemsSource` i `SelectedObject` właściwości oraz `ItemSelected` zdarzeń. Elementów wyświetlanych przez `MySpinner` klasy są dostarczane przez [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) skojarzonego z widokiem i elementy są umieszczane w `Adapter` podczas `ItemsSource` zdefiniowanych właściwości. Zawsze, gdy wybrany element w `MySpinner` klasy zmian, `OnBindableSpinnerItemSelected` aktualizacje programu obsługi zdarzeń `SelectedObject` właściwości.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób korzystać z natywnych widoków z plików XAML platformy Xamarin.Forms. W widokach natywny można ustawić właściwości i procedury obsługi zdarzeń i współdziałają z widokami platformy Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [NativeSwitch (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [Formularze natywne](~/xamarin-forms/platform/native-forms.md)
- [Przekazywanie argumentów w języku XAML](~/xamarin-forms/xaml/passing-arguments.md)
