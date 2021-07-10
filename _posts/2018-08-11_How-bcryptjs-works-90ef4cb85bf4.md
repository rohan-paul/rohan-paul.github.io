# How bcryptjs works

Using bcrypt is a secured way to store passwords in my database regardless of whatever language my app’s backend is built in — PHP, Ruby…

Learn how to use bcryptjs for a secured way of storing passwords in any database.

![](https://cdn-images-1.medium.com/max/800/1*cYPdg0MnTsOI_0kM5V4zXg.jpeg)
undefined

#### Using **bcrypt** is a secured way to store passwords in my database regardless of whatever language my app’s backend is built in — PHP, Ruby, Python, Node.js, etc.

To install it in a node app just run **npm install bcryptjs**

**First, what exatly is password hashing — see the below exmple**

undefined
undefined

Some features of bcrypt

**Salted hashing** — Generating random bytes (the salt) and combining it with the password before hashing creates unique hashes across each user’s password. If two users have the same password they will not have the same password hash. This is to prevent [rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) attacks which can reverse hashed passwords using common hashing functions that do not utilize a salt.

Uses Hashing algorithms that are **one way functions**. They turn any amount of data into a fixed-length “fingerprint” that cannot be reversed. They also have the property that if the input changes by even a tiny bit, the resulting hash is completely different (see the example above). This is great for protecting passwords, because we want to store passwords in a form that protects them even if the password file itself is compromised, but at the same time, we need to be able to verify that a user’s password is correct.

The general workflow for account registration and authentication in a hash-based account system is as follows:

1> The user creates an account.

2> Their password is hashed and stored in the database. At no point is the plain-text (unencrypted) password ever written to the hard drive.

3> When the user attempts to login, the hash of the password they entered is checked against the hash of their real password (retrieved from the database).

4> If the hashes match, the user is granted access. If not, the user is told they entered invalid login credentials.

5> Steps 3 and 4 repeat every time someone tries to login to their account.

**bcrypt works in 2 steps — The regular steps are A> Generate the salt first (if err throw err else give me the salt)**

**and then B> hash the password with the generated salt (passing a cb so if there’s error throw error else give me the hash).**

So from [official doc](https://github.com/dcodeIO/bcrypt.js#usage---async) the below function is for the first step of **generating the salt and hashing**

![](https://cdn-images-1.medium.com/max/800/1*3EnupfmEKRhzfrmfKF8UCQ.png)
undefined

Bcrypt allows us to choose the value of saltRounds, which gives us control over the cost of processing the data. The higher this number is, the longer it takes for the machine to calculate the hash associated with the password. It is important when choosing this value, to select a number high enough that someone who tries to find the password for a user by brute force, requires so much time to generate all the possible hash of passwords that does not compensate him. And on the other hand, it must be small enough so as not to end the user’s patience when registering and logging in (this patience is not usually very high). By default, the saltRounds value is 10.

[**One example of the first step of generating the salt and hashing from an app that I am building -**](https://github.com/rohan-paul/Tiny-Twitter-Clone/blob/master/models/user.js)

![](https://cdn-images-1.medium.com/max/800/1*LJJ8WC1BJADZJ-uulXerUA.png)
undefined

Bcrypt allows us to choose the value of saltRounds, which gives us control over the cost of processing the data. The higher this number is, the longer it takes for the machine to calculate the hash associated with the password. It is important when choosing this value, to select a number high enough that someone who tries to find the password for a user by brute force, requires so much time to generate all the possible hash of passwords that does not compensate him. And on the other hand, it must be small enough so as not to end the user’s patience when registering and logging in (this patience is not usually very high). By default, the saltRounds value is 10.

[**One example of the first step of generating the salt and hashing**](https://github.com/rohan-paul/Tiny-Twitter-Clone/blob/master/models/user.js)

![](https://cdn-images-1.medium.com/max/800/1*eIcisK2Buf53yKzPWfbnPg.png)
undefined

Explanation of **UserSchmea.pre** in above (as a side point) — Its the middleware — also known as “pre” and “post” hooks that tie particular functions to particular lifecycle and query events. This middleware is defined on the schema level and can modify the query or the document itself as it is executed. Middleware is invoked with two arguments: the event trigger (as a string) and the callback function that is triggered for that particular event. The callback itself takes in an argument of a function, which we typically call next , and when invoked — advances the document/query to the next awaiting middleware.

So what the below function does is — before (i.e. pre) user saves the normal text password into the database, making sure it encrypts it first

[**Example code of first hashing and then save the hashed password into mongo**](https://github.com/rohan-paul/Developer-Profile-App/blob/master/routes/api/users.js)

![](https://cdn-images-1.medium.com/max/800/1*VqcxwTzQB6nnNWTzoQFF4g.png)
undefined

**Example code for the 2-nd** [**step before signing-in a new user, it compares the password with that saved in the database**](https://github.com/rohan-paul/Developer-Profile-App/blob/master/routes/api/users.js)

![](https://cdn-images-1.medium.com/max/800/1*5UO30s40i7KHld5RRaoooQ.png)
undefined

**Authentication at the time of signing in a user**

The salt is incorporated into the hash. The compare function simply pulls the salt out of the hash and then uses it to hash the password and perform the comparison.

When a user logs into our system, we need to check that the password entered is correct. Unlike other systems that would decrypt the password in the database (if it is encrypted), and compare it with the one entered by the user, what I do with bcrypt ( **given it implements one-way hashing**) is encrypt the one entered by the user. To do this, I will pass the password to bcrypt to calculate the hash, but also the password stored in the database associated with the user (hash). This is because, as mentioned before, the bcrypt algorithm used a random segment (salt) to generate the hash associated with the pasword. This was stored along with the password, and you need it to recalculate the hash of the password entered by the user and finally compare with the one entered when registering and see if they match.

Looking at the [source code of **bcrypt.compare** function](https://github.com/dcodeIO/bcrypt.js/blob/b09f7f266a7015456b7b36deeb026dc636f64542/dist/bcrypt.js#L269) makes the above steps clear

![](https://cdn-images-1.medium.com/max/800/1*tuP93hhOymfu27O30zmaBA.png)
undefined

**A useful resource — Online bcrypt hashing and de-hashing generator and checker**

[https://bcrypt-generator.com/](https://bcrypt-generator.com/)

Just put the rounds (which is the salt length to generate, i.e. the function where I am hashing the plain-text password )

\`bcrypt.hashSync(plainTextPassword, 10)\` So the number 10 is the rounds in the above online tool

After hashing a plaintext password, for checking I will just put the hashed password from the mongo database — i.e. after running terminal command something like \`db.users.find()\` which will give all the users saved in the mongo database.

So an example is this hashed password — \`$2a$10$m0mq4PYOOvm74Gukml4FN.T0Ntobhzi42T6b5v1WIsJ5aZkVzJz3a\` And then put the round as 10 and I will get \`123\` which was my plaintext password in this case.

By [Rohan Paul](https://medium.com/@paulrohan) on [August 11, 2018](https://medium.com/p/90ef4cb85bf4).

[Canonical link](https://medium.com/@paulrohan/how-bcryptjs-works-90ef4cb85bf4)

Exported from [Medium](https://medium.com) on December 12, 2020.