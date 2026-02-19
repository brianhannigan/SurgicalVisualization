# Technical Deep Dive — Deterministic 3D Inspection in WPF

This project is intentionally engineered for **repeatability** under controlled conditions.

## Thread-Isolated Model Construction (STA)
WPF 3D objects are created on a dedicated STA thread to avoid cross-thread ownership issues.  
After construction, all geometry is **deep-frozen** before reaching the UI thread.

## Deterministic Metrics
All mesh metrics are computed using `double` precision:
- triangle count
- bounding box extents
- center of mass

Values are formatted using invariant culture and logged for traceability.

## Why This Matters
In controlled environments (surgical simulation, robotics validation, regulated research), reproducibility and auditability matter as much as visual fidelity.
