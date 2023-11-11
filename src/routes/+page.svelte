<script lang="ts">
  import TodoCard from "$lib/client/components/TodoCard.svelte";
  let todos = $state<
    {
      title: string;
      description: string;
    }[]
  >([
    {
      title: "Make More Original Demos",
      description:
        "I have built at least 10 different todo apps in the last year. I need new ideas.",
    },
    {
      title: "Rebuild Everything in Svelte 5",
      description: "Because its fun.",
    },
  ]);

  let nTodoTitle = $state("");
  let nTodoDescription = $state("");

  const addTodo = () => {
    todos = [...todos, { title: nTodoTitle, description: nTodoDescription }];

    nTodoTitle = "";
    nTodoDescription = "";
  };

  const removeTodo = (idx: number) => {
    const copy = todos;
    copy.splice(idx, 1);
    todos = copy;
  };

  const numberOfDescriptionWords = $derived(() => {
    let count = 0;
    todos.forEach((todo) => {
      const descWords = todo.description.split(" ");
      count += descWords.length;
    });
    return count;
  });

  const numberOfS = $derived(() => {
    let count = 0;
    todos.forEach((todo) => {
      const charactersD = todo.description.split("");
      const charactersT = todo.title.split("");
      const characters = [...charactersD, ...charactersT];
      characters.forEach((c) => {
        if (c.toLowerCase() === "s") {
          count += 1;
        }
      });
    });
    return count;
  });

  const numberOfTodosCubed = $derived(
    todos.length * todos.length * todos.length
  );

  $effect(() => {
    if (todos.length === 3) {
      alert("You now have 3 todos!");
    }
  });
</script>

<div
  class="bg-gradient-to-br from-gray-950 via-blue-700 to-orange-600 text-slate-100 w-full h-screen flex flex-row items-center justify-center gap-x-12"
>
  <div class="bg-white bg-opacity-20 backdrop-blur-lg p-4 rounded-xl w-1/4">
    <div class="flex flex-col items-center gap-y-1 pb-4">
      <h2 class="text-3xl font-bold">Basic Todo App</h2>
      <h4 class="text-sm font-light italic">
        Because I lack the time/creativity to make another example...
      </h4>
    </div>

    <div class="flex flex-col gap-y-4 items-end pb-4">
      <div class="w-full">
        <label for="title" class="text-sm font-light block">title</label>
        <input
          type="text"
          name="title"
          bind:value={nTodoTitle}
          class="text-gray-900 p-2 w-full rounded-lg focus:ring ring-orange-600"
        />
      </div>
      <div class="w-full">
        <label for="description" class="text-xm font-light block"
          >description</label
        >
        <textarea
          class="text-gray-900 p-2 w-full rounded-lg focus:ring ring-orange-600"
          name="description"
          bind:value={nTodoDescription}
        />
      </div>

      <button
        on:click={addTodo}
        class="bg-gradient-to-r from-blue-700 to-blue-600 bg-opacity-50 text-xl font-bold px-4 py-1 rounded-full hover:bg-gradient-to-l"
        >Create</button
      >
    </div>

    <h2 class="text-xl italic font-light text-center underline pb-2">
      Some Todo Stats
    </h2>
    <div class="flex flex-col items-center w-full">
      <h3>
        # of words in all descriptions: <span class="font-bold"
          >{numberOfDescriptionWords()}</span
        >
      </h3>
      <h3>
        # of todos cubed: <span class="font-bold">{numberOfTodosCubed}</span>
      </h3>
      <h3>
        # of the letter "s" in your todos: <span class="font-bold"
          >{numberOfS()}</span
        >
      </h3>
    </div>
  </div>

  <div class="w-1/2">
    {#each todos as todo, i}
      <TodoCard
        title={todo.title}
        description={todo.description}
        remove={() => removeTodo(i)}
      />
    {/each}
  </div>

  <div
    class="absolute bottom-0 w-full flex py-4 items-center justify-center gap-x-8"
  >
    <a
      href="https://twitter.com/benjamin41902"
      target="_blank"
      class="font-light hover:underline">@benjamin41902</a
    >
    <a
      href="https://github.com/bmdavis419/svelte-five-demo"
      target="_blank"
      class="font-light hover:underline">project repo</a
    >
  </div>
</div>
