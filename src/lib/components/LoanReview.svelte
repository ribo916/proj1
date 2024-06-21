<script lang="ts">
	export let vehicle;

	const vehicleImage = vehicle.DefaultImage;

	const {
		VIN,
		Year,
		Make,
		Model,
		InterestRate,
		CurrentBalance,
		NumberOfPayments,
		CurrentPaymentNumber
	} = vehicle;
	const PaymentAmount = vehicle.PaymentAmount ?? 0;
	const Engine = vehicle.NEOVIN.engine ?? '';
	const Version = vehicle.NEOVIN.version ?? '';
	const PricePredictionLow = vehicle.PricePredictionLow ?? 0;
	const PricePredictionHigh = vehicle.PricePredictionHigh ?? 0;

	function curr(price: string, decimal: boolean = false) {
		const USDollarAndCents = new Intl.NumberFormat('en-US', {
			style: 'currency',
			currency: 'USD',
			maximumFractionDigits: 2
		});

		const USDollar = new Intl.NumberFormat('en-US', {
			style: 'currency',
			currency: 'USD',
			maximumFractionDigits: 0
		});

		return decimal ? USDollarAndCents.format(Number(price)) : USDollar.format(Number(price));
	}

	function paymentsRemaining() {
		return Number(NumberOfPayments) - Number(CurrentPaymentNumber);
	}

	function highValueFeatures() {
		const hiVal = vehicle.NEOVIN.high_value_features;

		return hiVal ? hiVal.length : 0;
	}
</script>

<div class="flex p-6 font-mono w-[800px] border border-slate-200">
	<div
		class="flex-none w-48 mb-10 relative z-10 before:absolute before:top-1 before:left-1 before:w-full before:h-full before:bg-emerald-700"
	>
		{#if VIN === '1FTFW1E53MKF07665'}
			<img
				src="/ford-f150.webp"
				alt=""
				class="absolute bg-white z-10 inset-0 w-full h-full object-left object-cover"
				loading="lazy"
			/>
		{:else if vehicleImage}
			<img
				src={vehicleImage}
				alt=""
				class="absolute bg-white z-10 inset-0 w-full h-full object-center object-cover"
				loading="lazy"
			/>
		{:else}
			<img
				src="/minivan.jpg"
				alt=""
				class="absolute bg-white z-10 inset-0 w-full h-full object-left object-cover"
				loading="lazy"
			/>
		{/if}
	</div>
	<div class="flex-auto pl-6">
		<div
			class="relative flex flex-wrap items-baseline pb-6 before:bg-black before:absolute before:-top-6 before:bottom-0 before:-left-60 before:-right-6"
		>
			<h1 class="relative w-full flex-none mb-2 text-2xl font-semibold text-white">
				Your {Year}
				{Make}
				{Model}
			</h1>
			<div class="relative text-lg text-white">LOAN @ {curr(PaymentAmount, true)}/MO</div>
			<div class="relative uppercase text-emerald-700 ml-3">{Engine} {Version}</div>
		</div>
		<div class="flex items-baseline my-6">
			<div class="space-x-3 flex text-sm font-medium">
				<div
					class="relative w-30 h-10 flex items-center justify-center text-black before:absolute before:z-[-1] before:top-0.5 before:left-0.5 before:w-full before:h-full"
				>
					ESTIMATED VALUE:
				</div>

				<div
					class="relative w-36 h-10 flex items-center justify-center bg-black text-white before:absolute before:z-[-1] before:top-0.5 before:left-0.5 before:w-full before:h-full before:bg-emerald-700"
				>
					{curr(PricePredictionLow)}
				</div>

				<div
					class="relative w-16 h-10 flex items-center justify-center text-black before:absolute before:z-[-1] before:top-0.5 before:left-0.5 before:w-full before:h-full"
				>
					UP TO
				</div>
				<div
					class="relative w-36 h-10 flex items-center justify-center bg-black text-white before:absolute before:z-[-1] before:top-0.5 before:left-0.5 before:w-full before:h-full before:bg-emerald-700"
				>
					{curr(PricePredictionHigh)}
				</div>
			</div>
		</div>
		<div class="flex space-x-2 mb-4 text-sm font-medium">
			<p>
				<span class="font-bold">{paymentsRemaining()} months</span> remaining |
			</p>
			<p>
				Current rate:
				<span class="font-bold">{InterestRate}%</span> | Balance:
				<span class="font-bold">{curr(CurrentBalance)}</span>
			</p>
		</div>
		<div class="flex space-x-2 mb-4 text-sm font-medium">
			<p>
				Your {Make} has {highValueFeatures()} high value options |
				<a href="/" class="font-bold underline text-emerald-700">Valuation details >></a>
			</p>
		</div>
		<div class="flex space-x-2 mb-4 text-sm font-medium">
			<div
				class="relative w-32 h-6 flex items-center justify-center text-emerald-50 bg-emerald-900 before:absolute before:z-[-1] before:top-0.5 before:left-0.5 before:w-full before:h-full"
			>
				FREE CAR WASH
			</div>
			<p>Member benefit available from Feb 28 to Mar 15</p>
		</div>
		<p class="text-xs leading-6 text-slate-500">VIN <span class="font-bold">{VIN}</span></p>
	</div>
</div>
