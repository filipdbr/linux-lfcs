## Polecenie ls (Listowanie)

**Składnia:** `ls [opcje] [plik/katalog]` 

| Opcja | Nazwa | Opis |
| :--- | :--- | :--- |
| `-a` | All | Wyświetla wszystkie pliki, również ukryte (zaczynające się od kropki). |
| `-l` | Long | Długi format; pokazuje typ, uprawnienia, liczbę dowiązań, właściciela, grupę, rozmiarv
| `-d` | Directory | Nie wyświetla zawartości katalogów, jedynie ich nazwy. |
| `-i` | Inode | Wyświetla numery i-węzłów plików. |
| `-h` | Human-readable | W połączeniu z `-l` wyświetla rozmiary w jednostkach KB, MB, GB. |
| `-S` | Sort by Size | Sortuje pliki według rozmiaru. |
| `-t` | Time | Sortuje według czasu ostatniej modyfikacji. |
| `-F` | Classify | Dodaje znak typu: `/` (katalog), `*` (wykonywalny), `@` (link), `=` (gniazdo), `\|` (FIFO). |
| `-r` | Reverse | Odwraca kolejność sortowania. |
| `-R` | Recursive | Wyświetla zawartość katalogu oraz wszystkich jego podkatalogów. |

---

## Polecenie cp (Kopiowanie)

**Składnia:** `cp [opcje] plik_źródłowy plik_docelowy`

| Opcja | Nazwa | Opis |
| :--- | :--- | :--- |
| `-r` / `-R` | Rekurencyjnie | Kopiuje katalogi wraz z całą ich zawartością. |
| `-p` | Preserve | Zachowuje atrybuty pliku oryginalnego (uprawnienia, daty). |
| `-i` | Interaktywnie | Prosi o potwierdzenie przed nadpisaniem istniejącego pliku. |
| `-f` | Force | Usuwa istniejące pliki docelowe bez prośby o potwierdzenie. |
| `-v` | Verbose | Pokazuje szczegółowo, co jest aktualnie kopiowane. |

---

## Polecenie mv (Przenoszenie / Zmiana nazwy)

**Składnia:** `mv [opcje] źródło cel`

| Opcja | Nazwa | Opis |
| :--- | :--- | :--- |
| `-i` | Interaktywnie | Prosi o potwierdzenie operacji przed nadpisaniem pliku. |
| `-f` | Force | Usuwa istniejące pliki docelowe bez prośby o potwierdzenie. |
| `-v` | Verbose | Wyświetla informację o każdym przeniesionym pliku. |
| `-u` | Update | Przenosi tylko wtedy, gdy źródło jest nowsze niż cel lub cel nie istnieje. |

---

## mkdir

**Składnia:** `mkdir [opcje] katalog`

| Opcja | Nazwa | Opis |
| :--- | :--- | :--- |
| `-p` | Parents | Tworzy katalogi nadrzędne w razie potrzeby; nie zgłasza błędu, jeśli katalog już istnieje. |
| `-v` | Verbose | Wyświetla informację o każdym utworzonym katalogu. |
| `-m` | Mode | Ustawia uprawnienia przy tworzeniu katalogu (np. `-m 755`). |

Mozna zrobić tak:
```bash
mkdir -p projekt_chmura/{konfiguracja,skrypty}
```
Dwa katalogi (lub więcej) na raz

## Polecenie rm (Usuwanie)

**Składnia:** `rm [opcje] plik` 

| Opcja | Nazwa | Opis |
| :--- | :--- | :--- |
| `-r` / `-R` | Rekurencyjnie | Rekurencyjnie usuwa zawartość katalogów (całe gałęzie drzewa). |
| `-i` | Interaktywnie | Prosi o potwierdzenie operacji dla każdego usuwanego pliku. |
| `-f` | Force | Tryb forsowny; usuwa pliki bez prośby o potwierdzenie. |
| `-v` | Verbose | Informuje o każdym usuwanym obiekcie. |

# Optymalizacja tworzenia katalogów

Zamiast używać dwóch komend (`mkdir` + `chmod`), możesz użyć flagi `-m`:

* **Składnia:** `mkdir -m [uprawnienia] [nazwa_katalogu]`
* **Przykład z zadania:** `mkdir -m 100 /home/bob/LFCS`
* **Zaleta:** Folder powstaje od razu z docelowymi uprawnieniami, co jest bezpieczniejsze (brak momentu, w którym folder ma domyślne, szersze uprawnienia).

### Przypomnienie o systemie ósemkowym:
* **100** -> --x ------ (Tylko właściciel może wykonać/wejść)
* **400** -> r-- ------ (Tylko właściciel może czytać)
* **000** -> --------- (Nikt nie ma żadnych praw)