# Quantum_BellStates
# Set non-interactive backend first
import matplotlib
matplotlib.use('Agg')  # Non-interactive backend

import matplotlib.pyplot as plt

# QUantum computing Environment test script for Py3.9 on Win

# Check installed versions
import qiskit
import sys
import importlib
import numpy
import scipy

print(f"python version: {__import__('sys').version}")
print(f"Qiskit version: {sys.version}")
print("Successfully imported qiskit")

# first_circuit.py
from qiskit import QuantumCircuit, transpile
from qiskit_aer.primitives import Sampler as AerSampler
from qiskit.visualization import plot_histogram
from qiskit.primitives import Sampler
import matplotlib.pyplot as plt

# Create a Bell state circuit
print("Creating a Bell state circuit...")
qc = QuantumCircuit(6, 6)
qc.h(0)        # Put qubit 0 in superposition
qc.cx(0, 1)    # CNOT with control=qubit 0 and target=qubit 1
qc.measure([0, 1], [0, 1])  # Measure both qubits

# Draw the circuit
print("Drawing the circuit...")
circuit_diagram = qc.draw(output='mpl')
plt.title("Bell State Circuit")
plt.tight_layout()
plt.savefig("bell_circuit.png")
plt.close
print("Circuit diagram saved to bell_circuit.png")

# Try simulation with AerSimulator
try:
    print("\nUsing AerSimulator...")
    from qiskit_aer import AerSimulator
    
    simulator = AerSimulator()
    print("Simulator created")
    
    # Run the circuit
    result = simulator.run(qc, shots=20480).result()
    print("Simulation completed")
    
    counts = result.get_counts()
    print(f"Results: {counts}")
    
    # Save histogram without displaying
    plot_histogram(counts)
    plt.title("Bell State Results")
    plt.savefig("bell_results.png")
    plt.close()  # Explicitly close
    print("Results histogram saved to bell_results.png")
    
except Exception as e:
    print(f"Simulation failed: {e}")

print("\nScript completed")
