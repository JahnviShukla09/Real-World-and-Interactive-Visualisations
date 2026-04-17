
# 📊 Experiment 18 — Real-World and Interactive Visualizations

**Name:** Jahnvi Shukla <br>
**PRN:** 25070123055<br>
**Branch:** ENTC A3

---

## 🎯 Objectives

- Explore advanced, real-world-applicable visualization techniques beyond basic plots.
- Understand how to choose the **right chart type** for a given dataset and question.
- Gain hands-on experience with **Plotly** (interactive) and **Matplotlib** (static) ecosystems.
- Build visual intuition for hierarchical data, flow data, cluster data, and multi-attribute data.

---

## 🛠️ Libraries & Tools Used

| Library | Purpose |
|---|---|
| `pandas` | Data creation and DataFrame management |
| `plotly.express` | High-level interactive charts (Treemap, 3D Scatter) |
| `plotly.graph_objects` | Low-level interactive charts (Sankey, Radar) |
| `matplotlib.pyplot` | Static chart rendering |
| `matplotlib_venn` | Venn diagram rendering |
| `numpy` | Numerical data generation for clustering |
| `scipy.cluster.hierarchy` | Hierarchical clustering and dendrogram computation |

---

## 📌 Visualizations — In Detail

---

### 1. 🗂️ Treemap — Company Budget Distribution

**What it does:**
A Treemap encodes quantitative data as nested rectangles. The area of each rectangle is proportional to its value, making it immediately obvious which category dominates.

**Dataset:**
```
Department  | Budget (₹)
HR          | 20,000
IT          | 50,000
Sales       | 40,000
Marketing   | 30,000
```

**Key insight:**
IT commands the largest budget at ₹50,000. The treemap makes this hierarchy *visceral* — IT's rectangle dwarfs HR's in a way that numbers alone never quite do.

**Why Treemap over Pie Chart?**
Treemaps scale beautifully to dozens of categories. Pie charts become unreadable past ~6 slices. For organizational or financial hierarchies, treemaps are the professional standard.

```python
fig = px.treemap(df, path=['Department'], values='Budget',
                 title="Company Budget Distribution")
```

---

### 2. 🌲 Dendrogram — Hierarchical Clustering

**What it does:**
A Dendrogram is the visual output of **hierarchical (agglomerative) clustering**. It is a tree-like diagram showing how data points progressively merge into clusters. The height at which two branches merge indicates how dissimilar those clusters are — higher merge = greater distance.

**Algorithm:** Ward's linkage — minimizes total within-cluster variance at each merge step, producing compact, well-separated clusters.

**Reading the output:**
Points 1 and 2 merge first (closest in feature space), then point 3 joins that cluster, and finally the remaining points aggregate. The y-axis (distance) tells you *how much* the merged clusters differ.

**Real-world applications:** Gene expression analysis, document clustering, customer segmentation, anomaly detection.

```python
linked = linkage(data, method='ward')
dendrogram(linked)
```

---

### 3. 🔵 Venn Diagram — Set Intersection

**What it does:**
A Venn Diagram visualizes the logical relationships between sets — what is unique to each and what they share.

```
Set A = {1, 2, 3, 4}
Set B = {3, 4, 5, 6}

A only:      {1, 2}  → 2 elements
Shared A∩B:  {3, 4}  → 2 elements
B only:      {5, 6}  → 2 elements
```

Both sets share exactly 2 elements — a 50% overlap, indicating substantial similarity. The Venn diagram communicates this in a single glance.

**Real-world applications:** Database JOIN visualization, biology (shared gene expression), marketing audience overlap analysis, logic education.

```python
venn2([A, B], set_labels=('Set A', 'Set B'))
```

---

### 4. 🌊 Sankey Diagram — Student Academic Flow

**What it does:**
A Sankey Diagram visualizes **flow and quantity** through a system. The width of each band is proportional to the number of units flowing through that stage, making losses and transitions immediately visible.

**Flow modelled:**
```
Admission      → 100 students
First Year     → 80  students   (20 dropped out / transferred)
Second Year    → 60  students   (20 further attrition)
Placed         → 60  students   (all second-year students placed!)
```

**Key insight:** A 40% attrition from admission to second year is a stark institutional metric. This kind of flow analysis is invaluable for tracking student retention, spotting at-risk transition points, and planning interventions.

**Real-world applications:** Energy flow, supply chain, budget allocation, website user journey funnels, sales pipeline analysis.

```python
fig = go.Figure(data=[go.Sankey(
    node=dict(label=["Admission", "First Year", "Second Year", "Placed"]),
    link=dict(source=[0,1,2], target=[1,2,3], value=[100,80,60])
)])
```

---

### 5. 🔭 3D Scatter Plot — Student Performance Analysis

**What it does:**
A 3D Scatter Plot places data points in three-dimensional space, enabling simultaneous analysis of **three variables**. Each point represents one student; their position encodes Study Hours (X), Marks (Y), and Attendance (Z).

**Dataset:**
```
Student | Study Hours | Marks | Attendance
A       | 2           | 50    | 60%
B       | 4           | 65    | 75%
C       | 6           | 75    | 80%
D       | 8           | 90    | 90%
E       | 5           | 70    | 70%
```

**Key insight:** A clear positive correlation exists across all three dimensions — students who study more, attend more, and score more cluster together in the top-far-right region of 3D space. The Plotly version is rotatable and hoverable, making the relationship exploration interactive.

---

### 6. 🕸️ Radar Chart — Student Skill Assessment

**What it does:**
Also known as a **Spider Chart**, a Radar Chart plots multiple quantitative variables on axes radiating from a central point. The polygon formed by connecting the values reveals the overall skill profile — strong skills push the shape outward, weak skills pull it inward.

**Skills assessed:**
```
Skill            | Score (out of 5)
Python           | 4
Machine Learning | 3
DBMS             | 5
DSA              | 4
Communication    | 3
```

**Key insight:** DBMS is the standout strength (5/5), Python and DSA are solid (4/5), while ML and Communication have room for growth (3/5). The asymmetric polygon immediately communicates a technically strong, communication-developing profile — far more expressively than a table of numbers.

**Real-world applications:** Employee performance reviews, athlete fitness profiles, product feature comparison, personality assessments.

```python
fig.add_trace(go.Scatterpolar(
    r=values, theta=skills,
    fill='toself', name='Student Skills'
))
```

---

## 📐 Visualization Selection Guide

One of the most important skills in data science is knowing *which chart to use when*. Here's the framework demonstrated across this experiment:

| Scenario | Best Chart | Used In |
|---|---|---|
| Part-to-whole with many categories | Treemap | #1 |
| Cluster similarity and grouping | Dendrogram | #2 |
| Set membership and logical overlap | Venn Diagram | #3 |
| Flow and attrition through stages | Sankey Diagram | #4 |
| Three-variable relationship | 3D Scatter Plot | #5 |
| Multi-attribute profile comparison | Radar Chart | #6 |

---

## 🧠 Conclusion

This experiment demonstrates that **visualization is a form of communication**. The choice of chart type is not aesthetic — it is semantic. Each chart type encodes information in a fundamentally different way:

- **Treemaps** encode quantity as *area*.
- **Dendrograms** encode similarity as *tree height*.
- **Venn diagrams** encode set logic as *spatial overlap*.
- **Sankey diagrams** encode flow as *band width*.
- **3D scatters** encode multi-dimensional correlation as *spatial position*.
- **Radar charts** encode multi-attribute profiles as *polygon shape*.

