# AWS_CloudWatch_LogsInsights_Querys
AWS_CloudWatch_LogsInsights_Querys

**Filtrar IP Origem, URI e Ação**
fields @timestamp, @message, @logStream, @log
| stats count(*) as qty by httpRequest.clientIp, httpRequest.uri, action
| sort qty desc

**Fitrar Token autenticação vazio**
fields @timestamp, @message, @logStream, @log
| filter @message like '"value":"Bearer"'
| stats count(*) as qty by httpRequest.clientIp, action
| sort qty desc

**Filtrar path de API**
fields @timestamp, @message, @logStream, @log
| sort @timestamp desc
| filter httpRequest.uri = '/api/v1/login'
| stats count(*) by action
| limit 10000

**Filtrar por país de origem do tráfego**
fields @timestamp, httpRequest.clientIp 
| stats count(*) as requestCount by httpRequest.clientIp, httpRequest.country
| filter (httpRequest.country != 'BR')
| sort requestCount desc 
| limit 50
