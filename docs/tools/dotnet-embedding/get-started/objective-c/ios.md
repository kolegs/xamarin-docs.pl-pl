---
title: Rozpoczynanie pracy z systemem iOS
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: ab841c461356bd435db0c68e82c5e30d398d806a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="getting-started-with-ios"></a>Rozpoczynanie pracy z systemem iOS

## <a name="requirements"></a>Wymagania

Oprócz wymagań dotyczących z naszych [wprowadzenie do języka Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) przewodniku, należy również:

* [Xamarin.iOS 10.11](https://www.visualstudio.com/xamarin/) lub nowszy

## <a name="hello-world"></a>Cześć ludzie

Najpierw należy utworzyć przykład prostego Witaj świecie w języku C#.

### <a name="create-c-sample"></a>Tworzenie przykładowej C#

Otwórz program Visual Studio dla komputerów Mac, Utwórz nowy projekt biblioteki klas z systemem iOS, nadaj jej nazwę **hello z csharp**i zapisać go do **~/Projects/hello-from-csharp**.

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

### <a name="bind-the-managed-assembly"></a>Powiązanie zestawu zarządzanego

Po utworzeniu zestawu zarządzanego powiązać ją, wywołując osadzanie .NET.

Zgodnie z opisem w [instalacji](~/tools/dotnet-embedding/get-started/install/install.md) przewodnika, można to zrobić w kroku mające miejsce po kompilacji w projekcie, niestandardowe docelowy programu MSBuild lub ręcznie:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Platformę zostaną umieszczone w **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Użyj wygenerowanych danych wyjściowych w projekcie Xcode

Otwórz środowisko Xcode, tworzenie nowych pojedynczego widoku aplikacji systemu iOS, nadaj jej nazwę **hello z csharp**i wybierz **Objective-C** języka.

Otwórz **~/Projects/hello-from-csharp/output** katalogu w wyszukiwanie, wybierz **hello z csharp.framework**, przeciągnij je do projektu Xcode i upuść ją po prostu powyżej **hello z języka csharp**  folderu w projekcie.

! [Przeciągnij i upuść framework] Images/Hello-from-CSharp-IOS-Drag-DROP-Framework.png)

Upewnij się, że **skopiować elementy w razie potrzeby** zostanie zaznaczona w oknie dialogowym, które pojawia się, a następnie kliknij przycisk **Zakończ**.

![Kopiuje elementy w razie potrzeby](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Wybierz **hello z csharp** projektu i przejdź do **hello z csharp** docelowego **karta Ogólne**. W **binarnych osadzonego** Dodaj **hello z csharp.framework**.

![Osadzone pliki binarne](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Otwórz **ViewController.m**i Zastąp zawartość z:

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

Osadzanie .NET nie obsługuje obecnie kodu bitowego w systemach iOS, w którym włączono obsługę niektóre szablony projektu Xcode. 

Wyłącz go w ustawieniach projektu:

![Opcja kodu bitowego](../../images/ios-bitcode-option.png)

Na koniec Uruchom projekt Xcode i zostaną wyświetlone informacje podobne:

![Witaj z działających w symulatorze próbki C#](ios-images/hello-from-csharp-ios.png)
