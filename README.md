# SunTools

Zestaw darmowych narzędzi desktopowych dla programistów **Go** i **embedded**, z rodziny **SunGo**.
Każde narzędzie to **jeden plik `.exe`**: bez instalatora, wystarczy pobrać i uruchomić.

## 🇬🇧 English

**SunTools** is a set of free, single-file Windows desktop tools for Go and embedded developers:

- **SunWebUI Editor**: drag-and-drop GUI designer that exports HTML/CSS/JS plus a Go backend template.
- **SunGo DB Designer**: visual database designer that generates Go structs, SQL migrations, CRUD on `database/sql` and validation. Supports SQLite, PostgreSQL and MySQL/MariaDB.
- **SunMD View**: ad-free Markdown viewer with table of contents, search, live reload, source view, themes, HTML export and `.md` file association.
- **SunDEBUnal**: serial port (COM/UART) terminal for embedded work: text/HEX modes, built-in VT220/ANSI emulator, F1–F8 macro keys, signal lines, auto port detection, device-to-terminal commands, logging.

---
# ⚠️ SunTools a Windows Defender: -- > **fałszywy alarm**

Kilka osób mogło zobaczyć, że **Windows Defender przeniósł któryś program do kwarantanny**. Zgłoszenie wyglądało tak:

> **Behavior:Win32/DefenseEvasion.A!ml** lub podobny..

**Spokojnie, w programie nie ma wirusa.** Oto, co się stało.

To nie jest znaleziony wirus ani podejrzany kawałek kodu. Końcówka **!ml** oznacza ocenę **uczenia maszynowego na podstawie zachowania programu**. Defender patrzy, *co program robi*, i jeśli przypomina to sztuczki złośliwego oprogramowania, na wszelki wypadek blokuje plik. „DefenseEvasion” to po polsku mniej więcej „omijanie zabezpieczeń”.

## Co programy robią „podejrzanego”

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
czyli zanim ten kod w ogóle zdąży się wykonać. Umieściłem ją wsumie niejako testowo. 

##  Zmiany w kolejnych wersjach --- 

- **Funkcja `removeZoneIdentifier()` zostanie usunięta z Programów**  Program niczego już nie będzie kasował i nie bedzie dotykł swojego pliku.
- Usuną ją też z szablonu nowych projektów.


## Co możesz zrobić

Jeśli Defender zablokował Ci plik:

1. Jeśli chcesz odzyskać starą: **Zabezpieczenia Windows → Ochrona przed wirusami i zagrożeniami → Historia ochrony** → wpis z SunMDView → **Przywróć**.
2. Poczekaj na nowe wydanie toolsów.

Programy są podpisane moim własnym certyfikatem. Jeśli masz wątpliwości, pytaj śmiało na forum https://forum.lothar-team.pl/viewtopic.php?t=1121. 

---

