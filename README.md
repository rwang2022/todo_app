# todo_app

## things to keep in mind

### removing items that satisfy a condition
https://stackoverflow.com/a/54457105/17197153
    
    // // remove people that like blue
    // array.filter((x) => x.faveColor === 'blue').forEach((x) => array.splice(array.indexOf(x), 1));

    // remove items that have status (finished) as true
    todolist.filter((x) => x.status === true).forEach((x) => todolist.splice(todolist.indexOf(x), 1));
    todolist = todolist;

### date object into readable form
    return date.toLocaleString('en-US', {
                timeZone: 'America/New_York'
            });

### the $: keyword is used like so:


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


### register key presses 
https://svelte.dev/repl/d99826cdac4f4fdf8064f5b6a31676ff?version=3.18.2

    const onKeyPress = (e) => {
    if (e.charCode === 13) {
        addToList();
    }
    // e has key, charCode, keyCode
    // console.log(e);
	};

    <input
        type="text"
        placeholder="Add Task Here"
        on:keypress={onKeyPress}
        bind:value={task}
    />

### toggle a class?
    <span class:checked={item.status}>{item.text}</span>
