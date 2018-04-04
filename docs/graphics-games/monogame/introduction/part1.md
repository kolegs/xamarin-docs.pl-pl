---
title: Część 1 — Tworzenie MonoGame Międzyplatformowego
description: W tym przewodniku przedstawiono sposób tworzenia nowego projektu dla systemu iOS i Android przy użyciu MonoGame. Wynik jest programu Visual Studio for Mac rozwiązania z projektem i platform udostępnionego kodu, a także jednego projektu dla każdej platformy. Ten projekt wyświetla pusty ekran niebieski podczas wykonywania.
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 0cd12f23f8cb269b2a41a08bf641db08e18fb82b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>Część 1 — Tworzenie MonoGame Międzyplatformowego

_W tym przewodniku przedstawiono sposób tworzenia nowego projektu dla systemu iOS i Android przy użyciu MonoGame. Wynik jest programu Visual Studio for Mac rozwiązania z projektem i platform udostępnionego kodu, a także jednego projektu dla każdej platformy. Ten projekt wyświetla pusty ekran niebieski podczas wykonywania._

MonoGame umożliwia projektowanie gry dla wielu platform z dużą część ponowne użycie kodu. Ten przewodnik koncentruje się na temat konfigurowania rozwiązania zawierającego projekty dla systemu iOS i Android, jak również projektu udostępnionego kodu dla kodu i platform.

Gdy skończymy, możemy zostanie projekt, który ma struktura do wykonywania logiki aktualizacji gier oraz gier rysowania logiki 30 klatek na sekundę. Może służyć jako podstawowa projektu dla każdego projektu MonoGame. Nasze projektu będzie wyglądać następująco podczas wykonywania:

![](part1-images/image1.png "Projekt będzie wyglądać następująco podczas wykonywania")


# <a name="adding-monogame-to-visual-studio-for-mac"></a>Dodawanie MonoGame do programu Visual Studio dla komputerów Mac

MonoGame może zostać dodana jako dodatek do programu Visual Studio dla komputerów Mac. Dla komputerów Mac, wybierz **programu Visual Studio for Mac** > **Menedżera dodatków...**  . W systemie Windows, wybierz ** Narzędzia ** > **Menedżera dodatków...**  . Wybierz **galerii** karcie, rozwiń **programowanie gry** kategorii i wybierz **MonoGame Addin**, następnie kliknij przycisk **zainstalować**:

![](part1-images/image2.png "Wybierz kartę Galeria, rozwiń kategorię gry programowanie i wybierz MonoGame dodatek, a następnie kliknij przycisk Instaluj")

> [!IMPORTANT]
> **Uwaga**: Jeśli **programowanie gry** sekcji nie są wyświetlane w Menedżerze dodatku, możesz ręcznie pobrać i zainstalować najnowszą wersję, w tym miejscu: http://www.monogame.net/downloads/. Może być konieczne ponowne uruchomienie programu Visual Studio for Mac dla szablonów i pojawienie się.



Po zakończeniu instalacji szablony MonoGame pojawi się w programie Visual Studio dla komputerów Mac, ponieważ zostanie wyświetlone w następnej sekcji.


# <a name="creating-a-new-solution"></a>Tworzenie nowego rozwiązania

W programie Visual Studio dla komputerów Mac, wybierz opcję **Plik > nowe rozwiązanie**. W **nowy projekt** okna dialogowego, kliknij przycisk **różne**, przewiń do **ogólne** zaznacz ** aplikacji uniwersalnej MonoGame Mobile ** opcję i kliknij przycisk Dalej.

![](part1-images/image3.png "W oknie dialogowym Nowy Projekt kliknij na różne, przewiń do sekcji Ogólne wybierz opcję aplikacji uniwersalnej MonoGame Mobile i kliknij przycisk Dalej")

Nazwij projekt WalkingGame, a następnie kliknij przycisk Utwórz:

![](part1-images/image4.png "Nazwij projekt WalkingGame i kliknij przycisk Utwórz")

Teraz naszych projektu zostanie wykonany, podobnie jak inne systemu iOS lub Android projektu. Projekt powinno być ono uruchomione Wyświetlanie tła cornflower niebieski:

![](part1-images/image5.png "Projekt powinno być ono uruchomione Wyświetlanie tła cornflower niebieski")


# <a name="fixing-android-compile-errors"></a>Naprawianie błędów kompilacji systemu Android

Bieżąca wersja szablonów firmy MonoGame obejmuje kilka błędów składni w systemie Android `Activity1.cs` pliku. Aby rozwiązać te problemy, należy zastąpić `OnCreate` funkcji z następujących czynności:


```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```


# <a name="summary"></a>Podsumowanie

W tym przewodniku objętych sposobu tworzenia projektu MonoGame wielu platform przy użyciu programu Visual Studio dla komputerów Mac. Wynik jest pusty ekran niebieski. Ten projekt może służyć jako punkt początkowy dla każdego z systemem iOS i gier dla systemu Android.

## <a name="related-links"></a>Linki pokrewne

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [Dokumentacja interfejsu API MonoGame](http://www.monogame.net/documentation/?page=main)
