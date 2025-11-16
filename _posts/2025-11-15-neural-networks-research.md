---
layout: post
title: "What We've Built: Lessons on Robustness from Neural Networks Research"
nav_title: "Robustness from Neural Networks Research"
date: 2024-11-15 15:00:00
permalink: /neural-networks-research/
category: machine learning
---
In the early 2000s, I published research on using neural networks to locate noise sources with faulty microphone arrays in *The Journal of the Acoustical Society of America*.

That work planted seeds that have grown into something bigger. Two decades later, those same principles are scaling to solve real-world problems at enormous complexity. But the journey from simple acoustic arrays to modern AI systems has taught us something essential: **what we build depends on understanding not just what works, but why it works.**

### The Technical Approach

I processed SISO (Single Input Single Output) microphone signals using FFT (Fast Fourier Transform) analysis on unwrapped phase data to extract Time Difference of Arrival (TDOA) information. A Matched Filter Array (MFA) pipeline applied coherence filtering to clean noisy phase plots. Neural networks then learned patterns in residual errors to identify and localize faulty microphones.

**The core finding**: Neural networks didn't just detect faults—they localized them, learning that measurement corruption has statistical signatures without being programmed to look for specific fault types.

**The insight**: Neural networks excel at finding patterns in redundant systems trained on examples of correct behavior and failure modes.

### Fast Forward to Today

Scale the problem: autonomous vehicles with dozens of sensors (cameras, LiDAR (Light Detection and Ranging), radar, and IMU (Inertial Measurement Unit)). Some are faulty. Some are attacked. You need ground truth about the world.

The same approach applies: train neural networks on sensor fusion data with examples of correct and corrupted measurements. The network learns to detect anomalies.

**But here's what changes**: In the early 2000s, the acoustic system was simple enough to understand *why* the network decided what it did. Small input space. Stationary signals. Characterized noise.

In modern systems, sensor fusion is genuinely enormous:

- **1000+ dimensional input space**: Each camera produces thousands of pixel values. LiDAR creates point clouds with millions of 3D coordinates. Radar data, GPS signals, and inertial measurement readings add thousands more variables. Compare this to early 2000s: a handful of microphones, a few measurements. The complexity is orders of magnitude higher.

- **Non-stationary signals**: In acoustic work, sound traveled predictably—the physics was stable. In real-world driving, pedestrians appear suddenly, weather changes instantly, lighting shifts, road surfaces vary. The "rules" of what sensor data should look like are constantly changing.

- **Adversarial noise (not random)**: In the 2000s, microphones failed randomly—dead sensors, drift, electromagnetic interference. Predictable failure modes. Today, attackers deliberately craft false sensor readings designed specifically to fool the system. This is intentional deception, not random malfunction.

The network solves the problem. We can't explain how.

### What We Got Right

Neural networks are exceptional at anomaly detection in redundant systems with known failure modes. They distinguish random failures from systematic attacks, sensor noise from adversarial input.

**Where they excel:**
- Pattern recognition in high-dimensional data
- Anomaly detection with trained examples
- Rapid inference
- Adaptation to new patterns

### What We've Built On

I assumed: *If you have enough good examples and enough failure modes, the network generalizes to the full problem space.*

This assumption worked at small scale. But scaling it revealed something crucial: the world is more complex than our training data. That's not a failure—it's a feature of system design. Understanding that gap is what led us to build better systems.

This taught us:

**The failure space is infinite, so we design for adaptation**: In the early 2000s, I could enumerate faults. Dead microphone. Drift. Electromagnetic interference. 

Today, adversarial attacks work *because* they exploit the parts of input space you didn't train on. That's the insight: you can't train on all possible adversarial examples. So instead, we build systems that adapt, diversify, and recognize when they're outside their training distribution.

**Architecture matters more than we thought**: Training on one architecture doesn't immunize other architectures. This became our strength: **architectural diversity is a defense mechanism.**

Here's why: A CNN (Convolutional Neural Network) processes images by examining local neighborhoods of pixels. A Vision Transformer (ViT) breaks images into patches and processes them globally. An adversarial attack designed for one often fails on the other—not because of better training, but because they *think differently*. Different architectures have different vulnerabilities. Mix them, and you get genuine robustness.

**Identical redundancy fails together**: Two neural networks trained the same way have nearly identical vulnerabilities. But heterogeneous systems—mixing different architectures, training approaches, even different sensor types—fail differently. That's where real resilience lives.

**Structure constrains what can be learned**: The network architecture—how many layers, what types of layers, how neurons connect—shapes what patterns the network can discover. It's like the difference between a microscope and a telescope: same purpose (seeing), but the design constrains what you can observe. This isn't a limitation we work around; it's a design principle we leverage.

## What Two Decades Have Built

The core insight that has guided us: **diversity is a security feature.** This isn't new—engineers have known this for decades. But applying it to neural networks, understanding *why* it works, and building systems around it—that's what we've learned.

When redundant systems differ in structure, you get genuine robustness. When they're identical, coordinated failure is inevitable. That simple principle has shaped how we think about AI systems at scale.

From research to practice:
- Understand what your network has learned, not just its outputs
- Test for adversarial robustness, not just accuracy on test sets
- Architectural diversity isn't overhead—it's part of your defense
- Your training data shapes the model as much as your architecture
- Interpretability matters: if you can't explain a decision, you can't defend it

### The Frontier Ahead

Modern AI systems face a challenge we're learning to solve: **building robust learning in high-dimensional, adversarial spaces where you can't enumerate all failure modes.**

The naive approach—bigger networks, more data—doesn't solve it. The structural approach does: heterogeneous architectures, adversarial thinking, interpretability requirements, and systems designed to be resilient even when individual components fail.

This is where research becomes reality. This is what we're building.

---

**Technical references:**
- Bhatt, T. K., Darvennes, C. M., and Ossanya, E. ["Acoustic Source Location Using a Neural Network,"](https://pubs.aip.org/asa/jasa/article/101/5_Supplement/3057/562137/Acoustic-source-location-using-a-neural-network) *Journal of the Acoustical Society of America*, Vol. 101, pp. 3057, 1997.
- Bhatt, T. K., Darvennes, C. M., and Houghton, J. R. ["Feasibility of Using Imperfect Microphone Arrays in Noise Source Location,"](https://pubs.aip.org/asa/jasa/article/103/5_Supplement/2897/559655/Feasibility-of-using-imperfect-microphone-arrays) *Proceedings of the International Congress on Acoustics and Acoustical Society of America*, Vol. II, pp. 13/17-13/18, Seattle, WA, 1998.
- Bhatt, T. K. "Acoustic Source Location in a Noisy Environment Using a Microphone Array." Master's thesis, Tennessee Technological University (1999).
- Goodfellow et al., "Explaining and Harnessing Adversarial Examples" (2014)
- Beye & Kim, "Interpretable Machine Learning" (2019)
