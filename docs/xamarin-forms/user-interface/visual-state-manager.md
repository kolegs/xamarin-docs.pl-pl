---
title: Menedżer stanów wizualnych zestawu narzędzi Xamarin.Forms
description: Aby wprowadzić zmiany do elementów XAML na podstawie stanów wizualnych z kodu, należy użyć Visual State Manager.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 0fdcbd6467547647089b436a894b1bc490ba5ee1
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854821"
---
# <a name="the-xamarinforms-visual-state-manager"></a>Menedżer stanów wizualnych zestawu narzędzi Xamarin.Forms

_Aby wprowadzić zmiany do elementów XAML na podstawie stanów wizualnych z kodu, należy użyć Visual State Manager._

Menedżer stanu wizualnego (VSM) jest nowa w Xamarin.Forms 3.0. Migracja VSM zapewnia uporządkowany sposób, aby wprowadzić zmiany wizualne interfejsu użytkownika z kodu. W większości przypadków interfejsu użytkownika aplikacji jest zdefiniowany w XAML, a to XAML zawiera znaczników opisujące, jak Visual State Manager ma wpływ na elementy wizualne interfejsu użytkownika.

Migracja VSM wprowadzono pojęcie _stanów wizualnych_. Widok zestawu narzędzi Xamarin.Forms, takie jak `Button` może mieć kilka inny wygląd visual w zależności od stanu bazowego &mdash; czy go jest wyłączona, wciśnięty lub ma fokus wejścia. Są to stan przycisku.

Stany wizualne są gromadzone w _grup stanów wizualnych_. Wszystkie stany wizualne w ramach grupa stanów wizualnych wzajemnie się wykluczają. Stany wizualne i grup stanów wizualnych są identyfikowane przez ciągi zwykłego tekstu.

Zestaw narzędzi Xamarin.Forms Visual State Manager definiuje jedną o nazwie "CommonStates" z trzech stanów wizualnych — grupa stanów wizualnych:

- "Normal"
- "Wyłączone"
- "Skupia się"

Ta grupa stanów wizualnych jest obsługiwana dla wszystkich klas, które wynikają z [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), która jest klasą bazową dla [ `View` ](xref:Xamarin.Forms.View) i [ `Page` ](xref:Xamarin.Forms.Page). 

Można również definiować własne grupy stanu wizualnego i Stany wizualne, jak w tym artykule przedstawiono.

> [!NOTE]
> Deweloperzy platformy Xamarin.Forms zapoznać się z [wyzwalaczy](~/xamarin-forms/app-fundamentals/triggers.md) świadomość, że wyzwalaczy można również wprowadzać zmiany do wizualizacji w interfejsie użytkownika, w oparciu o zmiany we właściwościach widoku lub uruchomieniem zdarzenia. Jednak używanie wyzwalaczy do czynienia z różnych kombinacji tych zmian może stać się dość skomplikowane. W przeszłości Visual State Manager została wprowadzona w środowisk opartych na XAML Windows w celu złagodzenia pomyłek wynikających z kombinacji stanów wizualnych. Przy użyciu metody VSM stany wizualne w ramach grupa stanów wizualnych zawsze wzajemnie się wykluczają. W dowolnym momencie tylko jeden stan każdej grupy jest bieżący stan.

## <a name="the-common-states"></a>Typowe stanów

Visual State Manager umożliwia zawierają sekcje w pliku XAML, która może zmieniać wygląd widoku, jeśli widok jest normalne lub wyłączona lub ma fokus wprowadzania. Są to znane jako _typowych stany_.

Załóżmy, że masz `Entry` widoku na stronie, a ma wygląd `Entry` można zmienić w następujący sposób:

- `Entry` Powinny mieć różowy w tle podczas `Entry` jest wyłączona.
- `Entry` Powinny mieć tła wapna normalnie.
- `Entry` Powinni rozwinąć pozycję do dwukrotnością jej wysokości normalnej po jego wprowadził fokus.

Możesz dołączyć znaczników VSM poszczególnych widokowi lub można zdefiniować ją w stylu, jeśli ma zastosowanie do wielu widoków. W dwóch następnych sekcjach opisano te podejścia.

### <a name="vsm-markup-on-a-view"></a>Migracja VSM znaczników w widoku

