# OAuth 2.0 Notes

## Basics & Jargon

OAuth is a security protocol used to protect web APIs. 

The OAuth 2.0 `authorization framework` enables a third-party application to obtain limited access to an HTTP service, either on behalf of a resource owner by orchestrating an approval interaction between the resource owner and the HTTP service, or by allowing the third-party application to obtain access on its own behalf.

as an _authorization framework_, OAuth is all about getting the right of access from one component of a system to another. In particular, in the OAuth world, a client application wants to gain access to a protected resource on behalf of a resource owner (usually an end user).

- The _resource owner_ has access to an API and can delegate access to that API. The resource owner is usually a person and is generally assumed to have access to a web browser
- The _protected resource_ is the component that the resource owner has access to. This can take many different forms, but for the most part it’s a web API of some kind. Even though the name “resource” makes it sound as though this is something to be downloaded, these APIs can allow read, write, and other operations just as well.
- The _client_ is the piece of software that accesses the protected resource on behalf of the resource owner. If you’re a web developer, the name “client” might make you think this is the web browser, but that’s not how the term is used here. If you’re a business application developer, you might think of the “client” as the person who’s paying for your services, but that’s not what we’re talking about, either. In OAuth, the client is whatever software consumes the API that makes up the protected resource.

OAuth 2.0 is a delegation protocol, a means of letting someone who controls a resource allow a software application to access that resource on their behalf without impersonating them.  
The application requests authorization from the owner of the resource and receives _tokens_ that it can use to access the resource.  
This all happens without the application needing to impersonate the person who controls the resource, since the token explicitly represents a delegated right of access.

you can think of the OAuth token as a “valet key” for the web. the valet key provides additional security beyond simply handing over the regular key. The valet key of a car allows the owner of the car to give limited access to someone, the valet, without handing over full control in the form of the owner’s key. OAuth tokens can limit the client’s access to only the actions that the resource owner has delegated.

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

---

One key assumption in the design of OAuth 2.0 was that there would always be several orders of magnitude more clients in the wild than there would be authorization servers or protected resource servers.

**A single** `authorization server` can easily protect **multiple** `resource servers`, and there are likely to be many different kinds of clients wanting to consume any given API.

As a consequence of this architectural decision, wherever possible, complexity is shifted away from clients and onto servers. This is good for client developers, as the client becomes the simplest piece of software in the system. Client developers no longer have to deal with signature normalizations or parsing complicated security policy documents, as they would have in previous security protocols, and they no longer have to worry about handling sensitive user credentials. OAuth tokens provide a mechanism that’s only slightly more complex than passwords but significantly more secure when used properly.

The flip side is that authorization servers and protected resources are now responsible for more of the complexity and security. A client needs to manage securing only its own client credentials and the user’s tokens, and the breach of a single client would be bad but limited in its damage to the users of that client. Breaching the client also doesn’t expose the resource owner’s credentials, since the client never sees them in the first place.

An authorization server, on the other hand, needs to manage and secure the credentials and tokens for all clients and all users on a system. Although this does make it more of a target for attack, it’s significantly easier to make a single authorization server highly secure than it is to make a thousand clients written by independent developers just as secure.

---

These two actions—how to get a token and how to use a token—are the fundamental parts of OAuth.

OAuth isn’t defined outside of the HTTP protocol. Since OAuth 2.0 with bearer tokens provides no message signatures, it is not meant to be used outside of HTTPS (HTTP over TLS). Sensitive secrets and information are passed over the wire, and OAuth requires a transport layer mechanism such as TLS to protect these secrets.

**OAuth isn’t an authentication protocol**, even though it can be used to build one. As we’ll cover in greater depth in chapter 13, an OAuth transaction on its own tells you nothing about who the user is, or even if they’re there. Think of our photo-printing example: the photo printer doesn’t need to know who the user is, only that somebody said it was OK to download some photos. OAuth is, in essence, an ingredient that can be used in a larger recipe to provide other capabilities. Additionally, OAuth uses authentication in several places, particularly authentication of the resource owner and client software to the authorization server. This embedded authentication does not itself make OAuth an authentication protocol.

OAuth doesn’t define a mechanism for user-to-user delegation, even though it is fundamentally about delegation of a user to a piece of software. OAuth assumes that the resource owner is the one that’s controlling the client. In order for the resource owner to authorize a different user, more than OAuth is needed.

OAuth doesn’t define a token format. In fact, the OAuth protocol explicitly states that the content of the token is completely opaque to the client application. The client application does not need to be able to parse and process the token. However, the token still needs to be understood by the authorization server that issues it and the protected resource that accepts it.

