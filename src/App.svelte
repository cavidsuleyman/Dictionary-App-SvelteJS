<script>
	import Loading from './Loading.svelte';
    import WordData from './WordData.svelte';

	let word = "";
	let loading = false;
	let wordData = null;

	async function searchWord(){
		if(word.length === 0) {
			return;
		}
		loading = true;
		wordData = null;

		try{
			let res = await fetch("https://api.dictionaryapi.dev/api/v2/entries/en/" + word);
			let data = await res.json();
			if(Array.isArray(data)){
				wordData = data[0];
			}else{
				wordData = "No result";
			}
			loading = false
		}catch(err){
			loading = false;
		}
	}
</script>


<style>
	main{
		width: 100%;
		height: 100vh;
	}
	.header{
		width: 100%;
		padding: 20px;
		background: black;
		color: white;
		text-align: center;
		font-size: 20px;
	}
	.center{
		margin: 40px auto;
		width: 95%;
		max-width: 550px;
	}
	.form{
		background: #fff;
		padding: 20px;
		border: 1px solid #f5f5f5;
		box-shadow: 0 0 5px 2px rgba(0, 0, 0, 0.05);
		margin: 20px 0px;
	}
	.form .form-group{
		margin: 10px 0;
	}
	.form .form-group input{
		width: 100%;
		padding: 10px;
		font-size: 16px;
		border: 1px solid #bbb;
		border-radius: 5px;
	}
	.form .form-group button{
		padding: 10px 20px;
		background: #111;
		color: white;
		border: none;
		outline: none;
		font-size: 15px;
		cursor: pointer;
	}
	.result{
		background: #fff;
		border: 1px solid #f5f5f5;
		box-shadow: 0 0 5px 2px rgba(0, 0, 0, 0.05);
		padding: 10px;
	}
	.result .padding{
		padding: 20px;
	}

</style>

<main>
	<div class="header">MyWord</div>
	<div class="center">
		<div class="form">
			<div class="form-group">
				<input type="text" placeholder="Type word here" bind:value={word}>
			</div>
			<div class="form-group">
				<button on:click={searchWord}>Search</button>
			</div>

		</div>
		{#if loading === true || wordData !== null}
			<div class="result">
				{#if wordData !== null && typeof wordData !== "string"}
					<WordData wordData={wordData}/>
				{:else if wordData === null && loading === true}
					<Loading/>
				{:else}
					<p class="padding">No result data</p>
				{/if}
			</div>
		{/if}
	</div>
</main>