Aby dołączyć VSM znaczników w celu `Entry` wyświetlanie, najpierw oddzielić `Entry` do tagu początkowego i końcowego:

```xaml
<Entry FontSize="18">

</Entry>
```

Udzielił rozmiar czcionki jawnej, ponieważ jeden z stanów użyje `FontSize` właściwość podwojenia z rozmiarem tekstu w `Entry`.

Następnie wstaw `VisualStateManager.VisualStateGroups` tagi między tych znaczników:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) jest dołączona właściwość może być powiązana, zdefiniowane przez [ `VisualStateManager` ](xref:Xamarin.Forms.VisualStateManager) klasy. (Aby uzyskać więcej informacji, dołączone właściwości możliwej do wiązania, zobacz artykuł [dołączonych właściwości](~/xamarin-forms/xaml/attached-properties.md).) Jest to sposób, w jaki `VisualStateGroups` właściwość jest dołączony do `Entry` obiektu.

`VisualStateGroups` Właściwość jest typu [ `VisualStateGroupList` ](xref:Xamarin.Forms.VisualStateGroupList), który jest kolekcją [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) obiektów. W ramach `VisualStateManager.VisualStateGroups` tagów, wstawić parę `VisualStateGroup` tagów dla każdej grupy stanów wizualnych, które ma zawierać:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Należy zauważyć, że `VisualStateGroup` tag ma `x:Name` atrybut wskazujący, nazwę grupy. `VisualStateGroup` Klasa definiuje `Name` właściwość, która może zamiast tego użyj:

```xaml
<VisualStateGroup Name="CommonStates">
```

Można użyć dowolnego `x:Name` lub `Name` , ale nie oba w tym samym elemencie.

`VisualStateGroup` Klasa definiuje właściwość o nazwie [ `States` ](xref:Xamarin.Forms.VisualStateGroup.States), który jest kolekcją [ `VisualState` ](xref:Xamarin.Forms.VisualState) obiektów. `States` jest _zawartości właściwość_ z `VisualStateGroups` , dzięki czemu mogą obejmować `VisualState` tagów bezpośrednio między `VisualStateGroup` tagów. (Zawartości, właściwości zostały omówione w artykule [Essential składnia XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties).)

