---
title: Xamarin.Android Errors Matrix
ms.topic: article
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c3aab44fcb0522b762aaa101de0aeba08016b641
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin.Android Errors Matrix

## <a name="errors-reference"></a>Błędy odwołania

Ten dokument zawiera informacje na różnych kodów błędów z Xamarin.

<table>
    <thead>
        <tr>
            <td>Kategoria</td>
            <td>Opis</td>
        </tr>
    </thead>
    <tbody>
        <tr><td>XA0xxx</td><td><code>mandroid</code> Błędy</td></tr>
        <tr><td>XA1xxx</td><td>Skopiowanie pliku / łączy symbolicznych (projekt powiązane) błędów</td></tr>
        <tr><td>XA2xxx</td><td>Błędy konsolidatora</td></tr>
        <tr><td>XA3xxx</td><td>Błędy drzewa obiektów aplikacji</td></tr>
        <tr><td>XA4xxx</td><td>Błędy generowania kodu</td></tr>
        <tr><td>XA5xxx</td><td>GCC i łańcuch narzędzi błędów</td></tr>
        <tr><td>XA6xxx</td><td><code>mandroid</code> Wewnętrzne błędy narzędzi</td></tr>
        <tr><td>XA7xxx</td><td>Zastrzeżone</td></tr>
        <tr><td>XA8xxx</td><td>Zastrzeżone</td></tr>
        <tr><td>XA9xxx</td><td>Błędy licencjonowania</td></tr>
    </tbody>
</table>

## <a name="error-codes"></a>Kody błędów

### <a name="xa0xxx-errors"></a>Błędy XA0xxx

<table>
    <thead>
        <tr><td>Kod błędu</td><td>Opis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA0000</td><td>Nieoczekiwany błąd - wypełnij raport o usterce w <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA0001</td><td>"-devname został podany bez żadnych działań specyficzne dla urządzenia</td></tr>
        <tr><td>XA0002</td><td>Nie można przeanalizować zmiennej środowiskowej "{0}".</td></tr>
        <tr><td>XA0003</td><td>Aplikacji "{0} .exe" występuje konflikt nazw z nazwą zestawu (.dll) zestawu SDK lub produkt.</td></tr>
        <tr><td>XA0004</td><td>Nowe logiki refcounting wymaga sgen zbyt włączenia.</td></tr>
        <tr><td>XA0005</td><td>Katalog wyjściowy "{0}" nie istnieje.</td></tr>
        <tr><td>XA0006</td><td>W "{0}" nie ma żadnych platform devel, użyj opcji--platformy = PLAT do określenia zestawu SDK</td></tr>
        <tr><td>XA0007</td><td>Zestawu głównego "{0}" nie istnieje.</td></tr>
        <tr><td>XA0008</td><td>Należy podać tylko jeden zestaw głównego</td></tr>
        <tr><td>XA0009</td><td>Wystąpił błąd podczas ładowania zestawów: {0}</td></tr>
        <tr><td>XA0010</td><td>Nie można przeanalizować argumenty wiersza polecenia: {0}</td></tr>
        <tr><td>XA0011</td><td>{0} został skompilowany przy nowsza środowiska uruchomieniowego ({1}) nie obsługuje MonoTouch</td></tr>
        <tr><td>XA0012</td><td>Niekompletne dane są dostarczane do ukończenia `{0}`.</td></tr>
        <tr><td>XA0013</td><td>Profilowanie obsługi wymaga sgen zbyt włączenia tej funkcji</td></tr>
        <tr><td>XA0014</td><td>iOS {0} nie obsługuje tworzenie aplikacji przeznaczonych dla ARMv6</td></tr>
        <tr><td>XA0020</td><td>Nie można ustalić ścieżki mandroid.</td></tr>
        <tr><td>XA0100</td><td>EmbeddedNativeLibrary "{0}" jest nieprawidłowy w projekcie aplikacji systemu Android. Zamiast tego użyj AndroidNativeLibrary.</td></tr>
    </tbody>
