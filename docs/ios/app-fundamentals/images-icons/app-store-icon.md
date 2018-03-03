---
title: Ikona sklepu z aplikacjami
description: "Ten artykuł obejmuje tym i zarządzanie zasób obrazu w aplikacji platformy Xamarin.iOS ma być używany jako ikonę aplikacji sklepu."
ms.topic: article
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/26/2017
ms.openlocfilehash: 6ec4328f08859c5331a6250bf44ee705a7fd0744
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="app-store-icon"></a>Ikona sklepu z aplikacjami

_Ten artykuł obejmuje tym i zarządzanie zasób obrazu w aplikacji platformy Xamarin.iOS ma być używany jako ikonę aplikacji sklepu._

Przed Xcode 9 wszystkie ikony sklepu z aplikacjami zostały dodane za pomocą programu iTunes Connect. Jednak nie jest to już wielkość liter. Ikony sklepu z aplikacjami teraz musi być dołączone jako część pakietu z projektu i dodać w katalogu zasobów. Aplikacje, które nie zawierają ikona Sklepu z aplikacjami zostaną odrzucone przez firmę Apple.

Ikona magazynu aplikacji jest kroju aplikację do użytkowników, więc należy ją do zapamiętania i wyświetlanie dobrze przy mały rozmiar. Ikony do zapamiętania są czyste, proste i natychmiast rozpoznawalnych.

Apple sugeruje poniższe wskazówki podczas projektowania jako ikony aplikacji:

- Ikona odpowiedni dla twojej aplikacji.
- Tworzenie prostego ikonę, która jest zgodna z projektu aplikacji.
- Unikaj używania słowa w Twojej ikony.
- Wziąć pod uwagę globalnie: ikona jednej aplikacji jest używany w wszystkie obszary magazynu.

Obraz pikseli 1024 x 1024 jest wymagany dla ikony aplikacji, który będzie wyświetlany w sklepie z aplikacjami.  Apple ma już wspomniano, czy ikona magazynu aplikacji w katalogu zasobów nie może być przezroczysty, ani zawierać kanału alfa.

Aby uzyskać więcej informacji, zobacz firmy Apple [iOS Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/).

## <a name="adding-an-app-store-icon"></a>Dodawanie ikony sklepu z aplikacjami

Ikony sklepu z aplikacjami powinny teraz być dostarczona przez katalogu zasobów. 

Aby dodać ikony sklepu z aplikacjami, wykonaj następujące czynności:

1. Zlokalizuj **AppIcon** obraz ustawiony **Assets.xcassets** pliku projektu. 
    - Wszystkie nowe projekty powinna pochodzić z **Assets.xcassets** plik zawierający zestaw AppIcon obrazu.
    - Aby dodać nowy katalog zasobów, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj > Nowy plik > katalogu zasobów**.
    - Aby dodać nowy zestaw obrazu ikony aplikacji, kliknij prawym przyciskiem myszy w obszarze zestaw ikonę i wybierz **ikony aplikacji i uruchamianie obrazów > Nowa ikona aplikacji**:
    
    ![Dodaj nową opcję zestaw obrazu](app-store-icon-images/image1.png)

2. Przewiń do **sklepu z aplikacjami** ikonę na liście:

    ![Ikona sklepu z aplikacjami](app-store-icon-images/image2.png)

3. Kliknij ikonę, a następnie przejdź do 1024 x 1024 piksel obrazu. Zapisz katalogu zasobów.




## <a name="related-links"></a>Linki pokrewne

- [Zarządzanie ikon z katalogami zasobów](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
