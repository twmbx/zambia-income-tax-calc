<script>
	let salary = 10000;
	let response = '';
	let tsal;
	let bands = [800, 2100];
	let tax = [0.25, 0.3, 0.37];
	let dues =[];
	let totalDues;
	
	$: message = calcTax(salary);
	$: dues;
	$: tsal;
	$: totalDues;

	const arrSum = arr => arr.reduce((a,b) => a + b, 0);

	function getTaxable(salary) {
		return salary - arrSum(bands)
	}

	function isTaxable(salary) {
		if (salary > arrSum(bands)) {
			return true
		}
		return false
	}
	
	function calcTaxes(tsal) {
		// var bands = [800, 2100]
		// var tax = [0.25, 0.3, 0.37]
		// var dues = []

		if (tsal <= bands[0]) {
			dues[0] = tsal * tax[0]
			return dues[0]
		}

		if (tsal <= bands[1]) {
			dues[0] = bands[0] * tax[0]
			dues[1] = (tsal - bands[0]) * tax[1]
			return dues[0] + dues[1]
		}

		if (tsal > (bands[0] + bands[1])) {
			dues[0] = bands[0] * tax[0]
			dues[1] = bands[1] * tax[1]
			dues[2] = (tsal - (bands[0] + bands[1])) * tax[2]
			return dues[0] + dues[1] + dues[2]
		}

		return dues[0] + dues[1] + dues[2]
	}
	
	function calcTax(salary){
		if (isTaxable(salary)) {
			tsal = getTaxable(salary)
			totalDues = calcTaxes(tsal)
			
			response = "<p>The salary is taxable.</p>"
				+ "<p>The taxable salary is: ZMW " + tsal + "</p>"
				+ "<p>Due for the taxman: ZMW " + totalDues + "</p>"
				+ "<p>Salary after tax: ZMW "+ (salary-totalDues) + "</p>"
			
			return response
		} else {
			return "The salary is not taxable."
		}
	}
</script>

<h1>🇿🇲 Salary Calculator</h1>
<span class="input-symbol-zmw">
	<input bind:value={salary} placeholder="10000">
</span>
<p>{@html message || 'No Taxes'}</p>