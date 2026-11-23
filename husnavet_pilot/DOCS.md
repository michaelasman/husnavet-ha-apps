# Installation och uppdatering

## Första installationen på iPhone

1. Öppna **Inställningar → Appar → Installera app → tre punkter → Registries**. Lägg till `ghcr.io` med ditt GitHub-användarnamn och en giltig GitHub-token (classic) med `read:packages` för det privata containerpaketet.
2. Välj **tre punkter → Förråd/Repositories** och lägg till `https://github.com/michaelasman/husnavet-ha-apps`.
3. Öppna **Husnavet Pilot**, kontrollera version 0.5.0 och välj **Installera**. När installationen är klar väljer du **Starta** och sedan **Öppna webbgränssnittet**. Kontrollera att 21 av 21 syntetiska testfall är godkända.

## Uppdatera en befintlig installation

Du kan återanvända de sparade registeruppgifterna.

1. Under **Inställningar → System → Uppdateringar**, sök efter Husnavet Pilot 0.5.0. Behåll säkerhetskopiering av föregående appversion aktiverad och välj **Uppdatera**.
2. Om uppdateringsentiteten saknas, öppna Home Assistant CLI i Terminal på iPhone. Kontrollera installerad och tillgänglig version med `ha apps info 8f2ed67e_husnavet_pilot`. När 0.5.0 erbjuds, kör `ha apps update 8f2ed67e_husnavet_pilot --backup` och kontrollera versionen igen.
3. Öppna **Inställningar → Appar → Husnavet Pilot**, välj **Starta** vid behov och **Öppna webbgränssnittet**. Kontrollera versionen och 21 av 21 godkända syntetiska testfall. Livedata och anslutningen för värmeråd är avstängda som standard.

## Aktivera valfria källor

Öppna appens **Konfiguration**. För pris, återanvänd gärna en giltig Tibber-token i `tibber_token`. GitHub-token hör enbart till containerregistret. Lämna `tibber_home_id` tomt om token ger åtkomst till exakt ett hem. Vid flera hem anger du rätt Home ID lokalt. För väder, ange punktens latitud och longitud i `smhi_latitude` och `smhi_longitude`. Aktivera `live_enabled`, spara och starta om appen. Slå på **Starta vid uppstart** för kontinuerlig drift efter omstart av Green.

## Hur ofta uppgifterna hämtas

Alternativet `tibber_fetch_policy` väljer prisstrategi. Om alternativet saknas används **daily**, med ordinarie hämtning mellan 14:00 och 15:00 svensk tid. För tidigare upptäckt av morgondagens priser väljer du **publication_window** uttryckligen. Då startar kontrollen mellan 13:00 och 13:01, fortsätter med fem till sex minuters mellanrum medan ett komplett dygn saknas och avslutar de täta kontrollerna före 16:00. Högst 36 hämtningsomgångar får starta under fönstret, även över omstarter. Varje omgång kan innebära upp till fyra HTTP-försök när hemidentifiering och transportomförsök behövs. Leverantörens väntetider kan förlänga intervallen. Detta täta läge avviker från Tibbers rekommendation om hämtning en gång per dag.

När ett komplett morgondagsdygn har hämtats väntar det täta läget till en kontroll mellan 17:00 och 18:00 för eventuella prisändringar, med ett extra försök ungefär en timme senare vid behov. Om morgondagen fortfarande saknas efter 16:00 fortsätter kontroller ungefär varje timme. Saknade priser för innevarande dygn repareras också ungefär varje timme. Systemet kan upptäcka priser först när de finns i Tibbers API.

SMHI kontrolleras normalt ungefär varje timme. Cachevillkor och Retry-After kan kräva längre väntan. Vid tillfälliga fel används stegvis väntan på 5, 15, 30 och högst 60 minuter, med liten tidsförskjutning. Prognosens skapandetid behålls när samma prognos hämtas igen. Över 90 minuters ålder visas en varning och över 120 minuter visas gammal prognos; lyckad kontakt bedöms separat.

Öppna webbgränssnittet. Läs av **Senast kontrollerad**, **Hela morgondagen hämtad**, dygnets **Senast bekräftat**, **Prognos skapad**, **Prognosens ålder** och **Nästa hämtning**. En lyckad kontakt garanterar inte att ett komplett prisdygn eller en ny väderprognos har mottagits. Ett nytt ofullständigt prissvar ersätter inte ett tidigare komplett dygn. Rapporten använder formatet husnavet-live-status-v2; dess generatedAt är bara tidpunkten för lokal visning. Ett nytt fel kan visas tillsammans med äldre sparade värden. Vid automatisk hemidentifiering betyder `cached_confirmation` att senaste bekräftelsen återlästs och kontrolleras igen vid nästa planerade hämtning. Siduppdatering läser den lokala statusen. Spara den sanerade liverapporten eller den separata syntetiska testrapporten i **Filer**. Granska liverapporten före delning eftersom den innehåller valda pris- och vädervärden och tider.

