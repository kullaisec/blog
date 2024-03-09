---
title: Zero Click Account Takeover
published: true
---

Hello all **Kullai** here I came with another Interesting Finding Where I was able to take-over any ones Account without user interaction due to one misconfiguration in the guest login.

![Alt Text](https://miro.medium.com/v2/resize:fit:1100/format:webp/0*5812jVnrspzF0wEJ.png)

### [](#header-3)Let's hack

I know it had been many days without writeups ... busy with some **good projects** and **learning new things**... With out further due let’s dive.

This Web application is a type of **Bus Ticket booking platform**. Where I was able to book tickets by creating an account and also, I was able to book tickets as a **guest user** providing the **email**.

With a good example let me tell you how this will be exploited.

The victim Email: `victim@test.com`

This victim has an account in the web application.

I [ `kullai@wearehackerone.com`] also have an Account in that and they use **API** after Login with my email and password I found one **GET** based API endpoint that gives my all details with the **Authorization Bearer [JWT]**.

<p align="center">
    <img src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*H2xVaFKZRhq6eUO9yWgUlQ.jpeg" alt="Alt Text" />
</p>
*My POST API Request*

I First decoded that JWT it as some ID but more difficult to crack.

<p align="center">
    <img src="https://miro.medium.com/v2/resize:fit:4800/format:webp/1*Bz-EJdF8PxENyp4qAl_9MA.png" alt="Alt Text" />
</p>
*decoded JWT PAYLOAD part*

Looked for the way-back to find some JWT but no use.

I logged out and tried to book a ticket without login. When I tried to book the ticket, It asked to sign-in

<p align="center">
    <img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*13UpwYtDwujd1xrFF8apgg.png" alt="Alt Text" />
</p>
*Guest User Login*

I simply Clicked on Continue a GUEST. and enter the random name and Victim email [`victim@test.com`]

<p align="center">
    <img src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*q1jpLu8M8VFiKKM3Ioa1Pg.jpeg" alt="Alt Text" />
</p>
*Guest Login with Victim Email address*

When I see this response, I got victim JWT’s

```json
{"code":10001,"result":"success","msg":"","output":{"user":{"firstName":"hacker","lastName":"","email":"victim@test.com","guest":true,
"userName":"victim@test.com","countryCode":"","mobilePhone":"","gender":"string",
"promoEmail":true,"promoMobile":true,"reserveNotification":true,"arabic":false,"cardNumber":"","memberid":"",
"balance":"","balanceInCent":0,"userId":"","city":"","experienceIconUrl":"","needLogin":false,
"dob":null,
"tokens":{"access":{"token":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJDMUZwdFdNREw0Yz0iLCJpYXQiOjE3MDczNzc4MDgsImV4cCI6MTczODkxMzgwOH0.4hZ0jfMyFeNhNqp__6e8yK3pBsjZrVuPGN-oLMoIWo4","expires":"2025-02-07T07:36:48.738+00:00"},
"refresh":{"token":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJDMUZwdFdNREw0Yz0iLCJpYXQiOjE3MDczNzc4MDgsImV4cCI6OTI1MzEwNTc4MDh9.Gc5Zx-MWT0Th65s0L_l0x6RNknsWPwCCtm3WWFDwKdA",
"expires":"4902-03-12T07:36:48.753+00:00"}}}}}
```

You can see the Access token Leaked.

<p align="center">
    <img src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*x91PWjBfNy4rLRGGHCoymA.jpeg" alt="Alt Text" />
</p>

Just Copy the JWT. Now go to the request where We have tested our /getprofile endpoint look image: My POST API Request in this writeup Now replace that JWT and see the magic

<p align="center">
    <img src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*caVpCdODDH39YLHlVAzzPA.jpeg" alt="Alt Text" />
</p>
*Access to victim account with ZERO Click*

I have access to all Api endpoints with that JWT token and I was able to perform any actions as the victim user.

Note: Via Burpsuite I have all access but as a guest login I was able to just book a ticket nothing much with the help of JWT I was able to do all actions as a victim user.

## [](#header-2)Summary:

*    Tried to book a ticket without account and it asked for the Login and Guest Login.
*    Clicked on Guest Login
*    Enter the random name and enter the victim email ID.
*    Just intercept that request and see the response.
*    the Access tokens are leaked.
*    Got access the account without victim interaction.

This is all about finding. Hope you have learnt something new.

## [](#header-2)Future Writeups: [Coming Soon]

*    Able to Join as an admin to any organization and takeover full ORG [Priv Esc.]
*    Able to delete All movie Tickets in the webapps without interaction.

And many more critical findings….

Follow me for more content:

[LinkedIn]((https://www.linkedin.com/in/kullai-metikala-8378b122a/)) | [Twitter](https://twitter.com/kullai12) 

Please contact me any freelancing at <a href="mailto:metikalakullai.gtl@gmail.com">metikalakullai.gtl@gmail.com</a> you will get replies in min's

<a class="btn btn-outline-dark" id="button-2" href="mailto:metikalakullai.gtl@gmail.com"></a>

Thanks for reading!!

Your [Kullai](https://kullaisec/github.io/portfolio) :)