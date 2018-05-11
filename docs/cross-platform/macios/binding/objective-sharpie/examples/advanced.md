---
title: Zaawansowane przykład rzeczywistych (ręczna)
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 82bca525433e5c8fea3a29250afb83962f2e64fc
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="advanced-manual-real-world-example"></a>Zaawansowane przykład rzeczywistych (ręczna)


**W tym przykładzie użyto [biblioteki POP z usługi Facebook](https://github.com/facebook/pop).**


W tej sekcji omówiono bardziej zaawansowanych podejście do wiązania, w której będzie korzystać firmy Apple `xcodebuild` narzędzia, aby najpierw skompilować projekt POP i ręcznie wywnioskować dane wejściowe dla Sharpie cel. Obejmuje to zasadniczo Sharpie cel czynności kulisy w poprzedniej sekcji.

```csharp
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Ponieważ biblioteka POP ma projektu Xcode (`pop.xcodeproj`), możemy użyć `xcodebuild` do tworzenia protokołu POP. Ten proces z kolei może generować pliki nagłówkowe, które Sharpie celem może być konieczne analizy. Jest to, dlaczego tworzenie przed ważne jest powiązanie. Podczas kompilowania za pośrednictwem `xcodebuild` upewnij się, zostanie przekazany do tego samego identyfikatora zestawu SDK i architektury który chcesz przekazać do Sharpie cel (i należy pamiętać, że cel Sharpie 3.0 można to zrobić zwykle!):

```csharp
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

Będzie dużo danych wyjściowych informacji o kompilacji w konsoli jako część `xcodebuild`. Wyświetlane powyżej, możemy stwierdzić, czy element docelowy "CpHeader" zostało uruchomione którym pliki nagłówkowe zostały skopiowane do budowy katalogu wyjściowego. Jest to często wielkość liter i ułatwia powiązania: jako część kompilacji natywnej biblioteki, pliki nagłówkowe, często są kopiowane do "publiczne" eksploatacyjny lokalizacji co może uniemożliwić analizowania łatwiej dla powiązania. W takim przypadku wiemy, że pliki nagłówkowe POP firmy znajdują się w `build/Headers` katalogu.

Firma Microsoft są teraz gotowe do powiązania protokołu POP. Wiemy, że chęć kompilacji dla zestawu SDK `iphoneos8.1` z `arm64` architektury i pliki nagłówkowe Szanujemy znajdują się w `build/Headers` w obszarze wyewidencjonowania git POP. Jeśli szukamy `build/Headers` zajmiemy się liczba pliki nagłówkowe katalogu:

```csharp
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

Jeśli przyjrzymy się `POP.h`, zobaczysz jest plik głównego nagłówka najwyższego poziomu biblioteki `#import`s inne pliki. W związku z tym tylko należy przekazać `POP.h` do Sharpie cel i clang będzie wykonywać rest w tle:

```csharp
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

Można zauważyć, możemy przekazany `-scope build/Headers` argument Sharpie cel. Ponieważ bibliotek C i Objective-C musi `#import` lub `#include` inne pliki nagłówków, które są szczegóły implementacji biblioteki i nie API chcesz powiązać, `-scope` argument nakazuje Sharpie cel, aby zignorować jakiegokolwiek interfejsu API, który nie jest zdefiniowany w Plik gdzieś w `-scope` katalogu.

Można znaleźć `-scope` argument jest często opcjonalne bezpośrednio zaimplementowanym bibliotek, jednak nie powoduje żadnych problemów jawnie dzięki udostępnianiu.

Ponadto firma Microsoft określony `-c -Ibuild/headers`. Po pierwsze `-c` argument nakazuje Sharpie cel, aby zatrzymać interpretowanie argumenty wiersza polecenia i Przekaż wszystkie pozostałe argumenty _bezpośrednio do kompilatora clang_. W związku z tym `-Ibuild/Headers` tym clang argument kompilatora, która sprawia, że clang do wyszukiwania w obszarze `build/Headers`, czyli miejsca zamieszkania nagłówków protokołu POP. Bez tego argumentu clang nie wiedział, gdzie umieścić pliki który `POP.h` jest `#import`lasycznego, dokującego. _Prawie wszystkie "problemów" przy użyciu Sharpie cel gotować w dół do oceniania, co należy przekazać do clang_.

### <a name="completing-the-binding"></a>Kończenie pracy powiązania

Teraz wygenerowała celu Sharpie `Binding/ApiDefinitions.cs` i `Binding/StructsAndEnums.cs` plików.

Są to Sharpie cel podstawowe pierwszego przejścia na powiązanie, a w niektórych przypadkach może być wymagany. Jak już wspomniano jednak dewelopera zwykle należy ręcznie zmodyfikować wygenerowanego po zakończeniu Sharpie cel do [Rozwiąż wszelkie problemy](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) którego nie można automatycznie obsłużyć przez narzędzie.

Po zakończeniu aktualizacji tych plików można teraz dodać do powiązania projektu programu Visual Studio dla komputerów Mac lub bezpośrednio do przekazania `btouch` lub `bmac` narzędzi do tworzenia końcowego powiązania.

Dokładny opis proces wiązania, zobacz nasze [instrukcje pełny przewodnik](~/ios/platform/binding-objective-c/walkthrough.md).

