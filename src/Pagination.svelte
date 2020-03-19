<script>
	import {createEventDispatcher} from "svelte"; 
	
	let dispatch = createEventDispatcher();
	
	export let startPage = 1;
	export let ttlPage = 10;
	export let maxPage = 5;
	export let currPage = startPage;

	$: endPage = startPage + ttlPage -1;
	$:  arr =	buildArr(startPage, ttlPage, endPage);

	// dispatch on current page change
	$: dispatch("currPage", currPage );
	
	let primaryColor = "#3c89d0";

	let forwardArrow = ">";
	let backwardArrow = "<";
	let fastForwardArrow = ">>";
	let fastBackwardArrow = "<<";
	
	function buildArr() {	
		// console.log({startPage, endPage, maxPage, ttlPage});
		let tempArr  = [];
		for(
				let i =startPage; 
				(i <= endPage) && (tempArr.length < maxPage);
				i++
			 ) {
				let obj = {
					id:String(i),
					label: i
				}
				tempArr = [...tempArr, obj];
		 }
	
		return tempArr;
	}
	
	// increment setting
	function incPage() {
		if(ttlPage === currPage) return;
		if(currPage === startPage+maxPage-1) startPage++;
		currPage++;
	}

	function incSlot() {
		if(ttlPage===currPage) return;
		
		let newPage = currPage;
		newPage +=5;
		if( newPage > ttlPage ) newPage = ttlPage;

		currPage = newPage;
		if(currPage >  startPage + maxPage - 1) {
			startPage+=5;
			if(startPage > ttlPage - maxPage)	startPage = ttlPage - maxPage + 1;
		}
	}
	
	// decrement setting
	function decPage() {
		if(currPage === 1) return;
		if(currPage === startPage) startPage --;
		currPage--;
	}
	
	function decSlot() {
		if(currPage===1) return;
		
		let newPage = currPage;
		newPage -= 5;
		if( newPage < 1 ) newPage=1;

		currPage = newPage;
		
		if( currPage < startPage ) {
			startPage -= 5;
			if(startPage < 1) startPage = 1;
		}
	}
	
</script>


<div class="pagination" >
	
	<p class="btn backward"  on:click={decSlot} class:disable={currPage===1} >
		{fastBackwardArrow}
	</p >
	
	<p class="btn backward" on:click={decPage} class:disable={currPage===1}>
		{backwardArrow}
	</p>
	
	<div class="btns">
		{#each arr as el,  i}
			<p 
				 class="btn" 
				 class:active={currPage === el.label}
				 on:click={()=>currPage = el.label}
				 >
				{el.label}
			</p>
		{/each}
	</div>
	
	
	<p class="btn forward"  on:click={incPage} class:disable={currPage===ttlPage}>
		{forwardArrow}
	</p >
	<p class="btn forward"  on:click={incSlot} class:disable={currPage===ttlPage} >
		{fastForwardArrow}
	</p >
	
</div>

<style>
	.pagination {
		display: flex;
		width: 400px;
		
	}
	.pagination > .btns {
			justify-content: center;
			width:60%;
	}
	
	.btn {
		border : 1px solid black;
		border-radius: 100%;
		width: 30px;
		height: 30px;
		text-align: center;
		margin: 0px 5px;
		cursor: pointer;
		padding-top: 2px;

		
		-webkit-touch-callout: none; /* iOS Safari */
		-webkit-user-select: none; /* Safari */
		-khtml-user-select: none; /* Konqueror HTML */
		-moz-user-select: none; /* Old versions of Firefox */
		-ms-user-select: none; /* Internet Explorer/Edge */
		user-select: none; /* Non-prefixed version, currently
		supported by Chrome, Opera and Firefox */
	}
	
	.btns {
		display:flex;
		width: 50%;
		justify-content: center;
	}
	
	.active {
		background: #3c89d0;
		color: white;
		border-color: #3c89d0;
	}

	
	
	.forward, .backward {
		background: #eee;
	}
	
	.btn:hover {
		background: #eee;
	}
	.active:hover {
		background: #3c89d0;
		color: white;
		border-color: black;
	}

	.disable {
		cursor: no-drop;
	}

</style>