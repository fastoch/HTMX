# Build a Simple HTMX Project

Dave Gray teaches code  
https://www.youtube.com/watch?v=te_lYPEDycc  

Today we're going to look at how HTMX makes it easy to build a simple Todo CRUD application.  

>[!note]
>CRUD = create / read / update / delete

>[!warning]
>This tutorial is for people who already know HTML and a little bit of JavaScript.

---

## What is it? How to install it?

HTMX is a dependency-free, browser-oriented javascript library.  
It's a library that allows you to access modern browser features directly from HTML, rather than using javascript.  

To install it: https://htmx.org/docs/#installing  

>[!important]
>HTMX requires a server (a backend). If you're not familiar with backend development, checkout Dave Gray's **Node.js** course on youtube. 

---

## Reviewing the HTMX Web page (front-end)

After having downloaded a copy of `htmx.min.js` from [unpkg.com](https://unpkg.com/htmx.org@2.0.1/dist/htmx.min.js), add it to the appropriate  
directory in your project and include it where necessary with a `<script>` tag:

index.html
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HTMX Todo</title>
  <script src="/path/to/htmx.min.js"></script>
</head>

<body>
  <form hx-post="http://localhost:3000/todo" hx-target="#todo-list">
    <label for="newTodo">New Todo:</label>
    <input type="text" name="newTodo" id="newTodo" />
    <button type="submit">Submit</button>
  </form>
  <ol id="todo-list" hx-get="http://localhost:3000/todo" hx-trigger="load"></ol>
</body>

</html>
```

- The `hx-post`, `hg-target`, `hx-get `and `hx-trigger` attributes are unique to HTMX.  
- when using HTMX, you need your backend server to send .html, not.json data.
- the user submits a new todo via the front-end form
- and my backend server sends the corresponding list item to populate the ordered list
- when the page loads, the todo list is updated

---

What about the put method that we would use to update our todos with? And the delete method?  
>That needs to be in the list item elements themselves, and we can only see that by looking at the backend.

---

## Server delivering HTML to HTMX (back-end)

I have a simple Node.js server set up, and I'm using **Hono** for this.  
Hono quickly allows you to set up a backend server. https://hono.dev/  






@5/12
