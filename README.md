# SunTools

Zestaw darmowych narzędzi desktopowych dla programistów **Go** z rodziny **SunGo**.
Każde narzędzie to **jeden plik `.exe`**: bez instalatora i bez zależności, wystarczy pobrać i uruchomić.

| Narzędzie | Do czego służy | Pobierz |
|---|---|---|
| 🎨 **SunWebUI Edytor** (SunGo GUI Builder) | wizualne projektowanie interfejsu → HTML/CSS/JS + szablon backendu Go | [v01.17.00](SunWebUI_edytor_v01.17.00.exe) |
| 🗄️ **SunGo DB Designer** | projektowanie bazy danych na diagramie → struktury Go, migracje SQL, CRUD | [v01.00.00-beta-17](SunDB_designer_v01.00.00-beta-17.exe) |
| 📖 **SunMD View** | szybka przeglądarka plików Markdown | [v01.01.00](SunMDView_v01.01.00.exe) |

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

---

## Wymagania

- Windows 10 / 11,
- **Microsoft Edge WebView2 Runtime**: na Windows 11 jest zawsze, na Windows 10 instaluje się razem z Edge.

> ⚠️ Pliki `.exe` są podpisane cyfrowo certyfikatem własnym autora (lokalnym), a nie certyfikatem komercyjnego urzędu certyfikacji. Windows nie zna takiego certyfikatu, więc przy pierwszym uruchomieniu SmartScreen może pokazać ostrzeżenie. Kliknij **Więcej informacji → Uruchom mimo to**.

## Technologia

Go + [webview_go](https://github.com/webview/webview_go) (WebView2). Interfejs jest wbudowany w plik wykonywalny. Wszystkie narzędzia mają wersję **polską i angielską**.

---

## 🇬🇧 English

**SunTools** is a set of free, single-file desktop tools for Go developers (Windows 10/11, WebView2):

- **SunWebUI Editor**: drag-and-drop GUI designer that exports HTML/CSS/JS plus a Go backend template.
- **SunGo DB Designer**: visual database designer that generates Go structs, SQL migrations, CRUD on `database/sql` and validation. Supports SQLite, PostgreSQL and MySQL/MariaDB.
- **SunMD View**: ad-free Markdown viewer with table of contents, search, live reload, source view, themes, HTML export and `.md` file association.

---

**Autor:** Andrzej SunRiver Gromczyński · [Lothar TeaM](https://lothar-team.pl) · [Forum](https://forum.lothar-team.pl)
**Licencja:** MIT
