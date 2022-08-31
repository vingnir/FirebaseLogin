<script lang="ts">
	import { initializeApp } from 'firebase/app';
	import {
		getAuth,
		signInWithEmailAndPassword,
		signOut,
		type User,
		onAuthStateChanged,
		signInWithPopup,
		GoogleAuthProvider,
		GithubAuthProvider
	} from 'firebase/auth';
	import { onMount } from 'svelte';

	let email = '';
	let password = '';
	let user: User | null;

	const firebaseConfig = {
		apiKey: "AIzaSyAnUpp3NgwYiO1apm8St7pe6Svu3SbfdSM",
		authDomain: "groowe-9b24a.firebaseapp.com",
		projectId: "groowe-9b24a",
		storageBucket: "groowe-9b24a.appspot.com",
		messagingSenderId: "325037951480",
		appId: "1:325037951480:web:15b4ddce74067b2c19ae0d",
		measurementId: "G-P72VL5JS3Y"
	};

	const app = initializeApp(firebaseConfig);

	const login = () => {
		const auth = getAuth(app);
		signInWithEmailAndPassword(auth, email, password).catch((error) => {
			const errorCode = error.code;
			const errorMessage = error.message;
			console.log(errorCode, errorMessage);
		});
	};

	const loginWithGoogle = () => {
		const auth = getAuth(app);
		signInWithPopup(auth, new GoogleAuthProvider());
	};

	const loginWithGithub = () => {
		const auth = getAuth(app);
		signInWithPopup(auth, new GithubAuthProvider());
	};

	const logout = async () => {
		const auth = getAuth(app);
		signOut(auth);
	};

	onMount(async () => {
		const auth = getAuth(app);
		onAuthStateChanged(auth, (newUser) => {
			console.log(user);
			user = newUser;
		});
	});
</script>

{#if user}
	<p>Signed in with {user.providerData[0].providerId}!</p>
	<button on:click={logout}>Logout</button>
{:else}
	<input type="email" id="email" placeholder="email" bind:value={email} />
	<input type="password" id="password" placeholder="password" bind:value={password} />
	<button on:click={login}>Login</button>
	<button on:click={loginWithGoogle}>Login with Google</button>
	<button on:click={loginWithGithub}>Login with Github</button>
{/if}
