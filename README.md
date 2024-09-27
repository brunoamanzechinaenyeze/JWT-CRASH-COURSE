### AUTHOR: AMANZE BRUNO CHINAENYEZE |-->👋<--|

## JWT AUTH TUTORIAL IN NODE JS PART 1 (REGISTERING A USER)

## ARE YOU FINDING JWT AUTH HARD?? WELL WORRY NO MORE🥺😀 HERE IS THE CRASH COURSE👋

--Firstly let's take this as the DB that we are storing our users--

The name of this Fake DB in JSON format is user.json
[{
"username": "John Doe",
"password": "12345"
}]

1. Import the necessary modules as follows: npm i dotenv cookie-parse jsonwebtoken bcrypt

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

## IT IS A BEST PRACTICE TO GO AHEAD AN PUT THE FOLLOWING IN A TRY AND CATCH BLOCK

`TRY`
--> f) Hash the password with bcrypt: `const hashedPwd  = await bcrypt.hash(pwd, 10)` It would ensure the security of our user's password, incase of hackers

--> f) Now let’s store the password and the user
--> h) `const newUser = { "username": user, "password": hashedPwd}`

--Notice in section `f` we passed in the hashedPwd instead of the raw pwd--

--> g) This tutorial we are using a a fake DB using JSON
--> h) `userDB.setUsers([...userDB.users, newUser])`
--> i) The code in `h)` stores add the newUser to the existing array of object in the JSON db.
--> j) Now let's write the file to the existing JSON DB😁
--> k) await fsPromises.writeFile(path.join(\_\_dirname, "user.json")), JSON.stringify(userDB.users)
--REMEMBER WE ARE USING ASYNCHRONOUS FUNCTION, the error in the writeFile is sorted out in the catch err code block and we use fsPromises too in the async function--

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
