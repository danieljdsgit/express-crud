# Populating the Author

We have a problem now! Every time we click on a book to check the details, instead of the author info, we see the _id, because that is what we are storing in our database.

We should populate the author field on our /book/:id before sending the data to the view.

Population is the process of automatically replacing the specified paths in the document with document(s) from other collection(s). We may populate a single document, multiple documents, plain object, multiple plain objects, or all objects returned from a query.

We already see this, but just for refresh the brain! :wink:
```js
router.get('/book/:bookId', (req, res, next) => {
  let bookId = req.params.bookId;
  if (!/^[0-9a-fA-F]{24}$/.test(bookId)) { 
    return res.status(404).render('not-found');
  }
  Book.findOne({'_id': bookId})
    .populate('author')
    .then(book => {
      if (!book) {
          return res.status(404).render('not-found');
      }
      res.render("book-details", { book })
    })
    .catch(next)
});
```
Cool huh?! Now we have the full author array with all the info about each author.

We need to update our book-details.hbs:

```html
<h1>{{book.title}}</h1>
<span>Written by: 
{{#each book.author}}
{{name}} {{lastName}}
{{/each}}
</span>
<p>Summary: {{book.description}}</p>
<p>Rating: {{book.rating}}/10</p>
<a href="/books">Return</a>
```
