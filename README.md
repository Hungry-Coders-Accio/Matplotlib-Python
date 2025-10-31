# Matplotlib - Step‑by‑Step Guide

Dear Students. 
These notes are your go‑to reference for learning and using Matplotlib effectively. We will keep it practical, structured, and example‑driven so you can plug this into your projects and assignments immediately.

---

## 1. What is Matplotlib and When to Use It

Matplotlib is Python’s foundational plotting library. It gives you full control over every element of a figure and is the base for many higher‑level libraries like Seaborn. Use Matplotlib when you need:
1. Maximum control over plot elements
2. Publication‑quality figures
3. Custom or uncommon plot types
4. To understand how plotting works under the hood

---

## 2. Installation and Setup

```bash
pip install matplotlib
```

Recommended environment: Jupyter Notebook or JupyterLab.

In a notebook, enable inline plots:
```python
%matplotlib inline
```

Basic import:
```python
import matplotlib.pyplot as plt
import numpy as np
```

---

## 3. Two Ways to Plot: Stateful vs Object‑Oriented (OO)

### 3.1 Stateful (Quick and Simple)
Good for quick one‑off figures.
```python
x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y, label="sin")
plt.title("Sine Wave")
plt.xlabel("x")
plt.ylabel("sin(x)")
plt.legend()
plt.grid(True)
plt.show()
```

### 3.2 Object‑Oriented (Recommended for Real Projects)
Gives full control and scales to complex layouts.
```python
fig, ax = plt.subplots(figsize=(6, 4))
ax.plot(x, y, label="sin")
ax.set_title("Sine Wave")
ax.set_xlabel("x")
ax.set_ylabel("sin(x)")
ax.grid(True)
ax.legend()
plt.show()
```

Key idea: In OO, you always work with `Figure` and `Axes` objects (`fig`, `ax`).

---

## 4. The Figure Anatomy You Must Know

- Figure: The entire canvas
- Axes: The actual plotting area
- Axis: The x‑axis or y‑axis inside an Axes
- Artist: Any visible element on the figure, like lines, text, patches

Understanding this helps you customize any part of the plot.

---

## 5. Core Plot Types You’ll Use Daily

### 5.1 Line Plot
```python
fig, ax = plt.subplots()
ax.plot(x, np.sin(x), label="sin")
ax.plot(x, np.cos(x), label="cos", linestyle="--")
ax.set_title("Line Plot")
ax.set_xlabel("x")
ax.set_ylabel("y")
ax.legend()
ax.grid(True)
plt.show()
```

### 5.2 Scatter Plot
```python
rng = np.random.default_rng(0)
x = rng.normal(0, 1, 200)
y = rng.normal(0, 1, 200)
sizes = rng.integers(10, 200, 200)
colors = rng.random(200)

fig, ax = plt.subplots()
sc = ax.scatter(x, y, s=sizes, c=colors, alpha=0.7)
cbar = plt.colorbar(sc, ax=ax)
cbar.set_label("Color Scale")
ax.set_title("Scatter Plot")
ax.set_xlabel("x")
ax.set_ylabel("y")
plt.show()
```

### 5.3 Bar and Stacked Bar
```python
cats = ["A", "B", "C", "D"]
vals1 = [5, 7, 3, 4]
vals2 = [2, 3, 4, 1]

x = np.arange(len(cats))

fig, ax = plt.subplots()
ax.bar(x, vals1, label="Group 1")
ax.bar(x, vals2, bottom=vals1, label="Group 2")
ax.set_xticks(x, cats)
ax.set_title("Stacked Bar Chart")
ax.legend()
plt.show()
```

### 5.4 Histogram and Density‑Like View
```python
data = rng.normal(50, 10, 1000)

fig, ax = plt.subplots()
ax.hist(data, bins=30, alpha=0.8, edgecolor="black")
ax.set_title("Histogram")
ax.set_xlabel("Value")
ax.set_ylabel("Frequency")
plt.show()
```




### 5.6 Error Bars
```python
x = np.arange(1, 6)
y = np.array([3.4, 4.1, 5.2, 4.8, 5.6])
err = np.array([0.2, 0.3, 0.15, 0.2, 0.25])

fig, ax = plt.subplots()
ax.errorbar(x, y, yerr=err, fmt="o-", capsize=4)
ax.set_title("Error Bars")
ax.set_xlabel("Trial")
ax.set_ylabel("Measurement")
plt.show()
```

### 5.7 Images and Heatmaps
```python
mat = rng.normal(0, 1, (10, 10))

fig, ax = plt.subplots()
im = ax.imshow(mat, aspect="auto")
cbar = plt.colorbar(im, ax=ax)
cbar.set_label("Intensity")
ax.set_title("Heatmap")
plt.show()
```

---

## 6. Subplots and Layouts

### 6.1 Quick Grid of Subplots
```python
fig, axes = plt.subplots(2, 2, figsize=(8, 6), sharex=True, sharey=True)
axes = axes.ravel()
for i, ax in enumerate(axes):
    ax.plot(np.sin(x + i))
    ax.set_title(f"Plot {i+1}")
fig.suptitle("Grid of Subplots", y=1.02)
fig.tight_layout()
plt.show()
```


## 7. Styling: Titles, Labels, Ticks, Legends, Grids

```python
fig, ax = plt.subplots()
ax.plot(x, np.sin(x), label="sin")
ax.set_title("Custom Title", loc="left", fontsize=14, fontweight="bold")
ax.set_xlabel("Time")
ax.set_ylabel("Signal")

ax.legend(frameon=True, title="Functions")
ax.grid(True, which="major")
ax.minorticks_on()
ax.tick_params(axis="x", rotation=0)
plt.show()
```

