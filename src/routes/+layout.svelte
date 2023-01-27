<script>
	import '../app.postcss';

	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

	import firebase from '../firebase/firebase';
	import { getAuth, onAuthStateChanged } from 'firebase/auth';

	import Navbar from '../lib/components/Navbar.svelte';

	onMount(() => {
		const auth = getAuth();
		onAuthStateChanged(auth, (user) => {
			if (user) console.log('Welcome');
			else goto('/login');
		});
	});
</script>

<Navbar />
<slot />
