# Zalando's Gift Problem – Easy Step-by-Step Explanation

## The Goal

Ahmad wants to buy the **most expensive gift** possible for Sergey’s birthday.  
However, his backpack can only hold **40 liters**.

### What we know
- The **price** of every item (`items.json`)
- Many past packages and their **measured total volume** (`packages.json`)

### What we don’t know
- The real volume of each individual item  
  (the measuring machine has some random error)

---

## Step 1: Load the data

**What we do:**  
We open the two JSON files and read the lists of items and packages.

**Why we do it:**  
We need the prices and the package information before we can do any calculations.  
This is just preparing the raw material.

---

## Step 2: Build a big table of equations

Imagine every past package is a sentence like this:

> “The items A28 + A3 + A33 + A59 together measured 36.04 liters”

We create a big mathematical table (called matrix \( A \)) where:
- Each **row** = one package
- Each **column** = one item (A1, A2, … A60)
- We put a `1` if the item was in that package, and `0` if it was not

We also make a list \( V \) that contains all the measured volumes.

**Why we do it:**  
We turn the 1000 packages into 1000 equations.  
This is the only information we have about the volumes, so we must use it.

---

## Step 3: Estimate the volume of every single item

We ask the computer:

> “Find the volume for each item so that when I add the volumes of the items in a package, I get as close as possible to the measured volume.”

This is done with a method called **Least Squares**.

**Why we do it:**  
The measuring machine is not perfect (it has random error).  
By using *all* 1000 packages at once, the random errors mostly cancel out, and we get a good estimate of the true volume of every item.

After this step we finally have a number (in liters) for every item A1 … A60.

---

## Step 4: Solve the backpack problem (Knapsack)

Now we know:
- Price of every item
- Estimated volume of every item
- Maximum volume the backpack can hold = 40 liters

We want the combination of items that:
1. Has total volume ≤ 40 liters
2. Has the **highest total price**

This is a classic problem called the **0/1 Knapsack problem**.

Because volumes are decimal numbers, we temporarily multiply everything by 100 (so 0.01 liter becomes 1 unit). Then we use a standard computer algorithm (Dynamic Programming) that tries all sensible combinations efficiently.

**Why we do it:**  
There are 60 items. Checking every possible combination by hand is impossible (\(2^{60}\) is huge). The algorithm finds the best combination in a smart way.

---

## Step 5: Get the final answer

The algorithm tells us:

- The best total price is **757**
- The items that give this price are:  
  **A6, A8, A9, A23, A32, A35, A38, A44, A48**
- Their total estimated volume is about **39.97 liters** (safely under 40)

**Why this is the answer:**  
These 9 items give the highest possible value while still fitting in the backpack, according to the volumes we estimated from the real delivery data.

---

## Summary of the Logic

1. We don’t know item volumes → use past packages to **estimate** them.  
2. Once we have good volume estimates → solve the classic “most valuable items under a weight limit” problem.  
3. The result is the best gift Ahmad can put in his 40-liter backpack.

---

**Final Answer**  
**Maximum price: 757**  
**Selected items:** A6, A8, A9, A23, A32, A35, A38, A44, A48  
**Total volume ≈ 39.97 L**
