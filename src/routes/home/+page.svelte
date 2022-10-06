<script lang="ts">
  import { onMount } from 'svelte';
  import anime from 'animejs';

	interface Note {
    content: string;
    left: boolean;
    right: boolean;
	}

	function visibleInViewport(elem: Element, left: boolean) {
		const observer = new IntersectionObserver(
			(entries, observer) => {
				let entry = entries[0];
				if (!entry.isIntersecting) {
          return
        }
        
        console.log(anime.get(entry.target, 'translateX'))
        anime({
          targets: entry.target,
          translateX: [left ? -300 : 300, 0],
          opacity: 1,
          duration: 500,
          delay: 0,
          rotate: [0, 360],
          easing: "easeInOutSine"
        })
        observer.unobserve(entry.target);
			},
			{ threshold: 0.2 }
		);

		observer.observe(elem);
	}

	let notes: Note[] = [];
	for (let i = 0; i < 200; i++) {
		notes.push({
      content: `Sussy chungus ${i}`,
      left: i % 2 == 0, right: i % 2 == 1
    });
	}
</script>

<body>
	<div class="note-container">
		{#each notes as {content, left, right}}
			<div
				use:visibleInViewport={left}
        class="note"
			>
				<p>{content}</p>
				<div class="button-group">
					<button>Suss</button>
					<button>Big chungus</button>
				</div>
			</div>
		{/each}
	</div>
</body>

<style>
  :root {
    --note-x-offset: 300px;
  }

	.note {
		display: flex;
		border-radius: 2em;
		border: 1px solid;
		box-shadow: 0px 0px 3px;
		padding: 20px 30px;
		width: 20vw;

		word-break: break-all;

		flex-direction: column;
		align-items: center;
    min-width: 200px;

    scale: 1;
		opacity: 0;
		transition: all 300ms cubic-bezier(.29,.74,.39,.91);
	}

	.note button {
		border: 1px solid;
		border-radius: 0.4em;
		box-shadow: 0px 0px 3px;
		padding: 0.5em;

		transition: background-color 200ms, transform 150ms;
	}

	.note button:hover {
		background-color: #ccc;
		transform: scale(1.1);
	}

	.note-container {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 30px 60px;
		padding: 20px;
	}

	.button-group {
		display: flex;
		gap: 1.5em;
		margin-top: auto;
	}

	:global(body) {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

  .note.left {
    transform: translateX(calc(var(--note-x-offset) * -1px));
  }

  .note.right {
    transform: translateX(var(--note-x-offset));
  }
</style>
