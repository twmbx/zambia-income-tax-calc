<script>
	let salary = 10000;
	let response = '';
	
	$: message = calcTax(salary);

	function handleClick() {
		count += 1;
	}
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
		var bands = [800, 2100]
		var tax = [0.25, 0.3, 0.37]
		var dues = []

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
			var tsal = getTaxable(salary)
			var dues = calcTaxes(tsal)
			
			response = "<p>The salary is taxable.</p>"
				+ "<p>The taxable salary is: " + tsal + "</p>"
				+ "<p>Due for the taxman: " + dues + "</p>"
				+ "<p>Salary after tax: "+ (salary-dues) + "</p>"
			
			return response
		} else {
			return "The salary is not taxable."
		}
	}
</script>

<h1>Zambian Salary Calculator</h1>
<input bind:value={salary} placeholder="10000">
<p>{@html message || 'No Taxes'}</p>