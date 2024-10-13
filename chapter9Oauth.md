# Chapter 9: Authentication System

In this chapter, we will explore the authentication system of the application, which is implemented using **Passport.js** with the **Google OAuth2 strategy**.

## Setting Up Passport.js

First, we initialize Passport.js and configure it to use the Google OAuth2 strategy:
```javascript
passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: process.env.GOOGLE_CALLBACK_URL
}, (accessToken, refreshToken, profile, done) => {
    const email = profile.emails[0].value;
    const user = sqlm.getUserByEmail(email);
    
    if (!user) {
        console.log('User not found:', email);
        return done(new Error('User not found'), null);
    } else {
        return done(null, user);
    }
}));

## Forklaring
clientID and clientSecret: Disse verdiene finner du på Google developer console.
callbackURL: The URL hvor bruker vil bli redirected etter autentisering.
The code checks if the user exists in the database and handles errors accordingly.