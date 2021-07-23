## Understanding password hashing and salting

Passwords are the first and most important layer of security of any application. As a developer, it's your responsibility to ensure the highest possible security to your users. In this article, we will discuss password hashing which ensures the best possible security against a data breach


## What is password hashing?
Saving passwords as plain text is the worst. If an attacker compromises your database, he can see all the passwords of your users. This actually goes against privacy laws. Firstly, as the developer, you should never know the user's password. Secondly, a user might use common passwords across different devices and such data leaks might be devastating for an individual. So, how do you protect passwords?

You use password hashing. Hashing is a cryptographic way to secure users passwords. In this way, no one can find out the actual password even if he directly looks into the database. Even the application owners or developers wouldn't be able to tell the user's password.

In this procedure, the password is passed into a specific hash function, which then outputs a jumbled string. Let's say, my  password is `helloworld`, then:

```
// SHA256
hash('helloworld') = '936a185caaa266bb9cbe981e9e05cb78cd732b0b3280eb944412bb6f8f8f07af'
```

Here, the algorithm used for hashing is called `SHA256`. Given the same string, it is guaranteed that a hash function will always generate the same output. But hashing is a one-way algorithm. It means once a string is hashed, you can never get back to the original string using the hashed output.


> 
> **What is the difference between encryption and hashing?**
>
Encryption requires a secret key to encrypt with. Whoever has the secret key, can encrypt or decrypt as required. So, if you encrypt a string and you know the key, you can get back to the original version using that key. But hashing is one way. Meaning, there is no way to go back to the original string.

Whenever a new user registers with a service, his password is hashed and stored in their database. The database user table might look like this:


| id                   | name                               | password                         |
| -----------------------------------------------------------------  |
| 1                    | alice                                 | ea71c25a7a602246b4c39824b855678894a96f43bb9b71319c39700a1e045222|
| 2                    | bob                                 | 3caf14423f92dbac6f80589aa72f7be572d7b87562f9fd32ed0c4427b680b598|
| 3                    | john                                 | cc6db669c9856670ab5faf03a76b4885d146e62a1fd18ab7693a525368e7d654|

**Now that you have stored the hashed password, how do you compare the passwords during authentication?**

As I said earlier, hashing is a one-way algorithm. So, there is no way to get back to the original password. The authentication is done as below:

- User provides his plain password during login
- The plain password is hashed and the new hash is compared with the existing hash in the database
- If the hash matches, the user is authenticated

## Security considerations for password hashing
Unfortunately, such simple hashing is not enough. An attacker might use brute force attacks using a rainbow table or hash table. These two tables are different but for simplicity, we're not going to discuss the difference. A rainbow table or hash table is basically an original password - hashed password mapping using the most common password phrases. Hash tables are huge containing millions (if not billions) of common passwords and can size up to 100GB. Rainbow tables take less space but lookup is slow. For the rest of the article, we will use the terms interchangeably. A hash table might look like this:

| input                  | hash
| ------------------------------------------------------------------- |
| hello                    | 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824
| unlockme            | 06f088e12170d07e80fced4405bb4d7c3ec0153d1e3c136be0f58502ef97b364
| password1234    | a28d722529366b0e1ff5d75c7ce5676b1a574a982d6b0a2b32050c565139dc8b

We know, for a given string and the same hashing algorithm, it will always produce the same output. The hashing algorithms used for passwords are built to be **slow** for the attacker's inconvenience. But in a rainbow table, a hash is pre-computed (the attacker prepared the table earlier). The attacker just has to compare the hashes from the database and do a reverse lookup to find the password. Because of modern CPU/GPU power and distributed computing, an attacker can crack your password in record time in this way! That is why it is very important to use a strong password. Regardless, you have to solve this problem because you can't expect every user to use a very strong password.


> When a database is compromised, the attacker immediately dumps the database, which basically means saving a copy in your local disk. The attacks then run on the saved database to find out original passwords.


Oh, there is another problem. Let's say Bob and Alice use the same password `unlockme`. In that case, the hash would be the same for both user's passwords. The attacker will instantly know that the two people are using the same password, which can help the attacker to find patterns in the hashing and further accelerate the attack. For example, attackers will easily figure out there is no salting (described below) or your website uses default passwords for new accounts or you're using a weak hashing algorithm. You definitely don't want these to happen.

To overcome these above-mentioned issues, we *salt* the hashes.


### Salting your hashes
Salting is basically adding random strings to the passwords so that two passwords never match. Salts can be appended or prepended to the original password. For our previous example, Alice and Bob had the same password `unlockme`. If we salt this it would look something like this:

Alice's password (salted with append): unlockme**ea22$sas**

Bob's password (salted with append): unlockme**3xx#$/a5**

