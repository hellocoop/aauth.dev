<script>
	let {
		text,
		revealDirection = 'center',
		speed = 35,
		trigger = 0,
		chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%*+-='
	} = $props();

	let displayed = $state(text);
	let timer = null;

	function buildOrder(n, direction) {
		const idx = Array.from({ length: n }, (_, i) => i);
		if (direction === 'center') {
			const mid = (n - 1) / 2;
			return idx.sort((a, b) => Math.abs(a - mid) - Math.abs(b - mid) || a - b);
		}
		if (direction === 'end') return idx.reverse();
		return idx;
	}

	function randomChar() {
		return chars[Math.floor(Math.random() * chars.length)];
	}

	function scramble() {
		const n = text.length;
		const order = buildOrder(n, revealDirection);
		let step = 0;

		if (timer) clearInterval(timer);
		timer = setInterval(() => {
			const revealed = new Set(order.slice(0, step));
			displayed = text
				.split('')
				.map((ch, i) => {
					if (ch === ' ' || revealed.has(i)) return ch;
					return randomChar();
				})
				.join('');
			step += 1;
			if (step > n) {
				clearInterval(timer);
				timer = null;
				displayed = text;
			}
		}, speed);
	}

	$effect(() => {
		trigger;
		if (trigger > 0) scramble();
	});

	$effect(() => {
		return () => {
			if (timer) clearInterval(timer);
		};
	});
</script>

<span class="font-mono">{displayed}</span>
