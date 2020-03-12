# Edit Documents

Any user can add books to our library-project, but what about modifying one? Let’s add this feature to our project!

## Edit Form

First, we need an edit form, where the user will be able to modify the info of each book. Let’s create a book-edit.hbs view and add the following code:
```html
<form action="/book/edit" method="post">
    <label for="">Title:</label>
    <input type="text" name="title">

    <label for="">Author:</label>
    <input type="text" name="author">

    <label for="">Description:</label>
    <input type="text" name="description">

    <label for="">Rate:</label>
    <input type="number" name="rating">

    <button type="submit">EDIT</button>
</form>
```
Is this enough to edit a book? It is, but with the awfull user experience. Imagine you as a user clicking on an edit button and all the info about the element you are trying to edit is not there, so you have to fill all the fields again? That sucks right? So we need to set all the inputs value with prefilled values from the existing book.
How can we do that?

We need to create a route where we will render this view, but before we should pass the info about the book the user is trying to edit.

First, let’s add the edit link to each of our books on the /books route.

How can we pass the info about the book we are trying to edit?

    Route Params. We can set a route like the following: book/edit/:id where we will receive the id as a req.params.
    Query String. Another option is to set the route: book/edit and pass the data as a query string using the ?.

We choose the second option, but any of them is valid! :wink: Add the following code to our books.hbs file:
```html
<h1>BOOKS</h1>
{{#each books}}
    <p>
        <a href="book/{{this._id}}">{{this.title}}</a>
 
        <a href="/book/edit?book_id={{this._id}}" class="edit-button">EDIT</a>
    </p>
{{/each}}
```
Notice how we set the href attribute for editing the documents, this way the book_id property will be dynamic.

You can also add the following css to the style.css file, so we differentiate the edit button.
```css
.edit-button {
  margin-left:20px; 
  color: #fff;
  text-decoration: none;
  padding: 2px 4px;
  background-color: grey;
  border-radius: 6px;
}
```
## Get the data

We know where the user will go when clicking on the Edit button. We need to create the route to get that user request and render the view.
```js
router.get('/book/edit', (req, res, next) => {
  res.render("book-edit");
});
```
But before rendering the edit form, we should retrieve the data of the book from our database and pass that data to our view. How can we get the data of the book we are clicking? Remember we are passing the id through the query string.
```js
router.get('/book/edit', (req, res, next) => {
  Book.findOne({_id: req.query.book_id})
  .then((book) => {
    res.render("book-edit", {book});
  })
  .catch((error) => {
    console.log(error);
  })
});
```
We need the req.query object to get the id of the book and then query the database asking for all the info about it.

Notice we are using the findOne method, this way the database returns an object with the book we are looking for. If we use the find method, Mongoose retrieves an array of objects.
Prefilled fields

We are now rendering our edit form, but they are empty. We need to fill those input with the info we retrieve from the database. Add the value attribute, with the corresponding info about each field (in book-edit.hbs).
```html
<form action="/book/edit?book_id={{book._id}}" method="post">
    <label for="">Title:</label>
    <input type="text" name="title" value="{{book.title}}">

    <label for="">Author:</label>
    <input type="text" name="author" value="{{book.author}}">

    <label for="">Description:</label>
    <input type="text" name="description" value="{{book.description}}">

    <label for="">Rate:</label>
    <input type="number" name="rating" value="{{book.rating}}">

    <button type="submit">EDIT</button>
</form>
```
Perfect! Now every input is prefilled with the info of the book we are trying to edit. We also modify the action attribute of the form. When the user clicks on the EDIT button, the web will make a POST request to that URL. Let’s move forward!
Update the Document

Create the route with a POST method so we can get the info of the book. Inside the route, we should get all the info from the req.body, and the book id from the req.query and then use the update method to edit the book on our database.

Remember the syntax for the update method. The first parameter is the query to find the element we want to edit. On the second parameter, we specify the fields we want to update. Since we are getting all the fields from the req.body you can set all of them.
```js
Model.update({ query 
}, { $set : { key: value, key: value }})
.then()
.catch()
```
On the POST method of the route you should have the following:
```js
router.post('/book/edit', (req, res, next) => {
  const { title, author, description, rating } = req.body;
  Book.update({_id: req.query.book_id}, { $set: {title, author, 
description, rating }})
  .then((book) => {
    res.redirect('/books');
  })
  .catch((error) => {
    console.log(error);
  })
});
```
Get the updated Document

After updating the document, Mongoose returns us the old document from the database. And sometimes this can confuse us a bit because in some cases we want to pass to the view the updated document to print it. Fortunately, we can add a third parameter to the update method so Mongoose will return us the updated document. The third parameter should be an object we specify we want the new document: { new: true }. The full syntax, looks like this:
```js
Model.update({ query 
}, { $set : { key: value, key: value }}, { new: true })
.then()
.catch()
```