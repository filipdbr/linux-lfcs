# Archiwizacja i kompresja

Na egzaminie LFCS często pojawiają się pułapki dotyczące zachowania uprawnień plików oraz struktury katalogów wewnątrz archiwum.

### Zaawansowane flagi TAR
- `tar -p` (preserve permissions) – zachowuje oryginalne uprawnienia plików (bardzo ważne przy backupie systemowym).
- `tar --exclude='/home/filip/Downloads'` – pozwala pominąć konkretny folder podczas pakowania całości.
- `tar -u` (update) – dodaje pliki do archiwum tylko, jeśli są nowsze niż te już w nim istniejące (działa tylko na nieskompresowanych plikach `.tar`).
- `tar -rvf archiwum.tar plik.txt` – bezwarunkowo dopisuje (append) plik na koniec istniejącego archiwum `.tar`.
- `tar -W` (verify) – sprawdza spójność archiwum po jego utworzeniu.



### Praca ze ścieżkami (Kluczowe na egzaminie!)
Egzaminatorzy sprawdzają, czy nie robisz "tar-bombs" (rozpakowanie plików bezpośrednio do `/`).
- **Unikanie ścieżek bezwzględnych:** Domyślnie `tar` usuwa wiodący `/` ze ścieżek, aby archiwum było bezpieczne. Jeśli jednak MUSISZ go zachować, używasz flagi `-P`.
- **Wypakowanie konkretnego pliku:** Nie musisz rozpakowywać całego gigantycznego archiwum, by odzyskać jeden plik:
  `tar -xzvf backup.tar.gz etc/hosts` (wypakuje tylko plik hosts).

### Samodzielna kompresja (Gzip, Bzip2, XZ)
Czasami dostaniesz plik, który nie jest "tarem", a jedynie został skompresowany:
- `gzip plik.txt` – tworzy `plik.txt.gz` i **usuwa** oryginalny plik!
- `gunzip plik.txt.gz` – przywraca oryginał.
- `bzip2 -k plik.txt` – flaga `-k` (keep) sprawia, że oryginalny plik zostaje na dysku obok skompresowanego.
- `zcat` / `bzcat` / `xzcat` – pozwala podejrzeć zawartość skompresowanego pliku tekstowego bez jego rozpakowywania na dysk.



### Szybka ściąga z rozszerzeń i flag:
1. `.tar.gz` lub `.tgz` -> flaga `-z` (gzip)
2. `.tar.bz2` lub `.tbz2` -> flaga `-j` (bzip2)
3. `.tar.xz` lub `.txz` -> flaga `-J` (xz)