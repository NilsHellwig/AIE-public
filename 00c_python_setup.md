# Wie setze ich Python auf meinem Rechner auf?

## Installation von uv zur Erstellung einer Python-Umgebung

Quelle: https://docs.astral.sh/uv/getting-started/installation/#installation-methods

### MacOS / Linux

Öffne zunächst ein Terminal.

#### Option A (empfohlen): Installation via curl

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

#### Option B: Installation via Homebrew (auf MacOS)

Falls du Homebrew installiert hast (prüfe mit `brew --version`), kannst du uv ganz einfach mit folgendem Befehl installieren:


```bash
brew install uv
```

### Windows

Öffne PowerShell und führe folgenden Befehl aus:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

## Python in Jupyterlab verwenden

1. Erstellt einen Ordner in dem ihr arbeiten wollt, z.B. `AIE_course`
2. Wechselt in diesen Ordner im Terminal

**Mac / Linux:**

Rechtsklick auf Ordner im Finder → „Dienste“ → „Neues Terminal beim Ordner“

**Windows:**

Shift + Rechtsklick im Ordner → „PowerShell-Fenster hier öffnen“

3. Erstellt eine neue Python-Umgebung mit uv

```bash
uv init
```

4. Installiert Jupyterlab in der Umgebung

```bash
uv add jupyterlab
```

5. Startet Jupyterlab

```bash
uv run jupyter lab
```

6. Nun öffnet sich ein Browserfenster mit Jupyterlab. Dort könnt ihr neue Notebooks erstellen und Python-Code ausführen.

7. Eine kleine Einführung in Jupyterlab findet ihr hier: https://jupyterlab.readthedocs.io/en/stable/getting_started/overview.html / https://www.youtube.com/watch?v=7wf1HhYQiDg

**Hinweis 1:** Der Jupyter-Server kann im Terminal mit `CTRL + C` gestoppt werden bei Mac / Linux oder `STRG + C` bei Windows.
**Hinweis 2:** Wenn ihr später weitere Pakete installieren wollt, könnt ihr dies mit `uv add paketname` tun, z.B. `uv add numpy` etc. In Jupyterlab kann man ebenfalls ein Terminal öffnen und dort Pakete installieren.

## Einführung in uv

Wie ihr bereits gesehen habt, können wir mit `uv init` eine neue Python-Umgebung erstellen und mit `uv add paketname` Pakete installieren.
Ich fasse euch hier noch ein paar nützliche Befehle und Informationen zu uv zusammen.

### Was ist uv?

uv ist ein Tool zur Verwaltung von Python-Umgebungen und -Abhängigkeiten. Es ermöglicht die einfache Erstellung, Verwaltung und Reproduktion von isolierten Python-Umgebungen. Durch Ausführung von `uv init` wird eine `pyproject.toml` Datei erstellt, die die Abhängigkeiten und Konfigurationen der Umgebung enthält. Sobald mit `uv` Befehle ausgeführt werden, wird automatisch die Umgebung verwendet, die in `pyproject.toml` definiert ist.

### Welche Dateien werden erstellt bei `uv init`?

- `pyproject.toml`: Enthält die Abhängigkeiten und Konfigurationen der Python-Umgebung.
- `.python-version`: Speichert die Python-Version, die in der Umgebung verwendet wird.
- `uv.lock`: Enthält die exakten Versionen aller Abhängigkeiten für Reproduzierbarkeit.

Der Unterschied zwischen `pyproject.toml` und `uv.lock` ist, dass `pyproject.toml` die gewünschten Abhängigkeiten mit Versionsbereichen definiert (z.B. `numpy >= 1.21`), während `uv.lock` die exakten Versionen festhält, die installiert wurden (z.B. `numpy 1.21.2`). Dies stellt sicher, dass die Umgebung bei späteren Installationen exakt reproduziert werden kann.

### Wichtige uv Befehle

1. **`uv init`**: Erstellt eine neue Python-Umgebung im aktuellen Verzeichnis
   - Generiert `pyproject.toml` und `.python-version` Dateien

2. **`uv add paketname`**: Installiert ein Paket und fügt es zu den Abhängigkeiten hinzu
   - Beispiel: `uv add numpy pandas matplotlib`

3. **`uv remove paketname`**: Entfernt ein Paket aus der Umgebung
   - Beispiel: `uv remove numpy`

4. **`uv run befehl`**: Führt einen Befehl in der uv-Umgebung aus
   - Beispiel: `uv run python script.py` oder `uv run jupyter lab`

5. **`uv sync`**: Synchronisiert die Umgebung mit `pyproject.toml`
   - Installiert alle in der Datei definierten Abhängigkeiten

6. **`uv lock`**: Erstellt oder aktualisiert die `uv.lock` Datei
   - Fixiert exakte Versionen aller Abhängigkeiten für Reproduzierbarkeit

7. **`uv tree`**: Zeigt den Abhängigkeitsbaum der installierten Pakete
   - Hilfreich zum Verstehen von Paket-Abhängigkeiten

8. **`uv pip list`**: Listet alle installierten Pakete auf
   - Zeigt Paketnamen und Versionen

9. **`uv python install 3.12`**: Installiert eine spezifische Python-Version
   - uv kann verschiedene Python-Versionen verwalten
