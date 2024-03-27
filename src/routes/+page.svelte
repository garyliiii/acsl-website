<script lang="ts">
	import hljs from 'highlight.js/lib/core';
	import { onMount } from 'svelte';
	let hasScrolled = false;
	let index = 0;
	const messages = [
		'out.println("message")',
		'console.log("message");',
		'cout << "message";',
		'print("message")',
		'puts "message"',
		'echo "message"'
	];

	let currentMessage = messages[index];
	let message = 'Welcome to ACSL Club!';
	let displayedMessage = '';

	let typingTimer: number;

	onMount(() => {
		let messageSwitchTimer: number;

		if (!displayedMessage) {
			let i = 0;
			typingTimer = setInterval(
				() => {
					if (i < message.length) {
						displayedMessage = message.substring(0, i + 1);
						i++;
					} else {
						clearInterval(typingTimer);
					}
				},
				Math.random() * 300 + 100
			); // typing speed
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
	function scrollToContent() {
		hasScrolled = true;
		setTimeout(() => {
			const contentElement = document.getElementById('main-content');
			if (contentElement) {
				contentElement.scrollIntoView({ behavior: 'smooth' });
				document.body.style.overflow = 'auto';
				hljs.highlightAll();
			}
		}, 0);
	}
</script>

<div
	class="flex justify-center items-center font-roboto responsive-large-text transition-all duration-1000 text-center leading-tight p-5 h-full flex-col"
>
	<div>
		{#if displayedMessage}
			{#each currentMessage.split('message') as part, i}
				{#if i > 0}
					<span
						class="bg-clip-text text-transparent bg-gradient-to-r from-orange-400 via-pink-500 to-blue-500 text-enhanced-readability"
						>{displayedMessage}<span class="typing-cursor"></span></span
					>{part}
				{:else}
					{part}
				{/if}
			{/each}
		{/if}
	</div>
	<button
		on:click={scrollToContent}
		class={hasScrolled ? 'hidden' : 'btn variant-ghost-primary mt-32'}
	>
		<i class="fi fi-rr-caret-down"></i>
	</button>
</div>

{#if hasScrolled}
	<div class="m-5 p-4">
		<h1 id="main-content">What is ACSL Club</h1>
		<p>
			ACSL Club is a club for students who are interested in computer science and programming. We
			meet every week to learn new programming concepts and solve problems. We also participate in
			the American Computer Science League (ACSL) competition.
		</p>
		<h1>Who can join?</h1>
		<p>
			Our club is open to all students who are interested in computer science, regardless of their
			experience level. We welcome beginners who are just starting to learn programming, as well as
			more experienced students who want to improve their skills.
		</p>
		<h1>How does the competition work?</h1>
		<p>
			ACSL test consist of 5 computer science questions that are to be solved in 30 minutes and a
			programming question that are to be solved in 3 days(it doesnt take 3 days) ACSL uses java,
			C++, and python 3 for the programming questions. The questions are designed to test your
			problem-solving skills and your ability to write clean, efficient code.
		</p>
		<h1>What would I learn?</h1>
		<p>
			We will teach you computer science concepts and programming. Take a look at the code below to
			see an example of how we solve a problem in the ACSL
		</p>

		<!-- highlight js -->
		<pre>
			<code class="language-java">
				{`
import java.util.*;



class Result {
    private static String sentence [];
    private static String words[][];
    public static String encodeMessage(String text, String message) {
        //break up the text to sentence
        sentence = text.split("(\\.|\\?|!)  ");
        words = new String[sentence.length][];
        for(int i = 0; i < sentence.length; i ++){
            words[i] = Arrays.stream(sentence[i].split("[^a-zA-Z0-9]"))
                    .filter(s->!(s.trim().isEmpty())).toArray(String[]::new);
        }
        StringBuilder build = new StringBuilder();
        char msg [] = message.toCharArray();
        int accountletter = 0; //accounts for the offset, because space and special character is not a letter
        for(int k = 0; k < msg.length; k++){
            char c = msg[k];
            if(c == ' '){
                build.append("_");
                accountletter++;
                continue;
            }
            if(Character.isLetterOrDigit(c)){
                int [] find = nthOccurence(k-accountletter + 1, c); //index at 1
                build.append(find[0]).append(".").append(find[1]).append(".").append(find[2]);
                if(Character.isLetterOrDigit(msg[k + 1]))build.append(" ");
                continue;
            }
            build.append(c);
            accountletter++;
        }

        return build.toString();
    }

    private static int [] nthOccurence(int n, char c){
        int oc= 0; //occurence count
        for(int i = 0; i < sentence.length; i ++){
            String [] cache = words[i];
            for(int j = 0; j<cache.length; j++){
                for(int k = 0; k<cache[j].length(); k++){
                    if(cache[j].charAt(k) == c){
                        oc++;
                        if(oc == n){
                            return new int[]{i + 1,j + 1 ,k + 1}; //index start at 1
                        }
                    }
                }
            }
        }
        //divide by half
        return nthOccurence(n/2, c);
    }
}
				`}
			</code>
		</pre>
	</div>

	<div class="flex flex-col items-center">
		<h1>So what are you waiting for? Join ACSL Club today!</h1>
		<button class="btn btn-sm variant-ghost-surface m-8">
			<a href="https://discord.gg/swT6yFGnYb" target="_blank" rel="noreferrer">
				Join our Discord server!!!
			</a>
		</button>
	</div>
{/if}

<style>
	h1 {
		font-size: 3em;
		@apply text-primary-500;
		margin-bottom: 0.5em;
		line-height: 1.5;
	}

	p {
		font-size: 1.5em;
		@apply text-gray-700;
		line-height: 1.5;
	}

	@keyframes blink {
		0%,
		100% {
			opacity: 1;
		}
		50% {
			opacity: 0;
		}
	}

	.typing-cursor {
		opacity: 1;
		animation: blink 1s infinite;
		border-right: 4px solid #000;
	}

	.responsive-large-text {
		font-size: 4.5vw;
	}

	.text-enhanced-readability {
		text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
	}
</style>
