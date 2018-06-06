---
title: Jednostka testowania aplikacji platformy Xamarin.iOS
description: Ten dokument zawiera omówienie sposobu testu jednostkowego aplikacji platformy Xamarin.iOS. Przedstawiono sposób tworzenia projektu testu jednostkowego, c++ pozwala pisać testy i uruchamiania testów.
ms.prod: xamarin
ms.assetid: BD959779-3239-79B6-5289-3A9ECDFBD973
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: ce2b452d50222ac3561dab5b76915b7ae634934b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785466"
---
# <a name="unit-testing-xamarinios-apps"></a>Jednostka testowania aplikacji platformy Xamarin.iOS

Ten dokument zawiera opis sposobu tworzenia testów jednostkowych dla projektów platformy Xamarin.iOS.
Testowanie jednostkowe na platformie Xamarin.iOS odbywa się przy użyciu platformy Touch.Unit, która obejmuje zarówno dla systemu iOS uruchamiający, a także zmodyfikowanej wersji NUnit o nazwie [Touch.Unit](https://github.com/xamarin/Touch.Unit) udostępniająca znanych zestaw interfejsów API do pisania testów jednostkowych.

## <a name="setting-up-a-test-project"></a>Konfigurowanie projektu testowego

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Można skonfigurować testowania framework projektu jednostki, wszystko co należy zrobić jest dodanie do rozwiązania projekt typu **iOS projektu testów jednostkowych**. W tym celu prawym przyciskiem myszy rozwiązanie i wybierając **Dodaj > Dodawanie nowego projektu**. Wybierz z listy **systemu iOS > testy > Unified API > iOS projektu testów jednostkowych** (możesz wybrać C# lub języka F #).

![](touch.unit-images/00.png "Wybierz opcję C# lub języka F #")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Można skonfigurować testowania framework projektu jednostki, wszystko co należy zrobić jest dodanie do rozwiązania projekt typu **iOS projektu testów jednostkowych**. W tym celu prawym przyciskiem myszy rozwiązanie i wybierając **Dodaj > Nowy projekt...** . Wybierz z listy **Visual C# > iOS > aplikacji testów jednostkowych (iOS)**.

![](touch.unit-images/00a.png "IOS aplikacji testów jednostkowych")

-----

Powyższe utworzy podstawowy projekt, który zawiera program podstawowe modułu uruchamiającego i który odwołuje się do nowego zestawu MonoTouch.NUnitLite projektu będzie wyglądać następująco:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](touch.unit-images/01.png "Projekt w Eksploratorze rozwiązań")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](touch.unit-images/01a.png "Projekt w Eksploratorze rozwiązań")

-----

`AppDelegate.cs` Klasa zawiera uruchamiający i wygląda następująco:

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
        UIWindow window;
        TouchRunner runner;

        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
                // create a new window instance based on the screen size
                window = new UIWindow (UIScreen.MainScreen.Bounds);
                runner = new TouchRunner (window);

                // register every tests included in the main application/assembly
                runner.Add (System.Reflection.Assembly.GetExecutingAssembly ());

                window.RootViewController = new UINavigationController (runner.GetViewController ());

                // make the window visible
                window.MakeKeyAndVisible ();

                return true;
        }
}
```

## <a name="writing-some-tests"></a>Pisanie niektórych testów

Teraz, gdy masz podstawowe powłoki w miejscu, należy zapisać swój pierwszy zestaw testów.

Testy są zapisywane przez utworzenie klas, które mają `[TestFixture]` atrybut zastosowany do nich. W każdej klasie TestFixture powinny zostać zastosowane `[Test]` atrybutu do każdej metody, która ma uruchamiający do wywołania. Instalacje rzeczywistego badania można na żywo w dowolnym pliku w projekcie testy.

Aby szybko rozpocząć wybierz **Add/Dodaj nowy plik** i wybierz w grupie Xamarin.iOS **UnitTests**. Spowoduje to dodanie szkielet plik zawierający co testy ignorowane, niepowodzeniu jednego testu i przekazywanie jednego testu, wygląda następująco:

```csharp
using System;
using NUnit.Framework;

