### AUTHOR: AMANZE BRUNO CHINAENYEZE |-->👋<--|

### I AM PROUDLY A STUDENT OF COVENANT UNIVERSITY, OGUN STATE, OTA

## JWT AUTH TUTORIAL IN NODE JS PART 1 (REGISTERING A USER)

## ARE YOU FINDING JWT AUTH HARD?? WELL WORRY NO MORE🥺😀 HERE IS THE CRASH COURSE👋

--Firstly let's take this as the DB that we are storing our users--

The name of this Fake DB in JSON format is user.json
[{
"username": "John Doe",
"password": "12345"
}]

1. Import the necessary modules as follows: npm i dotenv cookie-parse jsonwebtoken bcrypt
   --we also need to create a .env file
   and the run node on the terminal window
   after that we run `require('crypto').randomBytes.toString('hex')` to get our token

then in the `.env`
ACCESS_TOKEN_SECRET = 559de9b3dc0828ce7d5f7eb74163440a2211a3efbb3f72ae1c0e4922497646c8b7a5f2869f743b393b5eef4a0d5398750af84c6353356e1094f85494dbbbbd85
REFRESH_TOKEN_SECRET = 87a9afebd2f60b1a5afdda3db602d2dbae3efe4177d57ffb70b606e540f2228e12355735074d89de06b4cc48d9180a68b1d2326867ec993666012607d394a170

we then write something like the above when we already have the randomly generated code
N.B: run the require('crypto').randomBytes.toSting('hex') twice to get two values for both the ACCESS and REFRESH TOKE

--> a) NOW LET’S SET UP THE REGISTER AUTH PAGE

--The following below is the neccesary modules we imported
`CONST USERDB = { users: require('./user.json'), setUsers: function (data) {this.users = data }} //Your Whatever your JSON file Path is` we are importing the JSON so we can pass the credentials we got from the user to it

const fsPromises = require("fs").promises;
const path = require("path");
const bcrypt = require("bcrypt");

--> b) req the credential from the user like the username and the password
`e.g`: const registerUser = async (req, res) => {
const { user, pwd } = req.body
}

N.B: We are routing this so we are going to export it

--> c) return an error Message if the user does not provide the credentials
--> d) check if there is a duplicate of the new user that wants to register
--> e) If there is a duplicate send a conflict error code `409`
e.g:
`const duplicate = userDB.users.find(person => person.username === user)`

## IT IS A BEST PRACTICE TO GO AHEAD AND PUT THE FOLLOWING IN A TRY AND CATCH BLOCK

`TRY`
--> f) Hash the password with bcrypt: `const hashedPwd  = await bcrypt.hash(pwd, 10)` It would ensure the security of our user's password, incase of hackers

--> g) Now let’s store the password and the user
--> h) `const newUser = { "username": user, "password": hashedPwd}`

--Notice in section `h` we passed in the hashedPwd instead of the raw pwd--

--> g) This tutorial we are using a a fake DB using JSON
--> h) `userDB.setUsers([...userDB.users, newUser])`
--> i) The code in `h)` stores add the newUser to the existing array of object in the JSON db.
--> j) Now let's write the file to the existing JSON DB😁
--> k) await fsPromises.writeFile(path.join(\_\_dirname, "user.json")), JSON.stringify(userDB.users)
--REMEMBER WE ARE USING ASYNCHRONOUS FUNCTION, the error in the writeFile is sorted out in the catch err code block and we use fsPromises too in the async function--

--We should also include the code below to know when our request is successful
--> res.status(201).json({
"success": `New User ${user} created`
})

--> Now let's catch any err at the end of the try block `You should understand👍`
catch (err) {
res.status(500).json({
"message": err.message,
})
}

--> Then module.export = { registerUser }

Remember We are importing it into our route page, probably you are probably doing this directly in the route page, it can work either way but setting a different route page makes your code more organised

`route.js`
const app = express.Router()
const registerController = require('./registerUser')

app.route('/', registerController.registerUser)

we can now pass this with app.use() in our server js file

N.B: This Crash Course is For Intermediate Developers, if you are a beginner you won't understand sh\*t🤣

[YAYY YOU'VE FINSHED REGISTERING THE NEW USER]

2. Now Let's Authenticate an existing user
   ¬¬Ofcourse from our previous section we used:
   `CONST USERDB = { users: require('./user.json'), setUsers: function (data) {this.users = data }}` to import our JSON DB, so that we can be able to make changes or fetch valid data from it. Let's Begin🪂

--> a.) We create a separate authentication js file
<--Let's import our necessary module for this section-->

const bcrypt = require("bcrypt");
const jwt = require("jsonwebtoken");
require("dotenv").config;
const fsPromises = require("fs").promises;
const path = require("path");

b) Request the credential from the user:
`const handleLogin = (req, res) => {
 const { user, pwd } = req.body;
 }`
--👍Now we are seeing a trend right? We first have to get the credentials from the user i.e (the username and password) they a making a request, so we a grabbing the values of their request by using the `req.body` method.

c) Next we need to check if the user actually provided the require credentials:

if(!user || !pwd) {
res.status(400).json({
"errorMessage": "Please provide the necessary credentials"
})
}

d) hmm, guess what's next🤭, absolutely!!! we have to check if the user is part of our DB (actually our fake DB🥺), how do we do that.🤨🤔

we say:
const foundUser = userDB.user.find(person => person.username === user);

e) Now let's check if the the user provided is part of our DB.

we say:
if (!foundUser) {
return res.sendStatus(401);
}
--Remember 401 ERROR CODE is for CONFLICT errors--
--You can check out the documentation for different error codes🙏--

--Now let's see if the password matches, this is where a very handy bcrypt module comes in--

we say:
const match = bcrypt.compare(pwd, foundUser.pwd);

then we have to check of the password matches the user's password

if (match) {
The sign is first going to take our payload, which is essentially what we want to serialize and we are going to serialize a user object

    and in order to serialize it we passed in a key into the jwt.sign() method

const accessToken = jwt.sign(
"username": foundUser.username,
process.env.ACCESS_TOKEN_SECRET,
{ expiresIn:
'40s'}
)
}

### TO BE CONTINUED
