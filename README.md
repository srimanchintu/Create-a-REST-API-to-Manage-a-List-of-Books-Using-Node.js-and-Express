# bash 
mkdir book-api
cd book-api
npm init -y
npm install express

# project structure
book-api/
│
├── index.js        # Main API file
└── (Optional: books.json for persistence later) 

# index.js 
const express = require('express');
const app = express();
const port = 3000;

// Middleware
app.use(express.json());

// In-memory book list
let books = [
  { id: 1, title: '1984', author: 'George Orwell' },
  { id: 2, title: 'To Kill a Mockingbird', author: 'Harper Lee' }
];

// GET all books
app.get('/books', (req, res) => {
  res.json(books);
});

// GET a book by ID
app.get('/books/:id', (req, res) => {
  const book = books.find(b => b.id === parseInt(req.params.id));
  if (!book) return res.status(404).send('Book not found');
  res.json(book);
});

// POST a new book
app.post('/books', (req, res) => {
  const { title, author } = req.body;
  const newBook = {
    id: books.length + 1,
    title,
    author
  };
  books.push(newBook);
  res.status(201).json(newBook);
});

// PUT (update) a book
app.put('/books/:id', (req, res) => {
  const book = books.find(b => b.id === parseInt(req.params.id));
  if (!book) return res.status(404).send('Book not found');

  const { title, author } = req.body;
  book.title = title || book.title;
  book.author = author || book.author;
  res.json(book);
});

// DELETE a book
app.delete('/books/:id', (req, res) => {
  books = books.filter(b => b.id !== parseInt(req.params.id));
  res.status(204).send();
});

// Start server
app.listen(port, () => {
  console.log(`Book API running at http://localhost:${port}`);
});

# Test Your API with Tools Like:
Postman

curl

REST Client (VSCode)


# 1. Initialize project with npm 
npm init -y

# 2.Install Express 
npm install express

# 3.Setup basic Express server (index.js)

const express = require('express');
const app = express();
const PORT = 3000;

// Middleware to parse JSON bodies
app.use(express.json());

app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});

# 4.Create an array to store books 
let books = [
  { id: 1, title: "The Great Gatsby", author: "F. Scott Fitzgerald" },
  { id: 2, title: "1984", author: "George Orwell" }
];

# 5.Implement GET /books — return all books 

app.get('/books', (req, res) => {
  res.json(books);
});
# 6. Implement POST /books — add new book from request body 

app.post('/books', (req, res) => {
  const { title, author } = req.body;
  if (!title || !author) {
    return res.status(400).json({ error: "Title and author are required" });
  }

  const newBook = {
    id: books.length ? books[books.length - 1].id + 1 : 1,
    title,
    author
  };

  books.push(newBook);
  res.status(201).json(newBook);
});

# 7. Implement PUT /books/:id — update a book by ID 
app.put('/books/:id', (req, res) => {
  const bookId = parseInt(req.params.id);
  const { title, author } = req.body;

  const book = books.find(b => b.id === bookId);
  if (!book) return res.status(404).json({ error: "Book not found" });

  if (title) book.title = title;
  if (author) book.author = author;

  res.json(book);
});

# 8. Implement DELETE /books/:id — remove a book 

app.delete('/books/:id', (req, res) => {
  const bookId = parseInt(req.params.id);
  const initialLength = books.length;
  books = books.filter(b => b.id !== bookId);

  if (books.length === initialLength) {
    return res.status(404).json({ error: "Book not found" });
  }

  res.status(204).send();
});

# Final index.js (Complete)

const express = require('express');
const app = express();
const PORT = 3000;

app.use(express.json());

let books = [
  { id: 1, title: "The Great Gatsby", author: "F. Scott Fitzgerald" },
  { id: 2, title: "1984", author: "George Orwell" }
];

app.get('/books', (req, res) => {
  res.json(books);
});

app.post('/books', (req, res) => {
  const { title, author } = req.body;
  if (!title || !author) {
    return res.status(400).json({ error: "Title and author are required" });
  }

  const newBook = {
    id: books.length ? books[books.length - 1].id + 1 : 1,
    title,
    author
  };

  books.push(newBook);
  res.status(201).json(newBook);
});

app.put('/books/:id', (req, res) => {
  const bookId = parseInt(req.params.id);
  const { title, author } = req.body;

  const book = books.find(b => b.id === bookId);
  if (!book) return res.status(404).json({ error: "Book not found" });

  if (title) book.title = title;
  if (author) book.author = author;

  res.json(book);
});

app.delete('/books/:id', (req, res) => {
  const bookId = parseInt(req.params.id);
  const initialLength = books.length;
  books = books.filter(b => b.id !== bookId);

  if (books.length === initialLength) {
    return res.status(404).json({ error: "Book not found" });
  }

  res.status(204).send();
});

app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});

# 9. Test your API endpoints with Postman or similar tool 
Method	URL	Body (JSON) example	Description
GET	http://localhost:3000/books	N/A	Get all books
POST	http://localhost:3000/books	{ "title": "New Book", "author": "Author" }	Add a new book
PUT	http://localhost:3000/books/1	{ "title": "Updated Title" }	Update book with id=1
DELETE	http://localhost:3000/books/1	N/A	Delete book with id=1

# OUTPUT 

# 1.GET /books
Request:
GET http://localhost:3000/books

Response:
[
  {
    "id": 1,
    "title": "The Great Gatsby",
    "author": "F. Scott Fitzgerald"
  },
  {
    "id": 2,
    "title": "1984",
    "author": "George Orwell"
  }
]
# 2. POST /books 
Request:
POST http://localhost:3000/books
Body (JSON): 
{
  "title": "Brave New World",
  "author": "Aldous Huxley"
}
Response:
Status: 201 Created 
{
  "id": 3,
  "title": "Brave New World",
  "author": "Aldous Huxley"
}
# 3.. PUT /books/3

Request:
PUT http://localhost:3000/books/3
Body (JSON): 
{
  "title": "Brave New World Revisited"
}

response :
{
  "id": 3,
  "title": "Brave New World Revisited",
  "author": "Aldous Huxley"
}

# 4.DELETE /books/3
Request:
DELETE http://localhost:3000/books/3

Response:
Status: 204 No Content (no response body)

# 5.GET /books after deletion 
[
  {
    "id": 1,
    "title": "The Great Gatsby",
    "author": "F. Scott Fitzgerald"
  },
  {
    "id": 2,
    "title": "1984",
    "author": "George Orwell"
  }
]

# i have done this task 3 by using Node. js and express 
