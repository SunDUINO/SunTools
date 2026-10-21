# SunTools — instalacja na Linuksie / Linux installation

🇵🇱 [Polski](#polski) | 🇬🇧 [English](#english)

---

## Polski

W tym folderze są linuksowe wersje programów SunTools:

| Program           | Plik                         | Silnik okna |
|-------------------|------------------------------|-------------|
| SunWebUI Edytor   | `SunWebUI_edytor_v01.17.01`  | WebKitGTK   |
| SunGo DB Designer | `SunDB_designer_v01.00.01`   | WebKitGTK   |
| SunMD View        | `SunMDView_v02.00.02`        | WebKitGTK   |
| SunMD Editor      | `SunMD_Editor_v02.00.02`     | WebKitGTK   |
| SunPLOT           | `SunPLOT_v00.05.00`          | WebKitGTK   |
| SunGopher AI      | `SunGopher_AI_v01.14.00`     | WebKitGTK   |
| SunVT220          | `SunVT220_v1.10.00`          | Ebitengine (OpenGL) |

Nazwy plików zawierają numer wersji — w poleceniach poniżej podmień je na swoje.
SunDEBUnal jest tylko na Windows; na Linuksie jego terminal zastępuje **SunVT220**.

### Wymagania

- Linux x86_64 (64-bit) ze środowiskiem graficznym
- glibc 2.34 lub nowsze
- programy na WebKitGTK: biblioteki **GTK 3** i **WebKitGTK 4.1**
- SunVT220: OpenGL i biblioteki X11 (na pulpicie zwykle już są)

Sprawdzone dystrybucje:

| Dystrybucja | Wersja |
|-------------|--------|
| Ubuntu      | 22.04 i nowsze |
| Linux Mint  | 21 i nowsze |
| Debian      | 12 i nowsze |

Starsze systemy (np. Ubuntu 20.04) nie są obsługiwane — nie mają WebKitGTK 4.1.

### 1. Zainstaluj wymagane biblioteki

Wystarczy zrobić to raz — dla wszystkich programów. GTK 3 jest zwykle już w systemie, WebKitGTK czasem trzeba doinstalować.

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

**SunVT220** (tylko jeśli czegoś brakuje — sprawdź `ldd`, patrz niżej):

```bash
sudo apt install libgl1 libx11-6 libxrandr2 libxcursor1 libxinerama1 libxi6 libxxf86vm1
```

### 2. Uruchom program

```bash
chmod +x SunMDView_v02.00.02
./SunMDView_v02.00.02
```

Tak samo dla pozostałych plików. Można też w menedżerze plików: prawy przycisk → *Właściwości* → *Uprawnienia* → zaznacz *Zezwól na wykonywanie pliku jako programu*, a potem uruchomić dwuklikiem.

### 3. Dostęp do portu szeregowego (SunVT220, SunPLOT)

Porty `/dev/ttyUSB0`, `/dev/ttyACM0` itd. należą do grupy `dialout` (na Arch: `uucp`). Dodaj się do niej raz:

```bash
sudo usermod -aG dialout $USER
```

Potem **wyloguj się i zaloguj ponownie**. Bez tego program zobaczy port, ale go nie otworzy (`permission denied`).

### 4. (Opcjonalnie) Instalacja dla wszystkich użytkowników

```bash
sudo install -m 755 SunMDView_v02.00.02 /usr/local/bin/sunmdview
sudo install -m 755 SunMD_Editor_v02.00.02 /usr/local/bin/sunmd_editor
sudo install -m 755 SunVT220_v1.10.00 /usr/local/bin/sunvt220
sudo install -m 755 SunPLOT_v00.05.00 /usr/local/bin/sunplot
sudo install -m 755 SunGopher_AI_v01.14.00 /usr/local/bin/sungopher
```

Od tej chwili programy uruchamiają się poleceniami `sunmdview`, `sunmd_editor`, `sunvt220`, `sunplot`, `sungopher` (nazwy wybierasz sam).

Odinstalowanie:

```bash
sudo rm /usr/local/bin/sunmdview /usr/local/bin/sunmd_editor /usr/local/bin/sunvt220 /usr/local/bin/sunplot
```

### Rozwiązywanie problemów

**`error while loading shared libraries: libwebkit2gtk-4.1.so.0`**
Brakuje WebKitGTK — wykonaj krok 1.

**`version 'GLIBC_2.34' not found`**
System jest za stary. Potrzebna dystrybucja z listy powyżej.

**`Permission denied` / `Brak dostępu` przy uruchamianiu**
Plik nie ma prawa wykonywania — wykonaj `chmod +x` z kroku 2. Jeśli plik leży na dysku FAT/NTFS lub pendrivie, skopiuj go najpierw do katalogu domowego.

**Port COM się nie otwiera**
Brak uprawnień do portu — wykonaj krok 3.

**Sprawdzenie, czego brakuje:**

```bash
ldd ./SunMDView_v02.00.02 | grep "not found"
```

Brak wyniku oznacza, że wszystkie biblioteki są na miejscu.



---

## English

This folder contains the Linux builds of the SunTools programs:

| Program           | File                         | Window engine |
|-------------------|------------------------------|---------------|
| SunWebUI Editor   | `SunWebUI_edytor_v01.17.01`  | WebKitGTK     |
| SunGo DB Designer | `SunDB_designer_v01.00.01`   | WebKitGTK     |
| SunMD View        | `SunMDView_v02.00.02`        | WebKitGTK     |
| SunMD Editor      | `SunMD_Editor_v02.00.02`     | WebKitGTK     |
| SunPLOT           | `SunPLOT_v00.05.00`          | WebKitGTK     |
| SunGopher AI      | `SunGopher_AI_v01.14.00`     | WebKitGTK     |
| SunVT220          | `SunVT220_v1.10.00`          | Ebitengine (OpenGL) |

The file names include the version number — replace them in the commands below with yours.
SunDEBUnal is Windows-only; on Linux use **SunVT220** as the terminal.

### Requirements

- Linux x86_64 (64-bit) with a graphical desktop
- glibc 2.34 or newer
- WebKitGTK programs: **GTK 3** and **WebKitGTK 4.1** libraries
- SunVT220: OpenGL and X11 libraries (usually already present on a desktop)

Tested distributions:

| Distribution | Version |
|--------------|---------|
| Ubuntu       | 22.04 and newer |
| Linux Mint   | 21 and newer |
| Debian       | 12 and newer |

Older systems (e.g. Ubuntu 20.04) are not supported — they do not ship WebKitGTK 4.1.

### 1. Install the required libraries

This only needs to be done once — for all programs. GTK 3 is usually present already; WebKitGTK sometimes has to be installed.

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

**SunVT220** (only if something is missing — check with `ldd`, see below):

```bash
sudo apt install libgl1 libx11-6 libxrandr2 libxcursor1 libxinerama1 libxi6 libxxf86vm1
```

### 2. Run the program

```bash
chmod +x SunMDView_v02.00.02
./SunMDView_v02.00.02
```

The same for the other files. Alternatively, in the file manager: right-click → *Properties* → *Permissions* → tick *Allow executing file as program*, then double-click to run.

### 3. Serial port access (SunVT220, SunPLOT)

Ports such as `/dev/ttyUSB0` and `/dev/ttyACM0` belong to the `dialout` group (`uucp` on Arch). Add yourself once:

```bash
sudo usermod -aG dialout $USER
```

Then **log out and log back in**. Without this the program lists the port but cannot open it (`permission denied`).

### 4. (Optional) Install for all users

```bash
sudo install -m 755 SunMDView_v02.00.02 /usr/local/bin/sunmdview
sudo install -m 755 SunMD_Editor_v02.00.02 /usr/local/bin/sunmd_editor
sudo install -m 755 SunVT220_v1.10.00 /usr/local/bin/sunvt220
sudo install -m 755 SunPLOT_v00.05.00 /usr/local/bin/sunplot
sudo install -m 755 SunGopher_AI_v01.14.00 /usr/local/bin/sungopher
```

The programs can then be started with `sunmdview`, `sunmd_editor`, `sunvt220`, `sunplot`, `sungopher` (pick any names you like).

To uninstall:

```bash
sudo rm /usr/local/bin/sunmdview /usr/local/bin/sunmd_editor /usr/local/bin/sunvt220 /usr/local/bin/sunplot
```

### Troubleshooting

**`error while loading shared libraries: libwebkit2gtk-4.1.so.0`**
WebKitGTK is missing — do step 1.

**`version 'GLIBC_2.34' not found`**
The system is too old. Use one of the distributions listed above.

**`Permission denied` when starting**
The file is not executable — run the `chmod +x` command from step 2. If the file is on a FAT/NTFS drive or USB stick, copy it to your home directory first.

**The serial port does not open**
No permission for the port — do step 3.

**Checking what is missing:**

```bash
ldd ./SunMDView_v02.00.02 | grep "not found"
```

No output means all libraries are in place.
