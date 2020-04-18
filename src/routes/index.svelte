<script>
	let salary = 10000;
	let response = '';
	let tsal;
	let bands = [800, 2100];
	let tax = [0.25, 0.3, 0.37];
	let dues =[];
	let napsa;
	let totalDues;
	
	$: message = calcTax(salary);
	$: dues;
	$: tsal;
	$: totalDues;

	const arrSum = arr => arr.reduce((a,b) => a + b, 0);

	function getTaxable(salary) {
		return salary - 3300
	}

	function isTaxable(salary) {

		if (salary > 3300) {
			return true
		}
		return false
	}
	
	function calcTaxes(tsal) {

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

	function calcNapsaContrib(salary){
		var contrib = salary * 0.05
		return ( (contrib > 1149.60) ? 1149.60 : contrib )
    }
	function calcTax(salary){
		
		napsa = calcNapsaContrib(salary)
		
		if (isTaxable(salary)) {
			tsal = getTaxable(salary)
			totalDues = calcTaxes(tsal)
			
			response = "<p>The salary is taxable.</p>"
				+ "<p>Amount exempt from tax: ZMW 3300</p>"
				+ "<p>The taxable salary is: ZMW " + tsal + "</p>"
				+ "<p>Due for the taxman: ZMW " + totalDues + "</p>"
				+ "<p>NAPSA Contribution: ZMW " + napsa + "</p>"
				+ "<p>Salary after tax: ZMW "+ (salary-totalDues-napsa) + "</p>"
			
			return response
		} else {
			response = "<p>The salary is not taxable. However, </p>"
				+ "<p>Amount due to NAPSA: ZMW "+ napsa +"</p>"
				+ "<p>Total Salary after Napsa Deduction: ZMW "+ (salary - napsa) +"</p>"

			return response
		}
	}
</script>

<svelte:head>
	<title>Zambian Income Tax Calculator</title>
</svelte:head>

<h1>🇿🇲 Income Tax Calculator</h1>
<label for="salary">Salary:</label>
<span class="input-symbol-zmw">
	<input name="salary" id="salary" bind:value={salary} placeholder="10000">
</span>
<p>{@html message || 'No Taxes'}</p>

