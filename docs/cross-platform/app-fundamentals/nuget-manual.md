---
title: "Ręczne tworzenie pakietów NuGet dla platformy Xamarin"
description: "Ta strona zawiera kilka wskazówek dotyczących ułatwienia tworzenia pakietów NuGet, które odnoszą się do platformy Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 1a77dcc8ae1c698e1f1ef40757ab03558f329719
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Ręczne tworzenie pakietów NuGet dla platformy Xamarin

_Ta strona zawiera kilka wskazówek dotyczących ułatwienia tworzenia pakietów NuGet, które odnoszą się do platformy Xamarin._

> [!NOTE]
> Program Xamarin Studio 6.2 (i Visual Studio for Mac) obejmuje możliwość _automatycznie_ generowania pakietów NuGet PCL, .NET Standard lub udostępnionych projektów. Zapoznaj się [Multiplatform biblioteki dla udostępniania kodu](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) przewodnika, aby uzyskać więcej informacji.

## <a name="nuget-package-xamarin-profiles"></a>Profile platformy Xamarin pakietu NuGet

Witryny NuGet [obsługi wielu wersje programu .NET Framework i profile](https://docs.nuget.org/create/enforced-package-conventions) omówiono sposób obsługi różnych platform firmy Microsoft i profile, ale nie ma nazwy framework obiektów docelowych, które są używane przez program Xamarin.

Główne docelowych platform Xamarin używany obecnie są:

* **MonoAndroid** -platformy Xamarin.Android
* **Xamarin.iOS** -Xamarin.iOS [Unified API](~/cross-platform/macios/unified/index.md) (obsługuje 64-bitowe)
* **Xamarin.Mac** -Xamarin.Mac w przenośnych profil jest odpowiednikiem interfejsów API platformy Xamarin.Android i Xamarin.iOS powierzchni.

Jest także docelowym dla systemu iOS starszych [klasycznego interfejsu API](~/cross-platform/macios/unified/index.md):

* **MonoTouch** -iOS klasycznego interfejsu API

A **.nuspec** pliku, którego wszystkie te będą wyglądać mniej więcej tak:

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

Powyższe ignoruje żadnych bibliotek klas przenośnych.

Większość **.nuspec** plików określić numer wersji platformy docelowej, ale jest opcjonalny, jeśli używanemu zestawowi działa w przypadku wszystkich wersji tej platformy docelowej. Tak, jeśli urządzenie docelowe **lib\MonoAndroid** może oznaczać to działa z dowolną wersją platformy Xamarin.Android.

Można określić wersji z zbioru liczb bez punktu dziesiętnego lub można określić przy użyciu miejsc dziesiętnych. Bez punktu dziesiętnego NuGet tylko otrzymuje każdą liczbę i włączyć ją do wersji, wstawiając "." między każdym cyfrę.

W powyższym "MonoAndroid10" oznacza "Android 1.0". Oznacza to po prostu projektu [platformy docelowej](~/android/app-fundamentals/android-api-levels.md) musi być MonoAndroid wersji 1.0 lub nowszej. Wersja jest określona w `<TargetFrameworkVersion>` elementu w pliku projektu.

Wyjaśnienie:

- **MonoAndroid403** jest zgodna z systemem Android 4.0.3 i nowsze (ie API level 15)
- **Xamarin.iOS10** odpowiada Xamarin.iOS 1.0 i nowsze
- **Xamarin.iOS1.0** zgodny Xamarin.iOS 1.0 i nowsze


## <a name="pcl-nugets-with-platform-dependencies"></a>NuGets PCL zależności platformy

Profile PCL są ograniczone w jaki .NET framework mogą uzyskiwać dostęp do interfejsów API, a ich na pewno nie ma dostępu do kodu specyficzne dla platformy. Te linki 3rd firm omówiono różne podejścia do tworzenia pakietów NuGet, korzystających z PCL i macierzystych interfejsów API zapewniają zgodność dla innych platform i Xamarin:

- [Jak dokonać pracy bibliotek klas przenośnych](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Lewy PCL przynęty i zmiany](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Tworzenie NuGet PCL, który współpracuje z platformy Xamarin.iOS](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

Ta zewnętrzne [listy profilów PCL z nazwą docelowej ich NuGet](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) jest również przydatne odwołania.

## <a name="examples"></a>Przykłady

Przykłady open source, odwołujące się do:

- [**ModernHttpClient** ](https://www.nuget.org/packages/modernhttpclient/) — pisania aplikacji z użyciem System.Net.Http, ale porzucić tę bibliotekę i nastąpi przejście co znacznie szybciej (widok [źródła](https://github.com/paulcbetts/ModernHttpClient)).
- [**Splat** ](https://www.nuget.org/packages/Splat/) — Biblioteka Postaramy między platformami, który powinien być (widok [źródła](https://github.com/paulcbetts/Splat)).
- [**NGraphics** ](https://www.nuget.org/packages/NGraphics/) — Biblioteka dla wielu platform do renderowania grafiki wektorowej na platformie .NET (widok [źródła](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)).


## <a name="related-links"></a>Linki pokrewne

- [Nugetizer 3000 automatycznego tworzenia Nuget](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)
- [Aktualizowanie NuGets dla systemu iOS 64-bitowych](http://blog.xamarin.com/how-to-update-nuget-packages-for-64-bit/)
- [W tym NuGet w projekcie](/visualstudio/mac/nuget-walkthrough/index.md)
