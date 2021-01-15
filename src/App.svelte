<script lang="ts">
  import Tailwindcss from "./Tailwindcss.svelte";
  import { v4 as uuid } from "uuid";

  interface Todo {
    id: string;
    text: string;
    completed: boolean;
  }

  let todoInput = "";
  let todos: Todo[] = [
    {
      id: "1",
      text: "todo 1",
      completed: true,
    },
    {
      id: "2",
      text: "todo 2",
      completed: false,
    },
    {
      id: "3",
      text: "todo 3",
      completed: false,
    },
  ];

  const onSubmit = () => {
    if (!todoInput.trim()) return;

    todos = [
      ...todos,
      {
        id: uuid(),
        text: todoInput,
        completed: false,
      },
    ];

    todoInput = "";
  };

  const onDelete = (id: string) => {
    todos = todos.filter((todo) => todo.id !== id);
  };
</script>

<Tailwindcss />
<main
  class="flex flex-col h-screen mx-auto max-w-screen-md items-center p-10 capitalize"
>
  <h1 class="text-3xl md:text-4xl font-semibold text-gray-800">
    Yet another todo app
  </h1>
  <form class="mt-10 md:mt-20 w-full" on:submit|preventDefault={onSubmit}>
    <input
      class="w-full h-12 px-3.5 shadow-sm focus:ring-4 rounded text-xl"
      name="todo"
      type="text"
      bind:value={todoInput}
    />
  </form>
  {#if todos.length === 0}
    <div>No todos.</div>
  {:else}
    <ul
      class="todos flex flex-wrap gap-1.5 mt-8 px-6 py-4 border border-gray-300 rounded shadow-sm w-full"
    >
      {#each todos as { completed, text, id } (id)}
        <li
          class="todo flex gap-2 items-center w-full cursor-pointer"
          on:click={() => (completed = !completed)}
        >
          <input type="checkbox" bind:checked={completed} />
          <span
            class={[
              "font-light",
              "text-xl",
              "select-none",
              "truncate",
              completed ? "line-through" : "",
            ].join(" ")}>
            {text}
          </span>
          <div
            class="text-gray-700 w-4 ml-auto flex-shrink-0"
            on:click={() => onDelete(id)}
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M6 18L18 6M6 6l12 12"
              />
            </svg>
          </div>
        </li>
      {/each}
    </ul>
  {/if}
</main>
