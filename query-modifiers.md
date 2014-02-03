## Query Modifiers

Query modifiers allow you pass conditions (as JSON objects) directly into find methods. This is an alternative syntax to chaining calls together like this:

```
User.find.where(...).where(...).sort(...).exec(cb);
```

--something about why/when this is preferable.


### Limit, Skip and Sort
One of the simplest examples is using the "limit" query modifier.  This example allows you to limit your results to just three user objects.
```
User.find({limit: 3}, cb)
```

Next, let's say you want to ignore the first three results, and just get the rest.  In that case, you would use "skip" like this:
```
User.find({skip: 3}, cb)
```

These in combination could be used to build a pagination like so:
```
User.find({limit: 3, skip: 0}, cb) //this would grab the first page of results
User.find({limit: 3, skip: 3}, cb) //this would grab the second page of results
```

If you're building out a pagination system like that, you will probably also want to sort your results.  With query modifiers, you would do that like this:
```
User.find({sort: 'name ASC'}) // same as User.find_({sort: 'name'})
User.find({sort: 'name DESC'})
```

### Where

Next, you can use the "where" modifier to build complex queries.
```
// find the User with id 17
User.find({where: {id: '17'}}, cb)  

// find users with a name that is < 'evan' (eg. letters a-d, ea-evam, as well as capital letters and numbers)
User.find({where: {name: {'<': 'evan'}}}, cb)
User.find({where: {name: {'<=': 'evan'}}}, cb) // before or equal to evan

// find users whose names start with a letter after 'evan'
User.find({where: {name: {'>': 'evan'}}}, cb)
User.find({where: {name: {'>=': 'evan'}}}, cb) // after or equal to 'evan'


// find users with a name NOT equal to 'evan'.  Note that "EVAN" is not equal to "evan" in this context.
User.find({where: {name: {'!': 'evan'}}}, cb) 

// find users with a name similar to 'eVan' (this will match 'eVan', 'evan' 'EVAN', etc.)
User.find({where: {name: {'like': 'eVan'}}}, cb) 

// Find users where the name contains 'va'  (this will match 'eva' 'EvA' 'Van' etc.)
User.find({where: {name: {'contains': 'evan'}}}, cb) 

// Find users where the name starts with 'ev'  (this will match 'eva' 'EvA' 'EV' etc.)
User.find({where: {name: {'startsWith': 'ev'}}}, cb) 

// Find users where the name ends with 'an'  (this will match 'AN' 'vaaaan' 'EvAn' 'Van' etc.)
User.find({where: {name: {'endsWith': 'an'}}}, cb) 
```

### Or

Didn't work...
```
Multiple where clauses (can't get this to work...)
User.find({or: [name: {endsWith: 'an'}]}, function(err, users){console.log(users)})
```
