<script>
	import { each } from 'svelte/internal';

	let task = '';

	let todolist = [
		{ text: '0: go to school', status: false, dateAdded: new Date() },
		{ text: '1: go to school', status: true, dateAdded: new Date() },
		{ text: '2: go to school', status: true, dateAdded: new Date() },
		{ text: '3: go to school', status: false, dateAdded: new Date() },
		{ text: '4: go to school', status: true, dateAdded: new Date() },
		{ text: '5: go to school', status: false, dateAdded: new Date() },
		{ text: '6: go to school', status: false, dateAdded: new Date() },
		{ text: '7: go to school', status: true, dateAdded: new Date() },
		{ text: '8: go to school', status: true, dateAdded: new Date() }
	];

	// unexplained: https://svelte.dev/repl/d99826cdac4f4fdf8064f5b6a31676ff?version=3.18.2
	const onKeyPress = (e) => {
		if (e.charCode === 13) {
			addToList();
		}
		// e has key, charCode, keyCode
		// console.log(e);
	};

	function addToList() {
		// only adds to list if not empty
		if (task != '') {
			todolist = [{ text: task, status: false, dateAdded: new Date() }, ...todolist];
			task = '';
		} else {
		}
	}

	function removeFromList(index) {
		todolist.splice(index, 1);
		todolist = todolist;
	}

	function clearEntireList() {
		todolist = [];
	}

	function clearFinishedList() {
		// https://stackoverflow.com/a/54457105/17197153
		// var array = [
		// 	{ id: 1, name: 'bob', faveColor: 'blue' },
		// 	{ id: 2, name: 'jane', faveColor: 'red' },
		// 	{ id: 3, name: 'sam', faveColor: 'blue' }
		// ];

		// // remove people that like blue
		// array.filter((x) => x.faveColor === 'blue').forEach((x) => array.splice(array.indexOf(x), 1));

        // remove items that have status (finished) as true
		todolist.filter((x) => x.status === true).forEach((x) => todolist.splice(todolist.indexOf(x), 1));
        todolist = todolist;
	}

	function convertDate(date) {
		return date.toLocaleString('en-US', {
			timeZone: 'America/New_York'
		});
	}

	function setAll(bool) {
		for (let i = 0; i < todolist.length; i++) {
			todolist[i].status = bool;
		}
		todolist = todolist;
	}
	function finishAll() {
		setAll(true);
	}
	function unfinishAll() {
		setAll(false);
	}

	// any statement after $: will rerun with any change in any variable used within the statement
	$: checkAll = () => {
		let allDone = true;
		for (let i = 0; i < todolist.length; i++) {
			if (todolist[i].status == false) {
				allDone = false;
			}
		}
		return allDone;
	};

	$: test2 = checkAll();
</script>

<h1>this is a basic todo app, index.svelte</h1>
{#if test2}
	<h1 style="color: green;">You have finished all tasks! ✅</h1>
{:else}
	<h1 style="color: red;">You have not finished all tasks. ❌</h1>
{/if}

<table>
	<tr>
		<td>Add task here</td>
		<td
			><input
				type="text"
				placeholder="Add Task Here"
				on:keypress={onKeyPress}
				bind:value={task}
			/></td
		>
		<td><button on:click={addToList}>add task</button></td>
	</tr>
	<tr>
		<td>
			<button on:click={finishAll}>complete all</button>
			<button on:click={unfinishAll}>unfinish all</button>
		</td>
		<td>Your Tasks</td>
		<td>
			<button on:click={clearEntireList}>clear all</button>
			<button on:click={clearFinishedList}>clear finished</button>
		</td>
		<td>Date Added</td>
		<td>Status of Task</td>
	</tr>
	{#each todolist as item, index}
		<tr>
			<td><input type="checkbox" class="checkbox" bind:checked={item.status} /></td>
			<td><span class:checked={item.status}>{item.text}</span></td>
			<td><button on:click={() => removeFromList(index)}> ❌ </button></td>
			<td>{convertDate(item.dateAdded)}</td>
			<td>{item.status}</td>
		</tr>
	{/each}
</table>

<style>
	.checked {
		color: crimson;
		text-decoration: line-through;
	}
	.checkbox {
		width: 30px;
		height: 30px;
	}
	button {
		width: 120px;
		height: 30px;
	}
	table {
		width: 70%;
	}
	table,
	td {
		border: 1px solid black;
		border-collapse: collapse;
	}
	input {
		width: auto;
		height: auto;
	}
</style>
