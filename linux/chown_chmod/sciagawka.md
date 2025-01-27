## Na jakich plikach mozna operować chmod?

| **Typ**           | **Opis**                                                                         | **Przykłady**                    | **Uprawnienia `rwx`**                                                                  |
| ----------------- | -------------------------------------------------------------------------------- | -------------------------------- | -------------------------------------------------------------------------------------- |
| **Pliki zwykłe**  | Zwykłe pliki (tekstowe, binarne, obrazy itp.).                                   | `file.txt`, `script.sh`          | Decydują o możliwości czytania, modyfikacji i wykonania pliku.                         |
| **Foldery**       | Katalogi przechowujące inne pliki i foldery.                                     | `~/Documents`, `/var/log`        | Decydują o dostępie do zawartości folderu (np. lista plików, zmiana zawartości).       |
| **Linki**         | Linki symboliczne (symlinki) – wskaźniki do innych plików/folderów.              | `ln -s /original/path link_name` | Nie mają własnych uprawnień – dziedziczą uprawnienia pliku/folderu, na który wskazują. |
| **Urządzenia**    | Pliki reprezentujące urządzenia sprzętowe (np. dyski, porty USB).                | `/dev/sda`, `/dev/ttyUSB0`       | Kontrolują dostęp do urządzenia (np. odczyt/zapis na dysku).                           |
| **Sockety**       | Pliki służące do komunikacji między procesami (IPC).                             | `/run/systemd/journal/socket`    | Decydują, kto może nawiązać połączenie.                                                |
| **Potoki (FIFO)** | Specjalne pliki umożliwiające komunikację "pierwszy wchodzi, pierwszy wychodzi". | Utworzone przez `mkfifo`         | Kontrolują dostęp do zapisu/odczytu z potoku.                                          |

---

## Działanie rwx w zależności od pliku

| **Typ**          | **r (read/odczyt)**                             | **w (write/zapis)**                                                                        | **x (execute/wykonanie)**                    |
| ---------------- | ----------------------------------------------- | ------------------------------------------------------------------------------------------ | -------------------------------------------- |
| **Pliki zwykłe** | Podejrzenie zawartości pliku, np. `im_stuck.sh` | Możliwość edycji i zapisania zmian w pliku, np. dopisanie linijki do skryptu `im_stuck.sh` | Możliwość uruchomienia skryptu `im_stuck.sh` |
| **Foldery**      | Podejrzenie zawartości folderu `praca_domowa`   | Możliwość utworzenia plików w folderze `praca_domowa`                                      | Możesz wejśc do folderu `praca_domowa`       |

---

## Ściągawka: `chmod`

### **Składnia:**

```bash
chmod [opcje] <tryb> <plik/folder>
```

### Tryby

#### Symboliczny (np. `u+rwx`, `g-w`, `o=r`):

- `u` – właściciel (user)
- `g` – grupa (group)
- `o` – inni (others)
- `a` – wszyscy (all)

Przykład: `chmod u+x,g-w,o=r script.sh`

#### Numeryczny (np. `755`, `644`):

- `4` – read (r)
- `2` – write (w)
- `1` – execute (x)

Przykład: `chmod 750 folder/` → właściciel: `rwx`, grupa: `r-x`, inni: `---`

### Przykłady

Nadaj wszystkim uprawnienie wykonania pliku:

```bash
chmod a+x im_stuck.sh
```

Usuń możliwość zapisu dla grupy i innych:

```bash
chmod go-w secret.txt
```

Ustaw rekurencyjnie uprawnienia `755` dla folderu:

```bash
chmod -R 755 ~/Documents/
```

## chown

### Składnia:

```bash
chown [opcje] <użytkownik>:<grupa> <plik/folder>
```

### Kluczowe opcje:

- `-R` – zmień rekurencyjnie (dla folderów i ich zawartości)
- `-v` – pokaż zmienione pliki (verbose)

### Przykłady

Zmień właściciela pliku na `root`:

```bash
chown root /var/log/app.log
```

Zmień właściciela i grupę folderu:

```bash
chown alice:developers ~/project/
```

Zmień rekurencyjnie grupę dla całego folderu:

```bash
chown -R :admin /opt/app/
```

### Uwaga

Dla **linków symbolicznych** użyj `-h`, aby zmienić uprawnienia samego linku (domyślnie `chown` modyfikuje cel linku).
