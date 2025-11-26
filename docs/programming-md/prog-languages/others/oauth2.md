# OAuth 2.0 notes

OAuth is a security protocol used to protect web APIs.

The server give a token to the client. The token explicitly represents a **delegated right of access** without the client application needing to impersonate the person who controls the resource.

OAuth tokens can limit the client’s access to only the actions that the resource owner has delegated.

Nếu mình đưa ai đó full password thì giống như mình đưa họ owner key của một chiếc xe hơi, họ có full permission. Còn đưa token là đưa valet key, mình có thể giới hạn quyền (delegate).

The **authorization server (AS)** is trusted by the protected resource to issue special-purpose security credentials—called OAuth access tokens—to clients. To acquire a token, the client first sends the resource owner to the authorization server in order to request that the resource owner authorize this client. The resource owner authenti-cates to the authorization server and is generally presented with a choice of whether to authorize the client making the request. The client is able to ask for a subset of func-tionality, or scopes, which the resource owner may be able to further diminish. Once the authorization grant has been made, the client can then request an access token from the authorization server. This access token can be used at the protected resource to access the API, as granted by the resource owner.

OAuth isn’t an authentication protocol, even though it can be used to build one.

**Refresh token** is used to get new access tokens without asking for authorization again.

- Các loại token trong OAuth
  * Bearer token = access token
  * Refresh token

- Sau khi đã lấy được access token, the client has several methods for presenting the access token to the protected resources:
  * using the `Authorization` header

- OIDC is an identity layer built on top of the OAuth 2.0 authorization framework.
  * OAuth 2.0 handles Authorization (What you are allowed to do).
  * OIDC handles Authentication (Who you are).

## OAuth’s actors: clients, authorization servers, resource owners, and protected resources

An OAuth client is a piece of software that attempts to access the protected resource on behalf of the resource owner, and it uses OAuth to obtain that access.

- The Authorization Server have two enpoints;
  * `authorizationEndpoint`
  * `tokenEndpoint`

## The OAuth Client

### Grant Types

- A **Grant Type (or Authorization Grant or Flow)** defines the specific method or workflow a client application uses to obtain an Access Token from an authorization server. If successful it ultimately results in the client getting a token.
  * `Authorization Code Grant Type` là một trong những authorization grant type. The entire OAuth process is the authorization grant: the client sending the user to the authorization endpoint, then receiving the code, then finally trading the code for the token.
  * Other OAuth grant types: implicit grant type or client credentials grant type

The **authorization code grant type** fully separates all of the different OAuth parties and is consequently the **most foundational** and complex of the core grant types that we’ll cover in this book. All of the other OAuth grant types are optimizations of this one, suited for specific use cases and environments.

- The Authorization Client can send the resource owner (which in our case is the end user at the client) over to the authorization server’s authorization endpoint.
- The server then sends an authorization code back to the client through its `redirect_uri`.
- The client finally sends the code that it received to the authorization server’s token endpoint to receive an OAuth access token, which it needs to parse and store.

### Use the token with a protected resource

- The client make a call to the protected resource and include the access token (bearer token) in one of the three valid locations.
  1. Send it in the `Authorization: {Bearer}` HTTP header. This is the method recommended by the specification wherever possible.
  2. As a form-encoded request body parameter
  3. As a URL-encoded query parameter

The Authorization header is recommended whenever possible because of limitations in the other two forms. When using the query parameter, the value of the access token can possibly inadvertently leak into server-side logs, because it’s part of the URL request. Using the form-encoded parameter limits the input type of the pro-tected resource to using form-encoded parameters and the POST method. If the API is already set up to do that, this can be fine as it doesn’t experience the same security limitations that the query parameter does. 

The Authorization header provides the maximum flexibility and security of all three methods, but it has the downside of being more difficult for some clients to use. A robust client or server library will provide all three methods where appropriate, and in fact our demonstration protected resource will accept an access token in any of the three locations.

### Refresh the access token

k

## Interactions between OAuth’s actors and components: back channel, front channel, and endpoints

OAuth is an HTTP-based protocol, but unlike most HTTP-based protocols, OAuth communication doesn’t always happen through a simple HTTP request and response.

Many parts of the OAuth process use a normal HTTP request and response format to communicate to each other. Since these requests generally occur outside the purview of the resource owner and user agent, they are collectively referred to as **back-channel communication**.

The `client_id` needs to be unique for each client at a given authorization server, and is there-fore almost always assigned by the authorization server to the client.

## The code framework

- The OAuth Client application (client.js) runs on `http://localhost:9000/`
- The OAuth Authorization Server application (`authorizationServer.js`) runs on 
`http://localhost:9001/`
- The OAuth Protected Resource Application (`protectedResource.js`) runs on 
`http://localhost:9002/`

## Spring Security

Spring Security is used to handle authentication and authorization.