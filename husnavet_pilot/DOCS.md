# Installation och uppdatering

## Första installationen på iPhone

1. Öppna **Inställningar → Appar → Installera app → tre punkter → Registries**. Lägg till `ghcr.io` med ditt GitHub-användarnamn och en giltig GitHub-token (classic) med `read:packages` för det privata containerpaketet.
2. Välj **tre punkter → Förråd/Repositories** och lägg till `https://github.com/michaelasman/husnavet-ha-apps`.
3. Öppna **Husnavet Pilot**, kontrollera version 0.3.0 och välj **Installera**. När installationen är klar väljer du **Starta** och sedan **Öppna webbgränssnittet**. Kontrollera att 21 av 21 syntetiska testfall är godkända.

## Uppdatera en befintlig installation

Du kan återanvända de sparade registeruppgifterna.

1. Under **Inställningar → System → Uppdateringar**, sök efter Husnavet Pilot 0.3.0. Behåll säkerhetskopiering av föregående appversion aktiverad och välj **Uppdatera**.
2. Om uppdateringsentiteten saknas, öppna Home Assistant CLI i Terminal på iPhone. Kontrollera installerad och tillgänglig version med `ha apps info 8f2ed67e_husnavet_pilot`. När 0.3.0 erbjuds, kör `ha apps update 8f2ed67e_husnavet_pilot --backup` och kontrollera versionen igen.
3. Öppna **Inställningar → Appar → Husnavet Pilot**, välj **Starta** vid behov och **Öppna webbgränssnittet**. Kontrollera versionen och 21 av 21 godkända syntetiska testfall. Livedata är avstängt som standard.

## Aktivera valfria källor

Öppna appens **Konfiguration**. För pris, återanvänd gärna en giltig Tibber-token i `tibber_token`. GitHub-token hör enbart till containerregistret. Lämna `tibber_home_id` tomt om token ger åtkomst till exakt ett hem. Vid flera hem anger du rätt Home ID lokalt. För väder, ange punktens latitud och longitud i `smhi_latitude` och `smhi_longitude`. Aktivera `live_enabled`, spara och starta om appen. Slå på **Starta vid uppstart** för kontinuerlig drift efter omstart av Green.

Öppna webbgränssnittet. Läs av täckning, senaste lyckade hämtning, senaste försök och prognosens källtider. Ett nytt fel kan visas tillsammans med äldre sparade värden. Vid automatisk hemidentifiering betyder `cached_confirmation` att senaste bekräftelsen återlästs och kontrolleras igen vid nästa planerade hämtning. Siduppdatering läser den lokala statusen. Spara den sanerade liverapporten eller den separata syntetiska testrapporten i **Filer**. Granska liverapporten före delning eftersom den innehåller valda pris- och vädervärden och tider.

## Återställning

Skydda säkerhetskopior eftersom appens inställningar kan innehålla Tibber-token, hem-ID och koordinater. För att återgå till 0.2.0, stoppa appen och återställ säkerhetskopian från före uppdateringen. Den privata imagen för 0.2.0 behålls.
