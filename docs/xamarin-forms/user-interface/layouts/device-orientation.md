---
title: Orientacja urządzenia
description: W tym artykule opisano sposób aplikacje Xamarin.Forms układu, które wygląda świetnie w orientacji pionowej i poziomej.
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: f6ca8f0900c8bc325cc49a7484dabe5bf2534257
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38999035"
---
# <a name="device-orientation"></a>Orientacja urządzenia

Należy wziąć pod uwagę użycia aplikacji i jak orientacja pozioma może być zawarte w taki sposób, aby ulepszyć środowisko użytkownika. Układy indywidualne mogą służyć do uwzględnienia wielu orientacje i najlepsze wykorzystanie dostępnego miejsca. Na poziomie aplikacji obrót mogą zostać wyłączone lub włączone.

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>Kontrolowanie orientacji

Korzystając z zestawu narzędzi Xamarin.Forms, obsługiwane metodą kontrolowanie orientacji urządzenia jest Użyj ustawień dla każdego indywidualnego projektu.

### <a name="ios"></a>iOS

W systemach iOS, orientacja urządzenia jest skonfigurowany dla aplikacji za pomocą **Info.plist** pliku. Ten plik będzie zawierać ustawienia orientacji telefonu iPhone i iPod, a także ustawienia dla tabletu iPad, jeśli aplikacja zawiera go jako obiekt docelowy. Poniżej przedstawiono instrukcje specyficzne dla używanego środowiska IDE. Użyj opcji IDE w górnej części tego dokumentu, aby wybrać instrukcje, które chcieliby zobaczyć:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W programie Visual Studio, otwórz projekt dla systemu iOS, a następnie otwórz **Info.plist**. Plik zostanie otwarty w panelu konfiguracji, zaczynając od karty informacje o wdrożeniu dla telefonu iPhone:

![iPhone informacje o wdrożeniu w programie Visual Studio](device-orientation-images/orientation-vs-iphone.png)

Aby skonfigurować orientacji urządzenia iPad, wybierz **iPad informacje o wdrożeniu** karty w lewym górnym rogu panelu, następnie wybierz z dostępne orientacje:

