## Step 1: Project Setup

#### 1. Create a new project folder

`$ mkdir Project-Folder`

`$ cd Project-Folder`

#### 2. Initialize npm

`$ npm init -y`

#### 3. Install Express.js

`$ npm install express`

## Step 2: Setting Up the Server

#### 1. Create `index.js` file in the root directory

`$ touch index.js`

#### 2. Write a basic Express server in `index.js`

```
const express = require("express");
const app = express();
const port = 8080;

app.listen(port, () => {
  console.log(`Listening on port ${port}`);
});
```

#### 3. Start the server

`$ nodemon index.js`

## Step 3: Setting Up Views & Public Folder

#### 1. Create folders for views and static files

`$ mkdir views public`

#### 2. Install EJS for templating

`$ npm install ejs`

#### 3. Configure EJS in `index.js`

```
const path = require("path");

app.set("view engine", "ejs");
app.set("views", path.join(__dirname, "views"));

app.use(express.static(path.join(__dirname, "public")));
```

## Step 4: Middleware Setup

To handle form submissions Express has built-in support, so update `index.js`:

```
app.use(express.urlencoded({ extended: true }));
```

## Step 5: Creating the Posts Data Structure

Since we don't have a database, we use an array to store posts.

Modify `index.js`:

```
let posts = [
  { username: "apnacollege", content: "I love coding!" },
  { username: "shradhakhapra", content: "Hard work is important!" },
  { username: "dibyajyoti", content: "I got selected for my 1st internship!" },
];
```

## Step 6: Display All Posts on a Webpage

#### 1. Create `views/index.ejs`

```
<title>All Posts</title>
<h1>Quora Posts</h1>
<% for (const post of posts) { %>
  <div class="post">
    <h3>@<%= post.username %></h3>
    <p><%= post.content %></p>
  </div>
<% } %>
```

#### 2. Update `index.js` to serve the page

```
app.get("/posts", (req, res) => {
  res.render("index.ejs", { posts });
});
```

## Step 7: Adding CSS Styling

#### 1. Create `public/styles.css`

```
h1 {
  color: maroon;
}
.post {
  background-color: blanchedalmond;
  padding: 10px;
  margin: 10px 0;
  border-radius: 5px;
}
```

#### 2. Link CSS in `index.ejs`

```
<link rel="stylesheet" href="/styles.css" />
```

## Step 8: Adding a Form to Create a New Post

#### 1. Create `views/new.ejs`

```
<title>Create a New Post</title>
<h1>Create a New Post</h1>
<form action="/posts" method="post">
  <input type="text" name="username" placeholder="Enter username" required />
  <br />
  <textarea name="content" placeholder="Write your post" required></textarea>
  <br />
  <button type="submit">Submit</button>
</form>
```

#### 2. Serve the form with a new route in `index.js`

```
app.get("/posts/new", (req, res) => {
  res.render("new.ejs");
});
```

#### 3. Adding a Button to Create a New Post

```
<a href="http://localhost:8080/posts/new">Create New Post</a>
```

## Step 9: Handling Form Submission (POST Request)

#### 1. Update `index.js` to handle form submission

```
app.post("/posts", (req, res) => {
  let { username, content } = req.body;
  posts.push({ username, content });
  res.redirect("/posts");
});
```

## Step 10: Adding Unique IDs Using UUID

#### 1. Install UUID

`$ npm install uuid`

#### 2. Update `index.js`

```
const { v4: uuidv4 } = require("uuid");

let posts = [
  {
    id: uuidv4(),
    username: "apnacollege",
    content: "I love coding!",
  },
]

app.post("/posts", (req, res) => {
  let { username, content } = req.body;
  let id = uuidv4();
  posts.push({ id, username, content });
  res.redirect("/posts");
});
```

## Step 11: Viewing Individual Posts

#### 1. Create `views/show.ejs`

```
<title>Post in Detail</title>
<h2>Here is your post in detail</h2>
<p>Post Id: <%= post.id %></p>
<div class="post">
  <h3>@<%= post.username %></h3>
  <p><%= post.content %></p>
</div>
```

#### 2. Add route in `index.js`

```
app.get("/posts/:id", (req, res) => {
  let { id } = req.params;
  let post = posts.find(p => p.id === id);
  res.render("show.ejs", { post });
});
```

#### 3. Add a "Show Post" button in `index.ejs` to go to `show.ejs`

```
<a href="/posts/<%= post.id %>">Show Post</a>
```

## Step 12: Adding Edit Feature (PATCH Request)

#### 1. Install method-override

`$ npm install method-override`

#### 2. Configure in `index.js`

```
const methodOverride = require("method-override");
app.use(methodOverride("_method"));
```

#### 3. Create `views/edit.ejs`

```
<title>Edit Post</title>
<form method="post" action="/posts/<%= post.id %>?_method=PATCH">
  <textarea name="content"><%= post.content %></textarea>
  <button type="submit">Update</button>
</form>
```

#### 4. Add edit route in `index.js`

```
app.get("/posts/:id/edit", (req, res) => {
  let { id } = req.params;
  let post = posts.find(p => p.id === id);
  res.render("edit.ejs", { post });
});

app.patch("/posts/:id", (req, res) => {
  let { id } = req.params;
  let newContent = req.body.content;
  let post = posts.find(p => p.id === id);
  post.content = newContent;
  res.redirect("/posts");
});
```

#### 5. Add an Edit button in `index.ejs` to go to `edit.ejs`

```
<a href="/posts/<%= post.id %>/edit">Edit</a>
```

## Step 13: Adding Delete Feature

#### 1. Add delete button in `index.ejs`

```
<form method="post" action="/posts/<%= post.id %>?_method=DELETE">
  <button>Delete</button>
</form>
```

#### 2. Add delete route in `index.js`

```
app.delete("/posts/:id", (req, res) => {
  let { id } = req.params;
  posts = posts.filter(p => p.id !== id);
  res.redirect("/posts");
});
```
