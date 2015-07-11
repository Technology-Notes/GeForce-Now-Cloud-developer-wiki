NVIDIA provides a backend IDM Service that application developers can use to validate users and obtain user information from. 
A simplified flow diagram of how it functions is shown below:

![Account Federation Flow](https://github.com/camify/GFN-Link/blob/master/docs/AccountFederationFlow.PNG)

## Components
### Game Seat
The Game Seat is a VM that is used to run the game on.
### GeForce NOW Agent
The GFN Agent is a service that is running on the game seat and is responsible for communication between the GFN Link SDK and other GFN external services such as the IDM, managing the game to provide start and stopping of the game along with game saves and general VM configuration. 
### GeForce NOW IDM Service
The GFN IDM (Identity Management) Service provides the single sign-on core of the GeForce NOW Platform. Users will be able to create accounts within and across titles that integrate the services. The identity service allows the federation of multiple external services, for associating NVIDIA-created accounts.
### GeForce NOW Link SDK
The GFN Link SDK is a C++/C SDK that provides a simple set of methods and events for developers to allow their games to run in the GeForce Now (GFN) environment.

## Steps
**1.	Launch Game (GeForce NOW Agent)**<br/>
Once the Game Seat is started the Agent will launch the game.

**2.	Request GFN Access Token (Game – GeForce NOW Link SDK)**<br/>
The game invokes a request to get a GFN Access Token. 

**3.	Provide Access GFN Access Token (GeForce NOW Agent)**<br/>
The Agent communicates with the GeForce NOW IDM Service to provide this token of the current user’s access to the Game Seat. 

**4.	Return Access Token (GeForce NOW IDM Service)**<br/>
The IDM service returns an access token to the GFN Agent that passes back to the Game via the GFN Link SDK. 

**5.	Pass Access Token to Game Backend (Game)**<br/>
The game will then pass the token to the game backend this may be the developer’s IDM service.

**6.	Authenticate Using GFN Token (Game  Backend)**<br/>
The game backend will validate the access token against the GFN IDM.

**7.	Validate and Return Session GFN Token**<br/>
The GFN IDM will respond, if successful, with a session token that can be used to get access to user data.

**8.	Request User Data**<br/>
The game backend will request the user’s profile information using the session token. 

**9.	Provide User Data**<br/>
The IDM will respond with the user’s profile data such as username, email address, birthday, profile image and 3rd party tokens.

**10.	Validate User Data**<br/>
Once the game backend has this data, it can check if the user already exists by verifying it against a shared token or matching email address.

**11.	Existing Account**<br/>
If there is a token match with an existing account, the user can be logged in.

**12.	Match Accounts**<br/>
If there is no shared token present in the user data, the game backend can choose to match to an existing account based on the user’s email address. If the account is found, the user can be logged in and the token created in the game backend should be sent to the GFN IDM.

**13.	Create Account**<br/>
If no shared token is found and there is no user data match, the user must then create a new account with the developer’s game backend. Once the account is created, the token created in the game backend can then be sent to the GFN IDM.