### Working with Limits and Aspect
```python
ax.set_xlim(0, 10)
ax.set_ylim(-1.5, 1.5)
ax.set_aspect("auto")
```

### Text and Annotations
```python
fig, ax = plt.subplots()
ax.plot(x, np.sin(x))
ax.annotate("Peak", xy=(np.pi/2, 1), xytext=(2, 1.3),
            arrowprops=dict(arrowstyle="->"))
plt.show()
```

---

## 8. Colormaps and Colorbars

- Use `plt.colormaps()` to list available colormaps
- Choose perceptually uniform maps for quantitative data
- Use discrete colors for categories

```python
mat = rng.random((20, 20))
fig, ax = plt.subplots()
im = ax.imshow(mat)
plt.colorbar(im, ax=ax, label="Scale")
plt.show()
```

---

## 9. Dates, Categories, and Strings

### 9.1 Dates
```python
import pandas as pd

dates = pd.date_range("2024-01-01", periods=30, freq="D")
vals = np.cumsum(rng.normal(0, 1, size=30))

fig, ax = plt.subplots(figsize=(8, 4))
ax.plot(dates, vals, marker="o")
fig.autofmt_xdate()
ax.set_title("Time Series")
plt.show()
```

### 9.2 Categories
```python
cats = ["Low", "Med", "High"]
vals = [10, 35, 25]

fig, ax = plt.subplots()
ax.bar(cats, vals)
ax.set_title("Categorical Bar")
plt.show()
```

---

## 10. Exporting Figures Correctly

Save with high resolution and appropriate format.
```python
fig, ax = plt.subplots()
ax.plot(np.sin(x))
fig.tight_layout()
fig.savefig("plot.png", dpi=300, bbox_inches="tight")
fig.savefig("plot.pdf", bbox_inches="tight")
```

Common formats: PNG, PDF, SVG, JPEG. Prefer vector formats for print like PDF or SVG.

---


## 12. Interactive Use and Backends

In Jupyter:
- `%matplotlib inline` for static images
- `%matplotlib notebook` for basic interactivity
- `%matplotlib widget` for ipywidgets‑based interactivity

Outside notebooks, Matplotlib uses interactive backends depending on your OS and environment.

---

## 13. Performance Tips

1. Reuse `Figure` and `Axes` objects instead of creating new figures in loops
2. Update existing artists rather than replotting from scratch
3. Limit the number of points on screen when possible
4. Use raster formats for extremely large scatter plots


---

## 14. Common Pitfalls and How to Avoid Them

1. Mixing stateful and OO styles in the same cell. Pick one
2. Calling `plt.show()` repeatedly in notebooks. One per figure is enough
3. Forgetting `tight_layout` or `constrained_layout` causing clipped labels
4. Using too many colors in one plot. Focus on readability
5. Saving figures without specifying `dpi` or `bbox_inches="tight"`

---

## 15. Mini Exercises for Practice

1. Create a line chart of `y = x^2` for x from 0 to 10 with grid and labels
2. Plot a scatter of 300 random points with a colorbar and varying sizes
3. Make a 2x2 subplot grid with four different trigonometric plots
4. Draw a stacked bar chart for three categories and two groups
5. Plot a histogram of 1000 random values with 40 bins and black edges
6. Create a time series plot using a date range of 60 days and cumulative sum
7. Add an annotation arrow to the highest point in a line plot
8. Save any figure as both PNG and PDF with high resolution

Try to implement each task in OO style using `fig, ax = plt.subplots()`.

---

## 16. Quick Reference

- Create figure and axes
  ```python
  fig, ax = plt.subplots(nrows=1, ncols=1, figsize=(6,4))
  ```
- Plotting
  ```python
  ax.plot(x, y, label="name")
  ax.scatter(x, y, s=50, c=y)
  ax.bar(labels, values)
  ax.hist(values, bins=30)
  ```
- Labels and legend
  ```python
  ax.set_title("Title")
  ax.set_xlabel("X")
  ax.set_ylabel("Y")
  ax.legend()
  ax.grid(True)
  ```
- Ticks and limits
  ```python
  ax.set_xticks([0, 2, 4, 6, 8, 10])
  ax.set_xlim(0, 10)
  ax.set_ylim(-1, 1)
  ```
- Save
  ```python
  fig.tight_layout()
  fig.savefig("figure.png", dpi=300, bbox_inches="tight")
  ```

---

## 17. How to Choose the Right Plot

- Trend over a continuous variable: Line plot
- Relationship between two numeric variables: Scatter
- Distribution of a single variable: Histogram, Box, Violin
- Compare categories: Bar, Stacked Bar
- Part‑to‑whole for a few categories: Bar with normalized heights
- Heat or intensity data on a grid: imshow or pcolormesh

---

## 18. FAQ

1. Why are my labels getting cut off
   - Use `fig.tight_layout()` or `constrained_layout=True` in `plt.subplots()`

2. Why does my legend cover the plot
   - Move it with `ax.legend(loc="best")` or place outside using `bbox_to_anchor`

3. Why is my plot slow
   - Reduce points, avoid complex markers, reuse artists, consider raster output

---

## 19. Next Steps

- Recreate charts from your favorite articles using Matplotlib
- Learn `Axes` methods from the official API reference
- Explore `matplotlib.animation` for simple animations
- Combine with NumPy and Pandas for efficient data visualization

If you practice the mini exercises and follow the patterns shown here, you will be comfortable with Matplotlib in real projects.
