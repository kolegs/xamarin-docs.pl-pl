---
title: Indeksowanie aplikacji i tworzenie Linku
description: W tym artykule przedstawiono sposób użycia indeksowanie aplikacji i tworzenie linku się zawartość aplikacji platformy Xamarin.Forms można wyszukiwać na urządzeniach z systemem Android i iOS.
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: 7a102765a3633b8abaf01b3f090d8253230bc16b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996099"
---
# <a name="application-indexing-and-deep-linking"></a>Indeksowanie aplikacji i tworzenie Linku

_Indeksowanie aplikacji umożliwia aplikacjom, które w przeciwnym razie mogłyby zapomniane, po kilku używa realizować przez pokazywanie w wynikach wyszukiwania. Tworzenie linku umożliwia aplikacjom na wynikach wyszukiwania, który zazwyczaj zawiera dane aplikacji, przechodząc do strony, stanowiący odwołanie z linku bezpośredniego. W tym artykule przedstawiono sposób użycia indeksowanie aplikacji i tworzenie linku się zawartość aplikacji platformy Xamarin.Forms można wyszukiwać na urządzeniach z systemem Android i iOS._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Głębokie konsolidowanie za pomocą platformy Xamarin.Forms i platformą Azure przez [platformy Xamarin University](https://university.xamarin.com/)**


Indeksowanie aplikacji platformy Xamarin.Forms i tworzenie linku do zapewnienia Publikowanie metadanych dla aplikacji indeksowania, jak użytkownicy nawigują w aplikacjach interfejsu API. Indeksowanej zawartości następnie mogą być wyszukiwane przez wyszukiwanie Spotlight, wyszukiwania Google lub wyszukiwania w sieci web. Naciskając pozycję w wynikach wyszukiwania, która zawiera link bezpośredni będzie wywołać zdarzenie, może być obsługiwany przez aplikację, która jest zwykle używana do przejdź do strony, stanowiący odwołanie z linku bezpośredniego.

Przykładowa aplikacja przedstawiono aplikację listy zadań do wykonania, gdy dane są przechowywane w lokalnej bazie danych SQLite, jak pokazano na poniższych zrzutach ekranu:

![](deep-linking-images/screenshots.png "Aplikacja listy zadań")

Każdy `TodoItem` wystąpienia utworzone przez użytkownika jest indeksowana. Wyszukiwanie specyficzne dla platformy, następnie może służyć do zlokalizowania indeksowane dane z aplikacji. Kiedy użytkownik naciska na element wyników wyszukiwania dla aplikacji, aplikacja zostanie uruchomiona, `TodoItemPage` przejście i `TodoItem` stanowiący odwołanie z dokładnego zostanie wyświetlony link.

Aby uzyskać więcej informacji na temat korzystania z bazy danych SQLite, zobacz [Praca z lokalnej bazy danych](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> Indeksowanie aplikacji platformy Xamarin.Forms głębokiego łączenie funkcjonalność dostępną tylko w systemach iOS i Android platform i odpowiednio wymaga systemu iOS 9 i interfejsu API 23.

## <a name="setup"></a>Konfiguracja

Wszelkie dodatkowe instrukcje dotyczące konfiguracji dla tej funkcji w systemach iOS i Android platform można znaleźć w poniższych sekcjach.

### <a name="ios"></a>iOS

Na platformie systemu iOS nie istnieje żadna dodatkowa konfiguracja, musieli korzystać z tej funkcji.

### <a name="android"></a>Android

Na platformie systemu Android istnieje kilka wymagań wstępnych, które muszą zostać spełnione, aby indeksowanie aplikacji i głębokiego łączenie funkcji:

1. Wersja aplikacji musi być na żywo w witrynie Google Play.
1. Pomocnik witryny sieci Web musi być zarejestrowany dla aplikacji w konsoli dewelopera Google. Gdy aplikacja jest skojarzona z witryny sieci Web, adresy URL mogą mieć indeksowane pracę zarówno dla witryny sieci Web i aplikacji, które następnie mogą być przekazywane w wynikach wyszukiwania. Aby uzyskać więcej informacji, zobacz [indeksowanie aplikacji wyszukiwania Google](https://support.google.com/googleplay/android-developer/answer/6041489) w witrynie internetowej firmy Google.
1. Aplikacja musi obsługiwać intencji adresem URL protokołu HTTP na `MainActivity` klasy, która Poinformuj aplikacji indeksowania jakiego rodzaju Schematy adresów URL danych aplikacji można odpowiedzieć. Aby uzyskać więcej informacji, zobacz [Konfigurowanie filtru intencji](~/android/platform/app-linking.md#configure-intent-filter).

Po spełnieniu tych wymaganiach wstępnych następujące dodatkowe ustawienia wymagane jest wprowadzenie indeksowanie aplikacji platformy Xamarin.Forms i tworzenie linku do platformy Android:

1. Zainstaluj [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) pakiet NuGet do projektu aplikacji dla systemu Android.
1. W `MainActivity.cs` plików, zaimportuj `Xamarin.Forms.Platform.Android.AppLinks` przestrzeni nazw.
1. W `MainActivity.OnCreate` zastąpienia, Dodaj następujący wiersz kodu poniżej `Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

Aby uzyskać więcej informacji, zobacz [głębokiego łącza zawartość Nawigacja URL przy użyciu zestawu narzędzi Xamarin.Forms](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) na blogu platformy Xamarin.

## <a name="indexing-a-page"></a>Indeksowania strony

Proces indeksowania strony i ujawnienie go do serwis Google czy Spotlight search jest w następujący sposób:

1. Tworzenie [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) zawierająca metadane wymagane do indeksowania strony, wraz z linku bezpośredniego, aby powrócić do strony, gdy użytkownik wybierze indeksowanej zawartości w wynikach wyszukiwania.
1. Zarejestruj [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) wystąpienia, aby je indeksować wyszukiwania.

Poniższy przykład kodu demonstruje sposób tworzenia [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) wystąpienie:

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

[ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) Wystąpienie zawiera wiele właściwości, których wartości są wymagane do strony indeksu i tworzenie linku bezpośredniego. [ `Title` ](xref:Xamarin.Forms.IAppLinkEntry.Title), [ `Description` ](xref:Xamarin.Forms.IAppLinkEntry.Description), I [ `Thumbnail` ](xref:Xamarin.Forms.IAppLinkEntry.Thumbnail) właściwości są używane do identyfikowania indeksowanej zawartości, gdy się pojawi się w wynikach wyszukiwania. [ `IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) Właściwość jest ustawiona na `true` do wskazania, że indeksowanej zawartości jest aktualnie wyświetlany. [ `AppLinkUri` ](xref:Xamarin.Forms.IAppLinkEntry.AppLinkUri) Właściwość `Uri` zawierający informacje wymagane do wróć do bieżącej strony i wyświetlić bieżącą `TodoItem`. W poniższym przykładzie pokazano przykład `Uri` dla przykładowej aplikacji:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

To `Uri` zawiera wszystkie informacje wymagane do uruchomienia `deeplinking` aplikację, przejdź do `DeepLinking.TodoItemPage`i wyświetlić `TodoItem` zawierający `ID` z `ec38ebd1-811e-4809-8a55-0d028fce7819`.

## <a name="registering-content-for-indexing"></a>Rejestrowanie zawartości na potrzeby indeksowania

Gdy [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) utworzono wystąpienie, musi być zarejestrowana do indeksowania pojawią się w wynikach wyszukiwania. Jest to realizowane przy [ `RegisterLink` ](xref:Xamarin.Forms.IAppLinks.RegisterLink(Xamarin.Forms.IAppLinkEntry)) metody, jak pokazano w poniższym przykładzie kodu:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Spowoduje to dodanie [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) wystąpienia z aplikacją [ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks) kolekcji.

> [!NOTE]
> `RegisterLink` Metodę można również można zaktualizować zawartości, indeksowanego dla strony.

Gdy [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) wystąpienia została zarejestrowana dla indeksowania, może być wyświetlany w wynikach wyszukiwania. Poniższy zrzut ekranu przedstawia indeksowanej zawartości znajdujących się w wynikach wyszukiwania na platformie systemu iOS:

![](deep-linking-images/ios-search.png "Indeksowanej zawartości w wynikach wyszukiwania w systemie iOS")

## <a name="de-registering-indexed-content"></a>Wyrejestrowywanie indeksowana zawartość

[ `DeregisterLink` ](xref:Xamarin.Forms.IAppLinks.DeregisterLink(Xamarin.Forms.IAppLinkEntry)) Metoda służy do usuwania indeksowanej zawartości z wyników wyszukiwania, jak pokazano w poniższym przykładzie kodu:

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Spowoduje to usunięcie [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) wystąpienia aplikacji [ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks) kolekcji.

> [!NOTE]
> W systemie Android nie jest możliwe usuwanie indeksowanej zawartości z poziomu wyników wyszukiwania.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Odpowiadanie na Link bezpośredni

Gdy indeksowana zawartość jest wyświetlana w wynikach wyszukiwania jest wybierany przez użytkownika, `App` klasy, aby aplikacja otrzyma żądanie, aby obsłużyć `Uri` zawarte w indeksowanej zawartości. To żądanie mogą być przetwarzane w [ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) zastąpić, jak pokazano w poniższym przykładzie kodu:

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

[ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) Metoda sprawdza, czy odebrano `Uri` jest przeznaczony dla aplikacji, przed analizą `Uri` do strony aby nastąpi przejście, a parametr do przekazania do strony. Wystąpienie strony, nastąpi przejście, zostanie utworzony i `TodoItem` reprezentowany przez stronę parametru są pobierane. [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Strony, nastąpi przejście, zostanie następnie ustawiona `TodoItem`. Daje to gwarancję, że w przypadku `TodoItemPage` jest wyświetlany [ `PushAsync` ](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page)) metody go będzie pokazywać `TodoItem` którego `ID` znajduje się w linku bezpośredniego.

## <a name="making-content-available-for-search-indexing"></a>Udostępnianie zawartości do indeksowania wyszukiwania

Każdorazowo, zostanie wyświetlona strona, reprezentowane przez link bezpośredni, [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) właściwość może być ustawiona na `true`. W systemach iOS i Android to sprawia, że [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) wystąpienia jest dostępna dla indeksowania wyszukiwania i tylko w systemie iOS, zapewnia także `AppLinkEntry` wystąpienia dostępne dla przekazywanie. Aby uzyskać więcej informacji na temat przekazywanie zobacz [wprowadzenie do przekazywanie](~/ios/platform/handoff.md).

Poniższy przykład kodu pokazuje ustawienie [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) właściwości `true` w [ `Page.OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) zastąpienia:

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

Podobnie, gdy strona reprezentowany przez link bezpośredni jest opuszczeniu, [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) właściwość może być ustawiona na `false`. W systemach iOS i Android, spowoduje to zatrzymanie [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) wystąpienia anonsowane dla indeksowania wyszukiwania, a na it tylko dla systemu iOS również zatrzymuje reklamy `AppLinkEntry` wystąpienie przekazywanie. Można to zrobić w [ `Page.OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) zastąpić, jak pokazano w poniższym przykładzie kodu:

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>Dostarcza dane do przekazywanie

W systemach iOS mogą być przechowywane dane specyficzne dla aplikacji, podczas indeksowania strony. Jest to osiągane przez dodanie danych do [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) kolekcji, która jest `Dictionary<string, string>` do przechowywania par klucz wartość, które są używane w przekazywanie. Program handoff to sposób użytkownik może uruchomić działanie na jednym z ich urządzeń i kontynuować działania na innym urządzeniu, (określonych przez użytkownika konta usługi iCloud). Poniższy kod przedstawia przykład przechowywanie par klucz wartość specyficzne dla aplikacji:

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

Wartości przechowywane w [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) kolekcji będą przechowywane w metadanych dla strony indeksowane i zostanie przywrócona po użytkownik naciska wyniku wyszukiwania, która zawiera link bezpośredni (lub gdy program Handoff służy do wyświetlania zawartości na inny zalogowanego urządzenia).

Ponadto można określić wartości dla następujących kluczy:

- `contentType` — `string` , który określa identyfikator typu jednolitego indeksowanej zawartości. Zalecane Konwencji do użycia dla tej wartości jest nazwa typu strony zawierającej indeksowanej zawartości.
- `associatedWebPage` — `string` reprezentujący strony sieci web, aby odwiedzić czy indeksowanej zawartości można również wyświetlać w sieci web, czy aplikacja obsługuje linków bezpośrednich przeglądarki Safari.
- `shouldAddToPublicIndex` — `string` albo `true` lub `false` sterującą umożliwia określenie, czy dodać indeksowanej zawartości do indeksu chmury publicznej firmy Apple, który następnie widoczne dla użytkowników, którzy nie zainstalowano aplikacji na urządzeniu z systemem iOS. Po prostu, ponieważ zawartość została ustawiona podczas indeksowania publicznych, go nie oznacza to jednak, czy zostanie on automatycznie dodany do indeksu chmury publicznej firmy Apple. Aby uzyskać więcej informacji, zobacz [publicznych indeksowanie wyszukiwania](~/ios/platform/search/nsuseractivity.md). Należy zauważyć, że ten klucz powinien być ustawiony na `false` podczas dodawania danych osobowych [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) kolekcji.

> [!NOTE]
> `KeyValues` Kolekcji nie jest używany na platformie systemu Android.

Aby uzyskać więcej informacji na temat przekazywanie zobacz [wprowadzenie do przekazywanie](~/ios/platform/handoff.md).

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak używać indeksowanie aplikacji i tworzenie linku się zawartość aplikacji platformy Xamarin.Forms można wyszukiwać na urządzeniach z systemem Android i iOS. Indeksowanie aplikacji umożliwia aplikacjom realizować przez pokazywanie w wynikach wyszukiwania, które w przeciwnym razie czy zapomniano o, po korzysta z kilku.


## <a name="related-links"></a>Linki pokrewne

- [Łączenie głębokiego (przykład)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [interfejsy API wyszukiwania w systemie iOS](~/ios/platform/search/index.md)
- [Łączenie aplikacji dla systemu Android 6.0](~/android/platform/app-linking.md)
- [AppLinkEntry](xref:Xamarin.Forms.AppLinkEntry)
- [IAppLinkEntry](xref:Xamarin.Forms.IAppLinkEntry)
- [IAppLinks](xref:Xamarin.Forms.IAppLinks)
