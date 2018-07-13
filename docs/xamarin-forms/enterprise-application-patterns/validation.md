---
title: Sprawdzanie poprawności w aplikacji dla przedsiębiorstw
description: W tym rozdziale wyjaśniono, jak aplikacja mobilna w ramach aplikacji eShopOnContainers wykonuje sprawdzanie poprawności danych wejściowych użytkownika. Obejmuje to określenie reguł sprawdzania poprawności, wyzwalając sprawdzania poprawności i wyświetlania błędów sprawdzania poprawności.
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 2b4be17e3c96ee223433b435a7b1011eafa8e9db
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995830"
---
# <a name="validation-in-enterprise-apps"></a>Sprawdzanie poprawności w aplikacji dla przedsiębiorstw

Dowolną aplikację, która akceptuje dane wejściowe od użytkowników należy upewnić się, że dane wejściowe są prawidłowe. Aplikację można na przykład, sprawdź dane wejściowe, która zawiera tylko znaki w określonym zakresie, jest o określonej długości lub odpowiada określonym formacie. Bez sprawdzania poprawności użytkownik może podawać dane, które powoduje, że aplikacja nie powiedzie się. Sprawdzanie poprawności wymusza stosowanie reguł biznesowych i uniemożliwia osobie atakującej iniekcję złośliwych danych.

W kontekście modelu ViewModel Model (MVVM) wzorca model widoku lub modelu będzie często wymagane do wykonywania sprawdzania poprawności danych i sygnałów wszystkie błędy weryfikacji do widoku, dzięki czemu użytkownik może poprawić je. Aplikacja mobilna w ramach aplikacji eShopOnContainers wykonuje synchroniczne weryfikacji po stronie klienta dla właściwości modelu widoku i powiadamia użytkownika wszelkie błędy sprawdzania poprawności, wyróżniając formant, który zawiera nieprawidłowe dane i wyświetlając komunikaty o błędach, które informują użytkownika Dlaczego danych jest nieprawidłowy. Rysunek 6-1 przedstawiono klasy związane z wykonaniem weryfikacji w aplikacji mobilnej w ramach aplikacji eShopOnContainers.

