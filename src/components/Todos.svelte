<script>
    import FilterButton from "./FilterButton.svelte";
    import Todo from "./Todo.svelte";
    export let todos = [];

    $: totalTodos = todos.length;
    $: completedTodos = todos.filter((todo) => todo.completed).length;

    let newTodoName = "";
    let filter = "all";

    $: newTodoId =
        totalTodos === 0 ? 1 : Math.max(...todos.map((t) => t.id)) + 1;

    const filterTodos = (filter, todos) =>
        filter === "active"
            ? todos.filter((t) => !t.completed)
            : filter === "completed"
            ? todos.filter((t) => t.completed)
            : todos;

    function removeTodo(todo) {
        todos = todos.filter((t) => t.id !== todo.id);
    }

    function addTodo() {
        let newTodo = { id: newTodoId, name: newTodoName, completed: false };
        todos = [...todos, newTodo];
        newTodoName = "";
    }

    function updateTodo(todo) {
        const i = todos.findIndex((t) => t.id === todo.id);
        todos[i] = { ...todos[i], ...todo };
    }
</script>

<div class="todoapp stack-large">
    <!-- NewTodo -->
    <form on:submit|preventDefault={addTodo}>
        <h2 class="label-wrapper">
            <label for="todo-0" class="label__lg">
                What needs to be done?
            </label>
        </h2>
        <input
            bind:value={newTodoName}
            type="text"
            id="todo-0"
            autocomplete="off"
            class="input input__lg"
        />
        <button type="submit" disabled={!newTodoName} class="btn btn__primary btn__lg">
            Add
        </button>
    </form>

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

    <!-- MoreActions -->
    <div class="btn-group">
        <button type="button" class="btn btn__primary">Check all</button>
        <button type="button" class="btn btn__primary">Remove completed</button>
    </div>
</div>
