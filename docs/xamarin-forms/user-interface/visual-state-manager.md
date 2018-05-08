---
title: Menedżer stanu Visual platformy Xamarin.Forms
description: Użyj Visual State Manager, aby wprowadzić zmiany w elementach XAML oparte na stany wizualne ustawić z kodu.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/01/2018
ms.openlocfilehash: 15519e504f7eec7b85bacb439e729b8be2422888
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="the-xamarinforms-visual-state-manager"></a>Menedżer stanu Visual platformy Xamarin.Forms

_Użyj Visual State Manager, aby wprowadzić zmiany w elementach XAML oparte na stany wizualne ustawić z kodu._

Menedżer stanu wizualnego (VSM) stanowi nowość w 3.0 platformy Xamarin.Forms. Migracja VSM umożliwia strukturalnych dokonać zmiany wizualne interfejsu użytkownika z kodu. W większości przypadków, interfejs użytkownika aplikacji jest zdefiniowany w języku XAML, i ten kod XAML zawiera znaczników opisujące, jak Visual State Manager ma wpływ na elementy wizualne interfejsu użytkownika.

Migracja VSM pojęcia związane z _stany wizualne_. Wyświetl platformy Xamarin.Forms, takich jak `Button` może mieć kilka różnych wystąpień visual w zależności od stanu źródłowego &mdash; czy go jest wyłączona lub naciśnięcie lub ma wejściowych fokus. Są to stan przycisku.

Stany wizualne są gromadzone w _grup stanu wizualnego_. Stany wizualne w obrębie grupy stanu wizualnego wzajemnie się wykluczają. Grup stany wizualne i stan wizualny są identyfikowane przez ciągi zwykły tekst.

Wydanie początkowej Xamarin.Florms Visual State Manager definiuje jedną grupę stanu wizualnego z trzy stany wizualne o nazwie "CommonStates":

- "Normal"
- "Wyłączone"
- "Fokus"

Ta grupa stan wizualny jest obsługiwana dla wszystkich klas, które pochodzą z [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/), która jest klasą bazową dla [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) i [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). 

Możesz również definiować własne grupy stanu wizualnego i Stany wizualne, co w tym artykule przedstawiono.

> [!NOTE]
> Deweloperom platformy Xamarin.Forms zapoznać się z [wyzwalaczy](~/xamarin-forms/app-fundamentals/triggers.md) wiedzą, że wyzwalaczy można także wprowadzić zmiany do elementów wizualnych w interfejsie użytkownika na podstawie zmian w widoku właściwości lub uruchamiania zdarzeń. Jednak przy użyciu wyzwalaczy radzenia sobie z różnych kombinacji tych zmian może stać się bardzo trudne. W przeszłości Visual State Manager została wprowadzona w środowiskach opartych na języku XAML systemu Windows w celu złagodzenia pomyłek wynikających z kombinacji stany wizualne. Przy użyciu metody VSM stany wizualne w obrębie grupy stanu wizualnego zawsze wzajemnie się wykluczają. W dowolnym momencie bieżący stan jest tylko jeden stanu w każdej grupie.

## <a name="the-common-states"></a>Typowe stanów

Wydanie początkowej Visual State Manager umożliwia zawierają sekcje w pliku XAML, które można zmienić wygląd widoku, jeśli widok normalny lub wyłączona lub ma fokus wprowadzania. Są one nazywane _wspólnej stanów_.

Na przykład, załóżmy, że masz `Entry` widoku na stronie. Poniżej przedstawiono sposób wygląd `Entry` zmiany:

- `Entry` Powinien mieć różowy w tle podczas `Entry` jest wyłączona.
- `Entry` Powinien mieć tła wapna normalnie.
- `Entry` Należy rozwinąć dwukrotnie wysokość normalne, gdy ma wejściowych fokus.

Znaczników VSM można dołączyć do poszczególnych widoku lub można zdefiniować ją w stylu jeśli ma to zastosowanie do wielu widoków. W dwóch następnych sekcjach opisano tych metod.

### <a name="vsm-markup-on-a-view"></a>Kod znaczników VSM na widoku