Następnym krokiem jest uwzględnienie parę tagi dla każdego stanu wizualnego w tej grupie. Ponadto można je zidentyfikować przy użyciu `x:Name` lub `Name`:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">

            </VisualState>

            <VisualState x:Name="Focused">

            </VisualState>

            <VisualState x:Name="Disabled">

            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualState` Definiuje właściwość o nazwie [ `Setters` ](xref:Xamarin.Forms.VisualState.Setters), który jest kolekcją [ `Setter` ](xref:Xamarin.Forms.Setter) obiektów. Są one takie same `Setter` obiektów, które w [ `Style` ](xref:Xamarin.Forms.Style) obiektu.

`Setters` jest _nie_ właściwość content `VisualState`, więc należy uwzględnić tagi element właściwości dla `Setters` właściwości:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
    
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Teraz można wstawić co najmniej jeden `Setter` obiektów między sąsiednimi `Setters` tagów. Są to `Setter` obiekty, które określają stany wizualne, opisanych wcześniej:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Lime" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
                    <Setter Property="FontSize" Value="36" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Pink" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Każdy `Setter` tag wskazuje wartość określonej właściwości, gdy ten stan jest aktualny. Wszystkie właściwości, które odwołuje się `Setter` obiektu musi być wspierany przez właściwości możliwej do wiązania.

Kod znaczników podobny do następującego będącą podstawą **VSM w widoku** strony w **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** przykładowy program. Strona zawiera trzy `Entry` widoków, ale tylko jeden ma znaczników VSM podłączone do niego:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VsmDemos"
             x:Class="VsmDemos.MainPage"
             Title="VSM Demos">

    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />

        <Entry />

        <Label Text="Entry with VSM: " />

        <Entry>
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    
                    <VisualState x:Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState x:Name="Focused">
                        <VisualState.Setters>
                            <Setter Property="FontSize" Value="36" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Pink" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>

        <Label Text="Entry to enable 2nd Entry:" />

        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Należy zauważyć, że drugi `Entry` ma również `DataTrigger` jako część jej `Trigger` kolekcji. Powoduje to, że `Entry` wyłączenie coś, co jest wpisane w trzecim `Entry`. Poniżej znajduje się Strona przy uruchamianiu z systemem iOS, Android i platformy uniwersalnej Windows (UWP):

[![Migracja VSM na widok: wyłączone](vsm-images/VsmOnViewDisabled.png "VSM na widok — wyłączone")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Bieżący stan wizualny jest "wyłączone" więc tła drugiego `Entry` jest różowy, w systemach iOS i Android ekranów. Implementacja platformy uniwersalnej systemu Windows `Entry` nie zezwala na ustawienie tła kolorów, kiedy `Entry` jest wyłączona. 

Po wprowadzeniu jakiś tekst do trzeciego `Entry`, drugi `Entry` przełączników w stanie "Normal", a tło jest teraz wapna:

[![Migracja VSM na widok: normalny](vsm-images/VsmOnViewNormal.png "VSM na widok — normalne")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

Gdy touch drugi `Entry`, otrzymuje fokus wprowadzania. Go przechodzi w stan "Focused" i rozwija dwukrotnością jej wysokości:

[![Migracja VSM na widok: skupia się](vsm-images/VsmOnViewFocused.png "VSM na widok — skupia się")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Należy zauważyć, że `Entry` nie zachowuje tła wapna otrzymuje fokus wprowadzania. Visual State Manager przełącza między stanów wizualnych, są nie ustawiono właściwości ustawione przez poprzedniego stanu. Należy pamiętać, że Stany wizualne wzajemnie się wykluczają. Stan "Normal" oznacza jedynie, `Entry` jest włączona. Oznacza to, że `Entry` jest włączona i nie ma fokusa wejścia. 

Jeśli chcesz `Entry` aby tła wapna w stanie "Focused", Dodaj kolejny `Setter` do tego stanu wizualnego:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

Aby te `Setter` obiektów, aby zapewnić prawidłowe działanie, `VisualStateGroup` musi zawierać `VisualState` obiektów dla wszystkich stanów w tej grupie. W przypadku stanu wizualnego, który nie ma żadnych `Setter` obiekty, obejmują go mimo to jako pustą etykietę:

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>Znaczników Menedżer stanów wizualnych w stylu

Często jest to niezbędne do współużytkowania tego samego znaczników Visual State Manager między co najmniej dwóch widoków. W takim przypadku należy umieścić znaczniki w `Style` definicji.

Oto istniejące niejawne `Style` dla `Entry` elementów w **VSM na widok** strony:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

Dodaj `Setter` tagi dla `VisualStateManager.VisualStateGroups` dołączonych właściwości możliwej do wiązania:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

Właściwość zawartości dla `Setter` jest `Value`, więc wartość `Value` właściwości można określić bezpośrednio z poziomu tych znaczników. Czy właściwość jest typu `VisualStateGroupList`:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>

        </VisualStateGroupList>
    </Setter>
</Style> 
```

W ramach tych tagów można dołączyć jednego lub więcej `VisualStateGroup` obiektów:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">

            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style> 
