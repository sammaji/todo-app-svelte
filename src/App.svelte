<script>
  import TodoItem from "./components/TodoItem.svelte";
  let counter = -1;
  let input_value;
  let data = {
    todo_data: [],
  };

  function updateCheckState(event) {
    let todo_id = Number(event.target.id.replaceAll(/todo-checkbox-/g, ""));
    for (let index = 0; index < data.todo_data.length; index++) {
      if (data.todo_data[index].id === todo_id) {
        data.todo_data[index].checkState = event.target.checked;
      }
    }
  }

  function addTodo() {
    if (typeof input_value !== undefined && input_value.length !== 0) {
      new TodoItem({
        target: document.getElementById("todo-con"),
        props: {
          id: ++counter,
          value: input_value,
          checkState: false,
          checkStateDispatcher: (event) => {
            updateCheckState(event);
          },
          removeTodoDispatcher: (event) => {
            removeTodo(event);
          },
        },
      });

      data.todo_data.push({
        id: counter,
        value: input_value,
        checkState: false,
      });
    } else throw new Error("INVALID INPUT");
    input_value = ""; // clearing inputs
  }

  /** removes todoitem and updates data*/
  function removeTodo(event) {
    let todo_id = Number(event.target.id.replaceAll(/remove-btn-/g, ""));
    document.getElementById(`todo-item-${todo_id}`)?.remove();
    data.todo_data.splice(todo_id);
  }
  /** removes todoitem but does not update data*/
  function removeSelf(event) {
    event.target.remove();
  }
  /** @param status -> { checked | unchecked | all } */
  function filterByStatus(status) {
    let checkStatus = status === "checked" ? true : false;
    let todoContainer = document.getElementById("todo-con");
    todoContainer.innerHTML = "";
    for (let index = 0; index < data.todo_data.length; index++) {
      if (
        data.todo_data[index].checkState === checkStatus &&
        status !== "all"
      ) {
        new TodoItem({
          target: todoContainer,
          props: {
            id: data.todo_data[index].id,
            value: data.todo_data[index].value,
            checkState: checkStatus,
            checkStateDispatcher: () => removeSelf(),
            removeTodoDispatcher: (event) => removeTodo(event),
          },
        });
      } else if (status === "all") {
        new TodoItem({
          target: todoContainer,
          props: {
            id: data.todo_data[index].id,
            value: data.todo_data[index].value,
            checkState: data.todo_data[index].checkState,
            checkStateDispatcher: (event) => updateCheckState(event),
            removeTodoDispatcher: (event) => removeTodo(event),
          },
        });
      }
    }
  }
</script>

<main>
  <div>
    <input
      placeholder="Type Something"
      id="inp-submit"
      type="text"
      bind:value={input_value}
    />
    <button type="submit" on:click={addTodo}>Add</button>
  </div>

  <div>
    <button on:click={() => filterByStatus("all")}>Remove filters</button>
    <button on:click={() => filterByStatus("checked")}
      >Filter: By Checked</button
    >
    <button on:click={() => filterByStatus("unchecked")}
      >Filter: By Unchecked</button
    >
  </div>

  <div id="todo-con" />
</main>

<style>
  main {
    display: flex;
    flex-direction: column;
    min-height: 80vh;
    height: fit-content;
    align-items: center;
    max-width: 45ch;
    width: 80%;
  }
  main > div {
    display: flex;
    padding: 0.5rem 0rem;
  }
  main > div:last-child {
    display: flex;
    flex-direction: column;
    padding-top: 1rem;
  }
  button,
  input {
    margin-left: 0.5rem;
    margin-right: 0.5rem;
  }
</style>
