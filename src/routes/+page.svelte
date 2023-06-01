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
		debugger
		console.log("Loggin abi: " + abi);
		
		
	}); // Load the bytecode
</script>

<main class="flex flex-col p-10 w-screen h-screen items-center gap-5 font-serif">

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

	<h1 class="text-5xl text-red font-bold">🥢 ChopSticks 🥢</h1>
	<p>A friendly frontend for whatsabi</p>
	<div><p>Contract Address: {address}</p></div>

	<table class="hover:table-fixed border-collapse border border-slate-400 border-spacing-72">
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
	
</main>