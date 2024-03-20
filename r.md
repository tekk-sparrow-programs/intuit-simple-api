# Intuit Simple API

This project is a simple bare bones demonstration on how to connect and make a call to the Intuit developer API. This is not intended to be a production style implementation/consumption of their API, but merely an exercise in applying some of the core concepts of making API calls for web applications:

* The basics of the OAuth2 protocol.
* Setting up an application with a resource provider. (in this case Intuit)
* Making use of [`requests`](https://github.com/kennethreitz/requests) and the [`requests-oauthlib`]((https://github.com/requests/requests-oauthlib)) libraries to simplify the coding.
* Some basic security considerations
  * Not exposing API keys in source code
  * Leveraging OAuth2 state parameter for CSRF vulerabilities
  * Implementing the HTTPS protocol to encrypt communications


## OAuth2 basics

### Workflow

1. **Register Your Application**: Before making API calls, you need to create/register an application in the resource provider's Developer Portal. This gains you access to the credentials and any other pieces of information necessary to acess a protected resource.
1. **User Authorization Request**: Send the user to the resource provider's OAuth2 authorization server, where they log into their Intuit account and grant your application permission according to the provided scope.
1. **Exchange Auth Code for Access Token**: In this example we will manually extract the code from the redirect url and exchange it for the token. This of course being a simple example, is not how things work in the real world. Ideally the resource provider, redirects our user to our application where the code would be extracted programatically.
1. **Make API Calls**: Configure our OAuth2Session object from the [`requests/request-oauthlib`](https://github.com/requests/requests-oauthlib) library and start firing off API requests.

Refresh Access Token: If the access token expires, use the refresh token to obtain a new access token without requiring the user to grant permission again.

### OAuth2 Sequence Diagram
                                            Those login prompts
    +-------+                               +---------------+
    |  You  |                               | Authorization |
    |       |---(1) Ask for permission----->|     Server    |
    +-------+       via the application     +---------------+
         ^                                         |
         |                                         |(2) You say "Okay"
         |                                         |    sends you to redirect website
         |                                         v
    +-------+                               +---------------+          +------------------+
    |  App  |<--(4) Trade code for an ------|   Website     |          |    Resource      |
    |       |       access token            | (you said ok) |<-(3)Code-|      Server      |
    +-------+                               +---------------+          +------------------+
      ^  |                                                                     ^    |
      |  |                                                                     |    |
      |  \---(5) Use access token to get stuff---------------------------------+    |
      |                                                                             |
      \---(6) Protected stuff sent back to the app----------------------------------+

## Register Your Application

1. Create a developer account with Intuit.
1. Sign into said account.
1. From the dashboard, click "Create an app".
1. Select a target platform (Intuit product), Quickbooks Online and Payments in the case of this example.
1. Give your app a name and select the com.intuit.quickbooks.accounting scope.
1. Select "Development Settings" in the left side navigation panel
1. Add your application URLs  
   **Host Domain**
   `tekk-sparrow-programs.github.io`  
   **Launch URL**
   `https://tekk-sparrow-programs.github.io/redirect/`
1. Select Keys & Credentials: Copy the keys down for later
1. Add a redirect URL
   `https://tekk-sparrow-programs.github.io/redirect/`
1. **[WIP]**


### Resources

* [Intuit create and start developing your application documentation page](https://developer.intuit.com/app/developer/qbo/docs/get-started/start-developing-your-app)

* [Create developer account link](https://developer.intuit.com/app/developer/myapps)
