---
title: Zarządzanie konfiguracją
description: W tym rozdziale opisano Implementowanie zarządzania konfiguracją ustawień aplikacji i ustawień użytkownika w aplikacji mobilnej w ramach aplikacji eShopOnContainers.
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 6f32d8f328232bdfc644da57bdb3201c60010063
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995364"
---
# <a name="configuration-management"></a>Zarządzanie konfiguracją

Ustawienia umożliwiają oddzielenie danych, który służy do konfigurowania zachowania aplikacji od kodu, umożliwiając zachowanie, aby go zmienić bez ponownie skompilować aplikację. Istnieją dwa typy ustawień: ustawień aplikacji i ustawień użytkownika.

Ustawienia aplikacji są dane, które tworzy i zarządza aplikacji. Może zawierać dane, takie jak punkty końcowe usługi web stały, klucze interfejsu API i stan czasu wykonywania. Ustawienia aplikacji są powiązane z istnienia aplikacji, a tylko mają znaczenie dla tej aplikacji.

Ustawienia użytkownika są dostosowywane ustawienia aplikacji, które wpływają na działanie aplikacji i nie wymagają częstego ponownego dopasowania. Na przykład aplikacja może zezwolić użytkownikom na określenie, gdzie można pobrać dane i sposób ich wyświetlania na ekranie.

Zestaw narzędzi Xamarin.Forms obejmuje trwałego słownik, który może służyć do przechowywania danych ustawienia. Tego słownika można uzyskać dostęp za pomocą [ `Application.Current.Properties` ](xref:Xamarin.Forms.Application.Properties) właściwości, a wszelkie dane, które znajduje się w niej jest zapisywana, gdy aplikacja przechodzi w stan uśpienia i zostanie przywrócona, gdy aplikacja wznawia lub jest uruchamiany ponownie. Ponadto [ `Application` ](xref:Xamarin.Forms.Application) ma również klasy [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) metodę, która umożliwia aplikacji jego ustawieniami zapisana, gdy jest to wymagane. Aby uzyskać więcej informacji na temat tego słownika, zobacz [słownika właściwości](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary).

Wadą do przechowywania danych za pomocą słownika trwałego Xamarin.Forms jest, że nie jest łatwo dane powiązane z. W związku z aplikacji mobilnej w ramach aplikacji eShopOnContainers korzysta z biblioteki Xam.Plugins.Settings, dostępnym [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). Ta biblioteka zawiera spójnej bezpiecznegop typu, Międzyplatformowe podejście przechowywanie i pobieranie ustawień aplikacji i użytkownika, podczas korzystania z zarządzania ustawieniami natywnych, dostarczone przez każdej z platform. Ponadto jest bardzo proste powiązanie danych umożliwia dostęp do ustawień danych udostępnianych przez bibliotekę.

> [!NOTE]
> Gdy biblioteka Xam.Plugin.Settings mogą przechowywać ustawienia aplikacji i użytkownika, go nie rozróżnia między nimi.

## <a name="creating-a-settings-class"></a>Tworzenie klasy ustawienia

Korzystając z biblioteki Xam.Plugins.Settings, jednej klasy statycznej należy utworzyć, będzie zawierać ustawienia aplikacji i użytkownika, wymagane przez aplikację. Poniższy przykład kodu pokazuje Klasa ustawień w aplikacji mobilnej w ramach aplikacji eShopOnContainers:

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

Ustawienia można Odczyt i zapis za pośrednictwem `ISettings` interfejsu API, która jest dostarczana przez bibliotekę Xam.Plugins.Settings. Ta biblioteka zawiera pojedynczego wystąpienia, które umożliwiają dostęp do interfejsu API, `CrossSettings.Current`, a klasa ustawień aplikacji powinny ujawniać tym pojedynczym wystąpieniu za pośrednictwem `ISettings` właściwości.

> [!NOTE]
> Dyrektywy Using dla przestrzeni nazw Plugin.Settings i Plugin.Settings.Abstractions powinna być dodana do klasy, która wymaga dostępu do biblioteki typów Xam.Plugins.Settings.

## <a name="adding-a-setting"></a>Dodawanie ustawienia

Każde ustawienie składa się z kluczem, wartość domyślna i właściwości. Poniższy przykład kodu pokazuje wszystkie trzy elementy, dla ustawienia użytkownika, który reprezentuje podstawowy adres URL dla usług online, które łączy się z aplikacji mobilnej w ramach aplikacji eShopOnContainers:

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

Klucz jest zawsze const ciąg, który definiuje nazwę klucza, wartością domyślną ustawienia jest wartość wymaganego typu statycznego tylko do odczytu. Podanie wartości domyślnej zapewnia prawidłową wartość dostępna, jeśli nie ustawiono ustawienie jest pobierane.

`UrlBase` Właściwość statyczna używa dwóch metod z `ISettings` interfejsu API w celu odczytu lub zapisu wartości ustawienia. `ISettings.GetValueOrDefault` Metoda jest używana do pobierania wartości ustawienia z magazynu specyficzne dla platformy. Jeśli nie zostanie określona wartość dla ustawienia, zamiast tego jest pobierana wartość domyślną. Podobnie `ISettings.AddOrUpdateValue` metoda jest używana do utrwalenia wartość tego ustawienia do magazynu specyficzne dla platformy.

