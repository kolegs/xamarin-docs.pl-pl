---
title: "Obsługa pustakowym wstępnego systemu Android przy użyciu pakietów pomocy technicznej"
ms.topic: article
ms.prod: xamarin
ms.assetid: DACD0C14-5DDF-7BDE-6844-80550D301307
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 9ffa3733908affb8a6f3cc5a1ae2e83c5a15f0f0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="supporting-pre-honeycomb-android-using-support-packages"></a>Obsługa pustakowym wstępnego systemu Android przy użyciu pakietów pomocy technicznej

*Android pakietu obsługi* składa się z biblioteki, w których ponownie portu niektóre z nowym interfejsem API &ndash; takich jak fragmenty &ndash; do starszych wersji systemu android. Tak przez dodanie pakietu obsługi systemu Android, firma Microsoft można uruchomić aplikacji na urządzeniach z systemem Android 2.3 pokazany następujące ekrany:

![Zrzut ekranu wskazówki fragmentów](supporting-pre-honeycomb-images/00.png)

![Zrzut ekranu przedstawiający informacje działania](supporting-pre-honeycomb-images/01.png)

<a name="Adding_the_Support_Package" />

## <a name="adding-the-support-package"></a>Dodawanie pakietu obsługi

Pakiet pomocy technicznej dla systemu Android nie jest automatycznie dodawany do aplikacji platformy Xamarin.Android. Udostępnia Xamarin [pakiet NuGet biblioteki obsługi systemu Android w wersji 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) uprościć dodawanie bibliotek obsługi do aplikacji platformy Xamarin.Android.
Pakiety mają być pomocy technicznej w Twojej platformy Xamarin.Android aplikacji obejmują [biblioteki obsługi systemu Android w wersji 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) składnika do projektu platformy Xamarin.Android, jak pokazano na poniższym zrzucie ekranu:

![Dodawanie pakietu biblioteki obsługi systemu Android w wersji 4](supporting-pre-honeycomb-images/02.png)

Po dodaniu pakietu zmienić platformę docelową dla systemu Android w wersji 2.2 lub nowszy:

![Zrzut ekranu przedstawiający zmiana docelowy Framework poziom interfejsu API](supporting-pre-honeycomb-images/03.png)

Upewnij się również, że minimalna wersja systemu Android przeznaczony na tym samym poziomie interfejsu API:

![Zrzut ekranu przedstawiający ustawienie wersji Minimum Android](supporting-pre-honeycomb-images/04.png)


<a name="Change_MainActivity_to_derive_from_FragmentActivity" />

### <a name="change-mainactivity-to-derive-from-fragmentactivity"></a>Zmień MainActivity pochodzić z FragmentActivity

Wszystkich działań korzystających fragmenty musi dziedziczyć z `Xamarin.Support.V4.App.FragmentActivity`. Ta klasa jest konieczne częścią pakietu obsługi, ponieważ zezwala ona na fragmenty ma być obsługiwana przez działania, niezależnie od systemu android. `MainActivity` wymaga tylko jednego niewielkie zmiany — teraz musi dziedziczyć z `Android.Support.V4.App.FragmentActivity`:

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```

<a name="Change_DetailsActivity_to_derive_from_FragmentActivity" />

### <a name="change-detailsactivity-to-derive-from-fragmentactivity"></a>Zmień DetailsActivity pochodzić z FragmentActivity

`DetailsActivity` musi również zostać zmieniony z `Activity` do `FragmentActivity`. Jako `FragmentManager` jest niezgodny z wersji wstępnej pustakowym systemu android, Android pakiet pomocy technicznej zawiera klasy otoki `SupportFragmentManager`, który zapewnia zgodność z poprzednimi wersjami. Każdy `FragmentActivity` ma `SupportFragmentManager` właściwości, oraz `DetailsActivity` zostanie zmieniona do używania `SupportFragmentManager` zamiast `FragmentManager`:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("index", 0);
       var details = DetailsFragment.NewInstance(index);
       var fragmentTransaction = SupportFragmentManager.BeginTransaction(); // Notice the change from FragmentManager to SupportFragmentManager
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

Po zakończeniu tych zmian, mamy teraz aplikacja, który można uruchomić na systemu Android w wersji 1.6 lub nowszej i używającą fragmenty, aby dopasować naszym interfejsu użytkownika do rozmiaru naszych urządzenia docelowego.


## <a name="related-links"></a>Linki pokrewne

- [V4 biblioteki obsługi systemu android](https://www.nuget.org/packages/Xamarin.Android.Support.v4)
