# 4-Qubit Quantum Circuit

We create and train a variational circuit that transforms input states into predefined output states with PennyLane and PyTorch.

Specifically, 

- we prepare 4 random 4-qubit quantum states that are defined with 4 randomly generated vectors of rotation angles
- we create and train a parametrized quantum circuit such that:
    * if random state 1 is provided, it returns state |0011>
    * if random state 2 is provided, it returns state |0101>
    * if random state 3 is provided, it returns state |1010>
    * if random state 4 is provided, it returns state |1100>
- we investigate what would happen if a different state is provided?

### Variational circuit
The circuit is built from simple gates, we use 5 repeated layer of RX, RY, RZ, and CNOT gates.

### Cost function

The cost function is constructued with repect to the Pauli-Z expectation value of qubits at the end of the circuit. Mapping the input to state |0> is equivalent to measuring a Pauli-Z expectation value of 1, since state |0> is an eightvector of the Pauli-Z matrix with eigenvalue $\lambda = 1$. State |1> us also an eigenvector of Pauli-Z matrix with eigenvalue  $\lambda = -1$.

Here, our desired outcome for each qubit is either |0> or |1>, which is equivalent to measuring Pauli-Z value of 1 and -1 respectively. Since the Pauli-Z expecttion is bound between [-1, 1], we can define our cost function as such

$C = \sum_{\phi_i'=|1>}{\sigma_z\phi_i} - \sum_{\phi_i'=|0>}{\sigma_z\phi_i}$

where $\phi_i'$ is the target outcome of $\phi_i'$. That is, if the target outcome of a qubit is |1>, we want to minimize its Pauli-Z expectation hence adding it to the cost function, and if the target outcome of a qubit is |0>, we want to maximize its Pauli-Z expectation hence substracting it from the cost function. 

### Different States

We test a few linear combination of our prepared initial states and use them as input. We print out the circuit output (i.e. the Pauli-Z expectation value of all the qubits), as well as the most likely observed classical bit.
