---
title: Automatyzowanie platformy Xamarin.Forms testowanie za pomocą Centrum aplikacji
description: Składnik Xamarin UITest może służyć z platformy Xamarin.Forms napisać testy interfejsu użytkownika do uruchamiania w chmurze na setki urządzeń.
ms.prod: xamarin
ms.assetid: b674db3d-c526-4e31-a9f4-b6d6528ce7a9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/31/2016
ms.openlocfilehash: dc43d8b5623b83be16d437e30290bc8b059be4bb
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35242950"
---
# <a name="automate-xamarinforms-testing-with-app-center"></a>Automatyzowanie platformy Xamarin.Forms testowanie za pomocą Centrum aplikacji

_Składnik Xamarin UITest może służyć z platformy Xamarin.Forms napisać testy interfejsu użytkownika do uruchamiania w chmurze na setki urządzeń._

## <a name="overview"></a>Omówienie

**[Test aplikacji Centrum](/appcenter/test-cloud/)**  umożliwia deweloperom napisać testy interfejsu użytkownika automatycznych dla systemów iOS i aplikacje dla systemu Android. Z niektórych ulepszeń niewielkie można przetestować aplikacji platformy Xamarin.Forms przy użyciu Xamarin.UITest, w tym udostępnianie tego samego kodu testu. W tym artykule przedstawiono określonej porady można pobrać Xamarin.UITest Praca z platformy Xamarin.Forms.

W tym przewodniku założono, że znajomość Xamarin.UITest. Do uzyskania znajomość Xamarin.UITest zalecane są następujące przewodniki:

- [Wprowadzenie do aplikacji Centrum testów](/appcenter/test-cloud/)
- [Wprowadzenie do UITest](/appcenter/test-cloud/preparing-for-upload/uitest/)

Po dodaniu projektu UITest rozwiązanie platformy Xamarin.Forms, kroki do zapisu i i Uruchamianie testów dla aplikacji platformy Xamarin.Forms są takie same, jak w przypadku aplikacji platformy Xamarin.Android i Xamarin.iOS.

## <a name="requirements"></a>Wymagania

Zapoznaj się [Xamarin.UITest](/appcenter/test-cloud/uitest/) aby potwierdzić gotowość do testów automatycznych interfejsu użytkownika projektu.

## <a name="adding-uitest-support-to-xamarinforms-apps"></a>Dodawanie obsługi UITest do aplikacji platformy Xamarin.Forms

UITest automatyzuje interfejsu użytkownika Aktywacja formantów na ekranie i wykonując danych wejściowych dowolnym użytkownika zwykle może współpracować z aplikacją. Aby włączyć testy, które można *naciśnij przycisk* lub *wprowadź tekst w polu* kod testu będą potrzebne do identyfikowania kontroli na ekranie.

Aby włączyć kod UITest odwołują się do formantów, każdej kontrolki musi Unikatowy identyfikator. W platformy Xamarin.Forms, zalecanym sposobem Ustaw ten identyfikator jest za pomocą `AutomationId` właściwości, jak pokazano poniżej:

```csharp
var b = new Button {
    Text = "Click me",
    AutomationId = "MyButton"
};
var l = new Label {
    Text = "Hello, Xamarin.Forms!",
    AutomationId = "MyLabel"
};
```

`AutomationId` Właściwość można ustawić w języku XAML:

```xaml
<Button x:Name="b" AutomationId="MyButton" Text="Click me"/>
<Label x:Name="l" AutomationId="MyLabel" Text="Hello, Xamarin.Forms!" />
```

Unikatową `AutomationId` powinny zostać dodane do wszystkich kontrolek, które są wymagane do testowania (w tym przycisków, wpisy tekstu i etykiety, którego wartość może być konieczne można zbadać).

> [!NOTE]
> Należy pamiętać, że `InvalidOperationException` zostanie wygenerowany, jeśli podjęto próbę ustawienia `AutomationId` właściwość `Element` więcej niż raz.

### <a name="ios-application-project"></a>Projekt aplikacji systemu iOS

