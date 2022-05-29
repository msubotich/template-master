<script>
	let todos = [];
	let todo = { id: "", texto: "", estado: false };

	if (localStorage.getItem("todos")) {
		todos = JSON.parse(localStorage.getItem("todos"));
	} // Los datos quedarán guardados solo en el navegador

	$: localStorage.setItem("todos", JSON.stringify(todos));
	//cada vez que se actualice "todos" ejecuta la función de arriba

	const addTodo = () => {
		if (!todo.texto.trim()) {
			console.log("texto vacio");
			todo.texto = "";
			return;
		}

		todo.id = Date.now();
		todos = [...todos, todo];
		console.log(todos);
		todo = { id: "", texto: "", estado: false };
	};

	const delTodo = (id) => {
		todos = todos.filter((item) => item.id !== id);
	};

	const editTodo = (id) => {
		//todos = todos.map(item => item.id === id ?
		//{...item, estado: !item.estado}:item)
		todos = todos.map((item) => {
			if (item.id === id) {
				return {
					...item,
					estado: !item.estado,
				};
			} else {
				return item;
			}
		});
		console.log(todos);
	};

	const classIcono = (valor) =>
		valor ? "bi bi-arrow-clockwise" : "bi bi-check2";

	const classEstado = (valor) => (valor ? "btn-success" : "btn-warning");
</script>

<div class="container">
	<h1 class="display-5 my-3">Lista de tareas</h1>
	<form on:submit|preventDefault={addTodo}>
		<input
			type="text"
			placeholder="Enter para agregar tarea"
			class="form-control shadow border-0"
			bind:value={todo.texto}
		/>
	</form>

	{#each todos as item}
		<div class="shadow my-3 p-3 lead">
			<p class={item.estado ? "text-decoration-line-through" : ""}>
				{item.texto}
			</p>
			<button
				class="btn btn-sm {classEstado(item.estado)} btn-"
				on:click={editTodo(item.id)}
			>
				<i class={classIcono(item.estado)} />
			</button>
			<button class="btn btn-sm btn-danger" on:click={delTodo(item.id)}>
				<i class="bi bi-trash" />
			</button>
		</div>
	{/each}

	<div
		class="toast align-items-center text-bg-primary border-0"
		role="alert"
		aria-live="assertive"
		aria-atomic="true"
	>
		<div class="d-flex">
			<div class="toast-body">Hello, world! This is a toast message.</div>
			<button
				type="button"
				class="btn-close btn-close-white me-2 m-auto"
				data-bs-dismiss="toast"
				aria-label="Close"
			/>
		</div>
	</div>
</div>
