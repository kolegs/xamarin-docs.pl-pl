---
title: Rozpoczynanie pracy z systemem iOS
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 34afdd9e91ebfbe7ad57c7eec6ba7f05fff1a2aa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-ios"></a>Rozpoczynanie pracy z systemem iOS


## <a name="requirements"></a>Wymagania

Oprócz wymagań dotyczących z naszych [wprowadzenie do języka Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) przewodniku, należy również:

* [Xamarin.iOS 10.11](https://www.visualstudio.com/xamarin/) lub nowszy

## <a name="hello-world"></a>Cześć ludzie

Pierwszy utworzymy przykład prostego Witaj świecie w języku C#.

### <a name="create-c-sample"></a>Tworzenie przykładowej C#

Otwórz program Visual Studio dla komputerów Mac, Utwórz nowy projekt biblioteki klas z systemem iOS, nadaj jej nazwę `hello-from-csharp`i zapisać go do `~/Projects/hello-from-csharp`.

Zastąp kod w `MyClass.cs` pliku następującym fragmentem kodu:

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

Skompiluj projekt, wynikowy zestaw zostanie zapisany jako `~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll`.

### <a name="bind-the-managed-assembly"></a>Powiązanie zestawu zarządzanego

Uruchom embeddinator pozwala utworzyć strukturę macierzystego dla zestawu zarządzanego:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Platformę zostaną umieszczone w `~/Projects/hello-from-csharp/output/hello-from-csharp.framework`.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Użyj wygenerowanych danych wyjściowych w projekcie Xcode

Otwórz środowisko Xcode i utworzyć nowe pojedynczego widoku aplikacji systemu iOS, nadaj jej nazwę `hello-from-csharp` i wybierz **Objective-C** języka.

Otwórz `~/Projects/hello-from-csharp/output` katalogu w wyszukiwanie, wybierz `hello-from-csharp.framework`, przeciągnij je do projektu Xcode i upuść ją po prostu powyżej `hello-from-csharp` folderu w projekcie.

! [Przeciągnij i upuść framework] Images/Hello-from-CSharp-IOS-Drag-DROP-Framework.png)

Upewnij się, że `Copy items if needed` zostanie zaznaczona w oknie dialogowym, które pojawia się, a następnie kliknij przycisk `Finish`.

![Kopiuje elementy w razie potrzeby](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Wybierz `hello-from-csharp` projektu i przejdź do `hello-from-csharp` docelowego **karta Ogólne**. W **binarnych osadzonego** Dodaj `hello-from-csharp.framework`.

![Osadzone pliki binarne](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Otwórz ViewController.m i Zastąp zawartość z:

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

Na koniec Uruchom projekt Xcode i zostaną wyświetlone informacje podobne:

![Witaj z działających w symulatorze próbki C#](ios-images/hello-from-csharp-ios.png)
