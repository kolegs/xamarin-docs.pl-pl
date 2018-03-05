---
title: "Cykl życia aplikacji"
description: "Odpowiadanie na cyklem życia aplikacji"
ms.topic: article
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: c374f261677a5bddc4ea9c73c066b69580ceedd5
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="app-lifecycle"></a>Cykl życia aplikacji

`Application` Klasy podstawowej oferuje następujące funkcje:

* [Cykl życia metody](#Lifecycle_Methods) `OnStart`, `OnSleep`, i `OnResume`.
* [Zdarzenia nawigacji modalne](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, i `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Metody cykl życia

`Application` Klasa zawiera trzy metody wirtualne, które może zostać zastąpiona w celu obsługi cyklu życia metody:

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

<a name="modal" />

## <a name="modal-navigation-events"></a>Zdarzenia nawigacji modalne

Istnieją cztery nowych zdarzeń na `Application` klasy 1.4 platformy Xamarin.Forms, każda z własnych argumenty zdarzeń:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - `ModalPoppingEventArgs` klasa zawiera `Cancel` właściwości. Gdy `Cancel` ustawiono `true` modalne pop zostało anulowane.
* **ModalPopped** - `ModalPoppedEventArgs`

Zdarzenia te pomogą lepiej zarządzać cyklu użytkowania Twojej aplikacji, umożliwiając uwzględniał modalne stronach są wyświetlane, a następnie zostanie odrzucony.

> [!NOTE]
> Do implementacji metody cyklem życia aplikacji i zdarzenia modalne nawigacji, wszystkie przed`Application` metody tworzenia aplikacji platformy Xamarin.Forms (tj. aplikacji napisanych w wersji 1.2 lub starsze, które używają statycznych `GetMainPage` — metoda) zostały zaktualizowane w celu utworzenia domyślne `Application` który jest ustawiony jako element nadrzędny `MainPage`.
>
> Aplikacji platformy Xamarin.Forms, które używają tego zachowania starszych muszą zostać zaktualizowane do `Application` podklasy zgodnie z opisem na [klasy aplikacji](~/xamarin-forms/app-fundamentals/application-class.md) strony.
