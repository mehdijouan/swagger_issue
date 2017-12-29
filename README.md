# swagger_issue

The repository is separated in two modules, the first one is with a valid json which cannot be built by swagger codegen and the second one contains a modified version of the json which is not valid but understood by the swagger codegen.

## Module valid_but_not_working

The module `valid_but_not_working` contains a json schema [card.json](https://github.com/mehdijouan/swagger_issue/blob/master/valid_but_not_working/src/main/resources/schemas/card.json) that refers to a second
[address.json](https://github.com/mehdijouan/swagger_issue/blob/master/valid_but_not_working/src/main/resources/schemas/address.json) schema.

The card schema is as following:

```json
{
   "$schema": "http://json-schema.org/draft-06/schema#",
   "description": "A representation of a person, company, organization, or place",
   "type": "object",
   "required": [
      "familyName",
      "givenName"
   ],
   "properties": {
      "familyName": {
         "type": "string"
      },
      "givenName": {
         "type": "string"
      },
      "nickname": {
         "type": "string"
      },
      "addresses": {
         "type": "array",
         "items": {
            "$ref": "address.json"
         }
      }
   }
}
```

The syntax of the schema has been verified using http://json-schema-validator.herokuapp.com/syntax.jsp but when running the swagger code generator, I get the error :
```
[ERROR] Failed to execute goal io.swagger:swagger-codegen-maven-plugin:2.3.0:generate (default) on project valid_but_not_working: Execution default of goal io.swagger:swagger-codegen-maven-plugin:2.3.0:generate failed: Could not find definitions/address.json in contents of ./schemas/card.json -> [Help 1]
```

## Module invalid_but_working

The module `invalid_but_working` contains a json schema [card.json](https://github.com/mehdijouan/swagger_issue/blob/master/invalid_but_working/src/main/resources/schemas/card.json) that refers to a second
[address.json](https://github.com/mehdijouan/swagger_issue/blob/master/invalid_but_working/src/main/resources/schemas/address.json) schema.

The card schema is as following:

```json
{
   "$schema": "http://json-schema.org/draft-06/schema#",
   "description": "A representation of a person, company, organization, or place",
   "type": "object",
   "required": [
      "familyName",
      "givenName"
   ],
   "properties": {
      "familyName": {
         "type": "string"
      },
      "givenName": {
         "type": "string"
      },
      "nickname": {
         "type": "string"
      },
      "addresses": {
         "type": "array",
         "items": {
            "$ref": "./address.json" <= here is the modification
         }
      }
   }
}
```

The syntax of the schema is in error using http://json-schema-validator.herokuapp.com/syntax.jsp :

```json
[ {
  "level" : "error",
  "message" : "URI \"./address.json\" is not normalized",
  "domain" : "syntax",
  "schema" : {
    "loadingURI" : "#",
    "pointer" : "/properties/addresses/items"
  },
  "keyword" : "$ref",
  "value" : "./address.json"
} ]
```

But swagger understands it correctly.