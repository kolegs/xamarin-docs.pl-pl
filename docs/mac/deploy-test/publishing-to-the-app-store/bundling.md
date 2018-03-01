---
title: "Tworzenie pakietów dla komputerów Mac App Store"
description: "Ten przewodnik przeprowadzi Cię przez funkcję tworzenia pakietów aplikacji Xamarin.Mac dla publikacji do sklepu Mac App Store."
ms.topic: article
ms.prod: xamarin
ms.assetid: 00a36d7c-937d-4657-bf6a-0de9684b8f94
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: e138fc1176c646a2e4e9caf94462028dd7c68e9f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="bundle-for-mac-app-store"></a>Pakiet dla sklepu Mac App Store

W tej sekcji opisano podstawowe informacje dotyczące tworzenia aplikacji dla wersji w sklepie z aplikacjami Mac za pomocą programu Visual Studio dla komputerów Mac. W oparciu o dodatkowe funkcje (takie jak iCloud dostępu i powiadomieniami wypychanymi), dodatkowe ustawienia mogą być wymagane skojarzoną beyon zakres tego artykułu.

> [!NOTE]
>  **Uwaga**: przed uruchomieniem tej sekcji, deweloper należy utworzyć produkcji profil dla Mac App Store inicjowania obsługi administracyjnej. Zapoznaj się z instrukcjami we wcześniejszej części tego dokumentu o tworzeniu wymagane profile inicjowania obsługi administracyjnej.

## <a name="code-signing-options"></a>Opcje podpisywania kodu

Zmień **konfiguracji** do **wersji** przed zaktualizowaniem podpisywania kodu i opcje tworzenia pakietów. Deweloper musi się upewnić, że używają one firma **tożsamości** i profil inicjowania obsługi administracyjnej, które zostały utworzone powyżej podczas podpisywania aplikacji w wersji w sklepie z aplikacjami.

 [![Edytowanie kodu opcje podpisywania](bundling-images/config02.png "edycji opcje podpisywania kodu")](bundling-images/config02-large.png)

Upewnij się, zaewidencjonowany opcję, aby utworzyć pakiet Instalatora **kompilacji Mac** ustawienia:

[![Opcje kompilacji edycji](bundling-images/config03.png "edycji opcje kompilacji")](bundling-images/config03-large.png)

## <a name="build"></a>Kompilacja

Przed rozpoczęciem budowania, upewnij się, że **wersji** konfiguracji została wybrana. Gdy dewelopera tworzy aplikację, monitowani będzie korzystać z obu certyfikatów:

 ![Możliwość aplikacji przy użyciu certyfikatu](bundling-images/image62.png "możliwość aplikacji przy użyciu certyfikatu")

 ![Możliwość aplikacji przy użyciu certyfikatu](bundling-images/image63.png "możliwość aplikacji przy użyciu certyfikatu")

Po Po skompilowaniu aplikacji deweloper może kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Otwórz Folder zawierający** można znaleźć pliku pakietu (w `bin/x86/AppStore` katalogu w przykładzie przedstawionym poniżej).  Ten plik zawiera Instalatora aplikacji, który może zostać przesłane do firmy Apple do włączenia Mac App Store.

 ![Wybieranie pakietu kompilacji w wyszukiwarce](bundling-images/image64.png "wybranie pakietu kompilacji w wyszukiwania")


## <a name="related-links"></a>Linki pokrewne

- [Instalacja](/visualstudio/mac/installation/)
- [Witaj, przykładowe Mac](~/mac/get-started/hello-mac.md)
- [Dystrybucję aplikacji ze sklepu Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Identyfikator deweloperów i strażnika](https://developer.apple.com/resources/developer-id/)
