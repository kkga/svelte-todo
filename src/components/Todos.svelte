<script>
  import FilterButton from "./FilterButton.svelte";
  import Todo from "./Todo.svelte";
  import MoreActions from "./MoreActions.svelte";
  import NewTodo from "./NewTodo.svelte";
  export let todos = [];

  $: totalTodos = todos.length;
  $: completedTodos = todos.filter((todo) => todo.completed).length;

  let filter = "all";

  $: newTodoId = totalTodos === 0 ? 1 : Math.max(...todos.map((t) => t.id)) + 1;

  const filterTodos = (filter, todos) =>
    filter === "active"
      ? todos.filter((t) => !t.completed)
      : filter === "completed"
      ? todos.filter((t) => t.completed)
      : todos;

  function removeTodo(todo) {
    todos = todos.filter((t) => t.id !== todo.id);
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
  <NewTodo on:addTodo={(e) => addTodo(e.detail)} />

  <!-- Filter -->
  <FilterButton bind:filter />

  <!-- TodosStatus -->
  <h2 id="list-heading">
    {completedTodos} out of {totalTodos} items completed
  </h2>

  <!-- Todos -->
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
