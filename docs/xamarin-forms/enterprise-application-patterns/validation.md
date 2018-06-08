---
title: Walidacja
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 484f3b3d45e41d0dd0406681250ac90943a1cdde
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847592"
---
# <a name="validation"></a>Walidacja

Dowolną aplikację, która akceptuje dane wejściowe od użytkowników powinny upewnij się, że dane wejściowe są poprawne. Aplikację można na przykład sprawdzić dla danych wejściowych zawiera tylko znaki w określonym zakresie, jest o określonej długości lub odpowiada określonym formacie. Bez sprawdzania poprawności użytkownik może podać dane, które powoduje awarię aplikacji. Sprawdzanie poprawności wymusza stosowanie reguł biznesowych i uniemożliwia osobie atakującej iniekcję szkodliwe dane.

W kontekście modelu ViewModel modelu (MVVM) wzorca model widoku lub model często będzie wymagane do wykonywania sprawdzania poprawności danych i sygnalizuje wszystkie błędy weryfikacji do widoku, dzięki czemu użytkownik może je poprawić. Aplikacja mobilna eShopOnContainers wykonuje synchroniczne weryfikacji po stronie klienta właściwości modelu widoku i powiadamia użytkownika o błędach weryfikacji przez wyróżnianie formant, który zawiera nieprawidłowe dane i wyświetlając komunikaty o błędach, które informują użytkownika Dlaczego danych jest nieprawidłowy. Rysunek 6-1 przedstawiono objętego przeprowadzania weryfikacji w aplikacji mobilnej eShopOnContainers klasy.

