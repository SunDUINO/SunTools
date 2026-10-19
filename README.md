# SunTools

Zestaw darmowych narzędzi desktopowych dla programistów **Go** i **embedded**, z rodziny **SunGo**.
Każde narzędzie to **jeden plik wykonywalny**: bez instalatora, wystarczy pobrać i uruchomić.
Większość programów ma też wersję dla **Linuksa** (folder [`Linux`](Linux)), a niektóre także dla **Androida** (folder [`Android`](Android)).

## 🇬🇧 English

**SunTools** is a set of free, single-file desktop tools for Go and embedded developers (Windows, most of them also Linux, some also Android):

- **SunWebUI Editor**: drag-and-drop GUI designer that exports HTML/CSS/JS, Fyne or TUI plus a Go backend template.
- **SunGo DB Designer**: visual database designer that generates Go structs, SQL migrations, CRUD on `database/sql` and validation. Supports SQLite, PostgreSQL and MySQL/MariaDB. [User manual](Manuals/SunGo_DB_Designer_user_manual.md).
- **SunMD View**: ad-free Markdown viewer with table of contents, search, live reload, source view, Mermaid diagrams, themes, HTML export and `.md` file association.
- **SunMD Editor**: Markdown editor with live preview, syntax completion, table formatting, link checking, image paste, Mermaid diagrams and session restore.
- **SunDEBUnal**: serial port (COM/UART) and TCP/Telnet terminal for embedded work: text/HEX modes, built-in VT220/ANSI emulator, F1–F8 macro keys, signal lines, auto port detection, device-to-terminal commands, logging.
- **SunVT220**: retro VT220/ANSI terminal over COM, TCP, Telnet and **SSH**, with amber/green/white phosphor and CRT effect, TTF/OTF fonts.
- **SunPLOT**: live plotter for data sent by a microcontroller over UART or TCP: multiple panes, oscilloscope-style trigger, A/B cursors, moving average, CSV/PNG export.
- **SunGopher AI**: desktop AI agent with its own personality: ChatGPT, Gemini, Claude and local Ollama in one window, skills and tool calling, camera and Vision. Windows, Linux and **Android** (conversations sync with the PC over the home network). [Android guide](Android/ANDROID.EN.md).

---
---

# ⚠️ SunTools a Windows Defender: -- > **fałszywy alarm**

<details>
<summary>Czytaj więcej ...</summary>


Kilka osób mogło zobaczyć, że **Windows Defender przeniósł któryś program do kwarantanny**. Zgłoszenie wyglądało tak:

> **Behavior:Win32/DefenseEvasion.A!ml**

**Spokojnie, w programie nie ma wirusa.** Oto, co się stało.

To nie jest znaleziony wirus ani podejrzany kawałek kodu. Końcówka **!ml** oznacza ocenę 
**uczenia maszynowego na podstawie zachowania programu**. Defender patrzy, *co program robi*, 
i jeśli przypomina to sztuczki złośliwego oprogramowania, na wszelki wypadek blokuje plik. 
„DefenseEvasion” to mniej więcej „omijanie zabezpieczeń”.

## Co wcześniejsze wersje programów robiły „podejrzanego”

W szablonie moich programów SunGo jest taka funkcja, wywoływana przy każdym starcie:


```go
// removeZoneIdentifier usuwa znacznik "Zone.Identifier" (Mark of the Web)
// z własnego pliku .exe na Windows, żeby SmartScreen nie blokował programu
// po pobraniu/skopiowaniu z internetu. Na innych systemach nic nie robi.

func removeZoneIdentifier() {
	if runtime.GOOS != "windows" {
		return
	}
	exePath, err := os.Executable()
	if err != nil {
		return
	}
	_ = os.Remove(exePath + ":Zone.Identifier")
}
```

Każdy plik pobrany z internetu dostaje od Windows ukrytą „karteczkę” **Zone.Identifier** (tzw. *Mark of the Web*) z informacją, 
że przyszedł z sieci. Na jej podstawie SmartScreen pyta „czy na pewno uruchomić?”.

