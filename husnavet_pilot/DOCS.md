# Installation

1. Lägg till läsbehörighet för det privata containerpaketet under Inställningar → Appar → Installera app → Registries. Registry: `ghcr.io`. Använd ditt GitHub-användarnamn och en personal access token (classic) med `read:packages` och åtkomst till paketet.
2. Installera Husnavet Pilot och välj Starta.
3. Öppna webbgränssnittet. Kontrollera att alla testfall har passerat och ladda ned rapporten.

Home Assistant Green (aarch64) stöds. Ingen konfiguration behövs. Start vid uppstart är avstängd för piloten. Manuella uppdateringar rekommenderas.

## Uppdatera en befintlig installation

Öppna Inställningar → System → Uppdateringar och sök efter uppdateringar. Välj Husnavet Pilot 0.2.0 och behåll säkerhetskopiering av föregående version aktiverad. Välj Uppdatera. Öppna sedan Inställningar → Appar → Husnavet Pilot. Om appen är stoppad, välj **Starta**. Öppna webbgränssnittet. När 21 av 21 testfall har passerat, ladda ned JSON-rapporten. En manuell omstart ger ett nytt körnings-ID och en ny rapport.

## Återställning

Stoppa appen och återställ säkerhetskopian av föregående appversion vid behov. Den tidigare privata imagen behålls för återställning.
