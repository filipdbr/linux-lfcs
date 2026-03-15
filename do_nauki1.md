📒 Notatki z Basha: Pętle i Masowa Zmiana Nazw

1. Pętla FOR (Fundament)
Używamy jej, gdy chcemy wykonać tę samą czynność na wielu plikach po kolei.
Bash

for plik in *.txt; do
    # tutaj wpisujemy co zrobić z każdym plikiem
done

2. Wycinanie rozszerzeń (${plik%.txt})
To sprytny sposób Basha, żeby "obrać" plik z końcówki:

    Jeśli zmienna $plik to config1.txt

    To zapis ${plik%.txt} wycina .txt i zostawia samo config1.

    Dzięki temu możesz dopisać datę przed rozszerzeniem, a nie za nim.

3. Magiczna Komenda Date

    $(date +%F) – ten zapis wstawia aktualną datę w formacie RRRR-MM-DD. Bash najpierw wykonuje to, co jest w środku nawiasów, a potem wkleja wynik do komendy.

4. Na co uważać? (Pułapki)

    Klamry: ${...} to całość. Nie stawiaj dodatkowego { przed dolarem (Twój ostatni błąd!).

    Cudzysłowy: Zawsze pisz "$plik", bo jeśli w nazwie pliku trafi się spacja, bez cudzysłowu pętla się "wywali".

5. Złota zasada: TESTUJ Z ECHO
Zanim użyjesz mv (które jest nieodwracalne), dopisz przed nim słowo echo:
echo mv "$f" "${f%.txt}_data.txt"
Zabaczysz w konsoli tekstowo, co by się stało, ale nic nie zostanie zmienione. To najlepszy sposób na naukę bez stresu!