Funkcja kasuje tę karteczkę **z własnego pliku .exe**, żeby przy kolejnym uruchomieniu SmartScreen już nie marudził. 
Niestety złośliwe programy robią dokładnie to samo, żeby zatrzeć ślad, że przyszły z internetu. 
Dla Defendera wygląda to więc jak klasyczne omijanie zabezpieczeń. Stąd alarm.

Do tego funkcja właściwie i tak jest bezużyteczna: SmartScreen sprawdza plik **przed** uruchomieniem, 
czyli zanim ten kod w ogóle zdąży się wykonać. Umieściłem ją w sumie niejako testowo. 

</details>

- **Funkcja `removeZoneIdentifier()` została usunięta z Programów**  Programy niczego już nie omijają !! 
- **Jeśli masz wątpliwości, pytaj śmiało na forum https://forum.lothar-team.pl/viewtopic.php?t=1121.**

---
---

| Narzędzie | Do czego służy | Windows | Linux | Android |
|---|---|---|---|---|
| 🎨 **SunWebUI Edytor** | wizualne projektowanie interfejsu → HTML/CSS/JS, Fyne, TUI + szablon backendu Go | [v01.17.01](SunWebUI_edytor_v01.17.01.exe) | [v01.17.01](Linux/SunWebUI_edytor_v01.17.01) | – |
| 🗄️ **SunGo DB Designer** | projektowanie bazy danych na diagramie → struktury Go, migracje SQL, CRUD | [v01.00.01](SunDB_designer_v01.00.01.exe) | [v01.00.01](Linux/SunDB_designer_v01.00.01) | – |
| 📖 **SunMD View** | szybka przeglądarka plików Markdown | [v02.03.02](SunMDView_v02.03.02.exe) | [v02.00.02](Linux/SunMDView_v02.00.02) | – |
| 📝 **SunMD Editor** | edytor Markdown z podglądem na żywo i podpowiadaniem składni | [v02.02.02](SunMD_Editor_v02.02.02.exe) | [v02.00.02](Linux/SunMD_Editor_v02.00.02) | – |
| 🔌 **SunDEBUnal** | terminal portu szeregowego (COM/UART) i TCP/Telnet z emulatorem VT220 | [v2.1.0](SunDEBUnal_v2.1.0.exe) | – | – |
| 🖥️ **SunVT220** | terminal VT220 (COM / TCP / Telnet / SSH) | [v01.10.00](SunVT220_v01.10.00.exe) | [v01.10.00](Linux/SunVT220_v1.10.00) | – |
| 📈 **SunPLOT** | wykres na żywo danych z mikrokontrolera (UART / TCP) | [v00.05.00](SunPLOT_v00.05.00.exe) | [v00.05.00](Linux/SunPLOT_v00.05.00) | – |
| 🤖 **SunGopher AI** | desktopowy agent AI: ChatGPT, Gemini, Claude i lokalna Ollama w jednym oknie | [v01.13.00](SunGopher_AI_v01.13.00.exe) | [v01.12.02](Linux/SunGopher_AI_v01.12.02) | [v01.12.02](Android/SunGopher_AI-android_v01.12.02.apk) |

Instrukcje: **SunGo DB Designer** [PL](Manuals/SunGo_DB_Designer_instrukcja.md) · [EN](Manuals/SunGo_DB_Designer_user_manual.md) · **SunGopher AI na Androida** [PL](Android/ANDROID.PL.md) · [EN](Android/ANDROID.EN.md) · instalacja na Linuksie: [Linux/readme.md](Linux/readme.md)

--- 

## 🎨 SunWebUI Edytor (SunGo GUI Builder)

Projektujesz okno programu, przeciągając komponenty myszką: przyciski, pola tekstowe, listy, checkboxy, obrazki, kontenery, belki i konsolę. Eksport daje gotowy frontend (`index.html`, `style.css`, `app.js`) i szablon **`backend.go`**, do którego dopisujesz logikę w Go. Projekt można też wyeksportować jako aplikację **Fyne** albo **TUI**.

