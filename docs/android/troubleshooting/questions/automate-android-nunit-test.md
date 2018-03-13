---
title: "Jak zautomatyzować projektu systemu Android NUnit testu?"
ms.topic: article
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 11b693193b36a80b55a61308d98b76f4f6984e8a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Jak zautomatyzować projektu systemu Android NUnit testu?

> [!NOTE]
> W tym przewodniku opisano kroki konfigurowania projektu testowego NUnit systemu Android nie Xamarin.UITest projektu. Xamarin.UITest znajdują się [tutaj](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

Po utworzeniu Android jednostkowy projekt testowy [Visual Studio for Mac] lub aplikacji testów jednostkowych (Android) [Visual Studio] Domyślnie jej uruchomienie nie będzie automatycznie testów.
Aby zautomatyzować z systemem android testu jednostkowego: do uruchomienia testów NUnit na urządzeniu docelowym, używamy `Android.App.Instrumentation` podklas, które można tworzyć i wykonywać za pomocą `adb shell am instrument` polecenia.

Najpierw utworzymy **TestInstrumentation.cs** pliku, który tworzy podklasy `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` (zadeklarowany w `Xamarin.Android.NUnitLite.dll`). `TestInstrumentation(IntPtr, JniHandleOwnership)` Konstruktor _musi_ pod warunkiem i wirtualnej `AddTests()` metoda musi zostać zastąpiona.
`AddTests()` formanty, które testy faktycznie są wykonywane. Ten plik jest przede wszystkim standardowego.

Następnie `.csproj` muszą zostać zmodyfikowane w celu dodania `TestInstrumentation.cs`.

Opcjonalnie `.csproj` mogą zostać zmodyfikowane w celu dodania `RunTests` docelowy programu MSBuild, które umożliwiałyby wywoływania testów jednostkowych jako:

```shell
msbuild /t:RunTests Project.csproj
```

Przy użyciu nowy element docelowy nie jest wymagana. Zamiast tego można stosować odpowiednie polecenie adb:

```shell
adb shell am instrument -w @PACKAGE_NAME@/app.tests.TestInstrumentation
```

Zastąp `@PACKAGE\_NAME@` odpowiednio; jest obecne w wartości **AndroidManifest.xml** `/manifest/@package` atrybutu.

*Ważna Uwaga*: Z [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) wersji domyślne nazwy pakietu dla systemu Android wywoływane otoki będą oparte na MD5SUM nazwa kwalifikowana zestawu typu eksportowane. Dzięki tej samej nazwie pełną, należy podać z dwóch różnych zestawów i nie błąd tworzenia pakietów. Dlatego należy upewnić się, że używasz \`nazwa\` właściwość \`Instrumentacji\` attibute można odczytać nazwy klas Aktywnej.

_Nazwa Aktywnej musi być używany w `adb` polecenia_. Zmiana nazwy/Refaktoryzacja klas C# w związku z tym wymaga modyfikacji `RunTests` polecenie ma korzystać z poprawną nazwę Aktywnej.

Dodatki do pliku .csproj:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <ItemGroup>
        <Compile Include="TestInstrumentation.cs" />
    </ItemGroup>
    <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
        <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
    </Target>
</Project>
```

**TestInstruments.cs**:

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

