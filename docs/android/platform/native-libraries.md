---
title: Używanie bibliotek natywnych
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 9175996f516a980d915d1501b4b18ea23ec86cef
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353583"
---
# <a name="using-native-libraries"></a>Używanie bibliotek natywnych

Platforma Xamarin.Android obsługuje korzystanie z bibliotek natywnych przy użyciu standardowego mechanizmu PInvoke. Można również powiązać dodatkowe natywnych bibliotek, które nie są częścią systemu operacyjnego do Twojego pliku apk.

Aby wdrożyć natywnej biblioteki za pomocą aplikacji platformy Xamarin.Android, Dodaj biblioteki binarne do projektu i ustaw jego **Build Action** do **AndroidNativeLibrary**.

Aby wdrożyć natywnej biblioteki w projekcie biblioteki platformy Xamarin.Android, Dodaj biblioteki binarne do projektu i ustaw jego **Build Action** do **EmbeddedNativeLibrary**.

Należy pamiętać, ponieważ system Android obsługuje wiele interfejsów binarnym aplikacji (interfejsów ABI), platformy Xamarin.Android musisz wiedzieć które ABI bibliotekę natywną zaprojektowano pod kątem.
Istnieją dwa sposoby, które można to zrobić:

1.  Ścieżka "wykrywania"
1.  Za pomocą `AndroidNativeLibrary/Abi` elementu w pliku projektu


Przy użyciu ścieżki wykrywanie, nazwę katalogu nadrzędnego bibliotekę natywną służy do określania interfejsu ABI, elementy docelowe biblioteki. W związku z tym jeśli dodasz `lib/armeabi/libfoo.so` do projektu, następnie ABI będzie mieć "ten" jako `armeabi`.

Alternatywnie można edytować plik projektu, aby jawnie określić ABI do użycia:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Aby uzyskać więcej informacji na temat korzystania z natywnych bibliotek, zobacz [współdziałanie z natywnych bibliotek](http://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio-2017"></a>Debugowanie kodu natywnego za pomocą programu Visual Studio 2017

Jeśli używasz *programu Visual Studio 2017* lub większą, nie trzeba modyfikować plików projektu, zgodnie z powyższym opisem.
Tworzenie i debugowanie można przeprowadzać C++ wewnątrz rozwiązania Xamarin.Android, dodając odwołanie projektu do C++ **dynamiczna Biblioteka udostępniona (Android)** projektu. 

Debugowanie kodu natywnego języka C++ w projekcie, wykonaj następujące kroki:

1. Kliknij dwukrotnie projektu **właściwości** i wybierz **opcje systemu Android** strony.
2. Przewiń w dół do **opcje debugowania**.
3. W **debugera** menu rozwijanego wybierz opcję **C++** (zamiast domyślnego **.Net (Xamarin)**).

Visual Studio C++ deweloperzy mogą zobaczyć [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) przykładowy próby debugowania języka C++ w programie Visual Studio 2017 z rozwiązaniem Xamarin; i odnoszą się do naszych [wpis w blogu](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) Aby uzyskać więcej informacji.



## <a name="related-links"></a>Linki pokrewne

- [SanAngeles_NativeDebug (przykład)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
- [Tworzenie natywnych aplikacji platformy Xamarin Android](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
