# AWS_CloudWatch_LogsInsights_Querys
AWS_CloudWatch_LogsInsights_Querys

**Filtrar IP Origem, URI e Ação**
fields @timestamp, @message, @logStream, @log <br>
| stats count(*) as qty by httpRequest.clientIp, httpRequest.uri, action <br>
| sort qty desc <br>

**Fitrar Token autenticação vazio**
fields @timestamp, @message, @logStream, @log <br>
| filter @message like '"value":"Bearer"' <br>
| stats count(*) as qty by httpRequest.clientIp, action <br>
| sort qty desc

**Filtrar path de API**
fields @timestamp, @message, @logStream, @log <br>
| sort @timestamp desc <br>
| filter httpRequest.uri = '/api/v1/login' <br>
| stats count(*) by action <br>
| limit 10000 <br>

**Filtrar por país de origem do tráfego**
fields @timestamp, httpRequest.clientIp <br>
| stats count(*) as requestCount by httpRequest.clientIp, httpRequest.country <br>
| filter (httpRequest.country != 'BR') <br>
| sort requestCount desc <br>
| limit 50 <br>
