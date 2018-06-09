---
title: Cykl życia aplikacji platformy Xamarin.Forms
description: W tym artykule wyjaśniono, jak odpowiedzieć na cyklu życia aplikacji, w tym cyklu życia metody, zdarzenia nawigacji strony i zdarzeń nawigacji modalne.
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: fb651494b63a77ede47dd246ee054b5c67af2a35
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240272"
---
# <a name="xamarinforms-app-lifecycle"></a>Cykl życia aplikacji platformy Xamarin.Forms

[ `Application` ](xref:Xamarin.Forms.Application) Klasy podstawowej oferuje następujące funkcje:

* [Cykl życia metody](#Lifecycle_Methods) `OnStart`, `OnSleep`, i `OnResume`.
* [Strona zdarzenia nawigacji](#page) [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing), [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing).
* [Zdarzenia nawigacji modalne](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, i `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Metody cykl życia

[ `Application` ](xref:Xamarin.Forms.Application) Klasa zawiera trzy metody wirtualne, które może zostać zastąpiona w celu obsługi cyklu życia metody:

* **OnStart** -wywoływana podczas uruchamiania aplikacji.

* **OnSleep** -wywoływana za każdym razem, aplikacja przechodzi do tła.

* **OnResume** -wywoływane, gdy aplikacja zostanie wznowione po są wysyłane do tła.

Należy pamiętać, że istnieje *nie* metodę Kończenie działania aplikacji.
W normalnych okolicznościach (ie. nie awarii) zakończenia aplikacji nastąpi z *OnSleep* stanu bez żadnych dodatkowych powiadomień w kodzie.

Aby obserwować, kiedy są wywoływane tych metod, zaimplementuj `WriteLine` wywołań w każdym (jak pokazano poniżej) i testowania na każdej z platform.

```csharp
protected override void OnStart()
{
    Debug.WriteLine ("OnStart");
}
protected override void OnSleep()
{
    Debug.WriteLine ("OnSleep");
}
protected override void OnResume()
{
    Debug.WriteLine ("OnResume");
}
```

Podczas aktualizowania *starsze* (np aplikacje platformy Xamarin.Forms. Tworzenie z platformy Xamarin.Forms 1.3 lub starsze), upewnij się, że działanie główne systemu Android zawiera `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` w `[Activity()]` atrybutu. Jeśli to nie jest obecny będzie widoczny `OnStart` metoda jest wywoływana na obracanie, a także po pierwszym uruchomieniu aplikacji. Ten atrybut jest automatycznie uwzględniany WE bieżące szablony aplikacji platformy Xamarin.Forms.

<a name="page" />

## <a name="page-navigation-events"></a>Nawigacja strony zdarzenia

Istnieją dwa zdarzenia na [ `Application` ](xref:Xamarin.Forms.Application) klasy, która zapewnić powiadomień strony znajdujących się i znika:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) -wywoływane, gdy strona ma są wyświetlane na ekranie.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) -wywoływane, gdy strona ma są usuwane z ekranu.

Zdarzenia te mogą służyć w scenariuszach, w którym chcesz śledzić strony, ponieważ są one wyświetlane na ekranie.

> [!NOTE]
> [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing) i [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing) zdarzenia są generowane z [ `Page` ](xref:Xamarin.Forms.Page) klasa podstawowa natychmiast po [ `Page.Appearing` ](xref:Xamarin.Forms.Page.Appearing) i [ `Page.Disappearing` ](xref:Xamarin.Forms.Page.Disappearing) zdarzenia, odpowiednio.

<a name="modal" />

## <a name="modal-navigation-events"></a>Zdarzenia nawigacji modalne

Istnieją cztery zdarzenia na [ `Application` ](xref:Xamarin.Forms.Application) klasy, każda z własnych argumenty zdarzenia, które pozwalają odpowiedzieć modalne stronach są wyświetlane, a następnie zostanie odrzucony:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - `ModalPoppingEventArgs` klasa zawiera `Cancel` właściwości. Gdy `Cancel` ustawiono `true` modalne pop zostało anulowane.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> Do implementacji metody cyklem życia aplikacji i zdarzenia modalne nawigacji, wszystkie przed`Application` metody tworzenia aplikacji platformy Xamarin.Forms (tj. aplikacji napisanych w wersji 1.2 lub starsze, które używają statycznych `GetMainPage` — metoda) zostały zaktualizowane w celu utworzenia domyślne `Application` który jest ustawiony jako element nadrzędny `MainPage`.
>
> Aplikacji platformy Xamarin.Forms, które używają tego zachowania starszych muszą zostać zaktualizowane do `Application` podklasy zgodnie z opisem na [klasy aplikacji](~/xamarin-forms/app-fundamentals/application-class.md) strony.