</table>

### <a name="xa1xxx-errors"></a>Błędy XA1xxx

<table>
    <thead>
        <tr><td>Kod błędu</td><td>Opis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA1001</td><td>Nie można odnaleźć aplikacji w określonym katalogu</td></tr>
        <tr><td>XA1002</td><td>Nie można utworzyć łączy symbolicznych, pliki zostały skopiowane</td></tr>
        <tr><td>XA1003</td><td>Nie można skasować aplikacji "{0}". Może być konieczne ręczne kill aplikacji.</td></tr>
        <tr><td>XA1004</td><td>Nie można pobrać listy zainstalowanych aplikacji.</td></tr>
        <tr><td>XA1005</td><td>Nie można zakończyć aplikacji "{0}" na urządzeniu "{1}": {2}. Może być konieczne ręczne kill aplikacji.</td></tr>
        <tr><td>XA1006</td><td>Nie można zainstalować aplikacji "{0}" na urządzeniu "{1}": {2}.</td></tr>
        <tr><td>XA1007</td><td>Nie można uruchomić aplikacji "{0}" na urządzeniu "{1}": {2}. Aplikację można uruchomić ręcznie, naciskając na nim.</td></tr>
        <tr><td>XA1008</td><td>Nie można uruchomić symulatora: {0}</td></tr>
        <tr><td>XA1009</td><td>Nie można skopiować zestawu "{0}" do "{1}": {2}</td></tr>
        <tr><td>XA1010</td><td>Nie można załadować zestawu "{0}": {1}</td></tr>
        <tr><td>XA1011</td><td>Nie można dodać brakujący plik zasobu: {0}</td></tr>
        <tr><td>XA1101</td><td>Nie można uruchomić aplikacji</td></tr>
        <tr><td>XA1102</td><td>Nie można dołączyć do aplikacji (w celu zakończenia jego): {0}</td></tr>
        <tr><td>XA1103</td><td>Nie można odłączyć</td></tr>
        <tr><td>XA1104</td><td>Nie można wysłać pakietu: {0}</td></tr>
        <tr><td>XA1105</td><td>Nieoczekiwana odpowiedź typu</td></tr>
        <tr><td>XA1106</td><td>Nie można pobrać listy aplikacji na urządzeniu: Upłynął limit czasu żądania.</td></tr>
        <tr><td>XA1107</td><td>Nie można uruchomić aplikacji</td></tr>
        <tr><td>XA1201</td><td>Nie można załadować symulator: {0}</td></tr>
        <tr><td>XA1301</td><td>Natywnej biblioteki `{0}` ({1}) została zignorowana, ponieważ nie pasuje do bieżącego architecture(s) kompilacji ({2})</td></tr>
    </tbody>
</table>

### <a name="xa2xxx-errors"></a>Błędy XA2xxx

<table>
    <thead>
        <tr><td>Kod błędu</td><td>Opis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA2001</td><td>Nie można połączyć zestawów</td></tr>
        <tr><td>XA2002</td><td>Nie można rozpoznać odwołania: {0}</td></tr>
        <tr><td>XA2003</td><td>Opcja "{0}" zostanie zignorowana, ponieważ połączenie jest wyłączona</td></tr>
        <tr><td>XA2004</td><td>Nie można zlokalizować pliku definicji dodatkowe konsolidatora "{0}".</td></tr>
        <tr><td>XA2005</td><td>Nie można przeanalizować definicji "{0}".</td></tr>
        <tr><td>XA2006</td><td>Nie można rozpoznać odwołania do metadanych elementu "{0}" (zdefiniowane w "{1}") z "{2}".</td></tr>
    </tbody>
</table>

### <a name="xa3xxx-errors"></a>Błędy XA3xxx

Są to błędy drzewa obiektów aplikacji.

