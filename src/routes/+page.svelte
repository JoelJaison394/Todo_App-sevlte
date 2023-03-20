<script>
	import Icon from '$lib/images/add.png';
	let todos = [];
	let tag = 'work';
	const tagsToEmoji = {
		work: '💼',
		study: '📚',
		read:'📖',
		write:'✍🏼',
		draw: '🎨',
		exercise:'🏃',
		hobby: '🎮',
		swimming: '🏊'
	};
	let task = '';
	let filter = 'all';
	function addTask() {
		todos = [
			{
				task: task,
				status: 'pending',
				tag: tag
			},
			...todos
		];
		task = '';
	}
	function markComplete(i) {
		todos[i].status = 'completed';
		todos = [...todos];
	}
	function removeTask(i) {
		todos.splice(i, 1);
		todos = [...todos];
	}
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<div class="container">
	<div class="todo">
		<div class="form">
			<input
				type="text"
				bind:value={task}
				on:keydown={(e) => {
					if (e.key === 'Enter') {
						addTask();
					}
				}}
			/>

			<button on:click={addTask}>
				<img src={Icon} alt="" srcset="" />
			</button>
			<select bind:value={tag}>
				<option value="work">Work</option>
				<option value="study">Study</option>
				<option value="draw">Drawing</option>
				<option value="read">Reading</option>
				<option value="hobby">Hobby</option>
				<option value="swimming">Swimming</option>
				<option value="exercise">Exercise</option>
				<option value="write">Writing</option>
			</select>
		</div>
		<div class="filters">
			<button
				class={filter == 'all' ? 'active' : ''}
				on:click={() => {
					filter = 'all';
				}}
			>
				All
			</button>
			<button
				class={filter == 'completed' ? 'active' : ''}
				on:click={() => {
					filter = 'completed';
				}}
			>
				Completed
			</button>
			<button
				class={filter == 'incomplete' ? 'active' : ''}
				on:click={() => {
					filter = 'incomplete';
				}}
			>
				Incomplete
			</button>
		</div>
		<div class={todos.length > 0 ? 'tasks' : ''}>
			{#each todos as todo, i}
				{#if filter == 'all'}
					<div class="task">
						<div class={todo.status == 'completed' ? 'active-ul' : ''}>
							{todo.task}
						</div>
						<span>{tagsToEmoji[todo.tag]}</span>						
						<button
							class={todo.status == 'completed' ? 'active' : ''}
							on:click={() => {
								markComplete(i);
							}}
						>
							&#10004;
						</button>
						<button
							on:click={() => {
								removeTask(i);
							}}
						>
							&#10006;
						</button>
					</div>
				{:else if filter == 'completed'}
					{#if todo.status == 'completed'}
						<div class="task">
							<div>
								{todo.task}
							</div>
							<button
								on:click={() => {
									removeTask(i);
								}}
							>
								&#10006;
							</button>
						</div>
					{/if}
				{:else if todo.status == 'pending'}
					<div class="task">
						<div>
							{todo.task}
						</div>
						<button
							class={todo.status == 'completed' ? 'active' : ''}
							on:click={() => {
								markComplete(i);
							}}
						>
							&#10004;
						</button>
					</div>
				{/if}
			{/each}
		</div>
	</div>
</div>

<style>
	.todo {
		padding: 20px;
		opacity: 0.8;
		width: 70vw;
		height: auto;
		border-radius: 20px;
		background: white;
	}

	.todo > div {
		margin: 20px 0px;
	}

	.todo .form {
		display: flex;
		align-items: center;
		justify-content: space-around;
		height: fit-content;
		border: 1px solid grey ;
		border-radius: 15px;
		padding: 5px;
	}

	img {
		height: 20px;
		object-fit: cover;
	}

	input,
	button {
		background: transparent;
		border: none;
	}

	input {
		border-top-left-radius: 8px;
		border-right: none;
		height: 35px;
		width: calc(70vw);
		padding: 2px;
		font-size: 22px;
	}

	input:focus {
		outline: none;
	}
	.form button {
		border-left: none;
		height: 35px;
		border-top-right-radius: 8px;
		padding: 2px;
		cursor: pointer;
	}

	.tasks {
		border: 1px solid grey;
		border-top-right-radius: 20px;

		border-top-left-radius: 20px;
		padding: 20px 5px;
	}

	.task {
		border-top: 1px solid grey;
		word-wrap: break-word;
		font-size: 20px;
		display: flex;
		align-items: center;
		padding: 10px;
	}
	.task span {
		margin-left: 5px;
		margin-right: 8px;
	}

	.todo .tasks > .task > button {
		width: 25px;
		height: 25px;
		border: 1px solid #000000;
		color: #000000;
		border-radius: 50%;
		margin: 0px 5px;
		cursor: pointer;

	}
	.todo .tasks > .task > button.active {
		background: #020202;
		color: #f5f5f5;
	}
	.todo .filters {
		display: flex;
		justify-content: space-between;
	}
	.todo .filters > button {
		min-width: 150px;
		padding: 10px 8px;
		border-bottom: 1px solid #000000ce;
		cursor: pointer;
		font-size: 20px;
	}
	.todo .filters > button.active {
		background: #000000;
		color: #f5f5f5;
	}
	select{
		padding: 10px;
		margin: 3px;
		font-size: 20px;
	}
	.active-ul{
		text-decoration: line-through;
	}
</style>
