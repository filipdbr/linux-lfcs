# Pakiety i repozytoria: APT, APT-GET i DPKG

Zarządzanie pakietami to nie tylko instalacja, to także dbanie o czystość systemu i rozwiązywanie konfliktów zależności.

### APT vs APT-GET
- `apt` – stworzony dla ludzi (ładniejszy interfejs, łączy funkcje `apt-get` i `apt-cache`).
- `apt-get` – stworzony do skryptów i automatyzacji (bardziej przewidywalny).

### Zaawansowane operacje APT/APT-GET
- `sudo apt update` – odświeża bazę danych o pakietach (`/var/lib/apt/lists/`).
- `sudo apt dist-upgrade` (lub `full-upgrade` w apt) – inteligentna aktualizacja, która potrafi usuwać/dodawać pakiety, aby rozwiązać konflikty przy nowej wersji systemu.
- `sudo apt autoremove` – **KLUCZOWE:** usuwa zbędne pakiety (zależności), które zostały zainstalowane automatycznie, a nie są już potrzebne.
- `sudo apt clean` – czyści lokalny magazyn pobranych plików `.deb` (`/var/cache/apt/archives/`), zwalniając miejsce na dysku.
- `apt-cache policy [pakiet]` – pokazuje, z którego repozytorium i w jakiej wersji zostanie zainstalowany pakiet.
- `apt-cache depends [pakiet]` – wyświetla listę wszystkich zależności wymaganych przez program.



### Narzędzie DPKG (Praca na plikach .deb)
Dpkg nie potrafi sam pobierać plików z internetu ani rozwiązywać zależności, ale jest niezastąpiony do inspekcji.
- `dpkg -i pakiet.deb` – instalacja lokalna.
- `dpkg -r pakiet` – usunięcie (remove).
- `dpkg -P pakiet` – całkowite usunięcie z konfiguracją (purge).
- `dpkg -L pakiet` – **BARDZO WAŻNE:** pokazuje listę wszystkich plików zainstalowanych przez dany pakiet (gdzie trafiły binarki, logi, konfigi).
- `dpkg -S /sciezka/do/pliku` – odwrotność powyższego: mówi Ci, który pakiet zainstalował ten konkretny plik na dysku.
- `dpkg --get-selections` – eksportuje listę wszystkich zainstalowanych pakietów (przydatne do odtworzenia systemu).

# 📦 dpkg - Debian Package Manager

Narzędzie niskiego poziomu do obsługi plików `.deb`. Nie rozwiązuje zależności automatycznie (w przeciwieństwie do `apt`).

| Flaga | Działanie | Przykład |
| :--- | :--- | :--- |
| `-i` | **Install** - instaluje plik .deb | `sudo dpkg -i google-chrome.deb` |
| `-r` | **Remove** - usuwa pakiet (zostawia konfigurację) | `sudo dpkg -r wget` |
| `-P` | **Purge** - usuwa wszystko (łącznie z konfami) | `sudo dpkg -P wget` |
| `-l` | **List** - lista zainstalowanych pakietów | `dpkg -l | grep python` |
| `-L` | **List files** - gdzie są pliki danego pakietu | `dpkg -L tar` |
| `-S` | **Search** - czyj to plik? | `dpkg -S /bin/bash` |
| `-s` | **Status** - szczegółowe info o pakiecie | `dpkg -s libc6` |

**Złota zasada:** Jeśli `dpkg -i` zgłosi błędy zależności (dependency errors), napraw je komendą:
`sudo apt install -f`



### Repozytoria i pliki źródłowe
- `/etc/apt/sources.list` – główny plik z listą serwerów (repozytoriów).
- `/etc/apt/sources.list.d/` – katalog na dodatkowe pliki repozytoriów (np. dla Dockera, Google Chrome).