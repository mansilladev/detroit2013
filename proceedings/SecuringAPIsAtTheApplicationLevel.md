# Keeping APIs Secure at the Application Level

## Convener

Neil Mansilla (@mansillaDEV)

## Attendees

Please (@please), Add (@add), Yourself (@yourself)

## Date/Time

Tuesday, July 30, 2013 @ 1:30 PM EST

## Notes

**Discussion Dump**

* Neil opens with the challenges faced with protecting API keys/credentials in applications, be they in JS/HTML5 or even compiled in binaries of mobile apps (IPAs/APKs/etc).
* Andreas makes the point that absolute security is not achievable and that effort one puts into security is relative to the resources that you want to protect.
* Andreas states that he believe it is good enough to leave it up to the developer to protect their credentials in their binaries.
* Group discussion opens up on risks related to poor developer choices/implementations for the platform providers (also varies based on the type of resources)
* Neil brings up tokens and server-side solutions
	* Neil brings up payment provider [Stripe](https://stripe.com/docs/stripe.js) and how they handle sensitive data (credit card info) with tokens
	  * Update: Upon further investigation, the "publishable key" is still public; the exchange of customer credit card info for a token (handled by functions in Stripe.js, sent over https to api.stripe.com), sparing developers from having to handle/store sensitive data. Stripe does **not** have a requirement or mechanism for launching your own lightweight server for server-to-server comms
	* Neil brings up [Twilio](http://www.twilio.com/docs/client/capability-tokens) and their use of "capability tokens", which are based on [JSON Web Token](http://self-issued.info/docs/draft-jones-json-web-token.html) standard. Capability tokens are created server side.
	* Andreas mentions that his firm created a light-weight server that a developer on their platform stands up that talks to their client app, and in turn talks to the API backend during the OAuth dance.
* Brad mentions great blog article titled something like "You Are Terrible At Security" - pinged him on Twitter for link - gist is, if you're trying to do security yourself, you're going to fail.
* Brad manages healthcare platforms, and has definitely settled on OAuth 2.0.
	* Has implemented session tokens that have configurable use (i.e. single use, x-uses)
* Neil asked group about differences between OAuth 1 & 2, and solicited opinions
	* Andreas said that a problem with OAuth 2 is that there is nothing in the spec in regards to bearer tokens and security. They didn't take HMAC serious.
	* Consensus was that 1.0A was simpler, but more of a pain for clients
* Neil asked group if they used SSL Pinning - an extra certificate validation check against MITM attacks
	* Group didn't know much about it. [Good blog post here](http://blog.lumberlabs.com/2012/04/why-app-developers-should-care-about.html) about SSL pinning.
* Trevor asked a question to the group about opinions on HTTP Basic. (Trevor, if you remember what the discussion was around this, please update)
* Question raised (by ??) about how to persist bearer token securely between sessions
	* Discussion about secure local storage, but no clear answers
	* Mark shared a practice of creating a "readonly" token (for welcome back messaging), but for more sensitive operations, you'd require an active login (i.e. Amazon.com)
* Andreas solicited best practices/advice on revoking application credentials and renewing them, to which the only response was to issue an app update.
* Brad asked for opinions on JSONP vs. CORS.
	* His stance was to wait on CORS because of JSONP grants arbitrary code execution
	* Issue was that IE only shows real support in v10, but acknowledges that most modern browsers have supported CORS for eons
	* One workaround was to build a proxy.

**Takeaways** (please feel free to modify/add if you were there)

* Protecting API keys in applications is very difficult
* Hurdles and obfuscation are not perfect, but they do make it more difficult for attacker
* One solution is using server-to-server communication, where developer stands up "local" server, hardens the security between the application and the "local" server, which in turn does the server-to-server auth dance outside of public channel -- a more trusted proxy.
