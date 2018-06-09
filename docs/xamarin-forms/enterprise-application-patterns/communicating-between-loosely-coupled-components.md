---
title: Komunikacji między luźno powiązane składniki
description: 'W tym rozdziale opisano, jak aplikacji mobilnej eShopOnContainers implementuje publikowanie — wzorzec, co pozwala na podstawie komunikatu komunikacji między składnikami, które są niewygodne przez odwołania do obiektu i typu subskrypcji '
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 797032d17babe986de1357c6ac3291a4960d87ff
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245084"
---
# <a name="communicating-between-loosely-coupled-components"></a>Komunikacji między luźno powiązane składniki

Publikuj-subskrypcji wzorzec jest wzorzec przesyłania komunikatów, w którym wydawców wysyłać bez wiedzy o dowolnym odbiornikami, znany jako subskrybentów. Podobnie subskrybentów nasłuchuje określonej wiadomości, bez konieczności znajomości wszyscy wydawcy.

Zdarzenia w .NET zaimplementować publikowanie-subskrybowanie wzorca i są najbardziej proste i bezpośrednie podejście do warstwy komunikacji między składnikami, jeśli luźne powiązanie nie jest wymagane, na przykład formant i strona, która go zawiera. Jednak okresy istnienia wydawcy i subskrybencie są powiązane przez obiekt odwołań do siebie, a typ subskrybenta musi mieć odwołanie do typu wydawcy. To można utworzyć pamięci problemów z zarządzaniem, szczególnie w przypadku krótkich okresów ważności obiektów subskrybowanie zdarzeń obiektu statyczny lub długotrwałe. Jeśli program obsługi zdarzeń nie są usuwane, subskrybenta będą utrzymywane przez odwołanie do niej w wydawcy i będzie to zapobiec lub opóźnić wyrzucanie elementów bezużytecznych subskrybenta.

## <a name="introduction-to-messagingcenter"></a>Wprowadzenie do MessagingCenter

Platformy Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) klasa implementuje publikowanie-subskrypcji wzorzec, co pozwala na podstawie komunikatu komunikacji między składnikami, które są niewygodne przez odwołania do obiektu i typu. Mechanizm ten umożliwia wydawcy i subskrybentów, do komunikowania się bez odwołania do siebie, aby zmniejszyć zależności między składnikami, umożliwiając również składniki niezależnie opracowany i przetestowany.

[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Klasa udostępnia multiemisji funkcji publikowania / subskrypcji. Oznacza to, że może istnieć wiele wydawców, które publikują w pojedynczym komunikacie i może być wielu subskrybentów nasłuchiwania dla tego samego komunikatu. Rysunek 4-1 przedstawiono tę relację.

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "Multiemisji funkcji publikowania / subskrypcji")

**Rysunek 4-1.** multiemisji funkcji publikowania / subskrypcji

Wydawcy wysyłanie wiadomości przy użyciu [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender%7D/p/TSender/System.String/) metody, gdy subskrybenci nasłuchiwać komunikatów za pomocą [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) metody. Ponadto subskrybentów również anulować subskrypcję wiadomości subskrypcji, w razie potrzeby z [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender%7D/p/System.Object/System.String/) metody.

Wewnętrznie [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) klasy używa słabe odwołania. Oznacza to, że nie będzie przechowywać obiekty aktywności i pozwolą się bezużytecznych. W związku z tym należy tylko niezbędne do subskrypcję wiadomości, gdy klasa nie chce odbierać wiadomości.

Używa aplikacji mobilnej eShopOnContainers [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) klasy do komunikacji między luźno powiązane składniki. Aplikacja definiuje trzy wiadomości:

-   `AddProduct` Wiadomości są publikowane przez `CatalogViewModel` klasy, gdy element zostanie dodany do koszyka. W zamian `BasketViewModel` klasy subskrybuje wiadomości i zwiększa liczbę elementów w koszyku w odpowiedzi. Ponadto `BasketViewModel` klasy również cofnięć subskrypcji z tej wiadomości.
-   `Filter` Wiadomości są publikowane przez `CatalogViewModel` klasy, gdy użytkownik dotyczy filtr typu lub marki elementy wyświetlane w katalogu. W zamian `CatalogView` klasy subskrybuje wiadomości i aktualizacji interfejsu użytkownika, dzięki czemu są wyświetlane tylko te elementy, które spełniają kryteria filtru.
-   `ChangeTab` Wiadomości są publikowane przez `MainViewModel` klasę gdy `CheckoutViewModel` przechodzi do `MainViewModel` po pomyślnym utworzeniu i przesyłanie nową kolejność. W zamian `MainView` klasy subskrybuje wiadomości i aktualizacje interfejsu użytkownika tak że **Mój profil** karta jest aktywna, aby pokazać zamówień użytkownika.

> [!NOTE]
> Gdy [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) klasa umożliwia komunikację między klasami luźno połączonych, nie oferuje tylko architektury rozwiązania tego problemu. Na przykład komunikacji między model widoku oraz widoku robić również przez aparat wiązania i za pośrednictwem powiadomienia o zmianie właściwości. Ponadto komunikację między dwa modele widoku robić również przez przekazanie danych podczas nawigacji.

