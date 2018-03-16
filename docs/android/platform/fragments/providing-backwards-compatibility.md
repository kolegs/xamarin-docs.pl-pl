---
title: "Zapewnianie zapewnienia zgodności z pakietem Obsługa systemu Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/12/2017
ms.openlocfilehash: f09aae1445cfcf9f4225af3de37b65ebb5a1b6b2
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Zapewnianie zapewnienia zgodności z pakietem Obsługa systemu Android

Użyteczności odłamków będzie ograniczony bez zapewnienia zgodności wstępnie urządzeń z systemem Android 3.0 (11 poziom interfejsu API). Mieli tej możliwości, wprowadzono Google [Biblioteka obsługi](http://developer.android.com/sdk/compatibility-library.html) (pierwotnie używana *biblioteki systemu Android zgodności* kiedy został zwolniony) które backports niektórych interfejsów API z nowszej wersji Android do starszych wersji systemu android. Jest Android pakiet pomocy technicznej, który umożliwia urządzeń z systemem Android w wersji 1.6 (interfejs API poziom 4) do systemu Android 2.3.3. (Interfejsu API na poziomie 10).

> [!NOTE]
> Tylko `ListFragment` i `DialogFragment` są dostępne za pośrednictwem pakietu obsługi systemu Android. Brak innych fragmentu podklasy, takich jak `PreferenceFragment,` są obsługiwane w pakiecie Obsługa systemu Android. Nie będą one działać w aplikacjach 3.0 wstępnie systemu Android. 


## <a name="adding-the-support-package"></a>Dodawanie pakietu obsługi

Pakiet pomocy technicznej dla systemu Android nie jest automatycznie dodawany do aplikacji platformy Xamarin.Android. Udostępnia Xamarin [pakiet NuGet biblioteki obsługi systemu Android w wersji 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) uprościć dodawanie bibliotek obsługi do aplikacji platformy Xamarin.Android. Pakiety mają być pomocy technicznej w Twojej platformy Xamarin.Android aplikacji obejmują [biblioteki obsługi systemu Android w wersji 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) składnika do projektu platformy Xamarin.Android, jak pokazano na poniższym zrzucie ekranu: 

[![Zrzut ekranu biblioteki obsługi systemu Android w wersji 4 pakietu zostanie dodany do projektu](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

Po wykonaniu tych kroków, staje się możliwe użycie fragmentów w starszych wersjach systemu android. Interfejsy API fragmentu działa teraz tego samego w starszych wersjach, z następującymi wyjątkami: 

-   **Zmień minimalna wersja systemu Android** &ndash; aplikacji nie będzie już potrzebował pod kątem systemu Android w wersji 3.0 lub nowszej, jak pokazano poniżej: 

    [![Zrzut ekranu z Minimum Android docelowej ustawiany w obszarze manifestu systemu Android](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

-   **Rozszerzanie FragmentActivity** &ndash; działań, które prowadzą hosting fragmenty musi dziedziczyć `Android.Support.V4.App.FragmentActivity` , a nie z `Android.App.Activity` . 

-   **Zaktualizuj przestrzenie nazw** &ndash; klasy, które dziedziczą z `Android.App.Fragment` teraz musi dziedziczyć z `Android.Support.V4.App.Fragment` . Usuwanie przy użyciu instrukcji " `using Android.App;` " w górnej części kodu źródłowego pliku i zastąpić go ciągiem " `using Android.Support.V4.App` ". 

-   **Użyj SupportFragmentManager** &ndash; `Android.Support.V4.App.FragmentActivity` przedstawia `SupportingFragmentManager` właściwość, która musi być używany do odwołać się do `FragmentManager` . Na przykład: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

Wprowadzone zmiany w miejscu będzie możliwe do uruchomienia aplikacji na podstawie fragmentu systemu Android w wersji 1.6 lub 2.x, a także na pustakowym i Sandwich lodów. 


## <a name="related-links"></a>Linki pokrewne

- [Obsługa systemu android biblioteki v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
