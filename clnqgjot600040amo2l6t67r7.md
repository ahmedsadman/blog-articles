---
title: "The essence of writing clean code: Part I"
datePublished: Sat Oct 14 2023 19:55:52 GMT+0000 (Coordinated Universal Time)
cuid: clnqgjot600040amo2l6t67r7
slug: the-essence-of-writing-clean-code-part-i
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1697312916242/db761cd2-d7d6-4818-8e99-5617a2947ac3.jpeg
tags: software-development, coding, best-practices, beginners, clean-code

---

Writing code is a form of art 🎨 Following the basic principles of clean code can help achieve the level of artistry. If you write code for a living 👩‍💻, then sooner or later clean code will matter a lot to you. The concepts shown here are the gist of the book [Clean Code: A Handbook of Agile Software Craftsmanship](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) 📖 by Robert C. Martin

Engineers spend **80%** of their time reading code, and the rest **20%** of their time actually writing 🤯. In a work environment, it's important that your colleagues can understand (and maintain) your code easily.

# Principles of writing clean code

## 1\. Naming 🪧

The most simple requirement, yet the most difficult to achieve 🫠 If naming isn't done properly, the rest of the clean code principles don't matter much. So this section will be a bit longer compared to others. While naming your variables, functions, classes and such keep in mind that the names should be:

* Descriptive but to the point
    
* Implies what kind of data being stored
    

### Variables and Properties

Now, look into the following Python code

```python
# BAD
ca = db.Column(db.DateTime, required=False);

# GOOD 
created_at = db.Column(db.DateTime, required=False);
# ---------

# Suppose, we're creating a user with admin level permission
user = createAdminUser(...) # BAD, not enough context
userWithAdminPermission = createAdminUser(...) # OKAY, long variable names are less preferred
admin_user = createAdminUser(...) # GOOD
super_user = createAdminUser(...) # GOOD
```

Very simple yet strong example. Abbreviating variables or shortening them only creates confusion. *Remember, when you're writing the code, only you have the full context. Nevertheless, you should write code in such a way that people without context still understand the code easily 🤝*

Now let's look at another example

```python
# Example 1
# Storing raw user data as Python dictionary
user_data = {
    'name': 'Muhib',
    'age': 27,
    'email': 'test@email.com'
}

# Example 2
# Creating a SQLAlchmey (ORM) User DB object
user_data = new User(name, age, email); # BAD (Arguable)
user = new User(name, age, email) # GOOD
db_user = new User(name, age, email) # GOOD
```

Here, `user_data` can mean anything, it can refer to a Python dictionary or a Database (db) object. Our goal is to be more **specific 🎯**. According to SQLAlchemy convention, a simple variable named `user` should refer to a database object. To make it even more specific, we can name it `db_user`.

Although variables and properties are usually **nouns**, sometimes we should include **adjectives** as well to further clarify the context 😇. This is mostly true for boolean values:

```python
# -- BAD --
@dataclass
class UserDetails:
    name: str
    login: bool
    premiumSubscription: bool

# -- GOOD --
@dataclass
class UserDetails:
    name: str
    isLoggedIn: bool
    hasPremiumSubscription: bool
```

In this code, we're storing the login state of the user and if the user is subscribed to a premium package.

### Functions and Methods

The same principles apply to functions and methods, except you should incorporate **verbs** as well. For example, `login()`, `createUser()` , `database.insert()` are some good names.

Don't use names like `user()`, `email()` as they sound like properties ❌. Prefer `getUser()`, `getEmail()` instead✅. Remember to be more specific. If you're creating a user, use `createUser(` instead of `create()` .

Avoid using generic names. `processTransaction()` or `processUserRecord()` ✅ is far better than `processData()` ❌

Make sure that you're consistent with naming patterns. If you use `fetchUser` in your code, stick with the `fetch*` prefix throughout the code. Don't mix `fetchUser` and `getProducts` as they can create confusion. Use `fetchUser` and `fetchProducts` to be consistent.

### Classes

Class names, by convention, should always be **nouns**. If one of your classes handle creating new user, then `UserFactory` (noun) is better than `CreateUser` (verb).

```python
# User creation class
class CreateUser: # BAD
class BuildUser: # BAD
class UserFactory: # GOOD
class User: # OKAY, not bad, not good (arguable)

# Process website transactions
class Transaction: # BAD, we're not creating transaction, we're processing it
class HandleTransaction: # BAD, verb
class ProcessTransaction: # BAD, verb
class TransactionHandler: # GOOD
class TransactionProcessor: # GOOD
```

