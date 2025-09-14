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

1. set of Users with:
    username (String)
    password (String)
    ==confirm flag==
    ==token (Secret)==

2. register (username: String, password: String): (user: User, ==token: Secret==)
require: unique username from the set of User info in our system
effect: create an user with the username and password, ==confirm = false, and a unique secret token==

authenticate (username: String, password: String): (user: User)
require: an user exists with the username and password combo, ==and the confirm status is true==
effect: return the corresponding user from our system

==confirm (user: User, token:Secret)
 require: the user is existing, the confirmation flag of user is false, and has token.
 effect: change confirm flag to true for the user==

3. An invariant would be the usernames in the set of Users are all unique from each other. This is preserved by the precondition requirement that one must register with an username not in our systems of users.

4. See above changes in highlight


## Exercise 3

<!--Notes: must verify email address
generate new token
able to select if it expires or not
select scopes token can grant access
Deleting token
token is used when performing git operations over https(website)
when entering username and token, it's the token authenticating you, but username is also needed

token only used for https git operations, not ssh remote url
-->
**Concept** Personal Access Tokens

**Purpose** Limit access to known user but with limited scope and only git operations over HTTPs

**Principle** a confirmed existing user can create their personal access token, choose a name for the token, choose expiration date for token, and choose the scope they want to limit if access is gained through token. After creating the token, user can login next time with username and the token associate with their account to be treated each time as the same user but with limit access specified by the specific token

**State**

Set of users:

    username (String)

    confirm flag (boolean)

    password (String)

    set of personalAccessToken (personalToken)

    Accessibility (Set of Scopes)

Set of Personal Token:

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

### Conference Room Booking
**Purpose** allowing users to book rooms for use

**Principle** Users can book available rooms with available time slots in advance for events, or cancel their bookings if no longer needed, or edit their booking

**State**

Set of users:

    set of bookings

Set of Bookings:
    Name (String)

    Owners(Users)

    date (Date)

    duration (slots)

    Location (Room)

duration
: includes the time start of the booking, since slot are assigned a timeframe starting from time to anothe time, each slot lasting one hour

Set of Dates:

    Rooms (set of rooms)

Set of Rooms:

    Name (String)

    Slots (set of slots)

Set of slots:

    booked (boolean)

    time start (Time)

**Actions**

**booking(user:User, location: room, date: Date, duration: slots, owners:set of users?): (booking: Booking)**

require: date or duration of booking must be after current time. Room is available during the date and all slots selected.

effect: Create a booking for the user at room on date and can be used for the duration slots. Owners added by the user can also have access to edit time, location, or to cancel the booking. The slots for that room under date selected by the user will be marked booked = true.

**cancelBooking(owner: User,booking: Booking)**

require: owner has full access to booking

effect: delete the booking, and mark the room on date for slots in the booking to booked=false

**editBooking(owner: User, booking: Booking, date: Date, room: Room, duration: slots, otherOwners: Users):(newBooking: Booking)**

require: owner has full access to booking, date or duration of booking must be after current time. Room is available during the date and all slots selected ignoring the slots booked by this booking.

effect: delete the booking, and mark the room on date for slots in the booking to booked=false, then create another booking under owner with the other parameters feed in.


### Address Verification

**Purpose** Authenticate users through address verification to ensure identity

**Principle** An user enters their address and authenticate if the address matches with the address hold in personal information, when system queries external information.

**State**

Set of users:
    Personal Information (info) [^1]
    Address (address)

**Action**

**Authenticate(user: User, address: Address): (user: User)**

**require**: an existing user, user has association with the address as provided by their additional information, address is also stored in the set of addresses of the user

**effect**: return the specific user associated with both the address and the same personal additional information

**updateAddress(user: User, newAddress: Address)**

**require:** an existing user, the personal information was already updated with the newAddress

**effect:** change the address associated with the user to the newAddress as provided by the personal information

[^1]: Since state is not stored at the location, authentication is an one time thing and is needed eveytime. Also personal information is depending on the stakeholders and we actually verify the address with the user by if the address is contained in the personal information already. For example, the users can provide a credit card, and only authenticate if they enter the exact same the billing address as stored. Or the same address associate with their phone number, utility bills, etc.


### URL Shortener

**Purpose** shortening long URLs for users to shorter URLs that can be user-defined or autogenerated and can be clicked by other users

*Assuming only focusing on the shortening feature, not including the account creation as in like [bit.ly](https://bitly.com/)*

**Principle** An user can enter their URL they want to shorten, and choose to whether to customize the suffix of the link or not. When users click on the link generated, they can be brought to the website of the original long url the user inputted

**State**

set of shortLink:

    longURL (link)

    urlSuffix (String)

**Action**
**shortenURL(longURL: link, suffix: String?):(shortLink: ShortLink)**

**require**: an existing working link, if suffix is given, must be a unique name in the domain

**effect**: return a shorter URL with the user-defined suffix if given, else an autogenerated suffix. The shorter URL can be clicked by other people but still brings to the same website by the longURL. The shorter URL is also stored in the user's data.

**deleteURL(shortURL: ShortLink)**

**require**: the shortURL exist

**effect**: remove the shortURL stored in the system along with the long URL associate with the shortURL. After removal, if people click through the shortURL, error webpage will pop up of not being able to find the shortURL in the system.
