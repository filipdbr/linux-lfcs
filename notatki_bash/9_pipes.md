# Przekierowania i potoki: Deskryptory i Strumienie

W Linuxie wszystko jest plikiem, a procesy komunikują się za pomocą trzech standardowych deskryptorów (File Descriptors - FD):
- **0 (stdin)**: Standardowe wejście (klawiatura).
- **1 (stdout)**: Standardowe wyjście (ekran).
- **2 (stderr)**: Standardowe wyjście błędów (ekran).

### 1. Złota zasada kolejności: Od lewej do prawej
System czyta przekierowania jedno po drugim. To, gdzie skierujesz strumień nr 2, zależy od tego, gdzie **w tym momencie** znajduje się strumień nr 1.

- **`command > plik 2>&1`** (POPRAWNIE):
  1. Najpierw stdout (1) zostaje skierowany do `plik`.
  2. Potem stderr (2) zostaje skierowany tam, gdzie aktualnie jest stdout (czyli do `plik`).
  *Efekt:* Wszystko ląduje w jednym pliku.

- **`command 2>&1 > plik`** (BŁĘDNIE):
  1. Najpierw stderr (2) zostaje skierowany tam, gdzie jest stdout (czyli na ekran).
  2. Potem stdout (1) zostaje skierowany do `plik`.
  *Efekt:* Błędy nadal lecą na ekran, a tylko normalny wynik trafia do pliku.



### 2. Rozdzielanie strumieni (Segregacja logów)
Często chcesz mieć czysty plik z wynikami i osobny plik z samymi błędami:
- `command > output.log 2> errors.log` – pełna separacja.
- `command >> backup.log 2>> backup.log` – dopisywanie obu strumieni do tego samego pliku (bezpieczniejsza wersja `>`).

### 3. Zaawansowane techniki
- **`&> plik`** – skrócony zapis w Bashu dla `> plik 2>&1`. Przekierowuje oba strumienie do jednego pliku (bardzo wygodne na egzaminie).
- **`/dev/null` (Czarna dziura)**:
  - `command 2> /dev/null` – ucisza tylko błędy.
  - `command &> /dev/null` – całkowicie ucisza komendę (nic nie wyświetla).

### 4. Narzędzie TEE (Rozgałęźnik)
`tee` jest jak trójnik w hydraulice. Standardowo przechwytuje tylko stdout. Jeśli chcesz, aby `tee` zapisywał też błędy, musisz je najpierw przekierować:
- `command 2>&1 | tee output.txt` – widzisz wszystko na ekranie i wszystko zapisuje się w pliku.
- `command | tee -a plik` – flaga `-a` (append) sprawia, że `tee` dopisuje do pliku zamiast go nadpisywać.

### 5. Here Documents (EOF)
Służy do przekazywania wielu linii tekstu do komendy bez edytowania plików:
```bash
cat << EOF > konfiguracja.conf
nazwa=serwer1
port=8080
EOF
```