### **Casing**

| Name | Example | Used in |
| --- | --- | --- |
| Camel Case | fetchProduct, getUser | JavaScript |
| Snake Case | fetch\_product, get\_user | Python |
| Pascal Case | FetchProduct, GetUser | C# |

These are the most common casing conventions. We should respect the cases while writing code for community acceptance. Consult the community guidelines for the language you use.

If you have been attentive for the past few minutes, you might have noticed that I violated this rule ⛔ somewhere above the code examples. Find it out! 🔎

Enough of naming, let's move forward to the next one

## 2\. Code Formatting 💻

A beginner's mistake 🧑‍🦱. They know about formatting practices but are not willing enough to follow them. There's nothing fancy here, so I am linking a good article which explains formatting: [Clean Code - Formatting](https://hackmd.io/@jenc/H1bgodhlo). If someone is interested, have a look!

## 3\. Writing better functions 🔨

### Keep it short

Functions should be relatively short. I'm going to keep this simple, so not giving any specific line count. But, functions should be easy to read and easy to understand. Consider the following example in Python, which handles uploading images to a file server.

```python
def upload_image(image_data):
    image_fields = image_data.subdict(['title', 'owner_id'])
    image = Image(image_fields)
    db.session.add(image)
    db.session.commit()

    version_fields = image_data.subdict(['title', 'mime_type', 'blob'])
    image_version = ImageVersion(version_fields)
    db.session.add(image_version)
    db.session.commit()

    tags = generate_tags(image_version)
    image_version.update_tags(tags)

    webhook_url = "https://example.com/webhook"
    payload = {
        "asset_id": image.id,
        "version_id": image_version.id,
        "tags": tags
    }
    response = requests.post(webhook_url, json=payload)

    if response.status_code != 200:
        raise WebhookError('Notify failed')
```

If you look into it for a good amount of time 😖, the code is not difficult to understand at all. But our goal is to reduce the thinking time and avoid unnecessary loads on our brains 🧠. So, here's what we're gonna do, we're going to refactor the above code 💡:

```python
# After refactor
def upload_image(image_data):
    image = create_asset(image_data, Image)
    image_version = create_version(image_data)

    try:
        notify_webhook(image, image_version)
    except WebhookError:
        logger.warn('Failed to notify')

def create_asset(asset_fields, model):
    fields = asset_fields.subdict(['title', 'owner_id'])
    asset = model(fields)
    db.session.add(asset)
    db.session.commit()
    return asset

def create_version(version_data):
    fields = version_data.subdict(['title', 'mime_type', 'blob'])
    version = Version(fields)
    db.session.add(version)
    db.session.commit()

    tags = generate_tags(version)
    version.update_tags(tags)

    return version

def generate_tags():
    pass

def notify_webhook(asset, version):
    webhook_url = "https://example.com/webhook"
    payload = {
        "asset_id": asset.id,
        "version_id": version.id,
        "tags": version.tags
    }
    response = requests.post(webhook_url, json=payload)

    if response.status_code != 200:
        raise WebhookError('Notify failed')
```

We basically **extracted** the concepts into different functions and tried to make them generic. After refactoring, just look into the `upload_image` method. It's so concise and easy to understand 🥳. Yes, the line of code might have increased a bit. But now, not only the code is **easy to understand**, but many functions are **reusable** as well 🐼. If we add a new method `upload_video`, we should be able to reuse and keep the flow simple. Furthermore, another engineer can look into the code and choose to delve deeper only if required, previously it was not possible, he had to read the full code to get the gist. There are more complex forms of refactoring, but those are out of scope for this article.

### Make it read like an instruction manual

There's another characteristic of the refactored code. Considering `upload_image` as the entry point, the code reads like a **step-by-step instruction manual**. That's how the functions are ordered one after another. We can use the `TO <describe>` pattern to read the code and evaluate ourselves.

> TO `upload_image`, we need to call `create_asset` , `create_version` and `notify_webhook` (sequentially)
> 
> TO `create_version`, we need to call `generate_tags`
> 
> DONE

If you look into the function ordering, you can see the functions come one after another: `upload_image -> create_asset -> create_version -> generate_tags -> notify_webhook` Think of it as **depth-first-search** but for reading functions 📥

But the big question is

**<mark>When should you extract a group of code to a separate function?</mark>**

In the last example, we just extracted different concepts to different functions. So it was simple. But in many cases, it might get more complicated 🚩. The next article in this series will discuss this in detail. That's all for today! Get your feet wet and start practising clean coding today 🫡 Thank you! 🙏