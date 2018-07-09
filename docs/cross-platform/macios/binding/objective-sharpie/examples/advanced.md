---
title: Zaawansowane (ręczne) przykład rzeczywistych
description: W tym dokumencie opisano sposób użyć danych wyjściowych xcodebuild jako dane wejściowe Objective Sharpie, co zapewnia wgląd w jak Objective Sharpie działa pod maską.
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: c4f7f1e9702fb2ee0f5525343a52e3aacd85d68c
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855263"
---
# <a name="advanced-manual-real-world-example"></a>Zaawansowane (ręczne) przykład rzeczywistych

**W tym przykładzie użyto [POP biblioteki z usługi Facebook](https://github.com/facebook/pop).**

W tej sekcji omówiono bardziej zaawansowane podejście do powiązania, których będziemy używać firmy Apple `xcodebuild` narzędzia, aby najpierw skompilować projekt POP i ręcznie wywnioskować dane wejściowe dla Objective Sharpie. Obejmuje to zasadniczo Objective Sharpie robi kulisy w poprzedniej sekcji.

```
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Ponieważ biblioteki POP projektu Xcode (`pop.xcodeproj`), możemy użyć `xcodebuild` do tworzenia punktu obecności. Ten proces z kolei może generować pliki nagłówkowe, które Objective Sharpie może być konieczne przeanalizować. Jest to, dlaczego tworzenie przed powiązanie jest ważne. Podczas kompilowania za pomocą `xcodebuild` upewnij się, możesz przekazać ten sam identyfikator zestawu SDK i architektury, których zamierzasz przekazać do Objective Sharpie (i należy pamiętać, że Objective Sharpie 3.0 można to zrobić zwykle!):

```
$ xcodebuild -sdk iphoneos9.0 -arch arm64

Build settings from command line:
    ARCHS = arm64
    SDKROOT = iphoneos8.1
 
=== BUILD TARGET pop OF PROJECT pop WITH THE DEFAULT CONFIGURATION (Release) ===
 
...
 
CpHeader pop/POPAnimationTracer.h build/Headers/POP/POPAnimationTracer.h
    cd /Users/aaron/src/sharpie/pop
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/Users/aaron/bin::/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/usr/local/git/bin:/Users/aaron/.rvm/bin"
    builtin-copy -exclude .DS_Store -exclude CVS -exclude .svn -exclude .git -exclude .hg -strip-debug-symbols -strip-tool /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/strip -resolve-src-symlinks /Users/aaron/src/sharpie/pop/pop/POPAnimationTracer.h /Users/aaron/src/sharpie/pop/build/Headers/POP
 
...
 
** BUILD SUCCEEDED **
```

Będzie istniało wiele kompilację informacji wyjściowych w konsoli jako część `xcodebuild`. Wyświetlane powyżej, możemy zobaczyć uruchomienia elementu docelowego "CpHeader" polegającego nagłówka pliki zostały skopiowane do katalogu wyjściowego kompilacji. To sytuacja często dotyczy i ułatwia powiązania: jako część kompilacji natywnej biblioteki, pliki nagłówkowe często są kopiowane do "publiczne" w użyciu lokalizacji, które ułatwia analizowanie łatwiejsze dla wiązania. W tym przypadku wiemy, że pliki nagłówkowe POP firmy znajdują się w `build/Headers` katalogu.

Jesteśmy teraz powiązać punktu obecności. Wiemy, że chcemy kompilacji dla zestawu SDK `iphoneos8.1` z `arm64` architektury i pliki nagłówkowe Dbamy o znajdują się w `build/Headers` w obszarze wyewidencjonowania git POP. Jeśli spojrzymy `build/Headers` katalogu, zobaczymy, wiele plików nagłówkowych:

```
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

Jeśli przyjrzymy się `POP.h`, możemy zobaczyć jest głównym nagłówek najwyższego poziomu biblioteki pliku, który `#import`s inne pliki. W związku z tym potrzebujemy tylko do przekazywania `POP.h` do Objective Sharpie, oraz clang rest w tle:

```
$ sharpie bind -output Binding -sdk iphoneos8.1 \
    -scope build/Headers build/Headers/POP/POP.h \
    -c -Ibuild/Headers -arch arm64

Parsing Native Code...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Binding Analysis:
  Automated binding is complete, but there are a few APIs which have
  been flagged with [Verify] attributes. While the entire binding
  should be audited for best API design practices, look more closely
  at APIs with the following Verify attribute hints:

  ConstantsInterfaceAssociation (1 instance):
    There's no fool-proof way to determine with which Objective-C
    interface an extern variable declaration may be associated.
    Instances of these are bound as [Field] properties in a partial
    interface into a near-by concrete interface to produce a more
    intuitive API, possibly eliminating the 'Constants' interface
    altogether.

  StronglyTypedNSArray (4 instances):
    A native NSArray* was bound as NSObject[]. It might be possible
    to more strongly type the array in the binding based on
    expectations set through API documentation (e.g. comments in the
    header file) or by examining the array contents through testing.
    For example, an NSArray* containing only NSNumber* instances can
    be bound as NSNumber[] instead of NSObject[].

  MethodToProperty (2 instances):
    An Objective-C method was bound as a C# property due to
    convention such as taking no parameters and returning a value (
    non-void return). Often methods like these should be bound as
    properties to surface a nicer API, but sometimes false-positives
    can occur and the binding should actually be a method.

  Once you have verified a Verify attribute, you should remove it
  from the binding source code. The presence of Verify attributes
  intentionally cause build failures.

  For more information about the Verify attribute hints above,
  consult the Objective Sharpie documentation by running 'sharpie
  docs' or visiting the following URL:

    http://xmn.io/sharpie-docs

Submitting usage data to Xamarin...
  Submitted - thank you for helping to improve Objective Sharpie!

Done.
```

Zauważysz, firma Microsoft przekazywane `-scope build/Headers` argument Objective Sharpie. Ponieważ biblioteki C i języka Objective-C, należy `#import` lub `#include` inne pliki nagłówkowe, znajdujących się na implementacji szczegóły biblioteki i nie API chcesz powiązać, `-scope` argument nakazuje Objective Sharpie ignorowanie dowolnego interfejsu API, który nie jest zdefiniowany w Plik gdzieś w ramach `-scope` katalogu.

Znajdziesz `-scope` argument jest często opcjonalne klarownie wdrożonych bibliotek, jednak nie przynosi żadnych szkód w sposób jawny dostarczaniu go.

Ponadto określonej `-c -Ibuild/headers`. Po pierwsze `-c` argument nakazuje Objective Sharpie w celu zatrzymania interpretowanie argumenty wiersza polecenia, a następnie przekazuje wszystkie pozostałe argumenty _bezpośrednio do kompilatora clang_. W związku z tym `-Ibuild/Headers` jest argument kompilatora clang, który powoduje, że clang, aby wyszukać obejmuje `build/Headers`, czyli, gdzie nagłówki POP na żywo. Bez tego argumentu, clang nie wiadomo, gdzie umieścić pliki które `POP.h` jest `#import`ing. _Prawie wszystkie "problemy z" przy użyciu Objective Sharpie zagotować w dół w celu ustalenie, co do przekazania do clang_.

### <a name="completing-the-binding"></a>Korzystanie z powiązania

Narzędzie Objective Sharpie teraz został wygenerowany `Binding/ApiDefinitions.cs` i `Binding/StructsAndEnums.cs` plików.

Są to podstawowe pierwszym przebiegu Objective Sharpie na powiązanie, a w niektórych przypadkach może być wszystko, czego potrzebujesz. Jak już wspomniano jednak deweloper będzie zazwyczaj muszą ręcznie zmodyfikować wygenerowanych plików po zakończeniu Objective Sharpie, aby [Rozwiąż wszelkie problemy](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) , może nie być automatycznie obsługiwane przez narzędzie.

Po zakończeniu aktualizacji tych plików można teraz dodać do powiązania projektu w programie Visual Studio dla komputerów Mac lub być przekazywana bezpośrednio do `btouch` lub `bmac` narzędzi, aby wygenerować ostateczny powiązania.

Dokładne opis proces wiązania, zobacz nasze [instrukcje szczegółowy przewodnik](~/ios/platform/binding-objective-c/walkthrough.md).

## <a name="related-links"></a>Linki pokrewne

- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)