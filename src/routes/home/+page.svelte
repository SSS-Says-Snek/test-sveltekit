<script lang="ts">
	interface Note {
		visible: boolean;
	}

	function actionWhenInViewport(e: Element) {
		const observer = new IntersectionObserver(
			(entries, observer) => {
				let entry = entries[0];

				if (entry.isIntersecting) {
					console.log('OMG!!');
					e.dispatchEvent(new CustomEvent('visible'));
					observer.unobserve(entry.target);
				}
			},
			{ threshold: 0.2 }
		);

		observer.observe(e);
	}

	function toggleVisibility(id: number) {
		notes[id].visible = notes[id].visible ? false : true;
	}

	let notes: Note[] = [];
	for (let i = 0; i < 20; i++) {
		notes.push({ visible: false });
	}
</script>

<body>
	<div class="note-container">
		{#each notes as array, id}
			<div
				class="note"
				use:actionWhenInViewport
				on:visible="{() => toggleVisibility(id)}"
				class:visible={array.visible}
			>
				<p>Sussy chungus</p>
				<div class="button-group">
					<button>Suss</button>
					<button>Big chungus</button>
				</div>
			</div>
		{/each}
	</div>
</body>

<style>
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

		opacity: 0;
		transform: translateX(100px);
		transition: all 500ms;
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
		gap: 40px 30px;
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
		overflow-x: hidden;
	}

	.note.visible {
		opacity: 1;
		transform: translateX(0);
	}
</style>