Kvällsbekräftelsen visas separat. Om kontrollen och dess återförsök bara ger ofullständiga priser visas en varning, medan tidigare hela dygn behålls med sina ursprungliga bekräftelsetider. **Senaste lyckade svarets ursprung** avser svaret, inte alla sparade prisdygn.

Vid sparfel återförsöks den lokala skrivningen utan nya leverantörsanrop. Om en avbruten hämtning saknar sparat slutresultat efter omstart pausas leverantören: en eventuell väntetid i det förlorade svaret kan inte återskapas. En skadad tidigare lagringspost ger också paus. Bevara rapporten för riktad felsökning; radera inte lagringen för att tvinga ett nytt anrop. En signerad post som ligger i framtiden kontrolleras igen varje timme och kan återläsas när systemklockan har rättats.

## Värmeråd från Homey, för jämförelse

Den valfria anslutningen låter ett HomeyScript läsa utvalda Logic-värden och skicka dem till Green. Appen räknar fram ett råd med Homeys indata och, när HA-priserna täcker hela perioden, en separat jämförelse med HA-priser. Homey fortsätter styra enheter och hantera manuella val. Scriptet skriver inga Logic-värden, kvitterar ingen styrkedja och anropar inga enheter eller Flows.

1. Öppna appens **Konfiguration**. Skapa en ny slumpmässig delad token med 32–128 tecken ur A–Z, a–z, 0–9, understreck och bindestreck. Ange den i lösenordsfältet `advisory_token` och sätt `advisory_enabled` till på. Använd en separat token för denna anslutning.
2. Under appens **Nätverk** kopplar du containerport `8100/tcp` till en ledig lokal port på Green. Mappningen är tom från början. Webbgränssnittets Ingress-port 8099 fortsätter vara separat.
3. Spara och starta om appen. När det syntetiska testet är godkänt ska avsnittet **Värmeråd från Homey** visa att anslutningen väntar på Homey. Ett konfigurationsfel för värmeråd stoppar inte rätt konfigurerad pris- eller väderhämtning.
4. Välj **Ladda ned HomeyScript med platshållare** i webbgränssnittet och lägg scriptet i HomeyScript. Använd en separat Flow med HomeyScript-kortet **Run a script with an argument**. Argumentet är JSON med fälten `url` och `token`; kopiera exemplet i scriptets första rader och ersätt platshållarna med Greens privata IPv4-adress, valda lokala port och samma token som i appen. Nedladdningen innehåller aldrig din token. Anslutningen använder lokal HTTP och förutsätter ett betrott hemnätverk; publicera den inte på internet.
5. Kör den separata Flowen manuellt till att börja med, utanför den befintliga styrkedjan. Öppna webbgränssnittet igen och kontrollera senaste insamling, producenternas tider och att rådet fortfarande är giltigt. Om återkommande körning behövs senare, börja med låg frekvens.

Två avläsningar kontrollerar stabilitet i hushållets konfiguration, tillstånd, policy och manuella val. Avläsningarna är inte en gemensam transaktion och de enskilda mätvärdenas tidsstämplar är okända. Effekt- och batterivärden som ändras redovisas separat. Ett nytt request-ID räcker inte för att göra gamla producenttider aktuella.

Råden använder högst tio minuter gammalt hushållsunderlag för jämförelse. Nästa priskvart och slutet på ett aktivt manuellt val kan begränsa giltigheten ytterligare. Läsning av rapporten förnyar aldrig rådets tider. Efter omstart krävs en ny insamling från Homey. Ett mottaget underlag och ett färdigt råd redovisas var för sig, så ett tidigare resultat kan visas medan ett nytt beräknas.

HA-prisjämförelsen kräver 96 sammanhängande kvartar från godkända prisdygn. Pris nu, nästa pris, trend och framtida prisserie kan bytas i just den jämförelsen. Framtidsvikt, värmetillstånd, policy, konfigurering och manuella val kommer fortfarande från Homey. Saknade HA-priser gör jämförelsen otillgänglig, medan ett giltigt råd med Homeys indata kan visas. SMHI:s separata sex timmars väderunderlag ersätter inte uppmätt utetemperatur och används inte i den här värmeberäkningen.

**Ladda ned värmerapporten som JSON** sparar valda plankontrakt, sammanfattningar, ursprungliga tider och orsaker när ett råd saknas. Rapporten innehåller inga tokens eller fullständiga Logic-register. Den innehåller hushållsuppgifter och manuella val, så granska den före delning. För att stänga anslutningen, stäng av `advisory_enabled`, ta bort portmappningen och starta om appen.

## Återställning

Skydda säkerhetskopior eftersom appens inställningar kan innehålla Tibber-token, delad token för värmeråd, hem-ID och koordinater. För att återgå till 0.4.0, stoppa appen och återställ säkerhetskopian från före uppdateringen. Version 0.5.0 behåller version 2 av leverantörslagringen. Återställ hela appens säkerhetskopia vid återgång. Hushållsunderlag och värmeråd sparas endast i minnet och kräver en ny insamling efter omstart. Aktiv styrning från Green kräver en senare uttrycklig integration.