The character set in bold is the salt. In a real-world scenario, salt is generated using a **cryptographically secure** algorithm. The said term refers to such systems which are resistant to  [**cryptanalysis**](https://en.wikipedia.org/wiki/Cryptanalysis). 

> Proving a system to be cryptographically secure requires a huge amount of time, research, extensive testing and community engagement. Thus, it is always recommended to use a proven and standard cryptographic system. Don't try to roll out your own.

Such algorithms guarantee that salt will be unique and unpredictable. Now, although the password is the same, Bob and Alice's password would have different hash

```
hash('unlockmeea22$sas') = '1471dc5d222f43483aee85051691f45c7bf135e55d0b53184ffaf4cc4d30905f'

hash('unlockme3xx#$/a5') = '5983db3704436d24a6acccb87405cb002dfd2f04b22b451247d541b1dbca295a'
```

The salt should be unique for each and every password. The application/backend has to know which salt was used to hash a specific password. Otherwise, the application will never be able to produce the same output. This is why the salt has to be stored along with the hashed password. Let's see how the user table would look with salting in place. For this example, the salt is stored with the password using dot notation:

| id                   | name                               | password                         |
| -----------------------------------------------------------------  |
| 1                    | alice                                 | **fE07$/**.ea71c25a7a602246b4c39824b855678894a96f43bb9b71319c39700a1e045222|
| 2                    | bob                                 | **Xfk7aa**.3caf14423f92dbac6f80589aa72f7be572d7b87562f9fd32ed0c4427b680b598|
| 3                    | john                                 | **Q//$cc**.cc6db669c9856670ab5faf03a76b4885d146e62a1fd18ab7693a525368e7d654|


When Alice wants to log in with her correct password `unlockme` (suppose), she will provide her password in the login form. The server will find Alice in the user table and see that the salt is `fE07$/`. So the salted string will be `unlockmefE07$/`. This string will be hashed and then matched with the existing database hash `ea71c25a7a602246b4c39824b855678894a96f43bb9b71319c39700a1e045222`. If they both match, Alice is authenticated. If it doesn't, it means Alice gave the wrong password.

At this point, there is no way for the attacker to identify duplicate passwords.


**But how does salting prevent rainbow table/hash table attacks?**

As we saw earlier, hashing didn't prevent the attacker from cracking common/weak passwords. But salting solves the problem for us. Unlike traditional hashing algorithms like SHA256 or MD5, password hashing algorithms (like `bcrypt` or `blowfish`) are built to be **very slow**. There is a very specific reason for it.

Previously, the attacker pre-computed the hash. The initial computation took a lot of time, but after the table was prepared the attacker could use it indefinitely.

But when salting is introduced, each and every hash is unique. Because every salt is unique and unpredictable. Now, the attacker can't use any pre-computed hash table. For each password, the attacker has to create a rainbow table using that password's salt. This is a very tedious and time-consuming process.

In short, when the passwords were unsalted, the attacker:

- Pre-computed a hash table containing the most common password phrases. 
- Whenever he compromised a database with unsalted hashes, he just had to compare the hashes with the hash table and do a reverse lookup.

Now, with the salting in place:
- Attacker compromises a database and sees the passwords are hashed and salted
- If the attacker wants to crack Alice's password, he has to use the salt `fE07$/` and create a rainbow table with the common password phrases.
- For all the other users, the attacker has to create a new rainbow table with their salt and hope that the password is cracked. Do remember that rainbow tables can contain billions of common phrases and creating them is extremely expensive both from in time and space perspective.

There's more to it. We said several times, hashing is **slow** for the attacker's inconvenience. Let's see exactly how slow it is.

One of the most widely used library for password hashing is `bcrypt`. Here is an example of password hashing using `bcrypt` with Node.js:

```javascript
// app.js
const bcrypt = require("bcrypt");
const saltRounds = 15;
const plainTextPassword1 = "veryweakpassword";
bcrypt
  .genSalt(saltRounds)
  .then(salt => {
    console.log(`Salt: ${salt}`);
    return bcrypt.hash(plainTextPassword1, salt);
  })
  .then(hash => {
    console.log(`Hash: ${hash}`);
    // Store hash in your password DB.
  })
  .catch(err => console.error(err.message));
```

Here, the variable `saltRounds` is important. It is the cost or work factor for the salting algorithm. The higher the cost, the more iterations will be done to generate the salt, thus significantly slowing down the hashing process. Actually, the time required for salting grows **exponentially** with the cost.


![graph.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1627023246851/p5T4i24JE.gif)

In a *Core i7, 16 GB RAM* system salting with 15 rounds requires around **20 seconds** to complete, whereas 20 rounds require **66 seconds** to complete. In general applications, the cost/round is reduced to not cause any significant delay when registering new users. Even a 4-5 seconds delay is more than enough. Just imagine the attacker has to wait 5 seconds to generate each hash, where there are billions of phrases and salting is in place to force the attacker to generate a new rainbow table for each password.


### Handling data breach
If your database is breached, you should immediately reset all user's passwords and inform them via email. Occasionally, a company can choose to inform their users to change passwords rather than resetting them from their side. 

With enough computation power, the attacker might be able to reveal few weak passwords although salting and hashing slowed them down drastically. In such breaches, it is not possible to know exactly which passwords were compromised. Thus, we should consider all passwords as compromised and inform the users accordingly.

## Protecting yourself from a data breach

- You're at risk only if you're using a weak password, a password that is found in the common password list. So, always use a strong password. 
- Use a password manager like [BitWarden](https://bitwarden.com/) . 
- When registering for new websites, generate a long (around 20-25 characters) password using BitWarden. With a strong password, the chance of password compromise is zero. `k8FQxVP&t28jB&!J!iCi` is an example of strong password which I generated using BitWarden while writing this article.
- Don't use the same password on multiple websites
- Change the password of important or transactional websites every 1-2 months.
- Additionally, use  [HaveIBeenPwned](https://haveibeenpwned.com) to find out if your passwords were ever compromised, if so, change all your existing passwords. 


Thank you for reading. Don't forget to leave your feedback and questions in the comments below. Also, if you like my articles then please follow/subscribe to the blog.