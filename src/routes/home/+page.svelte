<script lang="ts">
  import anime from 'animejs';

	interface Note {
    content: string;
    left: boolean;
	}

	function visibleInViewport(elem: Element, left: boolean) {
		const observer = new IntersectionObserver(
			(entries, observer) => {
				let entry = entries[0];
				if (!entry.isIntersecting) {
          return
        }
        
        anime({
          targets: entry.target,
          translateX: [left ? -noteXOffset : noteXOffset, 0],
          opacity: 1,
          duration: 500,
          delay: 0,
          rotate: [left ? 180 : -180, 0],
          //easing: 'easeOutElastic(1, 0.6)',
          easing: 'spring(0.9, 100, 13, 5)',
          scale: [0.5, 1]
        })

        // We don't care about anim anymore >:(
        observer.unobserve(entry.target);
			},
			{ threshold: 0.2 }
		);

		observer.observe(elem);
	}

	let notes: Note[] = [];
	for (let i = 0; i < 200; i++) {
		notes.push({
      content: `${i}: Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.`,
      left: i % 2 == 0
    });
	}

  let noteXOffset = 200;
</script>

<body>
	<div class="note-container">
		{#each notes as {content, left}}
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
  :global(body) {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.note {
		display: flex;
		border-radius: 2em;
		border: 1px solid;
		box-shadow: 0px 0px 3px;
		padding: 20px 30px;
		width: 20vw;

		/*word-break: break-all;*/
    word-wrap: break-word;

		flex-direction: column;
		align-items: center;
    min-width: 200px;
    min-height: 300px;

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
</style>
