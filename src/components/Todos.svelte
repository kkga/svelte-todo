<script>
  import FilterButton from "./FilterButton.svelte";
  import Todo from "./Todo.svelte";
  import MoreActions from "./MoreActions.svelte";
  import NewTodo from "./NewTodo.svelte";
  import TodoStatus from "./TodoStatus.svelte";

  export let todos = [];

  let todoStatus;

  let filter = "all";

  $: newTodoId =
    todos.length === 0 ? 1 : Math.max(...todos.map((t) => t.id)) + 1;

  const filterTodos = (filter, todos) =>
    filter === "active"
      ? todos.filter((t) => !t.completed)
      : filter === "completed"
      ? todos.filter((t) => t.completed)
      : todos;

  function removeTodo(todo) {
    todos = todos.filter((t) => t.id !== todo.id);
    todoStatus.focus();
  }

  function addTodo(name) {
    todos = [...todos, { id: newTodoId, name: name, completed: false }];
  }

  function updateTodo(todo) {
    const i = todos.findIndex((t) => t.id === todo.id);
    todos[i] = { ...todos[i], ...todo };
  }

  const checkAllTodos = (completed) => {
    todos.forEach((t) => (t.completed = completed));
    todos = todos;
  };

  const removeCompletedTodos = () =>
    (todos = todos.filter((t) => !t.completed));
</script>

<div class="todoapp stack-large">
  <NewTodo autofocus on:addTodo={(e) => addTodo(e.detail)} />

  <FilterButton bind:filter />

  <TodoStatus bind:this={todoStatus} {todos} />

  <ul role="list" class="todo-list stack-large">
    {#each filterTodos(filter, todos) as todo (todo.id)}
      <li class="todo">
        <Todo
          {todo}
          on:update={(e) => updateTodo(e.detail)}
          on:remove={(e) => removeTodo(e.detail)}
        />
      </li>
    {:else}
      <li>Nothing here!</li>
    {/each}
  </ul>

  <hr />

  <MoreActions
    {todos}
    on:checkAll={(e) => checkAllTodos(e.detail)}
    on:removeCompleted={removeCompletedTodos}
  />
</div>
