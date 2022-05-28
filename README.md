# oereb-sh

```
docker-compose up
```

```
export ORG_GRADLE_PROJECT_dbUriOerebV2="jdbc:postgresql://oereb-db/oereb"
export ORG_GRADLE_PROJECT_dbUserOerebV2="gretl"
export ORG_GRADLE_PROJECT_dbPwdOerebV2="gretl"
```

```
./start-gretl.sh --docker-image sogis/gretl:latest --docker-network oereb_gretljobs_default --job-directory $PWD/oereb_av replaceCadastralSurveyingData
```