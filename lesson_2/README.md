# Week 2: Adding Interior Nodes and Moving to 3D

Welcome to Week 2.

Last week, we learned about polar coordinates and plotted the **cell membrane** for the SCE model. This week, we will plot the **cell interior** and move partially from **2D to 3D**. The cell will still be 2D, but we will now model it in **3D coordinates** by including a `z` value.

---

## Step 1: Download or Update the Repo

If you do not already have the repo, please see the main `README.md` on the main page for instructions on how to download the code.

If you already have the repo, open a terminal in your local `SIAM_Workshop` folder and update your files with:

```bash
git pull
```

This will download the latest changes from the GitHub repo.

---

## Step 2: Change to the `lesson_2` Folder

In terminal, move into the `lesson_2` folder.

### Mac / Linux
```bash
cd lesson_2
```

### Windows
```cmd
cd lesson_2
```

If you are not already in the main `SIAM_Workshop` folder, first navigate there, then run the command above.

---

## The Plan for This Week

Last week, we created the **cell membrane**. This week, we will build on that code by adding **interior nodes**.

There are a few important changes from Week 1.

### Change 1: Make the membrane radius a constant

In Week 1, we modified `R` to change the shape of the plot. Going forward, the cell will be initialized as a **circle only**, so we now make the membrane radius a constant and move it outside the loop.

### Week 1 version
Delete this from the code.
```cpp
//Function to Change//
double R = 1;
//-----------------//
```

and add this

### Week 2 version
```cpp
const double R_membrane = 1.0;
```

Since the radius does not change from node to node, it is cleaner to define it once outside the loop. Copy it to the section with the other `const` variables. 

---

### Change 2: Use two `.dat` files instead of one

In Week 1, we created one data file:

- `circle.dat`

In Week 2, we will create two separate data files:

- `membrane.dat` — stores the membrane node locations
- `interior.dat` — stores the interior node locations

Delete this from the code 
```cpp
std::ofstream file("circle.dat");
```
and replace with
```cpp
std::ofstream membraneFile("membrane.dat");
std::ofstream interiorFile("interior.dat");
```

This keeps the two sets of node data separate and makes plotting easier.

---

You will also need to make one some change to where the data is stored for the membrane node. Inside the for loop for the membrane nodes, change
```cpp
file << x << " " << y << " " << z << "\n";
```
to 
```cpp
membraneFile << x << " " << y << " " << z << "\n";
```
## Generating Interior Nodes

We want the interior nodes to be **inside** the membrane, so we choose a maximum radius for the interior nodes that is smaller than the membrane radius.

```cpp
const double R_interior_max = 0.95;
```

Since the membrane radius is `1.0`, using `0.95` guarantees that the interior nodes stay strictly inside the membrane. Copy this to the variable declaration section.

---

## Random Number Generator

We also want the interior nodes to be distributed randomly.

Here is the random number generator code. Copyt this somewhere in the main body of your code. I reccomend above the for loop that creates the cell membrane.

```cpp
// Random number generator for interior nodes
std::random_device rd;
std::mt19937 gen(rd());
std::uniform_real_distribution<double> angleDist(0.0, 2.0 * PI);
std::uniform_real_distribution<double> radiusDist(0.0, 1.0);
```

Let us go over each part:

### `std::random_device rd;`
This creates a source of randomness used to generate a seed.

### `std::mt19937 gen(rd());`
This creates the actual random number generator. The generator is called `gen`, and it is initialized using the seed from `rd()`.

### `std::uniform_real_distribution<double> angleDist(0.0, 2.0 * PI);`
This creates a distribution for the angle `theta`, so each angle between `0` and `2π` is equally likely.

### `std::uniform_real_distribution<double> radiusDist(0.0, 1.0);`
This creates a distribution for a number between `0` and `1`, which we will use to generate the radius.

---

## Why Do We Use `sqrt(...)`?

When generating interior points, we use:

```cpp
double r = R_interior_max * std::sqrt(radiusDist(gen));
```

instead of something like:

```cpp
double r = R_interior_max * radiusDist(gen);
```

