<script>
	import { http_host, API_BASE } from '$lib/xs/config.json';
	import { toast } from '@zerodevx/svelte-toast';

	let payload = {
		email: '',
		username: '',
		password: ''
	};

	// @ts-ignore
	const handleChange = (event) => {
		Object.defineProperty(payload, event.target.name, {
			value: event.target.value,
			// @ts-ignore
			writeable: true
		});
	};
	// @ts-ignore
	const handleSubmit = (event) => {
		event.preventDefault();
	};

	function Register() {
		fetch(`${http_host}${API_BASE}auth/register`, {
			headers: {
				Accept: 'application/json',
				'Content-Type': 'application/json'
			},
			method: 'POST',
			body: JSON.stringify(payload)
		})
			.then((res) => res.json())
			.then(function (json) {
				if (!json.status) {
					return toast.push(`${json.message}`, {
						dismissable: false,
						theme: {
							'--toastBarBackground': 'red'
						}
					});
				}
				toast.push('Account Created. Redirecting to login page.');
				localStorage.setItem('token', json.token);
				window.location.href = '/auth/login';
			})
			.catch(function (res) {
				console.log(res);
			});
	}
</script>

<main
	class="flex h-screen flex-wrap items-center justify-center bg-gray-200 text-white"
	style="background: #0E1C36;"
>
	<form
		class="flex flex-wrap items-center justify-center"
		on:submit={(event) => handleSubmit(event)}
	>
		<div class="invisiblewrap flex w-full basis-full flex-wrap items-center justify-center">
			<div class="smartwrapper flex max-w-fit basis-full flex-wrap items-center justify-center">
				<div class="text-5xl font-semibold">Register</div>
				<div
					class="field email my-4 mx-4 flex max-w-xl basis-full flex-wrap items-center justify-center rounded bg-gray-800 bg-opacity-80 px-2 py-2"
				>
					<label for="email" class="basis-full px-2">Email</label>
					<input
						class="h-8 w-full rounded-sm bg-gray-800 bg-opacity-20 px-2"
						type="email"
						placeholder="Email"
						name="email"
						on:change={(event) => handleChange(event)}
					/>
				</div>

				<div
					class="field username my-4 mx-4 flex w-80 max-w-xl basis-full flex-wrap items-center justify-center rounded bg-gray-800 bg-opacity-80 px-2 py-2"
				>
					<label for="username" class="basis-full px-2">Username</label>
					<input
						class="h-8 w-full rounded-sm bg-gray-800 bg-opacity-20 px-2"
						type="text"
						placeholder="Array0x"
						name="username"
						on:change={(event) => handleChange(event)}
					/>
				</div>
			</div>
		</div>
		<div
			class="field password my-4 mx-4 flex w-80 max-w-xl basis-full flex-wrap items-center justify-center rounded bg-gray-800 bg-opacity-80 px-2 py-2"
		>
			<label for="password" class="basis-full px-2">Password</label>
			<input
				class="h-8 w-full rounded-sm bg-gray-800 bg-opacity-20 px-2"
				type="password"
				placeholder="supersecretphrase123"
				name="password"
				on:change={(event) => handleChange(event)}
			/>
		</div>
		<div class="footer flex w-full basis-full flex-wrap items-center justify-center text-center">
			<button
				on:click={() => {
					Register();
				}}
				class="max-w-xs basis-full rounded-md bg-secondary px-2 py-4 hover:opacity-80"
				style="background: #0070e8;">Create Account</button
			>
			<div class="my-4 flex basis-full flex-wrap items-center justify-center">
				<span class="basis-full">Already have an Account? </span>
				<a href="/auth/login" class="font-base text-blue-400 underline hover:text-blue-500">
					<span>Login</span>
				</a>
			</div>
		</div>
		<!-- <ToastContainer /> -->
	</form>
</main>
