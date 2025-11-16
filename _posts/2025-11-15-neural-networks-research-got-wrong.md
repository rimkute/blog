---
layout: post
title: "What My Neural Networks Research Got Wrong"
date: 2024-11-15 15:00:00
permalink: /neural-networks-research/
category: machine learning
---
# What My Neural Networks Research Got Wrong

In the early 2000s, I published research on using neural networks to locate noise sources with faulty microphone arrays in *The Journal of the Acoustical Society of America*.

That work contains seeds of insights now playing out at massive scale two decades later—and also contains assumptions that were fundamentally wrong.

## The Technical Approach

We processed SISO (Single Input Single Output) microphone signals using FFT (Fast Fourier Transform) analysis on unwrapped phase data to extract Time Difference of Arrival (TDOA) information. A Matched Filter Array (MFA) pipeline applied coherence filtering to clean noisy phase plots. Neural networks then learned patterns in residual errors to identify and localize faulty microphones.

**The core finding**: Neural networks didn't just detect faults—they localized them, learning that measurement corruption has statistical signatures without being programmed to look for specific fault types.

**The insight**: Neural networks excel at finding patterns in redundant systems trained on examples of correct behavior and failure modes.

## Fast Forward to 2025

Scale the problem: autonomous vehicles with dozens of sensors (cameras, LiDAR (Light Detection and Ranging), radar, and IMU (Inertial Measurement Unit)). Some are faulty. Some are attacked. You need ground truth about the world.

The same approach applies: train neural networks on sensor fusion data with examples of correct and corrupted measurements. The network learns to detect anomalies.

**But here's what changes**: In the early 2000s, the acoustic system was simple enough to understand *why* the network decided what it did. Small input space. Stationary signals. Characterized noise.

In 2025, sensor fusion is enormous. 1000+ dimensional input space. Non-stationary signals. Adversarial noise (not random).

The network solves the problem. We can't explain how.

## Where I Was Right

Neural networks are exceptional at anomaly detection in redundant systems with known failure modes. They distinguish random failures from systematic attacks, sensor noise from adversarial input.

**Where they excel:**
- Pattern recognition in high-dimensional data
- Anomaly detection with trained examples
- Rapid inference
- Adaptation to new patterns

## Where I Was Wrong

I assumed: *If you have enough good examples and enough failure modes, the network generalizes to the full problem space.*

False at scale. Here's why:

**The failure space is infinite**: In the early 2000s, I could enumerate faults. Dead microphone. Drift. Electromagnetic interference. 

In 2025, adversarial attacks work *because* they exploit the parts of input space you didn't train on. You can't train on all possible adversarial examples.

**Transfer is limited**: Here's the counterintuitive part: training one network architecture to resist attacks doesn't automatically protect a different architecture, even if trained identically.

Example: You develop defensive techniques to protect a CNN (Convolutional Neural Network) against adversarial attacks—carefully designed, imperceptible changes to images that fool the network. You test those defenses, they work.

Then you take the same defensive training and apply it to a Vision Transformer (ViT)—a different architecture that processes images differently—trained on identical data with identical safeguards.

**The problem:** A Vision Transformer "sees" images fundamentally differently than a CNN:
- CNNs process images by examining local neighborhoods of pixels (like moving a window across the image)
- Vision Transformers break images into patches and process them globally (like reading a document page by page)

An adversarial attack designed to exploit *how a CNN processes information* often fails on a Vision Transformer because a ViT *processes information differently*. But an attack designed for Vision Transformers might work perfectly on CNNs.

The vulnerability lives in the **architecture itself**, not just in the training data or methods.

**Why this matters for defense:** If you replicate the same CNN architecture across your redundant systems, one successful adversarial attack defeats all of them simultaneously. But if you mix a CNN with a Vision Transformer as backups, the same attack rarely works on both because they think differently.

**Identical redundancy creates coordinated failure**: Two neural networks trained the same way have nearly identical vulnerabilities. Redundancy fails when the redundant systems fail the same way.

**Architecture shapes what can be learned**: I thought topology was secondary to data. Wrong. The structure of the network—how many layers, what types of layers, how neurons connect—constrains what patterns the network can possibly discover. It's like the difference between a microscope and a telescope: same purpose (seeing), but the design limits what you can observe.

Two networks with identical training data but different architectures will have different learned representations and different vulnerabilities. This is the insight behind heterogeneous architectures: *architectural diversity is a security feature*.

## What Two Decades Taught Me

If I could tell my early 2000s self one thing about neural networks and security:

**Diversity is a security feature.** Architectural diversity isn't overhead—it's part of your defense. When your redundant systems differ in structure, you get genuine robustness. When they're identical, you get coordinated failure.

Also:
- Understand what your network has learned, not just its outputs
- Test for adversarial robustness, not just accuracy
- Your training data shapes the model as much as your architecture
- Interpretability matters for security: if you can't explain a decision, you can't defend it

## The Gap That Matters

Modern AI systems face exactly the problem I didn't anticipate: **robust learning in high-dimensional, adversarial spaces where you can't enumerate failure modes.**

The solution isn't bigger networks and more data. It's structural: heterogeneous architectures, adversarial thinking, interpretability requirements, and systems resilient even when individual components fail.

This is the gap between state-of-the-art and actually safe.

---

**Technical references:**

- Bhatt, T. K., Darvennes, C. M., and Ossanya, E. ["Acoustic Source Location Using a Neural Network,"](https://pubs.aip.org/asa/jasa/article/101/5_Supplement/3057/562137/Acoustic-source-location-using-a-neural-network) *Journal of the Acoustical Society of America*, Vol. 101, pp. 3057, 1997.
- Bhatt, T. K., Darvennes, C. M., and Houghton, J. R. ["Feasibility of Using Imperfect Microphone Arrays in Noise Source Location,"](https://pubs.aip.org/asa/jasa/article/103/5_Supplement/2897/559655/Feasibility-of-using-imperfect-microphone-arrays) *Proceedings of the International Congress on Acoustics and Acoustical Society of America*, Vol. II, pp. 13/17-13/18, Seattle, WA, 1998.
- Bhatt, T. K. "Acoustic Source Location in a Noisy Environment Using a Microphone Array." (1999).
- Goodfellow et al., "Explaining and Harnessing Adversarial Examples" (2014)
- Beye & Kim, "Interpretable Machine Learning" (2019)
