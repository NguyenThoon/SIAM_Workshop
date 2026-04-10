# Lesson 1: Plotting in Polar Coordinates

In this lesson, we will use C++ to generate points from **polar equations** and save them into a `.dat` file that can be plotted.

For this lesson, we will be visualizing the plots with **GNUplot**.

Once we generate our `.dat` file, we will use GNUplot to view it.

---

## Installing GNUplot

### Mac users: Install Homebrew
1. Download and install the `.pkg` installer from the homebrew webpage.
[Homebrew](https://brew.sh/)

or

1. Open the **Terminal** app.
2. Install **Homebrew** with:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

3. Follow any post-install instructions shown in Terminal.
4. Check that Homebrew works:

```bash
brew --version
```

5. Install GNUplot with:

```bash
brew install gnuplot
```

6. Check that GNUplot works:

```bash
gnuplot
```

If GNUplot opens, the installation worked.

### Windows users

1. Go to the GNUplot download page and download the Windows installer. [GNUplot](http://gnuplot.info/)
2. Run the installer.
3. If you see an option to add GNUplot to your **PATH**, check that box.
4. Finish the installation.
5. Open **Command Prompt**.
6. Type:

```cmd
gnuplot
```

If GNUplot opens, the installation worked.

If `gnuplot` is not recognized, then GNUplot may not have been added to your PATH. In that case, you can either reinstall it and enable the PATH option, or run it directly from its installed location, for example:

```cmd
"C:\Program Files\gnuplot\bin\wgnuplot.exe"
```

## How to experiment
When compiling a code using VScode, make sure to change the directory to the correct folder before running your code. For lesson 1, copy this into VScode terminal.

```bash
cd /SIAM_Workshop/lesson_1
```
Each time you want to try a new shape:

1. Change the formula for `R`
2. Recompile and run the program
3. Generate a new `circle.dat` file
4. Plot the new `circle.dat` file in GNUplot
5. Observe the shape

---

## Opening GNUplot in the correct folder

You want GNUplot to open in the same folder that contains your `circle.dat` file.

### Mac users

1. Open **Terminal**
2. Use the `cd` command to move into your lesson folder

Example:

```bash
cd /path/to/your/SIAM_Workshop/lesson_1
```

3. Start GNUplot:

```bash
gnuplot
```

You can also change folders **inside** GNUplot by typing:

```gnuplot
cd '/path/to/your/SIAM_Workshop/lesson_1'
```

### Windows users

1. Open **Command Prompt**
2. Use the `cd` command to move into your lesson folder

Example:

```cmd
cd C:\Users\YourName\Documents\GitHub\SIAM_Workshop\lesson_1
```

3. Start GNUplot:

```cmd
gnuplot
```

You can also change folders **inside** GNUplot by typing:

```gnuplot
cd 'C:\Users\YourName\Documents\GitHub\SIAM_Workshop\lesson_1'
```

Note: inside GNUplot, it is safest to put the folder name in quotes.

---

## Plotting the data directly in GNUplot

After GNUplot opens, try these commands.

### 2D plot

```gnuplot
plot 'circle.dat' using 1:2 with points lc rgb "black" pt 7 ps 1.2
```

This tells GNUplot to use:

- column 1 for $x$
- column 2 for $y$

### 3D plot

```gnuplot
splot 'circle.dat' using 1:2:3 with points lc rgb "black" pt 7 ps 1.2
```

This tells GNUplot to use:

- column 1 for $x$
- column 2 for $y$
- column 3 for $z$

Since your current code sets `z = 0`, the 3D plot will still lie in a flat plane.

---


## What are polar coordinates?

In Cartesian coordinates, we describe a point using $(x,y)$.

In polar coordinates, we describe a point using:

- $r$: the distance from the origin
- $\theta$: the angle from the positive $x$-axis

To convert polar coordinates into Cartesian coordinates, we use

$$
x = r\cos\theta, \qquad y = r\sin\theta
$$

That is exactly what this code is doing.

---

## The basic idea of this program

The program loops through many values of $\theta$, computes a radius `R`, and then converts that into $(x,y,z)$ coordinates.

Right now, the important part is this section:

```cpp
//Function to Change//
double R = 1;
//-----------------//
```

This is where you control the shape.

If `R` is constant, the program makes a circle.

If `R` depends on $\theta$, the program makes more interesting polar curves.

---

## Starting simple


## Example: Circle of radius 2

Change the code to

```cpp
double R = 2;
```

Run the program again and compare the result.

---
## Example: Archimedian Curve
This code will plot a spiral.
```cpp
double a = .3;
double R = a * theta;
```
What happens when the sign of `a` changes?
---
## Example: Limaçon

A limaçon can be written as

$$
r = a \pm b\cos\theta
$$

Try this in the code:

```cpp
double a = 1.0;
double b = 2.0;
double R = a + b * std::cos(theta);
```

You can also try

```cpp
double R = a - b * std::cos(theta);
```

### Questions to explore

Try different values of `a` and `b`.

- What happens when `a = b`?
- What happens when `1 < a/b < 2`?
- What happens when `a/b > 2`?
- When do you get an inner loop?
- How does changing `+` to `-` affect the graph?

---

## Example: Flower (Rose Curve)

A flower-shaped curve can be written as

$$
r = a\cos(n\theta)
$$

Try this code:

```cpp
double a = 1.0;
int n = 5;
double R = a * std::cos(n * theta);
```

### Questions to explore

Try different values of `n`.

- What happens when `n` is odd?
- What happens when `n` is even?
- How many petals do you get in each case?
- What happens when you increase `a`?

---


### Questions to explore

- What happens if you use `1 - cos(theta)` instead?
- What happens if you replace `cos(theta)` with `sin(theta)`?
- How does the orientation of the shape change?

---

## Example: Star-like shape

Try this star shaped object?

```cpp
double a = 1.0;
double b = 0.3;
int n = 5;
double R = a +b* std::cos(n * theta);
```

Can you make it a square?

---


## Challenge idea
---
### Mystery Shape

Using the change of variables

$$
x = r\cos(\theta), \qquad y = r\sin(\theta),
$$

convert the curve

$$
(x^2+y^2)^3 - x^2y^3 = 0
$$

into polar coordinates, then use our plotting setup to visualize it.

Hint: You will have squared or cubed terms? how do you write this as multiplication?
---

## Summary

In this lesson, the goal is simple:

- understand that polar coordinates use radius and angle
- change the radius formula
- run the program
- generate the `.dat` file
- use GNUplot to view the points
- see how the graph changes

The most important part of the code is the formula for `R`.

By changing that one line, you can create many different shapes.

Enjoy experimenting!
