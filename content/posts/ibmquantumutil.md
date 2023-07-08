+++
title = "Quantum Error Mitigation"
author = ["Yusheng Zhao"]
date = 2023-07-08T00:00:00+08:00
draft = false
+++

## Motivation {#motivation}

IBM recently published a paper that carried out the Trotterized time evolution
of a transverse-field Ising model Hamiltonian \\(H = -J \sum\_{<i,j>}Z\_{i}Z\_{j} + h
\sum\_{i}X\_{i}\\) on 127 qubits. The quantum circuit used for time evolution
consisted of "up to 60 layers of two-qubit gates, a total of 2880 CNOT
gates"[1]. This is a significant experiment due to the relatively low quality of
CNOT gates across all currently available quantum computing platforms. The
number and layers of CNOT gates involved generally can lead to meaningless
results due to noise. A brute-force classical simulation of such a quantum
circuit is definitely out of reach for even the most powerful supercomputers.

The wording of their title, suggesting that their experiment demonstrates
quantum utility (a weaker version of quantum advantage), received mixed reviews
within the Quantum Computing community on Twitter. However, some positive
results emerged from the discussions between proponents of this paper and
others. For instance, Sels and colleagues [2] demonstrated a method to exploit
the heavy-hexagon topology of IBM's superconducting quantum chip to speed up
classical simulation of the quantum circuit with belief propagation tensor
networks. This, although contradicting IBM's claim of demonstrating quantum
utility, helps extend our understanding of classical simulation of quantum
circuits. Unfortunately, an in-depth analysis of this topic is beyond the scope
of this blog. Interested readers are encouraged to consult the reference below
and investigate further on their own.

Conversely, IBM clarifies that the essence of this paper is not quantum utility
but two other crucial aspects. First, it demonstrates "advances in the coherence
and calibration of a superconducting processor at this scale", with 127 qubits
[1]. More importantly, it showcases "the ability to characterize and
controllably manipulate noise across such a large device" [1], which will be the
primary focus of this post.

This post will proceed as follows: First, I will discuss the importance of
Quantum Error Mitigation by examining the effect of noise on quantum computers
and why Quantum Error Correction is not a viable solution at present. Then, I
will delve into the Quantum Error Mitigation technique used in IBM's paper -
Zero Noise Extrapolation. Finally, I will conclude with how IBM improved upon
the basic Zero Noise Extrapolation with the Pauli-Lindblad noise model to
achieve promising experimental results on 127 qubits.


## Achilles Heel of Quantum Computer: Noise {#achilles-heel-of-quantum-computer-noise}


## Zero Noise Extrapolation {#zero-noise-extrapolation}


### Two level Defect {#two-level-defect}


## Pauli-Lindblad Noise Model {#pauli-lindblad-noise-model}


## Conclusion {#conclusion}

Quantum error mitigation is itself fascinating in the way that by presenting a
physically motivated reasonable noise model and probing the experiment platform
to parameterize it, we could obtain result from non-error corrected circuits.

However, it is not clear to me whether such overhead of noise model probing will
be scalable. I.e for industrial application that uses thousands of qubits, will
such technique hold onto itself.

Nevertheless, a successful quantum error mitigation experiment necessarily
indicates the validity of tomographic techniques about the noise model and the
accuracy of such nosie model. In that sense, quantum error mitigation
experiments are the touch stone to understanding the noise in currently NISQ
devices.


## Disclaimer {#disclaimer}

I have also done some [work](https://journals.aps.org/prresearch/abstract/10.1103/PhysRevResearch.5.013183) in quantum error mitigation. I found the idea of
using quantum error mitigation results to verify the noise model occurred in the
actual device interesting.

This blog is written with love using Emacs and Org Mode.


## Reference {#reference}

1.  [Evidence for the utility of quantum computing before fault tolerance](https://www.nature.com/articles/s41586-023-06096-3)
2.  [Efficient tensor network simulation of IBM's kicked Ising experiment](https://arxiv.org/abs/2306.14887)
