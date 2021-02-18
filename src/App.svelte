<!-- This code is a mix of
https://github.com/sveltejs/svelte-todomvc and
https://github.com/mvolkmann/svelte-todo
-->
<script>
  import Todo from "./Todo.svelte";
  import { v4 as uuidv4 } from "uuid";

  const createTodo = (text, done = false) => ({ id: uuidv4(), text, done });

  let todoText = "";
  let todos;

  try {
    todos = JSON.parse(localStorage.getItem("todos-svelte")) || [];
  } catch (err) {
    todos = [];
  }

  $: uncompletedCount = todos.filter((t) => !t.done).length;
  $: status = `${uncompletedCount} of ${todos.length} remaining.`;

  function addTodo() {
    todos = todos.concat(createTodo(todoText));
    todoText = "";
  }

  function deleteCompleted() {
    todos = todos.filter((t) => !t.done);
  }

  function deleteTodo(todoId) {
    todos = todos.filter((t) => t.id !== todoId);
  }

  function toggleDone(todo) {
    const { id } = todo;
    todos = todos.map((t) => (t.id === id ? { ...t, done: !t.done } : t));
  }

  $: try {
    localStorage.setItem("todos-svelte", JSON.stringify(todos));
  } catch (err) {}
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://cdn.jsdelivr.net/npm/bulma@0.9.1/css/bulma.min.css"
  />
  <script src="https://use.fontawesome.com/releases/v5.3.1/js/all.js"></script>
</svelte:head>

<div class="container">
  <div class="panel">
    <p class="panel-heading">ToDo List</p>
    <div class="notification">
      <p>{status}</p>
      <button
        class="button is-link"
        style="margin-left: 0px"
        on:click={deleteCompleted}>Delete Completed</button
      >
    </div>
    <div class="panel-block">
      <form on:submit|preventDefault>
        <p class="control">
          <input
            class="input"
            size="30"
            placeholder="Enter a new todo here..."
            bind:value={todoText}
          />
        </p>
        <div class="control">
          <button class="button is-link" disabled={!todoText} on:click={addTodo}
            >Add</button
          >
        </div>
      </form>
    </div>
    <div class="div">
      <ul>
        {#each todos as todo}
          <Todo
            {todo}
            on:delete={() => deleteTodo(todo.id)}
            on:toggleDone={() => toggleDone(todo)}
          />
        {/each}
      </ul>
    </div>
  </div>
</div>

<style>
  .button {
    margin-top: 10px;
  }

  ul {
    list-style: none;
    margin-left: 0px;
    padding-left: 0px;
  }
</style>
