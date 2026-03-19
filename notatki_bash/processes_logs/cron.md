# 🕒 Harmonogramowanie zadań: Cron, Anacron i At

W Linuxie mamy trzy główne sposoby na automatyczne uruchamianie komend. Każdy służy do czegoś innego.

---

## 1. CRON (Zadania cykliczne - Serwery 24/7)
Używany do zadań, które mają się powtarzać w określonych odstępach czasu.

### Składnia Crontab:
`* * * * * komenda`
1. **Minuta** (0-59)
2. **Godzina** (0-23)
3. **Dzień miesiąca** (1-31)
4. **Miesiąc** (1-12)
5. **Dzień tygodnia** (0-7, gdzie 0 i 7 to niedziela)



### Przykłady wpisów:
- `30 4 * * *` -> Codziennie o 04:30.
- `0 12 * * 1` -> W każdy poniedziałek o 12:00.
- `*/15 * * * *` -> Co 15 minut.
- `0 8-17 * * 1-5` -> O pełnej godzinie, od 8:00 do 17:00, od poniedziałku do piątku.

**Komendy:**
- `crontab -e` : Edycja (zawsze używaj tego!).
- `crontab -l` : Lista zaplanowanych zadań.
- `crontab -r` : Usunięcie wszystkich zadań użytkownika.

# 🕒 Harmonogramowanie zadań: Cron - Poziom Expert

## 1. Uprawnienia i Wykonywanie (Chmod +x)
Aby Cron mógł uruchomić skrypt, plik musi być **wykonywalny**. Sama obecność w crontabie nie nada mu magii.

- **Nadanie uprawnień:** `chmod +x /home/filip/backup.sh`
- **Właściciel:** Skrypt powinien należeć do użytkownika, który go uruchamia (lub do roota, jeśli to zadanie systemowe).

## 2. Pułapka Rozszerzeń (.sh)
W specyficznych folderach systemowych, takich jak `/etc/cron.daily/`, `/etc/cron.weekly/` czy `/etc/cron.d/`, system używa narzędzia `run-parts`.
- **ZASADA:** Pliki w tych folderach **NIE MOGĄ mieć rozszerzeń** (np. `.sh`).
- ✅ Dobrze: `/etc/cron.daily/backup`
- ❌ Źle: `/etc/cron.daily/backup.sh` (Cron go po prostu zignoruje!)

---

## 3. Przykład: Skrypt + Crontab

### Krok 1: Tworzymy skrypt (`/home/filip/cleaner.sh`)
```bash
#!/bin/bash
# Używamy PEŁNYCH ścieżek do komend!
/usr/bin/rm -rf /tmp/stare_pliki/*
/usr/bin/echo "Czyszczenie wykonane: $(/usr/bin/date)" >> /home/filip/clean.log

---

## 2. ANACRON (Zadania cykliczne - Komputery wyłączane)
Różnica względem Crona: Jeśli komputer był wyłączony w czasie, gdy zadanie miało się odbyć, **Anacron uruchomi je zaraz po włączeniu systemu.**

- Nie operuje na minutach, tylko na **dniach**.
- Konfiguracja: `/etc/anacrontab`.
- Głównie używany przez system do rotacji logów (`logrotate`) i sprzątania `/tmp`.



---

## 3. AT (Zadania jednorazowe)
Używany, gdy chcesz coś uruchomić **tylko raz** w przyszłości.

### Jak używać:
1. Wpisz `at [czas]`, np. `at 15:30` lub `at tomorrow`.
2. Wpisz komendę, np. `tar -czf backup.tar.gz /home/filip`.
3. Naciśnij **Ctrl+D**, aby zapisać i wyjść.

**Komendy:**
- `atq` : Kolejka zaplanowanych zadań (lista).
- `atrm [ID]` : Usunięcie zadania z kolejki.

---

## 💡 LFCS Cheat Sheet: Pułapki
1. **Zmienne środowiskowe:** Cron nie wie, gdzie jest `tar` czy `python`. Zawsze używaj **pełnych ścieżek**: `/usr/bin/tar` zamiast `tar`.
2. **Puste linie:** Zawsze zostawiaj jedną pustą linię na końcu pliku crontab, inaczej ostatnie zadanie może się nie wykonać!
3. **Logowanie błędów:** Cron nie wyświetla błędów na ekranie. Przekieruj je do pliku:
   `0 2 * * * /sciezka/skrypt.sh > /tmp/cron_log.txt 2>&1`