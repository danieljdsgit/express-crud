# Create Documents

All setup! We are ready for creating new documents and store them in our database.
Get the data from a POST Request & Creating the Document

How can we get the data in the /books/add route?

All the data will be included on the body property of the request object, so we can handle it to create a new book in the database, but let’s do it step by step:

You need to include all the following inside our books/add route with the POST method!

First, we need to get the data and store in new variables. ES6 destructuring will help with that:

```js
const { title, author, description, rating } = req.body;
```

Remember this is possible only if the variables have the same name on the req.body that means they need to have the same name on the name attribute of each input.

We can create a new Book using the model we import. It’s important to notice we are using some ES6 features that make our code looks much cleaner!

```js
const newBook = new Book({ title, author, description, rating});
```

Add the model to routes/index.js

```js
const Book = require('../models/book');
```

We can store it in the database, using the save() method. Since this is an asynchronous process, we need to process it as a Promise. If everything goes ok, we will receive the book we just save. Otherwise, we will have an error. We need to control both options!

```js
newBook.save()
.then((book) => {

})
.catch((error) => {

})
```

Finally, we can redirect our user to the /books view, where we list all the book we have in the database. The new book should be already there. We should have the following code on our route:

```js
router.post('/book/add', (req, res, next) => {
  const { title, author, description, rating } = req.body;
  const newBook = new Book({ title, author, description, rating})
  newBook.save()
  .then((book) => {
    res.redirect('/books');
  })
  .catch((error) => {
    console.log(error);
  })
});
```

Go to http://localhost:3000/book/add and create a book.

Awesome! Now we should see the new book we just created!

* /books does not exist yet, so don't worry if you see a 404 after the redirect.

Creating documents is a super standard action on web applications, and you need to be sure you understand step by step all the process you will need to do it, so if anything is not clear enough, now it is the perfect time to ask! 