# Dzień 2: Uprawnienia, Właściciele i Strumienie

## 1. Zarządzanie uprawnieniami (chmod)
Zmienia tryb dostępu do pliku. Składa się z trzech cyfr: **Właściciel | Grupa | Reszta**.

**Składnia:** `chmod [trzy_cyfry] nazwa_pliku`  
*Suma wag: 4 (r) + 2 (w) + 1 (x)*

| Opcja/Kod | Nazwa | Opis |
| :--- | :--- | :--- |
| `7` | rwx | Pełne uprawnienia (4+2+1). |
| `6` | rw- | Odczyt i zapis (4+2). |
| `5` | r-x | Odczyt i wykonanie (4+1). |
| `4` | r-- | Tylko odczyt. |
| `-R` | Recursive | Zmienia uprawnienia dla całego katalogu i plików wewnątrz. |

> **Przykłady:** > - `chmod 755 skrypt.sh` (Ty wszystko, inni tylko czytają/wykonują).
> - `chmod 600 id_rsa` (Tylko Ty czytasz/piszesz – standard dla kluczy SSH).

### 1.1 Tryb Symboliczny (chmod)
Zamiast cyfr, używamy literowych oznaczeń użytkowników i akcji. Jest to bardziej precyzyjne, jeśli chcesz zmienić tylko jeden parametr.

**Składnia:** `chmod [kogo][akcja][co] plik`

| Kogo (Who) | Co (Action) | Uprawnienie (Mode) |
| :--- | :--- | :--- |
| `u` - właściciel (user) | `+` - dodaj | `r` - odczyt |
| `g` - grupa (group) | `-` - zabierz | `w` - zapis |
| `o` - inni (others) | `=` - ustaw dokładnie | `x` - wykonanie |
| `a` - wszyscy (all) | | |

**Przykłady z życia DevOpsa:**
- `chmod +x skrypt.sh` -> Dodaje prawo wykonania wszystkim (najczęstsza komenda).
- `chmod u+x skrypt.sh` -> Dodaje prawo wykonania **tylko** właścicielowi.
- `chmod g-w config.php` -> Zabiera grupie prawo do edycji pliku.
- `chmod ug+rw baza.log` -> Daje właścicielowi i grupie prawo do czytania i pisania.

## Sticky Bit (Specjalne uprawnienie folderu)
Zabezpiecza katalog przed usuwaniem plików przez osoby, które nie są ich właścicielami.

**Składnia:** `sudo chmod +t nazwa_folderu`

| Uprawnienie | Widok w `ls -ld` | Działanie |
| :--- | :--- | :--- |
| **Sticky Bit** | `drwxrwxrwT` | Tylko właściciel pliku (lub root) może go skasować. |

**Zastosowanie:**
- Katalogi tymczasowe (np. `/tmp`).
- Wspólne foldery projektowe, gdzie nikt nie powinien kasować pracy innych.

**Przykład użycia:**
```bash
sudo chmod +t /opt/shared_space
```

---

## 2. Zmiana właściciela (chown)
Określa, do kogo należy plik lub katalog. Często wymaga `sudo`.

**Składnia:** `sudo chown [użytkownik]:[grupa] nazwa_pliku`

| Opcja | Nazwa | Opis |
| :--- | :--- | :--- |
| `użytkownik` | Owner | Nowy właściciel pliku. |
| `:grupa` | Group | Nowa grupa (można zmienić samą grupę przez `:grupa`). |
| `-R` | Recursive | Zmienia właściciela dla całej struktury katalogów. |

---

## 3. Strumienie i Przekierowania (>, >>, 2>)
Obsługa wejścia i wyjścia komend.

**Składnia:** `komenda [operator] plik`

| Operator | Nazwa | Opis |
| :--- | :--- | :--- |
| `>` | Overwrite | Zapisuje wynik do pliku (nadpisuje zawartość). |
| `>>` | Append | Dopisuje wynik na koniec pliku. |
| `2>` | Stderr | Przekierowuje tylko błędy (np. do `err.log`). |
| `&>` | All | Przekierowuje zarówno wynik, jak i błędy do jednego pliku. |
| `/dev/null` | Black Hole | "Wrzucenie do kosza" – wycisza niechciane błędy lub wyniki. |

