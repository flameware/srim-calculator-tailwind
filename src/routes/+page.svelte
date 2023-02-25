<script>
	// '억'을 계산하기 위한 변수
	const eok = 100000000;
	// 요구수익율 or 회사채 BBB- 5년물 (퍼센트)
	let expect_beta = null;
	// 지배주주 자기자본 (억원)
	let dominant_equity = null;
	//예상 ROE (퍼센트)
	let roe_expected = null;
	//보통주 발행 수
	let normal_stock = null;
	//우선주 발행 수
	let premium_stock = null;
	//자사주 보유 수
	let own_stock = null;
	// 기업가치 (초과수익 반영)
	let corp_value = null;
	// 주식 수
	let stock_count = null;
	//현재 적정가치
	let stock_value = null;
	//연간 10%씩 초과수익율 낮아진다는 가정
	let future_one = null;
	//연간 20%씩 초과수익율 낮아진다는 가정
	let future_two = null;

	function calcValue() {
		corp_value =
			dominant_equity +
			(dominant_equity * (roe_expected - expect_beta) * 0.01) / (expect_beta * 0.01);
		stock_count = normal_stock + premium_stock - own_stock;
		stock_value = (corp_value * eok) / stock_count;
		future_one =
			((dominant_equity +
				(dominant_equity * (roe_expected - expect_beta) * 0.01 * 0.9) /
					(1.0 + expect_beta * 0.01 - 0.9)) *
				eok) /
			stock_count;
		future_two =
			((dominant_equity +
				(dominant_equity * (roe_expected - expect_beta) * 0.01 * 0.8) /
					(1.0 + expect_beta * 0.01 - 0.8)) *
				eok) /
			stock_count;
	}

	function resetData() {
		expect_beta = null;
		dominant_equity = null;
		roe_expected = null;
		normal_stock = null;
		premium_stock = null;
		own_stock = null;
		corp_value = null;
		stock_count = null;
		stock_value = null;
		future_one = null;
		future_two = null;
	}
</script>