[![](validation-images/validation.png "Sprawdzania poprawności klas w aplikacji mobilnej w ramach aplikacji eShopOnContainers")](validation-images/validation-large.png#lightbox "sprawdzania poprawności klas w aplikacji mobilnej w ramach aplikacji eShopOnContainers")

**Rysunek 6-1**: sprawdzania poprawności klas w aplikacji mobilnej w ramach aplikacji eShopOnContainers

Widok modelu, które wymagają weryfikacji są typu `ValidatableObject<T>`i każdy `ValidatableObject<T>` wystąpienie ma reguły sprawdzania poprawności dodane do jego `Validations` właściwości. Sprawdzanie poprawności jest wywoływana z modelu widoku przez wywołanie metody `Validate` metody `ValidatableObject<T>` wystąpienia, który pobiera sprawdzania poprawności reguły i wykonuje je względem `ValidatableObject<T>` `Value` właściwości. Wszelkie błędy sprawdzania poprawności są umieszczane w `Errors` właściwość `ValidatableObject<T>` wystąpienia i `IsValid` właściwość `ValidatableObject<T>` wystąpienia jest aktualizowana w celu wskazania, czy sprawdzanie poprawności zakończyło się pomyślnie lub nie powiodło się.

Powiadomienie o zmianie właściwości są dostarczane przez `ExtendedBindableObject` klasy, a tym samym [ `Entry` ](xref:Xamarin.Forms.Entry) kontroli można powiązać `IsValid` właściwość `ValidatableObject<T>` wystąpienia w klasie modelu widoku, aby otrzymywać powiadomienia o tym, czy nie wprowadzone dane są prawidłowe.

## <a name="specifying-validation-rules"></a>Określanie reguł sprawdzania poprawności

Reguły sprawdzania poprawności są określone, tworząc klasę, która pochodzi od klasy `IValidationRule<T>` interfejsu, który jest pokazany w poniższym przykładzie kodu:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Ten interfejs określa, że klasy reguł sprawdzania poprawności, należy podać `boolean` `Check` metodę, która jest używana do wykonywania wymaganej weryfikacji i `ValidationMessage` właściwości, których wartość jest komunikat o błędzie weryfikacji, który będzie wyświetlany, jeśli Weryfikacja zakończy się niepowodzeniem.

Poniższy kod przedstawia przykład `IsNotNullOrEmptyRule<T>` reguły sprawdzania poprawności, który jest używany do wykonywania sprawdzania poprawności nazwy użytkownika i hasła podanego przez użytkownika `LoginView` podczas korzystania z usług makiety w ramach aplikacji eShopOnContainers aplikacji mobilnej:

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

`Check` Metoda zwraca `boolean` wskazującą, czy wartość argumentu jest `null`, pusta lub zawiera tylko białych znaków.

Chociaż nie jest używany przez aplikację mobilną w ramach aplikacji eShopOnContainers, poniższy kod ilustruje reguły sprawdzania poprawności sprawdzania poprawności adresów e-mail:

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

`Check` Metoda zwraca `boolean` wskazującą, czy wartość argumentu jest prawidłowy adres e-mail. Jest to osiągane przez wyszukiwanie wartość argumentu pierwsze wystąpienie ciągu do wzorca wyrażenia regularnego określone w `Regex` konstruktora. Czy znaleziono wzorzec wyrażenia regularnego do ciągu wejściowego można ustalić, sprawdzając wartość `Match` obiektu `Success` właściwości.

> [!NOTE]
> Sprawdzanie poprawności właściwości czasami może obejmować właściwości zależne. Na przykład właściwości zależne sytuacja zestawu prawidłowych wartości dla właściwości, A zależy od określonej wartości, która została ustawiona we właściwości B. Aby sprawdzić, czy wartość właściwości A jest jednym z dozwolonych wartości będą wiązać z pobieraniem wartość właściwości B. Ponadto po zmianie wartości właściwości B, właściwości, A musi być sprawdzony ponownie.

## <a name="adding-validation-rules-to-a-property"></a>Dodawania reguł sprawdzania poprawności właściwości

W ramach aplikacji eShopOnContainers aplikacji mobilnej, wyświetlanie właściwości modelu, wymagające weryfikacji są uznane za typu `ValidatableObject<T>`, gdzie `T` jest typ danych ma zostać zweryfikowana. Poniższy przykład kodu pokazuje przykład dwóch tych właściwości:

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

Dla sprawdzanie poprawności jest wykonywane, należy dodać reguły sprawdzania poprawności do `Validations` kolekcji każdego `ValidatableObject<T>` wystąpienia, jak pokazano w poniższym przykładzie kodu:

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

Metoda ta umożliwia dodanie `IsNotNullOrEmptyRule<T>` reguły sprawdzania poprawności do `Validations` kolekcji każdego `ValidatableObject<T>` wystąpienia, określając wartości dla reguły weryfikacji `ValidationMessage` właściwość, która określa komunikat o błędzie weryfikacji, który będzie wyświetlany, jeśli Weryfikacja zakończy się niepowodzeniem.

## <a name="triggering-validation"></a>Wyzwalanie sprawdzania poprawności

Metoda sprawdzania poprawności używana w aplikacji mobilnej w ramach aplikacji eShopOnContainers można ręcznie wyzwalać sprawdzania poprawności właściwości i automatycznie Walidacja wyzwalacza, po zmianie właściwości.

### <a name="triggering-validation-manually"></a>Ręczne wyzwalanie sprawdzania poprawności

Sprawdzanie poprawności mogą być wyzwalane ręcznie dla właściwości modelu widoku. Na przykład, dzieje się tak w aplikacji mobilnej w ramach aplikacji eShopOnContainers kiedy użytkownik naciska **logowania** znajdujący się na `LoginView`, podczas korzystania z usług makiety. Wywołuje delegata polecenia `MockSignInAsync` method in Class metoda `LoginViewModel`, która wywołuje sprawdzania poprawności, wykonując `Validate` metody, która jest wyświetlana w poniższym przykładzie kodu:

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

`Validate` Metoda wykonuje sprawdzanie poprawności nazwy użytkownika i hasła podanego przez użytkownika `LoginView`, przez wywołanie metody sprawdzania poprawności na każdym `ValidatableObject<T>` wystąpienia. Poniższy przykład kodu przedstawia metodę weryfikacji z `ValidatableObject<T>` klasy:

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

Ta metoda usuwa `Errors` kolekcji, a następnie pobiera wszystkich sprawdzania poprawności reguły, które zostały dodane do obiektu `Validations` kolekcji. `Check` Metody dla każdej reguły pobrane sprawdzania poprawności jest wykonywane i `ValidationMessage` wartości właściwości dla dowolnej reguły sprawdzania poprawności, której nie można sprawdzić poprawności danych jest dodawany do `Errors` zbiór `ValidatableObject<T>` wystąpienia. Na koniec `IsValid` właściwość jest ustawiona, a jego wartość jest zwracana do wywoływania metody, wskazującą, czy sprawdzanie poprawności zakończyło się pomyślnie lub nie powiodło się.

### <a name="triggering-validation-when-properties-change"></a>Wyzwalanie sprawdzania poprawności po zmianie właściwości

Sprawdzanie poprawności mogą być też wywoływane zawsze wtedy, gdy zmieni się właściwości powiązanej. Na przykład, gdy powiązanie dwustronne w `LoginView` ustawia `UserName` lub `Password` właściwości sprawdzania poprawności zostanie wywołany. Poniższy przykład kodu pokazuje, jak ten problem wystąpi:

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

[ `Entry` ](xref:Xamarin.Forms.Entry) Wiąże formant `UserName.Value` właściwość `ValidatableObject<T>` wystąpienie i kontrolki `Behaviors` kolekcja ma `EventToCommandBehavior` wystąpienia dodawanych do niego. To zachowanie jest wykonywana `ValidateUserNameCommand` w odpowiedzi na [`TextChanged`] zdarzenie na `Entry`, które jest wywoływane, gdy tekst w `Entry` zmiany. Z kolei `ValidateUserNameCommand` wykonuje delegata `ValidateUserName` metody, która wykonuje `Validate` metody `ValidatableObject<T>` wystąpienia. W związku z tym, każdym razem, gdy użytkownik wpisuje znak w `Entry` odbywa się kontroli dla nazwy użytkownika, weryfikacja wprowadzonych danych.

Aby uzyskać więcej informacji na temat zachowań, zobacz [Implementowanie zachowań](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Wyświetlanie błędów walidacji

Aplikacji mobilnej w ramach aplikacji eShopOnContainers powiadamia użytkownika o błędach weryfikacji przez wyróżnianie formant, który zawiera nieprawidłowe dane z czerwoną linią. Ponadto, wyświetlając komunikat o błędzie z informacją, dlaczego dane są nieprawidłowe poniżej zawierający kontrolki nieprawidłowe dane. Po naprawieniu nieprawidłowe dane wiersza zmieni się na czarny, a komunikat o błędzie zostanie usunięty. Rysunek 6-2 zawiera widoku logowania w aplikacji mobilnej w ramach aplikacji eShopOnContainers, gdy występują błędy sprawdzania poprawności.

![](validation-images/validation-login.png "Wyświetlanie błędów sprawdzania poprawności podczas logowania")

**Rysunek 6-2:** wyświetlanie błędów sprawdzania poprawności podczas logowania

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Wyróżnianie formant, który zawiera nieprawidłowe dane

`LineColorBehavior` Dołączonych zachowanie służy do wyróżniania [ `Entry` ](xref:Xamarin.Forms.Entry) kontrolek, w których wystąpiły błędy sprawdzania poprawności. Poniższy kod przedstawia przykładowy sposób, w jaki `LineColorBehavior` dołączonych zachowanie jest dołączony do `Entry` kontroli:

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

[ `Entry` ](xref:Xamarin.Forms.Entry) Kontroli wykorzystuje jawne stylu, który jest pokazany w poniższym przykładzie kodu:

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

Ustawia ten styl `ApplyLineColor` i `LineColor` dołączonych właściwości `LineColorBehavior` dołączony zachowanie [ `Entry` ](xref:Xamarin.Forms.Entry) kontroli. Aby uzyskać więcej informacji na temat style zobacz [style](~/xamarin-forms/user-interface/styles/index.md).

Po wartości `ApplyLineColor` dołączoną właściwość jest zestaw lub zmian, `LineColorBehavior` wykonuje dołączone zachowania `OnApplyLineColorChanged` metody, która jest wyświetlana w poniższym przykładzie kodu:

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

Parametry dla tej metody zapewniają wystąpienia kontrolki, dołączonego do zachowania i starej i nowej wartości z `ApplyLineColor` dołączona właściwość. `EntryLineColorEffect` Klasy jest dodawany do formantu [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekcji Jeśli `ApplyLineColor` jest dołączona właściwość `true`, w przeciwnym razie zostanie on usunięty z formantu `Effects` kolekcji. Aby uzyskać więcej informacji na temat zachowań, zobacz [Implementowanie zachowań](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

`EntryLineColorEffect` Podklasy [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) klasy i przedstawiono w poniższym przykładzie kodu:

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

[ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) Klasa reprezentuje efekt niezależne od platformy, która otacza efektu wewnętrzny, który jest specyficzny dla platformy. Upraszcza to proces usuwania efektu, ponieważ brak jest czasu kompilacji dostępu do informacji o typie dla efektu specyficzne dla platformy. `EntryLineColorEffect` Wywołuje konstruktora klasy bazowej, przekazując w parametrze składający się z składa się z nazwy grupy rozdzielczość i unikatowy identyfikator, który jest określony w każdej klasie efekt specyficzne dla platformy.

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

`OnAttached` Metoda pobiera natywne kontrolki dla platformy Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) sterowania, a następnie aktualizuje kolor linii, wywołując `UpdateLineColor` metody. `OnElementPropertyChanged` Zastąpienie reaguje na zmiany właściwości możliwej do wiązania na `Entry` kontroli, aktualizując kolor linii, jeżeli dołączonego `LineColor` zmiany właściwości lub [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) właściwość `Entry`zmiany. Aby uzyskać więcej informacji na temat skutków, zobacz [efekty](~/xamarin-forms/app-fundamentals/effects/index.md).

Po wprowadzeniu prawidłowych danych w [ `Entry` ](xref:Xamarin.Forms.Entry) kontroli czarna linia będzie stosowana do dolnej części kontrolki, aby wskazać, że nie ma błędu sprawdzania poprawności. Rysunek 6-3 przedstawiono przykład.

![](validation-images/validation-blackline.png "Czarna linia wskazujący braku błędów weryfikacji")

**Rysunek 6-3**: czarna linia wskazujący braku błędów weryfikacji

[ `Entry` ](xref:Xamarin.Forms.Entry) Kontroli ma również [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) dodawane do jej [ `Triggers` ](xref:Xamarin.Forms.VisualElement.Triggers) kolekcji. Poniższy kod przedstawia przykład `DataTrigger`:

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

To [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) monitorów `UserName.IsValid` właściwości i jest wartością staje się `false`, wykonuje [ `Setter` ](xref:Xamarin.Forms.Setter), jakie zmiany `LineColor` dołączone Właściwość `LineColorBehavior` dołączone zachowania na czerwony. Na przykład pokazano na rysunku 6-4.

![](validation-images/validation-redline.png "Czerwona linia wskazujący błąd sprawdzania poprawności")

**Rysunek 6-4**: czerwoną linią wskazujący błąd sprawdzania poprawności

Wiersz w [ `Entry` ](xref:Xamarin.Forms.Entry) sterowania pozostanie czerwony wprowadzonych danych jest nieprawidłowy, w przeciwnym razie zostanie on zmieniony na czarny, aby wskazać, że wprowadzone dane są prawidłowe.

Aby uzyskać więcej informacji na temat wyzwalaczy zobacz [wyzwalaczy](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Wyświetlanie komunikatów o błędach

Interfejs użytkownika wyświetla komunikaty o błędach weryfikacji w kontrolkach etykiety poniżej każdy formant, którego dane Weryfikacja nie powiodła się. Poniższy kod przedstawia przykład [ `Label` ](xref:Xamarin.Forms.Label) , wyświetla komunikat o błędzie weryfikacji, jeśli użytkownik nie wprowadził prawidłową nazwę użytkownika:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Każdy [ `Label` ](xref:Xamarin.Forms.Label) wiąże `Errors` właściwości obiektu modelu widoku, który jest sprawdzany. `Errors` Właściwości są dostarczane przez `ValidatableObject<T>` klasy i jest typu `List<string>`. Ponieważ `Errors` właściwość może zawierać wiele błędów sprawdzania poprawności, `FirstValidationErrorConverter` wystąpienie jest używane do pobierania od pierwszego błędu z kolekcji do wyświetlenia.

## <a name="summary"></a>Podsumowanie

Aplikacja mobilna w ramach aplikacji eShopOnContainers wykonuje synchroniczne weryfikacji po stronie klienta dla właściwości modelu widoku i powiadamia użytkownika wszelkie błędy sprawdzania poprawności, wyróżniając formant, który zawiera nieprawidłowe dane i wyświetlając komunikaty o błędach, które informują użytkownika Dlaczego dane są nieprawidłowe.

Widok modelu, które wymagają weryfikacji są typu `ValidatableObject<T>`i każdy `ValidatableObject<T>` wystąpienie ma reguły sprawdzania poprawności dodane do jego `Validations` właściwości. Sprawdzanie poprawności jest wywoływana z modelu widoku przez wywołanie metody `Validate` metody `ValidatableObject<T>` wystąpienia, który pobiera sprawdzania poprawności reguły i wykonuje je względem `ValidatableObject<T>` `Value` właściwości. Wszelkie błędy sprawdzania poprawności są umieszczane w `Errors` właściwość `ValidatableObject<T>`wystąpienia i `IsValid` właściwość `ValidatableObject<T>` wystąpienia jest aktualizowana w celu wskazania, czy sprawdzanie poprawności zakończyło się pomyślnie lub nie powiodło się.


## <a name="related-links"></a>Linki pokrewne

- [Pobierz książkę elektroniczną (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [ramach aplikacji eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
