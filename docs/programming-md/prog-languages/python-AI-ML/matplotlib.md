# Matplotlib & Seaborn

## Basics

Matplotlib is a paramount data visualization library used extensively by data analysts for generating a wide array of plots and graphs.

`Pyplot` is a state-based interface module within the Matplotlib data visualization library for Python that provides a MATLAB-like way of plotting.

---

Matplotlib supports two styles:

- State-based / MATLAB style (`plt.plot(...)`): Implicitly tracks the "current" active figure/subplot. Convenient for quick one-liners, but gets messy and error-prone when managing multiple subplots.
- Object-Oriented style (`fig, ax = plt.subplots()`): Explicitly gives you handles to the figure and axes. It is the recommended best practice because:
  - It makes code clearer when modifying individual subplots (e.g., `ax.set_title()`, `ax.set_xlabel()`).
  - It scales cleanly to grids of subplots (e.g., `fig, (ax1, ax2) = plt.subplots(1, 2)`).

## Parts of a Figure

`Axis` is the axis of the plot, the thing that gets ticks and tick labels. The `axes` is the area your plot appears in.  
In the context of matplotlib, `axes` is not the plural form of `axis`, it actually denotes the plotting area, including all `axis`.

## Seaborn

`seaborn` is a high level interface for drawing statistical graphics with Matplotlib.
