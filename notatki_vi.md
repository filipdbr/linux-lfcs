## Edytor vi (Vim)

**Podstawowa zasada:** Edytor ma dwa główne tryby: **wydawania poleceń** oraz **wprowadzania tekstu**. Przełączasz się między nimi klawiszem `Esc` (powrót do poleceń) oraz komendami takimi jak `i` lub `a` (wejście w tryb tekstowy).

### 1. Wyszukiwanie tekstu
*Aby szukać, musisz być w trybie wydawania poleceń.*

| Komenda | Nazwa | Opis |
| :--- | :--- | :--- |
| `/fraza` | Search down | Poszukiwanie wzorca w przód od pozycji kursora. |
| `?fraza` | Search up | Poszukiwanie wzorca w tył od pozycji kursora. |
| `n` | Next | Kontynuacja poszukiwania w tym samym kierunku. |
| `N` | Next reverse | Kontynuacja poszukiwania w przeciwnym kierunku. |

> **Pro Tip:** Jeśli najechałeś kursorem na słowo i chcesz znaleźć inne takie same, naciśnij gwiazdkę `*`. vi automatycznie zacznie szukać tego słowa w całym pliku.

---

### 2. Edycja (Tryb "Szybkie palce")

| Komenda | Nazwa | Opis |
| :--- | :--- | :--- |
| `dd` | Delete line | Usuwa bieżący wiersz (wycina go do schowka). |
| `dw` | Delete word | Usunięcie wyrazu od pozycji kursora. |
| `u` | Undo | Wycofanie zmian - powrót do stanu sprzed ostatniego polecenia. |
| `Ctrl + r` | Redo | Przywrócenie zmiany po "undo" (dostępne w vim). |
| `yy` | Yank | Kopiowanie do bufora (schowka) bieżącego wiersza. |
| `p` | Paste | Wstawienie usuniętego/skopiowanego tekstu za kursorem. |

---

### 3. Poruszanie się (Bez użycia strzałek)

| Komenda | Nazwa | Opis |
| :--- | :--- | :--- |
| `gg` | Top | Przejście na sam początek pliku. |
| `G` | Bottom | Przejście do ostatniego wiersza w pliku. |
| `0` | Start of line | Skok na początek wiersza. |
| `$` | End of line | Skok na koniec wiersza. |
| `nG` | Go to line | Przejście bezpośrednio do n-tego wiersza (np. `42G`). |

---

### 4. Magia zamiany (Search & Replace)

**Składnia:** `:%s/stara_nazwa/nowa_nazwa/g`

| Symbol | Nazwa | Opis |
| :--- | :--- | :--- |
| `%` | All lines | Operacja na wszystkich wierszach w pliku. |
| `s` | Substitute | Polecenie zamiany wzorca. |
| `g` | Global | Zamiana wszystkich wystąpień w każdym wierszu (nie tylko pierwszego). |

---

### 5. Jak wyjść (Zapis i zakończenie)

| Komenda | Nazwa | Opis |
| :--- | :--- | :--- |
| `:w` | Write | Zapis zmian do bieżącego pliku. |
| `:q!` | Quit force | Wyjście z edytora bez zapisywania zmian. |
| `:wq` | Write & Quit | Zapisanie pliku i wyjście z edytora. |
| `ZZ` | Quick save/exit| Szybki zapis i wyjście (skrót klawiszowy). |

## Zaawansowany vi

# Notatki Vim: Komenda Zamiany (Substitute)

Podstawowa składnia:
`:[zakres]s/[wzorzec]/[zamiennik]/[flagi]`

---

## 1. Wyjaśnienie Składni
- `:` – Wejście w tryb komend.
- `s` – Skrót od *substitute* (zamień).
- `/ / /` – Separatory (można je zastąpić np. `#`, jeśli zmieniasz ścieżki do plików).

## 2. Zakresy (Gdzie szukać?)
| Symbol | Zakres działania |
| :--- | :--- |
| `%` | Cały plik (od góry do dołu). |
| `.` | Tylko bieżąca linia (domyślne). |
| `5,15` | Tylko linie od 5 do 15. |
| `.,$` | Od bieżącej linii do końca pliku. |

## 3. Flagi (Jak zamieniać?)
| Flaga | Opis |
| :--- | :--- |
| **brak** | Zamienia tylko **pierwsze** wystąpienie w każdej linii. |
| `g` | **Global** – zamienia wszystkie wystąpienia w linii. |
| `c` | **Confirm** – pyta o potwierdzenie przed każdą zmianą. |
| `i` | **Ignore case** – wielkość liter nie ma znaczenia. |
| `I` | Wymusza rozróżnianie wielkości liter. |

## 4. Praktyczne przykłady
- `:%s/stary/nowy/g` – Zamień wszystko w całym pliku.
- `:%s/stary/nowy/gc` – Zamień wszystko, ale pytaj o zgodę (y/n).
- `:s/pies/kot/` – Zamień tylko pierwszego psa w aktualnej linii na kota.
- `:%s#/usr/bin#/usr/local/bin#g` – Użycie `#` zamiast `/` przy ścieżkach.

## 5. Przydatne skróty po zamianie
- `u` – Cofnij zamianę (Undo).
- `Ctrl + r` – Ponów zamianę (Redo).
- `&` – Powtórz ostatnią komendę zamiany.$