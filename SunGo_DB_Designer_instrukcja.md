# 📘 SunGo DB Designer — instrukcja obsługi

Wersja 01.00.00-beta-17 · Lothar TeaM

Instrukcja prowadzi od pustej planszy do działającej bazy w Twoim programie w Go. Przykład w tle: mały sklep — **klienci**, **zamówienia**, **produkty**.

---

## Spis treści

1. [Instalacja i uruchomienie](#1-instalacja-i-uruchomienie)
2. [Ekran startowy](#2-ekran-startowy)
3. [Okno programu](#3-okno-programu)
4. [Tabele](#4-tabele)
5. [Kolumny](#5-kolumny)
6. [Relacje](#6-relacje)
7. [Indeksy](#7-indeksy)
8. [Walidacja](#8-walidacja)
9. [Ustawienia projektu](#9-ustawienia-projektu)
10. [Podgląd kodu](#10-podgląd-kodu)
11. [Eksport](#11-eksport)
12. [Migracje — zmiany po pierwszym eksporcie](#12-migracje--zmiany-po-pierwszym-eksporcie)
13. [Podłączenie bazy do programu](#13-podłączenie-bazy-do-programu)
14. [Skróty klawiszowe](#14-skróty-klawiszowe)
15. [Najczęstsze problemy](#15-najczęstsze-problemy)

---

## 1. Instalacja i uruchomienie


Otworzy się okno programu. DB Designer działa na porcie **8788**, a GUI Builder na 8787, więc oba mogą być włączone jednocześnie.

Projekty zapisują się jako `projects/<Nazwa>.json` obok pliku wykonywalnego.

---

## 2. Ekran startowy

- **Nazwa nowego projektu**: wpisz nazwę. 💡 Jeśli do tego samego programu robisz interfejs w GUI Builderze, **użyj tej samej nazwy**. Wtedy oba narzędzia wyeksportują pliki do jednego folderu projektu.
- **Kafelki SQLite / PostgreSQL / MySQL**: wybierz bazę.
  - **SQLite**: plik na dysku, bez serwera. Najlepszy wybór dla aplikacji desktopowych.
  - **PostgreSQL**: serwer bazy, dla aplikacji sieciowych i wielu użytkowników.
  - **MySQL / MariaDB**: gdy tak jest na serwerze albo hostingu.
- **Otwórz istniejący**: lista zapisanych projektów.

Rodzaj bazy można zmienić później, ale najlepiej zrobić to przed pierwszym eksportem (zob. [rozdział 12](#12-migracje--zmiany-po-pierwszym-eksporcie)).

---

## 3. Okno programu

```
┌──────────────── topbar: projekt · cofnij/ponów · zoom · Kod · Zapisz · Wczytaj · Eksportuj ─────┐
│ Paleta         │                Plansza (diagram)                    │  Właściwości       │
│ • Nowa tabela  │   ┌─────────┐          ┌────────────┐               │  (projektu, tabeli │
│ • Relacje      │   │ klienci │──────────<│ zamowienia │               │   albo kolumny)    │
│ • Lista tabel  │   └─────────┘          └────────────┘               │                    │
└──────────── belka statusu: komunikaty · błędy · motyw · język · linki ─────────────────────┘
```

- **Paleta** (lewa strona): narzędzia, relacje i lista tabel. Kliknięcie tabeli na liście przewija do niej planszę.
- **Plansza** (środek): diagram. **Ctrl + kółko** przybliża, a przycisk **Dopasuj** pokazuje cały diagram.
- **Właściwości** (prawa strona) zależą od tego, co zaznaczysz:
  - puste miejsce → ustawienia **projektu**,
  - nagłówek tabeli → ustawienia **tabeli**,
  - wiersz kolumny → ustawienia **kolumny**.
- **Belka statusu**: komunikaty, licznik błędów, przełącznik motywu (Klasyczny / SunGo) i języka (PL/EN).

---

## 4. Tabele

### Dodawanie

Masz trzy sposoby:
- kliknij **Nowa tabela** w palecie,
- przeciągnij **Nowa tabela** na planszę,
- **kliknij dwukrotnie** w puste miejsce planszy.

**Tabela + znaczniki czasu** od razu dodaje kolumny `utworzono` i `zmieniono`, które program wypełnia sam.

Nowa tabela ma już kolumnę `id` (klucz główny z autonumeracją), a kursor stoi w polu nazwy, więc od razu możesz ją wpisać.

### Przesuwanie

Chwyć tabelę za **kolorowy nagłówek** i przeciągnij. Tabela przyciąga się do siatki. Z wciśniętym **Alt** możesz ją ustawić dokładnie, bez przyciągania.

### Ustawienia tabeli

| Pole | Opis |
|---|---|
| Nazwa tabeli (SQL) | np. `klienci`. Dozwolone są tylko litery, cyfry i `_`, bez polskich znaków. |
| Nazwa struktury Go | Puste = nazwa z tabeli (`klienci` → `Klienci`). Wpisz `Klient`, jeśli wolisz liczbę pojedynczą. |
| Komentarz | Trafia do kodu Go i do bazy (PostgreSQL, MySQL). |
| Kolor | Kolor nagłówka na diagramie. Służy tylko porządkowi. |

**Usuń tabelę**: przycisk na dole panelu albo klawisz **Delete**. Program ostrzeże, jeśli inne tabele mają do niej relacje. Te relacje zostaną usunięte razem z tabelą.

---

## 5. Kolumny

### Szybka edycja (panel tabeli)

Każdy wiersz na liście kolumn wygląda tak:

`🔑` · **nazwa** · **typ** · **NN** · `⋯` · `×`

- 🔑: klucz główny (klik włącza/wyłącza),
- NN: NOT NULL, czyli kolumna musi mieć wartość,
- ⋯: wszystkie ustawienia kolumny,
- ×: usuń kolumnę.

**Enter** w polu nazwy przechodzi do następnej kolumny, więc całą tabelę wpiszesz bez myszki.

Przyciski pod listą:
- **+ Kolumna** dodaje nową kolumnę,
- **+ id** dodaje klucz główny `id`,
- **+ czas** dodaje `utworzono` i `zmieniono`.

### Typy

| Typ | Do czego | W Go |
|---|---|---|
| `int` | zwykła liczba całkowita | `int` |
| `int64` | duża liczba, klucze główne | `int64` |
| `float` | liczba z przecinkiem | `float64` |
| `decimal` | kwoty, ceny (cyfry + miejsca po przecinku) | `float64` |
| `string` | krótki tekst z limitem znaków | `string` |
| `text` | długi tekst bez limitu | `string` |
| `bool` | tak / nie | `bool` |
| `datetime` | data i godzina | `time.Time` |
| `date` | sama data | `time.Time` |
| `uuid` | identyfikator UUID | `string` |
| `json` | dokument JSON | `string` |
| `blob` | dane binarne, pliki | `[]byte` |

💡 Kolumna **bez** NOT NULL jest w Go **wskaźnikiem** (`*string`, `*time.Time`). `nil` oznacza „brak wartości” (NULL).

### Pełne ustawienia kolumny (⋯)

**W bazie:**
- **Klucz główny**, a dla typu int/int64 także **Autonumeracja**.
- **NOT NULL**: kolumna musi mieć wartość.
- **Unikalne**: dwa wiersze nie mogą mieć takiej samej wartości (np. e-mail).
- **Wartość domyślna (SQL)**, np. `0`, `'nowe'`, `CURRENT_TIMESTAMP`. Tekst podaje się w apostrofach.
- **Czas ustawiany automatycznie** (tylko datetime/date):
  - *przy utworzeniu*: pole ustawia się przy `Create`,
  - *przy każdej zmianie*: pole ustawia się przy `Create` i przy każdym `Update`.
- **Komentarz**.

**Klucz obcy** i **Walidacja** są opisane w kolejnych rozdziałach.

Strzałki **↑ ↓** zmieniają kolejność kolumn. Taka sama kolejność będzie w strukturze Go.

Link **← tabela …** na górze panelu wraca do ustawień tabeli.

---

## 6. Relacje

### Rysowanie

1. W palecie kliknij rodzaj relacji. Na planszy pojawi się pomarańczowa podpowiedź.
2. Kliknij tabelę **nadrzędną**, czyli stronę „jeden” (np. `klienci`).
3. Kliknij tabelę **podrzędną**, czyli stronę „wiele” (np. `zamowienia`).

**Esc** anuluje rysowanie.

| Rodzaj | Co powstaje | Przykład |
|---|---|---|
| **1:N** (jeden do wielu) | w tabeli podrzędnej nowa kolumna z kluczem obcym, np. `klienci_id` | jeden klient ma wiele zamówień |
| **1:1** (jeden do jednego) | jak wyżej, ale kolumna jest unikalna | użytkownik ↔ jego profil |
| **N:M** (wiele do wielu) | nowa **tabela łącząca** z dwoma kluczami obcymi | zamówienie ma wiele produktów, produkt jest w wielu zamówieniach |

Tabela łącząca (np. `zamowienia_produkty`) ma przerywaną ramkę i znaczek **N:M**. Możesz dopisać do niej własne kolumny, np. `ilosc` albo `cena_w_dniu_zakupu`.

💡 Relację można zrobić do tej samej tabeli (np. kategoria nadrzędna → podkategorie). Kolumna klucza obcego jest wtedy opcjonalna.

### Linie na diagramie

- kreska `|` przy tabeli nadrzędnej oznacza „jeden”,
- rozgałęzienie ⋲ („kurza stopka”) przy tabeli podrzędnej oznacza „wiele”,
- kółko `o` oznacza, że relacja jest opcjonalna (klucz obcy może być pusty).

Kliknięcie linii zaznacza kolumnę klucza obcego.

### Co się dzieje po usunięciu wiersza nadrzędnego

W ustawieniach kolumny klucza obcego (⋯ → **Klucz obcy**) wybierasz, co ma się stać:

| Opcja | Co się dzieje, gdy usuniesz klienta, który ma zamówienia |
|---|---|
| **zablokuj** (domyślnie) | baza nie pozwoli usunąć klienta |
| **CASCADE** | zamówienia tego klienta też zostaną usunięte |
| **SET NULL** | zamówienia zostaną, a pole `klienci_id` zostanie wyczyszczone (kolumna nie może mieć NOT NULL) |

Tu możesz też ręcznie ustawić klucz obcy dowolnej kolumny: wybierz tabelę w polu **Wskazuje na tabelę**.

---

## 7. Indeksy

Indeks przyspiesza wyszukiwanie po kolumnie. Warto go dodać do kolumn, po których często filtrujesz, np. `status` albo `nazwisko`.

1. Zaznacz tabelę i kliknij **+ Indeks**.
2. Wpisz nazwę (program podpowie np. `ix_zamowienia_status`).
3. Zaznacz kolumny. Możesz wybrać kilka.
4. Opcjonalnie zaznacz **unikalny**, jeśli para lub zestaw wartości nie może się powtórzyć.

Indeksu dla pojedynczej unikalnej kolumny nie trzeba tworzyć ręcznie. Wystarczy zaznaczyć **Unikalne** w ustawieniach kolumny.

---

## 8. Walidacja

Ustawienia kolumny (⋯) → **Walidacja**. Na tej podstawie powstaje metoda `Validate()` dla każdej tabeli.

| Reguła | Dla typów | Przykład |
|---|---|---|
| Wymagane | wszystkie | imię nie może być puste |
| Minimalna długość | tekstowe | hasło: min. 8 znaków |
| Maksymalna długość | `string`, automatycznie z długości kolumny | `varchar(100)` → max 100 znaków |
| Adres e-mail | tekstowe | `jan@example.com` ✔, `jan@` ✘ |
| Wzorzec (regex) | tekstowe | kod pocztowy: `^[0-9]{2}-[0-9]{3}$` |
| Min / Max | liczbowe | ocena od 1 do 5 |

Błędy walidacji wracają jako lista pól z komunikatami, np. `email: nieprawidłowy adres e-mail`. Łatwo je pokazać przy polach formularza.

---

## 9. Ustawienia projektu

Kliknij puste miejsce planszy.

**Baza danych:**
- **Rodzaj bazy**: SQLite / PostgreSQL / MySQL.
- **Sterownik SQLite**: `modernc` (czyste Go, zalecany, nie wymaga GCC) albo `mattn` (cgo, wymaga GCC).
- **Nazwa bazy / pliku**: np. `sklep` → plik `sklep.db` albo baza `sklep` na serwerze.
- **Połączenie (DSN)**: pełny adres bazy, np. z hasłem do PostgreSQL. Puste = adres podpowiedziany w polu.

**Kod Go:**
- **Folder źródeł**: domyślnie `SRC`, tak jak w GUI Builderze.
- **Moduł Go**: zwykle zostaw puste. Program odczyta nazwę z `go.mod`.
- **Tagi validate / gorm**: dodatkowe tagi w strukturach, jeśli używasz tych bibliotek.

**Sprawdzenie projektu**: lista błędów i uwag. Kliknięcie pozycji prowadzi do miejsca problemu, a błędne tabele i kolumny są zaznaczone na czerwono na diagramie. Z błędami (⛔) eksport nie przejdzie. Uwagi (⚠) są tylko informacją.

---

## 10. Podgląd kodu

Przycisk **Kod** w topbarze pokazuje wszystkie pliki dokładnie tak, jak zostaną zapisane. Z lewej jest lista plików, z prawej treść wybranego pliku z kolorowaniem składni. Przycisk **Kopiuj** kopiuje treść do schowka.

Dobrze tu zajrzeć przed pierwszym eksportem, szczególnie do `handlersDb.go` i `db/doc.go`.

---

## 11. Eksport

1. Kliknij **Eksportuj…**.
2. Wybierz **folder nadrzędny**, ten sam co w GUI Builderze. Program zapamięta go na następny raz.
3. Przeczytaj podsumowanie:
   - **Pliki trafią do**: `<folder>/<Projekt>/SRC/db/` + `SRC/handlersDb.go`,
   - **Import w Go**: ścieżka pakietu i informacja, skąd pochodzi (z `go.mod`, z ustawień albo z nazwy projektu),
   - **Migracje**: czy to pierwszy eksport, czy powstanie kolejna migracja,
   - **Zmiany w schemacie** i ostrzeżenia.
4. Kliknij **Eksportuj**.

Co powstaje:

```
<Projekt>/SRC/
├── handlersDb.go          ← podłączenie do Twojego programu
└── db/
    ├── models_gen.go      ← struktury
    ├── crud_gen.go        ← operacje na danych
    ├── validate_gen.go    ← Validate()
    ├── db_gen.go          ← Open, Migrate, transakcje
    ├── doc.go             ← instrukcja w kodzie
    ├── schema.sql         ← pełny schemat (do podglądu)
    ├── schema.snapshot.json
    └── migrations/0001_…_init.sql
```

⚠️ **Nie edytuj** plików `*_gen.go`, `doc.go`, `schema.sql` ani `handlersDb.go`, bo są nadpisywane przy każdym eksporcie. Własny kod pisz w osobnym pliku, np. `SRC/db/queries.go` z `package db`.

💡 Projekt jest zapisywany automatycznie po udanym eksporcie.

---

## 12. Migracje — zmiany po pierwszym eksporcie

Zmieniasz diagram, eksportujesz ponownie i w `migrations/` pojawia się kolejny plik, np. `0002_…_klienci_dodano_telefon.sql`, zawierający **tylko różnice**.

| Robisz w edytorze | Dzieje się w bazie |
|---|---|
| zmieniasz nazwę kolumny lub tabeli | zmiana nazwy, **dane zostają** |
| dodajesz kolumnę, tabelę, indeks | zostaje dodana |
| usuwasz kolumnę lub tabelę | zostaje usunięta **razem z danymi** (okno eksportu ostrzeże) |
| zmieniasz typ, NOT NULL, wartość domyślną | kolumna jest zmieniana. W SQLite tabela jest przebudowywana z przeniesieniem danych |

Program przy starcie (`dbInit()`) sam wykonuje brakujące migracje, każdą w transakcji. Wykonane migracje zapisuje w tabeli `schema_migrations`, więc nic nie wykona się dwa razy.

⚠️ Nowa kolumna **NOT NULL bez wartości domyślnej** w tabeli, która ma już wiersze, nie może się udać, bo baza nie wie, co wpisać w istniejące wiersze. Dodaj wartość domyślną albo odznacz NOT NULL. Okno eksportu o tym przypomni.

### Zacznij migracje od nowa

Checkbox w oknie eksportu usuwa dotychczasowe migracje i tworzy nową `0001`. Przydaje się:
- na etapie zabawy i prototypu,
- po zmianie rodzaju bazy (np. SQLite → PostgreSQL). Program zaznaczy go wtedy sam.

Istniejącą bazę trzeba potem założyć od nowa (np. usunąć plik `.db`).

---

## 13. Podłączenie bazy do programu

Po eksporcie użyj w SunGo Project Manager opcji **Import all** (albo `go mod tidy`), żeby pobrać sterownik bazy.

### A) Projekt WebUI z GUI Buildera (`backend.go`)

**W `main()`**, przed `http.ListenAndServe`:

```go
if err := dbInit(); err != nil {
	log.Fatal(err)
}
defer dbClose()
```

**W `handleEvent`**, zaraz po dekodowaniu `req`:

```go
if resp, ok := dbEvent(req.Event, req.Data); ok {
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(resp)
	return
}
```

**We frontendzie (`app.js`):**

```js
// lista
const r = await sendToBackend("db.klienci.list", { limit: 50, offset: 0 });
if (r.ok) console.log(r.rows, r.total);

// dodanie
const n = await sendToBackend("db.klienci.create", {
    imie: "Jan", email: "jan@example.com", data_urodzenia: "1990-05-17"
});
if (!n.ok) console.log(n.error, n.fields); // błędy walidacji

// jeden rekord, zmiana, usunięcie
const k = (await sendToBackend("db.klienci.get", { id: 1 })).row;
k.imie = "Janek";
await sendToBackend("db.klienci.update", k);  // wysyłaj CAŁY rekord — patrz uwaga niżej
await sendToBackend("db.klienci.delete", { id: 1 });

// zamówienia jednego klienta (po kluczu obcym)
await sendToBackend("db.zamowienia.by_klienci_id", { klienci_id: 1 });
```

Wszystkie zdarzenia:

| Zdarzenie | Wysyłasz | Dostajesz |
|---|---|---|
| `db.<tabela>.list` | `{limit, offset}` | `{ok, rows, total}` |
| `db.<tabela>.get` | `{id}` | `{ok, row}` |
| `db.<tabela>.create` | pola | `{ok, row}`, z nadanym `id` |
| `db.<tabela>.update` | `id` + pola | `{ok, row}` |
| `db.<tabela>.delete` | `{id}` | `{ok}` |
| `db.<tabela>.count` | nic | `{ok, count}` |
| `db.<tabela>.by_<kolumna_fk>` | `{<kolumna_fk>: …}` | `{ok, rows}` |

⚠️ **`update` zapisuje wszystkie pola rekordu.** Pole, którego nie wyślesz, zostanie wyczyszczone. Dlatego najpierw pobierz rekord (`get`), zmień w nim co trzeba i odeślij całość. Wyjątkiem są pola „czas przy utworzeniu”, których `update` nigdy nie rusza.

Przy błędzie dostajesz `{ok: false, error: "…", fields: [...]}`. Daty mogą przyjść prosto z pól `<input type="date">` i `datetime-local`.

Twoje własne zdarzenia (bez `db.` na początku) działają jak dotąd, bo `dbEvent` je przepuszcza.

### B) Projekt Fyne z GUI Buildera (`handlers.go`)

```go
// w onReady():
if err := dbInit(); err != nil {
	log.Fatal(err)
}

// na początku handleEvent:
if resp, ok := dbEvent(event, data); ok {
	// np. wstaw resp["rows"] do listy
	return
}
```

### C) Bez GUI Buildera: bezpośrednio w Go

```go
if err := dbInit(); err != nil {
	log.Fatal(err)
}
defer dbClose()
ctx := context.Background()

k := db.Klienci{Imie: "Jan", Email: "jan@example.com"}
if err := k.Validate(); err != nil {
	fmt.Println(err) // walidacja: email: nieprawidłowy adres e-mail
}
dbStore.CreateKlienci(ctx, &k)             // k.ID jest już ustawione
lista, _ := dbStore.ListKlienci(ctx, 50, 0) // 50 pierwszych
jeden, err := dbStore.GetKlienci(ctx, k.ID)
if errors.Is(err, db.ErrNotFound) { /* nie ma takiego */ }
```

Pełną listę funkcji zobaczysz w podpowiedziach edytora (`dbStore.` + Ctrl+Spacja) albo w `SRC/db/doc.go`.

### Gdzie jest baza?

- **SQLite**: plik `<nazwa>.db` w katalogu, z którego uruchomisz program.
- **PostgreSQL / MySQL**: pod adresem z ustawień projektu.

Adres można zmienić bez ponownego eksportu, ustawiając zmienną środowiskową **`DB_DSN`**.

---

## 14. Skróty klawiszowe

| Skrót | Działanie |
|---|---|
| **Ctrl+Z** / **Ctrl+Y** | cofnij / ponów |
| **Ctrl+S** | zapisz projekt |
| **Delete** | usuń zaznaczoną tabelę lub kolumnę |
| **Esc** | anuluj rysowanie relacji / zamknij okno / odznacz |
| **Enter** (nazwa kolumny) | przejdź do następnej kolumny |
| **Ctrl + kółko** | przybliż / oddal |
| **Alt** + przeciąganie | przesuwanie bez przyciągania do siatki |
| podwójny klik na planszy | nowa tabela |
| podwójny klik na tabeli / kolumnie | przejście do edycji jej nazwy |

---

## 15. Najczęstsze problemy

**„Eksport nieudany: projekt zawiera błędy”**
Kliknij puste miejsce planszy i zobacz listę w **Sprawdzeniu projektu**. Kliknięcie błędu zaprowadzi Cię do właściwej tabeli.

**Import w `handlersDb.go` ma złą nazwę modułu (`Sklep/SRC/db` zamiast nazwy z `go.mod`)**
Eksportowałeś przed utworzeniem `go.mod`. Wystarczy wyeksportować ponownie, bo teraz nazwa zostanie odczytana z `go.mod`.

**„poprzednie migracje są dla bazy sqlite, a projekt ma teraz postgres”**
Zmieniłeś rodzaj bazy. Zaznacz **Zacznij migracje od nowa** i załóż bazę od nowa.

**Migracja wywala błąd przy starcie programu przez NOT NULL**
Dodałeś kolumnę NOT NULL bez wartości domyślnej do tabeli z danymi. Ustaw **Wartość domyślną** albo odznacz NOT NULL i wyeksportuj ponownie.

**MySQL: daty wracają jako tekst albo `update` zwraca „nie znaleziono”**
W DSN brakuje `parseTime=true` lub `clientFoundRows=true`. Domyślny adres je zawiera. Jeśli wpisujesz własny, dopisz je.

**MySQL: „kolumna typu text nie może być w indeksie/unikalna”**
To ograniczenie MySQL. Zmień typ na `string`, czyli VARCHAR z długością.

**SQLite: „database is locked”**
Wygenerowany kod używa jednego połączenia, więc w zwykłej aplikacji to się nie zdarza. Jeśli otwierasz bazę drugi raz sam, korzystaj z `dbConn` z `handlersDb.go`.

**Zmieniłem coś w `crud_gen.go` i zniknęło**
Pliki `*_gen.go` są nadpisywane przy każdym eksporcie. Własne funkcje trzymaj w osobnym pliku w folderze `db`, np. `queries.go`.

---

Pytania i błędy zgłaszaj na **[forum.lothar-team.pl](https://forum.lothar-team.pl)** 🐹
