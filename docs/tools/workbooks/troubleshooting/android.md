---
title: "Rozwiązywanie problemów z Xamarin skoroszytów w systemie Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: eb188abb3e757f6f66af7758ced311ae1236d3ce
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Rozwiązywanie problemów z Xamarin skoroszytów w systemie Android

## <a name="emulator-support"></a>Obsługa emulatora

Aby uruchomić Android skoroszytu, emulatorze systemu Android musi być dostępna do użycia. Fizyczne urządzenia z systemem Android nie są obsługiwane.

Firma Microsoft zaleca emulator firmy Google wraz z HAXM, jeśli komputer obsługuje tę funkcję.
Jeśli funkcji Hyper-V w systemie włączona, przejdź zamiast z programu Visual Studio Emulator systemu Android.

Musi mieć emulator z systemem Android 5.0 lub nowszej. Emulatory ARM nie są obsługiwane. Użyj `x86` lub `x86_64` tylko urządzenia.

Przeczytaj [naszej dokumentacji na temat konfigurowania systemu Android emulatorów] [ android-emu] Jeśli nie masz doświadczenia z procesem.

**Uwaga:** skoroszytów 1.1 i starszych będzie spróbuj (i zakończyć się niepowodzeniem!) do użycia ARM emulatory, jeśli są dostępne. W celu obejścia tego emulatora uruchamiania x86 wybranym przed otwarciem i utworzeniem Android skoroszytu. Skoroszyty zawsze preferowane nawiązać działającego emulatora, tak długo, jak jest zgodny.

## <a name="workbooks-wont-load"></a>Nie można załadować skoroszytów

### <a name="workbook-window-spins-forever-never-loads-windows"></a>Okno skoroszytu nieskończoność, obraca nigdy nie obciążenia (z systemem Windows)

Najpierw sprawdź, czy Twoje emulatora ma dostęp do sieci w pełni pracy testując wszystkich witryn sieci Web w przeglądarce sieci web to emulator.

### <a name="visual-studio-android-emulator-cannot-connect-to-internet"></a>Visual Studio Emulator systemu Android nie można połączyć z Internetem

Jeśli Twoje emulatora nie ma dostępu do sieci, może być konieczne wykonaj następujące kroki, aby naprawić przełącznika sieci funkcji Hyper-V. Przełączanie między sieci Wi-Fi często może być konieczne okresowo Powtórz tę czynność:

0. **Upewnij się, że wszystkie operacje sieciowe krytyczne są zakończone, jak to tymczasowo rozłączyć systemu Windows z Internetu.**
1. Zamknij emulatory.
2. Otwórz `Hyper-V Manager`.
3. W obszarze `Actions`, otwórz `Virtual Switch Manager...`.
4. Usuń wszystkie przełączniki wirtualne.
5. Kliknij przycisk `OK`.
6. Uruchamianie emulatora systemu Android w wersji programu VS. Będzie prawdopodobnie się monit o ponowne utworzenie przełącznika sieci wirtualnej.
7. Test, że przeglądarki VS emulatora systemu Android mogą uzyskać dostęp do Internetu.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>Linki pokrewne

- [Raportowanie błędów](~/tools/workbooks/install.md#reporting-bugs)
