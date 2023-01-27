<script>
	import {
		Navbar,
		NavBrand,
		Avatar,
		Dropdown,
		DropdownItem,
		DropdownHeader,
		DropdownDivider
	} from 'flowbite-svelte';

	import Icon from '@iconify/svelte';

	import { goto } from '$app/navigation';
	import { getAuth, GoogleAuthProvider, signOut } from 'firebase/auth';

	const handleSignOut = () => {
		const auth = getAuth();
		signOut(auth)
			.then(() => {
				localStorage.removeItem('uid');
				console.log('Logging Out');
				goto('/login');
			})
			.catch((err) => {
				console.error(err);
			});
	};
</script>

<nav
	class="relative w-full flex flex-wrap items-center justify-between py-4 bg-gray-100 text-gray-500 hover:text-gray-700 focus:text-gray-700 shadow-lg"
>
	<div class="container-fluid w-full flex flex-wrap items-center justify-between px-6">
		<NavBrand>
			<Icon icon="mdi:fire" color="orange" width="32" />
			<span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white"
				>Fireplace</span
			>
		</NavBrand>
		<div class="flex items-center md:order-2">
			<Avatar id="avatar-menu" src="" alt="dp" />
		</div>
		<Dropdown placement="bottom" triggeredBy="#avatar-menu">
			<DropdownHeader>
				<span class="block text-sm"> Bonnie Green </span>
				<span class="block truncate text-sm font-medium"> name@flowbite.com </span>
			</DropdownHeader>
			<DropdownItem>Dashboard</DropdownItem>
			<DropdownItem>Settings</DropdownItem>
			<DropdownItem>Earnings</DropdownItem>
			<DropdownDivider />
			<DropdownItem on:click={handleSignOut}>Sign out</DropdownItem>
		</Dropdown>
	</div>
</nav>
