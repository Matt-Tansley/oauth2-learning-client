<script>
	import axios from 'axios';
	import { PUBLIC_CLIENT_ID } from '$env/static/public';

	let count = $state(0);

	let loggedIn = $state(false);
	let user = $state(null);

	const signInURL = `https://github.com/login/oauth/authorize?client_id=${PUBLIC_CLIENT_ID}&redirect_uri=http://localhost:8080/oauth/redirect`;

	$effect(() => {
		const token = new URLSearchParams(window.location.search).get('access_token');

		axios
			.get('http://localhost:8010/proxy/user', {
				headers: {
					Authorization: 'token ' + token
				}
			})
			.then((res) => {
				user = res.data;
				loggedIn = true;
			})
			.catch((error) => {
				console.log('ERROR: ' + error);
			});
	});
</script>

<main class="container">
	<section>
		<h1>Welcome to SvelteKit</h1>
		<p>
			Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation
		</p>

		<button onclick={() => count++}>
			clicks: {count}
		</button>
	</section>

	<section class="center card">
		{#if !loggedIn}
			<img
				src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png"
				width="150"
				alt="github logo"
			/>
			<h1 class="black-heading">Sign in with GitHub</h1>
			<a role="button" href={signInURL}> Sign in </a>
		{:else}
			<h1 class="black-heading">Hello, world!</h1>
			<p class="black-heading">
				This is a simple integration between OAuth2 on GitHub with Node.js
			</p>

			<article>
				<img src={user.avatar_url} alt="github user avatar" />
				<h2>{user.name}</h2>
				<h2>{user.bio}</h2>
				<a role="button" href={user.html_url}>GitHub Profile</a>
			</article>
		{/if}
	</section>
</main>
