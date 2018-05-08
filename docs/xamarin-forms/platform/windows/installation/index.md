---
title: Instalator Windows projektów
description: Dodawanie nowych projektów systemu Windows do istniejącego rozwiązania platformy Xamarin.Forms
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: b6ea988aa8c058fe5a92a17e9b72f81e0ccb12db
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="setup-windows-projects"></a>Instalator Windows projektów

_Dodawanie nowych projektów systemu Windows do istniejącego rozwiązania platformy Xamarin.Forms_

Starsze rozwiązań platformy Xamarin.Forms (lub utworzone na macOS) nie będzie miał projekty aplikacji uniwersalnych platformy systemu Windows (UWP). W związku z tym należy ręcznie dodać projekt platformy uniwersalnej systemu Windows do tworzenia aplikacji systemu Windows 10 (UWP).

<a name="pcl" />

## <a name="update-the-pcl-profile"></a>Aktualizuj profil PCL

Istniejących aplikacji platformy Xamarin.Forms użycie szablonu przenośnej biblioteki klasy (PCL), należy zaktualizować jej profilu.

1. **Kliknij prawym przyciskiem myszy > właściwości** (istniejących ustawień może różnić się)

  ![](images/targets.png "Obiekty docelowe PCL")

2. Polecenie **zmian...**  przycisku

3. Upewnij się, **systemu Windows 8** i **Windows Phone 8.1** są zaznaczone opcje (i **Silveright Windows Phone** jest *cofnąć wybrane*):

  ![](images/pcl.png "Target — opcje PCL")

4. Naciśnij klawisz **OK** i zapisać zmiany.

To jest równa **111 profilu** w przypadku konfigurowania sieci PCL w programie Visual Studio dla komputerów Mac przy użyciu listy rozwijanej.

  ![](images/pcl-xs.png "Profil PCL 111")

## <a name="add-a-universal-windows-platform-app"></a>Dodaj uniwersalnych systemu Windows platformy aplikacji

Powinna ona działać **programu Visual Studio 2017** na **systemu Windows 10** umożliwia tworzenie aplikacji platformy uniwersalnej systemu Windows. Aby uzyskać więcej informacji na temat platformy uniwersalnej systemu Windows, zobacz [wprowadzenie do platformy uniwersalnej systemu Windows](/windows/uwp/get-started/universal-application-platform-guide/).

Platformy uniwersalnej systemu Windows jest dostępny w platformy Xamarin.Forms 2.1 i nowsze, a Xamarin.Forms.Maps jest obsługiwana w wersji 2.2 platformy Xamarin.Forms i nowszych.

Sprawdź <a href="#troubleshooting">Rozwiązywanie problemów z</a> sekcji, aby uzyskać przydatne porady.

