---
title: Przy użyciu natywnych bibliotek
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 0fa66f3a16047c18af19cb7257c778b498bc0c9b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774814"
---
# <a name="using-native-libraries"></a>Przy użyciu natywnych bibliotek

Xamarin.Android obsługuje natywnych bibliotek za pomocą standardowego mechanizmu PInvoke. Można również pakietu dodatkowych bibliotek natywnego, które nie są częścią systemu operacyjnego do programu apk.

Aby wdrożyć natywnej biblioteki z aplikacji platformy Xamarin.Android, Dodaj bibliotekę binarnego do projektu i ustawić jej **Akcja kompilacji** do **AndroidNativeLibrary**.

Aby wdrożyć natywnej biblioteki z projektu platformy Xamarin.Android biblioteki, Dodaj bibliotekę binarnego do projektu i ustaw jej **Akcja kompilacji** do **EmbeddedNativeLibrary**.

Należy pamiętać, że od systemu Android obsługuje wiele interfejsów binarne aplikacji (ABIs), platformy Xamarin.Android musi znać które ABI natywnej biblioteki zaprojektowano pod kątem.
Istnieją dwa sposoby, które można to zrobić:

1.  Ścieżka "wykrywania"
1.  Za pomocą `AndroidNativeLibrary/Abi` elementu w pliku projektu


Z wykrywanie ścieżka, nazwa katalogu nadrzędnego natywnej biblioteki służy do określania ABI który cele biblioteki. W związku z tym jeśli dodasz `lib/armeabi/libfoo.so` do projektu, następnie ABI będzie można "ten sposób" jako `armeabi`.

Alternatywnie można edytować pliku projektu, aby jawnie określić ABI do użycia:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Aby uzyskać więcej informacji o używaniu natywnych bibliotek, zobacz [współdziałanie z natywnych bibliotek](http://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio-2015"></a>Debugowanie kodu natywnego z programem Visual Studio 2015

Jeśli używasz *programu Visual Studio 2015*, nie trzeba zmodyfikować plików projektu (jak opisano powyżej).
Możesz skompilować i debugowanie C++ wewnątrz rozwiązania Xamarin.Android, po prostu dodając odwołanie projektu do C++ **dynamicznej biblioteki udostępnione (Android)** projektu.

Visual Studio C++ deweloperzy mogą zobaczyć [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) przykładowe próby debugowanie C++ z programu Visual Studio 2015 z platformą Xamarin; i zapoznaj się z naszym [wpis w blogu](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) Aby uzyskać więcej informacji.



## <a name="related-links"></a>Linki pokrewne

- [SanAngeles_NativeDebug (przykład)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
