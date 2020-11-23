# Session Initiation Protocol (SIP) Basic Call Flow Examples


Network Working Group                                        A. Johnston
Request for Comments: 3665                                           MCI
BCP: 75                                                       S. Donovan
Category: Best Current Practice                                R. Sparks
                                                           C. Cunningham
                                                             dynamicsoft
                                                              K. Summers
                                                                   Sonus
                                                           December 2003


Статус документа

Этот документ определяет лучшие текущие практики интернета для интернет-сообщества и запрашивает обсуждение и предложения по улучшению. Распространение данной памятки не ограничено.

Уведомление Об Авторских Правах

Copyright (C) The Internet Society (2003). все права защищены.

Абстрактно

В этом документе приведены примеры потоков вызовов протокола установления сеанса (SIP). Элементы в этих потоках вызовов включают SIP-агенты пользователей и клиентов, SIP-прокси и серверы перенаправления. Сценарии включают регистрацию SIP и создание сеанса SIP. Показаны схемы потоков вызовов и сведения о сообщениях.

## Содержание

1.  Overview . . . . . . . . . . . . . . . . . . . . . . . . . . .  2

  1.1.  General Assumptions. . . . . . . . . . . . . . . . . . .  3

  1.2.  Legend for Message Flows . . . . . . . . . . . . . . . .  3

  1.3.  SIP Protocol Assumptions . . . . . . . . . . . . . . . .  4

2.  SIP Registration . . . . . . . . . . . . . . . . . . . . . . .  4

  2.1.  Successful New Registration. . . . . . . . . . . . . . .  5

  2.2.  Update of Contact List . . . . . . . . . . . . . . . . .  7

  2.3.  Request for Current Contact List . . . . . . . . . . . .  8

  2.4.  Cancellation of Registration . . . . . . . . . . . . . .  9

  2.5.  Unsuccessful Registration. . . . . . . . . . . . . . . . 10

3.  SIP Session Establishment. . . . . . . . . . . . . . . . . . . 12

  3.1.  Successful Session Establishment . . . . . . . . . . . . 12

  3.2.  Session Establishment Through Two Proxies. . . . . . . . 15

  3.3.  Session with Multiple Proxy Authentication . . . . . . . 26

  3.4.  Successful Session with Proxy Failure. . . . . . . . . . 37

  3.5.  Session Through a SIP ALG. . . . . . . . . . . . . . . . 46

  3.6.  Session via Redirect and Proxy Servers with SDP in ACK . 54

  3.7.  Session with re-INVITE (IP Address Change) . . . . . . . 61

  3.8.  Unsuccessful No Answer . . . . . . . . . . . . . . . . . 67

  3.9.  Unsuccessful Busy. . . . . . . . . . . . . . . . . . . . 75

  3.10. Unsuccessful No Response from User Agent . . . . . . . . 80

  3.11. Unsuccessful Temporarily Unavailable . . . . . . . . . . 85

4.  Security Considerations. . . . . . . . . . . . . . . . . . . . 91

5. References . . . . . . . . . . . . . . . . . . . . . . . . . . 91

  5.1.  Normative References . . . . . . . . . . . . . . . . . . 91

  5.2.  Informative References . . . . . . . . . . . . . . . . . 91

6.  Intellectual Property Statement. . . . . . . . . . . . . . . . 91

7.  Acknowledgments. . . . . . . . . . . . . . . . . . . . . . . . 92

8.  Authors' Addresses . . . . . . . . . . . . . . . . . . . . . . 93

9.  Full Copyright Statement . . . . . . . . . . . . . . . . . . . 94

## 1.Обзор

Потоки вызовов, показанные в этом документе, были созданы при проектировании сети связи SIP IP. Они представляют собой пример минимального набора функциональных возможностей.

Авторы надеются, что этот документ будет полезен как разработчикам SIP, так и разработчикам протоколов и поможет в дальнейшем достижении цели стандартной реализации RFC 3261[1]. Эти потоки представляют собой тщательно проверенные и рассмотренные рабочей группой сценарии наиболее основных примеров в качестве дополнения к спецификациям.

Эти потоки вызовов основаны на текущей версии 2.0 SIP в RFC 3261[1] с использованием SDP, описанным в RFC 3264[2]. Другие RFC также включают стандарт SIP, но не используются в этом наборе основных потоков вызовов.

Показаны примеры взаимодействия потока SIP с ТфОП через шлюзы, содержащиеся в сопровождающем документе, документе RFC 3666[5].

Ключевые слова "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY" и "OPTIONAL" в настоящем документе следует толковать так, как описано в BCP 14, RFC 2119[4].

### 1.1 Главный постулат

В основе потоков вызовов в этом документе лежит ряд гипотез об архитектуре, сети и протоколе. Обратите внимание, что эти гипотезы не являются требованиями. Они изложены в этом разделе чтобы их можно было принять во внимание и помочь в понимании примеров потока вызовов.

Аутентификация пользовательских SIP-агентов в этих примерах потоков вызовов выполняется с использованием HTTP Digest, как определено в [1] и [3].

Некоторые прокси-серверы в этих потоках вызовов вставляют заголовки маршрута записи в запросы, чтобы гарантировать, что они находятся в сигнальном пути для будущих обменов сообщениями.

Эти потоки показывают TCP, TLS и UDP для транспорта. См. обсуждение в RFC 3261 для получения подробной информации о транспортных проблемах для SIP.

### 1.2. Legend for Message Flows

   Dashed lines (---) represent signaling messages that are mandatory to
   the call scenario.  These messages can be SIP or PSTN signaling.  The
   arrow indicates the direction of message flow.

   Double dashed lines (===) represent media paths between network
   elements.

   Messages with parentheses around their name represent optional
   messages.

   Messages are identified in the Figures as F1, F2, etc.  This
   references the message details in the list that follows the Figure.
   Comments in the message details are shown in the following form:

    /* Comments. \*/

### 1.3 SIP Protocol Assumptions

   This document does not prescribe the flows precisely as they are
   shown, but rather the flows illustrate the principles for best
   practice.  They are best practices usages (orderings, syntax,
   selection of features for the purpose, handling of error) of SIP
   methods, headers and parameters.  IMPORTANT: The exact flows here
   must not be copied as is by an implementer due to specific incorrect
   characteristics that were introduced into the document for
   convenience and are listed below.  To sum up, the basic flows
   represent well-reviewed examples of SIP usage, which are best common
   practice according to IETF consensus.

   For simplicity in reading and editing the document, there are a
   number of differences between some of the examples and actual SIP
   messages.  For example, the HTTP Digest responses are not actual MD5
   encodings.  Call-IDs are often repeated, and CSeq counts often begin
   at 1.  Header fields are usually shown in the same order.  Usually
   only the minimum required header field set is shown, others that
   would normally be present such as Accept, Supported, Allow, etc are
   not shown.

   Actors:

   Element       Display Name   URI                         IP Address
   -------       ------------   ---                         ----------

   User Agent    Alice          alice@atlanta.example.com   192.0.2.101
   User Agent    Bob            bob@biloxi.example.com      192.0.2.201
   User Agent                   bob@chicago.example.com     192.0.2.100
   Proxy Server                 ss1.atlanta.example.com     192.0.2.111
   Proxy/Registrar              ss2.biloxi.example.com      192.0.2.222
   Proxy Server                 ss3.chicago.example.com     192.0.2.233
   ALG                          alg1.atlanta.example.com    192.0.2.128

## 2. SIP Registration

   Registration binds a particular device Contact URI with a SIP user
   Address of Record (AOR).

### 2.1 Successful New Registration

```
    Bob                        SIP Server
     |                               |
     |          REGISTER F1          |
     |------------------------------>|
     |      401 Unauthorized F2      |
     |<------------------------------|
     |          REGISTER F3          |
     |------------------------------>|
     |            200 OK F4          |
     |<------------------------------|
     |                               |
```

