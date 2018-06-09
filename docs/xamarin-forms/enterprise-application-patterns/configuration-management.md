---
title: Zarządzanie konfiguracją
description: W tym rozdziale opisano, jak aplikacji mobilnej eShopOnContainers implementuje zarządzania konfiguracją w celu zapewnienia ustawienia aplikacji i ustawień użytkownika.
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d6cd9771760bc2932345fec24887842ce1c47376
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243954"
---
# <a name="configuration-management"></a>Zarządzanie konfiguracją

Ustawienia umożliwiają oddzielenie danych, który konfiguruje zachowanie aplikacji z kodu, umożliwiając zachowanie, które będzie można zmienić bez ponownie skompilować aplikację. Istnieją dwa typy ustawień: ustawienia aplikacji i ustawień użytkownika.

Ustawienia aplikacji są dane, które aplikacja tworzy i którymi zarządza. Może obejmować dane, takie jak punktów końcowych usługi sieci web stałej, klucze interfejsu API i stan czasu wykonywania. Ustawienia aplikacji są powiązane z istnienia aplikacji i tylko są poprawne dla danej aplikacji.

Ustawienia użytkownika są dostosowywane ustawienia aplikacji, które wpływają na zachowanie aplikacji i nie wymagają częstego ponownego dostosowania. Na przykład aplikacja może dać użytkownikowi określenie, gdzie można pobrać danych z i sposobu ich wyświetlania na ekranie.

