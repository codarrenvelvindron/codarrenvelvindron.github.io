---
published: false
---
## IDOR
![idor featured](https://github.com/codarrenvelvindron/codarrenvelvindron.github.io/raw/master/images/Black-Hat-Gamification.jpg)

In the security world, IDOR is kind of a big deal.


## What ?
**I**nsecure **D**irect **O**bject **R**eference.

As the name says, we are referencing an object directly.

The application will give access to an object without any authorization required (and this is the insecure part).

## Why ?
This happens if improper care is give to the different routes a user can take.

And usually this may be as simple as a parameter in an URL.

As an example, say you got a 25% discount code, accessible only through this URL:
```
https://superbuystuffsite.com/discount?promo=25
```

Now, say you were to change the promo value to 100.

And get 100% discount ! Item is free.

You've just discovered an IDOR.

## How ?
Normally, IDORs can be prevented or at least made harder by using secure hashes instead of plaintext.

Also, these hashes should be valid only for a limited amount of time.

Yep, that's all.

## Credits

[idor portswigger](https://portswigger.net/web-security/access-control/idor)

[thehackerish.com](https://thehackerish.com/idor-tutorial-hands-on-owasp-top-10-training/)

[Featured black hat](https://yukaichou.com/gamification-study/black-hat-gamification-design/)
## \Codarren/
