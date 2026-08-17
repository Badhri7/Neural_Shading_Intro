# Intro to Neural Shading

A one-session, hands-on class: train a small neural material model, turn its knobs, and see
what it costs to run.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Badhri7/Neural_Shading_Intro/blob/main/Intro_to_Neural_Shading.ipynb)

The notebook fits a **neural BRDF**: an 8-D latent texture plus a small MLP that a fragment
shader can evaluate in place of an analytic BRDF. It is aimed at graphics engineers who know
what a BRDF, a normal and a fragment shader are, and have never trained a network.

## What you do

1. Pick one of four materials.
2. Look at the 8 numbers per texel the model reads, and the ground truth it has to reproduce.
3. Choose a **loss function**, a **decoder size**, and whether to keep the **non-linearity**.
4. Train it — about 20 seconds — and see the result against held-out cameras.
5. Watch backprop verified by hand: nudge one weight, compare to autograd.
6. See the trained model as what it actually is — 9,955 numbers — re-implemented in a dozen
   lines of numpy, then as the Metal fragment shader that ships.

Total compute is roughly a minute on a Colab T4. Use **Runtime → Change runtime type → T4 GPU**;
a CPU runtime works but gets far fewer training steps.

## The data

The notebook downloads one ~18 MB slice from
[the v1 release](https://github.com/Badhri7/Neural_Shading_Intro/releases/tag/v1). Each slice is
a sample of a Cycles render of one material on a sphere — 200 cameras × 4 directional lights,
~1.38M rows — with the per-camera geometry and the source PBR maps at 256².

Checksums are in `SHA256SUMS.txt` on the release; the release notes document the array schema,
the train/test split, and the two cleaning steps applied.

If the download is blocked on your network, the notebook falls back to asking you to upload the
file into the session.