<table>
    <thead>
        <tr><td>Kod błędu</td><td>Opis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA3001</td><td>Można drzewa nie obiektów aplikacji zestawu {0}</td></tr>
        <tr><td>XA3002</td><td>Ograniczenie drzewa obiektów aplikacji: metoda "{0}" musi być statyczna, ponieważ ma ona [MonoPInvokeCallback].</td></tr>
        <tr><td>XA3003</td><td>Konflikt--opcje debugowania i--llvm. Debugowanie soft jest wyłączone.</td></tr>
    </tbody>
</table>

### <a name="xa4xxx-errors"></a>Błędy XA4xxx

Są to błędy generowania kodu.

<table>
    <thead>
        <tr><td>Kod błędu</td><td>Opis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA4001</td><td>Nie można expansed do szablonu głównego `{0}`.</td></tr>
        <tr><td>XA4101</td><td>Rejestrator nie może utworzyć podpis dla typu `{0}`.</td></tr>
        <tr><td>XA4102</td><td>Rejestratora znaleziono nieprawidłowy typ `{0}` w podpisie metody `{2}`. Zamiast nich należy używać słów kluczowych `{1}`.</td></tr>
        <tr><td>XA4103</td><td>Rejestratora znaleziono nieprawidłowy typ `{0}` w podpisie metody `{2}`: typ implementuje INativeObject, ale nie ma konstruktora, który przyjmuje dwa (IntPtr, wartość logiczna) argumentów</td></tr>
        <tr><td>XA4104</td><td>Rejestratora nie można zorganizować typu wartość zwracana `{0}` w podpisie metody `{1}`.</td></tr>
        <tr><td>XA4105</td><td>Rejestrator nie można zorganizować parametr typu `{0}` w podpisie metody `{1}`.</td></tr>
        <tr><td>XA4106</td><td>Rejestrator nie można zorganizować wartość zwracaną dla struktury `{0}` w podpisie metody `{1}`.</td></tr>
        <tr><td>XA4107</td><td>Rejestrator nie można zorganizować parametr typu `{0}` w podpisie metody `{1}`.</td></tr>
        <tr><td>XA4108</td><td>Rejestrator nie można pobrać typu ObjectiveC dla typu zarządzanego `{0}`.</td></tr>
        <tr><td>XA4109</td><td>Nie można skompilować kodu wygenerowanego rejestratora. Raport o usterce w pliku <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA4110</td><td>Rejestrator nie można zorganizować parametr wyjściowy typu `{0}` w podpisie metody `{1}`.</td></tr>
        <tr><td>XA4111</td><td>Rejestrator nie może utworzyć podpis dla typu `{0}' in method `{1} ".</td></tr>
        <tr><td>XA4200</td><td>W Aktywnej można generować tylko dla typów "claas".</td></tr>
        <tr><td>XA4201</td><td>Nie można ustalić nazwy JNI typu {0}.</td></tr>
        <tr><td>XA4203</td><td>Nazwa określonego typu musi być w pełni kwalifikowana.</td></tr>
        <tr><td>XA4204</td><td>Nie można rozpoznać typu interfejsu "{0}". Czy nie brakuje odwołania do zestawu?</td></tr>
        <tr><td>XA4205</td><td>[ExportField] można używać tylko w metodach o 0 parametrach.</td></tr>
        <tr><td>XA4206</td><td>[Eksportu] nie można używać w typie podstawowym.</td></tr>
        <tr><td>XA4207</td><td>[ExportField] nie można używać w typie podstawowym.</td></tr>
        <tr><td>XA4208</td><td>[Java.Interop.ExportFieldAttribute] nie można użyć metody zwraca typ void.</td></tr>
        <tr><td>XA4209</td><td>Nie można utworzyć klasy JavaTypeInfo: {0} z powodu {1}</td></tr>
        <tr><td>XA4210</td><td>Musisz dodać odwołanie do Mono.Android.Export.dll, korzystając z ExportAttribute lub ExportFieldAttribute.</td></tr>
        <tr><td>XA4211</td><td>AndroidManifest.xml //uses-sdk/@android:targetSdkVersion "{0}" jest mniejsza niż $(TargetFrameworkVersion) "{1}". Za pomocą interfejsu API-\ {1\} Aktywnej kompilacji.</td></tr>
    </tbody>
