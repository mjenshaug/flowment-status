# Flowment driftsstatus

Den offentlige statussida for Flowment: **status.flowment.no** (GitHub Pages fra `main`).

Sida er én fil, `index.html`, uten rammeverk og uten eksterne avhengigheter. Den henter
`https://admin.flowment.no/api/status` hvert minutt og viser samlet status, tilstand og
90 dagers oppetid per tjeneste (Plattformen, Nettsiden, Database, Møter, E-post, Video),
samt de siste hendelsene.

## Hvor dataene kommer fra

Driftsvakta i admin-appen (repoet `flowment-admin`) sjekker tjenestene hvert minutt og
eksponerer resultatet på `/api/status`. Det er den eneste kilden når admin er oppe.

## Når admin selv er nede

Driftsvakta kan ikke si fra om seg selv. Derfor kjører `.github/workflows/driftsvakt-reserve.yml`
i dette repoet hvert 5. minutt og sjekker admin.flowment.no direkte. Svarer ikke admin
i to kjøringer på rad:

- sendes én Telegram-melding til driftsgruppa (og én «tilbake»-melding når admin svarer igjen),
- skrives reservens egne funn til `status.json`, som `index.html` faller tilbake til når
  API-et ikke svarer. Sida viser da tydelig at det er sist kjente status, ikke live.

Secrets som må finnes i dette repoet: `TELEGRAM_BOT_TOKEN` og `DRIFT_TELEGRAM_CHAT_ID`.
Dedupe (én melding per nedetid) skjer via fila `reserve-tilstand.txt`, som workflowen committer selv.
