---
title: Widoki natywne w XAML
description: Widoki natywne z systemem iOS, Android i platformy uniwersalnej Windows można odwoływać się bezpośrednio z plików XAML zestawu narzędzi Xamarin.Forms. Na widoki natywne można ustawić właściwości i procedury obsługi zdarzeń, a ich mogą wchodzić w interakcje z widokami zestawu narzędzi Xamarin.Forms. W tym artykule pokazano, jak używać widoki natywne z plików XAML zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: b7ea75c13d84cf9fe74d7a606f6127aaa6bbe3b2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996337"
---
# <a name="native-views-in-xaml"></a>Widoki natywne w XAML

_Widoki natywne z systemem iOS, Android i platformy uniwersalnej Windows można odwoływać się bezpośrednio z plików XAML zestawu narzędzi Xamarin.Forms. Na widoki natywne można ustawić właściwości i procedury obsługi zdarzeń, a ich mogą wchodzić w interakcje z widokami zestawu narzędzi Xamarin.Forms. W tym artykule pokazano, jak używać widoki natywne z plików XAML zestawu narzędzi Xamarin.Forms._

W tym artykule omówiono następujące tematy:

- [Widoki natywne wykorzystywanie](#consuming) — proces, co umożliwia korzystanie z natywnych widoku w XAML.
- [Za pomocą natywnego powiązań](#native_bindings) — powiązania z właściwości widoki natywne i danych.
- [Przekazanie argumentów do widoki natywne](#passing_arguments) — przekazywanie argumentów do natywnego widoku konstruktorów i wywoływanie natywnych widoku metod fabryki.
- [Odwołujące się do widoków natywnych z kodu](#native_view_code) — pobieranie natywnych widoku wystąpień zadeklarowanych w pliku XAML, z jego pliku CodeBehind.
- [Tworzenie podklasy widoki natywne](#subclassing) — Tworzenie podklasy widoki natywne, aby zdefiniować interfejs API przyjaznego dla XAML.  

<a name="overview" />

## <a name="overview"></a>Omówienie

Aby osadzić natywnych wgląd w pliku XAML zestawu narzędzi Xamarin.Forms:

1. Dodaj `xmlns` deklarację przestrzeni nazw w pliku XAML dla przestrzeni nazw, który zawiera natywny widoku.
1. Utwórz wystąpienie widoku natywne w pliku XAML.

> [!NOTE]
> XAMLC muszą zostać wyłączone dla dowolnej strony XAML, które używają widoki natywne.

Zostać utworzone odwołanie natywne widoku z pliku związanego z kodem, możesz korzystać z udostępnionego projektu zasobów (SAP) i zawijania kodu specyficznego dla platformy za pomocą dyrektywy kompilacji warunkowej. Aby uzyskać więcej informacji, zobacz [odwołujące się do widoków natywnych z kodu](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Korzystanie z widoki natywne

Poniższy przykład kodu demonstruje, wykorzystywanie widoki natywne dla poszczególnych platform, aby rozwiązanie Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage):

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

A także `clr-namespace` i `assembly` dla natywnych widok przestrzeni nazw, `targetPlatform` musi być także określona. Ustawienie powinno mieć wartość do jednej z wartości [ `TargetPlatform` ](xref:Xamarin.Forms.TargetPlatform) wyliczenia i zazwyczaj będzie równa `iOS`, `Android`, lub `Windows`. W czasie wykonywania, analizator składni XAML będzie ignorować wszystkie prefiksy przestrzeni nazw XML, które mają `targetPlatform` niepasujący platformy, na którym działa aplikacja.

Każdy deklarację przestrzeni nazw można odwoływać się do dowolnej klasy lub struktury z określonego obszaru nazw. Na przykład `ios` deklarację przestrzeni nazw można odwoływać się do dowolnej klasy lub struktury ze sklepu iOS `UIKit` przestrzeni nazw. Można ustawić właściwości widoku natywnych przy użyciu XAML, ale właściwość i obiekt typy muszą być zgodne. Na przykład `UILabel.TextColor` właściwość jest ustawiona na `UIColor.Red` przy użyciu `x:Static` — rozszerzenie znaczników i `ios` przestrzeni nazw.

Właściwości możliwe do wiązania i dołączone właściwości możliwej do wiązania można również ustawić na widoki natywne za pomocą `Class.BindableProperty="value"` składni. Każdy widok natywnych jest opakowana w określonych platform `NativeViewWrapper` wystąpienia, która jest pochodną [ `Xamarin.Forms.View` ](xref:Xamarin.Forms.View) klasy. Ustawianie właściwości możliwej do wiązania lub dołączone właściwości możliwej do wiązania w widoku natywnych przesyła wartości właściwości do otoki. Na przykład można określić wyśrodkowany układ poziomy, ustawiając `View.HorizontalOptions="Center"` natywnych widoku.

> [!NOTE]
> Należy pamiętać, style, nie można używać z widoki natywne, ponieważ style tylko można wskazać właściwości, które są objęte `BindableProperty` obiektów.

Konstruktory dla systemu android widget wymagają programu Android `Context` obiekt jako argumentu, a mogą być udostępniane za pośrednictwem właściwości statycznej w `MainActivity` klasy. W związku z tym, podczas tworzenia Android widget w XAML, `Context` obiektu zwykle muszą być przekazywane do konstruktora widżetu przy użyciu `x:Arguments` atrybutem `x:Static` — rozszerzenie znaczników. Aby uzyskać więcej informacji, zobacz [przekazywania argumentów do widoki natywne](#passing_arguments).

> [!NOTE]
> Należy pamiętać nazw natywnych widoku z `x:Name` nie jest możliwe w projekcie biblioteki .NET Standard lub udostępnionego projektu zasobów (SAP). Ten sposób spowoduje wygenerowanie zmienną typu natywnego, co spowoduje błąd kompilacji. Jednak widoki natywne mogą być ujęte w `ContentView` wystąpień i pobrać w pliku związanym z kodem, pod warunkiem, że jest on używany SAP. Aby uzyskać więcej informacji, zobacz [odwołujące się do widoku natywnych z kodu](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Natywne powiązania

Powiązanie danych służy do synchronizowania z interfejsem użytkownika ze źródłem danych, a także upraszcza Wyświetla aplikacji platformy Xamarin.Forms i współpracuje ze swoimi danymi. Pod warunkiem, że obiekt źródłowy implementuje `INotifyPropertyChanged` interfejsu, zmiany w *źródła* obiektu są automatycznie przekazywane do *docelowej* obiektu powiązania, a zmiany w *docelowej* obiektu opcjonalnie może zostać przeniesiony do *źródła* obiektu.

Widoki natywne właściwości można także użyć powiązanie danych. Poniższy przykład kodu pokazuje powiązanie danych za pomocą właściwości widoki natywne:

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

Ta strona zawiera [ `Entry` ](xref:Xamarin.Forms.Entry) którego [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) wiąże właściwości `NativeSwitchPageViewModel.IsSwitchOn` właściwości. [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Strony ustawiono nowe wystąpienie klasy `NativeSwitchPageViewModel` klasy w pliku związanym z kodem, Implementowanie klas ViewModel `INotifyPropertyChanged` interfejsu.

Strona zawiera również przełącznika natywnego dla każdej platformy. Korzysta z każdego przełącznika natywnego [ `TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay) powiązanie, aby zaktualizować wartość `NativeSwitchPageViewModel.IsSwitchOn` właściwości. W związku z tym, gdy przełącznik jest wyłączony, `Entry` jest wyłączona, a gdy przełącznik jest włączony, `Entry` jest włączona. Poniższych zrzutach ekranu przedstawiono tę funkcję na każdej z platform:

![](xaml-images/native-switch-disabled.png "Wyłączone przełącznika natywnego")
![](xaml-images/native-switch-enabled.png "przełącznika natywnego włączone")

Powiązania dwukierunkowego są obsługiwane automatycznie, pod warunkiem, że implementuje właściwości macierzystej `INotifyPropertyChanged`, obsługuje obserwowania pary klucz-wartość (KVO) w systemie iOS lub jest `DependencyProperty` na platformy uniwersalnej systemu Windows. Powiadomienie o zmianie właściwości nie obsługuje jednak wiele widoki natywne. W przypadku tych widoków można określić [ `UpdateSourceEventName` ](xref:Xamarin.Forms.Binding.UpdateSourceEventName) wartości właściwości jako część wyrażenia wiązania. Tę właściwość należy ustawić na nazwę zdarzenia w widoku natywne, które powiadamia, gdy zmieniono właściwość docelowa. Następnie, kiedy wartość przełącznika natywnego zmienia, `Binding` klasy zostanie powiadomiony, że użytkownik zmienił wartość przełącznika i `NativeSwitchPageViewModel.IsSwitchOn` aktualizacji wartości właściwości.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Przekazywanie argumentów do widoki natywne

Argumenty konstruktora, może być przekazywany do widoki natywne za pomocą `x:Arguments` atrybutem `x:Static` — rozszerzenie znaczników. Ponadto, metodach fabryki natywnych widoku (`public static` metody, które zwracają obiekty lub wartości tego samego typu jak klasa lub struktura, która definiuje metody) może być wywoływany przez określenie metody przy użyciu `x:FactoryMethod` atrybut i argumenty za pomocą `x:Arguments` atrybutu.

Poniższy przykład kodu demonstruje obu tych technik:

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

[ `UIFont.FromName` ](https://developer.xamarin.com/api/member/UIKit.UIFont.FromName/) Metoda fabryki służy do ustawiania [ `UILabel.Font` ](https://developer.xamarin.com/api/property/UIKit.UILabel.Font/) nową właściwość [ `UIFont` ](https://developer.xamarin.com/api/type/UIKit.UIFont/) w systemie iOS. `UIFont` Nazwę i rozmiar, które są określone przez argumenty metody, które są elementami podrzędnymi `x:Arguments` atrybutu.

[ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/) Metoda fabryki służy do ustawiania [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/) nową właściwość [ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) w systemie Android. `Typeface` Nazwę rodziny i style, które są określone przez argumenty metody, które są elementami podrzędnymi `x:Arguments` atrybutu.

[ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) Konstruktor służy do ustawiania [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) nową właściwość `FontFamily` na uniwersalnej platformy Windows (UWP). `FontFamily` Nazwa jest określona przez argument metody, która jest elementem podrzędnym `x:Arguments` atrybutu.

> [!NOTE]
> Argumenty muszą być zgodne typy wymagane przez metodę konstruktora lub fabryki.

Poniższych zrzutach ekranu przedstawiono określenie argumentów metod i konstruktorów fabryki ustawić czcionkę w różne widoki natywne wynik:

![](xaml-images/passing-arguments.png "Ustawienia czcionki na widoki natywne")

Aby uzyskać więcej informacji na temat przekazywanie argumentów w XAML, zobacz [Passing Arguments w XAML](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Odwołujące się do widoków natywnych z kodu

Chociaż nie jest możliwe, nazwę widoku natywne z `x:Name` atrybutu, istnieje możliwość pobrania wystąpienia widoku natywnych, zadeklarowanych w pliku XAML z jego pliku związanego z kodem w projekcie dostęp do udostępnionych, pod warunkiem, że widok natywnych jest elementem podrzędnym [ `ContentView` ](xref:Xamarin.Forms.ContentView) określający `x:Name` wartość atrybutu. Następnie wewnątrz dyrektywy kompilacji warunkowej w pliku związanym z kodem należy:

1. Pobieranie [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) właściwości wartość i obsadź ją do określonych platform `NativeViewWrapper` typu.
1. Pobieranie `NativeViewWrapper.NativeElement` właściwości i go rzutować na typ natywny widoku.

Natywnych interfejsów API można następnie wywołać w widoku natywnej do wykonania żądanej operacji. To podejście zapewnia również korzyści, wiele widoki natywne XAML dla różnych platform może być elementami podrzędnymi tego samego [ `ContentView` ](xref:Xamarin.Forms.ContentView). W poniższym przykładzie kodu pokazano tej techniki:

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

W powyższym przykładzie widoki natywne dla każdej z platform są elementami podrzędnymi [ `ContentView` ](xref:Xamarin.Forms.ContentView) formantów, za pomocą `x:Name` wartość atrybutu jest używane do pobierania `ContentView` w związanym z kodem:

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

[ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) Właściwości jest dostępny do pobrania opakowana natywnych widoku w postaci specyficzne dla platformy `NativeViewWrapper` wystąpienia. `NativeViewWrapper.NativeElement` Następnie dostęp do właściwości można pobrać natywnych widoku jako typu natywnego. Widok natywnego interfejsu API, następnie wywoływana w celu wykonania żądanej operacji.

Systemów iOS i Android native przyciski współużytkować ten sam `OnButtonTap` procedura obsługi zdarzeń, ponieważ każdy przycisk natywnych zużywa `EventHandler` delegowanie w odpowiedzi na zdarzenie touch. Jednak uniwersalnej platformy Windows (UWP) używa oddzielnego `RoutedEventHandler`, co z kolei zużywa `OnButtonTap` program obsługi zdarzeń w tym przykładzie. W związku z tym, kiedy natywnych kliknięciu przycisku, `OnButtonTap` wykonuje procedury obsługi zdarzeń, która skaluje się i obraca natywne kontrolki zawartych w [ `ContentView` ](xref:Xamarin.Forms.ContentView) o nazwie `contentViewTextParent`. Poniższe zrzuty ekranu pokazują ten pojawiają się na każdej z platform:

![](xaml-images/contentview.png "ContentView zawierający natywne kontrolki")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>Widoki natywne podklasy

Wiele systemów iOS i Android widoki natywne nie są odpowiednie dla wystąpienia w XAML, ponieważ używają one metody, a nie właściwości, aby skonfigurować kontrolki. Rozwiązanie tego problemu jest podklasą widoki natywne w otoki, które definiują więcej API przyjaznego dla XAML, który używa właściwości można skonfigurować kontrolkę, a zdarzenia niezależne od platformy, który używa. Opakowana widoki natywne można być umieszczone w udostępnionych zasobów projektu (SAP) i w dyrektywy kompilacji warunkowej lub umieszczone w projektach specyficzne dla platformy i stanowiący odwołanie z XAML w projekcie biblioteki .NET Standard.

Poniższy przykład kodu demonstruje, że strona zestawu narzędzi Xamarin.Forms, który wykorzystuje należy do podklasy widoki natywne:

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

Ta strona zawiera [ `Label` ](xref:Xamarin.Forms.Label) wyświetlającą owoców wybierany przez użytkownika za pomocą natywnego formantu. `Label` Wiąże `SubclassedNativeControlsPageViewModel.SelectedFruit` właściwości. [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Strony ustawiono nowe wystąpienie klasy `SubclassedNativeControlsPageViewModel` klasy w pliku związanym z kodem, Implementowanie klas ViewModel `INotifyPropertyChanged` interfejsu.

Strona zawiera również widokiem natywnych selektora dla każdej platformy. Każdy widok natywnych Wyświetla zbiór owoców przez powiązanie jej `ItemSource` właściwość `SubclassedNativeControlsPageViewModel.Fruits` kolekcji. Zezwala użytkownikowi na pobranie owoców, jak pokazano na poniższych zrzutach ekranu:

![](xaml-images/sub-classed.png "Widoki natywne podklasy")

W systemach iOS i Android native selektora używać metod konfigurowania kontrolek. W związku z tym te formanty wyboru należy odziedziczenia do udostępnienia właściwości, aby były one przyjaznego dla XAML. Na Universal Windows Platform (platformy UWP), `ComboBox` jest już przyjaznego dla XAML i nie wymagają podklasy.

### <a name="ios"></a>iOS

Podklasy wdrożenia dla systemu iOS [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) widoku, udostępnia właściwości i zdarzenia, które mogą zostać łatwo użyte z XAML:

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

`MyUIPickerView` Klasy ujawnia `ItemsSource` i `SelectedItem` właściwości, a `SelectedItemChanged` zdarzeń. A [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) wymaga podstawowej [ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) model danych, który jest dostępny przez `MyUIPickerView` właściwości i zdarzeń. `UIPickerViewModel` Modelu danych są dostarczane przez `PickerModel` klasy:

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

`PickerModel` Klasa udostępnia podstawowy magazyn dla `MyUIPickerView` klasy, za pośrednictwem `Items` właściwości. Zawsze, gdy wybrany element w `MyUIPickerView` zmian [ `Selected` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.Selected/) metoda jest wykonywana, który służy do aktualizowania wybranego indeksu i generowane `ItemChanged` zdarzeń. Gwarantuje to, że `SelectedItem` właściwość zawsze zwraca ostatni element wybrany przez użytkownika. Ponadto `PickerModel` klasy zastępuje metody, które są używane do instalacji `MyUIPickerView` wystąpienia.

### <a name="android"></a>Android

Podklasy wdrożenia dla systemu Android [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) widoku, udostępnia właściwości i zdarzenia, które mogą zostać łatwo użyte z XAML:

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

`MySpinner` Klasy ujawnia `ItemsSource` i `SelectedObject` właściwości, a `ItemSelected` zdarzeń. Elementów wyświetlanych przez `MySpinner` klasy są dostarczane przez [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) skojarzony z widokiem i elementy są umieszczane w `Adapter` podczas `ItemsSource` najpierw ustawiono właściwość. Zawsze, gdy wybrany element w `MySpinner` klasy zmiany, `OnBindableSpinnerItemSelected` aktualizacje programu obsługi zdarzeń `SelectedObject` właściwości.

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak używać widoki natywne z plików XAML zestawu narzędzi Xamarin.Forms. Na widoki natywne można ustawić właściwości i procedury obsługi zdarzeń, a ich mogą wchodzić w interakcje z widokami zestawu narzędzi Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [NativeSwitch (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [Formularze natywne](~/xamarin-forms/platform/native-forms.md)
- [Przekazywanie argumentów w XAML](~/xamarin-forms/xaml/passing-arguments.md)