---

## 4. Potoki i Podgląd (|, tail)
Łączenie narzędzi w łańcuchy.

| Narzędzie | Nazwa | Opis |
| :--- | :--- | :--- |
| `\|` | Pipe (Potok) | Przekazuje wynik lewej komendy jako dane dla prawej. |
| `tail -f` | Follow | Podgląd końcówki pliku na żywo (idealne do monitorowania logów). |
| `grep` | Global search | Filtruje tekst w poszukiwaniu konkretnej frazy. |

# Uprawnienia specjalne w Linux (SUID, SGID, Sticky Bit)

W systemach Linux, oprócz standardowych uprawnień `rwx`, istnieją trzy bity specjalne, które zmieniają zachowanie systemu wobec plików i katalogów.

---

## 1. Tabela wartości i symboli

Aby obliczyć wartość numeryczną uprawnień, dodajemy wybraną cyfrę jako **czwartą (pierwszą od lewej)** przed standardowym kodem (np. `0755` -> `4755`).

| Nazwa | Wartość | Symbol | Działanie (Plik) | Działanie (Katalog) |
| :--- | :---: | :---: | :--- | :--- |
| **SUID** (Set User ID) | **4** | `u+s` | Program działa z uprawnieniami **właściciela** pliku. | Brak znaczącego wpływu. |
| **SGID** (Set Group ID) | **2** | `g+s` | Program działa z uprawnieniami **grupy** pliku. | Nowe pliki wewnątrz **dziedziczą grupę** tego katalogu. |
| **Sticky Bit** | **1** | `+t` | Brak znaczącego wpływu. | Tylko **właściciel pliku** może go usunąć z tego katalogu. |

---

## 2. Metody nadawania uprawnień

### Metoda numeryczna (4-cyfrowa)
Suma bitów specjalnych tworzy pierwszą cyfrę:
* `chmod 4755` -> SUID + rwxr-xr-x
* `chmod 2775` -> SGID + rwxrwxr-x
* `chmod 1777` -> Sticky Bit + rwxrwxrwx (częste dla folderów współdzielonych)
* **Zadanie:** Aby nadać wszystkie trzy: $4+2+1=7$.
  `sudo chmod 7755 /home/bob/datadir`

### Metoda symboliczna
Można dodawać bity bez zmieniania reszty uprawnień:
* `chmod u+s plik`
* `chmod g+s katalog`
* `chmod +t katalog`
* **Zadanie:** `sudo chmod u+s,g+s,+t /home/bob/datadir`

---

## 3. Jak interpretować wynik w `ls -l`?

Uprawnienia specjalne "zajmują" miejsce litery `x` (execute). Wielkość litery informuje nas o tym, czy prawo wykonywania jest również aktywne:

| Widok | Znaczenie | Stan bitu 'x' |
| :--- | :--- | :--- |
| **s / t** (małe) | Bit specjalny jest aktywny. | **OK:** Prawo `x` jest nadane. |
| **S / T** (duże) | Bit specjalny jest ustawiony, ale nie działa. | **BŁĄD:** Brak prawa `x`. |



---

## 4. Praktyczne zastosowania (Przykłady)

1.  **SUID na `/usr/bin/passwd`**: Pozwala zwykłemu użytkownikowi zmienić własne hasło, co wymaga zapisu do chronionego pliku `/etc/shadow`.
2.  **SGID na `/home/zespol/projekt`**: Gwarantuje, że każdy plik stworzony przez Annę będzie dostępny dla Marka, bo oba pliki będą miały tę samą grupę projektową.
3.  **Sticky Bit na `/tmp`**: Każdy może tam tworzyć pliki, ale nikt nie może złośliwie usunąć plików należących do innych osób.

---

## 5. Podsumowanie zadania
Aby nadać **SUID**, **SGID** oraz **Sticky Bit** na katalogu `/home/bob/datadir`, wykonaj:
```bash
sudo chmod 7775 /home/bob/datadir
# lub
sudo chmod u+s,g+s,+t /home/bob/datadir