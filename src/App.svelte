<script>
    let tasks = [];
    let newTask = '';

    // Function to add a new task
    async function addTask() {
        const response = await fetch('http://127.0.0.1:8080/add-todo', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({ task: newTask }),
        });
        if (response.ok) {
            const task = await response.json();
            tasks = [...tasks, task];
            newTask = '';
        }
    }

    // Function to update a task
    async function updateTask(task) {
        const response = await fetch('http://127.0.0.1:8080/update-todo', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({ completed: task.completed ? true : false, id: task.id, task: task.task }),
        });
        if (response.ok) {
            // Task updated successfully
        }
    }

    // Function to retrieve tasks from the server
    async function getTasks() {
        const response = await fetch('http://127.0.0.1:8080/get-todo');
        if (response.ok) {
            tasks = await response.json();
        }
    }

    // Load tasks on component initialization
    getTasks();
</script>

<h1>To-Do List</h1>

<div>
    <input type="text" bind:value={newTask} placeholder="Add a new task" />
    <button on:click={addTask}>Add</button>
</div>

<ul>
    {#each tasks as task (task.id)}
        <li>
            <input type="checkbox" bind:checked={task.completed} on:change={() => updateTask(task)} />
            <span class:completed={task.completed}>{task.task}</span>
        </li>
    {/each}
</ul>

<style>
    .completed {
        text-decoration: line-through;
        color: #888;
    }
</style>
