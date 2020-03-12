# Setting up everything

In our example, we will create new books and store them in our database.

Discussion

    Which routes you need to create to display a form where a user can fill the info, and then get the info and create a new book in the database?
    Which method will you use on each of those routes?

Yes, we will need two routes, the first one for rendering the form where the user can fill all the info about a new book, and another one for getting the data about the book and add it to the database. So let’s do it!

First, create a new route book/add on our routes/index.js file that should render a book-add.hbs. Which method will we use?
```js
router.get('/book/add', (req, res, next) => {
  res.render("book-add");
});
```
And the second one? We need another to get the data and add to our Mongo database. We can use the same route but changing the method.
```js
router.post('/book/add', (req, res, next) => {
  
});
```
Awesome! Now we need to add a form on the book-add.hbs file, with all the fields we need to create a new book.
```html
<form action="/book/add" method="post">
    <label for="">Title:</label>
    <input type="text" name="title">

    <label for="">Author:</label>
    <input type="text" name="author">

    <label for="">Description:</label>
    <input type="text" name="description">

    <label for="">Rate:</label>
    <input type="number" name="rating">

    <button type="submit">ADD</button>
</form>
```