</table>

### <a name="xa5xxx-errors"></a>Błędy XA5xxx

<table>
    <thead>
        <tr><td>Kod błędu</td><td>Opis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA5101</td><td>Brak {0} kompilatora. Zainstaluj zestaw Android NDK.</td></tr>
        <tr><td>XA5102</td><td>Konwersja z zestawu do kodu macierzystego nie powiodła się. Raport o usterce w pliku <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5103</td><td>Nie można skompilować pliku "{0}". Raport o usterce w pliku <a href="http://bugzilla.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5201</td><td>Łączenie natywnego nie powiodło się. Zapoznaj się z tematem flag użytkownika do gcc: {0}</td></tr>
        <tr><td>XA5202</td><td>Łączenie natywnego nie powiodło się. Przejrzyj dziennik kompilacji.</td></tr>
        <tr><td>XA5303</td><td>Łączenie ostrzeżenie Native: {0}</td></tr>
        <tr><td>XA5300</td><td>Zestaw SDK systemu android nie znaleziono lub nie są w pełni zainstalowany.</td></tr>
        <tr><td>XA5301</td><td>Brak narzędzia "Usuń". Zainstaluj program Xcode składnik "narzędzia wiersza polecenia.</td></tr>
        <tr><td>XA5302</td><td>Brak narzędzie "dsymutil". Zainstaluj program Xcode składnik "narzędzia wiersza polecenia.</td></tr>
        <tr><td>XA5203</td><td>Nie można wygenerować symboli debugowania (dSYM katalogu). Przejrzyj dziennik kompilacji.</td></tr>
        <tr><td>XA5204</td><td>Nie powiodło się paska końcowego pliku binarnego. Przejrzyj dziennik kompilacji.</td></tr>
        <tr><td>XA5205</td><td>Brak narzędzie "aapt". Zainstaluj pakiet narzędzi kompilacji zestawu SDK systemu Android.</td></tr>
        <tr><td>XA5206</td><td>{0}. {1} katalogu zasobów systemu android nie istnieje.</td></tr>
        <tr><td>XA5207</td><td>{0}. {1} pliku biblioteki języka Java nie istnieje.</td></tr>
        <tr><td>XA5208</td><td>Pobieranie nie powiodło się. Pobierz {0} i umieszcza je w katalogu {1}.</td></tr>
        <tr><td>XA5209</td><td>Nie można rozpakować. Pobierz {0}, a następnie wyodrębnij go do katalogu {1}.</td></tr>
        <tr><td>XA5210</td><td>{0}. Natywnej biblioteki {1} plik nie istnieje.</td></tr>
    </tbody>
</table>

### <a name="xa6xxx-errors"></a>Błędy XA6xxx

<table>
    <thead>
        <tr><td>Kod błędu</td><td>Opis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA6001</td><td>Uruchomiona wersja Cecil nie obsługuje usuwanie zestawu</td></tr>
        <tr><td>XA6002</td><td>Nie można usunąć zestawu `{0}`.</td></tr>
        <tr><td>XA6003</td><td>Unauthorizedaccessexception — komunikatów]</td></tr>
    </tbody>
</table>

### <a name="xa9xxx-errors"></a>Błędy XA9xxx

Ta kody błędów są błędy licencjonowania i aktywacji.

 <a name="xa9000" />


#### <a name="xa9000"></a>XA9000

 **Przyczyna:** licencja wygasła

 **Sprawdzane podczas:** kompilacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>OSTRZEŻENIE</td>
            <td>OSTRZEŻENIE</td>
            <td>OSTRZEŻENIE</td>
            <td>OSTRZEŻENIE</td>
            <td>OSTRZEŻENIE</td>
        </tr>
    </tbody>
