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
          hidden
        /><i class="fas fa-check" /></label
      >
    </div>
    <div class="card-footer-item">
      <label class="button">
        <button on:click={() => dispatch("delete")} hidden />
        <i class="fas fa-trash" />
      </label>
    </div>
  </footer>
</div>

<style>
  .done-true {
    color: gray;
    text-decoration: line-through;
  }
</style>
