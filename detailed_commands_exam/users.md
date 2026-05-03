# 👤 Zarządzanie Użytkownikami (LFCS)

usera dodaje useradd

- **Tworzenie użytkownika z katalogiem i grupą:**
  `sudo useradd -m -G grupa_dodatkowa -c "Komentarz" nazwa_usera`
- **Ustawianie hasła (metoda nieinteraktywna - polecana):**
  `echo "user:haslo" | sudo chpasswd`
  albo prościej sudo passwd nazwa_usera
- **Modyfikacja istniejącego usera:**
  `sudo usermod -aG kolejna_grupa nazwa_usera` (Pamiętaj o `-a` - append, inaczej zastąpisz wszystkie grupy tylko tą jedną!).