Why?

If we choose the radius uniformly, too many points will cluster near the center.

Using the square root spreads the points more evenly across the **area** of the circle. This gives a more natural distribution of interior nodes throughout the cell.

---

## Create the Interior Nodes

Now we can generate the interior nodes. Copy this code under the section for the membrane node.

```cpp
// Create interior nodes
for (int i = 0; i < N_interior; i++) {
    double theta = angleDist(gen);

    // Generate radius strictly inside the membrane
    double r = R_interior_max * std::sqrt(radiusDist(gen));

    double x = r * std::cos(theta);
    double y = r * std::sin(theta);
    double z = 0.0;

    interiorFile << x << " " << y << " " << z << "\n";
}
```

This code:
1. picks a random angle
2. picks a radius strictly less than the membrane radius
3. converts from polar coordinates to Cartesian coordinates
4. stores the point in `interior.dat`

---
### Final Changes.
Since we changed the `.dat`, we need to make one more change. Delete

```cpp
file.close();
```
and replace with

```cpp
    membraneFile.close();
    interiorFile.close();

    std::cout << "Created membrane.dat and interior.dat\n";
```
## Full Code for `Cell_Interior.cpp`

Here is the full code for reference. Your code should be similar.

```cpp
#include <fstream>
#include <iostream>
#include <cmath>
#include <random>

int main() {
    const int N_membrane = 120;
    const int N_interior = 120;
    const double PI = 3.141592653589793;

    const double R_membrane = 1.0;
    const double R_interior_max = 0.95;

    std::ofstream membraneFile("membrane.dat");
    std::ofstream interiorFile("interior.dat");

    // Random number generator for interior nodes
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_real_distribution<double> angleDist(0.0, 2.0 * PI);
    std::uniform_real_distribution<double> radiusDist(0.0, 1.0);

    // Create cell membrane
    for (int i = 0; i < N_membrane; i++) {
        double theta = 2.0 * PI * i / N_membrane;

        double x = R_membrane * std::cos(theta);
        double y = R_membrane * std::sin(theta);
        double z = 0.0;

        membraneFile << x << " " << y << " " << z << "\n";
    }

    // Create interior nodes
    for (int i = 0; i < N_interior; i++) {
        double theta = angleDist(gen);

        // Generate radius strictly inside the membrane
        double r = R_interior_max * std::sqrt(radiusDist(gen));

        double x = r * std::cos(theta);
        double y = r * std::sin(theta);
        double z = 0.0;

        interiorFile << x << " " << y << " " << z << "\n";
    }

    membraneFile.close();
    interiorFile.close();

    std::cout << "Created membrane.dat and interior.dat\n";

    return 0;
}
```

---

## Step 3: Compile the Code

Compile and run your code.

This should create two files in your folder:

- `membrane.dat`
- `interior.dat`

---

## Step 4: Plot the Data in GNUplot

Next, we will plot the data in GNUplot.

Open GNUplot in the same folder as the data files, or open a terminal in that folder and launch GNUplot.

Then paste in the following script:

```gnuplot
set xrange [-1.5:1.5]
set yrange [-1.5:1.5]
set zrange [-1:1]
set xyplane at 0
set view 60,30

splot "membrane.dat" using 1:2:3 with points pt 7 ps 1 lc rgb "black", \
      "interior.dat" using 1:2:3 with points pt 7 ps 1 lc rgb "red"
```

### What this does
- `set xrange`, `set yrange`, `set zrange` control the viewing window
- `set xyplane at 0` places the base plane at `z = 0`
- `set view 60,30` changes the viewing angle
- `splot` creates a 3D plot
- the membrane nodes are plotted in black
- the interior nodes are plotted in red

---

## Summary

This week we:
- kept the membrane as a circle
- changed the membrane radius to a constant
- added interior nodes
- stored membrane and interior data in separate files
- introduced random interior node placement
- moved from a 2D plot to a 3D visualization

Even though all nodes still lie in the plane `z = 0`, using 3D plotting prepares us for future lessons where the cell may sit on a curved surface.

---
