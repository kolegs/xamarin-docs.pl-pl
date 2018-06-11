---
title: Wieloekranowy nowości platformy Xamarin.Forms
description: W tym artykule przedstawiono Nawigacja strony i powiązanie danych w aplikacji platformy Xamarin.Forms i przedstawiono ich użycia w aplikacji i platform wielu ekranu.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: e4faa36c-6600-48c0-94c4-b4431103a4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 1c7edff3c71b9d7530b2acf21acaa06149156d43
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35242238"
---
# <a name="xamarinforms-multiscreen-deep-dive"></a>Wieloekranowy nowości platformy Xamarin.Forms

W [Wieloekranowy szybkiego startu platformy Xamarin.Forms](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/quickstart.md), aplikacja Phoneword została rozszerzona, aby uwzględnić drugi ekranu, która przechowuje informacje o historii połączeń dla aplikacji. W tym artykule opisano, co został utworzony, aby opracować zrozumienia Nawigacja strony i powiązanie danych w aplikacji platformy Xamarin.Forms.

## <a name="navigation"></a>Nawigacji

Platformy Xamarin.Forms zawiera wbudowane nawigacji modelu, który zarządza nawigacji i środowisko użytkownika stosu stron. Ten model implementuje ostatni na, wytworzenia stos `Page` obiektów. Aby przenieść z jednej strony aplikacji przeprowadzi wypychanie nową stronę na tym stosu. Aby wrócić do poprzedniej strony aplikacji będzie pop bieżącej strony ze stosu.

Ma platformy Xamarin.Forms [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) klasy, która zarządza stos [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) obiektów. `NavigationPage` Klasy również doda na pasku nawigacji do górnej części strony, która wyświetla tytuł i odpowiednie platformy <span class="uiitem">ponownie</span> przycisku, który wróci do poprzedniej strony. Poniższy przykład kodu pokazuje sposób zawijania `NavigationPage` wokół pierwszej strony w aplikacji:

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new MainPage ());
}
```

Wszystkie [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpienia mają [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) właściwość, która udostępnia metody, aby zmodyfikować stos strony. Te metody powinna być wywoływana tylko, jeśli aplikacja zawiera [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/). Aby przejść do `CallHistoryPage`, należy wywołać [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) — metoda, jak pokazano w poniższym przykładzie kodu:

```csharp
async void OnCallHistory(object sender, EventArgs e)
{
    await Navigation.PushAsync (new CallHistoryPage ());
}
```

Powoduje to, że nowe `CallHistoryPage` obiekt, aby zostać przeniesiony na stosie nawigacji. Aby programowo wrócić do oryginalnej strony `CallHistoryPage` należy wywołać obiekt [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) metody, jak pokazano w poniższym przykładzie kodu:

```csharp
await Navigation.PopAsync();
```

Jednak w aplikacji Phoneword ten kod nie jest wymagane jako [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) klasa dodaje pasek nawigacji do górnej części strony, który zawiera odpowiednią platformę <span class="uiitem">ponownie</span> przycisku, który zwróci do poprzedniej strony.

## <a name="data-binding"></a>Powiązanie danych

Powiązanie danych służy do uproszczenia jak aplikacji platformy Xamarin.Forms Wyświetla i interakcje z jego dane. Ustanawia ona połączenie między interfejs użytkownika i podstawowej aplikacji. [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) Klasy zawiera wiele infrastrukturę do obsługi wiązania z danymi.

Powiązanie danych definiuje relację między dwoma obiektami. *Źródła* obiektu będzie dostarczać dane. *Docelowej* obiektu będzie korzystać z (i często jest wyświetlany) dane z obiektem źródłowym. W aplikacji Phoneword jest cel wiązania [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kontrolkę wyświetlającą numery telefonów podczas `PhoneNumbers` kolekcji jest źródłem powiązania.

`PhoneNumbers` Kolekcji został zadeklarowany i zainicjować w `App` klasy, jak pokazano w poniższym przykładzie:

```csharp
public partial class App : Application
{
   public static List<string> PhoneNumbers { get; set; }

   public App ()
   {
     PhoneNumbers = new List<string>();
     ...
   }
   ...
}
```

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Wystąpienia został zadeklarowany i zainicjować w `CallHistoryPage` klasy, jak pokazano w poniższym przykładzie:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    ...
    <ContentPage.Content>
       ...
       <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
       ...
    </ContentPage.Content>
</ContentPage>
```

W tym przykładzie [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kontrolka będzie wyświetlać `IEnumerable` zbierania danych który [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView.ItemsSource/) wiąże właściwości. Zbieranie danych mogą być obiekty dowolnego typu, ale domyślnie `ListView` użyje `ToString` metody poszczególnych elementów do wyświetlenia tego elementu. [ `x:Static` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) — Rozszerzenie znaczników służy do wskazania, że `ItemsSource` właściwość zostanie powiązany statycznych `PhoneNumbers` właściwość `App` klasy, która może znajdować się w `local` przestrzeni nazw .

Aby uzyskać więcej informacji na temat wiązania danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md). Aby uzyskać więcej informacji na temat rozszerzeń znaczników XAML, zobacz [rozszerzenia znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="additional-concepts-introduced-in-phoneword"></a>Dodatkowe założenia Phoneword

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Odpowiada za wyświetlanie kolekcję elementów na ekranie. Każdy element `ListView` znajduje się w jednej komórce. Aby uzyskać więcej informacji o korzystaniu z `ListView` sterowania, zobacz [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="summary"></a>Podsumowanie

W tym artykule ma Nawigacja strony oraz powiązania danych w aplikacji platformy Xamarin.Forms i przedstawiono ich użycia w aplikacji i platform wielu ekranu.