W aplikacji mobilnej eShopOnContainers[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) służy do aktualizacji w Interfejsie użytkownika w odpowiedzi na akcję występujących w innej klasy. W związku z tym komunikaty są publikowane w wątku interfejsu użytkownika z subskrybentami odbieranie komunikatów na tym samym wątku.

> [!TIP]
> Podczas wykonywania interfejsu użytkownika aktualizacji, należy kierować do wątku interfejsu użytkownika. Jeśli komunikat, który jest wysyłany z wątku w tle jest wymagany do zaktualizowania interfejsu użytkownika, przetworzyć komunikatu w wątku interfejsu użytkownika na subskrybencie, wywołując [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) metody.

Aby uzyskać więcej informacji na temat [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/), zobacz [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).

## <a name="defining-a-message"></a>Definiowanie wiadomości

[`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) wiadomości są ciągów, które są używane do identyfikowania komunikatów. Poniższy przykład kodu pokazuje komunikatów zdefiniowanych w aplikacji mobilnej eShopOnContainers:

```csharp
public class MessengerKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

W tym przykładzie wiadomości są definiowane na stałe. Zaletą tej metody jest typu kompilacji bezpieczeństwa i refaktoryzacji pomocy technicznej.

## <a name="publishing-a-message"></a>Publikowanie wiadomości

Wydawcy powiadomić subskrybentów wiadomości z jedną z [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) przeciążenia. Poniższy przykład kodu pokazuje publikowanie `AddProduct` komunikat:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

W tym przykładzie [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) metody określa trzech argumentów:

-   Pierwszy argument określa klasę nadawcy. Klasa nadawcy musi zostać określona przez żadnych subskrybentów, którzy chcą odbierać wiadomości.
-   Drugi argument określa wiadomości.
-   Trzeci argument określa danych ładunku do wysłania do subskrybenta. W tym przypadku jest danych ładunku `CatalogItem` wystąpienia.

[ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Metoda będzie publikować wiadomość i jego danych ładunku metoda fire i zapomnij. W związku z tym komunikat jest wysyłany, nawet jeśli nie ma żadnych subskrybentów zarejestrowany do odbierania wiadomości. W takiej sytuacji komunikat wysłany jest ignorowana.

> [!NOTE]
> [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Metody umożliwia kontrolowanie sposobu dostarczania komunikatów parametrów ogólnych. W związku z tym wielu wiadomości, które udostępniasz tożsamość wiadomości, ale wysyłania ładunku różnych typów danych może zostać odebrany przez różne subskrybentów.

## <a name="subscribing-to-a-message"></a>Subskrybowanie wiadomości

Subskrybenci można zarejestrować na komunikat przy użyciu jednej z [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) przeciążenia. Poniższy przykład kodu pokazuje sposób aplikacji mobilnej eShopOnContainers subskrybuje i przetwarza, `AddProduct` komunikat:

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

W tym przykładzie [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) metody subskrybuje `AddProduct` wiadomości i wykonuje delegata wywołania zwrotnego w odpowiedzi na odbieranie komunikatu. Ten delegat wywołania zwrotnego, określone jako wyrażenia lambda, wykonuje kod, który aktualizuje interfejsu użytkownika.

> [!TIP]
> Rozważ użycie niezmienne ładunku danych. Nie próbuj można zmodyfikować danych ładunku z poziomu delegata wywołania zwrotnego, ponieważ kilka wątków może być jednocześnie dostęp do odebranych danych. W tym scenariuszu należy modyfikować w celu uniknięcia błędów współbieżność danych ładunku.

Subskrybent nie może być konieczne do obsługi każde wystąpienie opublikowany komunikat, a to może być kontrolowane na podstawie argumentów typu ogólnego, które są określone na [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) metody. W tym przykładzie subskrybenta będą otrzymywać tylko `AddProduct` komunikatów wysyłanych z `CatalogViewModel` klasy, którego dane ładunku `CatalogItem` wystąpienia.

## <a name="unsubscribing-from-a-message"></a>Anulowanie subskrypcji wiadomości

Subskrybenci można anulować subskrypcję wiadomości, które mają już otrzymywać. Jest to osiągane z jednym z [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) overloads, jak pokazano w poniższym przykładzie:

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

W tym przykładzie [ `Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) składni metody odzwierciedla argumentów typu określony podczas subskrybowania odbierania `AddProduct` wiadomości.

## <a name="summary"></a>Podsumowanie

Platformy Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) klasa implementuje publikowanie-subskrypcji wzorzec, co pozwala na podstawie komunikatu komunikacji między składnikami, które są niewygodne przez odwołania do obiektu i typu. Mechanizm ten umożliwia wydawcy i subskrybentów, do komunikowania się bez odwołania do siebie, aby zmniejszyć zależności między składnikami, umożliwiając również składniki niezależnie opracowany i przetestowany.


## <a name="related-links"></a>Linki pokrewne

- [Pobieranie książki elektronicznej (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