---

- Each component is set up to run on a different port on localhost, in a separate process:
  * The OAuth Client application (`client.js`) runs on `http://localhost:9000/`
  * The OAuth Authorization Server application (`authorizationServer.js`) runs on `http://localhost:9001/`
  * The OAuth Protected Resource Application (`protectedResource.js`) runs on `http://localhost:9002/`

---

refresh token is used to get new access tokens without asking for authorization again

## The bad old days: credential sharing (and credential theft)

One approach is the client copy the user’s credentials and replay them on the protected service.

This approach requires that the user have the same credentials at the client application and the protected resource (such as a username and password or a domain session cookie). This could occur if a single company controls the client, authorization server, and protected resources, and all of these run inside the same policy and network control. If the printing service is offered by the same company that provided the storage service, this technique might work as the user would have the same account credentials on both services.

---

Another common approach is to use a universal `developer key` issued to the client, which uses this to call the protected resource directly.

the developer key acts as a kind of universal key that allows the client to impersonate any user that it chooses, probably through an API parameter. This has the benefit of not exposing the user’s credentials to the client, but at the cost of the client requiring a highly powerful credential. The client effectively has free rein over the data on the protected resource.

This only works in instances in which the client can be fully known to and trusted by the protected resource. It is unlikely that any such relationship would be built across two organizations

Additionally, the damage done to the protected resource if the client’s credentials are stolen is potentially catastrophic, since all users of the storage service are affected by the breach whether they ever used the printer or not.

---

Another possible approach is the protected resource gives users a special password that’s only for sharing with third-party services (the client). Users don’t use this password to log in themselves, but paste it into applications that they want to work for them.

However, the usability of such a system is, on its own, not very good. This requires the user to generate, distribute, and manage these special credentials in addition to the primary passwords they already must curate.

## Delegating access (ủy quyền)

OAuth is a protocol designed to do exactly that: in OAuth, the end user _delegates_ some part of their authority to access the protected resource to the client application to act on their behalf. To make that happen, OAuth introduces another component into the system: the `authorization server`.

- The authorization server (AS) is trusted by the protected resource to issue special-purpose security credentials—called `OAuth access tokens`—to clients.
- To acquire a token, the client first sends the resource owner to the authorization server in order to request that the resource owner authorize this client.
- The resource owner authenticates to the authorization server and is generally presented with a choice of whether to authorize the client making the request. The client is able to ask for a subset of functionality, or `scopes`, which the resource owner may be able to further diminish.
- Once the authorization grant has been made, the client can then request an access token from the authorization server. This access token can be used at the protected resource to access the API, as granted by the resource owner

At no time in this process are the resource owner’s credentials exposed to the client: the resource owner authenticates to the authorization server separately from anything used to communicate with the client. Neither does the client have a high-powered developer key: the client is unable to access anything on its own and instead must be authorized by a valid resource owner before it can access any protected resources. 

The user generally never has to see or deal with the access token directly. Instead of requiring the user to generate tokens and paste them into clients, the OAuth protocol facilitates this process and makes it relatively simple for the client to request a token and the user to authorize the client. Clients can then manage the tokens, and users can manage the client applications.

---

OAuth was designed from the outset as a protocol for use with APIs. Some of the key use cases of OAuth occur when the user is no longer present at the client, yet the client is still able to act on the user’s behalf. 

OAuth is a protocol designed for the world of web APIs, accessed by client software.

---

Although OAuth is often called an authorization protocol (and this is the name given to it in the RFC which defines it), it is a `delegation protocol` (giao thức ủy quyền). It provides a means by which a client can request that a user delegate some of their authority to it. The user can then approve this request, and the client can then act on it with the results of that approval.

- Authentication: xác thực
- Authorization: phân quyền

In our printing example, the photo-printing service can ask the user, “Do you have any of your photos stored on this storage site? If so, we can totally print that.” The user is then sent to the photo-storage service, which asks, “This printing service is asking to get some of your photos; do you want that to happen?” The user can then decide whether they want that to happen, deciding whether to delegate access to the printing service.

---

OAuth systems often follow the principle of `TOFU: Trust On First Use`. In a TOFU model, the first time a security decision needs to be made at runtime, and there is no existing context or configuration under which the decision can be made, the user is prompted. The system offers to remember this decision for later use. In other words, the first time an authorization context is met, the system can be directed to trust the user’s decision for later processing: Trust On First Use.