Bob sends a SIP REGISTER request to the SIP server.  The request includes the user's contact list.  This flow shows the use of HTTP Digest for authentication using TLS transport.  TLS transport is used due to the lack of integrity protection in HTTP Digest and the danger of registration hijacking without it, as described in RFC 3261 [1].
The SIP server provides a challenge to Bob.  Bob enters her/his valid user ID and password.  Bob's SIP client encrypts the user information according to the challenge issued by the SIP server and sends the response to the SIP server.  The SIP server validates the user's credentials.  It registers the user in its contact database and returns a response (200 OK) to Bob's SIP client.  The response includes the user's current contact list in Contact headers.  The format of the authentication shown is HTTP digest.  It is assumed that Bob has not previously registered with this Server.

   Message Details

   F1 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
   Max-Forwards: 70
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>
   Content-Length: 0


   F2 401 Unauthorized SIP Server -> Bob

   SIP/2.0 401 Unauthorized
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>;tag=1410948204
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   WWW-Authenticate: Digest realm="atlanta.example.com", qop="auth",
    nonce="ea9c8e88df84f1cec4341ae6cbe5a359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0


   F3 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashd92
   Max-Forwards: 70
   From: Bob <sips:bob@biloxi.example.com>;tag=ja743ks76zlflH
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 2 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>
   Authorization: Digest username="bob", realm="atlanta.example.com"
    nonce="ea9c8e88df84f1cec4341ae6cbe5a359", opaque="",
    uri="sips:ss2.biloxi.example.com",
    response="dfe56131d1958046689d83306477ecc"
   Content-Length: 0


   F4 200 OK SIP Server -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashd92
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=ja743ks76zlflH
   To: Bob <sips:bob@biloxi.example.com>;tag=37GkEhwl6
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 2 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>;expires=3600
   Content-Length: 0

### 2.2. Update of Contact List
```
   Bob                        SIP Server
     |                               |
     |          REGISTER F1          |
     |------------------------------>|
     |            200 OK F2          |
     |<------------------------------|
     |                               |
```

Bob wishes to update the list of addresses where the SIP server will redirect or forward INVITE requests.

Bob sends a SIP REGISTER request to the SIP server.  Bob's request includes an updated contact list.  Since the user already has authenticated with the server, the user supplies authentication credentials with the request and is not challenged by the server. The SIP server validates the user's credentials.  It registers the user in its contact database, updates the user's contact list, and returns a response (200 OK) to Bob's SIP client.  The response includes the user's current contact list in Contact headers.

   Message Details

   F1 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
   Max-Forwards: 70
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Contact: mailto:bob@biloxi.example.com
   Authorization: Digest username="bob", realm="atlanta.example.com",
    qop="auth", nonce="1cec4341ae6cbe5a359ea9c8e88df84f", opaque="",
    uri="sips:ss2.biloxi.example.com",
    response="71ba27c64bd01de719686aa4590d5824"
   Content-Length: 0


   F2 200 OK SIP Server -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>;tag=34095828jh

   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>;expires=3600
   Contact: <mailto:bob@biloxi.example.com>;expires=4294967295
   Content-Length: 0

### 2.3. Request for Current Contact List

   Bob                        SIP Server
     |                               |
     |          REGISTER F1          |
     |------------------------------>|
     |            200 OK F2          |
     |<------------------------------|
     |                               |

   Bob sends a register request to the Proxy Server containing no
   Contact headers, indicating the user wishes to query the server for
   the user's current contact list.  Since the user already has
   authenticated with the server, the user supplies authentication
   credentials with the request and is not challenged by the server.
   The SIP server validates the user's credentials.  The server returns
   a response (200 OK) which includes the user's current registration
   list in Contact headers.

   Message Details

   F1 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
   Max-Forwards: 70
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Authorization: Digest username="bob", realm="atlanta.example.com",
    nonce="df84f1cec4341ae6cbe5ap359a9c8e88", opaque="",
    uri="sips:ss2.biloxi.example.com",
    response="aa7ab4678258377c6f7d4be6087e2f60"
   Content-Length: 0


   F2 200 OK SIP Server -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
    ;received=192.0.2.201

   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>;tag=jqoiweu75
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>;expires=3600
   Contact: <mailto:bob@biloxi.example.com>;expires=4294967295
   Content-Length: 0

### 2.4. Cancellation of Registration
```
   Bob                         SIP Server
     |                               |
     |          REGISTER F1          |
     |------------------------------>|
     |            200 OK F2          |
     |<------------------------------|
     |                               |
```

Bob wishes to cancel their registration with the SIP server.  Bob sends a SIP REGISTER request to the SIP server.  The request has an expiration period of 0 and applies to all existing contact locations. Since the user already has authenticated with the server, the user supplies authentication credentials with the request and is not challenged by the server.  The SIP server validates the user's credentials.  It clears the user's contact list, and returns a response (200 OK) to Bob's SIP client.

   Message Details

   F1 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
   Max-Forwards: 70
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Expires: 0
   Contact: *
   Authorization: Digest username="bob", realm="atlanta.example.com",
    nonce="88df84f1cac4341aea9c8ee6cbe5a359", opaque="",
    uri="sips:ss2.biloxi.example.com",
    response="ff0437c51696f9a76244f0cf1dbabbea"
   Content-Length: 0


   F2 200 OK SIP Server -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>;tag=1418nmdsrf
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Content-Length: 0

