# List Documents

The first thing we will do is to list all the books in a new route. Let’s create a /books route inside our routes/index.js file, and render a books view when we reach it (you should also create the books.hbs file inside the views folder).

```js
// routes/index.js

router.get('/books', (req, res, next) => {
  res.render('books');
});
```

Our view is empty, and we are not passing any data to render. Let’s query our Book collection to bring all the books we have in our database. First, we need to check that our collection model is in the index.js file where we created the route.

```js
// routes/index.js

const express = require('express');
const router  = express.Router();

const Book = require('../models/book.js'); // <== this line
```

Now we are ready to start doing some Mongoose queries inside the route:

```js
// routes/index.js

router.get('/books', (req, res, next) => {
  Book.find()
    .then(allTheBooksFromDB => {
      console.log('Retrieved books from DB:', allTheBooksFromDB);
    })
    .catch(error => {
      console.log('Error while getting the books from the DB: ', error);
    })
  res.render('books');
});
```

If our query goes ok, we should get all the books we have in the database.

    How we can pass that info to our view?
    Where we should call the res.render() method?

We know we can add a second parameter to the res.render() method, where we can pass some data, so that’s easy. But, where should we put our res.render() method at the first place?
The Mongoose query is asynchronous and if we put the res.render() outside the promise part it will render before the data is retrieved, so we won’t be able to display the information in the view.

```js
// routes/index.js

router.get('/books', (req, res, next) => {
  Book.find()
    .then(allTheBooksFromDB => {
      // console.log('Retrieved books from DB:', allTheBooksFromDB);
      res.render('books', { books: allTheBooksFromDB });
    })
    .catch(error => {
      console.log('Error while getting the books from the DB: ', error);
    })
});
```

Finally, see that we already have some code in our books.hbs to display every book. Go to:

http://localhost:3000/books/

Awesome! We have our library displaying all the books we have!