### GunJS-AUTH
#### SIGN-UP, LOGIN, LOGOUT
<br>

![image](https://user-images.githubusercontent.com/67427045/216623690-b1174b9b-b845-4abf-b9e7-2d0d948cedfa.png)
<br>
<br>

### Ready to go sign-up, login, logout for the decentralized database GunJS.
### GUNJS 5 min Quickstart + AI coding partner https://github.com/worldpeaceenginelabs/GUNJS-STARTERKIT-QUICKSTART-ARTIFICIAL-INTELLIGENCE-PAIR-CODING
<br>

## This code is a working example of a user authentication system using the GUN database.
- When a user signs in or creates an account, their username and password are stored in the GUN database for recall.
- The Passphrase the user chooses will be extended with PBKDF2 to make it a secure way to login.
- When a user logs out, their session is terminated.

# Next?
##### Working on 2FA with Barcode and Google Authenticator. Stay tuned
<br>

![image](https://user-images.githubusercontent.com/67427045/216617981-f93a18f4-b558-4881-ac76-11fc7b358271.png)

# There is a flaw to it for now!!!

## USER

- We know 99.9% it is the same user(cool)
- We don't know who it is (that's ok too), not if it's a human or bot, not if it's a human with multiple accounts, or a human with multiple humans or bots with multiple bots, all with their own user accounts.... (everyone with the relay address can run user.create and user.auth, this is intentional)

### This is an issue even Google and Co have to fight with.
Classic solutions would be e.g. Human Verification strategies such as Captcha, logic puzzles, Video Human to Human Verification(deepfake gefährdet🥴), Authenticator Connect at Account Creation (I'm working on it because Account Creation hurdle + Security Improvement for the user) and similar.

## FLOODING - without AUTH

- Anyone with our relay address, VSCode and Gun can still execute put/get. (this is also wanted)

Against DDOS proxy helps (Cloudflare for Cloud Server, noIP.com for Home/Desktop Relays) You can also mask the addresses with their, but this helps only against hobby hackers. But still.

- What helps against 100x .get(couchsurfing).get(freeCouchOnMap).put(data) from console hacker/spammer/troll?

Against that helps that our app's user subscribes to content exclusively by user.get(couchsurfing).get(freeCouchOnMap).on(data)

So our app only works within the user space, which we control client-side through common sense limiting/rate limiting. The processing of the data by the app is the only thing we can control at all, because the relay just does whatever it is told to do whether .get() or user.get().

## FLOODING - with AUTH

- What helps against 100x user.get(couchsurfing).get(freeCouchOnMap).put(data) from an authenticated user A?

Here the strategies "Common-Sense Request Limitations" and "Requests in a timeframe (request-rate)" will help.

- What helps against 100x user.get(couchsurfing).get(freeCouchOnMap).put(data) from 1000 authenticated users, all belonging to authenticated account A?
(automated script: 1000x user.create, 1000x authenticator connect, !!!1x!!! .get(couchsurfing).get(freeCouchOnMap).put(data) per user account, which is within our common sense limiting/rate limiting).

Theoretically pattern recognition helps against this, but we know how annoying this can be when legitimate content is blocked simply because it is "similar". (fb)

But we can certainly use something like.
"from X times the same post within X time period by different users = freeze posts and give them to moderation or delete them right away"
vllt even combined with swear word and XXX detection?

Maybe against bots and scripts a captcha for the first post, then all further X posts a captcha??? I think against 1000 people is just bad luck!

# Trying

I want to keep the relays as Mark thinks of them. So i will NOT touch anything on the relay!!!

Gun apps can only control 1. the data, 2. the encryption, and 3. the node path.

The rest is up to the relay logic(original Gun Relay = open, not modifiable), and the sender/receivers logic(our client-side app=modifiable).

## PROBLEM 1
VSCode + Gun dependency + 100x ```.get(couchsurfing).get(freeCouchOnMap).put(data)``` (hacker/spammer/troll)
(causes map full of sofas which not exist)

## SOLUTION (i think solves problem 100%???)
Our app users subscribe to content exclusively on User Spaces ```user.get(couchsurfing).get(freeCouchOnMap).on(data)``` (does this line subscribe to the graph ```freeCouchOnMap``` in all user-spaces? 😅)
(of course we still can use public space for other scenarios within the same app)

So our app only works within the user space, which we control client-side through "common sense limiting"/"rate limiting" per user.

The processing of the data itself by the app, is the only thing we can control at all, because the relay just does whatever it is told to do whether ```.get()```, ```user.get()```, ```user.create```, ```user.auth``` (Gun wants this openness, which is good)


## PROBLEM 2 (stucking here)
VSCode + Gun dependency + 1000 authenticated users posting only ONE time. (all belonging to one entity, manpower or botnet)(causes map full of sofas which not exist)

Automated Script: 1000x user.create + !!!1x!!! ```user.get(couchsurfing).get(freeCouchOnMap).put(data)``` per user account, which breaks through my "common sense limiting"/"rate limiting" of 1 post/each user/ each day).

Someone could execute user.create a thousand times offline and then sync with our relay.

## EFFECT: 
We can NOT limit the rate of user.create in a malicious app or the syncing of thousands of malicious user-accounts through the relay.

---

## POSSIBLE SOLUTIONS (stucking)

I think that against 1000 humans any strategy fails and is just bad luck!

But we can certainly use something like this, client-side:
```"X times the same post within a X time period by different users => hide the posts and give them to moderation"``` maybe even combined with XXX detection?

btw: Moderation does not have to be a centralized entity. Moderations of this kind could be polled by all users. "Please choose, is this spam?" for instance...

Pattern recognition theoretically helps against this, but we know how annoying this can be when legitimate content is blocked simply because it is "similar". (fb)

## STILL A PROBLEM
Bots and scripts seem to be a problem for Gun afaik for now!
One script attacks from one IP, a botnet with multiple scripts from multiple IPs.
IPs could be scanned for suspicious behaviour, but syncing 1000 user.creates and their follwing sync of 1000x .put() is not suspicious to DNS services. 🤷

# Continue Trying

## POSSIBLE SOLUTION
Gun maybe needs user.create + human verification integrated into the relays.

I imagine a new relay option (Like the production parameter ```production = true/false``` when spinning up a heroku Gun relay)

## VERIFICATION MODE(verification = true):
The signUp in our Gun app, forwards to a user.create process on the connected relay(same as Gun peers), which gives us a challenge: Human Verification (like reCaptcha or videocall with other users(like https://app.proofofhumanity.id/, but with Gun)??? I dislike Captchas but the video calls could be fun)

- If the human verification passes, the account gets distributed as always TO all other clients.
- If the user.create does not pass human verification, the account gets NOT synced with the relay.
- But relays do NEVER sync user-accounts FROM clients, when in verification mode. Only Relay to Relay.
 
This way we make 99% sure the user-account creator is a human and it will prevent automated user account flodding.<br>
We could also check regularly if the account-creator is still a human and take measurements if not passed.

But this can also easily be circumvented by VSCode + Gun dependency + 1000x offline user.create then sync with a normal desktop relay, then sync relay-to-relay.😭

Don't know further from here 🥴
