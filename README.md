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

* Holds resources (on behalf of the Resource Owner)
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
  * This field will be echoed back in the response from the OAuth server, for use by the Client (the app) when reconciling requests with responses
* scope
* response_type
  * Specifies if this request is for an Authorization Code or an Access Token
  * These two request types have different flows
* client_id
* redirect_uri
  * This is the URI of the Client (the app) service that will receive the responses from the OAuth server 

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

----

# Overview of OAuth Flows

* Authorization Code Grant
  * Most secure OAuth flow
  * Sometimes referred to as '3-legged OAuth'
  * Best choice if the Client (the app) has secure storage for ClientID and ClientSecret
* Implicit Grant
  * Must be used when the Client (the app) **does not** have secure storage for ClientID, ClientSecret and OAuth Tokens
    * example: Client-side JavaScript Client app
  * OAuth server will issue OAuth tokens with a shorter validity period
  * Token refresh is difficult (or not possible)
* Client Credentials Grant
  * Sometimes referred to as '2-legged OAuth'
  * Used when the Client (the app) is also the Resource Owner
* Resource Owner Password Credentials Grant
  * Used when the Resource Owner has trusted the Client (the app) with their username/password

## Authorization Code Grant flow

Sometimes referred to as '3-legged OAuth', because it checks the identity of the three involved actors:
  * OAuth servers
  * Resource Owner
  * Client (the app)
  
How?
  * The OAuth server authenticates the Resource Owner by username and password. 
    * These are provided interactively by the Resource Owner.
  * The OAuth server authenticates the Client by its ClientID and ClientSecret.
    * These are transmitted in the HTTP header.
    * HTTP BASIC authorization mechanism is used
  * OAuth server identity is verified by a Certificate Authority (CA)

High-level steps:
  * Get the Authorization Code
  * Get the Token
  * Use the Token to Access a Resource

----

# Using OAuth to access Facebook

## Registering my Client (my app) to use the Facebook OAuth server

  1.  First step, sign up for Facebook if you don't already have an account.
  2.  Next step, go here: https://developers.facebook.com/apps/
  3.  Click the "Create App" button/link/thing.
  4.  Enter whatever you like for "Display Name".
  5.  Click "Create App ID" button.
  6.  After the app creation process is complete, you should be presented with an app page that has a lot of options.
  7.  Expand the 'Settings' option on the left-side navigation bar, and click 'Basic'.
  8.  Retreive your App ID and App Secret from this page. **DO NOT SHARE THESE!** They act as your app's credentials on the Facebook OAuth server!
  9.  Click the "PRODUCTS +" button on the left-side navigation bar.
  10. Find "Facebook Login" in the list of available products and click the "Set Up" button.
  11. Choose "Web".
  12. Enter your Site URL, such as "https://jimtough.org", then click "Save".
  13. "Facebook Login" should now appear under the "PRODUCTS +" in the left-side navigation bar. Under it, click "Settings".
  14. Enter the OAuth Redirect URI that you want to use for your application, such as "https://api.jimtough.org/". Facebook wants a trailing slash in the URI. Click "Save Changes".
  15. Go to https://www.urlencoder.org/ and generate the encoded form of my redirect URL. Example: `https%3A%2F%2Fapi.jimtough.org%2F`
  











