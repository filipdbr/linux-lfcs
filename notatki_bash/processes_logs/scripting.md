# 🐚 Shell Scripting (Automatyzacja zadań)

Skrypty w Bashu pozwalają łączyć wiele komend w jedno narzędzie. Na egzaminie skup się na strukturze, zmiennych i uprawnieniach.

---

## 1. Anatomia skryptu
Każdy skrypt musi zawierać dwa elementy, aby system wiedział, jak go uruchomić:

1.  **Shebang (`#!`)**: Pierwsza linia wskazująca interpreter.
    * `#!/bin/bash` (najczęstszy wybór).
2.  **Uprawnienia do wykonywania**:
    * `chmod +x myscript.sh`

---

## 2. Zmienne i Podstawianie Komend
Zmienne pozwalają przechowywać dane, a "Command Substitution" pozwala zapisywać wyniki komend do zmiennych.

* **Definiowanie**: `NAME="Serwer-01"` (bez spacji wokół `=`).
* **Użycie**: `echo $NAME`.
* **Wynik komendy do zmiennej**:
    * `DATA=$(date +%F)`
    * `PLIKI=$(ls /etc | wc -l)`



---

## 3. Argumenty wejściowe (Positional Parameters)
Możesz przekazywać dane do skryptu podczas jego uruchamiania:
`./script.sh arg1 arg2`

* `$0` – Nazwa skryptu.
* `$1`, `$2`... – Pierwszy, drugi argument.
* `$#` – Liczba przekazanych argumentów.
* `$*` – Wszystkie argumenty jako jeden tekst.

---

## 4. Instrukcje warunkowe (IF)
Pozwalają skryptowi podejmować decyzje. Pamiętaj o spacjach wewnątrz nawiasów `[ ]`!

```bash
if [ $1 -gt 100 ]; then
    echo "Liczba jest większa niż 100"
else
    echo "Liczba jest mała"
fi