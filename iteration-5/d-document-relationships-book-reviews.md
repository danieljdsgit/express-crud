# Books Reviews

We practice relations using reference with the authors now let’s try to use the embedded strategies for reviews. Almost every application brings user reviews, and it’s a great feature to add to the projects.

The review will have two fields: user and comments. Since we are going to embed the reviews into the Book model, we don’t need to create a new one; we just need to add a new field to our Book model, so let’s do it:

```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const bookSchema = new Schema(
  {
    title: String,
    description: String,
    author: [{ type: Schema.Types.ObjectId, ref: 'Author' }],
    rating: Number,
    reviews: [
      {
        user: String,
        comments: String
      }
    ]
  },
  {
    timestamps: {
      createdAt: 'createdAt',
      updatedAt: 'updatedAt'
    }
  }
);

const Book = mongoose.model('Book', bookSchema);

module.exports = Book;
```

## Create Reviews

When we display the details of a book, on the book-detail.hbs file we should create a form to allow users to add reviews about that book. Let’s do it:

```js
/*--book-detail.hbs--*/

<p>Add a review</p>
<form action="/reviews/add?book_id={{book._id}}" method="post">
    <label for="">User:</label>
    <input type="text" name="user">

    <label for="">Comments:</label>
    <textarea type="text" name="comments"></textarea>

    <button type="submit">ADD</button>
</form>
```

And let’s create a POST method to the /reviews/add route, where we will get all the info from the req.body and update the reviews field of the book pushing the new review into the array.

```js
router.post('/reviews/add', (req, res, next) => {
  const { user, comments } = req.body;
  Book.update(
    { _id: req.query.book_id },
    { $push: { reviews: { user, comments } } }
  )
    .then(book => {
      res.redirect('/book/' + req.query.book_id);
    })
    .catch(error => {
      console.log(error);
    });
});
```

## Print the reviews

Finally, we have to print all the reviews. One of the advantages of embedding documents is that we don’t need to do another query before rendering the book-detail.hbs view. We are already passing the book data, so we just need to loop over the reviews array and print each of them.

```html
//...
<h2>Reviews</h2>
{{#each book.reviews}}
<div class="review-item">
  <p>{{this.comments}}</p>
  <span>{{this.user}}</span>
</div>
{{/each}}
```

And some css to style our application:

```css
.review-item {
  border-bottom: 1px solid grey;
  margin: 10px 0;
  padding-bottom: 10px;
}

.review-item p {
  margin-bottom: 0;
}

.review-item span {
  font-size: 10px;
  color: grey;
}
```

## Congratulations!
