<script>
	import Pagination from "./Pagination.svelte";
	let ttlPage = 20;
	
	const handleCurrPage = e =>{
		console.log("currPage :", e.detail );
	}
</script>

<!-- Test section -->
<button on:click={()=>ttlPage++}>
	Add MORE PAGE	
</button>

<!-- Test section end-->

<Pagination ttlPage={ttlPage} on:currPage={handleCurrPage} />