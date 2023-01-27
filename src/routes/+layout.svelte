<script>
	import '../app.postcss';

	import { onMount } from 'svelte';
	import { isLoggedIn } from '../lib/tools/stores';
	import { goto } from '$app/navigation';

	import firebase from '../firebase/firebase';
	import { getAuth, onAuthStateChanged } from 'firebase/auth';

	import Navbar from '../lib/components/Navbar.svelte';

	onMount(() => {
		const auth = getAuth();
		onAuthStateChanged(auth, (user) => {
			if (user) {
				console.log('Welcome to Fireplace 🔥');
				isLoggedIn.update(() => true);
			} else {
				goto('/login');
				isLoggedIn.update(() => false);
			}
		});
	});
</script>

<Navbar />
<slot />