Platformy Xamarin.Forms obejmuje trwałe słownik, który może służyć do przechowywania danych ustawień. Ten słownik jest możliwy za pomocą [ `Application.Current.Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) właściwości i dane, które znajduje się w niej jest zapisywana, gdy aplikacja przechodzi w stan uśpienia i zostanie przywrócona po wznowieniu działania lub ponownie uruchamiany aplikacji w. Ponadto [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) klasa ma również [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) metoda, która umożliwia aplikacji na jego ustawienia zapisane na żądanie. Aby uzyskać więcej informacji dotyczących tego słownika, zobacz [słownika właściwości](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary).

Zapisywanie danych przy użyciu platformy Xamarin.Forms słownik trwałe wadą interfejsu jest nie jest łatwo danymi powiązanymi. W związku z tym aplikacji mobilnej eShopOnContainers korzysta z biblioteki Xam.Plugins.Settings, dostępnej w sklepie [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). Ta biblioteka zawiera spójne, bezpieczne, i platform podejście do przechowywanie i pobieranie ustawień aplikacji i użytkownika, używając zarządzania ustawienia macierzysty pochodzącymi z każdej z platform. Ponadto jest bezpośrednia do używania wiązania z danymi dostępu do danych ustawienia widoczne w bibliotece.

> [!NOTE]
> Gdy biblioteka Xam.Plugin.Settings mogą przechowywać ustawienia zarówno aplikacji i użytkownika, jego nie rozróżnia między nimi.

## <a name="creating-a-settings-class"></a>Tworzenie klasy ustawień

Podczas korzystania z biblioteki Xam.Plugins.Settings, jednej klasy statyczne należy utworzyć który będzie zawierać ustawienia aplikacji i użytkowników wymagane przez aplikację. Poniższy przykład kodu pokazuje klasy ustawienia w aplikacji mobilnej eShopOnContainers:

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

Ustawienia można Odczyt i zapis za pośrednictwem `ISettings` interfejsu API, który jest dostępny w bibliotece Xam.Plugins.Settings. Ta biblioteka zawiera pojedynczą, który można uzyskać dostępu do interfejsu API, `CrossSettings.Current`, i klasy ustawienia aplikacji powinny ujawniać tym pojedynczym wystąpieniu za pośrednictwem `ISettings` właściwości.

> [!NOTE]
> Za pomocą dyrektywy dla przestrzeni nazw Plugin.Settings i Plugin.Settings.Abstractions należy dodać do klasy, która wymaga dostępu do Xam.Plugins.Settings biblioteki typów.

## <a name="adding-a-setting"></a>Dodawanie ustawienia

Każde ustawienie składa się z klucza, wartości domyślnej i właściwości. Poniższy przykład kodu pokazuje wszystkie trzy elementy reprezentujący podstawowy adres URL dla usług online, które łączy się z aplikacji mobilnej eShopOnContainers ustawienia użytkownika:

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

Klucz jest zawsze stałą ciąg, który definiuje nazwę klucza, wartością domyślną ustawienia jest wymagany typ wartość statycznego tylko do odczytu. Podanie wartości domyślnej zapewnia, że prawidłowa wartość jest dostępna, jeśli nie ustawiono ustawienie jest pobierane.

`UrlBase` Statycznej właściwości używa dwóch metod z `ISettings` interfejsu API do odczytu lub zapisu wartości ustawień. `ISettings.GetValueOrDefault` Metoda służy do pobierania wartości ustawienia z magazynu specyficzne dla platformy. Jeśli nie zostanie określona wartość dla ustawienia, zamiast tego są pobierane wartość domyślną. Podobnie `ISettings.AddOrUpdateValue` metoda jest używana do utrwalenia wartości ustawień do magazynu specyficzne dla platformy.

Zamiast definiującą wartość domyślną wewnątrz `Settings` klasy, `UrlBaseDefault` ciąg uzyskuje wartość `GlobalSetting` klasy. Poniższy kod przedstawia przykład `BaseEndpoint` właściwości i `UpdateEndpoint` metody tej klasy:

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

Zawsze `BaseEndpoint` właściwość jest ustawiona, `UpdateEndpoint` metoda jest wywoływana. Ta metoda aktualizacji szereg właściwości, które są oparte na `UrlBase` ustawienia użytkownika, które są dostarczane przez `Settings` klasy reprezentujące różnych punktów końcowych, które łączy się z aplikacji mobilnej eShopOnContainers.

## <a name="data-binding-to-user-settings"></a>Powiązanie ustawień użytkownika danych

W aplikacji mobilnej eShopOnContainers `SettingsView` udostępnia dwa ustawienia użytkownika. Te ustawienia umożliwiają konfigurowanie czy pobierania danych z mikrousług, które są wdrażane jako kontenery Docker aplikacji, lub czy pobierania danych z zasymulować usług, które nie wymagają połączenia internetowego aplikacji. W przypadku wybrania tej opcji można pobrać danych z konteneryzowanych mikrousług, należy określić adres URL punktu końcowego podstawowego mikrousług. Przedstawiono na rysunku 7-1 `SettingsView` gdy użytkownik wybierze do pobierania danych z konteneryzowanych mikrousług.

![](configuration-management-images/settings-endpoint.png "Udostępniany przez aplikację mobilną eShopOnContainers ustawień użytkownika")

**Rysunek 7-1**: udostępniany przez aplikację mobilną eShopOnContainers ustawień użytkownika

Powiązanie danych może służyć do pobrania i ustawienia udostępnione przez `Settings` klasy. Jest to osiągane przez formanty w widoku powiązania właściwości modelu widoku, które z kolei dostępu do właściwości w `Settings` klasy i wywoływanie właściwości zmienić powiadomienie, gdy zmieniono wartość ustawienia. Aby dowiedzieć się, jak aplikacji mobilnej eShopOnContainers tworzy widok modeli i skojarzyć je z widokami, zobacz [automatyczne tworzenie modelu widoku z lokalizatorem modelu widoku](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Poniższy kod przedstawia przykład [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kontrolować z `SettingsView` umożliwiająca użytkownikowi wprowadź adres URL punktu końcowego podstawowego konteneryzowanych mikrousług:

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

To [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) formant jest powiązany z `Endpoint` właściwość `SettingsViewModel` przy użyciu powiązanie dwustronne. Poniższy przykład kodu pokazuje właściwości punktu końcowego:

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

Gdy `Endpoint` ustawiono właściwość `UpdateEndpoint` metoda jest wywoływana, pod warunkiem, że podana wartość jest prawidłowa, i zmienić właściwości powiadomień jest wywoływane. Poniższy kod przedstawia przykład `UpdateEndpoint` metody:

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

Ta metoda aktualizacji `UrlBase` właściwości w `Settings` klasy z wartością podstawowy punkt końcowy adres URL wprowadzony przez użytkownika, co powoduje, że jego utrwalenia do magazynu specyficzne dla platformy.

Gdy `SettingsView` przejście, `InitializeAsync` metoda `SettingsViewModel` klasy jest wykonywana. Poniższy przykład kodu pokazuje tej metody:

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

Ustawia metodę `Endpoint` właściwości do wartości `UrlBase` właściwości w `Settings` klasy. Uzyskiwanie dostępu do `UrlBase` właściwości powoduje, że biblioteka Xam.Plugins.Settings można pobrać wartości ustawienia z magazynu specyficzne dla platformy. Aby uzyskać informacje o sposobie `InitializeAsync` wywołania metody, zobacz [przekazywanie parametrów podczas nawigacji](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Ten mechanizm zapewnia, że zawsze, gdy użytkownik przechodzi do SettingsView, ustawienia użytkownika są pobierane z magazynu specyficzne dla platformy i przedstawiony przez powiązanie danych. Następnie jeśli użytkownik zmieni wartości ustawień, powiązanie danych zapewnia natychmiast są zachowywane do magazynu specyficzne dla platformy.

## <a name="summary"></a>Podsumowanie

Ustawienia umożliwiają oddzielenie danych, który konfiguruje zachowanie aplikacji z kodu, umożliwiając zachowanie, które będzie można zmienić bez ponownie skompilować aplikację. Ustawienia aplikacji są dane, które aplikacja tworzy i którymi zarządza oraz ustawienia użytkownika są dostosowywane ustawienia aplikacji, które wpływają na zachowanie aplikacji i nie wymagają częstego ponownego dostosowania.

Biblioteka Xam.Plugins.Settings zapewnia spójne, bezpieczne, podejście między platformami, przechowywanie i pobieranie aplikacji i ustawień użytkownika i powiązania danych może być używany do ustawień utworzonych za pomocą biblioteki.


## <a name="related-links"></a>Linki pokrewne

- [Pobieranie książki elektronicznej (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
