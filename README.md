# qqcd(Qulacs's QuantumCircuit Drawer)
This is a library for outputting qulacs's QuantumCircuit. This is written by Python, so cannot use this from C/C++ qulacs.  
In creating this library, I got some advice from @corryvrequan.  

## MEMO
- The version of qulacs when I created this is 0.2.0.  
- Sorry, comments in program are Japanese.

## Requirement
* Numpy

## How to use
Please copy&paste the file "draw_circuit.py" to your directory, and run a program like below:

```
import qulacs
from qc_drawer import draw_circuit #import module

circuit = qulacs.QuantumCircuit(3)
circuit.add_X_gate(0)
circuit.add_Y_gate(1)
circuit.add_Z_gate(2)
circuit.add_dense_matrix_gate([0,1], [[1,0,0,0],[0,1,0,0],[0,0,0,1],[0,0,1,0]])
circuit.add_CNOT_gate(2,0)
circuit.add_X_gate(2)
draw_circuit(circuit) # call function
```
Then, you will get a circuit picture like this:
```
   ___     ___     ___
  | X |   |DeM|   |CX |
--|   |---|   |---|   |-----x----
  |___|   |   |   |___|     |
   ___    |   |     |       |
  | Y |   |   |     |       |
--|   |---|   |-----|-------x----
  |___|   |___|     |
   ___              |      ___
  | Z |             |     | X |
--|   |-------------●-----|   |--
  |___|                   |___|
```
Program "test.py" is test program. You can see how some gates are drawen by this library. If you run, you will get
```
   ___     ___     ___
  | X |   |DeM|   |CX |
--|000|---|003|---|004|----------
  |___|   |   |   |___|
   ___    |   |     |
  | Y |   |   |     |
--|001|---|   |-----|------------
  |___|   |___|     |
   ___              |      ___
  | Z |             |     | X |
--|002|-------------●-----|005|--
  |___|                   |___|
   ___     ___     ___
  | X |   |DeM|   |CX |
--|000|---|002|---|004|----------
  |___|   |   |   |___|
   ___    |   |     |
  | Y |   |   |     |
--|001|---|   |-----|------------
  |___|   |___|     |
   ___              |      ___
  | Z |             |     | X |
--|003|-------------●-----|005|--
  |___|                   |___|


------------●------------
            |
   ___      |      ___
  | X |     |     | X |
--|   |-----|-----|   |--
  |___|     |     |___|
           _|_
          |CZ |
----------|   |----------
          |___|
                   ___     ___
                  |DeM|   |DeM|            005     006
----●-------------|002|---|003|-------------x-------x------------
    |             |___|   |___|             |       |
    |               |       |               |       |
    |               |       |               |       |      007
----|-------●-------●-------●-------●-------x-------|-------x----
    |       |       |       |       |               |       |
    |       |       |       |      _|_              |       |
    |       |       |       |     |DeM|             |       |
----●-------●-------●-------|-----|004|-------------x-------|----
    |       |               |     |___|                     |
   _|_     _|_              |       |                       |
  |DeM|   |DeM|             |       |                       |
--|000|---|001|-------------●-------●-----------------------x----
  |___|   |___|
   ___     ___     ___
  |DeM|   |DeM|   |DeM|
--|   |---|   |---|   |----------
  |   |   |_ _|   |_ _|
  |   |    | |     | |     ___
  |   |    | |     | |    |DeM|
--|   |----|●|-----| |----|   |--
  |   |    | |     | |    |_ _|
  |   |    | |    _| |_    | |
  |   |    | |    |   |    | |
--|   |----| |----|   |----|●|---
  |___|    | |    |_ _|    | |
          _| |_    | |    _| |_
          |   |    | |    |   |
----------|   |----| |----|   |--
          |   |    | |    |___|
          |   |   _| |_
          |   |   |   |
----------|   |---|   |----------
          |___|   |___|
```

## Options
### Verbose argument
You can pass an argument "verbose" to "circuit_draw" function. If verbose=1, the order number added to the circuit will be written.  
Here, show the upper example circuit:
```
circuit_draw(circuit, verbose=1)
```
```
   ___     ___     ___
  | X |   |DeM|   |CX |    006
--|000|---|003|---|004|-----x----
  |___|   |   |   |___|     |
   ___    |   |     |       |
  | Y |   |   |     |       |
--|001|---|   |-----|-------x----
  |___|   |___|     |
   ___              |      ___
  | Z |             |     | X |
--|002|-------------●-----|005|--
  |___|                   |___|
```

### Control dot
In default, character "●" is used to mean control qubit. But in CommandPrompt, sometimes display layout is corrupted.  
In that case, please commentout a code `CON_DOT = "●"`(line2 in circuit_drwaer.py) and enable `CON_DOT = "･"`(line3).  
Then you can get tight output in exchange for visibility. Below is example:
```
   ___     ___     ___
  | X |   |DeM|   |CX |       
--|   |---|   |---|   |-----x----
  |___|   |   |   |___|     |
   ___    |   |     |       |
  | Y |   |   |     |       |
--|   |---|   |-----|-------x----
  |___|   |___|     |
   ___              |      ___
  | Z |             |     | X |
--|   |-------------･-----|   |--
  |___|                   |___|
```

## Gate name correspondence
In this library, a gate name is converted to a new name which the size is max 3. Below table shows the correspondence of original name and converted name.  
If original name does not exist, the converted name is "UDF" which means "UnDeFined".
|qulacs's gate name|circuit picture output|
|:-:|:-:|
|I|I|
|X|X|
|Y|Y|
|Z|Z|
|H|H|
|S|S|
|Sdag|Sdg|
|T|T|
|Tdag|Tdg|
|sqrtX|sqX|
|sqrtXdag|sXd|
|sqrtY|sqY|
|sqrtYdag|sYd|
|Projection-0|P0|
|Projection-1|P1|
|U1|U1|
|U2|U2|
|U3|U3|
|X-rotation|RX|
|Y-rotation|RY|
|Z-rotation|RZ|
|Pauli|Pau|
|Pauli-rotation|PR|
|CZ|CZ|
|CNOT|CX|
|SWAP|SWP|
|Reflection|Ref|
|ReversibleBoolean|ReB|
|DenseMatrix|DeM|
|DinagonalMatrix|DiM|
|SparseMatrix|SpM|
|Genericgate|GeG|
|ParametricRX|pRX|
|ParametricRY|pRY|
|ParametricRZ|pRZ|
|ParametricPauliRotation|pPR|