![Obsługiwane orientacje urządzenia w programie Visual Studio](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W programie Visual Studio dla komputerów Mac, otwórz projekt dla systemu iOS, a następnie otwórz **Info.plist**. W obszarze **aplikacji** karcie sekcje będą dostępne dla Ustaw orientację:

![iPhone informacje o wdrożeniu w programie Visual Studio dla komputerów Mac](device-orientation-images/orientation-xam-ui.png)

Jeśli wolisz edytować wartości przy użyciu interfejsu edytora pary klucz wartość, wybierz opcję **źródła**> karta w dolnej części ekranu:

![Obsługiwane orientacje urządzenia w programie Visual Studio dla komputerów Mac](device-orientation-images/orientation-xam-source.png)

-----

### <a name="android"></a>Android

Aby kontrolować orientację w systemie Android, należy otworzyć **MainActivity.cs** i ustaw orientację przy użyciu urządzanie atrybut `MainActivity` klasy:

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Określanie orientacji platformy Xamarin.Android obsługuje kilka opcji:

- **Orientacja pozioma** &ndash; wymusza orientację aplikacji, aby być pozioma, niezależnie od tego, dane czujników.
- **Orientacja pionowa** &ndash; wymusza orientację aplikacji, aby być pionowa, niezależnie od tego, dane czujników.
- **Użytkownik** &ndash; powoduje, że aplikacja przedstawiane za pomocą orientacji preferowany przez użytkownika.
- **Za** &ndash; powoduje, że orientację aplikacji, aby być taka sama jak orientacja [działania](https://developer.xamarin.com/api/type/Android.App.Activity/) związanych z nim.
- **Czujnik** &ndash; powoduje, że orientację aplikacji będzie określany przez czujnik, nawet jeśli użytkownik wyłączył automatyczne obracanie.
- **SensorLandscape** &ndash; powoduje, że aplikacja do użycia w orientacji poziomej, podczas korzystania z danych z czujników zmienić kierunek ekranu jest skierowany w stronę (tak, aby ekran nie jest traktowany jako nogami).
- **SensorPortrait** &ndash; powoduje, że aplikacja do użycia w orientacji pionowej podczas korzystania z danych z czujników zmienić kierunek ekranu jest skierowany w stronę (tak, aby ekran nie jest traktowany jako nogami).
- **ReverseLandscape** &ndash; powoduje, że aplikacja do użycia w orientacji poziomej, połączonego z przeciwnych kierunkach zwykle w taki sposób, aby wyświetlane "nogami."
- **ReversePortrait** &ndash; powoduje, że aplikacja do użycia w orientacji pionowej, połączonego z przeciwnych kierunkach zwykle w taki sposób, aby wyświetlane "nogami."
- **FullSensor** &ndash; powoduje, że aplikacja polegać na danych z czujników do wybrania poprawnej orientacji (poza możliwe 4).
- **FullUser** &ndash; powoduje, że aplikacji do korzystania z preferencji orientacji użytkownika. Jeśli włączono automatyczne obracanie, następnie wszystkie orientacje 4 może służyć.
- **UserLandscape** &ndash; _\[nieobsługiwane\]_ powoduje, że aplikacja do użycia w orientacji poziomej, chyba że użytkownik ma automatyczne obracanie włączone, w którym to przypadku zostanie użyty czujnika do określenia orientacji. Ta opcja spowoduje przerwanie kompilacji.
- **UserPortrait** &ndash; _\[nieobsługiwane\]_ powoduje, że aplikacja do użycia w orientacji pionowej, chyba że użytkownik ma automatyczne obracanie włączone, w którym to przypadku zostanie użyty czujnika do określenia orientacji. Ta opcja spowoduje przerwanie kompilacji.
- **Zablokowane** &ndash; _\[nieobsługiwane\]_ powoduje, że aplikacja korzysta orientacji ekranu niezależnie od rodzaju jest podczas uruchamiania, bez udzielania odpowiedzi na zmiany w urządzeniu w fizycznych Orientacja. Ta opcja spowoduje przerwanie kompilacji.

Pamiętaj, że macierzystych interfejsów API systemu Android dostarczali mnóstwa kontrolę nad jak odbywa się orientację, włącznie z opcjami, które jawnie sprzeczne użytkownika wyrażone Preferencje.

### <a name="universal-windows-platform"></a>Universal Windows platform

Obsługiwane orientacje na Universal Windows Platform (platformy UWP), są ustawiane w **Package.appxmanifest** pliku. Otwarcie manifestu wyświetli panel konfiguracji, gdy można wybrać obsługiwane orientacje.

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>Reagowanie na zmiany w orientacji

Xamarin.Forms nie oferuje żadnych macierzystych zdarzeń dotyczące powiadamiania aplikacji zmiany orientacji w udostępnionego kodu. Jednak `SizeChanged` zdarzenia `Page` generowane, gdy szerokość lub wysokość `Page` zmiany. Gdy szerokość `Page` jest większa niż wysokość, urządzenie jest w trybie poziomym. Aby uzyskać więcej informacji, zobacz [wyświetlania obrazu w oparciu o orientacji ekranu](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/).

> [!NOTE]
> Brak istniejących, bezpłatny pakiet NuGet do odbierania powiadomień zmiany orientacji w udostępnionego kodu. Zobacz [repozytorium GitHub](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) Aby uzyskać więcej informacji.

Alternatywnie możliwe do zastąpienia jest [ `OnSizeAllocated` ](xref:Xamarin.Forms.Page.OnSizeAllocated*) metody `Page`, wstawianie dowolny układ zmiany danych logicznych. `OnSizeAllocated` Metoda jest wywoływana zawsze wtedy, gdy `Page` jest przydzielany nowego rozmiaru, co się stanie, whenver obrócenia urządzenia. Należy pamiętać, że implementację podstawową `OnSizeAllocated` wykonuje układ ważnych funkcji, dlatego ważne jest, aby wywoływać implementację podstawową w przesłonięciu:

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

Nie można wykonać ten krok spowoduje przerwy w działaniu strony.

Należy pamiętać, że `OnSizeAllocated` metoda może być wywoływana wiele razy podczas obracania urządzenia. Zmienianie układu każdorazowo jest marnotrawstwa zasobów i może prowadzić do migotanie. Rozważ użycie zmienną instance w obrębie strony w celu sprawdzenia, czy jest orientacja pozioma lub pionowa, a tylko narysuj element ponownie, kiedy zmiany:

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

Po wykryciu zmiany orientacji urządzenia, można dodać lub usunąć dodatkowe widoki z interfejsu użytkownika, aby reagować na zmiany w dostępnego miejsca. Rozważmy na przykład wbudowane Kalkulator na każdej z platform w orientacji pionowej:

![](device-orientation-images/calculator-portrait.png "Aplikacja Kalkulator w orientacji pionowej")

i pozioma:

![](device-orientation-images/calculator-landscape.png "Aplikacja Kalkulator w orientacji poziomej")

Należy zauważyć, że aplikacje korzystać z dostępnego miejsca, dodając więcej funkcji w orientacji poziomej.

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>Układ dynamiczny

Istnieje możliwość projektowania interfejsów przy użyciu wbudowanych układów, przeniosą oni bez problemu zmieniała podczas obracania urządzenia. Podczas projektowania interfejsów, które będą w dalszym ciągu być atrakcyjne, podczas reagowania na zmiany w orientacji należy wziąć pod uwagę następujące zasady ogólne:

- **Należy zwrócić uwagę na wskaźniki** &ndash; zmiany orientacji może powodować problemy, gdy zostaną wprowadzone pewne założenia, w odniesieniu do wskaźników. Na przykład widok, w którym zostaną nadane wystarczająco dużo miejsca w pionowej przestrzeni ekranu w orientacji pionowej 1/3 mogą nie pasować do 1/3 miejsca w pionie w orientacji poziomej.
- **Należy zachować ostrożność przy użyciu wartości bezwzględne** &ndash; wartości bezwzględne (w pikselach), które są uwzględnione w orientacji pionowej może nie mieć sensu w orientacji poziomej. Jeśli konieczne są wartości bezwzględne, za pomocą układów zagnieżdżonych wykrywać ich wpływ. Na przykład będzie można użyć wartości bezwzględnej w `TableView` `ItemTemplate` przypadku, gdy szablon elementu zawiera gwarantowane wysokość jednolite.

Powyższe zasady mają zastosowanie, gdy implementacja interfejsy dla wielu rozmiarów ekranu i są zazwyczaj najlepszym rozwiązaniem z zakresu. Dalszej części tego przewodnika wyjaśnią konkretne przykłady układów odpowiada przy użyciu podstawowego układów w interfejsie Xamarin.Forms.

> [!NOTE]
> W celu uściślenia poniższe sekcje pokazują, jak zaimplementować elastyczny układy za pomocą tylko jednego typu `Layout` w danym momencie. W praktyce jest często łatwiejsze mieszać `Layout`s, aby osiągnąć żądany układ, przy użyciu prostszych lub najbardziej intuicyjnej `Layout` dla każdego składnika.

### <a name="stacklayout"></a>StackLayout

Należy wziąć pod uwagę następujące aplikacji, wyświetlana w orientacji pionowej:

![](device-orientation-images/photo-stack-portrait.png "Aplikacja zdjęcia w orientacji pionowej")

i pozioma:

![](device-orientation-images/photo-stack-landscape.png "Aplikacja zdjęcia w orientacji poziomej")

Który jest realizowane za pomocą następujących XAML:

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

Niektóre C# służy do zmiany orientacji `outerStack` oparciu o orientacji urządzenia:

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

- `outerStack` jest dostosowywany do przedstawienia obrazu i służy jako poziomy lub pionowy stos w zależności od orientacji, aby najlepiej korzystać z dostępnego miejsca.


### <a name="absolutelayout"></a>AbsoluteLayout

Należy wziąć pod uwagę następujące aplikacji, wyświetlana w orientacji pionowej:

![](device-orientation-images/photo-abs-portrait.png "Aplikacja zdjęcia w orientacji pionowej")

i pozioma:

![](device-orientation-images/photo-abs-landscape.png "Aplikacja zdjęcia w orientacji poziomej")

Który jest realizowane za pomocą następujących XAML:

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

- Ze względu na sposób, w jaki strony została zaprojektowana nie ma potrzeby dla kod proceduralny wprowadzenie czas odpowiedzi.
- `ScrollView` Jest używana do etykiety, które mają być wyświetlane, nawet jeśli wysokość ekranu jest mniejsza niż suma stałej wysokości przycisków i obraz.


### <a name="relativelayout"></a>RelativeLayout

Należy wziąć pod uwagę następujące aplikacji, wyświetlana w orientacji pionowej:

![](device-orientation-images/photo-rel-portrait.png "Aplikacja zdjęcia w orientacji pionowej")

i pozioma:

![](device-orientation-images/photo-rel-landscape.png "Aplikacja zdjęcia w orientacji poziomej")

Który jest realizowane za pomocą następujących XAML:

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

- Ze względu na sposób, w jaki strony została zaprojektowana nie ma potrzeby dla kod proceduralny wprowadzenie czas odpowiedzi.
- `ScrollView` Jest używana do etykiety, które mają być wyświetlane, nawet jeśli wysokość ekranu jest mniejsza niż suma stałej wysokości przycisków i obraz.

### <a name="grid"></a>Siatka

Należy wziąć pod uwagę następujące aplikacji, wyświetlana w orientacji pionowej:

![](device-orientation-images/photo-grid-portrait.png "Aplikacja zdjęcia w orientacji pionowej")

i pozioma:

![](device-orientation-images/photo-grid-landscape.png "Aplikacja zdjęcia w orientacji poziomej")

Który jest realizowane za pomocą następujących XAML:

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

- Ze względu na sposób, w jaki strony została zaprojektowana ma metodę, aby zmienić położenie kontrolki siatki.


## <a name="related-links"></a>Linki pokrewne

- [Układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Przykład BusinessTumble (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
- [Układ dynamiczny (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)
- [Wyświetlanie obrazu, w oparciu o orientacji ekranu](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/)
