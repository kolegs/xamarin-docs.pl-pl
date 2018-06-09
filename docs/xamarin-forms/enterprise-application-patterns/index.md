---
title: Wzorce aplikacji przedsiębiorstwa przy użyciu książki elektronicznej platformy Xamarin.Forms
description: Ten książki elektronicznej zawiera architektury wskazówki dotyczące tworzenia dostosowywalne, łatwy w obsłudze i testować aplikacje przedsiębiorstwa platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: c401465d8a57abe1d5a1cfaf9ee2616888332ea3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242166"
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Wzorce aplikacji przedsiębiorstwa przy użyciu książki elektronicznej platformy Xamarin.Forms

_Architektury wskazówki związane z opracowywaniem dostosowywalne, łatwy w obsłudze i testować aplikacje przedsiębiorstwa platformy Xamarin.Forms_

![](images/cover-sml.png "Wzorce aplikacji przedsiębiorstwa przy użyciu książki elektronicznej platformy Xamarin.Forms")

Ta książki elektronicznej znajdują się wskazówki dotyczące implementowania wzorca Model-View-ViewModel (MVVM), iniekcji zależności, nawigacji, weryfikacji i zarządzania konfiguracją, przy zachowaniu luźne powiązanie. Ponadto istnieje również wskazówki dotyczące przeprowadzania uwierzytelniania i autoryzacji z IdentityServer, dostęp do danych z konteneryzowanych mikrousług i testowania jednostek.

## <a name="prefaceprefacemd"></a>[Wstęp](preface.md)

W tym rozdziale opisano przeznaczenie i zakres przewodnika i który ma na celu.

## <a name="introductionintroductionmd"></a>[Wprowadzenie](introduction.md)

Deweloperzy aplikacji w przedsiębiorstwie stają przed kilka wyzwania, które można zmienić architektury aplikacji podczas tworzenia. W związku z tym należy do tworzenia aplikacji, tak aby można modyfikować i rozszerzony w czasie. Projektowanie dla takich zdolności adaptacyjnych może być trudne, ale zazwyczaj polega to na partycje aplikacji do odrębny, luźno powiązanych składników, które można łatwo integrować razem w aplikacji.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

Wzorca Model-View-ViewModel (MVVM) ułatwia prawidłowo oddzielnych działalności biznesowej i prezentacji logiki aplikacji za pomocą jego interfejsu użytkownika (UI). Obsługa czyste rozdzielenie aplikacji logiki i interfejsu użytkownika pomaga rozwiązywać problemy programowanie wiele i może ułatwić testowanie aplikacji, obsługa i rozwijać. Może znacznie zwiększyć możliwości ponownego użycia kodu i umożliwia deweloperom i projektantom interfejsu użytkownika więcej łatwiej współpracować przy opracowywaniu ich odpowiednich części aplikacji.

## <a name="dependency-injectiondependency-injectionmd"></a>[Wstrzykiwanie zależności](dependency-injection.md)

Iniekcji zależności umożliwia rozdzielenie typów konkretnych z kodu, która jest zależna od tych typów. Zwykle wykorzystuje kontener, który zawiera listę rejestracji i mapowania między interfejsami i typy abstrakcyjne i konkretne typy, które implementuje lub rozszerzyć tych typów.

Kontenery iniekcji zależności zmniejszyć sprzężenia między obiektami, zapewniając możliwość wystąpienia wystąpień klas i zarządzanie nimi życia na podstawie konfiguracji kontenera. Podczas tworzenia obiektów kontenera injects wszelkie zależności, które wymaga obiektu do niego. Jeśli te zależności nie zostały jeszcze utworzone, tworzy i jest rozpoznawany jako ich zależności kontenera.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[Komunikacja między luźno sprzężonymi składnikami](communicating-between-loosely-coupled-components.md)

Platformy Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) klasa implementuje publikowanie-subskrypcji wzorzec, co pozwala na podstawie komunikatu komunikacji między składnikami, które są niewygodne przez odwołania do obiektu i typu. Mechanizm ten umożliwia wydawcy i subskrybentów, do komunikowania się bez odwołania do siebie, aby zmniejszyć zależności między składnikami, umożliwiając również składniki niezależnie opracowany i przetestowany.

## <a name="navigationnavigationmd"></a>[Nawigacja](navigation.md)

Platformy Xamarin.Forms obsługuje nawigacji strony, co powoduje zwykle z interakcji użytkownika przy użyciu interfejsu użytkownika lub aplikacji, w wyniku zmian stanu wewnętrznego logiki driven. Jednak nawigacji może być skomplikowane do zaimplementowania w aplikacji korzystających ze wzorca MVVM.

