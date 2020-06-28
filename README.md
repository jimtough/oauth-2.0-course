# oauth-2.0-course
I took a Udemy course on OAuth 2.0 and kept my notes and other files in this repo

----

# OAuth Actors

* OAuth Provider
  * Server that issues the OAuth tokens
  * Usually capable of presenting the end user (the Resource Owner) with a web-based UI to input their credentials and to select which resources the OAuth token will grant access when used by the client (the mobile/cloud app)
  * The 'authentication' typically happens first, followed by the 'authorization' or 'consent' step where the resource owner selects the resources this token may access
* Resource Provider
  * Usually a set of web APIs
  * Client (mobile/cloud app) will use the APIs of the Resource Provider, plus the Resource Owner's OAuth token, to access the resource owner's protected resources
* Resource Owner
  * end user
* Client
  * Usually a mobile app or cloud-based app
  
## OAuth Server Endpoints

* /authorize (GET)
* /token (POST)

## Resource Server

* Holds resources
* Protects resources
* Makes resources available via an API

----

# Endpoints

* Authorization Endpoint (provided by OAuth server)
* Token Endpoint (provided by OAuth server)
* Redirect Endpoint (provided by Client app)
* Resource Endpoint (provided by Resource Server)

## Authorization Endpoint

### /authorize (GET)

Input Query Parameters:
* state
* scope
* response_type
  * Specifies if this request is for an Authorization Code or an Access Token
  * These two request types have different flows
* client_id
* redirect_url

Output:
* Authorization Code (for Authorization Code Grant)
* Access Token (for Implicit Grant)

The Output value will be returned via query parameter in the redirect URI 

### /token (POST)

Authorization: Basic {clientId}:{clientSecret}

Input Query Parameters:
* grant_type
* code
* client_id
* redirect_uri

Output:
* Access Token and Refresh Token for all flows
  * Authorization Code grant
  * Client Credentials grant
  * Resource Owner Password Credentials grant

## Client Endpoint

### Redirect URI (GET)

Input Query Parameters:
* state
* scopes
* code

## Resource Server - resource endpoint

### /api

Authorization: Bearer {AccessToken}

* Access Token and Refresh Token for all flows
  * Authorization Code grant
  * Client Credentials grant
  * Resource Owner Password Credentials grant

----

# Tokens and Credentials

* Tokens allow resource access to the **bearer** of the token!
  * Tokens must be protected, just as resource owner passwords would be protected if one were not using OAuth.
  * Transport layer security should be used in any interaction where tokens are exchanged.

## Token types

* Access Token (AT)
  * Used by Client (the app) to access resources
  * Access Tokens using have a limited validity period (24 hours, a month, etc)
  * These tokens can be reused in multiple requests, as long as the token is still valid.
* Refresh Token (RT)
  * Refresh Tokens using have a longer validity period than Access Tokens
  * Used to request a new Access Token after the Access Token has expired
  * If the Refresh Token is still valid, then the Resource Owner credentials (password or whatever) do not need to be checked again.
  * Refresh Tokens are stored and sent by the client to the **token endpoint**
* Authorization Code (Code)
  * Sent to the Client (the app) after the Resource Owner (the end user) has been authenticated and has consented to allow access to resources.
  * Authorization Code typically has a **very short** validity period (a few minutes)
  * The Client uses the Authorization Code to request an Access Token from the /token endpoint on the OAuth server.
  * Authorization Codes **cannot be used anywhere else** (such as submitting them to the Resource Server to access a resource) 

## Credentials

* Resource Owner Credentials (such as end-users's username/password)
  * These should never by provided directly to the Client (the app)!
* Client Credentials (ClientID and ClientSecret)
  * Used whenever a Client (the app) accesses a token endpoint (the OAuth server)
  * ClientID is a query parameter when calling the authorization endpoint or the token endpoint
* Access Token / Refresh Token / Authorization Code
  * (see notes in previous section)
  * Access Tokens are bearer tokens, and must be protected just like any other credentials (such as passwords)

----

# Client Registration

* Each Client (mobile/cloud app) must register with the OAuth server.
  * Must provide a Redirect URI
  * Must provide required Scopes
* Upon successful registration, the Client will receive:
   * ClientID
   * ClientSecret











