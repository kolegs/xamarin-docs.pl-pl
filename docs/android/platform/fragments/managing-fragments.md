---
title: Zarządzanie fragmentów
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: b39067b0cb1bbb344866761042db0125fd4a4dc5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="managing-fragments"></a>Zarządzanie fragmentów

Aby pomóc w zarządzaniu fragmenty, zapewnia Android `FragmentManager` klasy. Każde działanie ma wystąpienie `Android.App.FragmentManager` którego znaleźć lub dynamicznie zmieniać jego fragmenty. Każdego zestawu zmian jest nazywany *transakcji*, jest przeprowadzane za pomocą jednego z interfejsów API zawartych w klasie `Android.App.FragmentTransation`, która jest zarządzana przez `FragmentManager`. Działanie może rozpocząć transakcji następująco:

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

Te zmiany do fragmentów są wykonywane w `FragmentTransaction` wystąpienia przy użyciu metod, takich jak `Add()`, `Remove(),` i `Replace().` zmiany są następnie stosowane przy użyciu `Commit()`. Zmiany w transakcji nie są wykonywane bezpośrednio.
Zamiast tego są zaplanowane do uruchomienia w wątku interfejsu użytkownika działania tak szybko, jak to możliwe.

Poniższy przykład przedstawia sposób dodawania Fragment do istniejącego kontenera:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

Jeśli transakcja została przekazana po `Activity.OnSaveInstanceState()` jest wywoływana, zostanie wygenerowany wyjątek. Dzieje się tak, ponieważ podczas działania zapisuje swój stan, Android również zapisuje stan dowolnych hostowanej fragmentów. Jeśli wszystkie transakcje fragmentu są zatwierdzone po tym punkcie, stan tych transakcji zostaną utracone po przywróceniu działania.

Można zapisać transakcji fragmentu działania [stosie przechodzenia wstecz](http://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) poprzez wywołanie `FragmentTransaction.AddToBackStack()`. Umożliwia użytkownikowi Przejdź wstecz za pomocą fragmentu zmiany, kiedy **ponownie** przycisk jest naciśnięty. Bez wywołania do tej metody fragmentów, które zostały usunięte zostaną zniszczone i będą niedostępne, gdy użytkownik nawiguje wstecz do działania.

Poniższy przykład przedstawia użycie `AddToBackStack` metody `FragmentTransaction` zastąpić jeden Fragment, zachowując stan pierwszego fragmentu na stosie przechodzenia wstecz:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// Replace the fragment that is in the View fragment_container (if applicable).
fragmentTx.Replace(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Add the transaction to the back stack.
fragmentTx.AddToBackStack(null);

// Commit the transaction.
fragmentTx.Commit();
```


## <a name="communicating-with-fragments"></a>Komunikowania się z fragmentów

*FragmentManager* zna wszystkie fragmenty, które są dołączone do działania i udostępnia dwie metody, aby znaleźć je fragmenty:

-   **FindFragmentById** &ndash; ta metoda zostanie ustalone, Fragment przy użyciu Identyfikatora, który został określony w pliku układu lub Identyfikatora kontenera podczas Fragment został dodany jako część transakcji.

-   **FindFragmentByTag** &ndash; ta metoda jest używana do znajdowania Fragment, który zawiera tag, która została dostarczona w pliku układu lub który został dodany w ramach transakcji.

Zarówno fragmenty, jak i działań odwołania `FragmentManager`, więc te same techniki są używane do i z powrotem komunikacji między nimi. Aplikacja może znaleźć odwołanie fragmentu przy użyciu jednej z tych dwóch metod, rzutowanie odwołujące się do odpowiedniego typu i bezpośrednio wywoływać metody na Fragment. Poniższy fragment kodu przedstawiono przykładowy:

Istnieje również możliwość działania do użycia `FragmentManager` można znaleźć fragmenty:

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```


### <a name="communicating-with-the-activity"></a>Podczas komunikacji z działania

Istnieje możliwość Fragment do użycia `Fragment.Activity` właściwości, aby odwołać jej hosta. Rzutowanie działanie, aby dokładniej typ, istnieje możliwość dla działania wywołania metod i właściwości na jej hosta, jak pokazano w poniższym przykładzie:

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
