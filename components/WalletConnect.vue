<!-- Please remove this file from your project -->
<template>
  <div class="relative flex items-top justify-center md:min-h-screen bg-gray-100 md:items-center sm:pt-0 pb-4 md:pb-0">
    <div class="max-w-xl mx-auto sm:px-6 lg:px-8">
      <div class="mt-8 md:bg-white overflow-hidden md:shadow md:rounded-lg p-3 py-6">
        <h1 class="lg:w-1/2 text-4xl text-gray-900 tracking-tight leading-10 font-bold font-serif sm:text-5xl sm:leading-none md:text-6xl xl:pr-2">
          Say Hi! <br><span class="text-transparent bg-clip-text bg-gradient-to-br from-yellow-400 to-red-600">Ethereum + Nuxt</span>
        </h1>
        <p class="mt-3 text-lg text-gray-600 mb-8">
          Hey there! I´m Jean Ayala and this is my first project using ethereum. Connect you wallet and say hi 🖖 to the world!
        </p>
        <button v-if="hasMetamask === null" disabled class="w-full md:w-auto py-2 px-4 rounded border border-yellow-600 text-yellow-600 font-semibold text-lg shadow-md cursor-default">Checking Metamask...</button>
        <button v-else-if="hasMetamask && currentAccount === null" class="w-full md:w-auto py-2 px-4 rounded bg-yellow-600 hover:bg-yellow-400 text-white font-semibold text-lg shadow-md" @click="connectWallet()">Connect Wallet</button>
        <div v-else-if="hasMetamask && currentAccount !== null">
          <input
            v-model.trim="message"
            type="text"
            class="mb-4 border-2 border-gray-300 bg-white h-10 px-3 rounded text-sm focus:outline-none w-full"
            placeholder="Message"
          />
          <button class="w-full md:w-auto py-2 px-4 rounded bg-yellow-600 hover:bg-yellow-400 text-white font-semibold text-lg shadow-md" @click="sayHi()">{{ mining ? 'Sending...' : 'Say hi! 🖖' }}</button>
        </div>
        <div v-else-if="!hasMetamask" class="flex flex-wrap">
          <p class="w-full md:w-auto py-1 px-2 text-red-600 font-semibold text-lg text-center">You need Metamask to access</p>
          <a href="https://metamask.io/" target="_blank" class="w-full md:w-auto py-1 px-2 font-semibold text-lg underline text-center md:text-left md:ml-2 mt-4 md:mt-0">Get Metamask</a>
        </div>
      </div>
      <div class="flex justify-center -mt-2 md:mt-0 md:pt-4 space-x-2">
        <a href="https://github.com/mrsalitre/" target="_blank"><svg
          class="w-6 h-6 text-gray-600 hover:text-gray-800"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          aria-hidden="true"
          role="img"
          width="32"
          height="32"
          preserveAspectRatio="xMidYMid meet"
          viewBox="0 0 24 24"
        ><path d="M12 2.247a10 10 0 0 0-3.162 19.487c.5.088.687-.212.687-.475c0-.237-.012-1.025-.012-1.862c-2.513.462-3.163-.613-3.363-1.175a3.636 3.636 0 0 0-1.025-1.413c-.35-.187-.85-.65-.013-.662a2.001 2.001 0 0 1 1.538 1.025a2.137 2.137 0 0 0 2.912.825a2.104 2.104 0 0 1 .638-1.338c-2.225-.25-4.55-1.112-4.55-4.937a3.892 3.892 0 0 1 1.025-2.688a3.594 3.594 0 0 1 .1-2.65s.837-.262 2.75 1.025a9.427 9.427 0 0 1 5 0c1.912-1.3 2.75-1.025 2.75-1.025a3.593 3.593 0 0 1 .1 2.65a3.869 3.869 0 0 1 1.025 2.688c0 3.837-2.338 4.687-4.563 4.937a2.368 2.368 0 0 1 .675 1.85c0 1.338-.012 2.413-.012 2.75c0 .263.187.575.687.475A10.005 10.005 0 0 0 12 2.247z" fill="currentColor" /></svg></a>
        <a href="https://twitter.com/mrsalitre" target="_blank"><svg
          class="w-6 h-6 text-gray-600 hover:text-gray-800"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          aria-hidden="true"
          role="img"
          width="32"
          height="32"
          preserveAspectRatio="xMidYMid meet"
          viewBox="0 0 24 24"
        ><path d="M22.46 6c-.77.35-1.6.58-2.46.69c.88-.53 1.56-1.37 1.88-2.38c-.83.5-1.75.85-2.72 1.05C18.37 4.5 17.26 4 16 4c-2.35 0-4.27 1.92-4.27 4.29c0 .34.04.67.11.98C8.28 9.09 5.11 7.38 3 4.79c-.37.63-.58 1.37-.58 2.15c0 1.49.75 2.81 1.91 3.56c-.71 0-1.37-.2-1.95-.5v.03c0 2.08 1.48 3.82 3.44 4.21a4.22 4.22 0 0 1-1.93.07a4.28 4.28 0 0 0 4 2.98a8.521 8.521 0 0 1-5.33 1.84c-.34 0-.68-.02-1.02-.06C3.44 20.29 5.7 21 8.12 21C16 21 20.33 14.46 20.33 8.79c0-.19 0-.37-.01-.56c.84-.6 1.56-1.36 2.14-2.23z" fill="currentColor" /></svg></a>
      </div>
    </div>
  </div>
</template>
<script>
import { ethers } from "ethers";
import abi from '../static/SayHi.v0.0.1.json'

export default {
  data() {
    return {
      hasMetamask: null,
      currentAccount: null,
      message: '',
      mining: false,
    }
  },
  async mounted () {
    try {
      const { ethereum } = window;

      if (!ethereum) {
        this.hasMetamask = false;
        return;
      } else {
        this.hasMetamask = true;
      }

      const accounts = await ethereum.request({ method: 'eth_accounts' });

      if (accounts.length !== 0) {
        const account = accounts[0];
        this.currentAccount = account;
      } else {
        console.log("No authorized account found")
      }
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    async connectWallet() {
      try {
        const { ethereum } = window;
  
        if (!ethereum) {
          return;
        }
  
        const accounts = await ethereum.request({ method: "eth_requestAccounts" });
  
        this.currentAccount = accounts[0]; 
      } catch (error) {
        console.log(error)
      }
    },
    async sayHi() {
      if (!this.message.length) {
        return;
      }
      const contractAddress = '0xE48c9428CFADCe4448D52338f1EbB1eB7F656B46'
      const contractABI = abi.abi
      try {
        const { ethereum } = window;

        if (ethereum) {
          const provider = new ethers.providers.Web3Provider(ethereum);
          const signer = provider.getSigner();
          const wavePortalContract = new ethers.Contract(contractAddress, contractABI, signer);

          const waveTxn = await wavePortalContract.sayHi(this.message, { gasLimit: 300000 });
          this.mining = true;
  
          await waveTxn.wait();
          this.mining = false;

          this.message = '';
          this.$emit('message-sent');
        } else {
          console.log("Ethereum object doesn't exist!");
        }
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>