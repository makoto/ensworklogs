
# Port DNSRPOVE into js

## Summary

DNSPROVE allows users to query dns record and send the proof to DNSOracle so that users can registar dns based ENS names.

## What's in my way right now?

- Testing recursive function calls are hard.
- I may still not fully understanding the whole flow.
- Working with wrong hex and binaries are hard

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
			-  __done__ Decide on module name (should we prepend "@ensdomains" which is causing a problem on truffle?
					
	- __done__ Modify migration file to deploy ENS and DNSResolver into ganache
		- __done__ changed migration file to set only dummy algo/digest and integration tests are start failing.
		- __done__ claim to registrar with proof
			- __done__ keep getting `Error: Given parameter is not bytes: "_ensmatokenxyz,"` errors when trying to send with `registrar.methods.claim(dns.hexEncodeName("_ens.matoken.xyz."), proof).send();` using web3 1.0 => Had to pass everything as hex which I didin't for proof.
			__ __done__ getting `revert` => it was for 2 reasons. first, root tld had to be changed from `test` to `xyz`, and second proof rrdata had to contain `a=address` where address is the contract owner of the address which is claiming the domain for. 
	- Define interface to set ownership.
		- Make separate npm library for dnsregistrar (separate from smart contract)
		- __done__ Convert current integration test from jest to truffle to remove contract address dependencies
			- __done__ currently getting problem of using web3 1.0 (to decouple library from truffle) within truffle test environment (web3 1.0 method calls is returning empty value for some reason) => The problem was more about `truffle test` not running migration properly so contract was empty. Updated README to avoid same problem happending.
		- __here__ Include dnsprover into dnsregistrar
			- __done__ decide wrapper API interface
			- __done__ create batch API inside dnsprover which returns the list of unproven transactions
				- Add test
					- __done__ Refactor test on dnsprover integration to be able to run multiple tests
			- __done__ integrate with the batch API
-  __WIP__ Writing Unit tests
	- Investigate better way to  mock calls to smart contract
	- Investigate what is the good way to test recursive calls.
	- Investigate if I can separate out the flow logic from actual validation of signatures, etc
- __WIP__ Publish as a library 
	- __done__ use browserify and babelify to bundle js
	- __done__ Fix problem on Truffle not handling pacakge with namespace
		- __done__ Fix name space issue
	- __done__ Publish dnsregistrar as a npm package
	- __here__ Publish dnsprove as a npm package
		- Install ndsprove into separate project and make sure it works
		- Try out submitting proof as address owner
		- Try out submitting proof as non address owner
- __done__ Try it out
	- __done__ Create a simple web front end to DNS lookup
	- __done__ Create a simple web front end to DNS Oracle lookup
	- __done__ Create a simple web front end to submit proof
	- __done__ Create a simple web front end to claim ownership
- __WIP__ Submit proofs in one batch
	-- Check changes in dnssecoracle and dnsregstrar
	-- Create new function to submit in one function