Aby dołączyć VSM znaczników do `Entry` wyświetlić, najpierw oddziel `Entry` do tagu początkowego i końcowego:

```xaml
<Entry FontSize="18">

</Entry>
```

Została podana rozmiar czcionki jawnej, ponieważ jeden z stanów użyje `FontSize` właściwości podwojenia rozmiar tekstu w `Entry`.

Następnie wstaw `VisualStateManager.VisualStateGroups` tagi między tych tagów:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

Może to wyglądać nieco dziwne. Zazwyczaj jest tylko kod znaczników, który pojawi się między dwoma tagami tego rodzaju elementów zawartości lub właściwości oraz `VisualStateManager.VisualStateGroups` tag nie jest ani.

Jest to prawne składni języka XAML, ponieważ [ `VisualStateGroups` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty/) jest dołączona właściwość powiązania zdefiniowane przez [ `VisualStateManager` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualStateManager.VisualStateGroups) klasy. (Aby uzyskać więcej informacji na dołączone właściwości możliwej do wiązania, zobacz artykuł [dołączone właściwości](~/xamarin-forms/xaml/attached-properties.md).) Jest to sposób `VisualStateGroups` zostanie dołączona właściwość `Entry` obiektu.

`VisualStateGroups` Właściwość jest typu [ `VisualStateGroupList` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualStateGroupList/), która jest kolekcją [ `VisualStateGroup` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualStateGroup/) obiektów. W ramach `VisualStateManager.VisualStateGroups` tagów, wstawić parę `VisualStateGroup` tagi dla każdej grupy stany wizualne, które mają zostać uwzględnione:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Zwróć uwagę, że `VisualStateGroup` tag ma `x:Name` atrybut wskazujący nazwę grupy. `VisualStateGroup` Klasa definiuje `Name` właściwość, która służy to zamiast:

```xaml
<VisualStateGroup Name="CommonStates">
```

`VisualStateGroup` Klasa definiuje właściwość o nazwie [ `States` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualStateGroup/States/), która jest kolekcją [ `VisualState` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualState/) obiektów. `States` Właściwość content jest `VisualStateGroups` , mogą obejmować `VisualState` znaczniki bezpośrednio między `VisualStateGroup` tagów.

Następnym krokiem jest uwzględnienie pary znaczników dla każdego stanu wizualnego w tej grupie. Ponadto można je zidentyfikować przy użyciu `x:Name` lub `Name`:

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

`VisualState` Definiuje właściwości o nazwie [ `Setters` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualState/Setters/), która jest kolekcją [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) obiektów. Są one takie same `Setter` obiektów, które są używane w [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) obiektu.

`Setters` jest _nie_ właściwość content `VisualState`, więc konieczne jest stosowanie tagów element właściwości dla `Setters` właściwości:

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

Można teraz wstawić jeden lub więcej `Setter` obiektów każda para `Setters` tagów. Są to `Setter` obiekty, które określają stany wizualne opisanych wcześniej:

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

Każdy `Setter` tag wskazuje wartość określonej właściwości, gdy ten stan jest aktualny. Wszystkie właściwości odwołuje się `Setter` obiektu musi był zabezpieczony za pomocą właściwości możliwej do wiązania.

Kod znaczników podobny do poniższego jest podstawą **VSM w widoku** strony **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** przykładowy program. Strona zawiera trzy `Entry` widoków, ale tylko jeden ma znacznika VSM do niego dołączony:

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

Zwróć uwagę, że druga `Entry` ma również `DataTrigger` jako część jej `Trigger` kolekcji. Powoduje to `Entry` wyłączenia coś są wpisywane w trzecim polu `Entry`. Oto stronę przy uruchamianiu z systemem iOS, Android i platformy uniwersalnej systemu Windows (UWP):

