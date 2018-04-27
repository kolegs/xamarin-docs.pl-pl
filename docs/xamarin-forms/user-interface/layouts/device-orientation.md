---
title: Orientacja urządzenia
description: Zrozumienie sposobu określania układu aplikacji, które wygląda świetnie w orientacji pionowej i poziomej.
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: b06b17ce8f19f7f7cabe35c23de5b61db8f71dbe
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="device-orientation"></a>Orientacja urządzenia

Należy wziąć pod uwagę sposób używania aplikacji i jak orientacji poziomej mogą być uwzględniane w taki sposób, aby ulepszyć środowisko użytkownika. Układy poszczególnych mogą służyć do uwzględnienia wielu orientacje i najlepiej użyć dostępnego miejsca. Na poziomie aplikacji obrotu może zostać wyłączone lub włączone.

Ten artykuł przeprowadzi Cię przez proces tworzenia aplikacji, które korzystają z funkcji orientacji urządzenia i zawiera następujące sekcje:

- **[Kontrolowanie orientacji](#Controlling_Orientation)**  &ndash; zrozumieć sposób kontrolowania orientację na poziomie aplikacji na każdej platformie.
- **[Reagowanie na zmiany w orientacji](#Reacting_to_Changes_in_Orientation)**  &ndash; Dowiedz się, jak otrzymywać powiadomienia o i reagowania na, zmiany w orientacji.
- **[Elastyczny układ](#Responsive_Layout)**  &ndash; Dowiedz się, jak utworzyć układy automatycznie działały w orientacji poziomej i pionowej.

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>Kontrolowanie orientacji

Podczas korzystania z platformy Xamarin.Forms, obsługiwanej metody kontrolowania orientacji urządzenia jest użycie ustawienia dla każdego pojedynczego projektu.

### <a name="ios"></a>iOS

W systemach iOS, orientacji urządzenia jest skonfigurowany dla aplikacji za pomocą **Info.plist** pliku. Ten plik będzie zawierać ustawienia orientacji urządzenia iPhone i iPod, a także ustawienia dla urządzeń iPad, jeśli aplikacja zawiera ją jako miejsce docelowe. Poniżej przedstawiono instrukcje specyficzne dla użytkownika IDE. Użyj opcji IDE w górnej części tego dokumentu, aby wybrać instrukcje, które chcesz wyświetlić:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W programie Visual Studio Otwórz projekt dla systemu iOS, a następnie otwórz **Info.plist**. Plik zostanie otwarty w panelu konfiguracji w programie na karcie informacji o wdrożeniu iPhone:

![iPhone informacji o wdrożeniu w programie Visual Studio](device-orientation-images/orientation-vs-iphone.png)

Aby skonfigurować orientacji urządzenia iPad, wybierz **iPad informacji o wdrożeniu** u góry po lewej panelu, następnie wybierz opcję z dostępnych kierunków:

![Orientacji urządzenia obsługiwane w programie Visual Studio](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W programie Visual Studio dla komputerów Mac, otwórz projekt dla systemu iOS, a następnie otwórz **Info.plist**. W obszarze **aplikacji** karcie sekcje będzie można ustawić orientację:

![iPhone informacji o wdrożeniu w programie Visual Studio dla komputerów Mac](device-orientation-images/orientation-xam-ui.png)

Jeśli wolisz edytować wartości przy użyciu interfejsu edytora klucz wartość, wybierz **źródła**> u dołu ekranu:

![Obsługiwane urządzenia orientacji w programie Visual Studio dla komputerów Mac](device-orientation-images/orientation-xam-source.png)

-----

### <a name="android"></a>Android

Aby kontrolować orientację w systemie Android, otwórz **MainActivity.cs** i ustaw orientację przy użyciu dekoracji atrybut `MainActivity` klasy:

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Określanie orientacji Xamarin.Android obsługuje kilka opcji:

- **Pozioma** &ndash; wymusza orientację aplikacji, aby być pozioma, niezależnie od danych czujnika.
- **Portret** &ndash; wymusza orientację aplikacji jako pionowy, niezależnie od danych czujnika.
- **Użytkownik** &ndash; powoduje, że aplikacja przedstawiane za pomocą orientacji preferowanym przez użytkownika.
- **Za** &ndash; powoduje, że orientację aplikacji, aby być taka sama jak orientację [działania](https://developer.xamarin.com/api/type/Android.App.Activity/) za nią.
- **Czujnik** &ndash; powoduje, że orientacja aplikacji będzie określany przez czujnika, nawet jeśli użytkownik wyłączył automatyczne obrotu.
- **SensorLandscape** &ndash; powoduje aplikacji orientację poziomą podczas używania dane czujników w celu zmianę kierunku ekranu jest ukierunkowane (tak, aby ekranu nie jest widoczne jako odwrotnie).
- **SensorPortrait** &ndash; powoduje, że aplikacja do użycia w orientacji pionowej podczas używania dane czujników w celu zmianę kierunku ekranu jest ukierunkowane (tak, aby ekranu nie jest widoczne jako odwrotnie).
- **ReverseLandscape** &ndash; powoduje aplikacji orientację poziomą, skierowane w odwrotnym kierunku z zwykle były wyświetlane "odwrócony."
- **ReversePortrait** &ndash; powoduje, że aplikacja do użycia w orientacji pionowej, skierowane w odwrotnym kierunku z zwykle były wyświetlane "odwrócony."
- **FullSensor** &ndash; powoduje, że aplikacja polegać na dane czujników, aby wybrać poprawny orientację (poza możliwe 4).
- **FullUser** &ndash; powoduje aplikacji przy użyciu preferencji orientację użytkownika. Jeśli włączono automatyczne obracanie, wszystkie orientacje 4 można użyć.
- **UserLandscape** &ndash; _\[nieobsługiwane\]_ powoduje aplikacji orientację poziomą, chyba że użytkownik ma automatyczne obracanie włączone, w którym to przypadku zostanie użyty Czujnik ustalenie orientacji. Ta opcja spowoduje przerwanie kompilacji.
- **UserPortrait** &ndash; _\[nieobsługiwane\]_ powoduje, że aplikacja do użycia w orientacji pionowej, chyba że użytkownik ma automatyczne obracanie włączone, w którym to przypadku zostanie użyty Czujnik ustalenie orientacji. Ta opcja spowoduje przerwanie kompilacji.
- **Zablokowane** &ndash; _\[nieobsługiwane\]_ powoduje, że aplikacja do użycia orientacji ekranu niezależnie od jest podczas uruchamiania, nie odpowiada na zmiany w urządzeniu użytkownika fizycznych Orientacja. Ta opcja spowoduje przerwanie kompilacji.

Należy pamiętać, że wiele kontrolę nad jak jest zarządzany orientację Podaj macierzystych interfejsów API systemu Android, włącznie z opcjami, które jawnie są sprzeczne użytkownika wyrażone Preferencje.

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

W systemie Windows platformy Uniwersalnej, orientacje obsługiwane są ustawiane w **Package.appxmanifest** pliku. Otwieranie manifest ujawni panelu konfiguracji wybraniu obsługiwanych orientacji.

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>Reagowanie na zmiany w orientacji

Platformy Xamarin.Forms nie zapewnia natywnego zdarzenia powiadamiania aplikację zmiany orientacji w kodzie udostępnionego. Jednak `SizeChanged` zdarzenie `Page` generowane, gdy szerokość lub wysokość `Page` zmiany. Gdy szerokość `Page` jest większa niż wysokość urządzenie jest w trybie krajobraz. Aby uzyskać więcej informacji, zobacz [wyświetlania obrazu w oparciu o orientacji ekranu](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/).

> [!NOTE]
> Brak istniejących, wolne pakiet NuGet do odbierania powiadomień zmiany orientacji w kodzie udostępnionego. Zobacz [repozytorium GitHub](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) Aby uzyskać więcej informacji.

Alternatywnie można zastąpić jest [ `OnSizeAllocated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnSizeAllocated(System.Double,System.Double)/) metoda `Page`, wstawianie dowolny układ Zmień logikę istnieje. `OnSizeAllocated` Metoda jest wywoływana przy każdym `Page` jest przydzielany nowy rozmiar, które odbywa się whenver obracania urządzenia. Należy pamiętać, że podstawowa implementacja `OnSizeAllocated` wykonuje układu ważne funkcje, dlatego ważne jest, aby wywoływać implementację podstawową w zastąpienia:

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

Nie można wykonać tego kroku spowoduje strony nie działa.

Należy pamiętać, że `OnSizeAllocated` metody może zostać wywołana wiele razy podczas obracania urządzenia. Zmiana układu zawsze jest niepotrzebne zasobów i może prowadzić do migotanie. Rozważ użycie zmienna wystąpienia na Twojej stronie w celu sprawdzenia, czy jest orientacja pozioma lub pionowa i odświeżyć tylko po zmianie:

```csharp
private double width = 0;
private double height = 0;

protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
    if (this.width != width || this.height != height)
    {
        this.width = width;
        this.height = height;
        //reconfigure layout
    }
}
```

Gdy wykryto zmiany orientacji urządzenia można dodać lub usunąć dodatkowe widoki z interfejsu użytkownika do reagowania na zmiany w dostępnego miejsca. Rozważmy na przykład wbudowana Kalkulator na każdej z platform w orientacji pionowej:

![](device-orientation-images/calculator-portrait.png "Kalkulator aplikacji w orientacji pionowej")

i poziomej:

![](device-orientation-images/calculator-landscape.png "Kalkulator aplikacji w orientacji poziomej")

Należy zauważyć, że aplikacje wykorzystać dostępne miejsce, dodając więcej funkcji w orientacji poziomej.

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>Elastyczny układ

Istnieje możliwość interfejsy projektu za pomocą wbudowanych układów, dzięki czemu są bezpiecznie przejście podczas obracania urządzenia. Podczas projektowania interfejsów, które nadal będzie atrakcyjne w przypadku odpowiedzi na zmiany w orientacji należy wziąć pod uwagę następujące zasady ogólne:

- **Należy zwrócić uwagę na stosunek** &ndash; zmiany w orientacji może spowodować problemy w przypadku pewne założenia zostały wprowadzone w odniesieniu do wskaźników. Na przykład widok, w którym będzie mieć wystarczająco dużo miejsca w 1/3 przestrzeń w pionie ekranu w orientacji pionowej mogą nie pasować do 1/3 przestrzeń w pionie w orientacji poziomej.
- **Należy zachować ostrożność przy wartości bezwzględne** &ndash; wartości bezwzględne (w pikselach), które warto w orientacji pionowej nie może być uzasadniona w orientacji poziomej. Jeśli konieczne są wartości bezwzględne, za pomocą układów zagnieżdżonych wykrywać ich wpływu. Na przykład, będzie można użyć wartości bezwzględnej `TableView` `ItemTemplate` Jeśli szablon elementu zawiera gwarantowane wysokość uniform.

Powyższe zasady mają zastosowanie także do zazwyczaj implementowanie interfejsów dla wielu rozmiarów ekranu i są traktowane jako najlepsze rozwiązania. Szczegółowe przykłady układów reakcji przy użyciu podstawowego układów w platformy Xamarin.Forms objaśnia dalszej części tego przewodnika.

> [!NOTE]
> Dla uzyskania przejrzystości, poniższe sekcje przedstawiają sposób wdrożenia układów odpowiadać za pomocą tylko jednego typu `Layout` naraz. W praktyce, często jest łatwiejsze mieszać `Layout`s, aby osiągnąć żądany układ, przy użyciu prostszych lub najbardziej intuicyjnego `Layout` dla każdego składnika.

### <a name="stacklayout"></a>StackLayout

Należy wziąć pod uwagę następujące aplikacji, wyświetlane w orientacji pionowej:

![](device-orientation-images/photo-stack-portrait.png "Zdjęcie aplikacji w orientacji pionowej")

i poziomej:

![](device-orientation-images/photo-stack-landscape.png "Zdjęcie aplikacji w orientacji poziomej")

Które jest realizowane za pomocą następujących XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.StackLayoutPageXaml"
Title="Stack Photo Editor - XAML">
    <ContentPage.Content>
        <StackLayout Spacing="10" Padding="5" Orientation="Vertical"
        x:Name="outerStack"> <!-- can change orientation to make responsive -->
            <ScrollView>
                <StackLayout Spacing="5" HorizontalOptions="FillAndExpand"
                    WidthRequest="1000">
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Name: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer.jpg"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Date: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="07/05/2015"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Tags:" WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer, tiger"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Button Text="Save" HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                </StackLayout>
            </ScrollView>
            <Image  Source="deer.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Niektóre C# służy do zmiany orientacji `outerStack` oparte na orientacji urządzenia:

```csharp
protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            outerStack.Orientation = StackOrientation.Horizontal;
        } else {
            outerStack.Orientation = StackOrientation.Vertical;
        }
    }
}
```

Należy pamiętać o następujących kwestiach:

- `outerStack` jest dostosowany do prezentowania obrazu i formanty jako pozioma lub pionowa stosu w zależności od orientacji, aby najlepiej wykorzystać dostępnego miejsca.


### <a name="absolutelayout"></a>AbsoluteLayout

Należy wziąć pod uwagę następujące aplikacji, wyświetlane w orientacji pionowej:

![](device-orientation-images/photo-abs-portrait.png "Zdjęcie aplikacji w orientacji pionowej")

i poziomej:

![](device-orientation-images/photo-abs-landscape.png "Zdjęcie aplikacji w orientacji poziomej")

Które jest realizowane za pomocą następujących XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.AbsoluteLayoutPageXaml"
Title="AbsoluteLayout - XAML" BackgroundImage="deer.jpg">
    <ContentPage.Content>
        <AbsoluteLayout>
            <ScrollView AbsoluteLayout.LayoutBounds="0,0,1,1"
                AbsoluteLayout.LayoutFlags="PositionProportional,SizeProportional">
                <AbsoluteLayout>
                    <Image Source="deer.jpg"
                        AbsoluteLayout.LayoutBounds=".5,0,300,300"
                        AbsoluteLayout.LayoutFlags="PositionProportional" />
                    <BoxView Color="#CC1A7019" AbsoluteLayout.LayoutBounds=".5
                        300,.7,50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" />
                    <Label Text="deer.jpg" AbsoluteLayout.LayoutBounds = ".5
                        310,1, 50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" HorizontalTextAlignment="Center" TextColor="White" />
                </AbsoluteLayout>
            </ScrollView>
            <Button Text="Previous" AbsoluteLayout.LayoutBounds="0,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional"
                BackgroundColor="White" TextColor="Green" BorderRadius="0" />
            <Button Text="Next" AbsoluteLayout.LayoutBounds="1,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional" BackgroundColor="White"
                    TextColor="Green" BorderRadius="0" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

Należy pamiętać o następujących kwestiach:

- Ze względu na sposób, w jaki strony została zaprojektowana nie istnieje potrzeba procedurach kodu wprowadzenie czas odpowiedzi.
- `ScrollView` Jest używany do Zezwalaj etykiety, które mają być wyświetlane, nawet jeśli wysokości ekranu jest mniejsza niż suma stałej wysokości przycisków i obrazu.


### <a name="relativelayout"></a>RelativeLayout

Należy wziąć pod uwagę następujące aplikacji, wyświetlane w orientacji pionowej:

![](device-orientation-images/photo-rel-portrait.png "Zdjęcie aplikacji w orientacji pionowej")

i poziomej:

![](device-orientation-images/photo-rel-landscape.png "Zdjęcie aplikacji w orientacji poziomej")

Które jest realizowane za pomocą następujących XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.RelativeLayoutPageXaml"
Title="RelativeLayout - XAML"
BackgroundImage="deer.jpg">
    <ContentPage.Content>
        <RelativeLayout x:Name="outerLayout">
            <BoxView BackgroundColor="#AA1A7019"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}" />
            <ScrollView
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}">
                <RelativeLayout>
                    <Image Source="deer.jpg" x:Name="imageDeer"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.8}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.1}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=10}" />
                    <Label Text="deer.jpg" HorizontalTextAlignment="Center"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=1}"
                        RelativeLayout.HeightConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=75}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToView,ElementName=imageDeer,Property=Height,Factor=1,Constant=20}" />
                </RelativeLayout>

            </ScrollView>

            <Button Text="Previous" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                 />
            <Button Text="Next" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                />
        </RelativeLayout>
    </ContentPage.Content>
</ContentPage>

```

Należy pamiętać o następujących kwestiach:

- Ze względu na sposób, w jaki strony została zaprojektowana nie istnieje potrzeba procedurach kodu wprowadzenie czas odpowiedzi.
- `ScrollView` Jest używany do Zezwalaj etykiety, które mają być wyświetlane, nawet jeśli wysokości ekranu jest mniejsza niż suma stałej wysokości przycisków i obrazu.

### <a name="grid"></a>Siatka

Należy wziąć pod uwagę następujące aplikacji, wyświetlane w orientacji pionowej:

![](device-orientation-images/photo-grid-portrait.png "Zdjęcie aplikacji w orientacji pionowej")

i poziomej:

![](device-orientation-images/photo-grid-landscape.png "Zdjęcie aplikacji w orientacji poziomej")

Które jest realizowane za pomocą następujących XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.GridPageXaml"
Title="Grid - XAML">
    <ContentPage.Content>
        <Grid x:Name="outerGrid">
            <Grid.RowDefinitions>
                <RowDefinition Height="*" />
                <RowDefinition Height="60" />
            </Grid.RowDefinitions>
            <Grid x:Name="innerGrid" Grid.Row="0" Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Image Source="deer.jpg" Grid.Row="0" Grid.Column="0" HeightRequest="300" WidthRequest="300" />
                <Grid x:Name="controlsGrid" Grid.Row="0" Grid.Column="1" >
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="*" />
                    </Grid.ColumnDefinitions>
                    <Label Text="Name:" Grid.Row="0" Grid.Column="0" />
                    <Label Text="Date:" Grid.Row="1" Grid.Column="0" />
                    <Label Text="Tags:" Grid.Row="2" Grid.Column="0" />
                    <Entry Grid.Row="0" Grid.Column="1" />
                    <Entry Grid.Row="1" Grid.Column="1" />
                    <Entry Grid.Row="2" Grid.Column="1" />
                </Grid>
            </Grid>
            <Grid x:Name="buttonsGrid" Grid.Row="1">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Previous" Grid.Column="0" />
                <Button Text="Save" Grid.Column="1" />
                <Button Text="Next" Grid.Column="2" />
            </Grid>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Wraz z następujących procedurach kod obsługujący obrotu zmiany:

```csharp
private double width;
private double height;

protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            innerGrid.RowDefinitions.Clear();
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.RowDefinitions.Add (new RowDefinition{ Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 1, 0);
        } else {
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Auto) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 0, 1);
        }
    }
}
```

Należy pamiętać o następujących kwestiach:

- Ze względu na sposób, w jaki strony została zaprojektowana Brak metodę Zmień zasady umieszczania siatki formantów.


## <a name="related-links"></a>Linki pokrewne

- [Układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Przykład BusinessTumble (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
- [Elastyczny układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)
- [Wyświetl obraz oparty na orientacji ekranu](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/)
