---
title: Restringir o uso simultâneo com vários aplicativos pertencentes a proprietários diferentes
description: Restrição do uso simultâneo com vários aplicativos pertencentes a proprietários diferentes
exl-id: c358befc-36d6-4f55-92cc-4d02064c65fb
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 1%

---

# Restrição do uso simultâneo com vários aplicativos pertencentes a proprietários diferentes {#restr-concurr-usage}


## Descrição do caso de uso

O MVPD M tem um aplicativo do iPhone, um aplicativo do iPad e um site. Ele precisa se integrar ao Adobe Concurrency Monitoring (CM) a pedido do Programador P. O Programador P definiu no CM um conjunto de políticas com regras que restringirão o uso simultâneo. O CM tomará decisões sobre quais fluxos podem ser reproduzidos com base nas políticas e regras definidas. Isso significa que, mesmo que um fluxo tenha sido permitido de iniciar e reproduzir, durante a reprodução, o CM pode decidir pará-lo.








## Pré-requisitos

Para integrar com o CM, um ticket do Zendesk (https://adobeprimetime.zendesk.com) terá que ser criado e as seguintes informações especificadas:

1. o nome da empresa
1. os aplicativos que você deseja integrar ao CM. Para cada aplicativo, você deve fornecer:
   - nome do aplicativo
   - plataforma de aplicativos
1. o terceiro que solicitou a integração (se aplicável)


Depois de criar o ticket, as seguintes informações serão liberadas para uso:

| type | descrição | exemplo de valor | valor padrão |
| --- | --- | --- | --- |
| endpoint | o terminal para o Monitoramento de simultaneidade do Adobe | http://streams.adobeprimetime.com/v1/ | http://streams.adobeprimetime.com/v1/ |
| applicationId | ID do aplicativo iPhone | iphone54-75b4-431b-adb2-eb6b9e546013 | - |
| applicationId | ID do aplicativo iPad | ipad5d54-75b4-431b-adb2-eb6b9e546013 | - |
| applicationId | ID do aplicativo do site | site4-75b4-431b-adb2-eb6b9e546013 | - |
| intervalo para pulsações | Intervalo em segundos para enviar chamadas de pulsação para o Monitoramento de simultaneidade do Adobe | 60 | 60 |
| intervalo para conformidade de fluxo | Intervalo em segundos para verificar a conformidade do fluxo com o Adobe Concurrency Monitoring | 180 | 180 |


## Diretrizes de implementação

Os seguintes itens DEVEM ser empacotados no(s) aplicativo(s):

1. endpoint
1. ID do aplicativo
1. intervalo para pulsações
1. intervalo para verificação de conformidade

<!---
## Workflows
 



 

1. Concurrency Monitoring allows playback
Step    APIs    Description    Comments
1    N/A    user starts video playback    
Prerequisites:

user is authenticated with an MVPD

2    API call: Session initialization    the app/player calls CM to initiate a stream    the app/player sends all required metadata using information from Adobe Pass Authentication
3    CM responds with decision and streamId    
the streamId json node value is stored by the application for the duration of the video playback

the policyCompliant json node has the value true so the app/player starts playback

the heartbeat url is returned by the call as a JSON Hypertext Application Language(HAL) template

4    N/A    the app/player starts video playback    N/A
5    API call: Heartbeat call - event=alive    the app/player reports alive heartbeats every x seconds    
the x value is specified by Adobe in the Zendesk ticket at integration

the url to report heartbeats is taken from the response of the start stream call and the value for the event param is alive

6    API call: Check Stream Compliance    the app/player checks the stream status every n seconds    
the n value is specified by Adobe in the Zendesk ticket at integration

stream status check is performed to validate that Adobe Concurrency Monitoring is allowing playback

7    CM responds with decision to continue playback    the policyCompliant json node has value true so the app/player continues playback
8    N/A    the app/player continues to show the video stream to the user    N/A
9    API call - Heartbeat call - alive=stop    the app/player sends stop heartbeat when the video finishes    the url to report heartbeats is taken from the response of the start stream call and the value for the event param is stop
 

Steps 5,6,7,8 will be performed in a loop until the video stream is stopped.

 

 

2. Concurrency Monitoring denies playback after stream is started
Step    APIs    Description    Comments
1    N/A    user starts video playback    
Prerequisites:

user is authenticated with an MVPD

2    API call: Session initialization    the app/player calls CM to initiate a stream    the app/player send all required metadata using information from Adobe Pass Authentication
3    CM responds with decision and streamId    
the streamId json node value is stored by the application for the duration of the video playback

the policyCompliant json node has value true so the app/player starts playback

the heartbeat url is returned by the call as a JSON Hypertext Application Language(HAL) template

4    N/A    the app/player starts video playback    N/A
5    API call: Heartbeat call - event=alive    the app/player reports alive heartbeats every x seconds    
the x value is specified by Adobe in the Zendesk ticket at integration

the url to report heartbeats is taken from the response of the start stream call and the value for the event param isalive

6    API call: Check Stream Compliance    the app/player checks the stream status every n seconds    
the n value is specified by Adobe in the Zendesk ticket at integration

stream status check is performed to validate that Adobe Concurrency Monitoring is allowing playback

7    CM responds with decision to stop playback    the policyCompliant json node has value false so the app/player stops playback
8    N/A    the app/player stops the video stream and displays an error to the user    N/A
9    API call - Heartbeat call - alive=stop    the app/player sends a stop heartbeat    the url to report heartbeats is taken from the response of the start stream call and the value for the event param is stop
 
3. Concurrency Monitoring doesn't allow playback
Step    APIs    Description    Comments
1    N/A    user starts video playback    
Prerequisites:

user is authenticated with an MVPD

2    API call: Session initialization    the app/player calls CM to initiate a stream    the app/player send all required metadata using information from Adobe Pass Authentication
3    CM responds with decision and streamId    
the policyCompliant json node has value false so the app/player does NOT start playback

4    N/A    
the app/player does NOT start video playback

it displays an error message to the user

N/A
 

 
Sequence diagram
 



 

 

 

Description of the API calls
Session initialization call
 

The responsibility of the application developer is to:

Initialize a stream with Concurrency Monitoring before starting video playback
check that the response of the call contains the node policyCompliant with value true
start video playback
 

API Console - initialize session

 

name
value
obtained from
endpoint    
http://streams.adobeprimetime.com/v1/
Zendesk ticket at integration

HTTP method    POST    documentation
URI path template    [appID]/[mvpd]/accounts/[account_id]/streams     
Parameter values
name
value example
where to use it
obtained from
applicationId    iphone54-75b4-431b-adb2-eb6b9e546013    uri    Zendesk ticket at integration
mvpdName    Sample_MVPD    uri    
Adobe Pass Authentication from config endpoint when user selects the MVPD

accountId    12345    uri    
Adobe Pass Authentication upstreamUserID metadata after user login

User Metadata upstreamUserID - Adobe Pass Authentication

programmerName    Sample_Programmer_Id    form parameter    Adobe Pass Authentication
channel    CHANNEL    form parameter    Adobe Pass Authentication
device_id    30fe8d0f-35d4-4082-a728-405fcbae5ddb    form parameter    Adobe Pass Authentication
 

 
Session initialization - example
 
```
-data 'programmer=PROGRAMMER_ID&device_id=30fe8d0f-35d4-4082-a728-405fcbae5ddb&channel=CHANNEL' 'http://streams.adobeprimetime.com/v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams'
 
> POST /v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams HTTP/1.1
> Host: streams.adobeprimetime.com
> Accept: */*
> Content-Type: application/x-www-form-urlencoded; charset=UTF-8
> Content-Length: 101
>
< HTTP/1.1 201 Created
< Date: Thu, 06 Aug 2015 08:56:47 GMT
< Cache-Control: no-cache, no-store, max-age=0, must-revalidate
< Pragma: no-cache
< Expires: 0
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET
< Access-Control-Allow-Methods: POST
< Access-Control-Allow-Methods: OPTIONS
< Access-Control-Allow-Methods: HEAD
< Access-Control-Allow-Methods: PUT
< Access-Control-Allow-Methods: DELETE
< Access-Control-Allow-Headers: Content-Type
< Location: http://streams.adobeprimetime.com/v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams/f__-sE4
< Content-Type: application/json; charset=UTF-8
< Content-Length: 511
< Age: 0
 
{
  "_links": {
    "self": {
      "href": "
    },
    "heartbeat": {
      "href": ",
      "templated": true
    }
  },
  "streamId": "ff__-sE4",
  "streamStart": 1438850042533,
  "policyCompliant": true
```


Heartbeat call - event=alive


The responsibility of the application developer is to:

Send heartbeats at the required interval in seconds. The interval will be specified in the Zendesk ticket. 

name
example value
obtained from
endpoint    
http://streams.adobeprimetime.com/v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams/f__-sE4
session initialization call

_links.heartbeat.href
HTTP method    POST    documentation
Parameter values
name
value example
where to use it
obtained from
event    alive    GET parameter    documentation
 

 
Heartbeat call - example
 
curl -v -X POST 'http://streams.adobeprimetime.com/v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams/f__-sE4?event=alive'
 
> POST /v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams/f__-sE4?event=alive HTTP/1.1
> Host: streams.adobeprimetime.com
> Accept: */*
>
< HTTP/1.1 202 Accepted
< Date: Thu, 06 Aug 2015 14:19:54 GMT
< Cache-Control: no-cache, no-store, max-age=0, must-revalidate
< Pragma: no-cache
< Expires: 0
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET
< Access-Control-Allow-Methods: POST
< Access-Control-Allow-Methods: OPTIONS
< Access-Control-Allow-Methods: HEAD
< Access-Control-Allow-Methods: PUT
< Access-Control-Allow-Methods: DELETE
< Access-Control-Allow-Headers: Content-Type
< Content-Length: 0
< Age: 0
 
 
Check Stream Compliance 
The responsibility of the application developer is to:

Call the API at required interval in seconds. The interval will be specified in the Zendesk ticket. 
Stop video playback if the policyCompliant json node has value false
Send heartbeat to stop stream Heartbeat call to stop stream
 

API Console - current state of the stream resource

name
example value
obtained from
endpoint    
http://streams.adobeprimetime.com/v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams/f__-sE4
session initialization call

_links.self.href
HTTP method    GET    documentation
Parameter values
name
value example
where to use it
obtained from
mvpdName    MVPD_ID    uri    
Adobe Pass Authentication from config endpoint when user selects the MVPD

accountId    12345    uri    
Adobe Pass Authentication upstreamUserID metadata after user login

User Metadata upstreamUserID - Adobe Pass Authentication

 

Check Stream Compliance - example
curl 'http://streams.adobeprimetime.com/v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams/f__-sE4'

```
> POST /v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams/f__-sE4 HTTP/1.1
> Host: streams.adobeprimetime.com
> Accept: */*
> Content-Type: application/x-www-form-urlencoded; charset=UTF-8
> Content-Length: 30
>
< HTTP/1.1 200 Created
< Date: Thu, 06 Aug 2015 08:56:47 GMT
< Cache-Control: no-cache, no-store, max-age=0, must-revalidate
< Pragma: no-cache
< Expires: 0
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET
< Access-Control-Allow-Methods: POST
< Access-Control-Allow-Methods: OPTIONS
< Access-Control-Allow-Methods: HEAD
< Access-Control-Allow-Methods: PUT
< Access-Control-Allow-Methods: DELETE
< Access-Control-Allow-Headers: Content-Type
< Location: http://streams.adobeprimetime.com/v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams/f__-sE4
< Content-Type: application/json; charset=UTF-8
< Content-Length: 511
< Age: 0
```

```
{
  "_links": {
    "self": {
      "href": "
    },
    "heartbeat": {
      "href": ",
      "templated": true
    }
  },
  "streamId": "ff__-sE4",
  "streamStart": 1438850042533,
  "policyCompliant": true
}
```

Heartbeat call - alive=stop
name
example value
obtained from
endpoint    
http://streams.adobeprimetime.com/v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams/f__-sE4
session initialization call

_links.heartbeat.href
HTTP method    POST    documentation
Parameter values
name
value example
where to use it
obtained from
event    stop    GET parameter    documentation
 

Heartbeat call - example
curl -v -X POST 'http://streams.adobeprimetime.com/v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams/f__-sE4?event=stop'

```
> POST /v1/iphone54-75b4-431b-adb2-eb6b9e546013/MVPD_ID/accounts/12345/streams/f__-sE4?event=stop HTTP/1.1
> Host: streams.adobeprimetime.com
> Accept: */*
>
< HTTP/1.1 202 Accepted
< Date: Thu, 06 Aug 2015 14:19:54 GMT
< Cache-Control: no-cache, no-store, max-age=0, must-revalidate
< Pragma: no-cache
< Expires: 0
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: GET
< Access-Control-Allow-Methods: POST
< Access-Control-Allow-Methods: OPTIONS
< Access-Control-Allow-Methods: HEAD
< Access-Control-Allow-Methods: PUT
< Access-Control-Allow-Methods: DELETE
< Access-Control-Allow-Headers: Content-Type
< Content-Length: 0
< Age: 0
```

Related Information
Introduction - Adobe Concurrency Monitoring
API Console - Adobe Concurrency Monitoring
User Metadata - Adobe Pass Authentication
--->
