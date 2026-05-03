# 🐧 Ściągawka: Składnia i Operatory Bash

Notatka dotycząca kluczowych mechanizmów używania znaku dolara `$`, nawiasów oraz obliczeń matematycznych.

---

## 1. Magia Dolara ($) - Podstawienie

| Symbol | Nazwa | Funkcja | Przykład |
| :--- | :--- | :--- | :--- |
| `$zmienna` | **Variable Expansion** | Wyciąga wartość zapisany w zmiennej. | `echo $IMIE` |
| `${zmienna}` | **Parameter Expansion** | Bezpieczniejsza forma `$`. Pozwala łączyć zmienną z tekstem bez spacji. | `echo "${plik}_backup"` |
| `$(komenda)` | **Command Substitution** | Wykonuje komendę i zwraca jej wynik (zastępuje backticksy `` ` ``). | `data=$(date)` |
| `$((wyrażenie))`| **Arithmetic Expansion**| Wykonuje obliczenia na liczbach całkowitych. | `suma=$((2+2))` |

---

## 2. Porównywanie (Testowanie)

W Bashu używamy różnych operatorów dla tekstu i liczb. Używaj ich wewnątrz `if [ ... ]` lub `if [[ ... ]]`.

### A. Liczby (Integers)
*   `-eq` : Równe (Equal)
*   `-ne` : Nierówne (Not Equal)
*   `-lt` : Mniejsze niż (Less Than)
*   `-le` : Mniejsze lub równe (Less or Equal)
*   `-gt` : Większe niż (Greater Than)
*   `-ge` : Większe lub równe (Greater or Equal)

### B. Tekst (Strings)
*   `=` lub `==` : Takie same
*   `!=` : Różne
*   `-z` : Ciąg jest pusty
*   `-n` : Ciąg NIE jest pusty

---

## 3. Matematyka w Bashu

### Liczby całkowite (wbudowane)
Używamy `$(( ... ))`. Obsługuje: `+`, `-`, `*`, `/`, `%` (modulo).
```bash
wynik=$((10 / 3)) # Wynik: 3 (Bash ucina ułamki!)