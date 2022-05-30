# oereb-sh

## Konfiguration

### Zuständige Stellen
- Wird zwingend für die Konfiguration der katasterverantwortlichen Stelle benötigt.
- Wird optional für die Konfiguration sämtlicher zuständigen Stellen benötigt (inkl. Staatskanzlei). Damit müssen die zuständigen Stellen nicht mit den Geodaten mitgeliefert werden, sondern es reicht der Verweis. Vorteil: Die zuständigen Stellen werden an einer zentralen Stelle gepflegt. Es verhindert Redundanzen im ÖREB-Katastersystem: Ein Amt gibt es nur einmal.

### Gesetze
- Wird zwingend für die Verwaltung der kantonalen gesetzlichen Grundlagen benötigt.
- `java -jar /Users/stefan/apps/ilivalidator-1.11.11/ilivalidator-1.11.11.jar --allObjectsAccessible ch.sh.agi.oereb_zustaendigestellen_V2_0.xtf ch.sh.sk.oereb_gesetze_V2_0.xtf`

### Themen
- Wird zwingend für die Zuweisung der kantonalen gesetzlichen Grundlagen zu den Themen benötigt.
- Wird zwingend für die Verwaltung von Subthemen oder kantonalen Themen benötigt.
- `java -jar /Users/stefan/apps/ilivalidator-1.11.11/ilivalidator-1.11.11.jar --allObjectsAccessible OeREBKRM_V2_0_Gesetze_20210414.xml OeREBKRM_V2_0_Themen_20220301.xml ch.sh.agi.oereb_zustaendigestellen_V2_0.xtf ch.sh.sk.oereb_gesetze_V2_0.xtf ch.sh.agi.oereb_themen_V2_0.xtf`

### Verfügbarkeit
- Wird zwingend für die Steuerung der verfügbaren Gemeinden und Themen benötigt.
- Wird zwingend für das Datum des Standes der amtlichen Vermessung benötigt.

### Grundbuch
- Wird zwingend benötigt, falls es Grundbuchkreise (o.ä.) gibt.
- oereb-web-service: Schlüssel sind die NBIdent.

### MapLayering
- Wird zwingend benötigt, falls man die Opazität und die Reihenfolge der WMS-Bilder (d.h. der Darstellungsdienste) steuern will.
- oereb-web-service: Der Schlüssel ist der Wert des `LAYERS`-Key.

### Logo
- Wird zwingend für die Verwaltung der Logos der Gemeinden und des Kantons benötigt.

### Texte
- Wird zwingend benötigt, falls es neben den Bundestexten noch weitere kantonale Texte für den Glossar gibt.

### Konfiguration ausserhalb des Rahmenmodells
- Siehe Dockerfile / docker-compose.yml
- Für ein Multi-Tenant-Einsatz fehlt etwas wie "KVS pro Gemeinde" im Rahmenmodell. Stand heute ist die KVS pro laufenden Webservice konfiguriert (Schlüssel ist Url der KVS).

## Run

```
docker-compose up
```

```
export ORG_GRADLE_PROJECT_dbUriOerebV2="jdbc:postgresql://oereb-db/oereb"
export ORG_GRADLE_PROJECT_dbUserOerebV2="gretl"
export ORG_GRADLE_PROJECT_dbPwdOerebV2="gretl"
```

### Reihenfolge

- `./start-gretl.sh --docker-image sogis/gretl:latest --docker-network oereb_gretljobs_default --job-directory $PWD/oereb_av replaceCadastralSurveyingData`
- `./start-gretl.sh --docker-image sogis/gretl:latest --docker-network oereb_gretljobs_default --job-directory $PWD/oereb_plzo importPLZO`
- `./start-gretl.sh --docker-image sogis/gretl:latest --docker-network oereb_gretljobs_default --job-directory $PWD/oereb_bundesgesetze importData`
- `./start-gretl.sh --docker-image sogis/gretl:latest --docker-network oereb_gretljobs_default --job-directory $PWD/oereb_bundeskonfiguration importBundeskonfiguration`
- `./start-gretl.sh --docker-image sogis/gretl:latest --docker-network oereb_gretljobs_default --job-directory $PWD/oereb_kantonskonfiguration importKantonskonfiguration`
- `./start-gretl.sh --docker-image sogis/gretl:latest --docker-network oereb_gretljobs_default --job-directory $PWD/oereb_bundesdaten importData`
- `./start-gretl.sh --docker-image sogis/gretl:latest --docker-network oereb_gretljobs_default --job-directory $PWD/oereb_kbs importData`

- Alles: `./start-gretl.sh --docker-image sogis/gretl:latest --docker-network oereb_gretljobs_default --job-directory $PWD :oereb_av:replaceCadastralSurveyingData :oereb_plzo:importPLZO :oereb_bundesgesetze:importData :oereb_bundeskonfiguration:importBundeskonfiguration :oereb_kantonskonfiguration:importKantonskonfiguration :oereb_bundesdaten:importData :oereb_kbs:importData`
