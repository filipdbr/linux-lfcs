## 1. Wyszukiwanie po czasie (mtime, ctime, atime)
Linux śledzi trzy rodzaje czasu dla każdego pliku. Możesz szukać plików, które zmieniły się dokładnie `n` dni temu, więcej niż `+n` lub mniej niż `-n`.

* **-mtime n** (Modification time): Zmiana treści pliku.
* **-ctime n** (Change time): Zmiana metadanych (np. uprawnień, nazwy) lub treści.
* **-atime n** (Access time): Ostatnie otwarcie/odczytanie pliku.

**Przykłady:**
* `find . -mtime -1` – zmienione w ciągu ostatnich 24h.
* `find . -ctime +30` – metadane zmienione dawniej niż 30 dni temu.
* `find . -mmin -60` – treść zmieniona w ciągu ostatnich 60 minut (mmin = minuty).

---

## 2. Wyszukiwanie po uprawnieniach (perm)
Możesz szukać plików z dokładnie takimi uprawnieniami, jakie podasz, lub sprawdzić, czy dane bity są ustawione.

* **-perm 644** – szuka plików, które mają dokładnie uprawnienia 644.
* **-perm -644** – szuka plików, które mają **co najmniej** te uprawnienia (muszą mieć r dla właściciela/grupy i r dla reszty, inne bity też mogą być ustawione).
* **-perm /222** – szuka plików, które są zapisywalne przez **kogokolwiek** (właściciela LUB grupę LUB innych).
* **-perm -4000** – szuka plików z ustawionym bitem SUID.
* **-perm -2000** – szuka plików z ustawionym bitem SGID.

---

## 3. Rozmiar i typ pliku
* **-size [+/-]rozmiar** – szuka po wielkości: `k` (kB), `M` (MB), `G` (GB).
    * `find /var/log -size +100M` – pliki większe niż 100MB.
* **-type [typ]** – ogranicza wyniki do konkretnego rodzaju obiektu:
    * `f` – zwykły plik.
    * `d` – katalog.
    * `l` – link symboliczny.

---

## 4. Właściciele i Grupy
* **-user username** – pliki należące do konkretnego użytkownika.
* **-group groupname** – pliki należące do konkretnej grupy.
* **-nouser** / **-nogroup** – szuka "osieroconych" plików, których właściciel nie istnieje już w systemie.

---

## 5. Łączenie warunków (Logika)
Możesz tworzyć złożone zapytania:
* **-and** (domyślne): `find . -type f -name "*.sh"` (plik i kończy się na .sh).
* **-or**: `find . -name "*.jpg" -or -name "*.png"`.
* **-not** / **!**: `find . -type f -not -name "*.txt"` (wszystkie pliki poza .txt).

---

## 6. Wykonywanie akcji na wynikach
Zamiast tylko wyświetlać listę, możesz od razu coś z tymi plikami zrobić.

* **-delete** – usuwa znalezione pliki (używaj ostrożnie!).
* **-exec [komenda] {} \;** – wykonuje komendę na każdym pliku. `{}` to miejsce na nazwę pliku, a `\;` kończy polecenie.
    * `find . -type f -perm 777 -exec chmod 644 {} \;` – znajdź pliki z uprawnieniami 777 i zmień je na 644.

# Zaawansowana Logika Polecenia find

W `find` parametry takie jak `-perm` (uprawnienia) lub `-size` (rozmiar) mogą przyjmować trzy formy zapisu, które drastycznie zmieniają wynik wyszukiwania.

---

## 1. Operatory Prefiksów (Klucz do precyzji)

Dla przykładu użyjmy uprawnień numerycznych `644` (rw-r--r--):

| Prefiks | Przykład | Znaczenie (Logika) |
| :--- | :--- | :--- |
| **Brak** | `-perm 644` | **Dokładnie** 644. Ani mniej, ani więcej. |
| **Minus (-)** | `-perm -644` | **Co najmniej** te bity (AND). Plik 755 też pasuje, bo zawiera w sobie 644. |
| **Slash (/)** | `-perm /644` | **Którykolwiek** z tych bitów (OR). Znajdzie wszystko, co ma chociaż r lub w gdziekolwiek. |

> [!TIP]
> Prefiks `/` (lub w starszych systemach `+`) jest idealny do szukania "zagrożeń", np. `-perm /o+w` (ktokolwiek z 'others' może pisać).

---

## 2. Operatory Logiczne (Łączenie warunków)

Jeśli podajesz kilka flag jedna po drugiej, `find` domyślnie łączy je operatorem **AND**.

* **AND (domyślny)**: `find -type f -name "*.log"`
  (Musi być plikiem **I** musi mieć rozszerzenie .log).
* **-or (LUB)**: `find -name "*.sh" -or -name "*.py"`
  (Znajdzie albo skrypty Bash, **ALBO** Pythona).
* **! (NIE / Negacja)**: `find -type d ! -perm -g+w`
  (Znajdź katalogi, które **NIE** mają prawa zapisu dla grupy).

---

## 3. Praca na czasie: mtime, ctime, atime

Tutaj operatory `+` i `-` działają jak oś czasu:



* **-mtime +7**: Zmodyfikowane **dawniej** niż 7 dni temu (starsze niż tydzień).
* **-mtime -7**: Zmodyfikowane **w ciągu** ostatnich 7 dni (nowsze niż tydzień).
* **-mtime 7**: Zmodyfikowane **dokładnie** 7 dni temu (co do doby).

---

## 4. Symbole specjalne w akcjach (-exec)

Kiedy używasz `-exec`, musisz użyć dwóch specyficznych znaków:

* **`{}` (Klamry)**: To "placeholder". `find` wstawia tam ścieżkę do każdego znalezionego pliku po kolei.
* **`\;` (Średnik)**: Mówi systemowi: "Tu kończy się komenda, którą masz wykonać". Backslash `\` jest konieczny, aby powłoka (Bash) nie zinterpretowała średnika jako końca linii w terminalu.

---

## 5. Przykłady "z życia" (Analiza zadania)

**Zadanie:** Grupa może pisać (`-g+w`), inni nie mogą czytać ani pisać (`! -perm /o+rw`).

```bash
find /var/log -type f -perm -g+w ! -perm /o+rw
```

# LFCS: Analiza danych i strumienie (find + wc)

* **Zliczanie wyników:**
  `find [warunki] | wc -l` – policzy ile obiektów spełniło kryteria.

* **Filtrowanie uprawnień dla 'others':**
  * `-perm /o+r` – inni mogą czytać.
  * `-perm /o+w` – inni mogą pisać (niebezpieczne!).
  * `-perm /o+x` – inni mogą wykonywać.

* **Szukanie plików SUID (system-wide):**
  `sudo find / -type f -perm -4000 2>/dev/null`
  *Wyjaśnienie:* - `sudo` i `/` – szukamy w całym systemie.
  - `-4000` – szukamy bitu SUID.
  - `2>/dev/null` – ukrywamy błędy braku uprawnień do folderów systemowych.
  - `> plik.txt` – zapisujemy tylko czystą listę znalezionych plików.