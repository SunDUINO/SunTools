# SunMDView / SunMD Editor — instalacja na Linuksie / Linux installation

🇵🇱 [Polski](#polski) | 🇬🇧 [English](#english)

---

## Polski

Instrukcja dotyczy obu programów — mają identyczne wymagania:

| Program      | Plik                      |
|--------------|---------------------------|
| SunMDView    | `SunMDView_v02.00.01`     |
| SunMD Editor | `sunmd_editor_v02.00.01`  |

Nazwy plików zawierają numer wersji — w poleceniach poniżej podmień je na swoje.

### Wymagania

- Linux x86_64 (64-bit) ze środowiskiem graficznym
- glibc 2.34 lub nowsze
- Biblioteki GTK 3 i WebKitGTK 4.1

Sprawdzone dystrybucje:

| Dystrybucja | Wersja |
|-------------|--------|
| Ubuntu      | 22.04 i nowsze |
| Linux Mint  | 21 i nowsze |
| Debian      | 12 i nowsze |

Starsze systemy (np. Ubuntu 20.04) nie są obsługiwane — nie mają WebKitGTK 4.1.

### 1. Zainstaluj wymagane biblioteki

Wystarczy zrobić to raz — dla obu programów. GTK 3 jest zwykle już w systemie, WebKitGTK czasem trzeba doinstalować.

**Ubuntu / Linux Mint / Debian:**

```bash
sudo apt update
sudo apt install libgtk-3-0 libwebkit2gtk-4.1-0
```

**Fedora:**

```bash
sudo dnf install gtk3 webkit2gtk4.1
```

**Arch / Manjaro:**

```bash
sudo pacman -S gtk3 webkit2gtk-4.1
```

### 2. Uruchom program

```bash
chmod +x SunMDView_v02.00.01 sunmd_editor_v02.00.01
./SunMDView_v02.00.01
./sunmd_editor_v02.00.01
```

Można też w menedżerze plików: prawy przycisk → *Właściwości* → *Uprawnienia* → zaznacz *Zezwól na wykonywanie pliku jako programu*, a potem uruchomić dwuklikiem.

### 3. (Opcjonalnie) Instalacja dla wszystkich użytkowników

```bash
sudo install -m 755 SunMDView_v02.00.01 /usr/local/bin/sunmdview
sudo install -m 755 sunmd_editor_v02.00.01 /usr/local/bin/sunmd_editor
```

Od tej chwili programy uruchamiają się poleceniami `sunmdview` i `sunmd_editor`.

Odinstalowanie:

```bash
sudo rm /usr/local/bin/sunmdview /usr/local/bin/sunmd_editor
```

### Rozwiązywanie problemów

**`error while loading shared libraries: libwebkit2gtk-4.1.so.0`**
Brakuje WebKitGTK — wykonaj krok 1.

**`version 'GLIBC_2.34' not found`**
System jest za stary. Potrzebna dystrybucja z listy powyżej.

**`Permission denied` / `Brak dostępu`**
Plik nie ma prawa wykonywania — wykonaj `chmod +x` z kroku 2. Jeśli plik leży na dysku FAT/NTFS lub pendrivie, skopiuj go najpierw do katalogu domowego.

**Sprawdzenie, czego brakuje:**

```bash
ldd ./SunMDView_v02.00.01 | grep "not found"
ldd ./sunmd_editor_v02.00.01 | grep "not found"
```

Brak wyniku oznacza, że wszystkie biblioteki są na miejscu.



---

## English

This guide covers both programs — their requirements are identical:

| Program      | File                      |
|--------------|---------------------------|
| SunMDView    | `SunMDView_v02.00.01`     |
| SunMD Editor | `sunmd_editor_v02.00.01`  |

The file names include the version number — replace them in the commands below with yours.

### Requirements

- Linux x86_64 (64-bit) with a graphical desktop
- glibc 2.34 or newer
- GTK 3 and WebKitGTK 4.1 libraries

Tested distributions:

| Distribution | Version |
|--------------|---------|
| Ubuntu       | 22.04 and newer |
| Linux Mint   | 21 and newer |
| Debian       | 12 and newer |

Older systems (e.g. Ubuntu 20.04) are not supported — they do not ship WebKitGTK 4.1.

### 1. Install the required libraries

This only needs to be done once — for both programs. GTK 3 is usually present already; WebKitGTK sometimes has to be installed.

**Ubuntu / Linux Mint / Debian:**

```bash
sudo apt update
sudo apt install libgtk-3-0 libwebkit2gtk-4.1-0
```

**Fedora:**

```bash
sudo dnf install gtk3 webkit2gtk4.1
```

**Arch / Manjaro:**

```bash
sudo pacman -S gtk3 webkit2gtk-4.1
```

### 2. Run the program

```bash
chmod +x SunMDView_v02.00.01 sunmd_editor_v02.00.01
./SunMDView_v02.00.01
./sunmd_editor_v02.00.01
```

Alternatively, in the file manager: right-click → *Properties* → *Permissions* → tick *Allow executing file as program*, then double-click to run.

### 3. (Optional) Install for all users

```bash
sudo install -m 755 SunMDView_v02.00.01 /usr/local/bin/sunmdview
sudo install -m 755 sunmd_editor_v02.00.01 /usr/local/bin/sunmd_editor
```

The programs can then be started with the `sunmdview` and `sunmd_editor` commands.

To uninstall:

```bash
sudo rm /usr/local/bin/sunmdview /usr/local/bin/sunmd_editor
```

### Troubleshooting

**`error while loading shared libraries: libwebkit2gtk-4.1.so.0`**
WebKitGTK is missing — do step 1.

**`version 'GLIBC_2.34' not found`**
The system is too old. Use one of the distributions listed above.

**`Permission denied`**
The file is not executable — run the `chmod +x` command from step 2. If the file is on a FAT/NTFS drive or USB stick, copy it to your home directory first.

**Checking what is missing:**

```bash
ldd ./SunMDView_v02.00.01 | grep "not found"
ldd ./sunmd_editor_v02.00.01 | grep "not found"
```

No output means all libraries are in place.