[![](validation-images/validation.png "Klasy weryfikacji w aplikacji mobilnej eShopOnContainers")](validation-images/validation-large.png#lightbox "klasy weryfikacji w aplikacji mobilnej eShopOnContainers")

**Rysunek 6-1**: klasy weryfikacji w aplikacji mobilnej eShopOnContainers

Właściwości modelu widoku, wymagające weryfikacji są typu `ValidatableObject<T>`i każdego `ValidatableObject<T>` wystąpienia został dodany do reguł sprawdzania poprawności jej `Validations` właściwości. Sprawdzanie poprawności jest wywoływany z modelu widoku, wywołując `Validate` metody `ValidatableObject<T>` wystąpienia, która pobiera sprawdzania poprawności reguły i wykonuje je przed `ValidatableObject<T>` `Value` właściwości. Wszystkie błędy weryfikacji są umieszczane w `Errors` właściwość `ValidatableObject<T>` wystąpienia i `IsValid` właściwość `ValidatableObject<T>` jest aktualizowane wystąpienie, aby wskazać, czy Weryfikacja powiodła się czy nie.

Powiadomienia o zmianie właściwości są dostarczane przez `ExtendedBindableObject` klasy i dlatego [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) formantu można powiązać z `IsValid` właściwość `ValidatableObject<T>` wystąpienia klasy modelu widok ma być powiadamiany o tym, czy nie wprowadzone dane są prawidłowe.

## <a name="specifying-validation-rules"></a>Określanie reguł sprawdzania poprawności

Reguły sprawdzania poprawności zostały określone przez utworzenie klasy, która jest pochodną `IValidationRule<T>` interfejsu, co przedstawiono w poniższym przykładzie:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Ten interfejs określa Podaj klasy reguły sprawdzania poprawności `boolean` `Check` metodę, która jest używana do wykonywania wymaganej weryfikacji i `ValidationMessage` właściwości, której wartość jest komunikat o błędzie weryfikacji, który będzie wyświetlany, jeśli Weryfikacja zakończy się niepowodzeniem.

Poniższy kod przedstawia przykład `IsNotNullOrEmptyRule<T>` reguły weryfikacji, który jest używany do sprawdzania poprawności nazwy użytkownika i hasła podanego przez użytkownika `LoginView` podczas korzystania z usług zasymulować w aplikacji mobilnej eShopOnContainers:

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

`Check` Metoda zwraca `boolean` wskazującą, czy wartość argumentu jest `null`, pusty lub zawiera tylko znaków odstępu.

Chociaż nie jest używany przez aplikację mobilną eShopOnContainers, poniższy przykładowy kod przedstawia reguły walidacji sprawdzania poprawności adresów e-mail:

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

`Check` Metoda zwraca `boolean` wskazującą, czy wartość argumentu jest prawidłowy adres e-mail. Jest to osiągane przez wyszukiwanie wartość argumentu pierwsze wystąpienie określonego w wzorzec wyrażenia regularnego `Regex` konstruktora. Określa, czy wzorzec wyrażenia regularnego został odnaleziony w ciągu wejściowym można ustalić przy wartości `Match` obiektu `Success` właściwości.

> [!NOTE]
> Sprawdzanie poprawności właściwości czasami może obejmować zależne właściwości. Przykładem zależne właściwości jest zestawu prawidłowych wartości dla właściwości A zależy od określonej wartości ustawioną we właściwości B. Aby sprawdzić, czy wartość właściwości A jest jednym z dozwolonych wartości jest stosowane podczas pobierania wartości właściwości B. Ponadto po zmianie wartości właściwości B, A właściwości musi być sprawdzony ponownie.

## <a name="adding-validation-rules-to-a-property"></a>Dodawanie reguł walidacji do właściwości

W aplikacji mobilnej eShopOnContainers określonych jako typu właściwości modelu widoku, wymagające weryfikacji `ValidatableObject<T>`, gdzie `T` jest typu danych ma zostać zweryfikowana. Poniższy przykładowy kod przedstawia przykład dwóch takich właściwości:

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

W przypadku sprawdzanie poprawności jest wykonywane, należy dodać reguły sprawdzania poprawności do `Validations` kolekcji każdego `ValidatableObject<T>` wystąpienia, jak pokazano w poniższym przykładzie:

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

Ta metoda dodaje `IsNotNullOrEmptyRule<T>` reguły walidacji do `Validations` kolekcji każdego `ValidatableObject<T>` wystąpienia określania wartości dla reguły weryfikacji `ValidationMessage` właściwość, która określa komunikat o błędzie weryfikacji, który będzie wyświetlany, jeśli Weryfikacja zakończy się niepowodzeniem.

## <a name="triggering-validation"></a>Wyzwolenie sprawdzania poprawności

Podejście weryfikacji używane w aplikacji mobilnej eShopOnContainers mogą ręcznie uruchomić sprawdzanie poprawności właściwości i automatycznie wyzwalacza weryfikacji po zmianie właściwości.

### <a name="triggering-validation-manually"></a>Ręcznie wyzwalania sprawdzania poprawności

Sprawdzanie poprawności ręcznie wyzwolone dla właściwości modelu widoku. Na przykład dzieje się tak w aplikacji mobilnej eShopOnContainers po naciśnięciu **logowania** znajdującego się na `LoginView`, podczas korzystania z usług testowych. Polecenie wywołania delegata `MockSignInAsync` metody w `LoginViewModel`, który wywołuje sprawdzania poprawności, wykonując `Validate` metodę, która jest wyświetlana w poniższym przykładzie:

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

`Validate` Metoda sprawdza poprawność nazwy użytkownika i hasła podanego przez użytkownika `LoginView`, wywołując metodę Validate w każdym `ValidatableObject<T>` wystąpienia. Poniższy przykładowy kod przedstawia metodę Validate z `ValidatableObject<T>` klasy:

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

Ta metoda usuwa `Errors` kolekcji, a następnie pobiera wszystkich sprawdzania poprawności reguły, które zostały dodane do obiektu `Validations` kolekcji. `Check` Metody dla każdej reguły pobrane sprawdzania poprawności jest wykonywane i `ValidationMessage` wartość właściwości dla reguły weryfikacji nie powiedzie się, aby sprawdzić poprawność danych jest dodawana do `Errors` Kolekcja `ValidatableObject<T>` wystąpienia. Na koniec `IsValid` właściwość jest ustawiona, a jego wartość jest zwracana do wywołania metody, wskazującą, czy sprawdzanie poprawności zakończyło się pomyślnie lub nie powiodło się.

### <a name="triggering-validation-when-properties-change"></a>Wyzwolenie weryfikacji po zmianie właściwości

Sprawdzanie poprawności można również uruchomić przy każdej zmianie właściwości powiązania. Na przykład, jeśli Wiązanie dwukierunkowe w `LoginView` ustawia `UserName` lub `Password` właściwości, sprawdzanie poprawności zostanie wywołany. W poniższym przykładzie kodu pokazano, jak dzieje się tak:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Formant jest powiązany z `UserName.Value` właściwość `ValidatableObject<T>` wystąpienia i formantu `Behaviors` kolekcja ma `EventToCommandBehavior` wystąpienie do niego dodana. To zachowanie wykonuje `ValidateUserNameCommand` w odpowiedzi na [`TextChanged`] zdarzenie wywołujące na `Entry`, które jest wywoływane, gdy tekst w `Entry` zmiany. Z kolei `ValidateUserNameCommand` wykonuje delegata `ValidateUserName` metodę, która wykonuje `Validate` metoda `ValidatableObject<T>` wystąpienia. W związku z tym każdym razem, użytkownik musi wprowadzić znak w `Entry` kontroli dla nazwy użytkownika, weryfikacji wprowadzonych danych jest wykonywana.

Aby uzyskać więcej informacji dotyczących zachowania, zobacz [implementacja zachowania](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Wyświetlanie błędów walidacji

Aplikacji mobilnej eShopOnContainers powiadamia użytkownika o błędach weryfikacji przez wyróżnianie formant, który zawiera nieprawidłowe dane czerwoną linią, a następnie wyświetlając komunikat o błędzie z informacją, dlaczego dane są nieprawidłowe poniżej zawierająca formant nieprawidłowe dane. Gdy skorygowania nieprawidłowe dane wiersza zmieni się na kolor czarny i komunikat o błędzie zostanie usunięta. Rysunek 6-2 wyświetlany w aplikacji mobilnej eShopOnContainers LoginView, jeśli występują błędy sprawdzania poprawności.

![](validation-images/validation-login.png "Wyświetlanie błędy weryfikacji podczas logowania")

**Rysunek 6-2:** wyświetlanie błędy weryfikacji podczas logowania

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Wyróżnianie formant, który zawiera nieprawidłowe dane

`LineColorBehavior` Dołączone zachowanie służy do Wyróżnij [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kontrolki, w których wystąpiły błędy sprawdzania poprawności. Poniższy kod przedstawia przykład sposobu `LineColorBehavior` dołączone zachowanie jest dołączony do `Entry` sterowania:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Kontroli zużywa jawne stylu, w którym przedstawiono w poniższym przykładzie:

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

Ustawia ten styl `ApplyLineColor` i `LineColor` dołączonych właściwości `LineColorBehavior` dołączona zachowanie [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) formantu. Aby uzyskać więcej informacji na temat stylów, zobacz [style](~/xamarin-forms/user-interface/styles/index.md).

Gdy wartość `ApplyLineColor` dołączonej właściwości to zbiór lub zmiany, `LineColorBehavior` wykonuje dołączone zachowanie `OnApplyLineColorChanged` metodę, która jest wyświetlana w poniższym przykładzie kodu:

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

Parametry dla tej metody Podaj wystąpienie formantu podłączonego do zachowania i starej i nowej wartości `ApplyLineColor` dołączona właściwość. `EntryLineColorEffect` Klasa zostanie dodany do formantu [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekcji Jeśli `ApplyLineColor` jest dołączona właściwość `true`, w przeciwnym razie zostanie ono usunięte z formantu `Effects` kolekcji. Aby uzyskać więcej informacji dotyczących zachowania, zobacz [implementacja zachowania](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

`EntryLineColorEffect` Podklasy [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) klasy i przedstawiono w poniższym przykładzie:

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

[ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) Klasa reprezentuje efekt niezależne od platformy, który opakowuje wewnętrzny efekt, która jest specyficzna dla platformy. Upraszcza proces usuwania efektu, ponieważ nie istnieje żadne kompilacji dostęp do informacji o typie dla efektu specyficzne dla platformy. `EntryLineColorEffect` Wywołuje konstruktor klasy podstawowej, przekazując parametr składające się z łączenia rozpoznawania nazwy grupy i unikatowy identyfikator, który określono w każdej klasie efekt specyficzne dla platformy.

Poniższy kod przedstawia przykład `eShopOnContainers.EntryLineColorEffect` wdrożenia dla systemu iOS:

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

`OnAttached` Metoda pobiera macierzystego formantu dla platformy Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kontroli i aktualizuje kolor linii przez wywołanie metody `UpdateLineColor` metody. `OnElementPropertyChanged` Zastąpienie odpowiada zmiany właściwości możliwej do wiązania na `Entry` kontroli, aktualizując kolor linii, jeżeli dołączonego `LineColor` zmiany właściwości lub [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) właściwości `Entry`zmiany. Aby uzyskać więcej informacji na temat efektów, zobacz [efekty](~/xamarin-forms/app-fundamentals/effects/index.md).

Po wprowadzeniu prawidłowych danych w [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) formantu zostanie zastosowana czarnej linii w dół formantu, aby wskazać, że jest brak błędu sprawdzania poprawności. Rysunek 6-3 przedstawiono przykład.

![](validation-images/validation-blackline.png "Czarnej linii, wskazując nie błąd sprawdzania poprawności")

**Rysunek 6-3**: czarnej linii, wskazując nie błąd sprawdzania poprawności

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Formantu ma również [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) dodawane do jej [ `Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) kolekcji. Poniższy kod przedstawia przykład `DataTrigger`:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

To [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) monitorów `UserName.IsValid` właściwości i jest wartością staje się `false`, wykonuje on [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/), jakie zmiany `LineColor` dołączony Właściwość `LineColorBehavior` dołączony zachowanie na czerwony. Rysunek 6-4 przedstawiono przykład.

![](validation-images/validation-redline.png "Czerwona linia wskazujący błąd sprawdzania poprawności")

**Rysunek 6-4**: czerwony linia wskazująca błąd sprawdzania poprawności

Wiersz w [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) formant pozostanie czerwony podczas wprowadzonych danych jest nieprawidłowa, w przeciwnym razie zostanie on zmieniony na kolor czarny, aby wskazać, że wprowadzonych danych jest nieprawidłowa.

Aby uzyskać więcej informacji na temat wyzwalaczy, zobacz [wyzwalaczy](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Wyświetlanie komunikatów o błędach

Interfejs użytkownika wyświetla komunikaty o błędach weryfikacji w formantów etykiet poniżej każdej kontrolki, których Weryfikacja nie powiodła się. Poniższy kod przedstawia przykład [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) który wyświetla komunikat o błędzie weryfikacji, jeśli użytkownik nie wprowadził prawidłową nazwę użytkownika:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Każdy [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wiąże `Errors` weryfikowanego obiektu modelu widoku. `Errors` Właściwość jest udostępniana przez `ValidatableObject<T>` klasy, a jest typu `List<string>`. Ponieważ `Errors` właściwość może zawierać wiele błędów sprawdzania poprawności, `FirstValidationErrorConverter` wystąpienia służy do pobierania pierwszego błędu z kolekcji do wyświetlenia.

## <a name="summary"></a>Podsumowanie

Aplikacja mobilna eShopOnContainers wykonuje synchroniczne weryfikacji po stronie klienta właściwości modelu widoku i powiadamia użytkownika o błędach weryfikacji przez wyróżnianie formant, który zawiera nieprawidłowe dane i wyświetlając komunikaty o błędach, które informują użytkownika Dlaczego dane są nieprawidłowe.

Właściwości modelu widoku, wymagające weryfikacji są typu `ValidatableObject<T>`i każdego `ValidatableObject<T>` wystąpienia został dodany do reguł sprawdzania poprawności jej `Validations` właściwości. Sprawdzanie poprawności jest wywoływany z modelu widoku, wywołując `Validate` metody `ValidatableObject<T>` wystąpienia, która pobiera sprawdzania poprawności reguły i wykonuje je przed `ValidatableObject<T>` `Value` właściwości. Wszystkie błędy weryfikacji są umieszczane w `Errors` właściwość `ValidatableObject<T>`wystąpienia i `IsValid` właściwość `ValidatableObject<T>` jest aktualizowane wystąpienie, aby wskazać, czy Weryfikacja powiodła się czy nie.


## <a name="related-links"></a>Linki pokrewne

- [Pobieranie książki elektronicznej (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
