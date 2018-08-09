---
title: Wprowadzenie do systemu iOS
description: W tym dokumencie opisano, jak rozpocząć pracę, przy użyciu osadzanie kodu .NET dla systemu iOS. Go w tym artykule omówiono wymagania i przedstawia przykładową aplikację w celu zademonstrowania sposobu powiązania zestawu zarządzanego i użyć danych wyjściowych w projekcie Xcode.
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: d61eb8f1ad1def764c8552b2f047aa46cd712018
ms.sourcegitcommit: ef04a4ae1b19c1854a8e4e8315516d4030f4bbd6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39654831"
---
# <a name="getting-started-with-ios"></a>Wprowadzenie do systemu iOS

## <a name="requirements"></a>Wymagania

Oprócz wymagań dotyczących z naszych [wprowadzenie do języka Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) przewodnik, musisz również:

* [Xamarin.iOS 10.11](https://visualstudio.microsoft.com/xamarin/) lub nowszej

## <a name="hello-world"></a>Cześć ludzie

Najpierw należy utworzyć przykład proste Witaj świecie, w języku C#.

### <a name="create-c-sample"></a>Tworzenie przykładowej języka C#

Otwórz program Visual Studio dla komputerów Mac, Utwórz nowy projekt biblioteki klas dla systemu iOS, nadaj jej nazwę **hello z csharp**i zapisać go w celu **~/Projects/hello-from-csharp**.

Zastąp kod w **MyClass.cs** pliku następującym fragmentem kodu:

```csharp
using UIKit;
public class MyUIView : UITextView
{
    public MyUIView ()
    {
        Text = "Hello from C#";
    }
}
```

Skompiluj projekt, a wynikowy zestaw zostanie zapisany jako **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**.

### <a name="bind-the-managed-assembly"></a>Powiązania zestawu zarządzanego

Po utworzeniu zestawu zarządzanego, powiązać go za pomocą wywołania osadzanie kodu .NET.

Zgodnie z opisem w [instalacji](~/tools/dotnet-embedding/get-started/install/install.md) przewodnika, można to zrobić jako krok po kompilacji w projekcie z niestandardowy cel programu MSBuild lub ręcznie:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Struktura zostaną umieszczone w **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Użyj wygenerowanych danych wyjściowych w projekcie Xcode

Otwórz program Xcode, tworzenie nowych dla systemu iOS aplikacja pojedynczego widoku, nadaj mu nazwę **hello z csharp**i wybierz **języka Objective-C** języka.

Otwórz **~/Projects/hello-from-csharp/output** katalogu w programie Finder, wybierz **hello z csharp.framework**, przeciągnij go do projektu Xcode i upuść go tuż nad **hello z csharp**  folderu w projekcie.

![Przeciąganie i upuszczanie z framework](ios-images/hello-from-csharp-ios-drag-drop-framework.png)

Upewnij się, że **skopiuj elementy w razie potrzeby** zostanie zaznaczona w oknie dialogowym, które się pojawi, a następnie kliknij przycisk **Zakończ**.

![Skopiuj elementy w razie potrzeby](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Wybierz **hello z csharp** projektu, a następnie przejdź do **hello z csharp** obiektu docelowego **na karcie Ogólne**. W **osadzone binarne** Dodaj **hello z csharp.framework**.

![Osadzone pliki binarne](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Otwórz **ViewController.m**i zastąp jego zawartość, za pomocą:

```objective-c
#import "ViewController.h"
#include "hello-from-csharp/hello-from-csharp.h"

@interface ViewController ()
@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];

    MyUIView *view = [[MyUIView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}
@end
```

Osadzanie kodu .NET nie obsługuje obecnie kodu bitowego w systemach iOS, która jest włączona dla niektórych szablonów projektu Xcode. 

Wyłącz go w ustawieniach projektu:

![Opcja Bitcode](../../images/ios-bitcode-option.png)

Na koniec Uruchom projekt Xcode i będzie wyświetlane mniej więcej tak:

![Pozdrowienia z działających w symulatorze przykładowy języka C#](ios-images/hello-from-csharp-ios.png)
