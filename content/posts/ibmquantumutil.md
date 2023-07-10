+++
title = "Quantum Error Mitigation"
author = ["Yusheng Zhao"]
date = 2023-07-08T00:00:00+08:00
draft = false
+++

## Motivation {#motivation}

IBM recently published a paper that carried out the Trotterized time evolution
of a transverse-field Ising model Hamiltonian \(H = -J \sum\_{<i,j>}Z\_{i}Z\_{j} +
h \sum\_{i}X\_{i}\) on 127 qubits. The quantum circuit used for time evolution
consisted of "up to 60 layers of two-qubit gates, a total of 2880 CNOT gates"
[1]. This is a significant experiment due to the relatively low quality of CNOT
gates across all currently available quantum computing platforms. The number and
layers of CNOT gates involved generally can lead to meaningless results due to
noise. A brute-force classical simulation of such a quantum circuit is
definitely out of reach for even the most powerful supercomputers.

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

One of the most formidable challenges faced by quantum computers is noise. It's
widely recognized that quantum algorithms can outperform their classical
counterparts in solving critical problems. For instance, our daily
communication, web browsing, and bank transactions are safeguarded by public-key
encryption methods such as RSA and Diffie-Hellman techniques. These methods
hinge on the difficulty of period finding [3]. Shor's algorithm solves the
period finding problem exponentially faster than classical methods. However,
noise renders the results of Shor's algorithm useless on real quantum computers.
Since Shor's algorithm was used to factor 15 on an NMR quantum computer in 2012
[4], only one other experiment has successfully pushed this limit to 21 using
Shor's algorithm [5]. Noise has made it impossible to scale Shor's algorithm
further.

Of course, solutions have been proposed to tackle the noise problem on quantum
computers. Quantum Error Correction—an algorithm that shields information from
noise and decoherence—has been considered. This algorithm accomplishes its goal
using Quantum Error Correction codes, which encode one qubit's worth of
information onto multiple, physically separated qubits. As noise and decoherence
result from undesirable physical interactions, information encoded onto
physically separated qubits is safeguarded from noise. This protection is based
on the fact that our world predominantly supports local physical interactions.
To interfere with information that is physically separated, one would need
non-local interaction, which is typically unlikely.

However, a significant challenge with implementing Quantum Error Correction
codes is posed by the threshold theorem. This theorem proposes that "a quantum
computer with a physical error rate below a certain threshold can reduce the
logical error rate to arbitrarily low levels" through the application of Quantum
Error Correction schemes [6]. Today's most advanced physical qubits can barely
achieve parity in terms of logical error rate when implementing a Quantum Error
Correction code as compared to a basic repetition code [7]. Consequently, with
current engineering techniques, Quantum Error Correction cannot eliminate the
noise in quantum computers to arbitrary level.

Given the inherent noise accompanying all quantum computer operations and the
restrictions on the number of a physical quantum computer's qubits, we find
ourselves in the Noisy Intermediate Scale Quantum Era, a term coined by John
Preskill [8]. Various methods have been proposed to enhance the usability of
quantum computers in the face of noise. These methods, collectively known as
Quantum Error Mitigation methods, each focus on a specific type of noise. With a
particular type of noise in mind, logical quantum circuits are modified, and
post-processing of results is executed on quantum circuit outputs to mitigate
the noise's impact on these results. Notable Quantum Error Mitigation techniques
include Dynamical Decoupling, Measurement Mitigation, Pauli Twirling, and Zero
Noise Extrapolation. The remainder of this blog will be dedicated to explaining
Zero Noise Extrapolation, given its straightforward nature and its simplistic
assumptions about the underlying noise model of a quantum computer.


## Zero Noise Extrapolation {#zero-noise-extrapolation}

Zero Noise Extrapolation refers to an "algorithmic scheme(s) that reduce the
noise-induced bias in the expectation value by post-processing outputs from an
ensemble of circuit runs, using circuits at the same noise level as the original
unmitigated circuit or above" [9]. This technique was initially introduced by Li
et al [11] and Temme et al [12] and later popularized by Kandala et al [10] to
perform the variational optimization of molecular Hamiltonian of Hydrogen and
Lithium hydride. One notable feature of Zero Noise Extrapolation is that it
meets the requirement for a "reliable error mitigation method" as proposed by
Cai et al [9]. Specifically, Zero Noise Extrapolation "requires few assumptions
(or no assumptions) about the final state prepared by the computation" [9].
Additionally, it places a simple constraint on the noise model and the effect
ZNE could mitigate. Hence, Zero Noise Extrapolation stands as a broadly
applicable and straightforward-to-implement technique in Quantum Error
Mitigation.

