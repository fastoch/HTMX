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

*index.html*
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

*index.ts*
```ts
function getListItems(todos: typeof todoData.todos) {
  return todos.sort((a, b) => a.id - b.id).map(todo => (
    html`<li>
      <input
        type="checkbox"
        id="todo_${todo.id}"
        ${todo.completed ? 'checked' : ''}
        hx-put="http://localhost/todo:3000/${todo.id}"
        hx-trigger="click"
        hx-target="#todo-list"
      />
      <label for="todo_${todo.id}" >${todo.title}</label>
      <button
        hx-delete="http://localhost/todo:3000/${todo.id}"
        hx-trigger="click"
        hx-target="#todo-list"
      >❌</button>
    </li>`
  ))
}
```

### Code explanation

- `getListItems` is what we call a **helper** function.
- This helper function creates HTML list items
- `todos` is the parameter name
- `: typeof todoData.todos` is a TypeScript type query for the todos parameter.
  - It means the todos parameter should have the same type as `todoData.todos`.
  - `todoData` is likely an object or module that contains a `todos` property.
  - `todoData` probably contains the structure or initial data for the todo items.
  - By using `typeof todoData.todos`, the function can adapt to changes in the `todoData.todos` structure without needing to update the type annotation manually.
- The `<input>` element: As one of the todos are checked (click event), the `hx-put` attribute inside our input element will update the todo item (list item)
- The `<button>` allows us to delete a todo item
- The `hx-target` is always the todo list, where we want to see all of the todos listed out after any modification (created, updated, or deleted)

---

>[!tip]
>cross mark emoji copied from here: https://symbl.cc/en/274C/

---

We use the helper function `getListItems` inside each of the methods. For example:
```ts
app.put('/todo/:id'), async (c) => {
  html`${listItems}`
})

app.delete('/todo/:id'), async (c) => {
  const todoId = await c.req.param('id')
  todoData.deleteTodo(Number(todoId))

  const listItems = getListItems(todoData.todos)

  return c.html(
    html`${listItems}`
  )
})
```


--- 

### CRUD methods

We've covered all the methods now:
- CREATE = `hx-post`
- READ = `hx-get`
- UPDATE = `hx-put`
- DELETE = `hx-delete`

---

## Thoughts on HTMX

One of the key benefits of HTMX is to reduce the client-side complexity.  
By shifting much of the dynamic behavior to server-side logic, HTMX reduces the complexity on the client side, resulting in cleaner, more maintainable code.  

But the downside to that is it's fairly tied to your backend.



