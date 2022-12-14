Use case sceneario:
 - 
You have to check that the following schema is following the given bellow format, no matter the format of the file.

_schema.json_
```json
{
    "login" : "alex",
    "email" : "alex.andr@gmail.com",
    "password" : "asdf1234",
    "friend_list" : [
        {
            "email" : "calin.radu@gmail.com"
        },
        {
            "email" : "alea.konj@gmail.com"
        }
    ]
}
```

You write a query into a file, like this:

_schema.edrs_
```
OBJECT 
    FIELD "login"
        IS STRING
        IS LENGTH > 3
        IS NOT TEST /[,.?$%]/
    FIELD "email"
        IS EMAIL 
        IS NOT NULL THROWS "Field must not be null, baka!"
    FIELD "password"
        IS STRING
        IS LENGTH > 8 THROWS "Too small senpai"
        IS LENGTH < 32
        HAS NUMBER, UPPERCASE, LOWERCASE 
        IS NOT TEST /[0-9]{9,}/
    FIELD "friend_list"
        IS ARRAY
        IS LENGTH < 5
        HAS OBJECT
            FIELD "email"
                IS EMAIL
```

Runs like this:

```bash
endorser 
```

## multiple fields use case

To make an "or" check on a field, for 2 possible validations, simply declare it twice!

_schema.yml_
```yml
customers:
- name: "John Doe"
  phoneNumber: "1234321"
- name: "John Lennon"
  phoneNumber: 2223334
```

_schema.edrs_
```
OBJECT
  FIELD "customers"
    IS ARRAY
    IS LENGTH > 0
    EVERY OBJECT
      FIELD "name"
        IS STRING
      FIELD "phoneNumber"
        IS STRING
        IS LENGTH > 5
      FIELD "phoneNumber"
        IS NUMBER
        IS BETWEEN 100_000 AND 999_999_999
```
