### **Day 47: Randomization in SystemVerilog**

---

### **Objective:**

Learn how to use SystemVerilog’s **randomization features** to create variable and constrained stimulus for testbenches and functional verification.

---

## ✅ 1. What is Randomization?

Randomization allows automatic generation of values for class variables or data fields. This helps create a wide range of test cases with minimal manual effort.

There are **two types**:

* **Unconstrained randomization** – no restrictions
* **Constrained randomization** – specific rules or conditions applied

---

## ✅ 2. Random Variables

In SystemVerilog, you declare random variables in a class using the `rand` or `randc` keyword:

```systemverilog
class Packet;
    rand bit [7:0] addr;
    rand bit [7:0] data;
endclass
```

* `rand` – generates new values each time
* `randc` – generates values in random cyclic order (non-repeating until all used)

---

## ✅ 3. Randomize Method

Use `.randomize()` to generate values:

```systemverilog
Packet p = new();
p.randomize(); // fills p.addr and p.data with random values
```

---

## ✅ 4. Constraints

Use `constraint` blocks inside the class to restrict value generation:

```systemverilog
class Packet;
    rand bit [7:0] addr;
    rand bit [7:0] data;

    constraint valid_addr { addr > 8 && addr < 100; }
    constraint even_data  { data[0] == 0; } // data must be even
endclass
```

Constraints are **automatically applied** during randomization.

---

## ✅ 5. Inline Constraints (Optional Constraints)

You can also apply **inline constraints** during randomize:

```systemverilog
p.randomize() with { addr == 20; data inside {[50:60]}; };
```

This temporarily overrides or adds new constraints.

---

## ✅ 6. Pre and Post Randomize Hooks

You can define custom behavior before or after randomization:

```systemverilog
function void pre_randomize();
    $display("About to randomize");
endfunction

function void post_randomize();
    $display("Randomization done: addr = %0d, data = %0d", addr, data);
endfunction
```

These functions are automatically called if defined.

---

## ✅ 7. Randomization Failure Handling

`.randomize()` returns a boolean. If constraints can't be met, it returns 0:

```systemverilog
if (!p.randomize())
    $display("Randomization failed!");
```

---

## ✅ Summary

| Feature            | Description                             |
| ------------------ | --------------------------------------- |
| `rand`/`randc`     | Declare random variables                |
| `.randomize()`     | Generate values                         |
| `constraint`       | Limit the range or pattern of values    |
| Inline constraints | Temporary or additional conditions      |
| Hooks              | Custom logic before/after randomization |