The TOFU method strikes a good balance between the flexibility of asking end users to make security decisions in context and the fatigue of asking them to make these decisions constantly. Without the “Trust” portion of TOFU, users would have no say in how these delegations are made. Without the “On First Use” portion of TOFU, users would quickly become numb to an unending barrage of access requests. This kind of security system fatigue breeds workarounds that are usually more insecure than the practices that the security system is attempting to address.

## Getting and using tokens

There are two major steps to an OAuth transaction: issuing a token and using a token. 

The token represents the access that’s been delegated to the client and it plays a central role in every part of OAuth 2.0.

- The canonical OAuth transaction consists of the following sequence of events:
  * The Resource Owner indicates to the Client that they would like the Client to act on their behalf (for example, “Go load my photos from that service so I can print them”).
  * The Client requests authorization from the Resource Owner at the Authorization Server.
  * The Resource Owner grants authorization to the Client.
  * The Client receives a Token from the Authorization Server.
  * The Client presents the Token to the `Protected Resource`.

## OAuth 2.0 Authorization Grant process

An `authorization grant` is the means by which an OAuth client is given access to a protected resource using the OAuth protocol, and if successful it ultimately results in the client getting a token.

The authorization code by itself is not an authorization grant. Instead, the entire OAuth process is the authorization grant: the client sending the user to the authorization endpoint, then receiving the code, then finally trading the code for the token.

In other words, the authorization grant is the method for getting a token

Several different kinds of authorization grants exist in OAuth, each with its own characteristics. But most of our examples and exercises, such as those in the previous section, use the authorization code authorization grant type.

---

The `authorization code grant` uses a temporary credential, the authorization code, to represent the resource owner’s delegation to the client

The authorization server has two endpoints: the `authorization endpoint` & the `token endpoint`.

Mỗi protected resource có một authorization server ứng với nó. Client phải có cách tìm ra.

- First, the resource owner goes to the client application and indicates to the client that they would like it to use a particular `protected resource` on their behalf.
- When the client realizes that it needs to get a new OAuth access token, it sends the resource owner to the authorization server with a request that indicates that the client is asking to be delegated some piece of authority by that resource owner.
  * The client identifies itself and requests particular items such as scopes by including query parameters in the URL it sends the user to. The authorization server can parse those parameters and act accordingly, even though the client isn’t making the request directly.
