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

<div>
  <h1>ToDo List</h1>
  <div>
    {status}
    <button on:click={deleteCompleted}>Delete Completed</button>
  </div>
  <form on:submit|preventDefault>
    <input
      size="30"
      placeholder="Enter a new todo here..."
      bind:value={todoText}
    />
    <button disabled={!todoText} on:click={addTodo}>Add</button>
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

<style>
  button {
    margin-left: 10px;
  }

  ul {
    list-style: none;
    margin-left: 0px;
    padding-left: 0px;
  }
</style>