| Narzędzie | Do czego służy | Pobierz |
|---|---|---|
| 🎨 **SunGo GUI Builder** | wizualne projektowanie interfejsu → WebUI , Fyne, TUI + szablon backendu Go | [v01.17.00](https://forum.lothar-team.pl/viewtopic.php?p=3587#p3587) |
| 🗄️ **SunGo DB Designer** | projektowanie bazy danych na diagramie → struktury Go, migracje SQL, CRUD | [v01.00.00-beta-17](https://forum.lothar-team.pl/viewtopic.php?t=1119) |
| 📖 **SunMD View** | szybka przeglądarka plików Markdown | [v02.00.00](https://forum.lothar-team.pl/viewtopic.php?t=1120) |
| 📖 **SunMD Editor** | prosty edytor tekstu pracujący w Markdown z podpowiadaniem składni | [v02.00.00](https://forum.lothar-team.pl/viewtopic.php?t=1122) |
| 🔌 **SunDEBUnal** | terminal portu szeregowego (COM/UART) z emulatorem VT220 | [v2.0.0](https://forum.lothar-team.pl/viewtopic.php?p=3592#p3592) |


--- 

## 🎨 SunWebUI Edytor (SunGo GUI Builder)

Projektujesz okno programu, przeciągając komponenty myszką: przyciski, pola tekstowe, listy, checkboxy, obrazki, kontenery, belki i konsolę. Eksport daje gotowy frontend (`index.html`, `style.css`, `app.js`) i szablon **`backend.go`**, do którego dopisujesz logikę w Go.

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

> 💡 Edytor GUI i DB Designer mogą działać jednocześnie (porty 8787 i 8788). Jeśli w obu użyjesz tej samej nazwy projektu, wyeksportują pliki do wspólnego folderu.

## 📖 SunMD View

Przeglądarka plików `.md`, bez reklam.

- **spis treści** z boku, z podświetleniem aktualnego rozdziału,
- **wyszukiwanie** (Ctrl+F) z licznikiem trafień,
- **automatyczne odświeżanie** po zapisaniu pliku w edytorze,
- **podgląd ↔ kod źródłowy** (Ctrl+U) z numerami linii,
- kolorowanie składni w blokach kodu, tabele, listy zadań, przypisy, obrazki,
- trzy motywy: **jasny** (jak GitHub), **SunGo klasyczny**, **ciemny SunGo**,
- linki między plikami `.md`, eksport do **HTML** i druk,
- jednym kliknięciem **skojarzenie plików `.md`** z programem, z własną ikoną.

## 🔌 SunDEBUnal

Terminal portu szeregowego do testowania mikrokontrolerów i modułów (ESP8266, Arduino, STM32, GSM itd.). To nie jest tylko okienko z tekstem:

- tryb **tekstowy (UTF-8)** i **HEX**, wysyłanie tekstu, komend i ciągów HEX,
- wbudowany **emulator terminala VT220 / ANSI** z kolorami i pełnym ekranem (Alt+Enter); oprogramowanie w mikrokontrolerze może rysować menu i okna jak w starych terminalach,
- **przyciski F1–F8** z własnymi komendami i **szybkie kody**, np. komendy AT dla ESP8266,
- podgląd i sterowanie **liniami sygnałowymi** (DTR, RTS…),
- automatyczne wykrywanie portów; po odłączeniu urządzenia USB port sam się zamyka,
- **komendy od urządzenia**: program w mikrokontrolerze może przez UART kazać terminalowi zapiszczeć, zaznaczyć punkt w logu albo przekierować dane do osobnego okna,
- zapis logu do pliku, koder/dekoder **Base64**, tryb **terminala dyskowego (HDD)** do serwisowania dysków przez UART.

Napisany w C# (WinForms) i wymaga **.NET Framework 3.5** (Windows: Panel sterowania → Włącz lub wyłącz funkcje systemu Windows). Najnowsza wersja jest na [forum Lothar TeaM](https://forum.lothar-team.pl).

---

## Wymagania

- Windows 10 / 11,
- **Microsoft Edge WebView2 Runtime**: na Windows 11 jest zawsze, na Windows 10 instaluje się razem z Edge.

> ⚠️ Pliki `.exe` są podpisane cyfrowo certyfikatem własnym autora (lokalnym), a nie certyfikatem komercyjnego urzędu certyfikacji. Windows nie zna takiego certyfikatu, więc przy pierwszym uruchomieniu SmartScreen może pokazać ostrzeżenie. Kliknij **Więcej informacji → Uruchom mimo to**.

## Technologia

Narzędzia SunGo: Go + [webview_go](https://github.com/webview/webview_go) (WebView2), interfejs wbudowany w plik wykonywalny. SunDEBUnal: C# WinForms. Wszystkie narzędzia mają wersję **polską i angielską**.

---



---

**Autor:** Andrzej SunRiver Gromczyński · [Lothar TeaM](https://lothar-team.pl) · [Forum](https://forum.lothar-team.pl)
**Licencja:** MIT
