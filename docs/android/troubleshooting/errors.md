---
title: Xamarin.Android Errors Matrix
ms.topic: article
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: d113e7747f656eda2bedb88574f4b2027616add8
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin.Android Errors Matrix

## <a name="errors-reference"></a>Błędy odwołania

Ten dokument zawiera informacje na różnych kodów błędów z Xamarin.

|Kategoria|Opis|
|--- |--- |
|XA0xxx|mandroid błędów|
|XA1xxx|Skopiowanie pliku / łączy symbolicznych (projekt powiązane) błędów|
|XA2xxx|Błędy konsolidatora|
|XA3xxx|Błędy drzewa obiektów aplikacji|
|XA4xxx|Błędy generowania kodu|
|XA5xxx|GCC i łańcuch narzędzi błędów|
|XA6xxx|błędy wewnętrzne narzędzia mandroid|
|XA7xxx|Zastrzeżone|
|XA8xxx|Zastrzeżone|
|XA9xxx|Błędy licencjonowania|


## <a name="error-codes"></a>Kody błędów

### <a name="xa0xxx-errors"></a>Błędy XA0xxx

|Kod błędu|Opis|
|--- |--- |
|XA0000|Nieoczekiwany błąd - wypełnij [usterek — raport](http://bugzilla.xamarin.com).|
|XA0001|"-devname został podany bez żadnych działań specyficzne dla urządzenia.|
|XA0002|Nie można przeanalizować zmiennej środowiskowej "{0}".|
|XA0003|Aplikacji "{0} .exe" występuje konflikt nazw z nazwą zestawu (.dll) zestawu SDK lub produkt.|
|XA0004|Nowe logiki refcounting wymaga sgen zbyt włączenia.|
|XA0005|Katalog wyjściowy "{0}" nie istnieje.|
|XA0006|W "{0}" nie ma żadnych platform devel, użyj opcji--platformy = PLAT do określenia zestawu SDK|
|XA0007|Zestawu głównego "{0}" nie istnieje.|
|XA0008|Należy podać tylko jednego zestawu głównego.|
|XA0009|Wystąpił błąd podczas ładowania zestawów: {0}.|
|XA0010|Nie można przeanalizować argumenty wiersza polecenia: {0}.|
|XA0011|{0} został skompilowany przy nowsza środowiska uruchomieniowego ({1}) nie obsługuje MonoTouch.|
|XA0012|Niekompletne dane są dostarczane do wykonania "{0}".|
|XA0013|Profilowanie obsługi wymaga sgen zbyt włączenia tej funkcji.|
|XA0014|iOS {0} nie obsługuje tworzenie aplikacji przeznaczonych dla ARMv6.|
|XA0020|Nie można ustalić ścieżki mandroid.|
|XA0100|EmbeddedNativeLibrary "{0}" jest nieprawidłowy w projekcie aplikacji systemu Android. Zamiast tego użyj AndroidNativeLibrary.|

### <a name="xa1xxx-errors"></a>Błędy XA1xxx

|Kod błędu|Opis|
|--- |--- |
|XA1001|Nie można odnaleźć aplikacji w określonym katalogu.|
|XA1002|Nie można utworzyć łączy symbolicznych, pliki zostały skopiowane.|
|XA1003|Nie można skasować aplikacji "{0}". Może być konieczne ręczne kill aplikacji.|
|XA1004|Nie można pobrać listy zainstalowanych aplikacji.|
|XA1005|Nie można zakończyć aplikacji "{0}" na urządzeniu "{1}": {2}. Może być konieczne ręczne kill aplikacji.|
|XA1006|Nie można zainstalować aplikacji "{0}" na urządzeniu "{1}": {2}.|
|XA1007|Nie można uruchomić aplikacji "{0}" na urządzeniu "{1}": {2}. Aplikację można uruchomić ręcznie, naciskając na nim.|
|XA1008|Nie można uruchomić symulatora: {0}.|
|XA1009|Nie można skopiować zestawu "{0}" do "{1}": {2}.|
|XA1010|Nie można załadować zestawu "{0}": {1}.|
|XA1011|Nie można dodać brakujący plik zasobu: {0}.|
|XA1101|Nie można uruchomić aplikacji.|
|XA1102|Nie można dołączyć do aplikacji (w celu zakończenia jego): {0}.|
|XA1103|Nie można odłączyć.|
|XA1104|Nie można wysłać pakietu: {0}.|
|XA1105|Typ nieoczekiwaną odpowiedź.|
|XA1106|Nie można pobrać listy aplikacji na urządzeniu: Upłynął limit czasu żądania.|
|XA1107|Nie można uruchomić aplikacji.|
|XA1201|Nie można załadować symulator: {0}.|
|XA1301|Natywnej biblioteki ({1}) "{0}" została zignorowana, ponieważ nie pasuje do bieżącego architecture(s) kompilacji ({2}).|

### <a name="xa2xxx-errors"></a>Błędy XA2xxx

|Kod błędu|Opis|
|--- |--- |
|XA2001|Nie można połączyć zestawów.|
|XA2002|Nie można rozpoznać odwołania: {0}.|
|XA2003|Opcja "{0}" zostanie zignorowana, ponieważ połączenie jest wyłączona.|
|XA2004|Nie można zlokalizować pliku definicji dodatkowe konsolidatora "{0}".|
|XA2005|Nie można przeanalizować definicji "{0}".|
|XA2006|Nie można rozpoznać odwołania do metadanych elementu "{0}" (zdefiniowane w "{1}") z "{2}".|

### <a name="xa3xxx-errors"></a>Błędy XA3xxx

Są to błędy drzewa obiektów aplikacji.

|Kod błędu|Opis|
|--- |--- |
|XA3001|Można drzewa nie obiektów aplikacji zestawu "{0}".|
|XA3002|Ograniczenie drzewa obiektów aplikacji: metoda "{0}" musi być statyczna, ponieważ ma ona [MonoPInvokeCallback].|
|XA3003|Konflikt--opcje debugowania i--llvm. Debugowanie soft jest wyłączone.|


### <a name="xa4xxx-errors"></a>Błędy XA4xxx

Są to błędy generowania kodu.

|Kod błędu|Opis|
|--- |--- |
|XA4001|Nie można expansed do {0} głównym szablonu.|
|XA4101|Rejestrator nie można utworzyć podpisu dla typu "{0}".|
|XA4102|Rejestratora znaleziono nieprawidłowy typ "{0}" w podpisie metody "{2}". Zamiast tego użyj "{1}".|
|XA4103|Rejestratora znaleziono nieprawidłowy typ "{0}" w podpisie metody "{2}": typ implementuje INativeObject, ale nie ma konstruktora, który przyjmuje dwa (IntPtr, wartość logiczna) argumentów.|
|XA4104|Rejestrator nie można zorganizować wartość zwracana dla typu "{0}" w podpisie metody "{1}".|
|XA4105|Rejestrator nie można zorganizować parametr typu "{0}" w podpisie metody "{1}".|
|XA4106|Rejestrator nie można zorganizować wartość zwracaną dla struktury "{0}" w podpisie metody "{1}".|
|XA4107|Rejestrator nie można zorganizować parametr typu "{0}" w podpisie metody "{1}".|
|XA4108|Rejestrator nie można pobrać typu ObjectiveC dla zarządzanego typu "{0}".|
|XA4109|Nie można skompilować kodu wygenerowanego rejestratora. Zgłoś [usterek — raport](http://bugzilla.xamarin.com).|
|XA4110|Rejestrator nie można zorganizować parametr wyjściowy typu "{0}" w podpisie metody "{1}".|
|XA4111|Rejestrator nie można utworzyć podpisu dla typu "{0}" w metodzie "{1}".|
|XA4200|W Aktywnej można generować tylko dla typów "claas".|
|XA4201|Nie można ustalić nazwy JNI typu {0}.|
|XA4203|Nazwa określonego typu musi być w pełni kwalifikowana.|
|XA4204|Nie można rozpoznać typu interfejsu "{0}". Czy nie brakuje odwołania do zestawu?|
|XA4205|[ExportField] można używać tylko w metodach o 0 parametrach.|
|XA4206|[Eksportu] nie można używać w typie podstawowym.|
|XA4207|[ExportField] nie można używać w typie podstawowym.|
|XA4208|[Java.Interop.ExportFieldAttribute] nie można użyć metody zwraca typ void.|
|XA4209|Nie można utworzyć klasy JavaTypeInfo: {0} z powodu {1}.|
|XA4210|Musisz dodać odwołanie do Mono.Android.Export.dll, korzystając z ExportAttribute lub ExportFieldAttribute.|
|XA4211|AndroidManifest.xml //uses-sdk/@android:targetSdkVersion "{0}" jest mniejsza niż $(TargetFrameworkVersion) "{1}". Za pomocą interfejsu API-\ {1\} Aktywnej kompilacji.|


### <a name="xa5xxx-errors"></a>Błędy XA5xxx

|Kod błędu|Opis|
|--- |--- |
|XA5101|Brak {0} kompilatora. Zainstaluj zestaw Android NDK.|
|XA5102|Konwersja z zestawu do kodu macierzystego nie powiodła się. Zgłoś [usterek — raport](http://bugzilla.xamarin.com).|
|XA5103|Nie można skompilować pliku "{0}". Zgłoś [usterek — raport](http://bugzilla.xamarin.com).|
|XA5201|Łączenie natywnego nie powiodło się. Zapoznaj się z tematem flag użytkownika do gcc: {0}|
|XA5202|Łączenie natywnego nie powiodło się. Przejrzyj dziennik kompilacji.|
|XA5303|Łączenie ostrzeżenie Native: {0}.|
|XA5300|Zestaw SDK systemu android nie znaleziono lub nie są w pełni zainstalowany.|
|XA5301|Brak narzędzia "Usuń". Zainstaluj program Xcode składnik "Narzędzia wiersza polecenia".|
|XA5302|Brak narzędzie "dsymutil". Zainstaluj program Xcode składnik "Narzędzia wiersza polecenia".|
|XA5203|Nie można wygenerować symboli debugowania (dSYM katalogu). Przejrzyj dziennik kompilacji.|
|XA5204|Nie powiodło się paska końcowego pliku binarnego. Przejrzyj dziennik kompilacji.|
|XA5205|Brak narzędzie "aapt". Zainstaluj pakiet narzędzi kompilacji zestawu SDK systemu Android.|
|XA5206|{0}. {1} katalogu zasobów systemu android nie istnieje.|
|XA5207|{0}. {1} pliku biblioteki języka Java nie istnieje.|
|XA5208|Pobieranie nie powiodło się. Pobierz {0} i umieszcza je w katalogu {1}.|
|XA5209|Nie można rozpakować. Pobierz {0}, a następnie wyodrębnij go do katalogu {1}.|
|XA5210|{0}. Natywnej biblioteki {1} plik nie istnieje.|

### <a name="xa6xxx-errors"></a>Błędy XA6xxx

|Kod błędu|Opis|
|--- |--- |
|XA6001|Uruchomiona wersja Cecil nie obsługuje usuwanie zestawu.|
|XA6002|Nie można usunąć zestawu "{0}".|
|XA6003|Unauthorizedaccessexception — komunikat.|

### <a name="xa9xxx-errors"></a>Błędy XA9xxx

Te kody błędów są błędy licencjonowania i aktywacji.


#### <a name="xa9000"></a>XA9000

 **Przyczyna:** licencja wygasła

 **Sprawdzane podczas:** kompilacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|OSTRZEŻENIE|OSTRZEŻENIE|OSTRZEŻENIE|OSTRZEŻENIE|OSTRZEŻENIE|


#### <a name="xa9001"></a>XA9001

 **Przyczyna:** wersji próbnej wygasła

 **Sprawdzane podczas:** kompilacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|OSTRZEŻENIE|OSTRZEŻENIE|OSTRZEŻENIE|OSTRZEŻENIE|OSTRZEŻENIE|



#### <a name="xa9002"></a>XA9002

 **Przyczyna:** AndroidJavaSource

 **Sprawdzane podczas:** kompilacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|OK|OK|OK|OK|

 **Przyczyna:** AndroidJavaLibrary

 **Sprawdzane podczas:** kompilacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|OK|OK|OK|OK|

 **Jeśli w zestawie powiązania ma JAR osadzone, następnie to zostanie przechwycony w czasie pakietu, a nie czas kompilacji.**

 **Przyczyna:** AndroidNativeLibrary

 **Sprawdzane podczas:** kompilacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|OK|OK|OK|OK|


#### <a name="xa9003"></a>XA9003

 **Cause:** System.Runtime.Serialization

 **Sprawdzane podczas:** pakietu

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|OK|OK|OK|

 **Cause:** System.ServiceModel.Web

 **Sprawdzane podczas:** pakietu

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|OK|OK|OK|

 **Przyczyna:** Mono.Data.Tds

 **Sprawdzane podczas:** kompilacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|OK|OK|OK|

 **Odwołuje System.Data.dll, który jest dozwolony**

 **Przyczyna:** Mono.Android.Export

 **Sprawdzane podczas:** kompilacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|OK|OK|OK|OK|



#### <a name="xa9004"></a>XA9004

 **Przyczyna:** --profilowania

 **Sprawdzane podczas:** pakietu

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|OK|OK|OK|



#### <a name="xa9005"></a>XA9005

 **Przyczyna:** limit rozmiaru (32 kb).

 **Sprawdzane podczas:** pakietu

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|OK|-|-|-|



#### <a name="xa9006"></a>XA9006

 **Cause:** System.Data.SqlClient namespace.

 **Sprawdzane podczas:** pakietu

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|OK|OK|OK|


#### <a name="xa9008"></a>XA9008

 **Przyczyna:** tworzenie z wiersza polecenia.

 **Sprawdzane podczas:** kompilacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|OK|OK|OK|


#### <a name="xa9009"></a>XA9009

 **Przyczyna:** Brak numeru seryjnego.

 **Sprawdzane podczas:** Untestable

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### <a name="xa9010"></a>XA9010

 **Przyczyna:** ProductId nieprawidłowy.

 **Sprawdzane podczas:** kompilacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|

Wartość równoważna XA9018.



#### <a name="xa9011"></a>XA9011

 **Przyczyna:** nie może zaktualizować pliku licencji (do nowego formatu pliku).

 **Sprawdzane podczas:** aktywacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|

#### <a name="xa9012"></a>XA9012

 **Przyczyna:** bez dostępu do Internetu

 **Sprawdzane podczas:** aktywacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### <a name="xa9013"></a>XA9013

 **Przyczyna:** nieznany błąd

 **Sprawdzane podczas:** aktywacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### <a name="xa9014"></a>XA9014

 **Przyczyna:** kod aktywacji nieprawidłowy

 **Sprawdzane podczas:** aktywacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### <a name="xa9017"></a>XA9017

 **Przyczyna:** Serwer aktywacji nie zwraca prawidłowej licencji.

 **Sprawdzane podczas:** aktywacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### <a name="xa9018"></a>XA9018

**Przyczyna:** nieprawidłową licencją

**Sprawdzane podczas:** kompilacji

|Początkowe|Indie|Business(trial)|Biznesowa|Enterprise|
|--- |--- |--- |--- |--- |
|-|-|ERROR|-|-|

