<script>
  export let id;
  export let value;
  export let checkState;
  export let checkStateDispatcher;
  export let removeTodoDispatcher;
</script>

<div class="todo-item" id={`todo-item-${id}`}>
  <input
    id={`todo-checkbox-${id}`}
    type="checkbox"
    bind:checked={checkState}
    class="todo-checkbox"
    on:click={(event) => {
      checkStateDispatcher(event);
    }}
  />
  <label for={id}>{value} -> {id}</label>
  <button
    id={`remove-btn-${id}`}
    on:click={(event) => removeTodoDispatcher(event)}>x</button
  >
</div>

<style>
  .todo-item {
    border-radius: 0.5rem;
    width: 30ch;
    background-color: rgba(255, 255, 255, 0.05);
    padding: 0.5rem;
    margin: 0.5rem;
    display: flex;
    align-items: center;
    justify-content: flex-start;
  }

  .todo-item > button {
    margin-left: auto;
  }

  .todo-item > input {
    height: 1.5rem;
    width: 1.5rem;
  }

  .todo-item > * {
    margin-left: 1rem;
    margin-right: 1rem;
  }
</style>
