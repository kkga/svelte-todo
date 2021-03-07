<script>
  export let todos;
  export const focus = () => headingEl.focus();

  $: totalTodos = todos.length;
  $: completedTodos = todos.filter((todo) => todo.completed).length;

  let headingEl;
</script>

<h2 bind:this={headingEl} tabindex="-1" id="list-heading">
  {completedTodos} out of {totalTodos} items completed
</h2>
