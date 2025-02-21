## Step 1: Project Setup

#### 1. Create a new project folder

`mkdir Project-Folder`

`cd Project-Folder`

#### 2. Initialize npm

`npm init -y`

#### 3. Install Express.js

`npm install express`

## Step 2: Setting Up the Server

#### 1. Create `index.js` file in the root directory

`touch index.js`

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

`nodemon index.js`

## Step 3: Setting Up Views & Public Folder

#### 1. Create folders for views and static files

`mkdir views public`

#### 2. Install EJS for templating

`npm install ejs`

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

Since we don’t have a database, we use an array to store posts.

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