Wykonaj te instrukcje, aby dodać aplikację platformy uniwersalnej systemu Windows, które zostanie uruchomione na telefony, tablety i komputery stacjonarne systemu Windows 10:

 1 . Kliknij prawym przyciskiem myszy na rozwiązanie i wybierz **Dodaj > Nowy projekt...**  i Dodaj **pusta aplikacja (uniwersalna systemu Windows)** projektu:

  ![](universal-images/add-wu.png "Dodaj okno dialogowe nowego projektu")

 2 . W **nowego projektu platformy uniwersalnej systemu Windows** okno dialogowe, wybierz wersje systemu Windows 10, który aplikacja będzie uruchamiana na minimalna i docelowa:

  ![](universal-images/target-version.png "Dialogowym Nowy projekt platformy uniwersalnej systemu Windows")

 3 . Kliknij prawym przyciskiem myszy w projekcie platformy uniwersalnej systemu Windows i wybierz **Zarządzaj pakietami NuGet...**  i Dodaj **platformy Xamarin.Forms** pakietu. Upewnij się, że innych projektów w rozwiązaniu również zostaną zaktualizowane do tej samej wersji pakietu platformy Xamarin.Forms.

 4 . Upewnij się, że nowy projekt platformy uniwersalnej systemu Windows zostanie skompilowany **kompilacji > programu Configuration Manager** okna (to prawdopodobnie nie będzie się to zdarzyć domyślnie). Znaczników **kompilacji** i **Wdróż** pól dla uniwersalnych projektów:

  [![](universal-images/configuration-sml.png "Okno programu Configuration Manager")](universal-images/configuration.png#lightbox "okno programu Configuration Manager")

 5. Kliknij prawym przyciskiem myszy na projekt i wybierz **Dodaj > odwołania** i utworzyć odwołanie do projektu aplikacji platformy Xamarin.Forms (PCL, .NET Standard lub udostępnionego projektu).

  ![](universal-images/addref-sml.png "Okno dialogowe menedżera odwołań")

 6 . W projekcie platformy uniwersalnej systemu Windows, należy edytować **App.xaml.cs** uwzględnienie `Init` wywołanie metody wewnątrz `OnLaunched` metody w pobliżu wiersza 52:

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7 . W projekcie platformy uniwersalnej systemu Windows, należy edytować **MainPage.xaml** przez usunięcie `Grid` zawartych w `Page` elementu.

 8 . W **MainPage.xaml**, Dodaj nową `xmlns` wpis dla `Xamarin.Forms.Platform.UWP`:

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 9 . W **MainPage.xaml**, zmień katalog główny `<Page` elementu `<forms:WindowsPage`:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 10 . W projekcie platformy uniwersalnej systemu Windows, należy edytować **MainPage.xaml.cs** do usunięcia `: Page` specyfikator dziedziczenia nazwy klasy (ponieważ teraz będzie dziedziczyć `WindowsPage` z powodu zmiany wprowadzone w poprzednim kroku):

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11 . W **MainPage.xaml.cs**, Dodaj `LoadApplication` wywołanie w `MainPage` konstruktora, aby uruchomić aplikację platformy Xamarin.Forms:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

12 . Dodawanie dowolnych zasobów lokalnych (np.) pliki obrazów) istniejących projektach platformy, które są wymagane.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

<a name="target-invocation-exception" />

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>"Target wyjątek wywołania" używając "Kompiluj z użyciem łańcucha narzędzi natywnego platformy .NET"

Jeśli w aplikacji platformy uniwersalnej systemu Windows odwołuje się do wielu zestawów (na przykład firm trzecich kontrolować biblioteki lub Twojej aplikacji jest podzielony na wiele bibliotek), platformy Xamarin.Forms może być nie można załadować obiekty z tych zestawów (np. niestandardowe moduły renderowania).

Taka sytuacja może wystąpić, gdy za pomocą **Kompiluj z użyciem łańcucha narzędzi dla platformy .NET Native** czyli opcję dla aplikacji platformy uniwersalnej systemu Windows w **właściwości > kompilacji > Ogólne** okna dla projektu.

Za pomocą przeciążenia specyficzne dla platformy uniwersalnej systemu Windows może rozwiązać ten problem `Forms.Init` wywołanie w **App.xaml.cs** jak pokazano w poniższym kodzie (należy zastąpić `ClassInOtherAssembly` klasą rzeczywisty kod odwołuje się):

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

Dodaj odwołanie do każdego zestawu, do którego odwołuje się przez aplikację.

#### <a name="dependency-services-and-net-native-compilation"></a>Zależności usług oraz .NET natywnej kompilacji

Kompilacjami wydania przy użyciu platformy .NET Native kompilacji można nie można rozpoznać zależności usług, które są zdefiniowane poza aplikacji głównego pliku wykonywalnego (takich jak oddzielny projekt lub biblioteka).

Użyj `DependencyService.Register<T>()` metodę, aby ręcznie zarejestrować klasy usługi zależności. Na podstawie powyższego przykładu, Dodaj metody register następująco:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