To comprehend the simplified noise model and its implication on how it
influences the expectation value of an operator for the state prepared by a
quantum circuit, we need to introduce a few notations. First, denote locations
marked by indices \\(i\\), ranging from \\(0\\) to \\(n\\), in a quantum circuit where
**independent** Pauli errors could occur. At each location \\(i\\), the probability
of a Pauli error occurring is \\(p\_{i}\\). The sum of all probabilities is
\\(\lambda \equiv \sum\_{i=0}^{n}p\_i\\). This can be understood as a noise
parameter characterizing how noisy the circuit is. When \\(n\\) is large, Le Cam's
inequality suggests that the probability of \\(k\\) errors occurring in a noisy
circuit is \\(P\_k \approx e^{-\lambda}\lambda^{k}/k!\\) [11], i.e., it is
approximated by a Poisson distribution. We then model the erroneous expectation
value, \\(\left\langle O\_\lambda\right\rangle\\) as the weighted sum of operator
expectation values with \\(k\\) number of errors occurring during the quantum
circuit, \\(\left\langle O\_{|\mathbb{L}|=k}\right\rangle\\). Specifically,
\\( \left\langle O\_\lambda\right\rangle=\sum\_{k=0}^{\infty} P\_k\left\langle
O\_{|\mathbb{L}|=k}\right\rangle=e^{-\lambda} \sum\_{k=0}^{\infty}
\frac{\lambda^k}{k !}\left\langle O\_{|\mathbb{L}|=k}\right\rangle \\).

By focusing on the noise parameter \\( \lambda \\), we observe that the expectation
value of the operator follows an exponential decay. Methods like
pulse-stretching or unitary-folding [10] could be employed to manually increase
the noise level and obtain a noisier expectation value for the operator
\\( \hat{O} \\). After obtaining different \\( \hat{O} \\), we could use an exponential
fitting to achieve the noiseless estimation of \\( \hat{O} \\)."


### Two level Defect {#two-level-defect}

While Zero Noise Extrapolation is a readily implementable technique for quantum
error mitigation, it has its own shortcomings. The exponential fitting used to
extrapolate the noiseless expectation value requires that different circuits
have similar noise parameters, \\( \lambda \\). Often, on a cloud-based
superconducting quantum computer, this is violated at the physical level due to
a phenomenon known as Two-Level Defects (TLS).

In simple terms, the Josephson Junction in the superconducting transmon requires
an amorphous \\( AlO\_x \\)​ layer between the aluminium electrodes. At low
temperatures, TLS is formed around this amorphous layer by tunneling atoms,
dangling bonds, and trapped charges. Consequently, TLS induces noise parameter
fluctuations on an hourly scale. This destroys the extrapolatability of
expectation values obtained from multiple experiments [13].


## Pauli-Lindblad Noise Model {#pauli-lindblad-noise-model}


### What is it? {#what-is-it}


### How to efficiently model if through measurement {#how-to-efficiently-model-if-through-measurement}


### How to combine it with ZNE {#how-to-combine-it-with-zne}


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
3.  [Scot Aaronson's Lecture notes](https://www.scottaaronson.com/qclec/19.pdf)
4.  [Experimental realization of Shor's quantum factoring algorithm using nuclear magnetic resonance](https://www.nature.com/articles/414883a)
5.  [Experimental realisation of Shor's quantum factoring algorithm using qubit recycling](https://arxiv.org/abs/1111.4147)
6.  [Threshold Theorem Wikipedia](https://en.wikipedia.org/wiki/Threshold_theorem)
7.  [Suppressing quantum errors by scaling a surface code logical qubit](https://www.nature.com/articles/s41586-022-05434-1)
8.  [Quantum Computing in the NISQ era and beyond](https://arxiv.org/abs/1801.00862)
9.  [Quantum Error Mitigation](https://arxiv.org/pdf/2210.00921.pdf)
10. [Error mitigation extends the computational reach of a noisy quantum processor](https://www.nature.com/articles/s41586-019-1040-7)
11. [Le Cam's Inequality and Poisson Approximations](http://www-stat.wharton.upenn.edu/~steele/Papers/PDF/LIaPA.pdf)
12. [Efficient Variational Quantum Simulator Incorporating Active Error Minimization](https://journals.aps.org/prx/abstract/10.1103/PhysRevX.7.021050)
13. [Error Mitigation for Short-Depth Quantum Circuits](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.119.180509)
14. [Towards understanding two-level-systems in amorphous solids -- Insights from quantum circuits](https://arxiv.org/abs/1705.01108)
