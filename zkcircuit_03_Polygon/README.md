## ZkSnarks Circuit 

zkSNARKs are cryptographic proofs that allow one party (the prover) to prove to another party (the verifier) that a statement is true without revealing any information about the statement itself. This is achieved through a process called "zero-knowledge proofs," where the prover convinces the verifier of the truth of a statement without revealing any additional information beyond the validity of the statement.

## Project Objective 

To design and implement a new zkSNARK (Zero-Knowledge Succinct Non-Interactive Argument of Knowledge) circuit capable of executing specified logical operations. Additionally, deploy a verifier on the Polygon blockchain to authenticate proofs generated by this circuit.

## Circuit Explanation 

![circuit-pictures](https://github.com/Harmanpreet0112/PolyProof_ADV_Blockchain_Project/assets/165464422/13e43673-398d-4de8-82f1-ec6da05de857)

The circuit comprises an AND gate and an OR gate interconnected with specific inputs A and B. In this setup, A OR B is the input to the AND gate, while B may also be connected to a NOT gate. The output of the AND gate and the output of the NOT gate (if connected) are then fed into the OR gate. Finally, the output Q is determined by the result of the OR gate, encapsulating the combined logical operations of AND, OR, and optionally NOT gates.

## Circuit Code

```circom
pragma circom 2.0.0;

 template Multiplier2 () {  

   // User Inputs 
   signal input a;  
   signal input b;

   // Circuit Internal Inputs

   signal x;  
   signal y;  

   // Output Circuit 
   signal output q;

   //component

   component andGate = AND();
   component notGate = NOT();
   component orGate = OR();

   // Singals Operatio to AND Gate

   andGate.a <== a;
   andGate.b <== b;
   x <== andGate.out;

   // Signals Operation to Not Gate 

   notGate.in <== b;
   y <== notGate.out;

   // Getting Final Output From Logical Gates (q)

   orGate.a <== x;
   orGate.b <== y;
   q <== orGate.out;

}

template AND() {
    signal input a;
    signal input b;
    signal output out;

    out <== a*b;
}

template NOT() {
    signal input in;
    signal output out;

    out <== 1 + in - 2*in;
}

template OR() {
    signal input a;
    signal input b;
    signal output out;

    out <== a + b - a*b;

    log("Circuit Contract Complete!");
    log("Result - ", out);
}

component main = Multiplier2();
```

The central template, named Multiplier2, orchestrates the overall functionality of the circuit. It begins by defining two input signals, a and b, which represent the operands to be multiplied. These signals traverse through a series of logical gates, including AND, NOT, and OR gates, before culminating in the output signal q, representing the result of the multiplication operation.

Within the Multiplier2 template, the input signals a and b are initially directed into an AND gate component, where they undergo logical multiplication. This operation produces an intermediate signal denoted as x, representing the result of the logical AND operation between a and b. Subsequently, the input signal b is fed into a NOT gate component, which computes the logical negation of b, generating an intermediate signal denoted as y. These intermediary signals x and y serve as inputs to an OR gate component.

The OR gate combines the results of the AND and NOT operations by computing the logical OR operation between signals x and y. The output of this OR operation, denoted as q, constitutes the final result of the circuit, effectively representing the outcome of the multiplication operation performed on input signals a and b. Additionally, the Multiplier2 template incorporates logging functionality within the OR template, providing a message indicating the completion of the circuit contract along with the computed result.

Apart from the Multiplier2 template, the code also defines three auxiliary templates: AND, NOT, and OR, which encapsulate the functionality of the logical gates utilized within the main circuit. These templates facilitate modularization and abstraction, promoting code reusability and maintainability. Furthermore, the main component instantiates the Multiplier2 template, serving as the entry point for the circuit.

## Project Execution 

```
Install required libraries by running npm i.
Compile the code using npx hardhat circom.
Execute the final deployment command: npx hardhat run scripts/deploy.ts --network Mumbai.
This command handles deployment of the verifier contract.
Additionally, it generates a proof using circuit intermediaries.
It creates calldata necessary for verification.
Finally, the verifyProof() function is called on the verifier contract with the generated calldata for proof verification.
```

## Circuit Structure 

In the project structure, every new circuit resides within its own directory. At the root of each circuit directory, you'll find the circom circuit file and the input data required for the circuit. An out directory will be automatically generated to store compiled outputs, keys, and proofs. The Powers of Tau file, obtained from the Polygon Hermez ceremony, is included to expedite the process, eliminating the need for a new ceremony and saving time. This structured setup ensures organization and efficiency in managing multiple circuits within the project.