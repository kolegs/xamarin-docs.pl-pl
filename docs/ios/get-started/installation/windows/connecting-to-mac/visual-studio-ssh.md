---
title: Gdzie jest Mój Mac?
description: Instrukcje dotyczące konfigurowania komputerów Mac dla programu Visual Studio do tworzenia projektów platformy Xamarin.iOS
ms.prod: xamarin
ms.assetid: 4f792690-b3b1-4d56-a5e2-363b4f246059
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: f3e2988c9700549db24ad69277df9e412c99caed
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2018
---
# <a name="wheres-my-mac"></a>Gdzie jest Mój Mac?

_Instrukcje dotyczące konfigurowania komputerów Mac dla programu Visual Studio do tworzenia projektów platformy Xamarin.iOS_

Bieżący **stabilna** wersji programu Xamarin dla Visual Studio współdziała z komputera Mac inaczej w poprzednich wersjach[^](#earlier-versions).

Aby użyć Mac jako Xamarin Mac Agent, należy włączyć **logowania zdalnego** opartym na systemie

1. Otwórz *Spotlight* (`Cmd-Space`) i wyszukaj **logowania zdalnego** , a następnie wybierz opcję **udostępniania** wynik. Spowoduje to otwarcie **preferencjach systemowych** w **udostępniania** panelu.

  ![](visual-studio-ssh-images/spotlight.png "Zwracanie przez wyszukiwanie Spotlight do logowania zdalnego")

2. Znaczników **logowania zdalnego** opcji **usługi** liście z lewej strony, aby umożliwić Xamarin dla Visual Studio, aby nawiązać połączenie komputerów Mac.

  ![](visual-studio-ssh-images/sharing.png "Opcja logowania zdalnego na liście usług znaczników")

3. Upewnij się, że **logowania zdalnego** jest ustawiona, aby umożliwić dostęp do **wszyscy użytkownicy**, lub że Mac nazwa użytkownika lub grupy znajduje się na liście dozwolone użytkowników na liście po prawej stronie.

Jeśli znajduje się w tej samej sieci komputera Mac powinno być teraz wykrywalny przez program Visual Studio.
Gdy Visual Studio próbuje połączyć się z komputerem Mac, zostanie wyświetlony monit logowania z programu Visual Studio przy użyciu poświadczeń użytkownika Mac.

## <a name="where-can-i-find-more-information"></a>Gdzie można znaleźć więcej informacji?

Istnieje wiele przewodniki pomaga skonfigurować i zrozumieć nowy agent, są wymienione poniżej:

- [Łączenie z komputerów Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [Rozwiązywanie problemów](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin University - Xamarin Mac Agent](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)

<a name="earlier-versions" />

## <a name="migrating-from-previous-versions"></a>Migrowanie z poprzednich wersji

Użycie wcześniejszych wersji platformy Xamarin dla Visual Studio będzie można zapoznać się z **hosta kompilacji Xamarin.iOS** aplikacji, który wymagany do uruchomienia na komputerze Mac, a następnie *sparowanego* za pomocą numeru PIN z sieci Wystąpienie programu Visual Studio.

Kompilacja aplikacji hosta nie jest już wymagana w tej wersji programu Xamarin dla Visual Studio.
