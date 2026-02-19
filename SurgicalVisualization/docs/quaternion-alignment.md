# Quaternion Alignment — Precision Orientation Without Gimbal Lock

Euler rotations can introduce instability and gimbal lock in repeated orientation workflows.

This project uses quaternion math to:
- preserve rotational precision
- support deterministic alignment to a known target vector
- maintain stable orientation across repeated operations

## Alignment Outline
1. Normalize source axis (principal direction)
2. Normalize target axis
3. Compute quaternion rotation from source → target
4. Apply rotation deterministically
5. Log resulting orientation + metrics