Aby uruchomić testy w systemach iOS, [pakietu NuGet agenta Cloud testu Xamarin](https://www.nuget.org/packages/Xamarin.TestCloud.Agent/) musi zostać dodany do projektu. Gdy zostało ono dodane, skopiuj następujący kod do `AppDelegate.FinishedLaunching` metody:

```csharp
#if ENABLE_TEST_CLOUD
// requires Xamarin Test Cloud Agent
Xamarin.Calabash.Start();
#endif
```

Zestaw Calabash sprawia, że używa niepublicznych Apple interfejsu API co spowoduje aplikacji, które mają być odrzucane przez sklep App Store. Jednak konsolidator Xamarin.iOS spowoduje usunięcie zestawu Calabash końcowego IPA Jeśli jawnie nie jest wywoływany z kodu.

> [!NOTE]
>  Nie masz kompilacjami wydania `ENABLE_TEST_CLOUD` zmiennej kompilatora, która spowoduje, że zestaw Calabash ma zostać usunięty z pakietu aplikacji. Dyrektywy kompilatora zdefiniowane, uniemożliwia konsolidator usunięcie zestawu mają jednak kompilacji do debugowania.

Poniższy zrzut ekranu przedstawia `ENABLE_TEST_CLOUD` zmienna kompilatora ustawiona w przypadku kompilacji debugowania:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](uitest-and-test-cloud-images/12-compiler-directive-vs.png "Opcje kompilacji")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](uitest-and-test-cloud-images/11-compiler-directive-xs.png "Opcje kompilatora")

-----

### <a name="android-application-project"></a>Projekt aplikacji systemu android

W przeciwieństwie do systemu iOS projektów dla systemu Android nie ma potrzeby specjalne uruchamiania kodu.

## <a name="writing-uitests"></a>Zapisywanie UITests

Aby uzyskać informacje na temat pisania UITests, zobacz [dokumentacji UITest](/appcenter/test-cloud/preparing-for-upload/uitest/). Poniższe kroki są podsumowanie, w szczególności opisujące sposób [pokaz platformy Xamarin.Forms **UsingUITest** ](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/) jest wbudowana.

### <a name="use-automationid-in-the-xamarinforms-ui"></a>AutomationId używany w Interfejsie użytkownika platformy Xamarin.Forms

Przed żadnych UITests mogą być zapisywane, interfejs użytkownika aplikacji platformy Xamarin.Forms musi być skryptowe. Upewnij się, że wszystkie formanty w interfejsie użytkownika `AutomationId` , dzięki czemu można odwoływać w kodzie testu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="adding-a-uitest-project-to-a-new-solution"></a>Dodawanie projektu UITest do nowego rozwiązania

Podczas tworzenia nowego projektu platformy Xamarin.Forms przy użyciu programu Visual Studio dla komputerów Mac, nowy projekt UITest można dodać do rozwiązania, wybierając **Xamarin testowania chmury: Dodawanie projektu zautomatyzowanych testów interfejsu użytkownika**:

![](uitest-and-test-cloud-images/01-new-solution-xs.png "Konfigurowanie nowego projektu")

Nowe rozwiązanie zostaną skonfigurowane automatycznie uruchamiać Xamarin.UITests dla aplikacji platformy Xamarin.Forms.

-----

#### <a name="referring-to-the-automationid-in-uitests"></a>Odwołanie do AutomationId w UITests

Podczas zapisywania UITests, `AutomationId` wartość jest inaczej narażony na każdej platformie:

- **iOS** używa `id` pola.
- **Android** używa `label` pola.

Można zapisać UITests i platform, która znajdzie `AutomationId` w systemach iOS i Android za pomocą `Marked` zapytania testu:

```csharp
app.Query(c=>c.Marked("MyButton"))
```

Krótszej formy `app.Query("MyButton")` działa.

### <a name="adding-a-uitest-project-to-an-existing-solution"></a>Dodawanie projektu UITest do istniejącego rozwiązania

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio ma szablonu, aby dodać projekt Xamarin.UITest do istniejącego rozwiązania platformy Xamarin.Forms:

1. Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Plik > Nowy projekt**.
1. Z **Visual C#** szablony, wybierz **testu** kategorii. Wybierz **aplikacji testów interfejsu użytkownika > wieloplatformowych** szablonu:

    ![](uitest-and-test-cloud-images/08-new-uitest-project-vs.png "Dodaj nowy projekt")

    Spowoduje to dodanie nowego projektu z **NUnit**, **Xamarin.UITest**, i **NUnitTestAdapter** pakietów NuGet w rozwiązaniu:

    ![](uitest-and-test-cloud-images/09-new-uitest-project-xs.png "NuGet Package Manager")

    **NUnitTestAdapter** jest uruchamiający innej firmy, który umożliwia Visual Studio, aby uruchomić testy NUnit z programu Visual Studio.

    Nowy projekt również zawiera dwie klasy. **AppInitializer** zawiera kod, aby zainicjować i skonfigurować testy. Inne klasy **testy**, zawiera schematyczny kod, aby uruchomić UITests.

1. Dodaj odwołanie do projektu z projektu UITest w projekcie platformy Xamarin.Android:

    ![](uitest-and-test-cloud-images/10-test-apps-vs.png "Menedżer odwołania projektu")

    Dzięki temu będzie **NUnitTestAdapter** do uruchomienia UITests dla aplikacji systemu Android w programie Visual Studio.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Istnieje możliwość ręcznie dodać nowy projekt Xamarin.UITest do istniejącego rozwiązania:

