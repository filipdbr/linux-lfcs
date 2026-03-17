# Dzień 3: Zarządzanie użytkownikami i grupami

## 1. Tworzenie i usuwanie kont (useradd, userdel)
Podstawowe komendy administracyjne do zarządzania tożsamościami w systemie.

**Składnia:** `sudo useradd [opcje] nazwa_użytkownika`

| Opcja | Nazwa | Opis |
| :--- | :--- | :--- |
| `-m` | Create Home | Tworzy katalog domowy użytkownika w `/home/nazwa`. |
| `-s` | Shell | Określa domyślną powłokę (np. `-s /bin/bash`). |
| `-g` | Primary Group | Przypisuje użytkownika do głównej grupy (zazwyczaj o tej samej nazwie). |
| `-G` | Groups | Dodaje użytkownika do grup dodatkowych (po przecinku). |
| `-r` | System User | Tworzy konto systemowe (zazwyczaj bez folderu domowego). |
| `userdel -r` | Remove All | Usuwa użytkownika wraz z jego katalogiem domowym. |

> **Przykłady:**
> - `sudo useradd -m junior_admin` (Tworzy usera z folderem domowym).
> - `sudo userdel -r junior_admin` (Usuwa usera całkowicie z plikami).

---

## 2. Zarządzanie grupami i modyfikacja (groupadd, usermod)
Grupy pozwalają na masowe nadawanie uprawnień wielu użytkownikom do tych samych zasobów.

**Składnia:** `sudo usermod [opcje] nazwa_użytkownika`

| Opcja/Komenda | Nazwa | Opis |
| :--- | :--- | :--- |
| `groupadd` | Add Group | Tworzy nową grupę w systemie (np. `sudo groupadd devops`). |
| `-aG` | Append Group | Dodaje usera do grupy dodatkowej (**kluczowe: -a zapobiega usunięciu z obecnych grup!**). |
| `groups` | List Groups | Wyświetla grupy, do których należy dany użytkownik. |
| `id` | Identity | Wyświetla szczegółowe UID (User ID) i GID (Group ID) użytkownika. |
| `passwd` | Password | Ustawia lub zmienia hasło użytkownika (np. `sudo passwd junior_admin`). |

przykład usermod:
```bash
sudo usermod -aG devops junior_admin
```
komentarz: Gdybyś zapomniał o -a, system usunąłby go ze wszystkich innych grup (np. z grupy sudo) i zostawił tylko w devops

wyświetlanie do jakich grup nalezy user:
```bash
groups LOGIN
```
lub bardziej szczegółowo:
```bash
id LOGIN
```

---

## 3. Zarządzanie tożsamością (su, sudo, whoami)
Służy do przełączania się między kontami i weryfikacji aktualnych uprawnień.

**Składnia:** `su - [użytkownik]`

| Komenda | Nazwa | Opis |
| :--- | :--- | :--- |
| `whoami` | Who am I | Wyświetla nazwę użytkownika, na którym aktualnie pracujesz. |
| `su -` | Switch User | Przełącza na innego usera (flaga `-` ładuje jego zmienne środowiskowe). |
| `sudo` | SuperUser Do | Wykonuje pojedyncze polecenie z uprawnieniami administratora (root). |
| `exit` | Exit | Powrót do poprzedniej tożsamości lub zamknięcie sesji terminala. |

---

## 4. Kluczowe pliki systemowe (Baza użytkowników)
Linux przechowuje informacje o kontach w prostych plikach tekstowych.

| Plik | Zawartość | Struktura (uproszczona) |
| :--- | :--- | :--- |
| `/etc/passwd` | Dane kont | `użytkownik:x:UID:GID:opis:katalog:powłoka` |
| `/etc/group` | Dane grup | `nazwa_grupy:x:GID:lista_użytkowników` |
| `/etc/shadow` | Hasła | Przechowuje zaszyfrowane hasła (dostęp tylko dla roota). |

# todo
jeszcze poćwiczyć pracę z userami i przypomnieć sobie te poprzednie rzeczy