- siatka i „przyklejanie” do krawędzi innych elementów, układy wiersz / kolumna / siatka,
- panel właściwości: kolory, wymiary, czcionki, obramowania,
- **Podgląd** gotowego okna bez wychodzenia z edytora,
- zdarzenia (binding) przycisków podłączane do backendu Go,
- projekty zapisywane jako `projects/<Nazwa>.json` obok programu.

## 🗄️ SunGo DB Designer

Rysujesz tabele i relacje na diagramie, podobnie jak w projektancie Entity Framework z Visual Studio. Program generuje z nich kod:

- **struktury Go** z tagami `json`, `db` (opcjonalnie `validate`, `gorm`),
- **migracje SQL**: pierwsza tworzy wszystkie tabele, każda kolejna zawiera tylko zmiany,
- **funkcje CRUD** na czystym `database/sql`, bez ORM, razem z zapytaniami po relacjach,
- **walidację** (`Validate()`) bez zewnętrznych bibliotek,
- **`handlersDb.go`**: most do interfejsu z SunWebUI Edytora.

Obsługiwane bazy: **SQLite** (domyślnie, czyste Go), **PostgreSQL**, **MySQL 8 / MariaDB**.
Pełna instrukcja: [Manuals/SunGo_DB_Designer_instrukcja.md](Manuals/SunGo_DB_Designer_instrukcja.md).

> 💡 Edytor GUI i DB Designer mogą działać jednocześnie (porty 8787 i 8788). Jeśli w obu użyjesz tej samej nazwy projektu, wyeksportują pliki do wspólnego folderu.

## 📖 SunMD View

Przeglądarka plików `.md`, bez reklam.

- **spis treści** z boku, z podświetleniem aktualnego rozdziału,
- **wyszukiwanie** (Ctrl+F) z licznikiem trafień,
- **automatyczne odświeżanie** po zapisaniu pliku w edytorze,
- **podgląd ↔ kod źródłowy** (Ctrl+U) z numerami linii,
- kolorowanie składni w blokach kodu, tabele, listy zadań, przypisy, obrazki,
- diagramy **Mermaid** (bez internetu) oraz bloki `> [!NOTE]`, `[!TIP]`… i ramki z forum `[note]…[/note]`,
- trzy motywy: **jasny** (jak GitHub), **SunGo klasyczny**, **ciemny SunGo**,
- linki między plikami `.md`, eksport do **HTML** i druk,
- jednym kliknięciem **skojarzenie plików `.md`** z programem, z własną ikoną.

## 📝 SunMD Editor

Edytor plików Markdown, brat SunMD View: ten sam renderer i te same motywy. Edytor **CodeMirror 6** jest wbudowany w program, więc wszystko działa offline.

