<script>
  import Todos from "./components/Todos.svelte";

  let todos = [
    { id: 1, name: "woot", completed: true },
    { id: 2, name: "woot woot", completed: true },
    { id: 3, name: "hey", completed: false },
  ];
</script>

<Todos {todos} />