2.5.  Unsuccessful Registration

   Bob                        SIP Server
     |                               |
     |          REGISTER F1          |
     |------------------------------>|
     |      401 Unauthorized F2      |
     |<------------------------------|
     |          REGISTER F3          |
     |------------------------------>|
     |      401 Unauthorized F4      |
     |<------------------------------|
     |                               |

   Bob sends a SIP REGISTER request to the SIP Server.  The SIP server
   provides a challenge to Bob.  Bob enters her/his user ID and
   password.  Bob's SIP client encrypts the user information according
   to the challenge issued by the SIP server and sends the response to
   the SIP server.  The SIP server attempts to validate the user's
   credentials, but they are not valid (the user's password does not
   match the password established for the user's account).  The server
   returns a response (401 Unauthorized) to Bob's SIP client.

   Message Details

   F1 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>
   Content-Length: 0


   F2 Unauthorized SIP Server -> Bob

   SIP/2.0 401 Unauthorized
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=a73kszlfl
   To: Bob <sips:bob@biloxi.example.com>;tag=1410948204
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 1 REGISTER
   WWW-Authenticate: Digest realm="atlanta.example.com", qop="auth",
    nonce="f1cec4341ae6ca9c8e88df84be55a359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0


   F3 REGISTER Bob -> SIP Server

   REGISTER sips:ss2.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashd92
   Max-Forwards: 70
   From: Bob <sips:bob@biloxi.example.com>;tag=JueHGuidj28dfga
   To: Bob <sips:bob@biloxi.example.com>
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 2 REGISTER
   Contact: <sips:bob@client.biloxi.example.com>
   Authorization: Digest username="bob", realm="atlanta.example.com",
    nonce="f1cec4341ae6ca9c8e88df84be55a359", opaque="",
    uri="sips:ss2.biloxi.example.com",
    response="61f8470ceb87d7ebf508220214ed438b"
   Content-Length: 0

   /*  The response above encodes the incorrect password */


   F4 401 Unauthorized SIP Server -> Bob

   SIP/2.0 401 Unauthorized
   Via: SIP/2.0/TLS client.biloxi.example.com:5061;branch=z9hG4bKnashd92
    ;received=192.0.2.201
   From: Bob <sips:bob@biloxi.example.com>;tag=JueHGuidj28dfga
   To: Bob <sips:bob@biloxi.example.com>;tag=1410948204
   Call-ID: 1j9FpLxk3uxtm8tn@biloxi.example.com
   CSeq: 2 REGISTER
   WWW-Authenticate: Digest realm="atlanta.example.com", qop="auth",
    nonce="84f1c1ae6cbe5ua9c8e88dfa3ecm3459",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0

3.  SIP Session Establishment

   This section details session establishment between two SIP User
   Agents (UAs): Alice and Bob.  Alice (sip:alice@atlanta.example.com)
   and Bob (sip:bob@biloxi.example.com) are assumed to be SIP phones or
   SIP-enabled devices.  The successful calls show the initial
   signaling, the exchange of media information in the form of SDP
   payloads, the establishment of the media session, then finally the
   termination of the call.

   HTTP Digest authentication is used by Proxy Servers to authenticate
   the caller Alice.  It is assumed that Bob has registered with Proxy
   Server Proxy 2 as per Section 2 to be able to receive the calls via
   the Proxy.

3.1.  Successful Session Establishment

   Alice                     Bob
     |                        |
     |       INVITE F1        |
     |----------------------->|
     |    180 Ringing F2      |
     |<-----------------------|
     |                        |
     |       200 OK F3        |
     |<-----------------------|
     |         ACK F4         |
     |----------------------->|
     |   Both Way RTP Media   |
     |<======================>|
     |                        |
     |         BYE F5         |
     |<-----------------------|
     |       200 OK F6        |
     |----------------------->|
     |                        |

   In this scenario, Alice completes a call to Bob directly.

   Message Details

   F1 INVITE Alice -> Bob

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>

   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F2 180 Ringing Bob -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=8321234356
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   Content-Length: 0


   F3 200 OK Bob -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=8321234356
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F4 ACK Alice -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bd5
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=8321234356
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0

   /* RTP streams are established between Alice and Bob */

   /* Bob Hangs Up with Alice. Note that the CSeq is NOT 2, since
      Alice and Bob maintain their own independent CSeq counts.
      (The INVITE was request 1 generated by Alice, and the BYE is
      request 1 generated by Bob) */


   F5 BYE Bob -> Alice

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
   Max-Forwards: 70
   From: Bob <sip:bob@biloxi.example.com>;tag=8321234356
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0

   F6 200 OK Alice -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sip:bob@biloxi.example.com>;tag=8321234356
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0

3.2.  Session Establishment Through Two Proxies

   Alice           Proxy 1          Proxy 2            Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|                |                |
     |     407 F2     |                |                |
     |<---------------|                |                |
     |     ACK F3     |                |                |
     |--------------->|                |                |
     |   INVITE F4    |                |                |
     |--------------->|   INVITE F5    |                |
     |     100  F6    |--------------->|   INVITE F7    |
     |<---------------|     100  F8    |--------------->|
     |                |<---------------|                |
     |                |                |     180 F9     |
     |                |    180 F10     |<---------------|
     |     180 F11    |<---------------|                |
     |<---------------|                |     200 F12    |
     |                |    200 F13     |<---------------|
     |     200 F14    |<---------------|                |
     |<---------------|                |                |
     |     ACK F15    |                |                |
     |--------------->|    ACK F16     |                |
     |                |--------------->|     ACK F17    |
     |                |                |--------------->|
     |                Both Way RTP Media                |
     |<================================================>|
     |                |                |     BYE F18    |
     |                |    BYE F19     |<---------------|
     |     BYE F20    |<---------------|                |
     |<---------------|                |                |
     |     200 F21    |                |                |
     |--------------->|     200 F22    |                |
     |                |--------------->|     200 F23    |
     |                |                |--------------->|
     |                |                |                |

   In this scenario, Alice completes a call to Bob using two proxies
   Proxy 1 and Proxy 2.  The initial INVITE (F1) contains a pre-loaded
   Route header with the address of Proxy 1 (Proxy 1 is configured as a
   default outbound proxy for Alice).  The request does not contain the
   Authorization credentials Proxy 1 requires, so a 407 Proxy
   Authorization response is sent containing the challenge information.
   A new INVITE (F4) is then sent containing the correct credentials and
   the call proceeds.  The call terminates when Bob disconnects by
   initiating a BYE message.

   Proxy 1 inserts a Record-Route header into the INVITE message to
   ensure that it is present in all subsequent message exchanges.  Proxy
   2 also inserts itself into the Record-Route header.  The ACK (F15)
   and BYE (F18) both have a Route header.

   Message Details

   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74b43
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 1 challenges Alice for authentication */


   F2 407 Proxy Authorization Required Proxy 1 -> Alice

   SIP/2.0 407 Proxy Authorization Required
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74b43
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=3flal12sf
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 INVITE
   Proxy-Authenticate: Digest realm="atlanta.example.com", qop="auth",
    nonce="f84f1cec41e6cbe5aea9c8e88d359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0




   F3 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74b43
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=3flal12sf
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0

   /* Alice responds be re-sending the INVITE with authentication
      credentials in it. */


   F4 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="wf84f1ceczx41ae6cbe5aea9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="42ce3cef44b22f50c6a6071bc8"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 1 accepts the credentials and forwards the INVITE to Proxy
   2.  Client for Alice prepares to receive data on port 49172 from the
   network. */

   F5 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F6 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Content-Length: 0


   F7 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 68
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>

   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F8 100 Trying Proxy 2 -> Proxy 1

   SIP/2.0 100 Trying
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Content-Length: 0


   F9 180 Ringing Bob -> Proxy 2

   SIP/2.0 180 Ringing
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   CSeq: 2 INVITE
   Content-Length: 0

   F10 180 Ringing Proxy 2 -> Proxy 1

   SIP/2.0 180 Ringing
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   CSeq: 2 INVITE
   Content-Length: 0


   F11 180 Ringing Proxy 1 -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   CSeq: 2 INVITE
   Content-Length: 0


   F12 200 OK Bob -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE

   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F13 200 OK Proxy 2 -> Proxy 1

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F14 200 OK Proxy 1 -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159

   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.biloxi.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F15 ACK Alice -> Proxy 1

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74b76
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>,
    <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 ACK
   Content-Length: 0


   F16 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74b76
    ;received=192.0.2.101
   Max-Forwards: 69
   Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 ACK
   Content-Length: 0


   F17 ACK Proxy 2 -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1

   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74b76
    ;received=192.0.2.101
   Max-Forwards: 68
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 2 ACK
   Content-Length: 0

   /* RTP streams are established between Alice and Bob */

   /* Bob Hangs Up with Alice. */

   /* Again, note that the CSeq is NOT 3.  Alice and Bob maintain
      their own separate CSeq counts */


   F18 BYE Bob -> Proxy 2

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
   Max-Forwards: 70
   Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F19 BYE Proxy 2 -> Proxy 1

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   Max-Forwards: 69
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0

   F20 BYE Proxy 1 -> Alice

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   Max-Forwards: 68
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F21 200 OK Alice -> Proxy 1

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F22 200 OK Proxy 1 -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.101
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0

   F23 200 OK Proxy 2 -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 3848276298220188511@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0

3.3.  Session with Multiple Proxy Authentication

     Alice        Proxy 1     Proxy 2         Bob
       |            |           |             |
       |  INVITE F1 |           |             |
       |----------->|           |             |
       |  407 Proxy Authorization Required F2 |
       |<-----------|           |             |
       |   ACK F3   |           |             |
       |----------->|           |             |
       |  INVITE F4 |           |             |
       |----------->|           |             |
       |   100 F5   |           |             |
       |<-----------| INVITE F6 |             |
       |            |---------->|             |
       |            |  407 Proxy Authorization Required F7
       |            |<----------|             |
       |            |   ACK F8  |             |
       |            |---------->|             |
       |  407 Proxy Authorization Required F9 |
       |<-----------|           |             |
       |   ACK F10  |           |             |
       |----------->|           |             |
       |  INVITE F11|           |             |
       |----------->|           |             |
       |   100 F12  |           |             |
       |<-----------| INVITE F13|             |
       |            |---------->|             |
       |            |  100 F14  |             |
       |            |<----------|  INVITE F15 |
       |            |           |------------>|
       |            |           | 200 OK F16  |
       |            | 200 OK F17|<------------|
       | 200 OK F18 |<----------|             |
       |<-----------|           |             |
       |   ACK F19  |           |             |
       |----------->|  ACK F20  |             |
       |            |---------->|   ACK F21   |
       |            |           |------------>|
       |           RTP Media Path             |
       |<====================================>|

In this scenario, Alice completes a call to Bob using two proxies Proxy 1 and Proxy 2.  Alice has valid credentials in both domains. Since the initial INVITE (F1) does not contain the Authorization credentials Proxy 1 requires, so a 407 Proxy Authorization response is sent containing the challenge information. A new INVITE (F4) is then sent containing the correct credentials and the call proceeds after Proxy 2 challenges and receives valid credentials.  The call terminates when Bob disconnects by initiating a BYE message.

Proxy 1 inserts a Record-Route header into the INVITE message to ensure that it is present in all subsequent message exchanges.  Proxy 2 also inserts itself into the Record-Route header.

   Message Details

   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b03
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 1 challenges Alice for authentication */


   F2 407 Proxy Authorization Required Proxy 1 -> Alice

   SIP/2.0 407 Proxy Authorization Required
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b03
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=876321
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Proxy-Authenticate: Digest realm="atlanta.example.com", qop="auth",
    nonce="wf84f1cczx41ae6cbeaea9ce88d359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0

   F3 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Max-Forwards: 70
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b03
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=876321
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0

   /* Alice responds be re-sending the INVITE with authentication
      credentials in it.  The same Call-ID is used, so the CSeq is
      increased. */


   F4 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="wf84f1ceczx41ae6cbe5aea9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="42ce3cef44b22f50c6a6071bc8"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 1 accepts the credentials and forwards the INVITE to Proxy
   2.  Client for Alice prepares to receive data on port 49172 from the
   network. */

   F5 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Content-Length: 0


   F6 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 2 challenges Alice for authentication */


   F7 407 Proxy Authorization Required Proxy 2 -> Proxy 1

   SIP/2.0 407 Proxy Authorization Required
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
    ;received=192.0.2.101

   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=838209
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Proxy-Authenticate: Digest realm="biloxi.example.com", qop="auth",
    nonce="c1e22c41ae6cbe5ae983a9c8e88d359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0


   F8 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=838209
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 ACK
   Content-Length: 0

   /* Proxy 1 forwards the challenge to Alice for authentication from
   Proxy 2 */


   F9 407 Proxy Authorization Required Proxy 1 -> Alice

   SIP/2.0 407 Proxy Authorization Required
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=838209
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Proxy-Authenticate: Digest realm="biloxi.example.com", qop="auth",
    nonce="c1e22c41ae6cbe5ae983a9c8e88d359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0


   F10 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b21
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=838209
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com

   CSeq: 2 ACK
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="wf84f1ceczx41ae6cbe5aea9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="42ce3cef44b22f50c6a6071bc8"
   Content-Length: 0

   /* Alice responds be re-sending the INVITE with authentication
   credentials for Proxy 1 AND Proxy 2.  */


   F11 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="wf84f1ceczx41ae6cbe5aea9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="42ce3cef44b22f50c6a6071bc8"
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",
    nonce="c1e22c41ae6cbe5ae983a9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com", response="f44ab22f150c6a56071bce8"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 1 finds its credentials and authorizes Alice, forwarding the
   INVITE to Proxy.  */

   F12 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Content-Length: 0


   F13 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",
    nonce="c1e22c41ae6cbe5ae983a9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com", response="f44ab22f150c6a56071bce8"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 2 finds its credentials and authorizes Alice, forwarding the
   INVITE to Bob.  */

   F14 100 Trying Proxy 2 -> Proxy 1

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Content-Length: 0


   F15 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK31972.1
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 68
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Bob answers the call immediately */

   F16 200 OK Bob -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK31972.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=9103874
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F17 200 OK Proxy 2 -> Proxy 1

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=9103874
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-

   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F18 200 OK Proxy 1 -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=9103874
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F19 ACK Alice -> Proxy 1

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b44
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>,
    <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=9103874
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 ACK
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="wf84f1ceczx41ae6cbe5aea9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="42ce3cef44b22f50c6a6071bc8"
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",

    nonce="c1e22c41ae6cbe5ae983a9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com", response="f44ab22f150c6a56071bce8"
   Content-Length: 0


   F20 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b44
    ;received=192.0.2.101
   Max-Forwards: 69
   Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=9103874
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 ACK
   Contact: <sip:bob@client.biloxi.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",
    nonce="c1e22c41ae6cbe5ae983a9c8e88d359", opaque="",
    uri="sip:bob@biloxi.example.com", response="f44ab22f150c6a56071bce8"
   Content-Length: 0


   F21 ACK Proxy 2 -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK31972.1
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK230f2.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b44
    ;received=192.0.2.101
   Max-Forwards: 68
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=9103874
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 3 ACK
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0

3.4.  Successful Session with Proxy Failure

    Alice           Proxy 1          Proxy 2            Bob
      |                |                |                |
      |   INVITE F1    |                |                |
      |--------------->|                |                |
      |   INVITE F2    |                |                |
      |--------------->|                |                |
      |   INVITE F3    |                |                |
      |--------------->|                |                |
      |   INVITE F4    |                |                |
      |--------------->|                |                |
      |   INVITE F5    |                |                |
      |--------------->|                |                |
      |   INVITE F6    |                |                |
      |--------------->|                |                |
      |   INVITE F7    |                |                |
      |--------------->|                |                |
      |     INVITE F8                   |                |
      |-------------------------------->|                |
      |            407 F9               |                |
      |<--------------------------------|                |
      |             ACK F10             |                |
      |-------------------------------->|                |
      |           INVITE F11            |                |
      |-------------------------------->|   INVITE F12   |
      |             100  F13            |--------------->|
      |<--------------------------------|                |
      |                                 |     180 F14    |
      |             180 F15             |<---------------|
      |<--------------------------------|                |
      |                                 |     200 F16    |
      |             200 F17             |<---------------|
      |<--------------------------------|                |
      |             ACK F18             |                |
      |-------------------------------->|     ACK F19    |
      |                                 |--------------->|
      |                Both Way RTP Media                |
      |<================================================>|
      |                                 |     BYE F20    |
      |             BYE F21             |<---------------|
      |<--------------------------------|                |
      |             200 F22             |                |
      |-------------------------------->|     200 F23    |
      |                                 |--------------->|
      |                                 |                |


In this scenario, Alice completes a call to Bob via a Proxy Server. Alice is configured for a primary SIP Proxy Server Proxy 1 and a secondary SIP Proxy Server Proxy 2 (Or is able to use DNS SRV records to locate Proxy 1 and Proxy 2).  Alice has valid credentials for both domains.  Proxy 1 is out of service and does not respond to INVITEs (it is reachable, but unresponsive).  Alice then completes the call to Bob using Proxy 2.

   Message Details

   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK465b6d
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F2 INVITE Alice -> Proxy 1

   Same as Message F1


   F3 INVITE Alice -> Proxy 1

   Same as Message F1


   F4 INVITE Alice -> Proxy 1

   Same as Message F1

   F5 INVITE Alice -> Proxy 1

   Same as Message F1


   F6 INVITE Alice -> Proxy 1

   Same as Message F1


   F7 INVITE Alice -> Proxy 1

   Same as Message F1

   /* Alice gives up on the unresponsive proxy */


   F8 INVITE Alice -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b8a
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 2 challenges Alice for authentication */


   F9 407 Proxy Authorization Required Proxy 2 -> Alice

   SIP/2.0 407 Proxy Authorization Required
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b8a
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=2421452

   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 INVITE
   Proxy-Authenticate: Digest realm="biloxi.example.com", qop="auth",
    nonce="1ae6cbe5ea9c8e8df84fqnlec434a359",
    opaque="", stale=FALSE, algorithm=MD5
   Content-Length: 0


   F10 ACK Alice -> Proxy 2

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b8a
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=2421452
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0

   /* Alice responds by re-sending the INVITE with authentication
   credentials in it.  */


   F11 INVITE Alice -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",
    nonce="1ae6cbe5ea9c8e8df84fqnlec434a359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="8a880c919d1a52f20a1593e228adf599"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Proxy 2 accepts the credentials and forwards the INVITE to Bob.
   Client for Alice prepares to receive data on port 49172 from the
   network.
   */


   F12 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F13 100 Trying Proxy 2 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Content-Length: 0


   F14 180 Ringing Bob -> Proxy 2

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222



                 [Page 41]





   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F15 180 Ringing Proxy 2 -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F16 200 OK Bob -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000



                 [Page 42]







   F17 200 OK Proxy 2 -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F18 ACK Alice -> Proxy 2

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b8g
   Max-Forwards: 70
   Route: <sip:ss2.biloxi.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 2 ACK
   Content-Length: 0


   F19 ACK Proxy 2 -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b8g
    ;received=192.0.2.101
   Max-Forwards: 69
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com



                 [Page 43]





   CSeq: 2 ACK
   Content-Length: 0

   /* RTP streams are established between Alice and Bob */

   /* Bob Hangs Up with Alice. */


   F20 BYE Bob -> Proxy 2

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/UDP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
   Max-Forwards: 70
   Route: <sip:ss2.biloxi.example.com;lr>
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F21 BYE Proxy 2 -> Alice

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/UDP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   Max-Forwards: 69
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F22 200 OK Alice -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0




                 [Page 44]






   F23 200 OK Proxy 2 -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.biloxi.example.com:5060;branch=z9hG4bKnashds7
    ;received=192.0.2.201
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 4Fde34wkd11wsGFDs3@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0








































                 [Page 45]





3.5.  Session Through a SIP ALG

   Alice             ALG           Proxy 2            Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|   INVITE F2    |                |
     |     100 F3     |--------------->|   INVITE F4    |
     |<---------------|     100 F5     |--------------->|
     |                |<---------------|      180 F6    |
     |                |     180 F7     |<---------------|
     |     180 F8     |<---------------|                |
     |<---------------|                |      200 F9    |
     |                |    200 F10     |<---------------|
     |     200 F11    |<---------------|                |
     |<---------------|                                 |
     |     ACK F12    |                                 |
     |--------------->|             ACK F13             |
     |                |-------------------------------->|
     |    RTP Media   |        Both Way RTP Media       |
     |<==============>|<===============================>|
     |     BYE F14    |                                 |
     |--------------->|             BYE F15             |
     |                |-------------------------------->|
     |                |             200 F16             |
     |     200 F17    |<--------------------------------|
     |<---------------|                                 |
     |                |                                 |

   Alice completes a call to Bob through a ALG (Application Layer
   Gateway) and a SIP Proxy.  The routing through the ALG is
   accomplished using a pre-loaded Route header in the INVITE F1.  Note
   that the media stream setup is not end-to-end - the ALG terminates
   both media streams and bridges them.  This is done by the ALG
   modifying the SDP in the INVITE (F1) and 200 OK (F10) messages, and
   possibly any 18x or ACK messages containing SDP.

   In addition to firewall traversal, this Back-to-Back User Agent
   (B2BUA) could be used as part of an anonymizer service (in which all
   identifying information on Alice would be removed), or to perform
   codec media conversion, such as mu-law to A-law conversion of PCM on
   an international call.

   Also note that Proxy 2 does not Record-Route in this call flow.








                 [Page 46]





   Message Details

   F1 INVITE Alice -> SIP ALG

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Route: <sip:alg1.atlanta.example.com;lr>
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",
    nonce="85b4f1cen4341ae6cbe5a3a9c8e88df9", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="b3f392f9218a328b9294076d708e6815"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* Client for Alice prepares to receive data on port 49172 from the
   network. */


   F2 INVITE SIP ALG -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="biloxi.example.com",



                 [Page 47]





    nonce="85b4f1cen4341ae6cbe5a3a9c8e88df9", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="b3f392f9218a328b9294076d708e6815"
   Content-Type: application/sdp
   Content-Length: 150

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.128
   t=0 0
   m=audio 2000 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F3 100 Trying SIP ALG -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0

   /* SIP ALG prepares to proxy data from port 192.0.2.128/2000 to
   192.0.2.101/49172.   Proxy 2 uses a Location Service function to
   determine where Bob is located. Based upon location analysis the call
   is forwarded to Bob */


   F4 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 68
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp



                 [Page 48]





   Content-Length: 150

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.128
   t=0 0
   m=audio 2000 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F5 100 Trying Proxy 2 -> SIP ALG

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F6 180 Ringing Bob -> Proxy 2

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F7 180 Ringing Proxy 2 -> SIP ALG

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128



                 [Page 49]





   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F8 180 Ringing SIP ALG -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F9 200 OK Bob -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0



                 [Page 50]





   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F10 200 OK Proxy 2 -> SIP ALG

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F11 200 OK SIP ALG -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.128
   t=0 0



                 [Page 51]





   m=audio 1734 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* The ALG prepares to proxy packets from 192.0.2.128/
      1734 to 192.0.2.201/3456 */


   F12 ACK Alice -> SIP ALG

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bhh
   Max-Forwards: 70
   Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F13 ACK SIP ALG -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bhh
    ;received=192.0.2.101
   Max-Forwards: 69
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0

   /* RTP streams are established between Alice and the ALG and
   between the ALG and B*/

   /* Alice Hangs Up with Bob. */


   F14 BYE Alice -> SIP ALG

   BYE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74be5
   Max-Forwards: 70
   Route: <sip:alg1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com



                 [Page 52]





   CSeq: 2 BYE
   Content-Length: 0


   F15 BYE SIP ALG -> Bob

   BYE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74be5
    ;received=192.0.2.101
   Max-Forwards: 69
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 BYE
   Content-Length: 0


   F16 200 OK Bob -> SIP ALG

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP alg1.atlanta.example.com:5060;branch=z9hG4bK739578.1
    ;received=192.0.2.128
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74be5
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 BYE
   Content-Length: 0


   F17 200 OK SIP ALG -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74be5
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 BYE
   Content-Length: 0









                 [Page 53]





3.6.  Session via Redirect and Proxy Servers with SDP in ACK

   Alice        Redirect Server     Proxy 3             Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|                |                |
     |     302 F2     |                |                |
     |<---------------|                |                |
     |     ACK F3     |                |                |
     |--------------->|                |                |
     |     INVITE F4                   |                |
     |-------------------------------->|    INVITE F5   |
     |             100  F6             |--------------->|
     |<--------------------------------|      180 F7    |
     |             180 F8              |<---------------|
     |<--------------------------------|                |
     |                                 |     200 F9     |
     |             200 F10             |<---------------|
     |<--------------------------------|                |
     |             ACK F11             |                |
     |-------------------------------->|     ACK F12    |
     |                                 |--------------->|
     |                Both Way RTP Media                |
     |<================================================>|
     |                                 |     BYE F13    |
     |             BYE F14             |<---------------|
     |<--------------------------------|                |
     |             200 F15             |                |
     |-------------------------------->|     200 F16    |
     |                                 |--------------->|
     |                                 |                |

   In this scenario, Alice places a call to Bob using first a Redirect
   server then a Proxy Server.  The INVITE message is first sent to the
   Redirect Server.  The Server returns a 302 Moved Temporarily response
   (F2) containing a Contact header with Bob's current SIP address.
   Alice then generates a new INVITE and sends to Bob via the Proxy
   Server and the call proceeds normally.  In this example, no SDP is
   present in the INVITE, so the SDP is carried in the ACK message.

   The call is terminated when Bob sends a BYE message.










                 [Page 54]





   Message Details

   F1 INVITE Alice -> Redirect Server

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bKbf9f44
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Length: 0


   F2 302 Moved Temporarily Redirect Proxy -> Alice

   SIP/2.0 302 Moved Temporarily
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bKbf9f44
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=53fHlqlQ2
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@chicago.example.com;transport=tcp>
   Content-Length: 0


   F3 ACK Alice -> Redirect Server

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bKbf9f44
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=53fHlqlQ2
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F4 INVITE Alice -> Proxy 3

   INVITE sip:bob@chicago.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com



                 [Page 55]





   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Length: 0


   F5 INVITE Proxy 3 -> Bob

   INVITE sip:bob@client.chicago.example.com SIP/2.0
   Via: SIP/2.0/TCP ss3.chicago.example.com:5060;branch=z9hG4bK721e.1
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss3.chicago.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Length: 0


   F6 100 Trying Proxy 3 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Content-Length: 0


   F7 180 Ringing Bob -> Proxy 3

   SIP/2.0 180 Ringing
   Via: SIP/2.0/TCP ss3.chicago.example.com:5060;branch=z9hG4bK721e.1
    ;received=192.0.2.233
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss3.chicago.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.chicago.example.com;transport=tcp>
   Content-Length: 0




                 [Page 56]






   F8 180 Ringing Proxy 3 -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss3.chicago.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.chicago.example.com;transport=tcp>
   Content-Length: 0


   F9 200 OK Bob -> Proxy 3

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP ss3.chicago.example.com:5060;branch=z9hG4bK721e.1
    ;received=192.0.2.233
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss3.chicago.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.chicago.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 148

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.chicago.example.com
   s=-
   c=IN IP4 192.0.2.100
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F10 200 OK Proxy -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss3.chicago.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159



                 [Page 57]





   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 INVITE
   Contact: <sip:bob@client.chicago.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 148

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.chicago.example.com
   s=-
   c=IN IP4 192.0.2.100
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* ACK contains SDP of Alice since none present in INVITE */


   F11 ACK Alice -> Proxy 3

   ACK sip:bob@client.chicago.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bq9
   Max-Forwards: 70
   Route: <sip:ss3.chicago.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 ACK
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F12 ACK Proxy 3 -> Bob

   ACK sip:bob@client.chicago.example.com SIP/2.0
   Via: SIP/2.0/TCP ss3.chicago.example.com:5060;branch=z9hG4bK721e.1
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bq9
    ;received=192.0.2.101
   Max-Forwards: 69
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159



                 [Page 58]





   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 ACK
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /* RTP streams are established between Alice and Bob */

   /* Bob Hangs Up with Alice. */


   F13 BYE Bob -> Proxy 3

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/TCP client.chicago.example.com:5060;branch=z9hG4bKfgaw2
   Max-Forwards: 70
   Route: <sip:ss3.chicago.example.com;lr>
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F14 BYE Proxy 3 -> Alice

   BYE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/TCP ss3.chicago.example.com:5060;branch=z9hG4bK721e.1
    ;received=192.0.2.100
   Via: SIP/2.0/TCP client.chicago.example.com:5060;branch=z9hG4bKfgaw2
   Max-Forwards: 69
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F15 200 OK Alice -> Proxy 3

   SIP/2.0 200 OK



                 [Page 59]





   Via: SIP/2.0/TCP ss3.chicago.example.com:5060;branch=z9hG4bK721e.1
    ;received=192.0.2.233
   Via: SIP/2.0/TCP client.chicago.example.com:5060;branch=z9hG4bKfgaw2
    ;received=192.0.2.100
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0


   F16 200 OK Proxy 3 -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/TCP client.chicago.example.com:5060;branch=z9hG4bKfgaw2
    ;received=192.0.2.100
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 BYE
   Content-Length: 0






























                 [Page 60]





3.7.  Session with re-INVITE (IP Address Change)


     Alice                Proxy 2                Bob
        |   F1 INVITE        |                    |
        |------------------->|      F2 INVITE     |
        |   F3 100 Trying    |------------------->|
        |<-------------------|   F4 180 Ringing   |
        |   F5 180 Ringing   |<-------------------|
        |<-------------------|                    |
        |                    |    F6 200 OK       |
        |    F7 200 OK       |<-------------------|
        |<-------------------|                    |
        |                 F8  ACK                 |
        |---------------------------------------->|
        |      Both Way RTP Media Established     |
        |<=======================================>|
        |                                         |
        |           Bob changes IP address        |
        |                                         |
        |                 F9 INVITE               |
        |<----------------------------------------|
        |                F10 200 OK               |
        |---------------------------------------->|
        |                 F11  ACK                |
        |<----------------------------------------|
        |         New RTP Media Stream            |
        |<=======================================>|
        |                 F12 BYE                 |
        |---------------------------------------->|
        |               F13 200 OK                |
        |<----------------------------------------|
        |                                         |

   This example shows a session in which the media changes midway
   through the session.  When Bob's IP address changes during the
   session, Bob sends a re-INVITE containing a new Contact and SDP
   (version number incremented) information to A.  In this flow, the
   proxy does not Record-Route so is not in the SIP messaging path after
   the initial exchange.











                 [Page 61]





   Message Details

   F1 INVITE Alice -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F2 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000





                 [Page 62]





   F3 100 Trying Proxy 2 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F4 180 Ringing Bob -> Proxy 2

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F5 180 Ringing Proxy 2 -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F6 200 OK Bob -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl



                 [Page 63]





   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F7 200 OK Proxy 2 -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Type: application/sdp
   Content-Length: 147

   v=0
   o=bob 2890844527 2890844527 IN IP4 client.biloxi.example.com
   s=-
   c=IN IP4 192.0.2.201
   t=0 0
   m=audio 3456 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F8 ACK Alice -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74b7b
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0



                 [Page 64]






   /* RTP streams are established between Alice and Bob */

   /* Bob changes IP address and re-INVITEs Alice with new Contact and
   SDP */


   F9 INVITE Bob -> Alice

   INVITE sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/UDP client.chicago.example.com:5060;branch=z9hG4bKlkld5l
   Max-Forwards: 70
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 14 INVITE
   Contact: <sip:bob@client.chicago.example.com>
   Content-Type: application/sdp
   Content-Length: 149

   v=0
   o=bob 2890844527 2890844528 IN IP4 client.chicago.example.com
   s=-
   c=IN IP4 192.0.2.100
   t=0 0
   m=audio 47172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F10 200 OK Alice -> Bob

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.chicago.example.com:5060;branch=z9hG4bKlkld5l
    ;received=192.0.2.100
   Max-Forwards: 70
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 14 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 150

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0



                 [Page 65]





   m=audio 1000 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F11 ACK Bob -> Alice

   ACK sip:alice@client.atlanta.example.com SIP/2.0
   Via: SIP/2.0/UDP client.chicago.example.com:5060;branch=z9hG4bKlkldcc
   Max-Forwards: 70
   From: Bob <sip:bob@biloxi.example.com>;tag=314159
   To: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 14 ACK
   Content-Length: 0

   /* New RTP stream established between Alice and Bob */

   /* Alice hangs up with Bob */


   F12 BYE Alice -> Bob

   BYE sip:bob@client.chicago.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bo4
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 BYE
   Content-Length: 0


   F13 200 OK Bob -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bo4
    ;received=192.0.2.101
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 2 BYE
   Content-Length: 0








                 [Page 66]





3.8.  Unsuccessful No Answer

   Alice           Proxy 1          Proxy 2            Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|   INVITE F2    |                |
     |     100  F3    |--------------->|   INVITE F4    |
     |<---------------|     100  F5    |--------------->|
     |                |<---------------|                |
     |                |                |      180 F6    |
     |                |     180 F7     |<---------------|
     |     180 F8     |<---------------|                |
     |<---------------|                |                |
     |   CANCEL F9    |                |                |
     |--------------->|                |                |
     |     200 F10    |                |                |
     |<---------------|   CANCEL F11   |                |
     |                |--------------->|                |
     |                |     200 F12    |                |
     |                |<---------------|                |
     |                |                |   CANCEL F13   |
     |                |                |--------------->|
     |                |                |     200 F14    |
     |                |                |<---------------|
     |                |                |     487 F15    |
     |                |                |<---------------|
     |                |                |     ACK F16    |
     |                |     487 F17    |--------------->|
     |                |<---------------|                |
     |                |     ACK F18    |                |
     |     487 F19    |--------------->|                |
     |<---------------|                |                |
     |     ACK F20    |                |                |
     |--------------->|                |                |
     |                |                |                |

   In this scenario, Alice gives up on the call before Bob answers
   (sends a 200 OK response).  Alice sends a CANCEL (F9) since no final
   response had been received from Bob.  If a 200 OK to the INVITE had
   crossed with the CANCEL, Alice would have sent an ACK then a BYE to
   Bob in order to properly terminate the call.

   Note that the CANCEL message is acknowledged with a 200 OK on a hop
   by hop basis, rather than end to end.







                 [Page 67]





   Message Details

   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="ze7k1ee88df84f1cec431ae6cbe5a359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="b00b416324679d7e243f55708d44be7b"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /*Client for Alice prepares to receive data on port 49172 from the
   network.*/


   F2 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151



                 [Page 68]






   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F3 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F4 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   Max-Forwards: 68
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000



                 [Page 69]







   F5 100 Trying Proxy 2 -> Proxy 1

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F6 180 Ringing Bob -> Proxy 2

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F7 180 Ringing Proxy 2 -> Proxy 1

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE



                 [Page 70]





   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F8 180 Ringing Proxy 1 -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F9 CANCEL Alice -> Proxy 1

   CANCEL sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Route: <sip:ss1.atlanta.example.com;lr>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 CANCEL
   Content-Length: 0


   F10 200 OK Proxy 1 -> Alice

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 CANCEL
   Content-Length: 0









                 [Page 71]





   F11 CANCEL Proxy 1 -> Proxy 2

   CANCEL sip:alice@atlanta.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 CANCEL
   Content-Length: 0


   F12 200 OK Proxy 2 -> Proxy 1

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 CANCEL
   Content-Length: 0


   F13 CANCEL Proxy 2 -> Bob

   CANCEL sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 CANCEL
   Content-Length: 0


   F14 200 OK Bob -> Proxy 2

   SIP/2.0 200 OK
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 CANCEL
   Content-Length: 0





                 [Page 72]





   F15 487 Request Terminated Bob -> Proxy 2

   SIP/2.0 487 Request Terminated
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F16 ACK Proxy 2 -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F17 487 Request Terminated Proxy 2 -> Proxy 1

   SIP/2.0 487 Request Terminated
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0











                 [Page 73]





   F18 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F19 487 Request Terminated Proxy 1 -> Alice

   SIP/2.0 487 Request Terminated
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE


   F20 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="ze7k1ee88df84f1cec431ae6cbe5a359", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="b00b416324679d7e243f55708d44be7b"
   CSeq: 1 ACK
   Content-Length: 0













                 [Page 74]





3.9.  Unsuccessful Busy

   Alice           Proxy 1          Proxy 2            Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|   INVITE F2    |                |
     |     100  F3    |--------------->|   INVITE F4    |
     |<---------------|     100  F5    |--------------->|
     |                |<---------------|                |
     |                |                |      486 F6    |
     |                |                |<---------------|
     |                |                |     ACK F7     |
     |                |      486 F8    |--------------->|
     |                |<---------------|                |
     |                |      ACK F9    |                |
     |     486 F10    |--------------->|                |
     |<---------------|                |                |
     |     ACK F11    |                |                |
     |--------------->|                |                |
     |                |                |                |


   In this scenario, Bob is busy and sends a 486 Busy Here response to
   Alice's INVITE.  Note that the non-2xx response is acknowledged on a
   hop-by-hop basis instead of end-to-end.  Also note that many SIP UAs
   will not return a 486 response, as they have multiple line and other
   features.

   Message Details

   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="dc3a5ab2530aa93112cf5904ba7d88fa", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="702138b27d869ac8741e10ec643d55be"
   Content-Type: application/sdp
   Content-Length: 151



                 [Page 75]






   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /*Client for Alice prepares to receive data on port 49172 from the
   network.*/


   F2 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F3 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0



                 [Page 76]





   F4 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 68
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com;transport=tcp>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F5 100 Trying Proxy 2 -> Proxy 1

   SIP/2.0 100 Trying
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F6 486 Busy Here Bob -> Proxy 2

   SIP/2.0  486 Busy Here
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1



                 [Page 77]





    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F7 ACK Proxy 2 -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F8 486 Busy Here Proxy 2 -> Proxy 1

   SIP/2.0  486 Busy Here
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F9 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0





                 [Page 78]





   F10 486 Busy Here Proxy 1 -> Alice

   SIP/2.0  486 Busy Here
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F11 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/TCP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="dc3a5ab2530aa93112cf5904ba7d88fa", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="702138b27d869ac8741e10ec643d55be"
   Content-Length: 0
























                 [Page 79]





3.10.  Unsuccessful No Response from User Agent

   Alice           Proxy 1          Proxy 2            Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|   INVITE F2    |                |
     |     100  F3    |--------------->|   INVITE F4    |
     |<---------------|     100  F5    |--------------->|
     |                |<---------------|   INVITE F6    |
     |                |                |--------------->|
     |                |                |   INVITE F7    |
     |                |                |--------------->|
     |                |                |   INVITE F8    |
     |                |                |--------------->|
     |                |                |   INVITE F9    |
     |                |                |--------------->|
     |                |                |   INVITE F10   |
     |                |                |--------------->|
     |                |                |   INVITE F11   |
     |                |     480 F12    |--------------->|
     |                |<---------------|                |
     |                |     ACK F13    |                |
     |     480 F14    |--------------->|                |
     |<---------------|                |                |
     |     ACK F15    |                |                |
     |--------------->|                |                |
     |                |                |                |

   In this example, there is no response from Bob to Alice's INVITE
   messages being re-transmitted by Proxy 2.  After the sixth
   re-transmission, Proxy 2 gives up and sends a 480 No Response to
   Alice.

   Message Details

   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",



                 [Page 80]





    nonce="cf5904ba7d8dc3a5ab2530aa931128fa", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="7afc04be7961f053c24f80e7dbaf888f"
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /*Client for Alice prepares to receive data on port 49172 from the
   network.*/


   F2 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F3 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101



                 [Page 81]





   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F4 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 68
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
   <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F5 100 Trying Proxy 2 -> Proxy 1

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0




                 [Page 82]






   F6 INVITE Proxy 2 -> Bob

   Resend of Message F4


   F7 INVITE Proxy 2 -> Bob

   Resend of Message F4


   F8 INVITE Proxy 2 -> Bob

   Resend of Message F4


   F9 INVITE Proxy 2 -> Bob

   Resend of Message F4


   F10 INVITE Proxy 2 -> Bob

   Resend of Message F4


   F11 INVITE Proxy 2 -> Bob

   Resend of Message F4

   /* Proxy 2 gives up */


   F12 480 No Response Proxy 2 -> Proxy 1

   SIP/2.0 480 No Response
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0






                 [Page 83]





   F13 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F14 480 No Response Proxy 1 -> Alice

   SIP/2.0 480 No Response
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F15 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="cf5904ba7d8dc3a5ab2530aa931128fa", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="7afc04be7961f053c24f80e7dbaf888f"
   Content-Length: 0












                 [Page 84]





3.11.  Unsuccessful Temporarily Unavailable

   Alice          Proxy 1          Proxy 2            Bob
     |                |                |                |
     |   INVITE F1    |                |                |
     |--------------->|   INVITE F2    |                |
     |     100  F3    |--------------->|   INVITE F4    |
     |<---------------|     100  F5    |--------------->|
     |                |<---------------|      180 F6    |
     |                |     180 F7     |<---------------|
     |     180 F8     |<---------------|                |
     |<---------------|                |     480 F9     |
     |                |                |<---------------|
     |                |                |     ACK F10    |
     |                |     480 F11    |--------------->|
     |                |<---------------|                |
     |                |     ACK F12    |                |
     |     480 F13    |--------------->|                |
     |<---------------|                |                |
     |     ACK F14    |                |                |
     |--------------->|                |                |
     |                |                |                |


   In this scenario, Bob initially sends a 180 Ringing response to
   Alice, indicating that alerting is taking place.  However, then a
   480 Unavailable is then sent to Alice.  This response is
   acknowledged then proxied back to Alice.

   Message Details

   F1 INVITE Alice -> Proxy 1

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="aa9311cf5904ba7d8dc3a5ab253028fa", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="59a46a91bf1646562a4d486c84b399db"
   Content-Type: application/sdp



                 [Page 85]





   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000

   /*Client for Alice prepares to receive data on port 49172 from the
   network.*/


   F2 INVITE Proxy 1 -> Proxy 2

   INVITE sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 69
   Record-Route: <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F3 100 Trying Proxy 1 -> Alice

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE



                 [Page 86]





   Content-Length: 0


   F4 INVITE Proxy 2 -> Bob

   INVITE sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Max-Forwards: 68
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:alice@client.atlanta.example.com>
   Content-Type: application/sdp
   Content-Length: 151

   v=0
   o=alice 2890844526 2890844526 IN IP4 client.atlanta.example.com
   s=-
   c=IN IP4 192.0.2.101
   t=0 0
   m=audio 49172 RTP/AVP 0
   a=rtpmap:0 PCMU/8000


   F5 100 Trying Proxy 2 -> Proxy 1

   SIP/2.0 100 Trying
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0








                 [Page 87]





   F6 180 Ringing Bob -> Proxy 2

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F7 180 Ringing Proxy 2 -> Proxy 1

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>
   Content-Length: 0


   F8 180 Ringing Proxy 1 -> Alice

   SIP/2.0 180 Ringing
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   Record-Route: <sip:ss2.biloxi.example.com;lr>,
    <sip:ss1.atlanta.example.com;lr>
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Contact: <sip:bob@client.biloxi.example.com>



                 [Page 88]





   Content-Length: 0


   F9 480 Temporarily Unavailable Bob -> Proxy 2

   SIP/2.0 480 Temporarily Unavailable
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
    ;received=192.0.2.222
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F10 ACK Proxy 2 -> Bob

   ACK sip:bob@client.biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss2.biloxi.example.com:5060;branch=z9hG4bK721e4.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F11 480 Temporarily Unavailable Proxy 2 -> Proxy 1

   SIP/2.0 480 Temporarily Unavailable
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
    ;received=192.0.2.111
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0








                 [Page 89]





   F12 ACK Proxy 1 -> Proxy 2

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP ss1.atlanta.example.com:5060;branch=z9hG4bK2d4790.1
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 ACK
   Content-Length: 0


   F13 480 Temporarily Unavailable Proxy 1 -> Alice

   SIP/2.0 480 Temporarily Unavailable
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
    ;received=192.0.2.101
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   CSeq: 1 INVITE
   Content-Length: 0


   F14 ACK Alice -> Proxy 1

   ACK sip:bob@biloxi.example.com SIP/2.0
   Via: SIP/2.0/UDP client.atlanta.example.com:5060;branch=z9hG4bK74bf9
   Max-Forwards: 70
   From: Alice <sip:alice@atlanta.example.com>;tag=9fxced76sl
   To: Bob <sip:bob@biloxi.example.com>;tag=314159
   Call-ID: 2xTb9vxSit55XU7p8@atlanta.example.com
   Proxy-Authorization: Digest username="alice",
    realm="atlanta.example.com",
    nonce="aa9311cf5904ba7d8dc3a5ab253028fa", opaque="",
    uri="sip:bob@biloxi.example.com",
    response="59a46a91bf1646562a4d486c84b399db"
   CSeq: 1 ACK
   Content-Length: 0












                 [Page 90]





4.  Security Considerations

   Since this document contains examples of SIP session establishment,
   the security considerations in RFC 3261 [1] apply.  RFC 3261
   describes the basic threats including registration hijacking, server
   impersonation, message body tampering, session modifying or teardown,
   and denial of service and amplification attacks.  The use of HTTP
   Digest as shown in this document provides one-way authentication and
   protection against replay attacks.  TLS transport is used in
   registration scenarios due to the lack of integrity protection in
   HTTP Digest and the danger of registration hijacking without it, as
   described in RFC 3261 [1].  A full discussion of the weaknesses of
   HTTP Digest is provided in RFC 3261 [1].  The use of TLS and the
   Secure SIP (sips) URI scheme provides a better level of security
   including two-way authentication.  S/MIME can provide end-to-end
   confidentiality and integrity protection of message bodies, as
   described in RFC 3261.

5.  References

5.1.  Normative References

   [1] Rosenberg, J., Schulzrinne, H., Camarillo, G., Johnston, A.,
       Peterson, J., Sparks, R., Handley, M. and E. Schooler, "SIP:
       Session Initiation Protocol", RFC 3261, June 2002.

   [2] Rosenberg, J. and H. Schulzrinne, "An Offer/Answer Model with
       SDP", RFC 3264, April 2002.

   [3] Franks, J., Hallam-Baker, P., Hostetler, J., Lawrence, S., Leach,
       P., Luotonen, A. and L. Stewart, "HTTP authentication: Basic and
       Digest Access Authentication", RFC 2617, June 1999.

   [4] Bradner, S., "Key words for use in RFCs to Indicate Requirement
       Levels", BCP 14, RFC 2119, March 1997.

5.2.  Informative References

   [5] Johnston, A., Donovan, S., Sparks, R., Cunningham, C. and K.
       Summers, "Session Initiation Protocol (SIP) Public Switched
       Telephone Network (PSTN) Call Flows", BCP 76, RFC 3666, December
       2003.

6.  Intellectual Property Statement

   The IETF takes no position regarding the validity or scope of any
   intellectual property or other rights that might be claimed to
   pertain to the implementation or use of the technology described in



                 [Page 91]





   this document or the extent to which any license under such rights
   might or might not be available; neither does it represent that it
   has made any effort to identify any such rights.  Information on the
   IETF's procedures with respect to rights in standards-track and
   standards-related documentation can be found in BCP-11.  Copies of
   claims of rights made available for publication and any assurances of
   licenses to be made available, or the result of an attempt made to
   obtain a general license or permission for the use of such
   proprietary rights by implementors or users of this specification can
   be obtained from the IETF Secretariat.

   The IETF invites any interested party to bring to its attention any
   copyrights, patents or patent applications, or other proprietary
   rights which may cover technology that may be required to practice
   this standard.  Please address the information to the IETF Executive
   Director.

7.  Acknowledgments

   This document is has been a group effort by the SIP and SIPPING WGs.
   The authors wish to thank everyone who has read, reviewed, commented,
   or made suggestions to improve this document.

   Thanks to Rohan Mahy, Adam Roach, Gonzalo Camarillo, Cullen Jennings,
   and Tom Taylor for their detailed comments during the final review.
   Thanks to Dean Willis for his early contributions to the development
   of this document.

   The authors wish to thank Kundan Singh for performing parser
   validation of messages.

   The authors wish to thank the following individuals for their
   participation in the review of this call flows document: Aseem
   Agarwal, Rafi Assadi, Ben Campbell, Sunitha Kumar, Jon Peterson, Marc
   Petit-Huguenin, Vidhi Rastogi, and Bodgey Yin Shaohua.

   The authors also wish to thank the following individuals for their
   assistance: Jean-Francois Mule, Hemant Agrawal, Henry Sinnreich,
   David Devanatham, Joe Pizzimenti, Matt Cannon, John Hearty, the whole
   MCI WorldCom IPOP Design team, Scott Orton, Greg Osterhout, Pat
   Sollee, Doug Weisenberg, Danny Mistry, Steve McKinnon, and Denise
   Ingram, Denise Caballero, Tom Redman, Ilya Slain, Pat Sollee, John
   Truetken, and others from MCI WorldCom, 3Com, Cisco, Lucent and
   Nortel.







                 [Page 92]





## 8.  Authors' Addresses

   All listed authors actively contributed large amounts of text to this
   document.

   Alan Johnston
   MCI
   100 South 4th Street
   St. Louis, MO 63102
   USA

   EMail: alan.johnston@mci.com

   Steve Donovan
   dynamicsoft, Inc.
   5100 Tennyson Parkway
   Suite 1200
   Plano, Texas 75024
   USA

   EMail: sdonovan@dynamicsoft.com

   Robert Sparks
   dynamicsoft, Inc.
   5100 Tennyson Parkway
   Suite 1200
   Plano, Texas 75024
   USA

   EMail: rsparks@dynamicsoft.com

   Chris Cunningham
   dynamicsoft, Inc.
   5100 Tennyson Parkway
   Suite 1200
   Plano, Texas 75024
   USA

   EMail: ccunningham@dynamicsoft.com

   Kevin Summers
   Sonus
   1701 North Collins Blvd, Suite 3000
   Richardson, TX 75080
   USA

   EMail: kevin.summers@sonusnet.com




                 [Page 93]





## 9.  Full Copyright Statement

   Copyright (C) The Internet Society (2003).  All Rights Reserved.

   This document and translations of it may be copied and furnished to
   others, and derivative works that comment on or otherwise explain it
   or assist in its implementation may be prepared, copied, published
   and distributed, in whole or in part, without restriction of any
   kind, provided that the above copyright notice and this paragraph are
   included on all such copies and derivative works.  However, this
   document itself may not be modified in any way, such as by removing
   the copyright notice or references to the Internet Society or other
   Internet organizations, except as needed for the purpose of
   developing Internet standards in which case the procedures for
   copyrights defined in the Internet Standards process must be
   followed, or as required to translate it into languages other than
   English.

   The limited permissions granted above are perpetual and will not be
   revoked by the Internet Society or its successors or assignees.

   This document and the information contained herein is provided on an
   "AS IS" basis and THE INTERNET SOCIETY AND THE INTERNET ENGINEERING
   TASK FORCE DISCLAIMS ALL WARRANTIES, EXPRESS OR IMPLIED, INCLUDING
   BUT NOT LIMITED TO ANY WARRANTY THAT THE USE OF THE INFORMATION
   HEREIN WILL NOT INFRINGE ANY RIGHTS OR ANY IMPLIED WARRANTIES OF
   MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE.

Acknowledgement

   Funding for the RFC Editor function is currently provided by the
   Internet Society.



















                 [Page 94]