```

W pozostałej części znaczników VSM jest taka sama jak przed.

Oto **VSM w stylu** przedstawiający pełną znaczników VSM stronę:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmInStylePage"
             Title="VSM in Style">
    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">

                            <VisualState x:Name="Normal">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>

                            <VisualState x:Name="Focused">
                                <VisualState.Setters>
                                    <Setter Property="FontSize" Value="36" />
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Pink" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />

        <Entry />
        
        <Label Text="Entry with VSM: " />

        <Entry>
            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>

        <Label Text="Entry to enable 2nd Entry:" />

        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Teraz wszystkie `Entry` widoki na tej stronie odpowiedzieć ten sam sposób, aby ich stany wizualne. Zwróć uwagę, że stan "Focused" zawiera teraz chwilę `Setter` daje każdego `Entry` wapna w tle również po jej ma fokus wejścia:

[![Migracja VSM w stylu](vsm-images/VsmInStyle.png "VSM w stylu")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>Definiowanie własnych stanów wizualnych

Każda klasa, która pochodzi od klasy `VisualElement` obsługuje trzy stany wspólnej "Normal", "Skupia się" i "Wyłączone". Wewnętrznie [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) klasy wykrywa staje się włączone lub wyłączone, lub konkretną lub po przeniesieniu fokusu i wywołuje statyczną [ `VisualStateManager.GoToState` ](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String)) metody:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Jest to jedyny kod Visual State Manager, który znajdziesz w `VisualElement` klasy. Ponieważ `GoToState` jest wywoływana dla każdego obiektu, w oparciu o każdej klasy, która jest pochodną `VisualElement`, można użyć Visual State Manager ze wszystkimi `VisualElement` obiektu, aby odpowiedzieć na te zmiany.

Co ciekawe, nazwę grupy stanu wizualnego "CommonStates" nie jest jawnie wywoływany w `VisualElement`. Nazwa grupy nie jest częścią interfejsu API dla Visual State Manager. W ramach jednego z dwóch program przykładowych pokazano do tej pory możesz zmienić nazwę grupy z "CommonStates" na jakąkolwiek inną i program będą nadal działać. Nazwa grupy jest jedynie ogólny opis stanów w tej grupie. Niejawnie przyjmuje się, że Stany wizualne w dowolnej grupie wykluczają się wzajemnie: stan jeden i tylko jeden stan jest aktualny w dowolnym momencie.

Jeśli chcesz zaimplementować swoje własne stany wizualne, musisz wywołać `VisualStateManager.GoToState` z kodu. W większości przypadków będziesz robić to wywołanie funkcji z pliku związanego z kodem klasy strony.

**Weryfikacji VSM** strony w **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** przykład pokazuje, jak używać Visual State Manager w związku z sprawdzania poprawności danych wejściowych. Plik XAML składa się z dwóch `Label` elementów `Entry`, i `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout Padding="10, 10">
        
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />

        <Entry Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />

        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter Property="TextColor" Value="Transparent" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Invalid" />
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Label>

        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid" />

                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter Property="IsEnabled" Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

VSM znaczników jest dołączony do drugiego `Label` (o nazwie `helpLabel`) i `Button` (o nazwie `submitButton`). Istnieją dwa stany wzajemnie wykluczającymi się o nazwie "Ważne" i "Nieprawidłowy". Należy zauważyć, że każda z tych dwóch grup "ValidationState" zawiera `VisualState` tagów dla "Ważne" i "Nieprawidłowy", mimo że jeden z nich jest pusta w każdym przypadku. 

Jeśli `Entry` nie zawiera prawidłowego numeru telefonu, a następnie bieżący stan to "Nieprawidłowy" i dlatego drugi `Label` jest widoczna i `Button` jest wyłączone:

