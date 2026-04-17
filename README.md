# PostLaunch Dashboard — web-version

`index.html` er en selvindeholdt spejling af FCP-dashboardets karrusel.
Ingen eksterne dependencies — én fil, intet byggeskridt. Datapoint er
indlejret som JavaScript-objekter og spejler `FCPMockData.swift`.

## Funktioner

- 5 slides (Server-rack · Top produktioner · Klientfordeling · Post-assistenter · Nøgletal)
- Auto-rotering hvert 15. sekund
- Tryk på tab eller swipe venstre/højre for at skifte
- Live ur (klient-side)
- Mobil-først responsive layout — dobbelttjekket på iPhone

## Hosting — vælg én

### 1. Netlify Drop (hurtigst)

1. Gå til <https://app.netlify.com/drop>
2. Træk hele `docs/`-mappen ind i browservinduet
3. Få en tilfældig URL som `https://wonderful-tile-123.netlify.app`
4. Åbn den på iPhone. Del → **Føj til hjemmeskærm**

### 2. GitHub Pages

Repo'et er privat, så GitHub Pages kræver enten:

- **GitHub Pro** (betalt, ~$4/md), eller
- **Et separat public repo** kun til dashboardet

Hvis Pro:
1. Settings → Pages → Source: `main` branch, folder: `/docs`
2. URL: `https://ryaveldk.github.io/PostLaunch/`

### 3. iCloud Drive

1. Kopiér `index.html` til iCloud Drive
2. Åbn Files-appen på iPhone → trykker på filen → Safari renderer den
3. Ingen rigtig URL, men virker offline

### 4. Cloudflare Pages

Gratis, understøtter private GitHub-repos, deployer automatisk ved push.
Kræver 5-minutters engangsopsætning på <https://pages.cloudflare.com>.

## Opdatering

**Automatisk synkronisering er aktiv.** Efter hvert commit i PostLaunch-repoet
kører `.git/hooks/post-commit` som kalder `scripts/sync-dashboard.sh
--only-if-docs-changed`. Det:

1. Springer ud straks hvis `docs/` ikke var en del af commit'et (ingen forsinkelse)
2. Ellers: kloner/opdaterer mirror-repoet i `~/Library/Application Support/PostLaunch/dashboard-mirror/`
3. Kopierer `docs/*` over, committer og pusher til `postlaunch-dashboard`-repoet
4. GitHub Pages rebuilds automatisk inden for ~10 sekunder

Sync-log ligger i `/tmp/postlaunch-sync.log` efter hver kørsel.
Kan også køres manuelt: `./scripts/sync-dashboard.sh`

Ændrer du i `FCPMockData.swift` skal du stadig manuelt opdatere `SERVERS`,
`ASSISTANTS` og `PRODUCTIONS` i `index.html` — men så snart du committer,
ryger ændringen til web'en.