- **podpowiadanie składni**: `/` otwiera menu wstawiania (tabela, kod, zadanie, uwaga, przypis, spis treści, data, emoji…), po ```` ``` ```` lista języków, po `](` pliki z folderu dokumentu, po `](#` rozdziały; **Ctrl+Spacja** w dowolnym miejscu,
- **podgląd na żywo** z synchronizacją przewijania w obie strony; dwuklik w podglądzie skacze do linii,
- **tabele**: Tab / Shift+Tab między komórkami z wyrównaniem kolumn, Ctrl+Shift+F wyrównuje wszystkie tabele,
- **sprawdzanie linków**: brakujące pliki i rozdziały są podkreślane, F8 przechodzi do następnego problemu,
- **obrazki**: Ctrl+V ze zrzutem ekranu albo upuszczenie pliku zapisuje obrazek do `img/` i wstawia link,
- karty i sesja (niezapisane zmiany wracają po ponownym uruchomieniu), autozapis, wykrywanie zmian na dysku,
- konspekt, paleta poleceń (**Ctrl+Shift+P**), szukaj i zamień z regex, wiele kursorów, zaznaczanie prostokątne,
- diagramy **Mermaid**, eksport do HTML, druk, sprawdzanie pisowni, wpis „Edytuj w SunMD Editor” w menu Eksploratora.

## 🔌 SunDEBUnal

Terminal portu szeregowego do testowania mikrokontrolerów i modułów (ESP8266, Arduino, STM32, GSM itd.). To nie jest tylko okienko z tekstem:

- tryb **tekstowy (UTF-8)** i **HEX**, wysyłanie tekstu, komend i ciągów HEX,
- połączenie przez **port COM** albo przez sieć: **TCP** i **Telnet**,
- wbudowany **emulator terminala VT220 / ANSI** z kolorami i pełnym ekranem (Alt+Enter); oprogramowanie w mikrokontrolerze może rysować menu i okna jak w starych terminalach,
- **przyciski F1–F8** z własnymi komendami i **szybkie kody**, np. komendy AT dla ESP8266,
- podgląd i sterowanie **liniami sygnałowymi** (DTR, RTS…),
- automatyczne wykrywanie portów; po odłączeniu urządzenia USB port sam się zamyka,
- **komendy od urządzenia**: program w mikrokontrolerze może przez UART kazać terminalowi zapiszczeć, zaznaczyć punkt w logu albo przekierować dane do osobnego okna,
- zapis logu do pliku, koder/dekoder **Base64**, tryb **terminala dyskowego (HDD)** do serwisowania dysków przez UART.

Napisany w C# (WinForms) i wymaga **.NET Framework 3.5** (Windows: Panel sterowania → Włącz lub wyłącz funkcje systemu Windows). Tylko Windows; na Linuksa jest **SunVT220**.

## 🖥️ SunVT220

Emulator terminala VT220 wyjęty z SunDEBUnala i przepisany w Go, więc działa i pod Windows, i pod Linuksem. Sam terminal, bez dodatków.

- połączenia: **COM** (300–921600, 8N1, 7E1…), **TCP**, **Telnet** (z negocjacją, przedstawia się jako VT220) i **SSH**,
- **SSH**: logowanie w oknie terminala jak w `ssh` / PuTTY, zapamiętywanie kluczy serwera w `known_hosts` z ostrzeżeniem przy zmianie klucza, logowanie kluczem z `~/.ssh`, `xterm-256color`, zmiana rozmiaru okna, keepalive,
- sekwencje **VT100 / VT220 / ANSI**, kolory 16 / 256, ramki DEC rysowane liniami,
- trzy luminofory (**bursztynowy**, zielony, biały) i wyłączalny **efekt kineskopu**,
- układy ekranu Auto, 80×25, 100×37 i **132×43**, pełny ekran pod **Alt+Enter**,
- własne czcionki **TTF / OTF** (folder `fonts` obok programu), wklejanie Ctrl+Shift+V / Shift+Insert / prawy przycisk myszy,
- ustawienia w panelu **Set-Up** (Alt+S).

## 📈 SunPLOT

Wykres na żywo tego, co wysyła mikrokontroler, czyli Serial Plotter z Arduino, tylko porządny.

- źródła danych: **port COM**, **TCP** (np. ESP8266 po Wi-Fi) i wbudowane **DEMO**,
- sam rozpoznaje dane: `23.4,55,1013` (CH1, CH2, CH3), `temp=23.4 hum=55`, `temp:23.4`; reszta trafia do konsoli,
- **kilka paneli** jeden pod drugim, każdy z własną skalą Y i wspólnym czasem,
- **pauza** (Spacja) z przybliżaniem, przesuwaniem i zaznaczaniem fragmentu, a dane zbierają się dalej w tle,
- **trigger jak w oscyloskopie**: zbocze, poziom (przeciągany myszą), tryby Auto / Normal / Single, pre-trigger, histereza,
- **kursory pomiarowe A i B**: Δt, 1/Δt, wartości, min / max / średnia między kursorami,
- **średnia krocząca** dla każdej serii, legenda z min / max / średnią, konsola z wysyłaniem komend,
- eksport do **CSV** i **PNG**, własny silnik rysowania (płynnie nawet przy 921600 bodów).

## 🤖 SunGopher AI

Mały agent AI na pulpicie, nazywa się **Gophersun** i ma własną osobowość. Silnik wybierasz sam, także lokalny przez **Ollamę**, bez wysyłania danych w świat. Powstał dla zabawy, a jest tak dobry, jak podłączony do niego model.

- silniki: **OpenAI (ChatGPT)**, **Google Gemini**, **Anthropic (Claude)**, **OpenRouter** i **Ollama** (modele lokalne, tylko na komputerze),
- klucze API wpisuje się w oknie ustawień i działają od razu, bez restartu,
- historia rozmów zapisywana w **SQLite**,
- **skille** i wywoływanie narzędzi przez model,
- zdjęcie z kamery jako załącznik i skill **Vision** (agent sam może poprosić o zdjęcie),
- Windows, Linux i **Android**.

### 📱 Wersja na Androida

Ten sam backend w Go i ten sam interfejs co na komputerze, dopasowany do ekranu telefonu. Plik: [`Android/SunGopher_AI-android_v01.12.02.apk`](Android/SunGopher_AI-android_v01.12.02.apk).

- **synchronizacja rozmów** z komputerem w sieci domowej (od wersji desktopowej v1.10.00): parowanie kodem QR, szyfrowanie TLS 1.3, bez serwerów pośredniczących,
- aparat w telefonie: zdjęcie jako załącznik i **podgląd na żywo**, przez który agent może sam patrzeć,
- tylko modele online (Ollama działa wyłącznie na komputerze); klucze API ustawia się osobno na każdym urządzeniu.

Instalacja, parowanie i rozwiązywanie problemów: [Android/ANDROID.PL.md](Android/ANDROID.PL.md) · [English](Android/ANDROID.EN.md).

---

## Wymagania

**Windows**

- Windows 10 / 11,
- programy na WebView2 (SunWebUI Edytor, DB Designer, SunMD View, SunMD Editor, SunPLOT, SunGopher AI): **Microsoft Edge WebView2 Runtime**; na Windows 11 jest zawsze, na Windows 10 instaluje się razem z Edge,
- SunDEBUnal: **.NET Framework 3.5**,
- SunVT220: nic dodatkowego.

**Linux**

- x86_64, glibc 2.34 lub nowsze (Ubuntu 22.04+, Linux Mint 21+, Debian 12+),
- programy na WebView: **GTK 3** i **WebKitGTK 4.1** (`sudo apt install libgtk-3-0 libwebkit2gtk-4.1-0`),
- po pobraniu nadaj prawo wykonywania: `chmod +x <plik>`.

Szczegóły i rozwiązywanie problemów: [Linux/readme.md](Linux/readme.md).

**Android**

- Android 8.0 lub nowszy, procesor ARM64 (`arm64-v8a`), praktycznie każdy telefon z ostatnich lat,
- aplikacja jest spoza Sklepu Play: przy instalacji zezwól na **instalowanie nieznanych aplikacji**, a jeśli Play Protect ostrzeże, wybierz **Więcej szczegółów → Zainstaluj mimo to**,
- aktualizacja: zainstaluj nowszy plik `.apk` na istniejący, rozmowy i ustawienia zostają.

Szczegóły: [Android/ANDROID.PL.md](Android/ANDROID.PL.md).

> ⚠️ Pliki `.exe` są podpisane cyfrowo certyfikatem własnym autora (lokalnym), a nie certyfikatem komercyjnego urzędu certyfikacji. Windows nie zna takiego certyfikatu, więc przy pierwszym uruchomieniu SmartScreen może pokazać ostrzeżenie. Kliknij **Więcej informacji → Uruchom mimo to**.

## Technologia

Narzędzia SunGo: Go + [webview_go](https://github.com/webview/webview_go) (WebView2 na Windows, WebKitGTK na Linuksie), interfejs wbudowany w plik wykonywalny. SunGopher AI na Androida: backend Go uruchamiany w aplikacji Kotlin z WebView, ten sam interfejs co na komputerze. SunVT220: Go + [Ebitengine](https://ebitengine.org). SunDEBUnal: C# WinForms. Wszystkie narzędzia mają wersję **polską i angielską**.

---

**Autor:** Andrzej SunRiver Gromczyński · [Lothar TeaM](https://lothar-team.pl) · [Forum](https://forum.lothar-team.pl)
**Licencja:** MIT
