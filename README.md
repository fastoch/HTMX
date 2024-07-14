# Build a Simple HTMX Project

Dave Gray teaches code  
https://www.youtube.com/watch?v=te_lYPEDycc  

Today we're going to look at how HTMX makes it easy to build a simple Todo CRUD application.  

>[!note]
>CRUD = create / read / update / delete

>[!warning]
>This tutorial is for people that already know HTML and a little bit of JavaScript.

---

## What is it?

HTMX is a dependency-free, browser-oriented javascript library.  
It's a library that allows you to access modern browser features directly from HTML, rather than using javascript.  

To install it: https://htmx.org/docs/#installing  

>[!important]
>HTMX does require a server (a backend). If you're not familiar with backend development, checkout Dave Gray's Node.js course on youtube. 

---

## Code sample

After having downloaded a copy of `htmx.min.js` from [unpkg.com](https://unpkg.com/htmx.org@2.0.1/dist/htmx.min.js), add it to the appropriate directory in your project and include it where necessary with a <script> tag:

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
  <form hx-post="" hx-target="#todo-list">

  </form>
  <ol> </ol>
</body>
```

@3/12
