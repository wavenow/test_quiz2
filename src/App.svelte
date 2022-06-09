<script>
	
	import { onMount } from "svelte";
	import { Card, CardBody, CardHeader, Input, Table, Column, Styles } from 'sveltestrap';
	
	const endpoint = "https://api.publicapis.org/categories";
  let categories = [];
	let status = "Loading...." ;
	let search = undefined;
	
	$: visibleCategories = search ?
		categories.filter(category => {
			return category.toLowerCase().match(`${search.toLowerCase()}.*`)
		}) : categories;
	
	var action = () => {
		console.log(search);
	}
	
	const myFetch = async (myRequest) => {  
		
		try {  
			const response = await fetch(myRequest);  
			const text = await response.text();  
			const data = JSON.parse(text); 
			categories = data.categories;
			status = "";
		} catch (err) {  
			status = "Error : " + err.message;
			console.log(err)  
		}  
		
	}  
	
	myFetch(endpoint);

</script>

<style>
  @import url('https://gthomas-appfolio.github.io/bootstrap-coastline/bootstrap-coastline.css');
</style>

<Card>
	<CardHeader>
		<Input type="search" bind:value={search} class="ms-auto w-auto" placeholder="Search" />
	</CardHeader>
	<div>
		{status}
	</div>
	<CardBody>
		<Table striped rows={visibleCategories} let:row={category}>
			<Column header="Categories">
				{category}
			</Column>
		</Table>
	</CardBody>
</Card>