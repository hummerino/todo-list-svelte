<!--
--	SCRIPT
-->
<script>
	import CheckCircleOutline from "svelte-material-icons/CheckCircleOutline.svelte";
	import CircleOutline from "svelte-material-icons/CircleOutline.svelte";
	import DeleteOutline from "svelte-material-icons/Delete.svelte";

	//Elementi della toDo List
	let items = [ ];
	let itemsDeleted = [];
	let itemsChecked = [];

	//Elemento da utilizzare per l'input
	let itemToAdd = {
		value : "", 
		checked : false
	};

	function onClickAddItem() {

		if( itemToAdd.value === "") return;

		const item = {
			...itemToAdd, 
			id : items.length + 1
		}
		items = [ item, ...items] ;
	}

	function onClickChecked( id, index ) {
		const oldItem = {...items[index], checked : !items[index].checked };
		const newItems = items.filter( item => item.id !== id );
		items = newItems;
		itemsChecked = [oldItem, ...itemsChecked ];
	}

	function onClickUncheck( id, index ) {
		const oldItem = {...itemsChecked[index], checked : !itemsChecked[index].checked };
		const newItems = itemsChecked.filter( item => item.id !== id );
		itemsChecked = newItems;
		items = [oldItem, ...items ];
	}

	function onClickDeleteItem( id, index, listName ){

		if( listName === 'tocheck' ){
			const oldItem = {...items[index]};
			const newItems = items.filter( item => item.id !== id );
			items = newItems;
			itemsDeleted = [ oldItem, ...itemsDeleted];
		}else{
			const oldItem = {...itemsChecked[index]};
			const newItems = itemsChecked.filter( item => item.id !== id );
			itemsChecked = newItems;
			itemsDeleted = [ oldItem, ...itemsDeleted];
		}
	}
</script>

<!--
--	TAMPLATE
-->

<div class="component_wrapper">

	<h1>ToDo List</h1>

	<h2>To check</h2>
	<input type="text" bind:value={itemToAdd.value} >
	<button on:click={onClickAddItem}>Aggiungi</button>
	<ul>
		{#each items as item, i}
			<li on:click={() => onClickChecked(item.id, i) }>
				<div class="start">
					{#if item.checked }
						<CheckCircleOutline color={'#23cdf4'} />
					{:else}
						<CircleOutline color={'#85e9ff'}/>
					{/if}
				</div>
	
				<div class="center">
					{item.value}
				</div>
				
				<div class="end" on:click|stopPropagation={() => onClickDeleteItem( item.id, i, 'tocheck' )}>
					<DeleteOutline color={'#d95252'}/>
				</div>
			</li>
		{/each}
	</ul>
	
	
	
	<h2>Checked</h2>
	<ul>
		{#each itemsChecked as item, i}
			<li on:click={() => onClickUncheck(item.id, i) }>
				<div class="start">
					{#if item.checked }
						<CheckCircleOutline color={'#23cdf4'} />
					{:else}
						<CircleOutline color={'#85e9ff'}/>
					{/if}
				</div>
	
				<div class="center">
					{item.value}
				</div>
				
				<div class="end" on:click|stopPropagation={() => onClickDeleteItem( item.id, i, 'checked' )}>
					<DeleteOutline color={'#d95252'}/>
				</div>
			</li>
		{/each}
	</ul>
	
	<h2>Deleted</h2>
	<ul>
		{#each itemsDeleted as item }
			<li>
				<div class="center deleted">
					{item.value}
				</div>
			</li>
		{/each}
	</ul>

</div>



<!--
--	STYLE
-->
<style>

	.component_wrapper{
		width: 80%;
		max-width: 600px;
		margin: 0 auto;
	}
	ul{
		list-style-type: none;
		margin: 0px;
		padding: 0px;
		width: 100%;
		max-height: 200px;
		min-height: 200px;
		overflow: auto;
		border: 1px solid rgba( 0,0,0,.1);
		border-radius: 10px;
		padding: 10px;
	}

	li{
		margin-bottom: 10px;
		padding-bottom: 5px;
		border-bottom: 1px solid rgba(0,0,0,.3);
		display: flex;
		letter-spacing: .8px;
		font-weight: 600;
		color: rgba( 0, 0, 0, .7);
	}

	li .start,.end { width:  5%; }
	li .center { width: 90%; text-transform: capitalize; }

	input{
		border-radius: 5px;
		padding-left: 15px;
		padding-right: 15px;
		outline-color: #85e9ff;
	}

	button{
		border-radius: 5px;
		padding-left: 20px;
		padding-right: 20px;
		letter-spacing: .8px;
		font-weight: 600;
		background: #85e9ff;
	}

	button:active{
		background: #23cdf4;
	}

	h1,h2{
		letter-spacing: .8px;
		color: rgba( 0, 0, 0, .7);
	}

	.deleted{
		color: rgba( 0, 0, 0, .3);
	}

</style>