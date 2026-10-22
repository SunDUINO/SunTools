# SunGopher AI na Androida

[English version](ANDROID.EN.md)

Wersja mobilna agenta SunGopher AI. Ten sam backend w Go i ten sam interfejs co na desktopie, dopasowany do ekranu telefonu, z synchronizacją rozmów z komputerem w sieci domowej.

---

## Spis treści

- [Wymagania](#wymagania)
- [Instalacja](#instalacja)
- [Pierwsze uruchomienie](#pierwsze-uruchomienie)
- [Używanie aplikacji](#używanie-aplikacji)
- [Synchronizacja z komputerem](#synchronizacja-z-komputerem)
- [Dane i prywatność](#dane-i-prywatność)
- [Rozwiązywanie problemów](#rozwiązywanie-problemów)

---

## Wymagania

| | |
|---|---|
| System | Android 8.0 lub nowszy |
| Procesor | ARM64 (`arm64-v8a`) — praktycznie każdy telefon z ostatnich lat |
| Internet | wymagany do rozmów z modelami AI |
| Klucz API | co najmniej jeden: OpenAI, Gemini, Anthropic lub OpenRouter |

Wersja na Androida obsługuje **wyłącznie modele online**. Lokalna Ollama jest dostępna tylko na desktopie.

---

## Instalacja

### Sposób 1 — plik APK na telefonie

1. Pobierz plik `SunGopher_AI-android-*.apk` .
2. Otwórz pobrany plik na telefonie (np. z powiadomienia albo w aplikacji *Pliki*).
3. Jeśli Android zapyta o zgodę na **instalowanie nieznanych aplikacji**, zezwól przeglądarce lub menedżerowi plików, z którego otwierasz APK.
4. Stuknij **Zainstaluj**.

> **Google Play Protect** może ostrzec, że aplikacja jest nieznana — to normalne dla aplikacji spoza Sklepu Play. Wybierz **Więcej szczegółów → Zainstaluj mimo to**.

### Sposób 2 — przez `adb` z komputera

1. Na telefonie włącz **Opcje programisty**: *Ustawienia → Informacje o telefonie →* stuknij 7 razy w **Numer kompilacji**.
2. W *Opcjach programisty* włącz **Debugowanie USB** i podłącz telefon kablem.
3. Zainstaluj Platform-Tools (Windows: `winget install Google.PlatformTools`) i wykonaj:

```bash
adb devices                                   # zaakceptuj pytanie na telefonie
adb install -r SunGopher_AI-android-debug.apk
```

### Aktualizacja

Zainstaluj nowszy APK na istniejący (albo `adb install -r`). **Klucze API, rozmowy i parowanie z komputerem zostają zachowane.** Odinstalowanie aplikacji usuwa wszystkie jej dane.

---

## Pierwsze uruchomienie

1. Uruchom **SunGopher AI**.
2. Stuknij **⚙** w prawym górnym rogu.
3. Wpisz swoje imię (opcjonalnie) i **klucz API** co najmniej jednego silnika.
4. Stuknij **Zapisz**.
5. Otwórz menu **☰** i wybierz silnik AI, dla którego podałeś klucz.

Klucze API ustawia się **osobno na każdym urządzeniu** — nie są przesyłane podczas synchronizacji.

---

## Używanie aplikacji

### Ekran główny

| Element | Działanie |
|---|---|
| **☰** | wysuwa menu: nowy czat, wybór silnika AI, zapisane rozmowy |
| **⚙** | ustawienia: klucze API, synchronizacja z komputerem |
| **📎** | dołącz plik lub obrazek do wiadomości |
| **📷** | zrób zdjęcie i dołącz je do wiadomości |
| **👁️** | podgląd na żywo — agent może sam patrzeć przez kamerę |
| **Wyślij / ⏹ Stop** | wyślij wiadomość / przerwij generowanie odpowiedzi |

Wybranie rozmowy albo **+ Nowy czat** w menu ☰ automatycznie je zamyka. Rozmowę usuwa się przyciskiem **✕** przy jej nazwie.

### Kamera i skill Vision

- **📷 Zdjęcie** — otwiera aparat na pełnym ekranie. **📸 Zrób zdjęcie**, potem **Użyj zdjęcia** (albo **Ponów**). Zdjęcie trafia jako załącznik do następnej wiadomości.
- **🔄** — przełącza kamerę przód/tył. Domyślnie używany jest tylny aparat.
- **Agent prosi o zdjęcie** — gdy model uzna, że musi coś zobaczyć, pojawi się okno aparatu z informacją, czego potrzebuje. Możesz wysłać zdjęcie albo **Odmówić**.
- **👁️ Podgląd na żywo** — w rogu ekranu pojawia się mała miniatura kamery. Dopóki jest włączona, agent może pobierać klatki **bez pytania** (każda pobrana klatka mignie białą ramką). Miniaturę można przesunąć, przeciągając jej pasek. **✕** wyłącza podgląd i kamerę.

Przy pierwszym użyciu kamery Android zapyta o uprawnienie — wybierz **Zezwól**.

Zdjęcia są przed wysłaniem zmniejszane do 1024 px po dłuższym boku: szybciej idą przez sieć i zużywają mniej tokenów.

---

## Synchronizacja z komputerem

Rozmowy synchronizują się z wersją desktopową SunGopher AI (od v1.10.00), gdy **telefon i komputer są w tej samej sieci domowej**.

### Parowanie (jednorazowo)

1. **Na komputerze:** ⚙ Ustawienia → sekcja **Synchronizacja z telefonem**. Zostaw okno z kodem QR na ekranie.
2. **Na telefonie:** ⚙ Ustawienia → **📷 Skanuj kod QR z komputera**.
3. Skieruj aparat na kod QR. Telefon sam go odczyta, znajdzie komputer w sieci i od razu się zsynchronizuje.
4. W Ustawieniach telefonu pojawi się adres komputera z kłódką **🔒** — parowanie zakończone.

> **Windows:** przy pierwszym uruchomieniu SunGopher AI zapora zapyta o dostęp do sieci. Zezwól dla **sieci prywatnych** — synchronizacja używa portu **8766**.

**Bez kamery:** rozwiń **Wpisz adres ręcznie** i przepisz adres oraz kod widoczne na komputerze pod kodem QR, potem **Zapisz i synchronizuj**.

### Na co dzień

- Telefon synchronizuje się **automatycznie** przy uruchomieniu aplikacji i przy każdym powrocie do niej (najwyżej raz na 30 sekund).
- Poza domem synchronizacja po cichu się nie wykonuje — nic nie trzeba wyłączać.
- **Synchronizuj teraz** w Ustawieniach wymusza synchronizację od razu i pokazuje, ile wiadomości pobrano i wysłano.
- Na komputerze lista rozmów odświeża się, gdy wracasz do okna aplikacji.
- **Usunięcie rozmowy** na jednym urządzeniu usuwa ją także na drugim.

### Bezpieczeństwo

- Synchronizacja działa tylko w sieci lokalnej, bez żadnych serwerów pośredniczących.
- Połączenie jest szyfrowane **TLS 1.3**. Komputer ma własny certyfikat, a jego odcisk SHA-256 jest zapisany w kodzie QR — telefon połączy się tylko ze sparowanym komputerem.
- Dodatkowo wymagany jest kod dostępu z komputera.
- Interfejs aplikacji na komputerze nadal działa tylko lokalnie (`127.0.0.1`); w sieci dostępny jest wyłącznie port synchronizacji.

---

## Dane i prywatność

- Rozmowy, ustawienia i klucze API są zapisane **tylko w pamięci aplikacji na telefonie** (oraz na sparowanym komputerze po synchronizacji).
- Treść rozmów i zdjęcia trafiają do wybranego dostawcy AI (OpenAI, Google, Anthropic, OpenRouter) — zgodnie z jego zasadami.
- Wbudowany serwer aplikacji nasłuchuje tylko na `127.0.0.1` i przyjmuje połączenia wyłącznie z okna SunGopher AI — inne aplikacje na telefonie nie mają do niego dostępu.
- Odinstalowanie aplikacji usuwa wszystkie jej dane z telefonu.

---

## Rozwiązywanie problemów

| Problem | Rozwiązanie |
|---|---|
| *„nie znaleziono modelu lub adresu API”* | Sprawdź klucz API w ⚙. Jeśli klucz jest poprawny, zamknij aplikację (usuń ją z ostatnich aplikacji) i uruchom ponownie. |
| Kod QR nie jest rozpoznawany | Zbliż telefon, aby kod zajmował większą część ekranu, popraw ostrość, zmniejsz odblaski. Ewentualnie użyj **Wpisz adres ręcznie**. |
| *„kod odczytany, ale komputer nie odpowiada”* | Telefon i komputer muszą być w tej samej sieci Wi-Fi. Sprawdź, czy SunGopher AI działa na komputerze i czy zapora przepuszcza port 8766 (sieć prywatna). Sieci „dla gości” często blokują ruch między urządzeniami. |
| *„komputer odrzucił kod synchronizacji”* | Kod na komputerze się zmienił — sparuj telefon ponownie przez QR. |
| *„certyfikat komputera się nie zgadza”* | Komputer ma nowy certyfikat (np. po reinstalacji). Zeskanuj kod QR ponownie. |
| Kamera nie działa | *Ustawienia Androida → Aplikacje → SunGopher AI → Uprawnienia → Aparat → Zezwól*. |
| Aplikacja pokazuje *„Backend Go nie wystartował”* | Zbierz logi: `adb logcat -s gophersun` i zgłoś problem w [Issues](../../issues). |



### Jak to działa

```
┌──────────────── APK ────────────────┐
│ MainActivity (Kotlin)               │
│  ├─ uruchamia libgophersun.so       │  ← backend Go: GOOS=android, CGO_ENABLED=0
│  └─ WebView → http://127.0.0.1:8765 │  ← ten sam frontend co na desktopie
└─────────────────────────────────────┘
```

 
