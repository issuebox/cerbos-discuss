To check if a person can read an article, I have two ways:

## The first way
set all roles to the resourcePolicy; when checked, no need to query roles using the IdP system
### resource policy define
```yaml
  - actions: ['read']
    effect: EFFECT_ALLOW
    roles: ["member", "editor", "admin"]
```
In the program, I don't even need an IdP, just read the user role from `user` table of database; Then checked like following:

### program code
```ts

// get currentRole from the user table of database
const currentRole = 'admin';

await cerbos.isAllowed({
  principal: { roles: [currentRole], /** ... */ },
  resource: {
    // ...
  },
  action: "read",
}); 
```
The disadvantage of this approach is: I have to list all roles with read permission in resourcePolicy, this makes dynamic creating roles very tricky.


## The Second way
set the low required role to the resourcePolicy; when checked, have to query all roles using the IdP system

### resource policy define
```yaml
  - actions: ['read']
    effect: EFFECT_ALLOW
    roles: ["member"]
```
In the program, I don't even need an IdP, just read the user role from `user` table of database; Then checked like following:

### program code
```ts
// get currentRole from the user table of database
const currentRole = 'admin';

// get all roles of the user;
const roles = await rbac.getAllExtendRoles(currentRole); // ['admin', 'editor', 'member', 'guest']
await cerbos.isAllowed({
  principal: { roles, /** ... */ },
  resource: {
    // ...
  },
  action: "read",
}); 
```
The disadvantage of this approach is: I have to use another rbac permission management system.
