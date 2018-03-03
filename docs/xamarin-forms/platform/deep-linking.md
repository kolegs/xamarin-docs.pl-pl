---
title: "Indeksowanie aplikacji i połączeń bezpośrednich"
description: "Indeksowanie aplikacji umożliwia aplikacji, które w przeciwnym razie mogłyby zapomniane, po kilku używa pozostanie odpowiednich przez znajdujących się w wynikach wyszukiwania. Bezpośrednich połączeń umożliwia aplikacjom na odpowiadanie na wynik wyszukiwania, które zwykle zawiera dane aplikacji, przechodząc na stronę odwołanie z link bezpośredni. W tym artykule przedstawiono sposób użycia aplikacji indeksowanie i bezpośrednich połączeń aby zawartość aplikacji platformy Xamarin.Forms można wyszukiwać w systemach iOS i urządzeniach z systemem Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: b2decf1331764ed6b1696126d8b23318e329e0c7
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="application-indexing-and-deep-linking"></a>Indeksowanie aplikacji i połączeń bezpośrednich

_Indeksowanie aplikacji umożliwia aplikacji, które w przeciwnym razie mogłyby zapomniane, po kilku używa pozostanie odpowiednich przez znajdujących się w wynikach wyszukiwania. Bezpośrednich połączeń umożliwia aplikacjom na odpowiadanie na wynik wyszukiwania, które zwykle zawiera dane aplikacji, przechodząc na stronę odwołanie z link bezpośredni. W tym artykule przedstawiono sposób użycia aplikacji indeksowanie i bezpośrednich połączeń aby zawartość aplikacji platformy Xamarin.Forms można wyszukiwać w systemach iOS i urządzeniach z systemem Android._

## <a name="overview"></a>Omówienie

Indeksowanie aplikacji platformy Xamarin.Forms i połączeń bezpośrednich podanie Publikowanie metadanych dla aplikacji indeksowania, jak użytkownicy nawigują między aplikacji interfejsu API. Indeksowana zawartość następnie mogą być wyszukiwane przez wyszukiwanie Spotlight, wyszukiwania Google lub wyszukiwania w sieci web. Naciskając pozycję w wynikach wyszukiwania, która zawiera link bezpośredni będą wyzwalać zdarzenia mogą być obsługiwane przez aplikację, która jest zwykle używana, można przejść do strony odwołanie z link bezpośredni.

Przykładowa aplikacja przedstawiono aplikację listy Todo, gdy dane są przechowywane w lokalnej bazie danych SQLite, jak pokazano na poniższych zrzutach ekranu:

![](deep-linking-images/screenshots.png "Aplikacja TodoList")

Każdy `TodoItem` jest indeksowana wystąpienia utworzone przez użytkownika. Aby zlokalizować zindeksowanych danych z aplikacji można następnie wyszukiwania specyficznego dla platformy. Po naciśnięciu element wyników wyszukiwania dla aplikacji, aplikacja jest uruchamiana, `TodoItemPage` przejście i `TodoItem` odwołanie z dokładnego zostanie wyświetlony link.

