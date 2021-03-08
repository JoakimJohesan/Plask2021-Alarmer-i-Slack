## Henter data fra exceptions

- Bruker 'summarize' for å gruppere exceptions som er lik

```
exceptions
| summarize count() by type
| order by count_ desc

```

## Henter data fra traces
- Bruker severityLevel for å kun få ut logger markert som Error
- Sensurerer alle tall fra 'message' feltet siden disse kan inneholde hemmelig informasjon
- Grupperer etter feilmelding for å slippe dupliserte resulateter
- Projiserer resulatene ut til de kolonnene jeg er interessert å sende ut i Slack

```
traces 
| where severityLevel >= 3
| extend censoredMessage = replace(@'\d', @'*', message)
| summarize count() by censoredMessage, method=operation_Name
| project Count=count_, censoredMessage, method
```