[![Sprawdzanie poprawności VSM: Nieprawidłowy stan](vsm-images/VsmValidationInvalid.png "weryfikacji VSM — nieprawidłowy")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

Po wprowadzeniu prawidłowego numeru telefonu, bieżący stan staje się "Ważne". Drugi `Entry` znika i `Button` jest teraz włączony:

[![Weryfikacji VSM: Nieprawidłowy stan](vsm-images/VsmValidationValid.png "weryfikacji VSM — nieprawidłowa")](vsm-images/VsmValidationValid-Large.png#lightbox)

Plik związany z kodem jest odpowiedzialnej za obsługę `TextChanged` zdarzenie z `Entry`. Program obsługi używa wyrażenia regularnego, aby ustalić, czy ciąg wejściowy jest prawidłowy. Metody w pliku związanym z kodem o nazwie `GoToState` wywołuje statyczną `VisualStateManager.GoToState` metody dla obu `helpLabel` i `submitButton`:

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage ()
    {
        InitializeComponent ();

        GoToState(false);
    }

    void OnTextChanged(object sender, TextChangedEventArgs args)
    {
        bool isValid = Regex.IsMatch(args.NewTextValue, @"^[2-9]\d{2}-\d{3}-\d{4}$");
        GoToState(isValid);
    }

    void GoToState(bool isValid)
    {
        string visualState = isValid ? "Valid" : "Invalid";
        VisualStateManager.GoToState(helpLabel, visualState);
        VisualStateManager.GoToState(submitButton, visualState);
    }
}
```

Zauważ również, że `GoToState` metoda jest wywoływana z konstruktora, aby zainicjować stanu. Zawsze należy bieżący stan. Ale daleko w kodzie ma dowolnego odwołania do nazwy grupy stanu wizualnego, mimo że odwołuje się do niego XAML jako "ValidationStates" do celów przejrzystości. 

Należy zauważyć, że pliku CodeBehind musi uwzględniać każdy obiekt na stronie, która ma to wpływ na przez te stany wizualne i wywołać `VisualStateManager.GoToState` dla każdego z tych obiektów. W tym przykładzie jest tylko dwa obiekty ( `Label` i `Button`), ale może być kilka więcej.

Być może zastanawiasz się: Jeśli każdy obiekt na stronie, która jest zależna od tych stanów wizualnych musi odwoływać się w pliku związanym z kodem, dlaczego nie pliku związanego z kodem po prostu obiekty bezpośredni dostęp? Z pewnością można. Jednak zaletą używania VSM jest, że możecie kontrolować elementy wizualne jak reagować na innym stanie całkowicie w XAML, który przechowuje wszystkie projektu interfejsu użytkownika w jednym miejscu. Umożliwia to uniknięcie wyglądu ustawienie, uzyskując dostęp do elementów wizualnych bezpośrednio z kodem.

Może być kuszące wziąć pod uwagę wyprowadzanie klasy z `Entry` i prawdopodobnie Definiowanie właściwości ustawionej dla funkcji zewnętrznej walidacji. Klasa, która jest pochodną `Entry` następnie wywołać `VisualStateManager.GoToState` metody. Ten program będzie działać prawidłowo, ale tylko wtedy, gdy `Entry` były tylko obiekt, który według różnych stanów wizualnych. W tym przykładzie `Label` i `Button` również będzie zmienione. Nie ma możliwości dla znaczników VSM dołączone do `Entry` do kontrolowania inne obiekty na stronie, a nie dla znaczników VSM dołączone do tych innych obiektów, aby odwołać zmiana stanu wizualnego z innego obiektu.

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>Za pomocą Visual State Manager adaptacyjne układu

Xamarin.Forms, na których działa aplikacja na telefon, zwykle mogą być wyświetlane w pionowa lub pozioma współczynnik proporcji i programu Xamarin.Forms uruchomiona na komputerze można zmienić rozmiar, aby założył, wiele różnych rozmiarów i współczynniki proporcji. Dobrze zaprojektowana aplikacja może wyświetlić jego zawartość w różny sposób dla tych różnych czynników formularzy strony lub okna. 

Ta technika jest czasem nazywana _adaptacyjne układ_. Ponieważ układ adaptacyjne wyłącznie wiąże się wizualizacje programu, jest idealnym rozwiązaniem stosowania Visual State Manager.

Prostym przykładem jest aplikacja zawiera zbiór małe przyciski, które wpływają na zawartość aplikacji. W trybie portret przyciski te mogą być wyświetlane w rzędzie górnej części strony:

[![Układ adaptacyjne VSM: Pionowa](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM adaptacyjne układ — orientacja pionowa")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

W trybie poziomym tablica przycisków mogą przenieść z jednej strony, a wyświetlane w kolumnie:

[![Układ adaptacyjne VSM: Landscape —](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM układ adaptacyjne — orientacja pozioma")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

Od góry do dołu program działa na platformie uniwersalnej Windows, Android i iOS.

**Układ adaptacyjne VSM** strony w [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/) Przykładowa aplikacja definiuje grupę o nazwie "OrientationStates" z dwóch stanów wizualnych o nazwie "Pionowo" i "Landscape". (To bardziej skomplikowane podejście może opierać się na kilka różnych szerokości strony lub okna.) 

Kod znaczników VSM odbywa się w czterech miejsc w pliku XAML. `StackLayout` o nazwie `mainStack` zawiera menu i zawartość, która jest `Image` elementu. To `StackLayout` powinny mieć orientacji pionowej w trybie pionowym i orientacji poziomej w trybie poziomym:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmAdaptiveLayoutPage"
             Title="VSM Adaptive Layout">

    <StackLayout x:Name="mainStack">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup Name="OrientationStates">

                <VisualState Name="Portrait">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>

                <VisualState Name="Landscape">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        
        <ScrollView x:Name="menuScroll">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="OrientationStates">
                    
                    <VisualState Name="Portrait">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Horizontal" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Landscape">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Vertical" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
            
            <StackLayout x:Name="menuStack">
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup Name="OrientationStates">

                        <VisualState Name="Portrait">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Horizontal" />
                            </VisualState.Setters>
                        </VisualState>

                        <VisualState Name="Landscape">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Vertical" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <StackLayout.Resources>
                    <Style TargetType="Button">
                        <Setter Property="VisualStateManager.VisualStateGroups">
                            <VisualStateGroupList>
                                <VisualStateGroup Name="OrientationStates">

                                    <VisualState Name="Portrait">
                                        <VisualState.Setters>
                                            <Setter Property="HorizontalOptions" Value="CenterAndExpand" />
                                            <Setter Property="Margin" Value="10, 5" />
                                        </VisualState.Setters>
                                    </VisualState>

                                    <VisualState Name="Landscape">
                                        <VisualState.Setters>
                                            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                                            <Setter Property="HorizontalOptions" Value="Center" />
                                            <Setter Property="Margin" Value="10" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateGroupList>
                        </Setter>
                    </Style>
                </StackLayout.Resources>

                <Button Text="Banana"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="Banana.jpg" />
                
                <Button Text="Face Palm"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="FacePalm.jpg" />
                
                <Button Text="Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="monkey.png" />
                
                <Button Text="Seated Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="SeatedMonkey.jpg" />
            </StackLayout>
        </ScrollView>

        <Image x:Name="image"
               VerticalOptions="FillAndExpand"
               HorizontalOptions="FillAndExpand" />
    </StackLayout>
</ContentPage>
```

Wewnętrzny `ScrollView` o nazwie `menuScroll` i `StackLayout` o nazwie `menuStack` zaimplementować menu dla przycisków. Orientacja tych układów jest przeciwny z `mainStack`. Menu powinien być poziomy w orientacji pionowej i pionowy w trybie poziomym.

Czwartą sekcję VSM znaczników jest w stylu niejawnego przycisków samodzielnie. Ustawia ten kod znaczników `VerticalOptions`, `HorizontalOptions`, i `Margin` właściwości specyficzne dla orientacji portait i poziomej.

Ustawia plik związany z kodem `BindingContext` właściwość `menuStack` do zaimplementowania `Button` polecenia, a także dołącza obsługę do `SizeChanged` zdarzeń strony:

```csharp
public partial class VsmAdaptiveLayoutPage : ContentPage
{
    public VsmAdaptiveLayoutPage ()
    {
        InitializeComponent ();

        SizeChanged += (sender, args) =>
        {
            string visualState = Width > Height ? "Landscape" : "Portrait";
            VisualStateManager.GoToState(mainStack, visualState);
            VisualStateManager.GoToState(menuScroll, visualState);
            VisualStateManager.GoToState(menuStack, visualState);

            foreach (View child in menuStack.Children)
            {
                VisualStateManager.GoToState(child, visualState);
            }
        };

        SelectedCommand = new Command<string>((filename) =>
        {
            image.Source = ImageSource.FromResource("VsmDemos.Images." + filename);
        });

        menuStack.BindingContext = this;
    }

    public ICommand SelectedCommand { private set; get; }
}
```

`SizeChanged` Obsługi zdarzeń wywołuje `VisualStateManager.GoToState` dwóch `StackLayout` i `ScrollView` elementów i następnie pętlę elementy podrzędne `menuStack` do wywołania `VisualStateManager.GoToState` dla `Button` elementów.

Może wydawać się tak, jakby plik związany z kodem może obsługiwać zmiany orientacji bardziej bezpośrednio przez ustawienie właściwości elementów w pliku XAML, ale Visual State Manager zdecydowanie jest bardziej ustrukturyzowane podejście. Wszystkie wizualizacje są przechowywane w pliku XAML, w którym stają się łatwiejsze do sprawdzenia, konserwację i modyfikowanie.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Menedżer stanów wizualnych za pomocą Xamarin.University

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Menedżer stanów wizualnych produkt Xamarin.Forms 3.0, [platformy Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Linki pokrewne

- [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)

