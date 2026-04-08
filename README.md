# SIAM Workshop on Mathematical Modeling in C++

Repository: [SIAM_Workshop](https://github.com/NguyenThoon/SIAM_Workshop)

## Welcome

Welcome to the **SIAM Workshop on Mathematical Modeling in C++**.

This is a **non-credit workshop** structured similarly to a course. Over the span of **10 weeks**, we will work on a single large modeling project while gradually learning both:

* basic **C++ programming**
* the **mathematics** behind the model
* some foundations of **scientific computing**

The goal is for you to build practical experience in computational modeling and contribute to a project that you can later showcase when applying for:

* research positions
* internships
* labs
* graduate school

You do **not** need prior C++ experience. The workshop is designed to be beginner-friendly.

### The model we will study

We will work with an **SCE (Sub-Cellular Element) model** of cell motility.

In this simplified model:

* the **cell membrane** is represented by **60 nodes** arranged in a circular structure
* the **cytoplasm** is represented by **60 randomly distributed internal nodes**

By simulating the interactions between these elements, we can model how cells:

* move
* deform
* interact with their environment

Some ideas from **vector calculus, linear algebra, and probability** may appear, but these are **not prerequisites**. Any math needed for the workshop will be introduced as we go.

---

## Before the first workshop

To follow along with the coding portion of the workshop, please make sure you have the following installed.

### 1. Install Git

Git is the version control system we will use to download code, track changes, and upload work.

* [Install Git](https://github.com/git-guides/install-git)

To verify Git is installed, open a terminal and run:

```bash
git --version
```

If Git is installed correctly, you should see a version number.

### 2. Install Visual Studio Code

You may use another C++ IDE if you prefer, but **VS Code** will be the default editor used in this workshop.

* [Download Visual Studio Code](https://code.visualstudio.com/)

### 3. Install a C++ compiler

VS Code is an editor. You still need a **compiler** to build and run C++ code.

#### Windows

Use one of the following:

* [Configure VS Code for Microsoft C++](https://code.visualstudio.com/docs/cpp/config-msvc)
* [Using GCC with MinGW in VS Code](https://code.visualstudio.com/docs/cpp/config-mingw)

#### macOS

Use one of the following:

* [Using Clang in Visual Studio Code](https://code.visualstudio.com/docs/cpp/config-clang-mac)

You may also need Apple command line tools. In the Terminal, run:

```bash
xcode-select --install
```

### 4. Create a GitHub account

If you do not already have one, sign up here:

* [Create a GitHub account](https://github.com/)

---

## Recommended VS Code extensions

After installing VS Code, open the Extensions tab and install:

* **C/C++** (by Microsoft)
* **C/C++ Extension Pack** (optional but helpful)

These improve syntax highlighting, debugging, IntelliSense, and compilation support.

---

## How to download the workshop code using Git

The easiest way to get the workshop code onto your computer is to **clone** the repository.

### Step 1. Copy the repository URL

```text
https://github.com/NguyenThoon/SIAM_Workshop.git
```

### Step 2. Open a terminal

#### Windows

You can use:

* **PowerShell**
* **Command Prompt**
* the **terminal inside VS Code**

#### macOS

You can use:

* **Terminal**
* the **terminal inside VS Code**

### Step 3. Move to the folder where you want the project saved

For example:

#### Windows

```bash
cd Documents
```

#### macOS

```bash
cd ~/Documents
```

### Step 4. Clone the repository

Run:

```bash
git clone https://github.com/NguyenThoon/SIAM_Workshop.git
```

This creates a new folder named:

```text
SIAM_Workshop
```

### Step 5. Move into the project folder

```bash
cd SIAM_Workshop
```

### Step 6. Open the folder in VS Code

```bash
code .
```

If `code .` does not work, open VS Code manually and choose:

**File -> Open Folder**

Then select the `SIAM_Workshop` folder.

---

## Lesson 0: test your setup

Before we start the main project, we will make sure your computer can compile and run a simple C++ file.

The test file is located in:

```text
lesson_0_test
```

Inside that folder, you should see:

```text
main.cpp
```

This program should print:

```text
Welcome to the SIAM workshop
```

### Open the file in VS Code

After opening the repository in VS Code:

1. Look at the **Explorer** panel on the left.
2. Open the folder **lesson_0_test**.
3. Click on **main.cpp**.

---

## How to run `lesson_0_test`

There are two common ways to do this.

### Option 1: Compile and run from the terminal

Open a terminal in VS Code or on your computer, then move into the lesson folder.

From the main project folder:

```bash
cd lesson_0_test
```

#### macOS (Clang)

Compile:

```bash
clang++ -std=c++17 main.cpp -o main
```

Run:

```bash
./main
```

#### Windows (g++ / MinGW)

Compile:

```bash
g++ -std=c++17 main.cpp -o main.exe
```

Run:

```bash
main.exe
```

#### Windows (MSVC Developer Command Prompt)

Compile:

```bash
cl /EHsc main.cpp
```

Run:

```bash
main.exe
```

If everything is set up correctly, the terminal should display:

```text
Welcome to the SIAM workshop
```

### Option 2: Run from inside VS Code

If your compiler and C/C++ extension are set up correctly:

1. Open `lesson_0_test/main.cpp`
2. Click **Run** or **Run and Debug** in VS Code
3. Follow any prompts to configure your compiler the first time

If VS Code asks you to select a compiler, choose the one you installed.

---

## Troubleshooting

If the test file does **not** run, check the following:

* Git cloned the repository successfully
* you opened the correct folder: `SIAM_Workshop`
* you are inside `lesson_0_test` when compiling from the terminal
* your compiler is installed and available in the terminal
* VS Code has the **C/C++** extension installed

A good test is to run one of these commands in a terminal:

#### Windows (g++)

```bash
g++ --version
```

#### macOS (clang++)

```bash
clang++ --version
```

#### Windows (MSVC)

```bash
cl
```

If the command is not recognized, the compiler is not set up yet.

---

## How to upload your own copy to your own GitHub account

Once you have successfully run `lesson_0_test`, you may want to save your own copy of the repository on your GitHub account.

This is useful if you want to:

* keep your own version of the workshop code
* make edits without affecting the original repository
* back up your work online

### Step 1. Create a new empty repository on GitHub

On GitHub:

1. Log in
2. Click the **+** icon in the upper-right corner
3. Select **New repository**
4. Choose a name, for example:

   * `SIAM_Workshop_My_Copy`
5. Keep it **Public** or **Private**, your choice
6. **Do not** initialize it with a README, `.gitignore`, or license
7. Click **Create repository**

GitHub will then show you a repository URL like:

```text
https://github.com/your-username/SIAM_Workshop_My_Copy.git
```

### Step 2. Check your current remote

In a terminal, go back to the main project folder if needed:

```bash
cd ..
```

Then run:

```bash
git remote -v
```

You should see the original workshop repository listed as `origin`.

### Step 3. Rename the original remote to `upstream`

This keeps a connection to the workshop repository so you can still download future updates.

```bash
git remote rename origin upstream
```

### Step 4. Add your own repository as the new `origin`

Replace `your-username` with your GitHub username:

```bash
git remote add origin https://github.com/your-username/SIAM_Workshop_My_Copy.git
```

### Step 5. Verify your remotes

```bash
git remote -v
```

You should now see:

* `origin` -> your repository
* `upstream` -> the workshop repository

### Step 6. Push the code to your repository

```bash
git push -u origin main
```

If your branch is called `master` instead of `main`, use:

```bash
git push -u origin master
```

After that, refresh GitHub in your browser. You should now see your own copy of the workshop repository.

---

## Basic Git workflow for this workshop

Here are the most common commands you may use.

### See which files changed

```bash
git status
```

### Save changes locally

```bash
git add .
git commit -m "Describe your changes here"
```

### Upload changes to your GitHub repository

```bash
git push
```

### Download new updates from the workshop repository

If you kept the original repository as `upstream`, you can later do:

```bash
git pull upstream main
```

If the workshop uses `master` instead of `main`, use:

```bash
git pull upstream master
```

---

## Suggested checklist before Workshop 1

* [ ] Install Git
* [ ] Install VS Code
* [ ] Install a C++ compiler
* [ ] Create a GitHub account
* [ ] Clone the workshop repository
* [ ] Open the folder in VS Code
* [ ] Open `lesson_0_test/main.cpp`
* [ ] Compile and run the test file
* [ ] Confirm that it prints `Welcome to the SIAM workshop`
* [ ] Push your own copy to your GitHub account

---

## Final note

Regular attendance is encouraged because the project will build from week to week, but materials and code will be shared so you can follow along if you miss a session.

Please bring a laptop if you would like to code during the workshop.

I am looking forward to working with you all.
