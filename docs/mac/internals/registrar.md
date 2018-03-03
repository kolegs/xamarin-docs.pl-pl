---
title: Xamarin.Mac registrar
description: "W tym dokumencie opisano celem rejestratora Xamarin.Mac i jego użycia różnych konfiguracji."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7CAAA6B7-D654-4AD3-BAEC-9DD01210978A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 8d5061850a4cb73a81e1bf4c93583eb5b6eeefec
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinmac-registrar"></a>Xamarin.Mac registrar

_W tym dokumencie opisano celem rejestratora Xamarin.Mac i jego użycia różnych konfiguracji._

## <a name="overview"></a>Omówienie

Xamarin.Mac stanowi pomost między świecie zarządzany (.NET) i środowiska uruchomieniowego w Cocoa, umożliwiając klasy zarządzane wywołać klasy niezarządzane Objective-C i można wywołać z powrotem po wystąpieniu zdarzenia. Pracy wymaganej w celu przeprowadzić to "Magia" jest obsługiwany przez rejestratora i ogólnie rzecz biorąc, ukrytych z widoku.

Brak wpływu na wydajność tej rejestracji, w szczególności na czas, uruchamiania aplikacji i opis bitowa, co się dzieje "kulisy", czasami mogą być pomocne.

## <a name="configurations"></a>Konfiguracje

Zasadniczo zadania rejestratora przy uruchamianiu można podzielić na dwie kategorie:

- Skanowanie każdej zarządzanej klasy dla wynikających z NSObject i zebrać listę elementów, które mają być widoczne dla środowiska uruchomieniowego języka Objective-C.
- Zarejestruj te informacje o środowisku uruchomieniowym języka Objective-C.

Wraz z upływem czasu trzech konfiguracji różnych rejestratora zostały utworzone, aby pokrywał innych przypadków użycia. Każdy ma inną kompilację i uruchom czasu konsekwencje:

- **Dynamiczne rejestratora** — podczas uruchamiania przy użyciu odbicia .NET Sprawdź listę odpowiednich elementów, co typ załadować skanowania, a informuje natywnym środowisku uruchomieniowym. Ta opcja dodaje bez przełączania do trybu kompilacji, ale jest bardzo kosztowna do obliczenia podczas uruchamiania (do kilku sekund).
- **Statyczne rejestratora** — podczas kompilacji, obliczeniowe zestaw elementów, które mają być rejestrowane i generowania kodu języka Objective-C do obsługi rejestracji. Ten kod jest wywoływana podczas uruchamiania, aby szybko zarejestrować wszystkie elementy. Dodaje znaczących Wstrzymaj kompilacji, ale może Wytnij znaczną ilość czasu od początku aplikacji.
- **"Częściowej" static** — nowszej metody "hybrydowe", która oferuje większość zalety obu. Ponieważ eksportów z **Xamarin.Mac.dll** są stałe, Zapisz wstępnie obliczonych biblioteki obsługi ich rejestracji, a połączenie w. Użyj odbicia, aby obsługiwać biblioteki użytkownika, ale jako biblioteki użytkownika Eksportuj znacznie mniej typów powiązania platformy się to często zamiast szybkiego. Neglectable wpływ czas kompilacji i zmniejsza większość "koszt" dynamiczny.

Obecnie statyczne z częściowa jest ustawieniem domyślnym dla konfiguracji debugowania i statyczna jest ustawieniem domyślnym dla wersji konfiguracji.

Istnieje kilka scenariuszy:

- Dodatki plug-in załadowany po uruchamiania klasy wywodzące się z NSObject
- Dynamicznie utworzyć wystąpienia klasy wywodzące się z NSObject

gdy rejestratora nie wiadomo, że musi zarejestrować pewien typ podczas uruchamiania. `ObjCRuntime.Runtime.RegisterAssembly` Metody są dostarczane do rejestratora informuje, że aplikacja ma dodatkowe typy wziąć pod uwagę.
