# Issue

**Title:** Fix Plotly titlefont Deprecation in build_graph.py

## Description

**Summary of the Issue**

When running `build_graph.py` with more recent versions of Plotly (v6+), the layout code attempts to set `titlefont` directly on the Y-axis (`yaxis5`). This property is no longer valid in newer Plotly releases. Instead, Plotly now expects the axis title to be specified with a nested `title` dictionary (e.g., `title=dict(text="...", font={...})`). As a result, the code raises this error:

```
ValueError: Invalid property specified for object of type plotly.graph_objs.layout.YAxis: 'titlefont'
```

**How This PR Fixes the Problem**

- Removes the direct use of `titlefont={"color":"dimgray"}`.
- Replaces it with:
  ```python
  title=dict(text="", font={"color": "dimgray"})
  ```
  which aligns with Plotly’s updated API.

**Testing**

- Confirmed that with this change, no `ValueError` is raised when creating the figure.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5d1ce15bb6c9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.