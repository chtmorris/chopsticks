<script lang="ts">
	import {
		accountChainId,
		networkProviders,
		connected,
		connectMetamask,
		connectWalletConnect,
		disconnect,
		walletAddress
	} from '$lib/stores/provider';
	import { balanceOnBlock } from '$lib/stores/state';
	import { formatEther } from 'ethers/lib/utils';
	import { whatsabi } from "@shazow/whatsabi";
    import { debug } from 'svelte/internal';
   
	const provider = networkProviders[1];
  
	const address = "0x00000000006c3852cbEf3e08E8dF289169EdE581"; // Or your fav contract address
	var selectors = "loading";
	var abi = "loading";
	const code =  provider.getCode(address).then(function(r) {
		console.log(r)
	
		// Get just the callable selectors
		selectors = whatsabi.selectorsFromBytecode(r);
		console.log("Logging selectors: " + selectors); // -> ["0x06fdde03", "0x46423aa7", "0x55944a42", ...]

		// Get an ABI-like list of interfaces
		abi = whatsabi.abiFromBytecode(r);
		// debugger
		console.log("Loggin abi: " + abi);
		
		
	}); // Load the bytecode
</script>

<main class="flex flex-col p-10 w-screen h-screen items-center gap-5 font-mono text-white">

	<header class="absolute inset-x-0 top-5 z-50">
		<nav class="flex items-center justify-between p-6 lg:px-8" aria-label="Global">
		  <div class="hidden lg:flex lg:flex-1 lg:justify-end">
			<button on:click={connectWalletConnect} class="p-1 rounded mr-4 text-m bg-red-900 text-white font-semibold hover:scale-[1.05] transition transition-200">
				Connect via WalletConnect
			</button>
			<!-- <button on:click={connectMetamask} class="p-1 rounded-l text-m bg-black text-white font-semibold hover:scale-[1.05] transition transition-200">
				Connect via MetaMask
			</button>
			<button on:click={disconnect} class="p-1 rounded-l text-m bg-black text-white font-semibold hover:scale-[1.05] transition transition-200">
				Disconnect
			</button> -->
		  </div>
		</nav>
	</header>
	
	<div class="flex flex-col p-10 w-full h-screen items-center gap-5 font-mono bg-rose-500/80 rounded-lg">
		<h1 class="text-5xl font-bold">🥢 ChopSticks 🥢</h1>
		<p>A kitchy frontend for whatsabi</p>
		<!-- <div><p>Contract Address: {address}</p></div> -->
	</div>

	<div class="flex flex-col p-10 w-full h-screen items-center gap-5 font-mono bg-green-500/80 rounded-lg">
		<form>   
			<label for="default-search" class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white">Search</label>
			<div class="relative">
				<div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
					<svg aria-hidden="true" class="w-5 h-5 text-gray-500 dark:text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
				</div>
				<input 
					type="search" id="default-search" class="block w-auto desktop-search p-4 pl-10 text-sm text-gray-900 border-1 border-yellow-300/50 border-s-orange-40 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" 
					placeholder="Search contract" required
				>
				<button type="submit" class="text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Search</button>
			</div>
		</form>

		<table class="hover:table-fixed border-collapse border border-slate-400 border-spacing-72 bg-green-500/50">
			<thead>
			<tr>
				<th class="border border-slate-300 px-8">Selectors</th>
				<th class="border border-slate-300 px-8">Type</th>
				<th class="border border-slate-300 px-8">Payable</th>
				<th class="border border-slate-300 px-8">State Mutability</th>
			</tr>
			</thead>
			<tbody>
				{#each selectors as selector, i}
					<tr>
						<td class="border border-slate-300">{selector}</td>
						<td class="border border-slate-300">{abi[i].type}</td>
						<td class="border border-slate-300">{abi[i].payable}</td>
						<td class="border border-slate-300">{abi[i].stateMutability}</td>
					</tr>
				{/each}
			</tbody>
	  	</table>
	</div>

	<!-- WALLET AND PROVIDER -->
	<!-- <section class="flex flex-col p-10 items-start gap-5 border-2 rounded-xl">
		<h1 class="text-2xl text-black font-bold">Wallet and Provider</h1>
		<h2 class="text-xl text-black font-semibold">$connected: {$connected}</h2>
		<h2 class="text-xl text-black font-semibold">$walletAddress: {$walletAddress}</h2>
		<h2 class="text-xl text-black font-semibold">
			$accountChainId: {JSON.stringify($accountChainId)}
		</h2>
	</section> -->

	<!-- Sync contract state -->
	<!-- <section class="flex flex-col p-10 items-start gap-5 border-2 rounded-xl">
		<h1 class="text-2xl text-black font-bold">Sync contract state on block, event or interval</h1>
		<p class="text-md text-black font-semibold">
			Set In '$lib/globalState.ts', will load as long as a store is used from '$lib/stores/state.ts'
			or you import the globalState in the root of your project with 'import "$lib/globalState";'
		</p>
		<h2 class="text-xl text-black font-semibold">
			$balanceOnBlock: {formatEther($balanceOnBlock)} ETH
		</h2>
	</section> -->
	
	<div class="flex flex-col p-10 pl-1 w-full h-screen items-left gap-5 font-mono rounded-lg mb-8">
		<p>
			Made by <span>@chtmorris</span>
			<br>
			Powered by @shazow/whatsabi and 4byte.directory
		</p>
	</div>

</main>

<style>
	:global(body){
		background-image: url("https://www.dropbox.com/s/vo41ctymekx8d17/bg.jpg?raw=1");
		background-repeat:no-repeat;
   		background-size:cover;
	}

	.desktop-search{
		width:610px;
	}
</style>
