---
title: MessagingCenter
description: "Platformy Xamarin.Forms zawiera proste usługą obsługi wiadomości do wysyłania i odbierania wiadomości."
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: ede78d4a041f8619ff97b3da802efb18943ef8ae
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="messagingcenter"></a>MessagingCenter

_Platformy Xamarin.Forms zawiera proste usługą obsługi wiadomości do wysyłania i odbierania wiadomości._

<a name="Overview" />

## <a name="overview"></a>Omówienie

Platformy Xamarin.Forms `MessagingCenter` umożliwia wyświetlanie modeli i inne składniki do komunikowania się z bez znajomości cokolwiek innego niż prosty kontraktu komunikatu.

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>Jak działa MessagingCenter

Istnieją dwie części `MessagingCenter`:

-  ** Ten sam komunikat może nasłuchuje wielu subskrybentów.
-  ** Jeśli nie odbiorników masz subskrypcję wiadomości jest ignorowana.


`MessagingService` Jest Klasa statyczna z `Subscribe` i `Send` metod, które są używane w rozwiązaniu.

Komunikaty zawierać ciąg `message` parametr, który jest używany jako sposób *adres* wiadomości. `Subscribe` i `Send` metody umożliwia dalsze kontrolowania sposobu dostarczania komunikatów parametry ogólne — dwa komunikaty o tej samej `message` tekstu, ale argumenty typu ogólnego różnych nie zostanie dostarczony do tego samego subskrybenta.

Interfejs API dla `MessagingCenter` jest prosty:

-  Subskrypcja&lt;TSender > (subskrybent obiektu, ciąg komunikatu akcji&lt;TSender > wywołania zwrotnego, TSender źródło = null)
-  Subskrypcja&lt;TSender, TArgs > (subskrybent obiektu, ciąg komunikatu akcji&lt;TSender, TArgs > wywołania zwrotnego, TSender źródło = null)
-  Wyślij&lt;TSender > (TSender nadawcy wiadomości ciąg)
-  Wyślij&lt;TSender, TArgs > (TSender nadawcy wiadomości ciąg argumentów TArgs)
-  Anulowanie subskrypcji&lt;TSender, TArgs > (subskrybent obiektu, ciąg komunikatu)
-  Anulowanie subskrypcji&lt;TSender > (subskrybent obiektu, ciąg komunikatu)


Poniżej opisano te metody.

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>Przy użyciu MessagingCenter

W wyniku interakcji użytkownika (np. Kliknij przycisk), zdarzenia systemowe (np. Zmiana stanu formantów) lub niektóre inne zdarzenia (np. wykonanie asynchroniczne pobierania) mogą być wysyłane wiadomości. Subskrybenci może nasłuchiwanie Zmienianie wyglądu interfejsu użytkownika, zapisywanie danych wyzwalacza lub inną operację.

### <a name="simple-string-message"></a>Prosty ciąg komunikatu

Najprostsza komunikat zawiera po prostu określonym ciągiem w `message` parametru. A `Subscribe` metody który *nasłuchuje* dla poniżej — zostanie wyświetlony komunikat prostego ciągu zauważyć typu ogólnego, określając nadawca powinien być typu `MainPage`. Wszystkie klasy w rozwiązaniu mogą subskrybować wiadomości przy użyciu następującej składni:

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

W `MainPage` następujący kod do klasy *wysyła* wiadomości. `this` Parametru jest wystąpieniem `MainPage`.

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

Ciąg nie zmienia — wskazuje *typ komunikatu* i służy do określania, które subskrybentów do powiadamiania. Tego rodzaju wiadomości służy do wskazują, że niektóre zdarzenia wystąpił, taki jak "Zakończono przekazywanie", gdzie dalsze informacje nie są wymagane.

### <a name="passing-an-argument"></a>Przekazywanie argumentów

Aby przekazać argumentu z komunikatem, należy określić argument typu w `Subscribe` argumentów rodzajowych w podpis akcji.

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

Aby wysłać wiadomość z argumentem, obejmują parametr ogólny typu i wartości argumentu w `Send` wywołania metody.

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

Ten prosty przykład używa `string` argument, ale dowolny obiekt C# mogą być przekazywane.

### <a name="unsubscribe"></a>Anulowanie subskrypcji

Obiekt można zrezygnować z podpisu wiadomości, aby nie przyszłych wiadomości są dostarczane. `Unsubscribe` Składni metody powinien odzwierciedlać podpisu wiadomości (dlatego może być konieczne uwzględnienie parametr typu ogólnego argumentu komunikat).

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

MessagingCenter to prosty sposób zmniejszyć sprzężenia, szczególnie między modelami widoku. Może służyć do wysyłania i odbierania wiadomości prostego lub przekazując argument między klasami. Klasy należy zrezygnować z ich nie chce otrzymywać wiadomości.


## <a name="related-links"></a>Linki pokrewne

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