- Next, the authorization server will usually require the user to authenticate. This step is essential in determining who the resource owner is and what rights they’re allowed to delegate to the client
- Next, the user authorizes the client application. In this step, the resource owner chooses to delegate some portion of their authority to the client application, and the authorization server has many different options to make this work.
  * The client’s request can include an indication of what kind of access it’s looking for (known as the `OAuth scope`.
  * Furthermore, many authorization servers allow the storage of this authorization decision for future use. If this is used, then future requests for the same access by the same client won’t prompt the user interactively. The user will still be redirected to the authorization endpoint, and will still need to be logged in, but the decision to delegate authority to the client will have already been made during a previous attempt.
- Next, the authorization server redirects the user agent (resource owner) back to the client application with an `authorization code`.
  * Since we’re using the `authorization code grant type`, this redirect includes the special `code` query parameter. The value of this parameter is a one-time-use credential known as the authorization code, and it represents the result of the user’s authorization decision. The client can parse this parameter to get the authorization code value when the request comes in, and it will use that code in the next step
- client sends authorization code and its own credentials to the auth server's token endpoint
  * client authenticates using its own credentials
  * This HTTP request is made directly between the client and the authorization server, without involving the browser or resource owner at all.
- The authorization server performs a number of steps to ensure the request is legitimate. First, it validates the client’s credentials (passed in the Authorization header here) to determine which client is requesting access. Then, it reads the value of the code parameter from the body and looks up any information it has about that authorization code, including which client made the initial authorization request, which user authorized it, and what it was authorized for. If the authorization code is valid, has not been used previously, and the client making this request is the same as the client that made the original request, the authorization server generates and returns a new access token for the client.
- client sends access token to protected resource

The user’s authentication passes directly between the user (and their browser) and the authorization server; it’s never seen by the client application. This essential aspect protects the user from having to share their credentials with the client application, the antipattern that OAuth was invented to combat

The access token is a JSON object

```json
{
  "access_token": "987tghjkiu6trfghjuytrghj",
  "token_type": "Bearer"
}
```

The resource server and the authorization server to share a database that contains the token information. The authorization server writes new tokens into the store when they’re generated, and the resource server reads tokens from the store when they’re presented.

## OAuth’s Actors: Clients, Authorization servers, Resource Owners, and Protected Resources

An OAuth client is a piece of software that attempts to access the protected resource on behalf of the resource owner, and it uses OAuth to obtain that access.

- The Authorization Server have two enpoints;
  * `authorizationEndpoint`
  * `tokenEndpoint`

There are four main actors in an OAuth system: clients, resource owners, authorization servers, and protected resources

The client doesn’t have to understand the token, nor should it ever need to inspect the token’s contents. Instead, the client uses the token as an opaque string.

For at least part of the process, the resource owner interacts with the authorization server using a web browser (more generally known as the user agent).

## Access Tokens

OAuth tokens are opaque to the client, which means that the client has no need (and often no ability) to look at the token itself. The client’s job is to carry the token, requesting it from the authorization server and presenting it to the protected resource.

## Refresh Tokens

An OAuth refresh token is similar in concept to the access token, in that it’s issued to the client by the authorization server and the client doesn’t know or care what’s inside the token. What’s different, though, is that the token is never sent to the protected resource. Instead, the client uses the refresh token to request new access tokens without involving the resource owner

## Interactions between OAuth’s actors and components: back channel,- , front channel, and endpoints

Many parts of the OAuth process use a normal HTTP request and response format to communicate to each other. Since these requests generally occur outside the purview of the resource owner and user agent, they are collectively referred to as `back-channel communication`.

Back channel uses direct HTTP connections between component, the browser is not involved.

The authorization server provides a token endpoint that the client uses to request access tokens and refresh tokens. The client calls this endpoint directly, presenting a form-encoded set of parameters that the authorization server parses and processes. The authorization server then responds with a JSON object representing the token.

Additionally, when the client connects to the protected resource, it’s also making a direct HTTP call in the back channel.

---

In OAuth there are several instances in which two components cannot make direct requests of and responses to each other, such as when the client interacts with the authorization endpoint of the authorization server. Front-channel communication is a method of using HTTP requests to communicate indirectly between two systems through an intermediary web browser.

Front channel uses HTTP redirects through the web browser, no direct connections.

Front-channel communication works by attaching parameters to a URL and indicating that the browser should follow that URL. The receiving party can then parse the incoming URL, as fetched by the browser, and consume the presented information. The receiving party can then respond by redirecting the browser back to a URL hosted by the originator, using the same method of adding parameters. The two parties are thus communicating with each other indirectly through the use of the web browser as an intermediary. This means that each front-channel request and response is actually a pair of HTTP request and response transactions 

For example, in the authorization code grant that we saw previously, the client needs to send the user to the authorization endpoint, but it also needs to communicate certain parts of its request to the authorization server. To do this, the client sends an HTTP redirect to the browser. The target of this redirect is the server’s URL with certain fields attached to it as query parameters:

The authorization server can parse the incoming URL, just like any other HTTP request, and find the information sent from the client in these parameters. The authorization server can interact with the resource owner at this stage, authenticating them and asking for authorization over a series of HTTP transactions with the browser. When it’s time to return the authorization code to the client, the authorization server sends an HTTP redirect to the browser as well, but this time with the client’s redirect_uri as the base. The authorization server also includes its own query parameters in the redirect:

When the browser follows this redirect, it will be served by the client application, in this case through an HTTP request. The client can parse the URL parameters from the incoming request. In this way, the client and authorization server can pass messages back and forth to each other through an intermediary without ever talking to each other directly.

## The OAuth Client

## Register an OAuth client with an authorization server

the OAuth client and the authorization server need to know a few things about each other before they can talk.

An OAuth client is identified by a special string known as the client identifier. The client identifier needs to be unique for each client at a given authorization server, and is therefore almost always assigned by the authorization server to the client.

This assignment could happen through a developer portal, dynamic client registration, or through some other process. 

Our client is also what’s known as a `confidential client` in the OAuth world, which means that it has a shared secret that it stores in order to authenticate itself when talking with the authorization server, known as the `client_secret`. The client_secret can be passed to the authorization server’s token endpoint in several different ways, but in our example we will be using `HTTP Basic`.

The client_secret is also nearly always assigned by the authorization server

### Grant Types

- A **Grant Type (or Authorization Grant or Flow)** defines the specific method or workflow a client application uses to obtain an Access Token from an authorization server. If successful it ultimately results in the client getting a token.
  * `Authorization Code Grant Type` là một trong những authorization grant type. The entire OAuth process is the authorization grant: the client sending the user to the authorization endpoint, then receiving the code, then finally trading the code for the token.
  * Other OAuth grant types: implicit grant type or client credentials grant type

The `authorization code grant type` fully separates all of the different OAuth parties and is consequently the **most foundational** and complex of the core grant types that we’ll cover in this book. All of the other OAuth grant types are optimizations of this one, suited for specific use cases and environments.

- The Authorization Client can send the resource owner (which in our case is the end user at the client) over to the authorization server’s authorization endpoint.
- The server then sends an authorization code back to the client through its `redirect_uri`.
- The client finally sends the code that it received to the authorization server’s token endpoint to receive an OAuth access token, which it needs to parse and store.

### Use the token with a Protected Resource

- The client make a call to the protected resource and include the access token (bearer token) in one of the three valid locations.
  1. Send it in the `Authorization: {Bearer}` HTTP header. This is the method recommended by the specification wherever possible.
  2. As a form-encoded request POST body. This method isn’t recommended by the OAuth specification because it artificially limits the input of the API to a form-encoded set of values.
  3. As a URL-encoded query parameter. This method is recommended by OAuth only as a last resort when the other two methods aren’t appli-cable. 

Tương ứng thì Protected Resource có 3 cách khác nhau để parse token gởi lên từ phía OAuth client.

The Authorization header is recommended whenever possible because of limitations in the other two forms. When using the query parameter, the value of the access token can possibly inadvertently leak into server-side logs, because it’s part of the URL request. Using the form-encoded parameter limits the input type of the pro-tected resource to using form-encoded parameters and the POST method. If the API is already set up to do that, this can be fine as it doesn’t experience the same security limitations that the query parameter does. 

The Authorization header provides the maximum flexibility and security of all three methods, but it has the downside of being more difficult for some clients to use. A robust client or server library will provide all three methods where appropriate, and in fact our demonstration protected resource will accept an access token in any of the three locations.

### Refresh the access token

How does an OAuth client know whether its access token is any good? The only real way to be sure is to use it and see what happens. If the token is expected to expire, the authorization server can give a hint as to the expected expiration by using the optional expires_in field of the token response. This is a value in seconds from the time of token issuance that the token is expected to no longer work. A well-behaved client will pay attention to this value and throw out any tokens that are past the expiration time.

However, knowledge of the expiration alone isn’t sufficient for a client to know the status of the token. In many OAuth implementations, the resource owner can revoke the token before its expiration time. A well-designed OAuth client must always expect that its access token could suddenly stop working at any time, and be able to react accordingly.

- Khi client gởi request tới `authorization_server` để xin token thì client phải kèm `grant_type` vào body:
  * `grant_type=authorization_code`: Used in the Authorization Code Flow (the standard web app flow). This tells the server to look for the mandatory `code` parameter and the `client_secret`.
  * `grant_type=refresh_token`: Used to renew an expired Access Token. This tells the server to look for the `refresh_token` parameter. 

As you can see, refreshing an access token is a special case of an authorization grant, and we use the value refresh_token for our `grant_type` parameter. We also include our refresh token as one of the parameters.

Khi refresh token thì auth_server sẽ trả về new access token và một cái refresh token (có thể mới hoặc giống y chang cái cũ). If that happens, the client needs to throw away the old refresh token that it’s been saving and immediately start using the new one. 

## The OAuth Protected Resource

You built your web-based API, now you want to protect it using OAuth. This is the way.

Although the protected resource and authorization server are conceptually separate components in the OAuth structure, many OAuth implementations co-locate the resource server with the authorization server.

The OAuth Bearer token specification tells us that when the token is passed as an HTTP Authorization header, the value of the header consists of the keyword Bearer, followed by a single space, and followed by the token value itself. Furthermore, the OAuth specification tells us that the Bearer keyword is not case sensitive. Additionally, the HTTP specification tells us that the Authorization header keyword is itself not case sensitive. This means that all of the following headers are equivalent:

```
Authorization: Bearer 987tghjkiu6trfghjuytrghj
Authorization: bearer 987tghjkiu6trfghjuytrghj
authorization: BEARER 987tghjkiu6trfghjuytrghj
```

The token value itself is case sensitive.

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

## Spring Security, JWT

Spring Security is used to handle authentication and authorization.

The tokens themselves can contain information that the protected resource can parse and understand directly. One such structure is a JSON Web Token, or JWT, which carries a set of claims in a cryptographically protected JSON object. We’ll cover both of these techniques in chapter 11.

## Notes

`GET` request không được phép có phần body. Backlog nó nói Authorization Request của nó là `GET`, **form parameters** mà nó muốn là url param, không phải url-encode trong phần body nha.

- `application/x-www-form-urlencoded` is simple text sent inside the body and is not attached to the url
- `multipart/form-data` is required for file uploads.