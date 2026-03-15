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

## 3. Sygnały i kończenie procesów (kill)
Zamykanie procesów odbywa się poprzez wysyłanie do nich sygnałów.

**Składnia:** `kill -[SYGNAŁ] PID`

| Sygnał (Numer) | Nazwa | Opis |
| :--- | :--- | :--- |
| `15` | SIGTERM | **Grzeczne zamknięcie.** Daje procesowi czas na zapisanie danych i wyjście. |
| `9` | SIGKILL | **Bezwzględne ubicie.** System natychmiast wycina proces z pamięci. |
| `1` | SIGHUP | **Restart:** Często używany do przeładowania konfiguracji bez wyłączania usługi. |

> **Przykład:** `kill -9 1234` (Natychmiast ubiij proces o numerze 1234).

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