[![Migracja VSM na widoku: wyłączone](vsm-images/VsmOnViewDisabled.png "VSM na widok — wyłączone")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Bieżący stan visual jest "wyłączone" tak tła drugiego `Entry` jest różowy w systemach iOS i Android ekranów. Implementacja platformy uniwersalnej systemu Windows `Entry` nie zezwala na ustawienie tła kolor, kiedy `Entry` jest wyłączona. 

Po wprowadzeniu informacji do innych `Entry`, drugi `Entry` przełączników w stanie "Normal", a tło jest teraz wapna:

[![Migracja VSM na widoku: normalny](vsm-images/VsmOnViewNormal.png "VSM na widok - normalne")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

Gdy touch drugi `Entry`, otrzymuje fokus wprowadzania. Zmienia na stan "Focused", a rozszerza dwukrotnie wysokość:

[![Migracja VSM na widoku: fokus](vsm-images/VsmOnViewFocused.png "VSM na widok - fokus")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Zwróć uwagę, że `Entry` nie zachowuje tła wapna otrzymuje fokus wprowadzania. Visual State Manager przełącza między stany wizualne, są nie ustawiono właściwości ustawione przez poprzedniego stanu. Należy pamiętać, że Stany wizualne wzajemnie się wykluczają. Stan "Normal" oznacza wyłącznie to `Entry` jest włączona. Oznacza, że `Entry` jest włączona i nie ma fokus wprowadzania. 

Jeśli chcesz `Entry` aby tła wapna w stanie "Focused", należy dodać kolejne `Setter` do tego stanu wizualnego:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

Aby te `Setter` obiektów działało poprawnie, `VisualStateGroup` musi zawiera `VisualState` obiektów dotyczące wszystkich stanów w tej grupie. W przypadku stanu wizualnego, który nie ma żadnych `Setter` obiektów, dołącz ją mimo to jako pusty:

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="vsm-markup-in-a-style"></a>Migracja VSM znaczników w stylu

Często jest to niezbędne do współużytkowania tego samego znacznika Visual State Manager między co najmniej dwa widoki. W takim przypadku należy umieścić znaczniki w `Style` definicji.

Oto istniejące niejawne `Style` dla `Entry` elementów w **VSM na widoku** strony:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

Dodaj `Setter` znaczniki dla `VisualStateManager.VisualStateGroups` dołączona właściwość powiązania:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

Właściwość zawartości `Setter` jest `Value`, dlatego wartość `Value` bezpośrednio z poziomu tych tagów można określić właściwości. Czy właściwość jest typu `VisualStateGroupList`:

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

W ramach tych tagów może zawierać jeden lub więcej `VisualStateGroup` obiektów:

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

Oto **VSM w stylu** strona wyświetlająca pełną znaczników VSM:

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

Teraz wszystkie `Entry` widoków na tej stronie odpowiedzieć ten sam sposób, aby ich stany wizualne. Spójrz również, że stan "Focused" teraz obejmuje drugiej `Setter` zapewniającą każdego `Entry` wapna w tle również podczas wprowadzania fokus:

[![Migracja VSM w stylu](vsm-images/VsmInStyle.png "VSM w stylu")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>Definiowanie własnych stany wizualne

Każda klasa, która jest pochodną `VisualElement` obsługuje trzy stany wspólnej "Normal", "Fokus" oraz "Wyłączone". Wewnętrznie [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) klasy wykrywa staje się włączony lub wyłączony lub ukierunkowanych lub bez fokusu i wywołuje statycznych [ `VisualStateManager.GoToState` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualStateManager.GoToState/p/Xamarin.Forms.VisualElement/System.String/) metody następująco:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Jest to metoda ważnych i tylko kodu Visual State Manager znajdziesz w `VisualElement` klasy. Ponieważ `GoToState` jest wywoływana dla każdego obiektu oparte na poprawną co klasa pochodzi od `VisualElement`, Visual State Manager można użyć z dowolnym `VisualElement` obiektu odpowiedzieć na te zmiany.

Interesujące jest, że, Nazwa stanu wizualnego grupy "CommonStates" nie jest jawnie używany w `VisualElement`. Nazwa grupy nie jest częścią interfejsu API dla Visual State Manager. W jednym z dwóch program przykładowych pokazano wykonanej do tej pory można zmienić nazwę grupy z "CommonStates" na inny i program będą nadal działać. Nazwa grupy jest jedynie ogólny opis stanów w tej grupie. Niejawnie przyjmuje się, że Stany wizualne w dowolnej grupie wykluczają się wzajemnie: stan jeden i tylko jeden stan jest aktualny w dowolnym momencie.

Jeśli chcesz wdrożyć własne stany wizualne, należy wywołać `VisualStateManager.GoToState` z kodu. W większości przypadków należy podjąć tego wywołania z pliku CodeBehind klasy strony.

**Weryfikacji VSM** strony **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** przykład przedstawia sposób użycia Visual State Manager w związku z sprawdzania poprawności danych wejściowych. Plik XAML składa się z dwóch `Label` elementów `Entry`, i `Button`:

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

VSM znaczników jest dołączony do drugiego `Label` (o nazwie `helpLabel`) i `Button` (o nazwie `submitButton`). Istnieją dwa stany wzajemnie wykluczającymi, o nazwie "Ważne" i "Nieprawidłowe". (Zobaczysz pliku CodeBehind, która ustawia te stany wkrótce.) Zwróć uwagę, czy każdy z tych dwóch grup "ValidationState" zawiera `VisualState` tagi "Ważne" i "Nieprawidłowe", chociaż jeden z nich jest pusta w każdym przypadku. 

Jeśli `Entry` nie zawiera prawidłowy numer telefonu, a następnie bieżący stan to "Nieprawidłowy". Drugi `Label` jest widoczna i `Button` jest wyłączone:

[![Sprawdzanie poprawności VSM: Nieprawidłowy stan](vsm-images/VsmValidationInvalid.png "VSM sprawdzanie poprawności — nieprawidłowy")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

Po wprowadzeniu prawidłowy numer telefonu, bieżący stan staje się "Ważne". Drugi `Entry` zniknie i `Button` jest teraz włączony:

[![Weryfikacji VSM: Nieprawidłowy stan](vsm-images/VsmValidationValid.png "sprawdzanie poprawności VSM — nieprawidłowa")](vsm-images/VsmValidationValid-Large.png#lightbox)

Plik CodeBehind jest odpowiedzialnej za obsługę `TextChanged` zdarzenia z `Entry`. Program obsługi używa wyrażenia regularnego, aby określić, czy ciąg wejściowy jest nieprawidłowy. W pliku związanym z kodem o nazwie metody `GoToState` wywołuje statycznych `VisualStateManager.GoToState` metody dla obu `helpLabel` i `submitButton`:

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

Zauważ również, że `GoToState` metoda jest wywoływana z konstruktora w celu zainicjowania stanu. Należy zawsze bieżącego stanu. Ale daleko w kodzie wszystkich odwołań do nazwy grupy stanu wizualnego, mimo że odwołuje się do niego XAML jako "ValidationStates" do celów jasności. 

Zwróć uwagę, że plik CodeBehind musi uwzględniać każdy obiekt strony, który ma to wpływ na przez te stany wizualne oraz wywołanie `VisualStateManager.GoToState` dla każdego z tych obiektów. W tym przykładzie jest tylko dwa obiekty ( `Label` i `Button`), ale może być kilka więcej.

Być może zastanawiasz się: Jeśli plik CodeBehind musi odwoływać się każdy obiekt na stronie, których dotyczy te stany wizualne, dlaczego nie pliku CodeBehind po prostu obiekty bezpośredni dostęp? Z pewnością można. Jednak przy użyciu Visual State Manager, możesz kontrolować, jak te obiekty reagowania na różne stany wizualne w całości w języku XAML, który zachowuje w projekcie interfejsu użytkownika w jednym miejscu.

Może być kuszące wziąć pod uwagę wyprowadzanie klasy z `Entry` i prawdopodobnie Definiowanie właściwości ustawionej dla funkcji zewnętrznych sprawdzania poprawności. Klasa, która jest pochodną `Entry` może wywoływać `VisualStateManager.GoToState` metody. Ten program będzie działać prawidłowo, ale tylko wtedy, gdy `Entry` były tylko obiekt wpływ różne stany wizualne. W tym przykładzie `Label` i `Button` również będzie zmienione. Nie istnieje sposób dla znacznika VSM dołączony do `Entry` do kontrolowania na stronie, a nie inne obiekty, dla znacznika VSM dołączonych do tych innych obiektów, aby odwołać zmianę stanu wizualnego z innego obiektu.

<a name="adaptive-layout" />

## <a name="using-the-vsm-for-adaptive-layout"></a>Przy użyciu metody VSM adaptacyjną układu

Program platformy Xamarin.Forms działający na telefonie zwykle można wyświetlić w orientacji poziomej lub pionowej współczynnik proporcji, a program platformy Xamarin.Forms działający na pulpicie można zmienić rozmiar założenie wiele różnych rozmiarach i współczynnik proporcji. Dobrze zaprojektowanej aplikacji może wyświetlić zawartość inaczej dla tych różne czynniki formularza strony lub okno. 

Ta metoda jest czasami nazywane _adaptacyjną układu_. Ponieważ układ adaptacyjną obejmuje wyłącznie elementy wizualne programu, jest idealny aplikacji Visual State Manager.

Prosty przykład to program, który wyświetla małych kolekcję przycisków, które wpływają na zawartość aplikacji. W trybie portret tych przycisków może być wyświetlany w rzędzie górnej części strony:

[![Układ adaptacyjną VSM: Portret](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM adaptacyjną układ — orientacji pionowej")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

W trybie krajobraz tablica przycisków może być przeniesiony do jednej stronie i wyświetlane w kolumnie:

[![Układ adaptacyjną VSM: Pozioma](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM układ adaptacyjną — orientacji poziomej")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

Od góry do dołu program jest uruchomiony na platformy uniwersalnej systemu Windows, Android i iOS.

To zadanie Visual State Manager. **Układu adaptacyjną VSM** strony [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/) próbki definiuje grupę o nazwie "OrientationStates" z dwoma stanami visual o nazwie "Pionowej" i "Poziomej". (Bardziej złożone podejście może być oparta na kilka różnych szerokości strony lub okna). 

Migracja VSM znaczników zostaną wyświetlone w czterech miejsca w pliku XAML. `StackLayout` o nazwie `mainStack` zawiera menu i zawartość, która jest `Image` elementu. To `StackLayout` powinny mieć orientacji pionowej w trybie portret i orientację poziomą w trybie krajobraz:

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

Wewnętrzny `ScrollView` o nazwie `menuScroll` i `StackLayout` o nazwie `menuStack` zaimplementować menu przycisków. Orientacja tych układów jest przeciwny z `mainStack`: menu powinna być w trybie portret pionie i w trybie krajobraz.

Czwarty fragmentu znaczników VSM jest niejawne styl przycisków się. Ustawia tego znacznika `VerticalOptions`, `HorizontalOptions`, i `Margin` właściwości specyficzne dla orienations portait i orientacji poziomej.

Ustawia plik CodeBehind `BindingContext` właściwość `menuStack` do zaimplementowania `Button` droższe, a także dołącza program obsługi do `SizeChanged` zdarzeń strony:

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

`SizeChanged` Wywołań obsługi `VisualStateManager.GoToState` dla dwóch `StackLayout` i `ScrollView` elementy, a następnie wykonuje pętlę elementów podrzędnych `menuStack` do wywołania `VisualStateManager.GoToState` dla `Button` elementów.

Na początku wydaje się tak, jakby plik CodeBehind można obsługiwać zmiany orientacji więcej bezpośrednio przez ustawienie właściwości elementów w pliku XAML, ale Visual State Manager ostatecznie jest to bardziej ustrukturyzowanymi podejście. Wszystkie elementy wizualne są przechowywane w pliku XAML, gdzie stają się łatwiejsze do sprawdzenia, obsługiwanie i modyfikowanie.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Stan wizualny w Menedżerze Xamarin.University

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Menedżer stanu Visual platformy Xamarin.Forms 3.0, przez [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Linki pokrewne

- [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)

