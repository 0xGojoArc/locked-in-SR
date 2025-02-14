##### Section 2
In your report, your job as an auditor is to do whatever it takes to make the protocol more secure. This is for private audit.

**Security Tools**
	- **Static Analysis:** Automatically checking code for issues without executing anything. It just looks for pattern matching
		- **Tools**: Slither, Aderyn, Mythril
	- **Fuzzing**: random data as inputs
		- Stateful Fuzz Testing: the system remembers the state of the last fuzz test and continues with a new fuzz test.
		- **Tools**: - Foundry, Echidna, Consensys
	- **Formal Verification:** FV is a generic term for applying formal method (FM) to verify the correctness of the hardware
		- applying FM means anything based on mathematical proofs, in the software often used as a proof of correctness or proof of "bug".
		- Symbolic execution: form of FV where you convert software to a mathematical expression
		- **Tools**: Matt, Manticore, Certora, Z3, Solidity SMT Checker 














##### Section 3 - PasswordStore Audit

