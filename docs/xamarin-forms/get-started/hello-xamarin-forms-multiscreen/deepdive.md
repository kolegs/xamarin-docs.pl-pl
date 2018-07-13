---
title: Deep Dive Xamarin.Forms (wiele ekranów)
description: Ten artykuł zawiera wprowadzenie do nawigowania po stronach i powiązanie danych w aplikacji platformy Xamarin.Forms i przedstawiono ich użycie w aplikacji dla wielu platform wieloma ekranami.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: e4faa36c-6600-48c0-94c4-b4431103a4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 355d050fea2516dfc8ad532675048c5c5293368a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997559"
---
# <a name="xamarinforms-multiscreen-deep-dive"></a>Deep Dive Xamarin.Forms (wiele ekranów)

W [Xamarin.Forms (wiele ekranów) szybkiego startu](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/quickstart.md), aplikacja Phoneword zostało rozszerzone, aby drugi ekran, który śledzi historię wywołań dla aplikacji. W tym artykule opisano, jakie została opracowana, do tworzenia zrozumienia nawigowania po stronach i powiązanie danych w aplikacji platformy Xamarin.Forms.

## <a name="navigation"></a>Nawigacja

Xamarin.Forms udostępnia model nawigacyjny wbudowanych, który zarządza nawigacji i komfortu stos stron. Ten model implementuje ostatni na wejściu, first-out (LIFO) stos `Page` obiektów. Aby przenieść z jednej strony aplikacji będzie umożliwiać wypychanie powiadomień nowej strony do tego stosu. Aby wrócić do poprzedniej strony aplikacji zostanie wyświetlona bieżąca strona ze stosu.

Zawiera zestaw narzędzi Xamarin.Forms [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) klasę, która zarządza stos [ `Page` ](xref:Xamarin.Forms.Page) obiektów. `NavigationPage` Klasy również doda na pasku nawigacji do górnej części strony, która wyświetla tytuł i odpowiednie platformy <span class="uiitem">ponownie</span> przycisku, który będzie wrócić do poprzedniej strony. Poniższy przykład kodu pokazuje, jak opakowywać `NavigationPage` wokół pierwszej strony w aplikacji:

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new MainPage ());
}
```

Wszystkie [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpienia [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) właściwość, która udostępnia metody, aby zmodyfikować stosu strony. Te metody należy wywołać tylko, jeśli aplikacja zawiera [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Aby przejść do `CallHistoryPage`, należy wywołać [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) metody, jak pokazano w poniższym przykładzie kodu:

```csharp
async void OnCallHistory(object sender, EventArgs e)
{
    await Navigation.PushAsync (new CallHistoryPage ());
}
```

Powoduje to, że nowe `CallHistoryPage` obiekt ma zostać wypchnięty na stos nawigacji. Aby programowo wrócić do oryginalnej strony `CallHistoryPage` musi zostać wywołany obiekt [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) metody, jak pokazano w poniższym przykładzie kodu:

```csharp
await Navigation.PopAsync();
```

Jednak w aplikacji Phoneword ten kod nie jest wymagane jako [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) klasy dodaje pasek nawigacji do górnej części strony, który zawiera odpowiednią platformę <span class="uiitem">ponownie</span> przycisku, który zwróci do poprzedniej strony.

## <a name="data-binding"></a>Powiązanie danych

Wiązanie danych jest używany do uproszczenia jak Wyświetla i współpracuje ze swoimi danymi w aplikacji platformy Xamarin.Forms. Ustanawia ona połączenie między usługą interfejsu użytkownika i aplikacji. [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) Klasy zawiera wiele infrastrukturę do obsługi powiązanie danych.

Powiązanie danych definiuje relację między dwoma obiektami. *Źródła* obiektu będzie dostarczać dane. *Docelowej* obiektu spowoduje wykorzystanie (i często wyświetlany) danych z obiektu źródłowego. W aplikacji Phoneword jest cel wiążący [ `ListView` ](xref:Xamarin.Forms.ListView) formant, który wyświetla numery telefonów, podczas gdy `PhoneNumbers` kolekcja jest źródło wiążące.

`PhoneNumbers` Kolekcji są deklarowane i inicjowane w `App` klasy, jak pokazano w poniższym przykładzie kodu:

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

[ `ListView` ](xref:Xamarin.Forms.ListView) Wystąpienia są deklarowane i inicjowane w `CallHistoryPage` klasy, jak pokazano w poniższym przykładzie kodu:

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

W tym przykładzie [ `ListView` ](xref:Xamarin.Forms.ListView) kontrolka będzie wyświetlać `IEnumerable` zbierania danych, [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) wiąże właściwości. Zbieranie danych mogą być obiekty dowolnego typu, ale domyślnie `ListView` użyje `ToString` metoda każdy element, aby wyświetlić tego elementu. [ `x:Static` ](xref:Xamarin.Forms.Xaml.StaticExtension) — Rozszerzenie znaczników jest używany do wskazania, że `ItemsSource` będą powiązane właściwości statycznej `PhoneNumbers` właściwość `App` klasy, która może znajdować się w `local` przestrzeni nazw .

Aby uzyskać więcej informacji na temat tworzenia powiązań danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md). Aby uzyskać więcej informacji na temat rozszerzeń struktury znaczników XAML, zobacz [rozszerzeń struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="additional-concepts-introduced-in-phoneword"></a>Dodatkowe założenia Phoneword

[ `ListView` ](xref:Xamarin.Forms.ListView) Odpowiada za wyświetlanie kolekcję elementów na ekranie. Każdy element na `ListView` znajduje się w jednej komórce. Aby uzyskać więcej informacji o korzystaniu z `ListView` sterowania, zobacz [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="summary"></a>Podsumowanie

W tym artykule ma wprowadzone nawigowania po stronach i powiązanie danych w aplikacji platformy Xamarin.Forms i przedstawiono ich użycie w aplikacji dla wielu platform wieloma ekranami.
