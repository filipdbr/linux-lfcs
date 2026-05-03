# 🔍 Zaawansowane wyszukiwanie: Komenda `find`

Składnia: `find [ścieżka] [kryteria] [akcja]`

## 1. Wyszukiwanie po ROZMIARZE (`-size`)
To najczęstszy motyw na egzaminie. Pamiętaj o jednostkach: `k` (kB), `M` (MB), `G` (GB).

- **Dokładnie 100MB:**
  `find / -size 100M`
- **Większe niż 100MB:**
  `find / -size +100M`
- **Mniejsze niż 100MB:**
  `find / -size -100M`
- **Zakres (np. większe niż 10MB i mniejsze niż 50MB):**
  `find / -size +10M -size -50M`

## 2. Wyszukiwanie po CZASIE (`-mtime`, `-atime`, `-ctime`)
- `mtime` (modification): zmiana zawartości pliku.
- `atime` (access): odczytanie pliku.
- `ctime` (change): zmiana metadanych (np. uprawnień).

- **Dokładnie 2 dni temu:**
  `find /var/log -mtime 2`
- **Dawniej niż 5 dni temu (starsze):**
  `find /var/log -mtime +5`
- **W ciągu ostatnich 24 godzin (nowsze):**
  `find /var/log -mtime -1`
- **W ciągu ostatnich 60 minut (w minutach):**
  `find /etc -mmin -60`



## 3. Wyszukiwanie po UPRAWNIENIACH (`-perm`)
- **Dokładnie 777:**
  `find / -perm 777`
- **Przynajmniej te bity (np. każdy, który ma 'write' dla grupy):**
  `find / -perm -020`
- **Pliki z bitem SUID (częste na egzaminie z security):**
  `find /usr/bin -perm /4000`

## 4. Wyszukiwanie po TYPIE (`-type`)
- **Tylko pliki:** `find /etc -type f`
- **Tylko katalogi:** `find /etc -type d`
- **Tylko dowiązania symboliczne:** `find /etc -type l`

## 5. Łączenie warunków (Operatory Logiczne)
- **AND (I) - domyślne:**
  `find /etc -type f -name "*.conf"` (musi być plikiem ORAZ kończyć się na .conf)
- **OR (LUB) - flaga `-o`:**
  `find /etc -name "*.conf" -o -name "*.txt"`
- **NOT (ZAPRZECZENIE) - flaga `!`:**
  `find /etc -type f ! -name "*.conf"` (wszystkie pliki, które NIE kończą się na .conf)

## 6. Automatyzacja akcji (`-exec` vs `-delete`)
- **Usuwanie znalezionych plików (niebezpieczne, ale szybkie):**
  `find /tmp -type f -mtime +10 -delete`
- **Zmiana właściciela dla wyników:**
  `find /home/bob -user root -exec chown bob:bob {} +`
- **Kopiowanie wyników do katalogu (częste zadanie):**
  `find /etc -name "*.conf" -exec cp {} /root/backup/ \;`



## 7. Wyszukiwanie "zepsutych" plików
- **Pliki bez właściciela (po usuniętym userze):**
  `find / -nouser`
- **Puste pliki lub katalogi:**
  `find /data -empty`

  # 🔑 Prefiksy uprawnień w find

- **Brak prefiksu** (`777`): Dokładnie takie uprawnienia.
- **Prefiks minus** (`-777`): Musi mieć **wszystkie** te bity (Minimum).
- **Prefiks slash** (`/777`): Musi mieć **chociaż jeden** z tych bitów (Którykolwiek).

*Tip egzaminacyjny:* Jeśli każą Ci znaleźć "pliki wykonywalne przez kogokolwiek", użyj:
`find . -perm /111`

# 📂 Masowe operacje na plikach (Find + Exec)

Klucz do sukcesu na LFCS to łączenie wyszukiwania z działaniem.

- **Kopiowanie wielu plików naraz (najszybsze):**
  `find /src -type f -name "*.conf" -exec cp -t /dst/ {} +`
- **Zmiana uprawnień tylko dla katalogów:**
  `find /var/www -type d -exec chmod 755 {} +`
- **Usuwanie starych logów:**
  `find /var/log -name "*.log" -mtime +30 -delete`

**Pamiętaj:** Zawsze testuj `find` bez `-exec` zanim uruchomisz wersję niszczącą (np. z `rm` lub `-delete`), żeby upewnić się, że lista plików jest poprawna!