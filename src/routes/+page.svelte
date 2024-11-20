<script lang="ts">
	// Automatically focuses on the first input field
	$effect(() => document.getElementById('Input0')?.focus());

	let numbers = $state(Array(6).fill(''));

	const handleInput = (index: number, value: string) => {
		numbers[index] = value || '';
		if (value && index < 5) document.getElementById(`Input${index + 1}`)?.focus();
	};

	const handleKeyNavigation = (index: number, event: KeyboardEvent) => {
		const { key } = event;

		if (key === 'Backspace') {
			if (!numbers[index] && index > 0) document.getElementById(`Input${index - 1}`)?.focus();
			numbers[index] = '';
		} else if (key === 'ArrowLeft' && index > 0) {
			document.getElementById(`Input${index - 1}`)?.focus();
		} else if (key === 'ArrowRight' && index < 5) {
			document.getElementById(`Input${index + 1}`)?.focus();
		}
	};

	// Checks OTP validity
	const otpCheck = () => {
		const isComplete = numbers.every((n) => n !== '');
		const isCorrect = numbers.join('') === '666777';
		return { isComplete, isCorrect };
	};
</script>

<div class="flex h-screen w-screen items-center justify-center bg-[#E4F4FF] font-sans">
	<div
		class={[
			'flex flex-col items-center justify-center rounded-xl bg-white p-8 shadow-xl',
			otpCheck().isComplete && !otpCheck().isCorrect ? 'animate-shake' : ''
		].join(' ')}
	>
		<div
			class={`flex h-24 w-24 items-center justify-center rounded-full ${
				otpCheck().isComplete
					? otpCheck().isCorrect
						? 'bg-green-100 text-green-500'
						: 'bg-[#FEF7F6] text-rose-500'
					: 'bg-[#E8F5FF] text-[#3C8AC6]'
			} ${otpCheck().isComplete ? 'animate-lock-success' : ''}`}
		>
			{#if otpCheck().isComplete && otpCheck().isCorrect}
				<svg
					fill="#22c55e"
					width="30"
					height="30"
					viewBox="0 0 32 32"
					version="1.1"
					xmlns="http://www.w3.org/2000/svg"
				>
					<path
						d="M25 12.034l-14.28 0-0.518-2.321c-0.883-3.293 0.65-6.576 4.159-7.516 3.473-0.93 6.534 1.061 7.432 4.41l0.425 1.686c0.143 0.534 0.691 0.85 1.225 0.707s0.85-0.691 0.707-1.225l-0.425-1.687c-1.187-4.433-5.325-7.045-9.881-5.824-4.574 1.226-6.741 5.607-5.573 9.966l0.402 1.803h-1.673c-2.206 0-4 1.794-4 4v12c0 2.206 1.794 4 4 4h18c2.206 0 4-1.794 4-4v-12c0-2.206-1.794-4-4-4zM27 28.035c0 1.102-0.898 2-2 2h-18c-1.103 0-2-0.898-2-2v-12c0-1.102 0.897-2 2-2h18c1.102 0 2 0.898 2 2v12zM16 18.035c-1.104 0-2 0.895-2 2 0 0.738 0.405 1.376 1 1.723v3.277c0 0.552 0.448 1 1 1s1-0.448 1-1v-3.277c0.595-0.346 1-0.985 1-1.723 0-1.105-0.895-2-2-2z"
					></path>
				</svg>
			{:else}
				<svg
					fill={otpCheck().isComplete ? '#f43f5e' : '#3C8AC6'}
					width="30"
					height="30"
					viewBox="0 0 32 32"
				>
					<path
						d="M25 12h-1v-3.816c0-4.589-3.32-8.184-8.037-8.184-4.736 0-7.963 3.671-7.963 8.184v3.816h-1c-2.206 0-4 1.794-4 4v12c0 2.206 1.794 4 4 4h18c2.206 0 4-1.794 4-4v-12c0-2.206-1.794-4-4-4zM10 8.184c0-3.409 2.33-6.184 5.963-6.184 3.596 0 6.037 2.716 6.037 6.184v3.816h-12v-3.816zM27 28c0 1.102-0.898 2-2 2h-18c-1.103 0-2-0.898-2-2v-12c0-1.102 0.897-2 2-2h18c1.102 0 2 0.898 2 2v12zM16 18c-1.104 0-2 0.895-2 2 0 0.738 0.405 1.376 1 1.723v3.277c0 0.552 0.448 1 1 1s1-0.448 1-1v-3.277c0.595-0.346 1-0.985 1-1.723 0-1.105-0.895-2-2-2z"
					></path>
				</svg>
			{/if}
		</div>
		<div class="mt-4 text-4xl font-semibold">Easy peasy</div>
		<div class="mt-4 text-xs text-[#7A8496]">
			Enter 6-digit code from your two-factor authenticator APP.
		</div>
		<div class="mt-8 flex gap-2">
			{#each numbers as _, index}
				<div
					class={`flex h-12 w-10 items-center justify-center rounded-lg border px-3 text-2xl ${
						otpCheck().isComplete && !otpCheck().isCorrect
							? 'border-rose-500 bg-[#FEF7F6] text-rose-500'
							: 'text-[#3C8AC6]  focus-within:border-[#3C8AC6]'
					}`}
				>
					<input
						id={`Input${index}`}
						class={`no-cursor w-4 border-[#3C8AC6] focus:border-b focus:outline-none `}
						type="number"
						maxlength="1"
						oninput={(event: any) => {
							const value = event.target?.value.slice(0, 1); // Keep only the first digit
							handleInput(index, value);
							event.target.value = value; // Ensure UI consistency
						}}
						onkeydown={(event) => handleKeyNavigation(index, event)}
					/>
					{#if index === 2}<div class="w-2"></div>{/if}
				</div>
			{/each}
		</div>
		<button
			class={`mt-6 !w-full rounded-lg py-3 transition-all duration-300 ease-in-out ${
				otpCheck().isComplete
					? otpCheck().isCorrect
						? 'bg-blue-600 text-white'
						: 'animate-shake bg-red-600 text-white'
					: 'bg-[#ECEEF1] text-[#798694]'
			}`}
		>
			{#if otpCheck().isComplete}
				{#if otpCheck().isCorrect}
					Let's go!
				{:else}
					Wrong code!
				{/if}
			{:else}
				{6 - numbers.filter((n) => n !== '').length} digits left
			{/if}
		</button>
	</div>
</div>

<style>
	.no-cursor {
		caret-color: transparent;
	}

	@keyframes lock-success {
		0% {
			transform: scale(1);
		}
		50% {
			transform: scale(1.1);
		}
		100% {
			transform: scale(1);
		}
	}
	.animate-lock-success {
		animation: lock-success 0.5s;
	}

	@keyframes shake {
		0% {
			transform: translate(1px, 1px) rotate(0deg);
		}
		10% {
			transform: translate(-1px, -2px) rotate(-1deg);
		}
		20% {
			transform: translate(-3px, 0px) rotate(1deg);
		}
		30% {
			transform: translate(3px, 2px) rotate(0deg);
		}
		40% {
			transform: translate(1px, -1px) rotate(1deg);
		}
		50% {
			transform: translate(-1px, 2px) rotate(-1deg);
		}
		60% {
			transform: translate(-3px, 1px) rotate(0deg);
		}
		70% {
			transform: translate(3px, 1px) rotate(-1deg);
		}
		80% {
			transform: translate(-1px, -1px) rotate(1deg);
		}
		90% {
			transform: translate(1px, 2px) rotate(0deg);
		}
		100% {
			transform: translate(1px, -2px) rotate(-1deg);
		}
	}

	.animate-shake {
		animation: shake 0.5s;
	}

	input[type='number']::-webkit-inner-spin-button,
	input[type='number']::-webkit-outer-spin-button {
		-webkit-appearance: none;
		margin: 0;
	}
</style>
