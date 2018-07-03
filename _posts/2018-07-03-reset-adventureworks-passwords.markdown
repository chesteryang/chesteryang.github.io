---
layout: post
title:  "Reset adventureworks person passwords"
date:   2018-07-03 13:14:21 -0600
categories: C#
---

One way to reset all passwords of Person.Password table of AdventureWorks database.
===================================================================================

In case you need to use Person.Password table to demo login, the password is the email address in the record of Person.EmailAddress table. I failed to find how the hashed passwords were generated. As this is for demo purpose, we can just reset it using our alogrithm.

```C#
    using System;
    using System.Linq;
    using System.Security.Cryptography;
    using System.Text;

    namespace CoreAngular.AdventureWorks
    {
        public class SecurityService
        {
            public static string GenerateHashedPassword(string salt, string password)
            {
                var sBytes = Convert.FromBase64String(salt);
                var pBytes = Encoding.ASCII.GetBytes(password);
                var sVal = sBytes.Concat(pBytes).ToArray();
                sVal = SHA256.Create().ComputeHash(sVal);
                return Convert.ToBase64String(sVal);
            }
        }
    }
```

And then we can write a test to reset it

```C#
   [Fact]
    public void CanResetPasswords()
    {
        var emails = _context.EmailAddress.ToList();
        foreach (var email in emails)
        {
            var password = _context.Password.Single(p => p.BusinessEntityId == email.BusinessEntityId);
            if(password == null) continue;
            var encryptedPassword = SecurityService
                .GenerateHashedPassword(password.PasswordSalt, email.EmailAddress1);
            if(encryptedPassword == password.PasswordHash) continue;
            password.PasswordHash = encryptedPassword;
            _context.SaveChanges();
        }
    }

```

As we update records one by one and it is quite slow but it works.