# Git notities

Samenvatting van uitgezochte Git-begrippen en opgeloste problemen (Windows Git GUI + Linux git-cola).

## Unstaged vs. Untracked

- **Untracked**: bestand is nooit bijgehouden door Git (nog nooit `git add` gedaan).
- **Unstaged changes**: wijziging aan een bestand dat Git al kent, maar nog niet gestaged.
- Git GUI (Windows) toont beide onder één label "Unstaged Changes" → verwarrend.
- git-cola (Linux) en `git status` maken het onderscheid wel expliciet.
- Bij twijfel: `git status` in de terminal is de betrouwbare bron.

## Repository aanmaken

- "New Repository" in zowel Git GUI als git-cola voert intern `git init` uit.
- Er wordt een verborgen `.git`-map aangemaakt met alle geschiedenis/config.
- Werkt identiek ongeacht welke GUI je gebruikt.

## Basis workflow

| Terminal | Git GUI | git-cola |
|---|---|---|
| `git add .` | Stage Changed | Stage |
| `git commit -m "bericht"` | Commit-veld + Commit | Commit summary + Commit |
| `git push` | Push | Push... |

## Fetch vs. Pull

- `git fetch`: haalt remote-info op, **past je werkbestanden niet aan**.
- `git pull` = `git fetch` + `git merge` in één stap.
- Fetch eerst gebruiken om te zien wat er veranderd is, zonder risico op conflicten.

## master vs. main

- Oudere Git-installaties: standaard branchnaam `master`.
- GitHub (sinds ~2020): standaard `main` bij nieuwe repositories.
- Lokale branch hernoemen:
  ```
  git branch -m master main
  ```
- Standaard voor nieuwe repo's instellen:
  ```
  git config --global init.defaultBranch main
  ```

## Pushen (command line)

```
git push origin main          # normale push
git push -u origin main       # eerste keer, legt tracking-koppeling
git push                      # daarna volstaat dit
git remote -v                 # remote-URL controleren
```

## Author identity ontbreekt ("Commit failed", exit status 128)

Foutmelding: `Please tell me who you are`. Oplossing:
```
git config --global user.email "jouw-email@voorbeeld.com"
git config --global user.name "Jouw Naam"
```
Moet op **elke machine apart** ingesteld worden (Windows én Linux).

## SSH-sleutel instellen voor GitHub

1. Sleutel aanmaken (gewoon Enter drukken bij alle vragen, geen pijltjestoetsen tussendoor typen!):
   ```
   ssh-keygen -t ed25519 -C "jouw-email@voorbeeld.com"
   ```
2. Controleren dat de bestanden bestaan:
   ```
   ls -la ~/.ssh
   ```
3. Toevoegen aan de ssh-agent:
   ```
   eval $(ssh-agent -s)
   ssh-add ~/.ssh/id_ed25519
   ```
4. Publieke sleutel tonen en kopiëren:
   ```
   cat ~/.ssh/id_ed25519.pub
   ```
5. Plakken op GitHub: Settings → SSH and GPG keys → New SSH key.
6. Testen:
   ```
   ssh -T git@github.com
   ```
   Succesbericht: `Hi <gebruikersnaam>! You've successfully authenticated...`

**Veelgemaakte fout**: sleutel lokaal aanmaken en testen zonder 'm ooit op GitHub.com te hebben geplakt → blijft "Permission denied (publickey)" geven.

## git-cola voor Windows

Beschikbaar via:
- Officiële installer / ZIP op GitHub releases-pagina
- `winget install` (zoek "git-cola")
- Chocolatey: `choco install git-cola`

Vereist Python en Git.

## Opgeruimde rommelbestanden

Per ongeluk aangemaakte bestanden met escape-codes (`[C`, `[D`) in de naam, ontstaan door pijltjestoetsen tijdens terminalinvoer. Opgeruimd met:
```
git clean -n     # dry run, toont wat verwijderd zou worden
git clean -f     # daadwerkelijk verwijderen
```
