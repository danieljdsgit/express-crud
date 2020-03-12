# Introduction

We have seen how we can list, read, create and update documents while using Express with Mongoose, so we are ready to make great web applications. In the following learning unit, we are going to learn how we can integrate a crucial concept we learned from MongoDB, documents relationships.

To move faster, let’s keep using our library-project.
The Author

Having only the name and lastName of the author on the Book model is really few information so that we will create an Author model separate from the Book one. In the Author model we will add the name, lastName, nationality, birthday and a pictureUrl, this way we can display the full info about the author of the book.

Is also true that a book could have more than one author, so the author field of the Book model will be an array of Author ids.

Why we decided to reference the authors instead of embedded the object them into the Book model?

Imagine we want to update the pictureUrl of a specific author? If we have them embedded in the Book model, we need to search through every book where this author shows up and update the field. But if we are referencing them, we just need to update the author once.

## Author Model

Let’s start creating our Author model. Inside our models folder add a author.js file and copy/paste the following code:
```js
const mongoose = require("mongoose");
const Schema   = mongoose.Schema;

const authorSchema = new Schema({
  name: String,
  lastName: String,
  nationality: String,
  birthday: Date,
  pictureUrl: String
});

const Author = mongoose.model("Author", authorSchema);

module.exports = Author;
```
We also need to update our Book model, replacing the type of data we are assigning to the author field. Instead of String, now we will have an Array of ObjectID pointing to the User model. So we will have the following:
```js
const mongoose = require("mongoose");
const Schema   = mongoose.Schema;

const bookSchema = new Schema({
  title: String,
  description: String,
  author: [ { type : Schema.Types.ObjectId, ref: 'Author' } ],
  rating: Number
}, {
  timestamps: {
    createdAt: "createdAt",
    updatedAt: "updatedAt"
  }
});

const Book = mongoose.model("Book", bookSchema);

module.exports = Book;
```