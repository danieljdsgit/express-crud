# Show detailed Document

Now we have displayed only the title of each book but we want the users of our app to be able to see all the other information. We can take them to the detailed view where they can see everything we have saved in our DB related to that specific book. Let’s create a view inside views folder views/book-details.hbs, where all these data will be displayed. For sure we won’t create a view per book, we know how to manipulate data so the view gets changed dynamically.

## Adding the routes

First, make sure all the titles on the books.hbs are hyperlinks. The link should include a field that we can use to query the database to find a specific book.

Which field can we use? Yes! It seems that the _id is the best one.

```html
// views/books.hbs

<h1>BOOKS</h1>
{{#each books}}
    <p>
        <a href="book/{{this._id}}">{{this.title}}</a>
 
        <a href="/book/edit?book_id={{this._id}}" class="edit-button">EDIT</a>
    </p>
{{/each}}
```
When we click on one of the titles, where we will navigate?

Exactly! We will navigate to a route like the following: http://localhost:3000/book/5a79d85fd642ff1f1e6a479e (yes, the final part will be different from book to book!)

So how we have to structure the route? Hm, it should be something like this: http://localhost:3000/book/:bookId.

Let’s add this route to our project and make it render a book-details view, for now.
```js
// routes/index.js

router.get('/book/:bookId', (req, res, next) => {
  res.render('book-details');
});
```
## Get the id

How we can get the id from the URL?

There are a different ways to get the id info, but the best one is using req.params.
```js
// routes/index.js

router.get('/book/:bookId', (req, res, next) => {
  console.log('The ID from the URL is: ', req.params.bookId);
  res.render('book-details');
});
```
:bulb: Is there any other way we can do this rather than using the route params?

Go ahead and click on different books, and you should see on the console the _id field of each book you clicked on.
Querying the DB

We have all we need to query our database, retrieve all the info about the clicked book and pass the data to our view.

Which Mongoose method should we use to query?

We have a couple of options: find(), findOne(), findById() are the most common ones. After having the information about the book we are looking for, we should pass that data to the view.
```js
// routes/index.js

router.get('/book/:bookId', (req, res, next) => {
  Book.findOne({'_id': req.params.bookId})
    .then(theBook => {
      res.render('book-details', { book: theBook });
    })
    .catch(error => {
      console.log('Error while retrieving book details: ', error);
    })
});
```
We use findOne(), so the database retrieves an object with the book. If we use the find() method, it will return an array with the objects that match the criteria, but in our case there’s only one object with the passed id so we will get the array with one element. Although we showed you how to use find() method, we will keep using findById() method since it’s the most self-explanatory.
```js
// routes/index.js

router.get('/book/:bookId', (req, res, next) => {
  Book.findById(req.params.bookId)
    .then(theBook => {
      res.render('book-details', { book: theBook });
    })
    .catch(error => {
      console.log('Error while retrieving book details: ', error);
    })
});
```
Let’s add some code to the book-details.hbs so we can display the information we’ve got from the DB:
```html
<h1>{{book.title}}</h1>
<span>Written by: {{book.author}}</span>
<p>Summary: {{book.description}}</p>
<p>Rating: {{book.rating}}/10</p>
<a href="/books">Return</a>
```
When clicking in any of the books, we should see it's details.

