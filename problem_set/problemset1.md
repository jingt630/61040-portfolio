# Problem Set 1

## Exercise 1

Each user has only one registry, one to one, is unique.

1.
The first invariant is the number of items a purchaser can buy is at most of the requested item count in the registry.

The second invariant is number of original requested count minus the total purchased count should be equal to the current remaining count of an item.

**The more important one is the first invariant** because allowing the purchaser to buy more than the requested item count meant extra copies of gift for the user. The user doesn't want that much and the gift givers can be spending more than what is needed. Invariant 2 is only for record keeping. **The action most affected by the second invariant would be the purchase action.** The action preserves the invariant by having a pre-condition that the item being purchased from the registry must have at least count.

2. RemoveItem action can breaks the invariant by removing items after somebody bought it for the user. The problem can be fixed by hiding away the request when a user want to remove an item in their registry, but the record is saved in the system.

3. The registry can be open and closed repeatedly, none of the actions has an effect of deleting a registry. Open and close only sets the active status of the registry, not the existence of a registry.

4. This wouldn't matter much in practice because if the registry is not needed, it can be simply closed. Data storing is cheap, and we can archive the user and its registry if needed instead of deleting.

5. User can look into set of Requests, into set of Purchases, then into the purchaser to reveal who bought it. The giver of a gift look into set of requests then the remaining count number to see if the request is fulfilled yet.

6. We can add a surprise flag for each registry to allow the user to toggle if they want to see the purchases.

7. SKU is an easy representation of the item and can store the item with the information (names,descriptions,prices,etc) if one really wants to look it up. SKU code makes every item unique even if they are similar avoiding confusion.

## Exercise 2

1. set of Users with
    username (String)
    password (String)
    <p style="color:blue;">confirm flag</p>
     <p style="color:blue;">token (Secret)</p>
2. register (username: String, password: String): (user: User,  <p style="color:blue;">token: Secret</p>)
require: unique username from the set of User info in our system
effect: create an user with the username and password, <p style="color:blue;"> confirm = false, and a unique secret token</p>

authenticate (username: String, password: String): (user: User)
require: an user exists with the username and password combo, <p style="color:blue;"> and the confirm status is true</p>
effect: return the corresponding user from our system

 <p style="color:blue;">confirm (user: User, token:Secret)
 require: the user is existing, the confirmation flag of user is false, and has token.
 effect: change confirm flag to true for the user</p>

3. An invariant would be the usernames in the set of Users are all unique from each other. This is preserved by the precondition requirement that one must register with an username not in our systems of users.

4. See above changes in blue


## Exercise 3

<!--Notes: must verify email address-->
generate new token
able to select if it expires or not
select scopes token can grant access
Deleting token
token is used when performing git operations over https(website)
when entering username and token, it's the token authenticating you, but username is also needed

token only used for https git operations, not ssh remote url

**Concept** Personal Access Tokens
**Purpose** Limit access to known user but with limited scope and only git operations over HTTPs

**Principle** a confirmed existing user can create their personal access token, choose a name for the token, choose expiration date for token, and choose the scope they want to limit if access is gained through token. After creating the token, user can login next time with username and the token associate with their account to be treated each time as the same user but with limit access specified by the specific token

**State**
Set of users
    username (String)
    confirm flag (boolean)
    password (String)
    set of personalAccessToken (personalToken)
    Accessibility (Set of Scopes)

Set of Personal Token
    TokenName (string)
    TokenValue (Secret)
    expiration date (time)
    Accessibility (Set of Scopes)


**Actions**

generateToken(user: User, TokenName: String, Expiration: Date?, Accesses: Set of Scopes): (token: personalToken)
require: an existing, confirmed user. The date must be after current date of generation if provided.
effect: create an unique token, with name and expiration date (if not provided, use default option), for the user that login with next time instead of a password along with username, but under the accesses (set of scope the user picked)

deleteToken(user: User, token: personalToken)
require: the user has token in them, and the token is not expired
effect: remove this personal token from the user, so the user can't login with it next time for limit access

authenicate(username: String, token: personalToken): (user:User)
require: user exists and has the token, username is not empty string
effect: return the user with the associate unique token, but can only access scopes specified in token

**NOTE:** Personal access token concept is different from *PasswordAuthentication* in the way of how account logins are authenticate, the pre-condition that a verified user exists, and limiting the access the user can have if they choose to login through this way. Password authenticate through username and password combination, but token only needs to verify through the token along, even though an username is still needed. Password gives full access and one user to one password, while one user can have many tokens.

We can add a section where we clarify the difference in purpose, authenication, and the accessibility limit for safety reasons.

## Exercise 4
