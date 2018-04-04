---
title: Jak zautomatyzować projektu systemu Android NUnit testu?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/29/2018
ms.openlocfilehash: f63a25f36682038b7fcd85d711d980b9e3ec869d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Jak zautomatyzować projektu systemu Android NUnit testu?

> [!NOTE]
> W tym przewodniku objaśniono sposób automatyzowania projektu testowego NUnit systemu Android nie Xamarin.UITest projektu. Xamarin.UITest znajdują się [tutaj](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

Po utworzeniu **aplikacji testów jednostkowych (Android)** projektu programu Visual Studio (lub **Android testu jednostkowego** projektu programu Visual Studio dla komputerów Mac), to projektu zostaną automatycznie są domyślnie uruchamiane testy.
Aby uruchomić testy NUnit na urządzeniu docelowym, można utworzyć [Android.App.Instrumentation](https://developer.xamarin.com/api/type/Android.App.Instrumentation/) podklasy, który jest uruchamiany za pomocą następującego polecenia: 

```shell
adb shell am instrument 
```

Poniższe kroki objaśniają ten proces:

1.  Utwórz nowy plik o nazwie **TestInstrumentation.cs**: 

    ```cs 
    using System;
    using System.Reflection;
    using Android.App;
    using Android.Content;
    using Android.Runtime;
    using Xamarin.Android.NUnitLite;
     
    namespace App.Tests {
     
        [Instrumentation(Name="app.tests.TestInstrumentation")]
        public class TestInstrumentation : TestSuiteInstrumentation {
     
            public TestInstrumentation (IntPtr handle, JniHandleOwnership transfer) : base (handle, transfer)
            {
            }
     
            protected override void AddTests ()
            {
                AddTest (Assembly.GetExecutingAssembly ());
            }
        }
    }
    ```
    W tym pliku [Xamarin.Android.NUnitLite.TestSuiteInstrumentation](https://developer.xamarin.com/api/type/Xamarin.Android.NUnitLite.TestSuiteInstrumentation/) (z **Xamarin.Android.NUnitLite.dll**) jest podklasą klasy, aby utworzyć `TestInstrumentation`.

2.  Implementowanie [TestInstrumentation](https://developer.xamarin.com/api/constructor/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.TestSuiteInstrumentation/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Konstruktor i [AddTests](https://developer.xamarin.com/api/member/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.AddTests%28%29) metody. `AddTests` Formanty metody testy, które są rzeczywiście wykonywane.

3.  Modyfikowanie `.csproj` plik, aby dodać **TestInstrumentation.cs**. Na przykład:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        ...
        <ItemGroup>
            <Compile Include="TestInstrumentation.cs" />
        </ItemGroup>
        <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
            <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
        </Target>
        ...
    </Project>
    ```

3.  Użyj następującego polecenia do uruchomienia testów jednostkowych. Zastąp `PACKAGE_NAME` z nazwą pakietu aplikacji (nazwa pakietu znajdują się w aplikacji `/manifest/@package` atrybutu znajduje się w **AndroidManifest.xml**):

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

4.  Opcjonalnie można zmodyfikować `.csproj` plik, aby dodać `RunTests` docelowy programu MSBuild. Dzięki temu można wywołać testów jednostkowych za pomocą polecenia podobne do poniższych:

    ```shell
    msbuild /t:RunTests Project.csproj
    ```
    (Należy pamiętać, że przy użyciu tego nowy obiekt docelowy nie jest wymagana; wcześniej `adb` polecenia można użyć zamiast `msbuild`.)

Aby uzyskać więcej informacji o korzystaniu z `adb shell am instrument` polecenia do uruchomienia testów jednostkowych, zobacz deweloperów systemu Android [uruchamiania testów z ADB](https://developer.android.com/studio/test/command-line.html#RunTestsDevice) tematu.


> [!NOTE]
> Z [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) wersji domyślne nazwy pakietu dla systemu Android wywoływane otoki będą oparte na MD5SUM nazwa kwalifikowana zestawu typu eksportowane. Dzięki tej samej nazwie pełną, należy podać z dwóch różnych zestawów i nie błąd tworzenia pakietów. Dlatego należy upewnić się, że używasz `Name` właściwość `Instrumentation` atrybutu można odczytać nazwy klas Aktywnej.

_Nazwa Aktywnej musi być używany w `adb` polecenie powyżej_.
Zmiana nazwy/Refaktoryzacja klas C# w związku z tym wymaga modyfikacji `RunTests` polecenie ma korzystać z poprawną nazwę Aktywnej.

