---
title: Rozwiązywanie problemów z środowiska Xamarin Workbooks w systemie Android
description: Ten dokument zawiera wskazówki dotyczące rozwiązywania problemów do pracy ze skoroszytami platformy Xamarin w systemie Android. Omówiono w nim obsługi emulatora, skoroszyty, które nie będą ładowane i innych zagadnień.
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: b0333e1a40570374ee6218b7a848d2dd1c06b872
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351720"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Rozwiązywanie problemów z środowiska Xamarin Workbooks w systemie Android

## <a name="emulator-support"></a>Obsługa emulatora

Aby uruchomić skoroszytu programu dla systemu Android, emulatora systemu Android musi być dostępna do użycia. Fizyczne urządzenia z systemem Android nie są obsługiwane.

Firma Microsoft zaleca emulatora firmy Google, wraz z aparatu HAXM, jeśli komputer obsługuje tę funkcję.
Jeśli funkcja Hyper-V w systemie włączona, zawsze pod ręką emulatora systemu Android programu Visual Studio zamiast tego.

Konieczne jest posiadanie emulatorze z systemem Android 5.0 lub nowszy. Emulatory ARM nie są obsługiwane. Użyj `x86` lub `x86_64` tylko na urządzeniach.

Przeczytaj [naszą dokumentację na temat konfigurowania emulatorów systemu Android] [ android-emu] Jeśli użytkownik nie jest zaznajomiony z procesem.

> [!NOTE]
> Skoroszyty, 1.1 i starsze wersje będą spróbuj (i zakończyć się niepowodzeniem!) przy użyciu ARM emulatorów, jeśli są one dostępne. W celu obejścia tego, emulatora uruchamiania x86 wybranym przed otwarciem i utworzeniem skoroszytu programu dla systemu Android. Skoroszyty będą zawsze najpierw nawiązać połączenie z uruchomionego emulatora tak długo, jak są one zgodne.

## <a name="workbooks-wont-load"></a>Nie można załadować skoroszytów

### <a name="workbook-window-spins-forever-never-loads-windows"></a>Okno skoroszytu w nieskończoność, uruchamia nigdy nie obciążeń (Windows)

Najpierw sprawdź, czy emulator usługi ma dostęp do sieci w pełni pracy dzięki testom dowolnej witrynie sieci Web w przeglądarce sieci web emulatora usługi.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Visual Studio Emulator systemu Android nie może połączyć się z Internetem

Jeśli emulator usługi nie ma dostępu do sieci, może być konieczne wykonaj następujące kroki, aby naprawić przełącznik sieci funkcji Hyper-V. Możesz przełączać się między sieciami Wi-Fi często może być konieczne Powtórz tę czynność okresowo:

0. **Upewnij się, że dowolne sieci krytyczne operacje mają zostać wykonane zgodnie z tym tymczasowo odłączyć Windows z Internetu.**
1. Zamknij emulatorów.
2. Otwórz `Hyper-V Manager`.
3. W obszarze `Actions`, otwórz `Virtual Switch Manager...`.
4. Usuń wszystkie przełączniki wirtualne.
5. Kliknij przycisk `OK`.
6. Uruchamianie emulatora systemu Android programu VS. Będzie prawdopodobnie się monit o ponowne utworzenie przełącznika sieci wirtualnej.
7. Test, że przeglądarka VS emulatora systemu Android ma dostęp do Internetu.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>Linki pokrewne

- [Raportowania błędów](~/tools/workbooks/install.md#reporting-bugs)
