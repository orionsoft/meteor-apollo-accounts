# Meteor Apollo Accounts

A implementation of Meteor Accounts only in GraphQL with Apollo.

This package uses the Meteor Accounts methods in GraphQL, it's compatible with the accounts you have saved in your database and you may use apollo-accounts and Meteor's DPP accounts at the same time.

## Examples

- [janikvonrotz/meteor-apollo-accounts-example](https://github.com/janikvonrotz/meteor-apollo-accounts-example): Meteor client and server side.

# Installing

### Install on Meteor server

```sh
meteor add nicolaslopezj:apollo-accounts
```

Load schema Types and Mutations

```js
import { SchemaMutations, SchemaTypes } from 'meteor/nicolaslopezj:apollo-accounts'

const rootSchema = `

${SchemaTypes({
  UserProfileInput: `
    firstname: String
    lastname: String
    name: String
  `
})}

type Mutation {  
  ${SchemaMutations}
}
`
```

Load auth resolvers into your Mutation resolver

```js
import { Resolvers } from 'meteor/nicolaslopezj:apollo-accounts'

export default {
  Mutation: {
    ...Resolvers
  }
}
```

### Install on your apollo app

May or may not be the same app.

```sh
npm install meteor-apollo-accounts
```

# Methods

Meteor accounts methods, client side only. All methods are promises.

#### loginWithPassword

Log the user in with a password.

```js
import { loginWithPassword } from 'meteor-apollo-accounts'

loginWithPassword({username, email, password, plainPassword}, apollo)
```

- ```username```: Optional. The user's username.

- ```email```: Optional. The user's email.

- ```password```: The hashed user's password.

- ```plainPassword```: Optional. The plain user's password. Recommended only for use in testing tools, like GraphiQL.

- ```apollo```: Apollo client instance.

#### changePassword

Change the current user's password. Must be logged in.

```js
import { changePassword } from 'meteor-apollo-accounts'

changePassword({oldPassword, newPassword}, apollo)
```

- ```oldPassword```: The user's current password. This is not sent in plain text over the wire.

- ```newPassword```: A new password for the user. This is not sent in plain text over the wire.

- ```apollo```: Apollo client instance.

#### logout

Log the user out.

```js
import { logout } from 'meteor-apollo-accounts'

logout(apollo)
```

- ```apollo```: Apollo client instance.

#### createUser

Create a new user.

```js
import { createUser } from 'meteor-apollo-accounts'

createUser({username, email, password}, apollo)
```

- ```username```: A unique name for this user.

- ```email```: The user's email address.

- ```password```: The user's password. This is not sent in plain text over the wire.

- ```profile```: The profile object based on the UserProfileInput input type.

- ```apollo```: Apollo client instance.

#### verifyEmail

Marks the user's email address as verified. Logs the user in afterwards.

```js
import { verifyEmail } from 'meteor-apollo-accounts'

verifyEmail({token}, apollo)
```

- ```token```: The token retrieved from the verification URL.

- ```apollo```: Apollo client instance.


#### forgotPassword

Request a forgot password email.

```js
import { forgotPassword } from 'meteor-apollo-accounts'

forgotPassword({email}, apollo)
```

- ```email```: The email address to send a password reset link.

- ```apollo```: Apollo client instance.

#### resetPassword

Reset the password for a user using a token received in email. Logs the user in afterwards.

```js
import { resetPassword } from 'meteor-apollo-accounts'

resetPassword({newPassword, token}, apollo)
```

- ```newPassword```: A new password for the user. This is not sent in plain text over the wire.

- ```token```: The token retrieved from the reset password URL.

- ```apollo```: Apollo client instance.


#### loginWithFacebook

Logins the user with a facebook accessToken

```js
import { loginWithFacebook } from 'meteor-apollo-accounts'

loginWithFacebook({accessToken}, apollo)
```

- ```accessToken```: A Facebook accessToken. It's recommended to use
https://github.com/keppelen/react-facebook-login to fetch the accessToken.

- ```apollo```: Apollo client instance.

#### loginWithGoogle

Logins the user with a google accessToken

```js
import { loginWithGoogle } from 'meteor-apollo-accounts'

loginWithGoogle({accessToken}, apollo)
```

- ```accessToken```: A Google accessToken. It's recommended to use
https://github.com/anthonyjgrove/react-google-login to fetch the accessToken.

- ```apollo```: Apollo client instance.


#### onTokenChange

Register a function to be called when a user is logged in or out.

```js
import { onTokenChange } from 'meteor-apollo-accounts'

onTokenChange(function () {
  console.log('token did change')
  apollo.resetStore()
})
```

#### userId

Returns the id of the logged in user.

```js
import { userId } from 'meteor-apollo-accounts'

console.log('The id is:', userId())
```
