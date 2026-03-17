# 🛡️ Kompendium Dokumentacji Systemowej (LFCS Survival Guide)

Na egzaminie nie masz dostępu do Internetu. Twoim jedynym źródłem wiedzy jest terminal. Oto jak wycisnąć z niego 100%.

---

## 1. Apropos – Gdy nie wiesz, jakiej komendy użyć
`apropos` przeszukuje krótkie opisy wszystkich stron pomocy. To Twoja "wyszukiwarka Google" w trybie offline.

* `apropos partition` – znajdź wszystkie komendy związane z partycjonowaniem.
* `apropos -s 8 .` – wyświetla listę **wszystkich** komend administracyjnych (sekcja 8).
* `man -k "change password"` – przeszukiwanie bazy podręcznika po frazach.

---

## 2. Man (Manual) – Pełna dokumentacja
Manual jest podzielony na sekcje. Najważniejsze dla administratora:
- **Sekcja 1**: Standardowe komendy (np. `ls`, `cat`).
- **Sekcja 5**: Formaty plików konfiguracyjnych (np. `/etc/fstab`, `/etc/passwd`).
- **Sekcja 8**: Komendy administracyjne (np. `systemctl`, `ip`, `fdisk`).

### Triki wewnątrz `man` (przy użyciu `less`):
Zanim zaczniesz, upewnij się, że używasz `less`: `export PAGER=less`.

| Skrót | Akcja |
| :--- | :--- |
| `/fraza` | Szukanie w dół. |
| `?fraza` | Szukanie w górę. |
| `-i` | **KLUCZOWE:** Przełącza ignorowanie wielkości liter (Case Insensitive). |
| `&fraza` | **Filtr:** Ukrywa wszystkie linie, które nie zawierają słowa. Idealne do szukania flag. |
| `G` | Skok na sam dół (zazwyczaj tam są **EXAMPLES**). |
| `g` | Skok na samą górę. |
| `n` / `N` | Następne / Poprzednie dopasowanie. |

### Szukanie flagi "z chirurgiczną precyzją":
Zamiast wpisywać `/ -p`, co znajdzie każdą literę 'p' w tekście, wpisz:
` /^ *-p`
(Szuka linii, która zaczyna się od dowolnej liczby spacji i znaku `-p`).

---

## 3. Metoda "Grep & Man" – Szybkie wyciąganie info
Jeśli nie chcesz wchodzić do manuala, użyj potoku. To najszybszy sposób na znalezienie składni.

* **Szukanie flagi z opisem:**
  `man journalctl | grep -i -A 5 "\-p"`
  (Wyświetla flagę `-p` oraz 5 linii pod nią – flaga `-A` to Context After).

* **Wyciąganie przykładów:**
  `man tar | grep -i -A 10 "EXAMPLES"`

---

## 4. Pomoc wbudowana (--help)
Zawsze pierwszy krok przed `man`. Jest krótsza i bardziej treściwa.
* `systemctl --help | grep "mask"` – szybkie upewnienie się co do nazwy flagi.

---

## 5. Whatis & Which – Identyfikacja
* `whatis [komenda]` – krótka definicja w jednej linii.
* `which [komenda]` – pokazuje pełną ścieżkę do pliku wykonywalnego (np. `/usr/bin/python3`).

---

## 💡 Pro-Tips na egzamin LFCS:
1. **Zapomniałeś składni pliku `/etc/fstab`?** Wpisz `man 5 fstab`.
2. **Zapomniałeś parametrów w `/etc/ssh/sshd_config`?** Wpisz `man 5 sshd_config`.
3. **Nie wiesz, jak zrestartować system?** `man 8 shutdown` lub `man 8 systemctl`.
4. **Tryb "Panic":** Jeśli `man` działa dziwnie, wpisz `export PAGER=less` i `export LANG=C` (wymusza angielską dokumentację, która jest najpełniejsza).