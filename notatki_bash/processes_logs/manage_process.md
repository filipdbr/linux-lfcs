# 🔍 Diagnose and Manage Processes

Zarządzanie procesami to kontrola nad zasobami systemu (CPU, RAM). Jako administrator musisz umieć monitorować aktywność i interweniować, gdy procesy działają nieprawidłowo.

---

## 1. Monitorowanie w czasie rzeczywistym
Zanim podejmiesz działanie, musisz wiedzieć, co się dzieje.

| Komenda | Opis |
| :--- | :--- |
| `top` | Standardowe narzędzie. Pokazuje zużycie CPU, RAM i listę procesów. |
| `htop` | Bardziej czytelna, kolorowa wersja `top` (jeśli dostępna, używaj jej!). |
| `ps aux` | Wyświetla statyczną listę wszystkich procesów wszystkich użytkowników. |
| `ps -ef` | Podobne do `aux`, ale pokazuje relacje rodzic-dziecko (PPID). |



---

## 2. Zabijanie procesów (Signals)
Jeśli proces się zawiesił lub "zjada" 100% CPU, musisz go zatrzymać. Służą do tego sygnały.

* **`kill [PID]`**: Wysyła domyślny sygnał **SIGTERM (15)**. Prosi proces o bezpieczne zamknięcie się.
* **`kill -9 [PID]`**: Wysyła **SIGKILL (9)**. Brutalne ubicie procesu (używaj tylko w ostateczności!).
* **`kill -HUP [PID]`**: Wysyła **SIGHUP (1)**. Prosi proces o przeładowanie konfiguracji.
* **`pkill [nazwa]`**: Zabija procesy po nazwie zamiast PID (np. `pkill firefox`).
* **`killall [nazwa]`**: Zabija wszystkie wystąpienia procesu o danej nazwie.



---

## 3. Priorytety (Nice i Renice)
Możesz zdecydować, który proces jest ważniejszy dla procesora. Skala wynosi od **-20** (najwyższy priorytet) do **19** (najniższy).

* **`nice -n 10 [komenda]`**: Uruchamia nowy proces z niższym priorytetem.
* **`renice -n -5 -p [PID]`**: Zmienia priorytet już działającego procesu (tylko root może zwiększać priorytet, czyli nadawać wartości ujemne).

---

## 4. Narzędzia diagnostyczne (Głęboka analiza)
Gdy `top` to za mało, użyj tych komend:

| Komenda | Co diagnozuje? |
| :--- | :--- |
| `lsof -p [PID]` | Jakie pliki i gniazda sieciowe ma otwarte dany proces? |
| `strace -p [PID]` | Jakie wywołania systemowe (syscalls) wykonuje proces w tej chwili? |
| `free -h` | Szybki podgląd zajętości pamięci RAM i Swap. |
| `uptime` | Obciążenie systemu (Load Average) z ostatnich 1, 5 i 15 minut. |



---

## 5. Praca w tle (Job Control)
Czasami chcesz uruchomić skrypt i odzyskać terminal:

* **`komenda &`**: Uruchamia proces od razu w tle.
* **`jobs`**: Pokazuje listę zadań uruchomionych w bieżącej sesji terminala.
* **`fg %[numer]`**: Przywraca zadanie z tła na pierwszy plan (*foreground*).
* **`bg %[numer]`**: Wznawia zatrzymane zadanie w tle (*background*).
* **Ctrl + Z**: Zatrzymuje (pauzuje) bieżący proces i wrzuca go do listy `jobs`.

---

## 💡 LFCS Pro-Tips:
1. **Load Average**: Jeśli `uptime` pokazuje wartości wyższe niż liczba rdzeni Twojego procesora, system jest przeciążony.
2. **Zombie Processes**: W `top` możesz zobaczyć procesy "zombie" (oznaczone jako `Z`). To procesy, które zakończyły działanie, ale ich rodzic nie odebrał informacji o zakończeniu. Nie zajmują CPU, ale zajmują miejsce w tablicy procesów.
3. **Pgrep**: Zamiast `ps aux | grep ssh`, używaj szybszego `pgrep -l ssh`.