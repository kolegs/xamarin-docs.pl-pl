---
title: "Inteligentny dla systemu Xamarin Android obsługuje v4 / v13 pakietów NuGet"
ms.topic: article
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: ab8d90ebd938a8417e5a48155347e03191ef9ca4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>Inteligentny dla systemu Xamarin Android obsługuje v4 / v13 pakietów NuGet

## <a name="about-the-android-support-libraries"></a>Informacje o bibliotekach Obsługa systemu Android

Google utworzył biblioteki pomocy technicznej, aby udostępnić nowe funkcje do starszych wersji systemu android. Ogólnie rzecz biorąc, bibliotek obsługi podano numeru wersji w ich nazwy, która jest najniższy poziom interfejsu API systemu Android są zgodne z (np: v4 pomocy technicznej można używać tylko na interfejs API poziom 4 lub nowszym. Aby dowiedzieć się więcej w tym [przepełnienie stosu dyskusji](http://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

Dwa z biblioteki obsługi: `Support-v4` i `Support-v13` nie mogą być używane razem w tej samej aplikacji, oznacza to, że są one wykluczają się wzajemnie. Jest to spowodowane `Support-v13` zawiera wszystkie typy i stosowania `Support-v4`. Jeśli spróbuj i odwołać się zarówno w tym samym projekcie wystąpią błędy zduplikowany typ.

## <a name="problems-with-referencing"></a>Problemy z wywoływanym

Ponieważ `Support-v4` stała się więc popularna, partii 3 bibliotek strona teraz od niego zależne. Może wybrano zależeć v13 obsługi zamiast niego, ale jest ona większa często są zależne od _v4_ ponieważ zapewniającą wszystkie aplikacje przy użyciu tych 3 bibliotek strony opcji poziomy interfejsu API, aż do 4 pomocniczych.

Jeśli odwołuje się do biblioteki strony Xamarin 3rd `Xamarin.Android.Support.v4.dll` powiązanie z `Support-v4`, również musi odwoływać się dowolną aplikację, która korzysta z tej biblioteki `Xamarin.Android.Support.v4.dll`. Jest problem, gdy ta sama aplikacja chce również korzystać z niektórych funkcji `Xamarin.Android.Support.v13.dll` powiązanie z `Support-v13`. Jeśli odwołanie zarówno powiązań, wystąpią błędy zduplikowany typ.

## <a name="type-forwarded-v4-binding-assembly"></a>Typ przekazany v4 powiązania zestawu

Aby ominąć ten problem, został utworzony specjalnie `Xamarin.Android.Support.v4.dll` zestawu, który ma implementacji, ale po prostu `[assembly: TypeForwardedTo (..)]` atrybuty, które przekazywania wszystkich `Support-v4` typów do wykonania w ciągu `Xamarin.Android.Support.v13.dll` zestawu.

Oznacza to, że deweloper może odwoływać się to _typ przekazany_ zestawu w swoich aplikacji, która będzie spełniać odwołanie do `Xamarin.Android.Support.v4.dll` przez dowolnego 3 biblioteki firm, umożliwiając `Xamarin.Android.Support.v13.dll` do użycia w aplikacji.

## <a name="nuget-assistance"></a>Pomoc NuGet

Gdy dewelopera można ręcznie dodać niezbędne odwołania poprawne, możemy użycie narzędzia NuGet, aby wybrać prawa zestawu (albo normalnej _v4_ binding lub typ przekazany _v4_ zestawu) po zainstalowano pakiet NuGet.

Tak `Xamarin.Android.Support.v4` pakietu NuGet zawiera teraz następującą logiką:

Jeśli aplikacja jest docelowo 13 poziom interfejsu API (pierniki 3.2) lub nowszy:

*   `Xamarin.Android.Support.v13` NuGet zostanie automatycznie dodany jako zależność
*   _Typ przekazany_ `Xamarin.Android.Support.v4.dll` zostanie dodane odwołanie w projekcie

Jeśli aplikacja jest przeznaczona dla niczego niższa niż 13 poziom interfejsu API, otrzymasz normalnej `Xamarin.Android.Support.v4.dll` powiązanie, do którego odwołuje się projekt.

## <a name="do-i-have-to-use-support-v13"></a>Czy mam użyć v13 obsługi?

Jeśli aplikacja jest docelowo poziom interfejsu API 13 lub nowszej i chcesz użyć `Xamarin Android Support-v4` pakietu NuGet, a następnie `Xamarin Android Support v13` pakiet NuGet jest wymagana zależność.

Uważamy, że bardzo niewielkie wzrost rozmiaru aplikacji (dwa pliki JAR różnią się 17 kb) jest również warto zgodności i mniej problemów, które jej wynikiem.

Jeśli jesteś adamant o korzystaniu z `Support-v4` w aplikacji, która dotyczy 13 poziom interfejsu API lub wyższym, należy zawsze ręcznie pobrać `.nupkg`, wyodrębnij go i odwoływać się do zestawu.