<div class="container bg-white mx-w-2xl">
	<div class="h-16 md:h-24 p-4 flex flex-col justify-center">
		<h1 class="text-2xl text-gray-800 font-sans font-bold">📈 S-RIM 계산기</h1>
	</div>
	<div class="m-4 p-4 bg-white rounded-lg space-y-4 border-solid border-2">
		<h3 class="text-lg font-sans font-bold">주주가치</h3>
		<div>
			<label class="text-sm text-gray-700" for="expectedbeta"
				>요구수익율(or 회사채 BBB- 5년금리, %)</label
			>
			<input
				id="expectedbeta"
				type="number"
				bind:value={expect_beta}
				placeholder="요구 수익율 (%)"
				class="mt-1 mb-2 block w-full px-4 py-3 bg-white border border-slate-300 rounded-md text-sm shadow-sm placeholder-slate-400 
                focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500"
			/>
			<label class="text-sm text-gray-700" for="dominantequity">지배주주 자기자본(억 원)</label>
			<input
				id="dominantequity"
				type="number"
				bind:value={dominant_equity}
				placeholder="지배주주 자기자본 (억 원)"
				class="mt-1 mb-2 block w-full px-4 py-3 bg-white border border-slate-300 rounded-md text-sm shadow-sm placeholder-slate-400 
                focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500"
			/>
			<label class="text-sm text-gray-700" for="expectedroe">ROE 예상치 (%)</label>
			<input
				id="expectedroe"
				type="number"
				bind:value={roe_expected}
				placeholder="기대 ROE (%)"
				class="mt-1 mb-2 block w-full px-4 py-3 bg-white border border-slate-300 rounded-md text-sm shadow-sm placeholder-slate-400 
                focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500"
			/>
		</div>
		{#if dominant_equity && roe_expected && expect_beta}
			<p class="text-right text-lg">
				주주가치: {new Intl.NumberFormat().format(
					Math.round(
						dominant_equity +
							(dominant_equity * (roe_expected - expect_beta) * 0.01) / (expect_beta * 0.01)
					)
				)} 억원
			</p>{/if}
	</div>
	<div class="m-4 p-4 bg-white rounded-lg space-y-4 border-solid border-2">
		<div class="space-y-4">
			<h3 class="text-lg font-sans font-bold">주식 수</h3>
			<div>
				<label class="text-sm text-gray-700" for="normalstock">보통주</label>
				<input
					id="normalstock"
					type="number"
					bind:value={normal_stock}
					placeholder="보통주 발행 수량"
					class="mt-1 mb-2 block w-full px-4 py-3 bg-white border border-slate-300 rounded-md text-sm shadow-sm placeholder-slate-400 
                    focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500"
				/>
				<label class="text-sm text-gray-700" for="premiumstock">우선주</label>
				<input
					id="premiumstock"
					type="number"
					bind:value={premium_stock}
					placeholder="우선주 발행 수량"
					class="mt-1 mb-2 block w-full px-4 py-3 bg-white border border-slate-300 rounded-md text-sm shadow-sm placeholder-slate-400 
                    focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500"
				/>
				<label class="text-sm text-gray-700" for="ownstock">자사주</label>
				<input
					id="ownstock"
					type="number"
					bind:value={own_stock}
					placeholder="자사주 보유 수량"
					class="mt-1 mb-2 block w-full px-4 py-3 bg-white border border-slate-300 rounded-md text-sm shadow-sm placeholder-slate-400 
                    focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500"
				/>
			</div>
		</div>
		{#if normal_stock && premium_stock && own_stock}
			<p class="text-right text-lg">
				주식 수: {new Intl.NumberFormat().format(normal_stock + premium_stock - own_stock)} 주
			</p>
		{/if}
	</div>
	{#if stock_value}
		<div class="m-4 p-4 bg-white rounded-lg space-y-4 border-solid border-2">
			<h3 class="text-lg font-sans font-bold">주당 가치 (적정가치)</h3>
			<!-- <p>주당 가치: {Math.round((dominant_equity + (dominant_equity*(roe_expected-expect_beta)*0.01)/(expect_beta*0.01))*eok/(normal_stock+premium_stock-own_stock)*100)/100} 원</p> -->
			<div class="space-y-2">
				<p class="text-sm text-gray-700">주당 가치 :</p>
				<p class="text-lg text-right">
					{new Intl.NumberFormat().format(Math.round(stock_value))}원
				</p>
				<p class="text-sm text-gray-700">미래 가치 (10%씩 감소):</p>
				<p class="text-lg text-right">
					{new Intl.NumberFormat().format(Math.round(future_one))} 원
				</p>
				<p class="text-sm text-gray-700">미래 가치 (20%씩 감소):</p>
				<p class="text-lg text-right">
					{new Intl.NumberFormat().format(Math.round(future_two))} 원
				</p>
			</div>
		</div>
	{/if}
	<div class="flex space-x-2 m-4">
		<button
			class="block w-1/2 px-4 py-3 bg-slate-200  border border-slate-300 rounded-md text-lg shadow-sm text-slate-700 font-medium hover:bg-slate-100 hover:font-bold"
			on:click={resetData}>초기화</button
		>
		<button
			class="block w-1/2 px-4 py-3 bg-green-600  border border-slate-300 rounded-md text-lg shadow-sm text-white font-medium hover:bg-green-700 hover:font-bold"
			on:click={calcValue}>계산하기</button
		>
	</div>
	<footer
		class="p-4 bg-white md:flex md:items-center md:justify-between md:p-6 dark:bg-gray-800 h-40"
	>
		<span class="text-sm text-gray-500 sm:text-center dark:text-gray-400"
			>© 2023 <a href="https://massivevoid.com" class="hover:underline">Seongki Sohn</a>. All Rights
			Reserved.
		</span>
	</footer>
</div>