namespace Fixtures {

        [TestFixture]
        public class Tests {

                [Test]
                public void Pass ()
                {
                        Assert.True (true);
                }

                [Test]
                public void Fail ()
                {
                        Assert.False (true);
                }

                [Test]
                [Ignore ("another time")]
                public void Ignore ()
                {
                        Assert.True (false);
                }
        }
}
```

## <a name="running-your-tests"></a>Uruchamiania testów

Aby uruchomić ten projekt w rozwiązaniu kliknij prawym przyciskiem myszy go i wybierz **debugowania elementu** lub **Uruchom element**.

Uruchamiający pozwala zobaczyć, które testy są rejestrowane i wybierz pojedynczo, które testy, które mogą być wykonywane.

[![](touch.unit-images/02.png "Lista zarejestrowanych testów")](touch.unit-images/02.png#lightbox) 

[![](touch.unit-images/03.png "Poszczególne tekstu")](touch.unit-images/03.png#lightbox) 

[![](touch.unit-images/04.png "Wyniki uruchomienia")](touch.unit-images/04.png#lightbox)

Można uruchomić poszczególnych testów osprzętu, wybierając osprzętu tekstu z zagnieżdżone widoki lub można uruchamiać wszystkie testy z "Uruchom wszystko". Jeśli uruchomienie testu domyślny, który ma zostać obejmują przekazywanie jednego testu, jeden błąd i jeden test została zignorowana. Jest to, jak wygląda raport i można przejść bezpośrednio do testów się niepowodzeniem i dowiedzieć się więcej o awarii:

[![](touch.unit-images/05.png "Przykładowy raport") ](touch.unit-images/05.png#lightbox) [ ![ ] (touch.unit-images/05.png "przykładowy raport") ](touch.unit-images/05.png#lightbox) [ ![ ] (touch.unit-images/05.png "próbki Raport")](touch.unit-images/05.png#lightbox)

W oknie danych wyjściowych aplikacji można również sprawdzić w środowiskiem IDE, aby zobaczyć, które testy są wykonywane i ich bieżący stan.

## <a name="writing-new-tests"></a>Zapisywanie nowych testów

NUnitLite jest zmodyfikowanej wersji NUnit o nazwie [Touch.Unit](https://github.com/xamarin/Touch.Unit) projektu. Jest lekki framework testowania dla platformy .NET, oparte na pomysły w [NUnit](http://nunit.com/) i oferujący podzbiór funkcji.
Używa zasobów minimalny, a zostanie uruchomiony na platformach ograniczonej zasobów takich jak te używane w tworzeniu osadzonych i przenośnych. Możesz [Przeglądaj interfejs API NUnitLite](https://developer.xamarin.com/api/namespace/NUnitLite/) dostępnych w programie platformy Xamarin.iOS. Podstawowy szkielet dostarczone przez szablon testów jednostkowych, są Twoje główny punkt wejścia [Assert klasy](https://developer.xamarin.com/api/type/NUnit.Framework.Assert/) metody.

Oprócz metody assert klasy funkcji testowania jednostki jest podzielony na następujące obszary nazw, które są częścią NUnitLite:

-   [NUnit.Framework](https://developer.xamarin.com/api/namespace/NUnit.Framework/)
-   [NUnit.Constraints](https://developer.xamarin.com/api/namespace/NUnit.Framework.Constraints/)
-   [NUnitLite](https://developer.xamarin.com/api/namespace/NUnitLite/)
-   [NUniteLite.Runner](https://developer.xamarin.com/api/namespace/NUnitLite.Runner/)


Jednostka specyficzne dla platformy Xamarin.iOS uruchamiający jest opisane tutaj:

-   [NUnit.UI.TouchRunner](https://developer.xamarin.com/api/type/NUnit.UI.TouchRunner/)