Przeciwnie, zdefiniuj wartość domyślną wewnątrz `Settings` klasy `UrlBaseDefault` ciąg uzyskuje wartość ze `GlobalSetting` klasy. Poniższy kod przedstawia przykład `BaseEndpoint` właściwości i `UpdateEndpoint` metody tej klasy:

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

Każdorazowo `BaseEndpoint` ustawiono właściwość `UpdateEndpoint` metoda jest wywoływana. Ta metoda aktualizuje szereg właściwości, które są oparte na `UrlBase` ustawienia użytkownika, który jest dostarczany przez `Settings` klasy, które reprezentują różne punkty końcowe, które łączy się z aplikacji mobilnej w ramach aplikacji eShopOnContainers.

## <a name="data-binding-to-user-settings"></a>Powiązanie do ustawień użytkownika danych

W ramach aplikacji eShopOnContainers aplikacji mobilnej `SettingsView` udostępnia dwa ustawienia użytkownika. Te ustawienia umożliwiają konfigurację tego, czy aplikacja powinna pobierać dane z mikrousług, które są wdrażane jako kontenery platformy Docker lub tego, czy aplikacja powinna pobierać dane z makiety usług, które nie wymagają dostępu do Internetu. Podczas wybierania pobrać dane z konteneryzowanych mikrousług, należy określić podstawowego punktu końcowego adresu URL dla mikrousług. Rysunek 7-1 wskazuje `SettingsView` po użytkownik wybierze do pobierania danych z konteneryzowanych mikrousług.

![](configuration-management-images/settings-endpoint.png "Ustawienia użytkownika udostępnianych przez aplikację mobilną w ramach aplikacji eShopOnContainers")

**Rysunek 7-1**: ustawienia użytkownika udostępnianych przez aplikację mobilną w ramach aplikacji eShopOnContainers

Powiązanie danych może służyć do pobrania i ustawienia udostępnianych przez `Settings` klasy. Jest to osiągane przez formanty w widoku powiązania właściwości modelu widoku, które z kolei dostępu do właściwości w `Settings` klasy i wywoływanie właściwości zmienić powiadomienie, gdy zmieniono wartość ustawienia. Aby dowiedzieć się, jak aplikacja mobilna w ramach aplikacji eShopOnContainers tworzy widok modeluje i kojarzy je z widokami, zobacz [automatyczne tworzenie modelu widoku z lokalizatorem modelu widoku](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Poniższy kod przedstawia przykład [ `Entry` ](xref:Xamarin.Forms.Entry) z kontrolować `SettingsView` umożliwiająca użytkownikowi wprowadzenie adresu URL podstawowego punktu końcowego dla konteneryzowane mikrousługi:

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

To [ `Entry` ](xref:Xamarin.Forms.Entry) wiąże formant `Endpoint` właściwość `SettingsViewModel` klasy przy użyciu powiązanie dwustronne. Poniższy przykład kodu pokazuje właściwości punktu końcowego:

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

Gdy `Endpoint` zostaje ustalona `UpdateEndpoint` metoda jest wywoływana, pod warunkiem, że podana wartość jest prawidłowa, i zmienić właściwości zgłaszany jest powiadomienie. Poniższy kod przedstawia przykład `UpdateEndpoint` metody:

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

Ta metoda aktualizuje `UrlBase` właściwość `Settings` klasie z atrybutem wartość adresu URL podstawowego punktu końcowego wprowadzonej przez użytkownika, co powoduje, że utrwalenia do magazynu specyficzne dla platformy.

Gdy `SettingsView` przejście, `InitializeAsync` method in Class metoda `SettingsViewModel` klasy jest wykonywany. Poniższy przykład kodu pokazuje tę metodę:

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

Metody ustawia `Endpoint` właściwości na wartość `UrlBase` właściwość `Settings` klasy. Uzyskiwanie dostępu do `UrlBase` właściwości powoduje, że biblioteka Xam.Plugins.Settings można pobrać wartości ustawienia z magazynu specyficzne dla platformy. Aby uzyskać informacje o tym, jak `InitializeAsync` wywołania metody, zobacz [przekazywanie parametrów podczas nawigacji](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Ten mechanizm gwarantuje, że zawsze wtedy, gdy użytkownik przejdzie do SettingsView, ustawienia użytkownika są pobierane z magazynu specyficzne dla platformy i przedstawiane za pomocą powiązania danych. Następnie jeśli użytkownik zmieni wartości ustawień, powiązanie danych zapewnia, natychmiast są zachowywane do magazynu specyficzne dla platformy.

## <a name="summary"></a>Podsumowanie

Ustawienia umożliwiają oddzielenie danych, który służy do konfigurowania zachowania aplikacji od kodu, umożliwiając zachowanie, aby go zmienić bez ponownie skompilować aplikację. Ustawienia aplikacji są dane, które tworzy i zarządza aplikacji i ustawienia użytkownika są dostosowywane ustawienia aplikacji, które wpływają na działanie aplikacji i nie wymagają częstego ponownego dopasowania.

Biblioteka Xam.Plugins.Settings zapewnia spójne, bezpieczne, Międzyplatformowe podejście, przechowywanie i pobieranie aplikacji i ustawień użytkownika i powiązania danych można uzyskać dostępu do ustawień utworzonych za pomocą biblioteki.


## <a name="related-links"></a>Linki pokrewne

- [Pobierz książkę elektroniczną (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [ramach aplikacji eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
