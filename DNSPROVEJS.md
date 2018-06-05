
# Port DNSRPOVE into js

## Summary

DNSPROVE allows users to query dns record and send the proof to DNSOracle so that users can registar dns based ENS names.

## What's in my way right now?

- Testing recursive function calls are hard.
- I may still not fully understanding the whole flow.
- Working with wrong hex and binaries are hard
- Getting some npm dependency issues.

## Logs

- __done__  it retrieves the chain of record from DNS over http
	- __done__ copy the logic from go version
	- __WIP__ Test edge cases
- __WIP__ Writing integration tests
	- __done__ The call to dns servers are mocked
	- __done__ Test a happy path
        - __WIP__ Test not found case
        - __WIP__ Test invalid data case
- __done__  it can submit rrdata into DNSOracle
	- __done__  Includes dnssec-oracle into dnsprove-js
- it claims domain via dnsresolver
	-  __done__ Includes dnsresolver into dnsprove-js
		- __done__ Make dnsresolver as a npm module
			-  __WIP__ Decide on module name (should we prepend "@ensdomains" which is causing a problem on truffle?
				- Investigate how Aragon handle packaging
					- Investigate if we should use Lerna to manage it
					
	- __done__ Modify migration file to deploy ENS and DNSResolver into ganache
		- __done__ changed migration file to set only dummy algo/digest and integration tests are start failing.
		- __done__ claim to registrar with proof
			- __done__ keep getting `Error: Given parameter is not bytes: "_ensmatokenxyz,"` errors when trying to send with `registrar.methods.claim(dns.hexEncodeName("_ens.matoken.xyz."), proof).send();` using web3 1.0 => Had to pass everything as hex which I didin't for proof.
			__ __done__ getting `revert` => it was for 2 reasons. first, root tld had to be changed from `test` to `xyz`, and second proof rrdata had to contain `a=address` where address is the contract owner of the address which is claiming the domain for. 
	- __here__ Define interface to set ownership.
		- Should dnsprove-js wrap resolver and ens as well?
-  __WIP__ Writing Unit tests
	- Investigate better way to  mock calls to smart contract
	- Investigate what is the good way to test recursive calls.
	- Investigate if I can separate out the flow logic from actual validation of signatures, etc
- __WIP__ Publish as a library 
	- __done__ use browserify and babelify to bundle js
	- Investigate the best way to transpile to work in both browser and node.
        - publish
- __WIP__ Try it out
	- Create a simple web front end to test.