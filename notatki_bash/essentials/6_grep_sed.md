# Przetwarzanie tekstu: Grep i Sed (Wersja Rozszerzona)

Narzędzia te są kluczowe przy analizie logów i automatyzacji edycji plików konfiguracyjnych.

### Grep (Wyszukiwanie i Filtrowanie)
- `grep -n` – wyświetla numer linii, w której znaleziono frazę (kluczowe przy dużych plikach).
- `grep -c` – zamiast linii, wyświetla tylko LICZBĘ znalezionych dopasowań (count).
- `grep -l` – wyświetla tylko nazwy plików, w których występuje fraza (zamiast samej treści).
- `grep -w` – szuka dokładnie całego słowa (nie znajdzie "root" w "rootless").
- `grep -f wzorce.txt plik` – pobiera listę fraz do wyszukania z innego pliku.

**Kontekst (Analiza logów):**
- `grep -C 2 "Error" log.txt` – pokazuje błąd oraz 2 linie przed i 2 linie po (Context).
- `grep -vE '^#|^$'` – potężne combo: usuwa komentarze i puste linie jednocześnie.

### Sed (Edycja Strumieniowa)
Sed operuje na liniach (tzw. pattern space). Możesz precyzyjnie wskazać, gdzie ma działać.

**Adresowanie (Wskazywanie linii):**
- `sed '3s/stary/nowy/' plik` – zamień tylko w 3. linii.
- `sed '1,10s/stary/nowy/g' plik` – zamień we wszystkich liniach od 1 do 10.
- `sed '/fraza/s/stary/nowy/g' plik` – zamień tylko w liniach, które zawierają słowo "fraza".
- `sed '$d' plik` – usuń ostatnią linię pliku ($ oznacza koniec).

**Wiele operacji naraz:**
- `sed -e 's/A/B/g' -e 's/C/D/g' plik` – wykonaj kilka zmian w jednym przebiegu.

**Triki z flagą -i (In-place):**
- `sed -i.bak 's/A/B/g' plik` – edytuje plik, ale ZAPISUJE kopię zapasową o nazwie `plik.bak` (bardzo bezpieczne na egzaminie!).

### Wyrażenia Regularne (Regex) - Szybka Ściąga
- `^` – początek linii (np. `^root` szuka linii zaczynających się od root).
- `$` – koniec linii (np. `bash$` szuka linii kończących się na bash).
- `.` – dowolny jeden znak.
- `*` – zero lub więcej wystąpień poprzedniego znaku.
- `[0-9]` – dowolna cyfra.

## Trick
Na egzaminie i w pracy warto dodać sobie alias lub używać flagi `--color=auto`. 