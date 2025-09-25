# Runtime Requirements and Setup

This document describes everything needed to run the Autonomous Delivery Agent for automated evaluation. It focuses on non-interactive, reproducible runs across Windows, Linux, and macOS.

## Supported Python
- Python 3.8 – 3.12

## Python Dependencies
Install with pip:

```bash
pip install -r requirements.txt
```

`requirements.txt` includes:
- numpy (numerical routines)
- matplotlib (plots/figures)
- Pillow (GIF writer for optional animations)

## OS-Level Notes
- No system libraries are required for core runs.
- Optional: FFmpeg is only needed if you want to export MP4 animations from `visualization.py`. GIF export works via Pillow without FFmpeg.
  - Windows: You can install FFmpeg via winget/choco (optional)
  - Linux/macOS: Install via your package manager (optional)

## Non-Interactive/Headless Execution
Automated graders often run without a display. Matplotlib uses a non-interactive backend automatically when no display is present. If you need to force headless mode:

```bash
# Before running any scripts
export MPLBACKEND=Agg   # Linux/macOS
setx MPLBACKEND Agg     # Windows (new shells)
```

All scripts that generate figures save them directly to files with `plt.savefig(...)` and do not require a GUI.

## Entry Points
Typical commands (all non-interactive):

```bash
# Create maps used by experiments
python cli.py create-maps

# Compare algorithms on a given map
python cli.py compare --map small --start 0,0 --goal 9,9

# Run a simulation with a chosen strategy
python cli.py simulate --map medium --strategy astar_manhattan --max-steps 500

# Run dynamic replanning demo (logs and figures saved to files)
python dynamic_replanning_demo.py

# Run the full experiment suite and analysis
python cli.py experiment
python cli.py analyze

# Run tests
python -m unittest test_system -v
```

Artifacts (plots, logs, JSON outputs) are written in the project root by default (e.g., `algorithm_success_rates.png`, `comprehensive_analysis.json`).

## Windows-specific Tips
- Use the bundled PowerShell or CMD. Paths with spaces are handled by quoting, e.g. `"D:\\Coding\\Projects\\Work\\Ai Delivery Agent"`.
- If `setx MPLBACKEND Agg` is used, open a new terminal window so the environment variable is visible.

## Performance Considerations
- Numpy and Matplotlib are pure-Python wheels for the supported versions; no compilation is required.
- Large maps or long simulations can increase runtime. Use `--max-steps` or smaller maps for quicker automated checks.

## Troubleshooting
- If GIF saving fails: ensure Pillow is installed (`pip show Pillow`).
- If MP4 saving fails: install FFmpeg or switch to GIF writer in `visualization.py`.
- If plots don’t show in CI: confirm `MPLBACKEND=Agg` and use the non-interactive commands above.