1. Rozpocznij od dodania nowego projektu wybranie rozwiązania, a następnie klikając polecenie **pliku > Dodawanie nowego projektu**. W **nowy projekt** okno dialogowe, wybierz opcję **wieloplatformowych > testy > chmury testowej Xamarin > aplikacji testów interfejsu użytkownika**:

    ![](uitest-and-test-cloud-images/02-new-uitest-project-xs.png "Wybieranie szablonu")

    Spowoduje to dodanie nowego projektu, który ma już **NUnit** i **Xamarin.UITest** pakietów NuGet w rozwiązaniu:

    ![](uitest-and-test-cloud-images/03-new-uitest-project-xs.png "Pakiety NuGet Xamarin UITest")

    Nowy projekt również zawiera dwie klasy. **AppInitializer** zawiera kod, aby zainicjować i skonfigurować testy. Inne klasy **testy**, zawiera schematyczny kod, aby uruchomić UITests.

1. Wybierz **Widok > konsole > testów jednostkowych** do wyświetlenia w konsoli testu jednostkowego. Rozwiń węzeł **UsingUITest > UsingUITest.UITests > testowania aplikacji**:

    ![](uitest-and-test-cloud-images/04-unit-test-pad-xs.png "Konsola testów jednostkowych")

1. Kliknij prawym przyciskiem myszy **aplikacje testu**, kliknij **dodać projekt aplikacji**i wybierz w wyświetlonym oknie dialogowym projektów dla systemu Android i iOS:

    ![](uitest-and-test-cloud-images/05-add-test-apps-xs.png "Testowanie aplikacji w oknie dialogowym")

    **Testu jednostkowego** konsoli teraz powinny istnieć odwołania do projektów dla systemu Android i iOS. Pozwoli to programu Visual Studio for Mac uruchamiający lokalnie wykonać UITests przed dwa projekty platformy Xamarin.Forms.

#### <a name="adding-uitest-to-the-ios-app"></a>Dodawanie UITest do aplikacji dla systemu iOS

Istnieją pewne dodatkowe zmiany, które należy wykonać w aplikacji systemu iOS przed Xamarin.UITest będzie działać:

1. Dodaj **agenta Cloud testowego Xamarin** pakietu NuGet. Kliknij prawym przyciskiem myszy **pakiety**, wybierz pozycję **Dodawanie pakietów**, wyszukaj NuGet dla **agenta Cloud testowego Xamarin** i dodaj go do projektu platformy Xamarin.iOS:

    ![](uitest-and-test-cloud-images/07-add-test-cloud-agent-xs.png "Dodawanie pakietów NuGet")

1. Edytuj `FinishedLaunching` metody **AppDelegate** klasy zainicjować agenta Cloud testowego Xamarin podczas uruchamiania aplikacji dla systemu iOS oraz ustawić `AutomationId` właściwości widoków. `FinishedLaunching` — Metoda powinien być podobny do następującego przykładu:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
        #if ENABLE_TEST_CLOUD
        Xamarin.Calabash.Start();
        #endif

        global::Xamarin.Forms.Forms.Init();

        LoadApplication(new App());

        return base.FinishedLaunching(app, options);
}
```

-----

Po dodaniu Xamarin.UITest do rozwiązania platformy Xamarin.Forms, jest możliwość tworzenia UITests, uruchomić je lokalnie i przesyłanie ich do chmury testowej Xamarin.

## <a name="summary"></a>Podsumowanie

Można łatwo przetestować aplikacji platformy Xamarin.Forms z **Xamarin.UITest** przy użyciu mechanizmu proste do udostępnienia `AutomationId` jako identyfikator unikatowy widoku dla testów automatycznych. Po dodaniu projektu UITest rozwiązanie platformy Xamarin.Forms, kroki do zapisu i i Uruchamianie testów dla aplikacji platformy Xamarin.Forms są takie same, jak w przypadku aplikacji platformy Xamarin.Android i Xamarin.iOS.

Informacje o sposobie zgłaszania testów Test Centrum aplikacji, zobacz [przesyłanie UITests](/appcenter/test-cloud/preparing-for-upload/uitest/). Aby uzyskać więcej informacji o UITest, zobacz [Test aplikacji Centrum dokumentacji](/appcenter/test-cloud/).


## <a name="related-links"></a>Linki pokrewne

- [UITestSample](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
- [Test Centrum aplikacji](/appcenter/test-cloud/)
- [Xamarin.UITest](/appcenter/test-cloud/uitest/)
- [NUnit](http://www.nunit.org)