</table>

 <a name="XA9001" />


#### <a name="xa9001"></a>XA9001

 **Przyczyna:** wersji próbnej wygasła

 **Sprawdzane podczas:** kompilacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>OSTRZEŻENIE</td>
            <td>OSTRZEŻENIE</td>
            <td>OSTRZEŻENIE</td>
            <td>OSTRZEŻENIE</td>
            <td>OSTRZEŻENIE</td>
        </tr>
    </tbody>
</table>

 <a name="XA9002" />


#### <a name="xa9002"></a>XA9002

 **Przyczyna:** AndroidJavaSource

 **Sprawdzane podczas:** kompilacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Przyczyna:** AndroidJavaLibrary

 **Sprawdzane podczas:** kompilacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Jeśli w zestawie powiązania ma JAR osadzone, następnie to zostanie przechwycony w czasie pakietu, a nie czas kompilacji.**

 **Przyczyna:** AndroidNativeLibrary

 **Sprawdzane podczas:** kompilacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9003" />


#### <a name="xa9003"></a>XA9003

 **Cause:** System.Runtime.Serialization

 **Sprawdzane podczas:** pakietu

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Cause:** System.ServiceModel.Web

 **Sprawdzane podczas:** pakietu

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Przyczyna:** Mono.Data.Tds

 **Sprawdzane podczas:** kompilacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Odwołuje System.Data.dll, który jest dozwolony**

 **Przyczyna:** Mono.Android.Export

 **Sprawdzane podczas:** kompilacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9004" />


#### <a name="xa9004"></a>XA9004

 **Przyczyna:** --profilowania

 **Sprawdzane podczas:** pakietu

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9005" />


#### <a name="xa9005"></a>XA9005

 **Przyczyna:** limit rozmiaru (32 kb)

 **Sprawdzane podczas:** pakietu

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>OK</td>
            <td>-</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>

 <a name="XA9006" />


#### <a name="xa9006"></a>XA9006

 **Cause:** System.Data.SqlClient namespace

 **Sprawdzane podczas:** pakietu

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9008" />


#### <a name="xa9008"></a>XA9008

 **Przyczyna:** tworzenie z wiersza polecenia

 **Sprawdzane podczas:** kompilacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9009" />


#### <a name="xa9009"></a>XA9009

 **Przyczyna:** Brak numer seryjny

 **Sprawdzane podczas:** Untestable

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9010" />


#### <a name="xa9010"></a>XA9010

 **Przyczyna:** nieprawidłowy identyfikator produktu

 **Sprawdzane podczas:** kompilacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

Wartość równoważna XA9018

 <a name="XA9011" />


#### <a name="xa9011"></a>XA9011

 **Przyczyna:** nie może zaktualizować pliku licencji (do nowego formatu pliku)

 **Sprawdzane podczas:** aktywacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9012" />


#### <a name="xa9012"></a>XA9012

 **Przyczyna:** bez dostępu do Internetu

 **Sprawdzane podczas:** aktywacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9013" />


#### <a name="xa9013"></a>XA9013

 **Przyczyna:** nieznany błąd

 **Sprawdzane podczas:** aktywacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9014" />


#### <a name="xa9014"></a>XA9014

 **Przyczyna:** kod aktywacji nieprawidłowy

 **Sprawdzane podczas:** aktywacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9017" />


#### <a name="xa9017"></a>XA9017

 **Przyczyna:** Serwer aktywacji nie zwraca prawidłowej licencji

 **Sprawdzane podczas:** aktywacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

<a name="XA9018" />

#### <a name="xa9018"></a>XA9018

**Przyczyna:** nieprawidłową licencją

**Sprawdzane podczas:** kompilacji

<table>
    <thead>
        <tr>
            <th>Początkowe</th>
            <th>Indie</th>
            <th>Business(trial)</th>
            <th>Biznesowa</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-</td>
            <td>-</td>
            <td>ERROR</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>
