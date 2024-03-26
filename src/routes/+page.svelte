<script lang="ts">
	import { onMount } from 'svelte';
  
	let index = 0;
	const messages = [
	  'out.println("message")',
	  'console.log("message");',
	  'cout << "message";',
	  'print("message")',
	  'puts "message"',
	  'echo "message"',
	];
  
	let currentMessage = messages[index];
	let message = 'Welcome to ACSL Club!';
	let displayedMessage = '';
  
	let typingTimer: number;
  
	onMount(() => {
	  let messageSwitchTimer: number;
  
	  if (!displayedMessage) {
		let i = 0;
		typingTimer = setInterval(() => {
		  if (i < message.length) {
			displayedMessage = message.substring(0, i + 1);
			i++;
		  } else {
			clearInterval(typingTimer);
		  }
		}, Math.random() * 300 + 100); // typing speed
	  }
  
	  messageSwitchTimer = setInterval(() => {
		index = (index + 1) % messages.length;
		currentMessage = messages[index];
	  }, 5000);
  
	  return () => {
		clearInterval(typingTimer);
		clearInterval(messageSwitchTimer);
	  };
	});
  </script>


<style>
	@keyframes blink {breaks
	  to {
		opacity: 0;
	  }
	}
  
	.typing-cursor {
	  opacity: 1;
	  animation: blink 1s steps(5, start) infinite;
	  /* Make the cursor darker and slightly thicker for visibility */
	  border-right: 4px solid #000; /* Darker color */
	}
  
	/* Responsive text size adjustments */
	.responsive-large-text {
	  font-size: 4.5vw;
	}
  
	/* Enhanced readability with text shadow */
	.text-enhanced-readability {
	  text-shadow: 0 2px 4px rgba(0,0,0,0.3);
	}	
  
	/* Adjustments for very small and very large screens */
	@media (max-width: 600px) {
	  .responsive-large-text {
		font-size: 5.5vw;
	  }
	}
  
	@media (min-width: 1200px) {
	  .responsive-large-text {
		font-size: 3.5rem; /* Cap the size on large screens */
	  }
	}
  </style>
  
  <div class="flex justify-center items-center h-screen font-roboto responsive-large-text transition-all duration-1000 text-center leading-tight p-5">
	{#if displayedMessage}
	  {#each currentMessage.split('message') as part, i}
		{#if i > 0}
		  <span class="bg-clip-text text-transparent bg-gradient-to-r from-orange-400 via-pink-500 to-blue-500 text-enhanced-readability">{displayedMessage}<span class="typing-cursor"></span></span>{part}
		{:else}
		  {part}
		{/if}
	  {/each}
	{/if}
  </div>
  