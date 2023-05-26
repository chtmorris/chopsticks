<script>
	import './main.css';
	import { ethers } from 'ethers';
	import { whatsabi } from "@shazow/whatsabi";
   
	const provider = new ethers.providers.Web3Provider(window.ethereum);
  
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
		console.log("Loggin abi: " + abi);
		
	}); // Load the bytecode

</script>

<main>
	<h1>🥢 ChopSticks 🥢</h1>
	<p>A friendly frontend for whatsabi</p>
	<div><p>Contract Address: {address}</p></div>
	<div><p>Selectors: {selectors}</p></div>
	<div><p>ABI: {abi}</p></div>
</main>