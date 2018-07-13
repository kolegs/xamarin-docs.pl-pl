---
title: Komunikacja między luźno sprzężonymi składnikami
description: 'W tym rozdziale wyjaśniono, jak aplikacja mobilna w ramach aplikacji eShopOnContainers implementuje publikowania — wzorzec, umożliwiając oparta na komunikatach komunikacji między składnikami, które są wygodne, aby połączyć za pomocą obiektu i typu odwołania do subskrybowania '
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: ddc33d28aad4e00c9259893c0f8e7a1ab40ee429
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998547"
---
# <a name="communicating-between-loosely-coupled-components"></a>Komunikacja między luźno sprzężonymi składnikami

Publikuj — Subskrybuj wzorzec jest wzorzec przesyłania komunikatów dotyczących, w którym wydawcy wysyłania wiadomości bez dysponowania wiedzą na temat dowolnego odbiornikami, znane jako subskrybentów. Podobnie subskrybenci nasłuchuje określonych wiadomości, bez konieczności znajomości wszyscy wydawcy.

Zdarzenia w .NET zaimplementować publikowania-subskrypcji wzorzec i są najbardziej niezwykle proste podejście do warstwa komunikacji między składnikami, jeśli luźne powiązanie nie jest wymagane, takie jak formant i na stronie zawierającej go. Jednak wydawcą a transakcyjnym subskrybentem okresy istnienia są powiązane przez odwołania do obiektu ze sobą, a typ subskrybenta musi mieć odwołanie do typu wydawcy. To można utworzyć pamięci problemów z zarządzaniem, szczególnie w przypadku, gdy krótki czas życia obiektów, które subskrybują zdarzenia obiektu statycznego lub długotrwałe. Jeśli program obsługi zdarzeń nie są usuwane, subskrybenta pozostaną aktywne przez odwołanie do niego w wydawcy i będzie to zapobiec lub opóźnienie wyrzucanie elementów bezużytecznych subskrybenta.

## <a name="introduction-to-messagingcenter"></a>Wprowadzenie do MessagingCenter

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) klasa implementuje Publikuj — Subskrybuj wzorzec, dzięki czemu oparta na komunikatach komunikacji między składnikami, które są używane przez odwołania do obiektu i typu nie można użyć. Ten mechanizm pozwala wydawcy i subskrybenci do komunikowania się bez konieczności odwołanie do siebie, co pomogło w zmniejszeniu zależności między składnikami w zachowaniu składniki niezależne opracowany i przetestowany.

[ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) Udostępnia multiemisji publikowania/subskrybowania funkcji. Oznacza to, że może istnieć wiele wydawców, publikowanych jeden komunikat o i może być wielu subskrybentów nasłuchiwanie tego samego komunikatu. Rysunek 4-1 ilustruje tę relację:

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "Multiemisji publikowania/subskrybowania funkcji")

**Rysunek 4-1:** multiemisji publikowania/subskrybowania funkcji

Wydawcy wysyłania komunikatów przy użyciu [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) metody, gdy subskrybenci nasłuchiwać komunikatów za pomocą [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) metody. Ponadto subskrybenci również anulować subskrypcję z subskrypcji wiadomości, w razie potrzeby z [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) metody.

Wewnętrznie [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) klasa używa słabe odwołania. Oznacza to, że nie będą przechowywane obiektów aktywności i umożliwi im się bezużyteczne. W związku z tym należy tylko niezbędne zrezygnować z wiadomości, gdy klasa nie jest już chce otrzymywać wiadomości.

Zastosowań aplikacji mobilnej w ramach aplikacji eShopOnContainers [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) klasy do komunikacji między luźno sprzężonymi składnikami. Aplikacja definiuje trzy wiadomości:

-   `AddProduct` Komunikat zostanie opublikowany przez `CatalogViewModel` klasy po dodaniu elementu do koszyka. W zamian `BasketViewModel` klasy subskrybuje wiadomości i zwiększa liczbę elementów w koszyku w odpowiedzi. Ponadto `BasketViewModel` klasy również anulowań subskrypcji z tej wiadomości.
-   `Filter` Komunikat zostanie opublikowany przez `CatalogViewModel` klasy po użytkownik stosuje filtr typu lub marki elementy wyświetlane z katalogu. W zamian `CatalogView` klasy subskrybuje wiadomości i aktualizacje interfejsu użytkownika, aby były wyświetlane tylko te elementy, które spełniają kryteria filtru.
-   `ChangeTab` Komunikat zostanie opublikowany przez `MainViewModel` klasę gdy `CheckoutViewModel` przechodzi do `MainViewModel` po pomyślnym utworzeniu i przesyłania nowego zamówienia. W zamian `MainView` klasy subskrybuje wiadomości i aktualizacje interfejsu użytkownika tak, **Mój profil** karta jest aktywny, aby pokazać zamówienia przez użytkownika.

> [!NOTE]
> Gdy [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) klasy umożliwia komunikację między klasami luźno powiązane, nie oferuje tylko architektury rozwiązania tego problemu. Na przykład komunikacji między model widoku oraz widoku robić również przez mechanizm wiązania i za pośrednictwem powiadomienia o zmianie właściwości. Ponadto komunikację między dwoma modelami widoku robić również przez przekazanie danych podczas nawigowania.

