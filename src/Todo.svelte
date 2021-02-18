<script xmlns="http://www.w3.org/1999/html">
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  export let todo;
</script>

<div class="card">
  <div class="card-content">
    <span class={"done-" + todo.done}>{todo.text}</span>
  </div>
  <footer class="card-footer">
    <div class="card-footer-item">
      <label class="checkbox"
        ><input
          type="checkbox"
          checked={todo.done}
          on:change={() => dispatch("toggleDone")}
        />Mark As Done</label>
    </div>
    <div class="card-footer-item">
      <button on:click={() => dispatch("delete")}>Delete</button>
    </div>
  </footer>
</div>

<style>
  .done-true {
    color: gray;
    text-decoration: line-through;
  }

  li {
    margin-top: 5px;
  }
</style>
