# Environment

> (optional) Where tasks run. Read by every coder before starting: list what is there,
> what is not, and the limits. Keep it short and current.

## Machine

- OS / CPU / RAM / disk free: <...>
- Tools available: <git, uv, python 3.x, ...>
- Not available: <node, ffmpeg, sudo, GPU, ...>

## Limits

- Writable area: `<project>/` only (no system temp dir, no commands on global caches).
- Heavy environments and caches: prefer the package manager's own cache (e.g. uv's) over
  `<project>/`.
- Network: <allowed | restricted>
- Concurrency: <one coder at a time | max N> (<reason: quota, shared machine>)

## Recipes

- <How to install heavy or tricky dependencies, e.g. a CPU-only build of a large library>