W ramach aplikacji eShopOnContainers aplikacji mobilnej[ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) służy do aktualizacji w Interfejsie użytkownika w odpowiedzi na akcję, które pojawiają się w innej klasy. W związku z tym komunikaty są publikowane w wątku interfejsu użytkownika z subskrypcją odbierania komunikatów na tym samym wątku.

> [!TIP]
> Podczas wykonywania interfejsu użytkownika aktualizacji, należy kierować do wątku interfejsu użytkownika. Jeśli komunikat, który jest wysyłany z wątku w tle jest wymagany do zaktualizowania interfejsu użytkownika, przetworzyć komunikatu w wątku interfejsu użytkownika na subskrybencie, wywołując [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) metody.

Aby uzyskać więcej informacji na temat [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter), zobacz [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).

## <a name="defining-a-message"></a>Definiowanie wiadomości

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) komunikaty są ciągów, które są używane do identyfikowania komunikatów. Poniższy przykład kodu pokazuje wiadomości w aplikacji mobilnej w ramach aplikacji eShopOnContainers zdefiniowane:

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

W tym przykładzie można definiować komunikaty przy użyciu stałych. Zaletą tego podejścia jest zapewnia bezpieczeństwo typów w czasie kompilacji i refaktoryzacji pomocy technicznej.

## <a name="publishing-a-message"></a>Publikowanie komunikatu

Wydawcy Powiadom subskrybentów wiadomości przy użyciu jednego z [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) przeciążenia. Poniższy przykład kodu demonstruje publikowania `AddProduct` komunikat:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

W tym przykładzie [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) metody określa trzy argumenty:

-   Pierwszy argument określa klasę nadawcy. Klasa nadawcy musi zostać określona przez żadnych subskrybentów, którzy chcą odbierać wiadomości.
-   Drugi argument określa komunikat.
-   Trzeci argument określa dane ładunku do wysłania do subskrybenta. W tym przypadku dane ładunku jest `CatalogItem` wystąpienia.

[ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) Metoda będzie publikować wiadomości i jego danych ładunku, przy użyciu podejścia pożarowego i zapominać. W związku z tym wiadomość jest wysyłana, nawet jeśli nie mają żadnych subskrybentów zarejestrowanych do odbioru komunikatu. W takiej sytuacji komunikat wysłany jest ignorowany.

> [!NOTE]
> [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) Metoda umożliwia kontrolowanie sposobu dostarczania komunikatów parametrów ogólnych. W związku z tym wiele wiadomości, które mają tożsamości wiadomości, ale Wyślij ładunku różnych typów danych może zostać odebrany przez subskrybentów w innej.

## <a name="subscribing-to-a-message"></a>Subskrybowanie wiadomości

Subskrybenci mogą rejestrować do odbierania wiadomości przy użyciu jednej z [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) przeciążenia. Poniższy przykład kodu demonstruje, jak aplikacja mobilna w ramach aplikacji eShopOnContainers subskrybuje i przetwarza, `AddProduct` komunikat:

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

W tym przykładzie [ `Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) metoda subskrybuje `AddProduct` komunikat i wykonuje delegata wywołania zwrotnego w odpowiedzi do odbierania komunikatów. Ten delegat wywołania zwrotnego, określony jako wyrażenie lambda wykonuje kod, który aktualizuje interfejsu użytkownika.

> [!TIP]
> Należy rozważyć użycie danych ładunku niezmienne. Nie podejmuj próby modyfikowania danych ładunku z poziomu delegata wywołania zwrotnego, ponieważ wiele wątków może być jednocześnie dostęp do odebranych danych. W tym scenariuszu należy niezmienne, aby uniknąć błędów współbieżność danych ładunku.

Subskrybent nie może być konieczne do obsługi każdego wystąpienia opublikowany komunikat, a to może być kontrolowane przez argumenty typu ogólnego, które są określone na [ `Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) metody. W tym przykładzie, subskrybent otrzyma tylko `AddProduct` komunikatów wysyłanych z `CatalogViewModel` klasy, którego dane ładunku `CatalogItem` wystąpienia.

## <a name="unsubscribing-from-a-message"></a>Anulowanie subskrypcji wiadomość

Subskrybentów można anulować subskrypcję wiadomości, które ich nie chcesz już otrzymywać. Jest to osiągane przy użyciu jednego z [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) przeciążenia, jak pokazano w poniższym przykładzie kodu:

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

W tym przykładzie [ `Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) składni metody odzwierciedla argumentów typu, określony podczas subskrybowania do odbierania `AddProduct` wiadomości.

## <a name="summary"></a>Podsumowanie

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) klasa implementuje Publikuj — Subskrybuj wzorzec, dzięki czemu oparta na komunikatach komunikacji między składnikami, które są używane przez odwołania do obiektu i typu nie można użyć. Ten mechanizm pozwala wydawcy i subskrybenci do komunikowania się bez konieczności odwołanie do siebie, co pomogło w zmniejszeniu zależności między składnikami w zachowaniu składniki niezależne opracowany i przetestowany.


## <a name="related-links"></a>Linki pokrewne

- [Pobierz książkę elektroniczną (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [ramach aplikacji eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
