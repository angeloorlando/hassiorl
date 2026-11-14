# CLAUDE.md

Questo repo contiene la configurazione di Home Assistant (HAOS su Raspberry Pi), versionata con git. Il repo è la fonte di verità: non si modificano file direttamente sul Pi.

## Scopo

- Gestire configurazione YAML, automazioni, script, scene e dashboard di Home Assistant.
- Le dashboard sono in modalità YAML e si modificano solo tramite questo repo.
- Il Pi legge il repo tramite l'add-on Git pull.

## Struttura attesa

```
ha-config/
├── configuration.yaml
├── automations.yaml
├── scripts.yaml
├── scenes.yaml
├── ui-lovelace.yaml        # dashboard principale (modalità YAML)
├── packages/               # moduli per dominio (luci, clima, sicurezza, ...)
├── dashboards/             # eventuali dashboard aggiuntive in YAML
├── CLAUDE.md
└── .gitignore
```

Se la struttura cambia, aggiorna questa sezione.

## File da non toccare

- `secrets.yaml`: non è nel repo e non va mai creato, letto o riportato nelle risposte. Per riferirsi a un segreto usa sempre `!secret nome_chiave` e suggeriscine la modifica.
- `.storage/`, `*.db`, `*.log`: file generati da Home Assistant, esclusi da git.
- Non inserire token, password, IP privati o URL con credenziali in nessun file versionato.

## Regole di lavoro

1. Lavora sempre su un branch dedicato, mai direttamente su `main`. Nome: `feature/<descrizione>` o `fix/<descrizione>`.
2. Prima di modificare, leggi i file coinvolti e le entità già usate, così non introduci duplicati o ID inesistenti.
3. Usa ID entità stabili e descrittivi, ad esempio `light.soggiorno_plafoniera`. Non rinominare entità esistenti senza dirmelo, perché rompe automazioni e dashboard.
4. Per le automazioni usa `id` univoci e leggibili. Preferisci `alias` chiari in italiano.
5. Mantieni le modifiche piccole e focalizzate. Un commit per modifica logica.
6. Non eseguire azioni fisiche (accendere luci, attivare serrature, modificare termostati) senza una mia richiesta esplicita.
7. Quando modifichi una dashboard, mantieni coerenza visiva con le altre viste e segnala le entità che non esistono ancora.

## Commit

Usa messaggi in italiano al presente, brevi e descrittivi:

- `feat: aggiunge vista soggiorno nella dashboard`
- `fix: corregge trigger del sensore porta ingresso`
- `refactor: sposta automazioni luci in packages/luci.yaml`

Prima del commit mostrami sempre il diff e aspetta la mia conferma.

## Validazione

Prima di ogni proposta di merge va eseguito il check della configurazione sul Pi via SSH:

```bash
ha core check
```

Se il check fallisce, correggi prima di procedere. Non proporre mai un merge con errori di configurazione.

## Deploy

Dopo il merge su `main`:

1. Il Pi esegue il pull tramite Git pull.
2. Si lancia `ha core check`.
3. Solo se il check passa si ricaricano i componenti interessati o si riavvia Home Assistant.

Non riavviare Home Assistant senza una mia conferma.

## Backup

Prima di modifiche strutturali (nuovi package, riorganizzazione dei file, cambio di integrazioni) ricordami di creare uno snapshot da Impostazioni → Sistema → Backup.

## Lingua

Rispondi sempre in italiano. Commenti nei file YAML in italiano. Nomi di entità e ID in inglese o snake_case come da convenzione di Home Assistant.