Aby uzyskać więcej informacji na temat korzystania z bazy danych SQLite, zobacz [Praca z lokalnej bazy danych](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> **Uwaga**: aplikacji platformy Xamarin.Forms indeksowanie i głębokość łączenie funkcji jest dostępna w systemach iOS i Android platform tylko i odpowiednio wymaga systemu iOS 9 i 23 interfejsu API.

## <a name="setup"></a>Konfiguracja

Wszelkie dodatkowe instrukcje dotyczące konfiguracji dla tej funkcji w systemach iOS i Android platform można znaleźć w poniższych sekcjach.

### <a name="ios"></a>iOS

Na platformie iOS nie istnieje żadne dodatkowe ustawienia wymagane do używania tej funkcji.

### <a name="android"></a>Android

Na platformie Android istnieje szereg wymagań wstępnych, które muszą zostać spełnione, aby indeksowania aplikacji i bezpośrednie łączenie funkcji:

1. Wersja aplikacji musi być na żywo w witrynie Google Play.
1. Pomocnik witryny sieci Web musi być zarejestrowana przed aplikacji w konsoli dla deweloperów firmy Google. Gdy aplikacja jest skojarzony z witryny sieci Web, adresy URL może być indeksowane pracy dla witryny sieci Web i aplikacji, które następnie mogą być przekazywane w wynikach wyszukiwania. Aby uzyskać więcej informacji, zobacz [aplikacji indeksowanie wyszukiwania Google](https://support.google.com/googleplay/android-developer/answer/6041489) w witrynie sieci Web firmy Google.
1. Aplikacja musi obsługiwać intencje adresu URL HTTP na `MainActivity` może odpowiadać klasy, którą ustalić aplikacji indeksowania rodzaje systemów danych adresu URL aplikacji. Aby uzyskać więcej informacji, zobacz [Konfigurowanie filtru zamiar](~/android/platform/app-linking.md#configure-intent-filter).

Po spełnieniu tych wymaganiach wstępnych następujące dodatkowe ustawienia musisz wybrać indeksowanie aplikacji platformy Xamarin.Forms i bezpośrednich połączeń na platformie Android:

1. Zainstaluj [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) pakiet NuGet do projektu aplikacji systemu Android.
1. W `MainActivity.cs` pliku, zaimportuj `Xamarin.Forms.Platform.Android.AppLinks` przestrzeni nazw.
1. W `MainActivity.OnCreate` zastępowania, Dodaj następujący wiersz kodu poniżej `Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

Aby uzyskać więcej informacji, zobacz [głębokiego łącza zawartości z platformy Xamarin.Forms adres URL nawigacji](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) na blogu Xamarin.

## <a name="indexing-a-page"></a>Indeksowania strony

Proces indeksowania strony i ujawnienie go do wyszukiwania Google i uwagi wygląda następująco:

1. Utwórz [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) zawierający metadane wymagane do indeksu strony, oraz link bezpośredni, aby powrócić do strony, gdy użytkownik wybierze indeksowana zawartość w wynikach wyszukiwania.
1. Zarejestruj [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) wystąpienie do indeksu go do wyszukiwania.

Poniższy przykładowy kod przedstawia sposób tworzenia [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) wystąpienie:

```csharp
AppLinkEntry GetAppLink (TodoItem item)
{
  var pageType = GetType ().ToString ();
  var pageLink = new AppLinkEntry {
    Title = item.Name,
    Description = item.Notes,
    AppLinkUri = new Uri (string.Format ("http://{0}/{1}?id={2}",
      App.AppName, pageType, WebUtility.UrlEncode (item.ID)), UriKind.RelativeOrAbsolute),
    IsLinkActive = true,
    Thumbnail = ImageSource.FromFile ("monkey.png")
  };

  return pageLink;
}
```

[ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) Wystąpienie zawiera wiele właściwości, których wartości są wymagane do strony indeksu i tworzenie głębokiego łącza. [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Title/), [ `Description` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Description/), I [ `Thumbnail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Thumbnail/) właściwości są używane do identyfikowania indeksowana zawartość, gdy jest wyświetlana w wynikach wyszukiwania. [ `IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) Właściwość jest ustawiona na `true` aby wskazać, że indeksowana zawartość jest aktualnie wyświetlany. [ `AppLinkUri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.AppLinkUri/) Właściwość jest `Uri` zawierający informacje wymagane, aby powrócić do bieżącej strony i wyświetlić bieżącą `TodoItem`. W poniższym przykładzie przedstawiono przykład `Uri` dla przykładowej aplikacji:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

To `Uri` zawiera wszystkie informacje wymagane do uruchomienia `deeplinking` aplikacji, przejdź do `DeepLinking.TodoItemPage`i wyświetlić `TodoItem` mający `ID` z `ec38ebd1-811e-4809-8a55-0d028fce7819`.

## <a name="registering-content-for-indexing"></a>Rejestrowanie zawartości na potrzeby indeksowania

Raz [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) wystąpienia został utworzony, musi być zarejestrowana do indeksowania pojawią się w wynikach wyszukiwania. Jest to realizowane przy użyciu [ `RegisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.RegisterLink/p/Xamarin.Forms.IAppLinkEntry/) metody, jak pokazano w poniższym przykładzie:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Spowoduje to dodanie [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) wystąpienia aplikacji [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) kolekcji.

> [!NOTE]
> **Uwaga**: `RegisterLink` metody mogą służyć do zaktualizowania zawartości indeksowanego ze stroną.

Raz [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) wystąpienia został zarejestrowany do indeksowania, może występować w wynikach wyszukiwania. Poniższy zrzut ekranu przedstawia indeksowana zawartość znajdujących się w wynikach wyszukiwania na platformie iOS:

![](deep-linking-images/ios-search.png "Indeksowana zawartość w wynikach wyszukiwania w systemie iOS")

## <a name="de-registering-indexed-content"></a>Wyłączyć rejestrowanie indeksowana zawartość

[ `DeregisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.DeregisterLink/p/Xamarin.Forms.IAppLinkEntry/) Metoda służy do usuwania indeksowana zawartość z wyników wyszukiwania, jak pokazano w poniższym przykładzie:

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Spowoduje to usunięcie [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) wystąpienie z aplikacji [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) kolekcji.

> [!NOTE]
> **Uwaga**: W systemie Android nie jest możliwe usunięcie indeksowana zawartość z wyników wyszukiwania.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Odpowiada na Link bezpośredni

Gdy indeksowana zawartość jest wyświetlana w wynikach wyszukiwania i jest wybierany przez użytkownika, `App` klasy dla aplikacji będą otrzymywać żądania obsługi `Uri` zawartych w indeksowanej zawartości. To żądanie mogą być przetwarzane w [ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) zastąpić, jak pokazano w poniższym przykładzie:

```csharp
public class App : Application
{
  ...

  protected override async void OnAppLinkRequestReceived (Uri uri)
  {
    string appDomain = "http://" + App.AppName.ToLowerInvariant () + "/";
    if (!uri.ToString ().ToLowerInvariant ().StartsWith (appDomain)) {
      return;
    }

    string pageUrl = uri.ToString ().Replace (appDomain, string.Empty).Trim ();
    var parts = pageUrl.Split ('?');
    string page = parts [0];
    string pageParameter = parts [1].Replace ("id=", string.Empty);

    var formsPage = Activator.CreateInstance (Type.GetType (page));
    var todoItemPage = formsPage as TodoItemPage;
    if (todoItemPage != null) {
      var todoItem = App.Database.Find (pageParameter);
      todoItemPage.BindingContext = todoItem;
      await MainPage.Navigation.PushAsync (formsPage as Page);
    }

    base.OnAppLinkRequestReceived (uri);
  }
}
```

[ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) Metoda sprawdza, czy odebranej `Uri` jest przeznaczona dla aplikacji, przed analizą `Uri` do strony, aby nastąpi przejście, a parametr do przekazania do strony. Wystąpienie strony, nastąpi przejście do tworzenia i `TodoItem` reprezentowany przez strony są pobierane parametru. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Strony, nastąpi przejście, następnie ustawiono `TodoItem`. Gwarantuje to, że w przypadku `TodoItemPage` jest wyświetlany za [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/) metody go będzie pokazywał `TodoItem` którego `ID` znajduje się w głębokiego łącza.

## <a name="making-content-available-for-search-indexing"></a>Udostępnianie zawartości do indeksowania wyszukiwania

Zawsze jest wyświetlana strona reprezentowany przez link bezpośredni, [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) ustawioną właściwość `true`. W systemach iOS i Android w ten sposób [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) dostępne dla indeksowanie wyszukiwania, a w systemie iOS tylko wystąpienia, zapewnia także `AppLinkEntry` dostępne dla przekazaniem wystąpienia. Aby uzyskać więcej informacji na temat przekazaniem zobacz [wprowadzenie do przekazaniem](~/ios/platform/handoff.md).

Poniższy przykład kodu pokazuje ustawienie [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) właściwości `true` w [ `Page.OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) zastąpienia:

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

Podobnie, gdy strona reprezentowany przez link bezpośredni jest opuszczeniu, [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) ustawioną właściwość `false`. W systemach iOS i Android, powoduje to zatrzymanie [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) wystąpienia anonsowane dla indeksowanie wyszukiwania, a na it tylko systemu iOS również zatrzymuje reklamy `AppLinkEntry` wystąpienie przekazaniem. Można to zrobić w [ `Page.OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) zastąpić, jak pokazano w poniższym przykładzie:

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>Dostarcza dane do przekazaniem

W systemach iOS mogą być przechowywane dane specyficzne dla aplikacji, podczas indeksowania strony. Jest to osiągane przez dodanie danych do [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) kolekcji, która jest `Dictionary<string, string>` do przechowywania par klucz wartość, które są używane w przekazaniem. Przekazaniem daje użytkownikowi uruchomienia działania w jednej ze swoich urządzeń i kontynuować danego działania na innym urządzeniu (określone przez użytkownika konto iCloud). Poniższy kod przedstawia przykład przechowywania par klucz wartość specyficznych dla aplikacji:

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

Wartości przechowywane w [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) kolekcji będą przechowywane w metadanych dla indeksowanego strony i zostanie przywrócona po naciśnięciu wyniku wyszukiwania zawiera link bezpośredni (lub gdy programowi Handoff służy do wyświetlania zawartości na inny zalogowany urządzenia).

Ponadto można określić wartości dla następujących kluczy:

- `contentType` — `string` , który określa identyfikator typu uniform indeksowana zawartość. Zalecane Konwencji do użycia dla tej wartości jest nazwą typu strony zawierającej indeksowana zawartość.
- `associatedWebPage` — `string` reprezentujący strony sieci web do odwiedzenia czy indeksowana zawartość można również wyświetlać w sieci web, czy aplikacja obsługuje przez przeglądarkę Safari głębokiego łącza.
- `shouldAddToPublicIndex` — `string` albo `true` lub `false` sterującą, czy należy dodać indeksowana zawartość do indeksu chmury publicznej firmy Apple, która następnie widoczne dla użytkowników, którzy nie zainstalowano aplikacji na urządzeniu z systemem iOS. Jednak wyłącznie z powodu zawartość została ustawiona dla publicznych indeksowania, go nie oznacza, że zostanie ona automatycznie dodana do indeksu chmury publicznej firmy Apple. Aby uzyskać więcej informacji, zobacz [publicznego indeksowanie wyszukiwania](~/ios/platform/search/nsuseractivity.md). Należy pamiętać, że ten klucz powinien być ustawiony na `false` podczas dodawania danych osobowych [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) kolekcji.

> [!NOTE]
> **Uwaga**: `KeyValues` kolekcji nie jest używany na platformie Android.

Aby uzyskać więcej informacji na temat przekazaniem zobacz [wprowadzenie do przekazaniem](~/ios/platform/handoff.md).

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia aplikacji indeksowanie i bezpośrednich połączeń aby zawartość aplikacji platformy Xamarin.Forms można wyszukiwać w systemach iOS i urządzeniach z systemem Android. Indeksowanie aplikacji umożliwia aplikacjom pozostanie odpowiednich przez znajdujących się w wynikach wyszukiwania, które w przeciwnym razie czy zapomniano o, po kilku używa.


## <a name="related-links"></a>Linki pokrewne

- [Głębokie konsolidację (przykład)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [interfejsy API wyszukiwania systemu iOS](~/ios/platform/search/index.md)
- [Łączenie aplikacji z systemem Android 6.0](~/android/platform/app-linking.md)
- [AppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)
- [IAppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinkEntry/)
- [IAppLinks](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinks/)
