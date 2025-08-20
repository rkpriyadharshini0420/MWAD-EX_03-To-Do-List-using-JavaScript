# MWAD-EX_03-To-Do-List-using-JavaScript
## Date:

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM

# Index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>To-Do App</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <main class="todo-container">
    <h2>To-Do List ✅</h2>

    <form id="todoForm" class="todo-input" autocomplete="off">
      <input type="text" id="taskInput" placeholder="Add a new task..." />
      <button type="submit">Add</button>
    </form>

    <ul id="taskList" class="todo-list"></ul>

    <div class="toolbar">
      <div class="filters">
        <button data-filter="all" class="active">All</button>
        <button data-filter="active">Active</button>
        <button data-filter="completed">Completed</button>
      </div>
      <div class="actions">
        <span id="itemsLeft">0 items left</span>
        <button id="clearCompleted">Clear Completed</button>
      </div>
    </div>
  </main>

  <script>
    let tasks = JSON.parse(localStorage.getItem("tasks") || "[]");
    let filter = "all";

    const els = {
      form: document.getElementById("todoForm"),
      input: document.getElementById("taskInput"),
      list: document.getElementById("taskList"),
      filters: document.querySelector(".filters"),
      clearCompleted: document.getElementById("clearCompleted"),
      itemsLeft: document.getElementById("itemsLeft"),
    };

    function save() { localStorage.setItem("tasks", JSON.stringify(tasks)); }
    function itemsLeftCount() { return tasks.filter(t => !t.completed).length; }

    function render() {
      els.list.innerHTML = "";
      const filtered = tasks.filter(t =>
        filter === "active" ? !t.completed :
        filter === "completed" ? t.completed : true
      );
      for (const task of filtered) {
        const li = document.createElement("li");
        li.className = "todo-item";
        li.dataset.id = task.id;

        const checkbox = document.createElement("input");
        checkbox.type = "checkbox";
        checkbox.checked = task.completed;
        checkbox.addEventListener("change", () => toggleTask(task.id));

        const span = document.createElement("span");
        span.className = "todo-text" + (task.completed ? " completed" : "");
        span.textContent = task.text;
        span.addEventListener("dblclick", () => startEdit(task.id, span));

        const actions = document.createElement("div");
        actions.className = "item-actions";
        const editBtn = document.createElement("button");
        editBtn.textContent = "✏️";
        editBtn.onclick = () => startEdit(task.id, span);
        const delBtn = document.createElement("button");
        delBtn.textContent = "🗑️";
        delBtn.onclick = () => removeTask(task.id);
        actions.append(editBtn, delBtn);

        li.append(checkbox, span, actions);
        els.list.append(li);
      }
      els.itemsLeft.textContent = `${itemsLeftCount()} item${itemsLeftCount() !== 1 ? "s" : ""} left`;
      document.querySelectorAll(".filters button").forEach(b => {
        b.classList.toggle("active", b.dataset.filter === filter);
      });
    }

    function addTask(text) {
      const trimmed = text.trim();
      if (!trimmed) return;
      tasks.push({ id: Date.now().toString(), text: trimmed, completed: false });
      save(); render();
    }
    function removeTask(id) { tasks = tasks.filter(t => t.id !== id); save(); render(); }
    function toggleTask(id) { const t = tasks.find(t => t.id === id); t.completed = !t.completed; save(); render(); }
    function updateTask(id, newText) { const t = tasks.find(t => t.id === id); if (newText.trim()) { t.text = newText.trim(); save(); render(); } }

    function startEdit(id, spanEl) {
      const input = document.createElement("input");
      input.type = "text";
      input.value = spanEl.textContent;
      spanEl.replaceWith(input);
      input.focus();
      input.addEventListener("blur", () => updateTask(id, input.value));
      input.addEventListener("keydown", e => { if (e.key === "Enter") updateTask(id, input.value); });
    }

    els.form.addEventListener("submit", e => { e.preventDefault(); addTask(els.input.value); els.input.value = ""; });
    els.filters.addEventListener("click", e => { if (e.target.dataset.filter) { filter = e.target.dataset.filter; render(); } });
    els.clearCompleted.addEventListener("click", () => { tasks = tasks.filter(t => !t.completed); save(); render(); });

    render();
  </script>
</body>
</html>
```
# Style.css
```
* { box-sizing: border-box; }
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f4f4f9;
  display: flex;
  justify-content: center;
  padding: 40px;
}
.todo-container {
  width: 400px;
  background: #fff;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}
h2 { text-align: center; margin: 0 0 16px; }
.todo-input { display: flex; gap: 10px; }
.todo-input input { flex: 1; padding: 10px; border: 1px solid #ccc; border-radius: 6px; }
.todo-input button { padding: 10px; border: none; background: #4caf50; color: white; border-radius: 6px; cursor: pointer; }
.todo-list { list-style: none; padding: 0; margin-top: 20px; }
.todo-item { display: flex; justify-content: space-between; align-items: center; padding: 8px; border-bottom: 1px solid #eee; }
.todo-text.completed { text-decoration: line-through; color: gray; }
.item-actions button { margin-left: 5px; cursor: pointer; }
.filters { display: flex; gap: 8px; margin-top: 12px; }
.filters button { border: none; background: #eee; padding: 5px 10px; border-radius: 5px; cursor: pointer; }
.filters .active { background: #4caf50; color: white; }
.toolbar { display: flex; justify-content: space-between; align-items: center; margin-top: 15px; font-size: 14px; }
.actions { display: flex; gap: 10px; align-items: center; }
```

## OUTPUT

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/9b933244-cc20-4257-b580-6135dd1aae4f" />


## RESULT
The program for creating To-do list using JavaScript is executed successfully.
