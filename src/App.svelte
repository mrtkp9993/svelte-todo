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

<div class="container is-fluid">
  <div class="columns is-centered is-vcentered is-mobile">
    <div class="column is-narrow" style="width: 70%">
      <h1 class="has-text-centered title">ToDo List</h1>
      <p class="has-text-centered">{status}</p>
      <div class="has-text-centered">
        <button
          class="button is-secondary is-text has-text-centered"
          on:click={deleteCompleted}>Delete Completed</button
        >
      </div>
      <form
        class="field has-addons"
        style="justify-content: center"
        on:submit|preventDefault
      >
        <div class="control">
          <input
            class="input"
            size="30"
            placeholder="Enter a new todo here..."
            bind:value={todoText}
          />
        </div>
        <div class="control">
          <button
            class="button is-primary"
            disabled={!todoText}
            on:click={addTodo}
            ><span class="icon is-small">
              <i class="fas fa-plus" />
            </span>
          </button>
        </div>
      </form>
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
  ul {
    list-style: none;
    margin-left: 0px;
    padding-left: 0px;
  }
</style>
