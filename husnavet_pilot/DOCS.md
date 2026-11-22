# Installation

1. Lägg till läsbehörighet för det privata containerpaketet under Inställningar → Appar → Installera app → Registries. Registry: `ghcr.io`. Använd ditt GitHub-användarnamn och en personal access token (classic) med `read:packages` och åtkomst till paketet.
2. Installera Husnavet Pilot och välj Starta.
3. Öppna webbgränssnittet. Kontrollera att alla testfall har passerat och ladda ned rapporten.

Home Assistant Green (aarch64) stöds. Ingen konfiguration behövs. Start vid uppstart är avstängd för piloten. Manuella uppdateringar rekommenderas.

## Återställning

Stoppa appen för att avbryta piloten. Ta en app-säkerhetskopia före uppdatering; återställ kopian om föregående version behöver återtas. Appen har ingen enhetsstyrning eller koppling till Homey i denna version.