Przedstawia informacje o tym rozdziale `NavigationService` klasy, która jest używana do wykonywania nawigacji pierwszego modelu widoku z widoku modeli. Wprowadzenie do logiki nawigacji w widoku klasy modelu oznacza, że logika może być wykonywane za pomocą testów automatycznych. Ponadto model widoku następnie można wdrożyć logikę do sterowania nawigacji, aby upewnić się, że niektóre reguły biznesowe są wymuszane.

## <a name="validationvalidationmd"></a>[Walidacja](validation.md)

Dowolną aplikację, która akceptuje dane wejściowe od użytkowników powinny upewnij się, że dane wejściowe są poprawne. Bez sprawdzania poprawności użytkownik może podać dane, które powoduje awarię aplikacji. Sprawdzanie poprawności wymusza stosowanie reguł biznesowych i uniemożliwia osobie atakującej iniekcję szkodliwe dane.

W kontekście modelu ViewModel modelu (MVVM) wzorca model widoku lub model często będzie wymagane do wykonywania sprawdzania poprawności danych i sygnalizuje wszystkie błędy weryfikacji do widoku, dzięki czemu użytkownik może je poprawić.

## <a name="configuration-managementconfiguration-managementmd"></a>[Zarządzanie konfiguracją](configuration-management.md)

Ustawienia umożliwiają oddzielenie danych, który konfiguruje zachowanie aplikacji z kodu, umożliwiając zachowanie, które będzie można zmienić bez ponownie skompilować aplikację. Ustawienia aplikacji są dane, które aplikacja tworzy i którymi zarządza oraz ustawienia użytkownika są dostosowywane ustawienia aplikacji, które wpływają na zachowanie aplikacji i nie wymagają częstego ponownego dostosowania.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[Konteneryzowane mikrousługi](containerized-microservices.md)

Mikrousług oferują podejście do tworzenia aplikacji i wdrażania, który jest odpowiedni do wymagań elastyczności, skalę i niezawodność nowoczesnych aplikacji w chmurze. Jedną z wielu zalet mikrousług jest, aby mogły być skalowalnych w poziomie niezależnie, co oznacza, że określonym obszarze funkcjonalności mogą być skalowane która wymaga więcej przetwarzania zasilania lub sieci przepustowość do obsługi żądanie, bez niepotrzebnie skalowania obszarów Aplikacja, która nie występuje żądanie zwiększenia.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[Uwierzytelnianie i autoryzacja](authentication-and-authorization.md)

Istnieje wiele sposobów integracji aplikacji platformy Xamarin.Forms, który komunikuje się z aplikacji sieci web platformy ASP.NET MVC uwierzytelniania i autoryzacji. W tym miejscu są wykonywane z mikrousługi konteneryzowanych tożsamości, która używa IdentityServer 4 uwierzytelniania i autoryzacji. IdentityServer to struktura OpenID Connect i OAuth 2.0 typu open source dla platformy ASP.NET Core, która integruje się z ASP.NET Identity Core do uwierzytelniania tokenu elementu nośnego.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[Uzyskiwanie dostępu do danych zdalnych](accessing-remote-data.md)

Wiele nowoczesnych rozwiązań opartych na sieci web wprowadzić korzystanie z usług sieci web, znajdujących się na serwerach sieci web, umożliwiają korzystanie z funkcji zdalnego klienta aplikacji. Operacje, które udostępnia usługi sieci web stanowi interfejs API sieci web i aplikacje klienckie powinno być możliwe korzystanie z interfejsu API sieci web bez znajomości sposobu implementacji danych lub operacji, które udostępnia interfejs API.

## <a name="unit-testingunit-testingmd"></a>[Testowanie jednostek](unit-testing.md)

Testowanie modeli i wyświetlanie modeli z modelem MVVM aplikacji są identyczne z testowaniem innych klas, a następnie można użyć tego samego narzędzi i technik. Istnieją pewne wzorce, które są typowe dla modelu i klasy modelu widoku, które mogą korzystać z techniki testowania określonej jednostki.

## <a name="feedback"></a>Opinia

Ten projekt zawiera witryna społeczności, na którym można Opublikuj pytania i wyrazić swoją opinię. Witryna społeczności znajduje się na [GitHub](https://github.com/dotnet-architecture/eShopOnContainers). Alternatywnie swoją opinię na temat księgi elektronicznej może otrzymywać pocztą e-mail powiadomienia do [ dotnet-architecture-ebooks-feedback@service.microsoft.com ](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com).


## <a name="related-links"></a>Linki pokrewne

- [Pobieranie książki elektronicznej (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (przykład)](https://github.com/dotnet-architecture/eShopOnContainers)
