# Dzień 4: Procesy i Zarządzanie Zasobami

## 1. Monitorowanie na żywo (top)
Komenda `top` to dynamiczny podgląd tego, co dzieje się w systemie. Pozwala na interakcję za pomocą klawiszy bez wychodzenia z programu.

| Klawisz (w top) | Działanie |
| :--- | :--- |
| `1` | **Widok procesorów:** Rozbija zbiorcze zużycie CPU na poszczególne rdzenie. |
| `M` | **Sortuj po pamięci:** Ustawia procesy od tych, które zużywają najwięcej RAM. |
| `P` | **Sortuj po CPU:** Powrót do domyślnego sortowania według obciążenia procesora. |
| `k` | **Kill:** Pozwala ubić proces bezpośrednio z poziomu top (zapyta o PID). |
| `u` | **Filtr użytkownika:** Pokazuje procesy tylko wybranego usera. |
| `h` | **Pomoc:** Wyświetla listę wszystkich dostępnych skrótów. |
| `q` | **Wyjście:** Zamyka program. |

---

## 2. Statyczna lista procesów (ps)
Używamy, gdy chcemy "zrobić zdjęcie" procesom i np. przefiltrować je przez `grep`.

**Składnia:** `ps [opcje]`

| Opcja | Opis |
| :--- | :--- |
| `aux` | Najczęstszy zestaw: **a** (wszyscy userzy), **u** (szczegóły), **x** (procesy tła). |
| `-ef` | Alternatywny format (standard UNIX), pokazuje drzewo powiązań (PPID). |

**Kluczowe kolumny:**
- **PID:** Process ID (identyfikator procesu).
- **PPID:** Parent Process ID (kto uruchomił ten proces – "rodzic").
- **%CPU / %MEM:** Procentowe zużycie zasobów.
- **STAT:** Stan procesu (np. `S` – śpi, `R` – działa, `Z` – zombie).

---

## 3.Zarządzanie Procesami: Tabela Sygnałów (Signals)

Sygnały to sposób komunikacji z procesami. Można je wysyłać za pomocą komendy `kill`, `pkill` lub skrótów klawiszowych.

| Numer | Nazwa | Skrót | Opis i Zastosowanie |
| :--- | :--- | :--- | :--- |
| **1** | **SIGHUP** | - | **Restart/Hangup:** Przeładowanie konfiguracji procesu bez jego wyłączania. |
| **2** | **SIGINT** | `Ctrl+C` | **Przerwanie:** Grzeczne zatrzymanie procesu z klawiatury. |
| **3** | **SIGQUIT**| `Ctrl+\` | **Wyjście:** Zamknięcie procesu z wygenerowaniem zrzutu pamięci (core dump). |
| **9** | **SIGKILL** | - | **Zabójstwo:** Natychmiastowe i bezwzględne ubicie. Proces nie może go zignorować. |
| **15** | **SIGTERM** | - | **Zakończenie:** Domyślny sygnał `kill`. Prośba o zamknięcie (pozwala zapisać dane). |
| **18** | **SIGCONT** | - | **Kontynuacja:** Wznowienie procesu, który został zatrzymany sygnałem STOP. |
| **19** | **SIGSTOP** | `Ctrl+Z` | **Zatrzymanie:** Pauza. Proces zostaje w pamięci, ale nie zużywa CPU. |

### Praktyczne przykłady:
- `kill -l` – wyświetla listę wszystkich dostępnych sygnałów.
- `kill -9 1234` – zabija proces o PID 1234 (używaj w ostateczności).
- `pkill -HUP nginx` – przeładowuje konfigurację Nginxa (użycie nazwy zamiast numeru).

---

## 4. Praca w tle i na pierwszym planie (Jobs)
Czasami chcesz uruchomić coś, co trwa długo, i nie blokować sobie terminala.

| Komenda/Skrót | Opis |
| :--- | :--- |
| `&` | Dodane na końcu komendy (np. `sleep 100 &`) uruchamia ją od razu w tle. |
| `CTRL + Z` | Zawiesza aktualnie działający proces i wyrzuca go do tła (stan: Stopped). |
| `jobs` | Wyświetla listę procesów działających w tle Twojej sesji. |
| `fg [nr]` | **Foreground:** Przywraca proces z tła na pierwszy plan. |
| `bg [nr]` | **Background:** Wznawia zawieszony proces, ale pozwala mu działać w tle. |

---

## 5. Inne przydatne narzędzia
- `htop` – Nowocześniejsza, kolorowa wersja `top` (jeśli masz, używaj zamiast top!).
- `pgrep [nazwa]` – Szybkie wyszukiwanie PID po nazwie (zamiast `ps aux | grep ...`).
- `pkill [nazwa]` – Zabija wszystkie procesy o danej nazwie.
- `uptime` – Pokazuje czas pracy systemu i **Load Average** (obciążenie z ostatnich 1, 5 i 15 minut).