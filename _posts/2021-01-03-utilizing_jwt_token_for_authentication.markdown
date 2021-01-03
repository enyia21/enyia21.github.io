---
layout: post
title:      "Utilizing JWT Token for authentication"
date:       2021-01-03 17:04:12 +0000
permalink:  utilizing_jwt_token_for_authentication
---


JWT Tokens are vital for authentication in modern web applications.  It provides a light weight means of securely transmitting information between parties as a JSON object.  JWT tokens allow for a secure way to either verify authorization, or to allow for secure information exchange.  JWT tokens are difficult to understand and implement at timesso I'd like to walk through the primary questions I've encountered when approaching JWT:

1.  How do you utilize JWT web tokens for authorization ?
2.  What is a JWT web token 
3.  Why we should utilize them as Full Stack web developers.     

We will answer how to use JWT web tokens by walking through the implementation of JWT web tokens on a web applications server side.  This application will utilize a Rails server with React on the client side.  What JWT tokens are and why we should utilize them are discussed in depth [here](https://jwt.io/introduction).  After CRUD functionality and authentication have been added to your rails application you may want to manage[ authorization](https://en.wikipedia.org/wiki/Authorization) using JWT tokens.   
*1. To start you need to import gems to manage JWT as well as your password secret.*  The two gems you should import are: 
```
gem 'jwt'
gem 'dotenv-rails'
```

`gem 'jwt'` - Imports the [jwt gem](https://github.com/jwt/ruby-jwt) that you will utilize to encode and decode JWT tokens.  

`gem 'dotenv-rails'` - Allows administrators to store secret keys server side, in a hidden file, prevent the exposur of secret keys to the web.  In order to protect you key make sure you add .env file to your .gitignore file before you push your application to github.   DON'T EXPOSE YOUR PASSWORD SECRET.  You will also be able to access your keys from a .env file using `ENV["<place key here>"]`.  

You will then run `bundle install` to install these two gems for your application.  

*2. Add a JWTkey to your .env file.* 
At this step you will use command line(`touch .env`) or GUI to create a .env file to be located in your applications root directory.  After creating the .env file you will edit the file and add a "JWT_KEY" and a "SECRET" using the syntax: 
``` 
JWT_KEY=<SECRET>
```
Please note that the secret should be something only you know.   Otherwise your jwt_token will not be secure.  

*3.   Create Code to manage the encoding of JWT tokens.  I chose to place my code to manage JWT tokens into my application_controller.rb file to ensure that any controllers inheriting function for the application controller would have access to these methods.  To encode a JWT token you can utilize the code:*
```
    def encode_token(user_info)
        JWT.encode({user_info: user_info}, ENV["JWT_KEY"])
    end
```
By creating a method that takes in *user_info* and passes a hash with* user_info* and the* ENV["JWT_KEY"]* into the **JWT.encode()** method, we can have a JWT token returned.  This token can be passed to our client side of the the application and then utilized for authorization purposes.  

*4.  After the creation of a JWT_Token we need to be able to decode a JWT token so that we can use it to authorize users.  The jwt gem is packaged with a **JWT.decode()** method that allows users to decode a passed JWT if the hash and secret key are correct.*

```
    def decode_token
        JWT.decode(get_token, ENV["JWT_KEY"])[0]["user_info"]
    end
```

*5.  In order to decode a JWT the JWT token must be passed to the server.   The best way to achieve this is to pass the JWT as a header with the key of Authorization.   This header can then be retrieved when a fetch request is made by using the request.headers command. *

```
    def get_token
        request.headers["Authorization"]
    end
```

*6.  Rember that you can utilize JWT.decode and find_by to your user by name, id. etc.*
```
    def current_app_user
        User.find_by_id(decode_token)
    end
```

*7.   The final steps for the server management of jwt tokens is the sessions controlller.  At login a user should be passed the jwt token if they are a verified user.  Here is some sample code of how to execute this on the server side.*
```
  def login
    user = User.find_by(username: params[:username])
    if user && user.authenticate(params[:password])
      # byebug
      user = User.find_by(username: params[:username])
      token = encode_token(user.id)
      render json: {user: UserSerializer.new(user), token: token}
    else
      render json: {errors: "LOGIN FAILED!!! PLEASE TRY AGAIN!"}
    end
  end
```


That is the primary walkthrough for the implementation of JWT tokens server side.

