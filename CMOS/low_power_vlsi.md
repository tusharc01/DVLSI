<p align="center">
  <a href="https://youtu.be/TFOO1JAll2Y">
    <img src="https://img.youtube.com/vi/TFOO1JAll2Y/0.jpg" alt="Watch the video: Low Power VLSI Design" width="500"/>
  </a>
</p>

The video discusses the critical importance of **low power design** in modern VLSI (Very Large Scale Integration) chips, especially in today's scenario where it significantly impacts how chips are designed, including the physical design process.

### The Challenge of Power Consumption

The speaker emphasizes that **power consumption is perhaps the most important challenge in modern-day VLSI design**, even pushing performance to a secondary level. The primary reason for this is the prevalence of **portable devices** like mobile phones, laptops, notebooks, and tabs, which all run on battery power. Designing circuits that consume less power directly **increases the battery life** of these devices, which is extremely desirable from the user's perspective.

### The Four Dimensions of VLSI Design

Traditionally, VLSI design focused on certain key parameters, but power has emerged as a crucial "fourth dimension".

*   **Area and Delay (1970s-1980s):** In the past, the primary emphasis was on minimizing the **area** occupied by circuits on the silicon chip and maximizing **speed** (reducing delay). These two parameters often conflict, leading to figures of merit like the product of area and delay squared (AD^2).
*   **Testability (1980s-1990s):** As circuits became more complex, ensuring the chip did not contain fabrication defects became challenging. This led to the incorporation of **Design for Testability (DFT)** techniques, making **testability** the third axis in the design process.
*   **Power (2000s onwards):** With the advent of battery-operated mobile devices, **power** became a very important fourth parameter, requiring designers to address power consumption alongside area, delay, and testability.

### Understanding Power Terminologies

In a typical CMOS chip, power is dissipated when current flows from the power supply (V_DD) to the ground. The video distinguishes between three key power-related terminologies:

*   **Instantaneous Power (P(t)):** This is defined as the **product of the current (I_DD) flowing from V_DD to ground and the voltage (V_DD)** at a particular point in time. However, instantaneous power alone does not provide a clear picture of battery drain because it might be high momentarily but low over a longer period.
*   **Energy (E):** This is defined as the **integral of the instantaneous power over a period of time (T)**. Graphically, energy represents the **total area under the instantaneous power curve** from time zero to T.
*   **Average Power (P_avg):** This is calculated by **dividing the total energy (E) by the time period (T)**. When evaluating the power consumption of a circuit or chip, **average power or energy is normally used** because it provides a more meaningful measure.

### Types of Power Dissipation in CMOS Circuits

Power dissipation in CMOS circuits is broadly classified into three main types:

1.  **Dynamic Power:**
    *   **Definition:** **Dynamic power** is dissipated when signals in the circuits are **changing**. It is primarily caused by the **charging and discharging of load capacitances** as transistors switch states.
    *   **Mechanism:** When an output node changes from low to high, the load capacitance (C) gets **charged** through the pull-up transistor (to V_DD). When it changes from high to low, the capacitance **discharges** through the pull-down transistor (to ground). This charging and discharging process requires current to flow (I = C * dV/dt), leading to power consumption. If the output switches multiple times within a given period, the capacitor charges and discharges repeatedly.
    *   **Calculation:** The average dynamic power can be expressed as **C * V_DD^2 * F_sw**, where C is the load capacitance, V_DD is the supply voltage, and F_sw is the signal switching frequency.
    *   **Factors Affecting Dynamic Power:**
        *   **Load Capacitance (C):** Dynamic power is **proportional to C**. Reducing capacitance helps.
        *   **Supply Voltage (V_DD):** It is **proportional to the square of V_DD**. Reducing V_DD is highly effective for power reduction.
        *   **Signal Switching Frequency (F_sw):** It is **proportional to F_sw**. Reducing switching frequency also leads to less dynamic power.
    *   **Activity Factor (Alpha):** To relate the signal switching frequency (F_sw) to the clock frequency (F), an **activity factor (alpha)** is introduced: **F_sw = alpha * F**. Alpha represents the fraction of clock edges that actually result in a change in the signal state and thus dynamic power consumption.
        *   If a signal changes state at every clock edge (e.g., a clock connected directly to a gate input), alpha is 1.
        *   If the output switches once per clock cycle (for every two clock transitions), alpha is 0.5.
        *   For CMOS dynamic gates, the average alpha is about 0.5.
        *   For conventional CMOS static gates, alpha is typically around 0.1.
    *   **Final Dynamic Power Equation:** Incorporating the activity factor, the dynamic power equation becomes **alpha * C * V_DD^2 * F**.

2.  **Short Circuit Power:**
    *   **Definition:** This type of power dissipation occurs during signal transitions when, for a very small amount of time, **both the NMOS and PMOS networks of a CMOS gate are conducting simultaneously**. This is also known as **crowbar current**.
    *   **Impact:** In modern designs, short circuit power can contribute significantly, up to **20% of the total power dissipation**. Higher clock frequencies lead to more short circuit power.
    *   **Mechanism:** Input signals have a finite rise and fall time, meaning they don't change instantaneously. During this transition period, there's an intermediate voltage region where both the pull-up (PMOS) and pull-down (NMOS) transistors might be conducting, creating a direct path for current from V_DD to ground. This momentarily draws a "spike" of current.

3.  **Static Power:**
    *   **Definition:** **Static power** is dissipated **even when no signal transitions are occurring**.
    *   **Source:** It is primarily due to **leakage effects** in transistors, even when they are supposedly "off" or non-conducting. These leakage sources include drain junction leakage, sub-threshold current, and gate leakage.
    *   **Importance:** As transistors shrink and billions are packed into a chip, the total contribution of static power dissipation has become increasingly significant, even though each individual transistor consumes only a small amount.
    *   While static power cannot be entirely avoided, it can be reduced through specific design manipulations.

<p align="center">
  <img src="https://raw.githubusercontent.com/tusharc01/DVLSI/main/CMOS/static_power.png" alt="Static Power Illustration" width="400"/>
</p>

The video concludes by stating that subsequent lectures will delve into more details about controlling power dissipation using various design techniques.

---

<p align="center">
  <a href="https://youtu.be/ORtlxpW_LMU">
    <img src="https://img.youtube.com/vi/ORtlxpW_LMU/0.jpg" alt="Watch the video" width="500"/>
  </a>
</p>

The video continues the discussion on low power design, moving into **techniques to control or reduce power dissipation** by making design changes and modifications, and following specific design rules. These techniques can be applied at various levels, from the **device/transistor level** up to **higher design levels, such as the architecture or even behavioral level**, indicating that power considerations are not just for the final transistor-level circuit.

The primary focus of these techniques is on **reducing dynamic power and static power**.

### Techniques for Reducing Dynamic Power

Dynamic power dissipation for a typical CMOS gate is given by the expression: **`alpha * C * V_DD^2 * F`**. To reduce dynamic power, strategies can include the reduction of any of these four parameters.

1.  **Reduction of Activity Factor (alpha):**
    *   **Alpha** represents how frequently the output is changing state, meaning it is charging or discharging.
    *   **Clock Gating:** This is a technique where the **clock is sometimes stopped from coming into a circuit** when it's not required. The basic idea is that since the clock triggers activities in various parts of the circuit, if a part is not being used, switching off the clock entirely from that part will stop signal switching activity, thus reducing power consumption.
        *   While effective for power reduction, it can make **testing more difficult** because if the clock altogether stops reaching a sub-circuit due to an error, testing that sub-circuit becomes impossible. It's a trade-off.
        *   Implementing clock gating involves **additional hardware and control logic** to decide when a functional unit is not required and to generate the disable signal.
        *   This additional hardware can introduce **additional delay or skew in the clock signal**. To mitigate this, one approach is to replace an existing buffer in the clock network with the OR gate used for gating, so the delay is balanced.
    *   **Sleep Mode:** This is another technique that can be adopted to reduce the activity factor.

2.  **Reduction of Load Capacitance (C):**
    *   Dynamic power is proportional to `C`. Techniques to reduce load capacitance include using **small transistors, short wires, and smaller fan-outs**.

3.  **Reduction of Supply Voltage (V_DD):**
    *   Reducing the supply voltage is **very important** because dynamic power is proportional to the **square of `V_DD`**. However, reducing `V_DD` also **increases delay**, creating a trade-off.
    *   **Static Approach:** This involves analyzing the circuit at the design level to find parts that are **not critical in terms of delay**. For these blocks, the **power supply can be reduced statically**, meaning the distribution of supply voltages is fixed beforehand among various functional blocks.
        *   For example, in a circuit with multiple functional modules, some requiring fast operation and others tolerating slower speeds, **two different supply voltage rails (low and high)** can be used. Fast blocks are fed with high `V_DD`, and slower blocks with low `V_DD`.
        *   This approach requires **additional power networks and more complex power routing**. Also, when driving signals between blocks operating at different voltages, **voltage level translation may be required** at the interfaces to avoid leakage current.
        *   This can be implemented on a chip using **voltage islands**, where cells meant for high performance (e.g., orange rows) are powered by high `V_DD`, and lower performance cells (e.g., blue rows) by low `V_DD`. Even within the same row, different voltage islands can exist, though this complicates routing. Power grids can be designed vertically to align with these voltage islands.
    *   **Dynamic Approach (Dynamic Voltage and Frequency Scaling - DVFS):** This involves **adjusting the operating voltage and frequency dynamically** to match performance requirements.
        *   Modern processors often have **several power modes** (e.g., high performance mode with higher `V_DD` and frequency, or power saving mode with lower `V_DD` and frequency). This provides flexibility based on user needs or program demands.
        *   However, switching between modes can take **several milliseconds**, so transitions should not be carried out very frequently. Additional logic is also needed for implementation.
        *   The underlying idea is to **always run at the lowest supply voltage that meets the timing constraint**.
        *   **Dynamic Frequency Scaling (DFS)** alone saves power (by reducing average power over a longer period) but **does not change the total energy consumed** for a computation, as the number of transitions remains the same.
        *   **Dynamic Voltage Scaling (DVS) combined with DFS** is more effective because **voltage scaling directly reduces the magnitude of current drawn, thereby saving energy**.
        *   A typical DVFS system requires a **programmable clock generator** (often based on a Phase-Locked Loop, PLL) that can set clock frequency over a wide range with fine increments (e.g., 200-700 MHz in 33 MHz increments). It also needs a **programmable power supply network** to set `V_DD` across a range with many levels (e.g., 1.12-1.6 volts in 32 levels). Processor instructions and the operating system can be delegated the responsibility to set these voltage and clock levels based on task completion deadlines or system load. If the system load is heavy, the OS can increase `V_DD` first, then frequency, to run faster. If the load is light, it can reduce clock frequency, then `V_DD`.

4.  **Reduction of Frequency (F):**
    *   Frequency can be reduced to the extent possible without sacrificing performance beyond acceptable levels, potentially using a **power down mode** in the circuit.

### Techniques for Reducing Static Power

Static power is consumed even when no signal transitions are occurring, primarily due to **leakage effects** in transistors.

1.  **Ratioed Circuits:** In conventional CMOS circuits with pull-up and pull-down networks (unlike dynamic CMOS), having several gates in series can reduce the chance of simultaneous conduction, thereby reducing switching currents (though the primary focus of static power is leakage).

2.  **Leakage Reduction Techniques:**
    *   **Transistor Stacking:** This technique involves connecting **multiple transistors in series**. When transistors are stacked, the overall resistance of the series path increases, which significantly **reduces the total leakage current** flowing through the "off" transistors.
    *   **Dual Threshold Partitioning / Selectively Using Low Threshold Devices:** The threshold voltage (`Vth`) of transistors can be changed during fabrication.
        *   **Low threshold voltage transistors run faster but incur more leakage current**.
        *   **High threshold voltage transistors are slower but have less leakage current**.
        *   The approach involves classifying transistors in the circuit netlist into low threshold (for critical path elements requiring high speed) and high threshold (for non-critical path elements) to **balance speed and overall power consumption**.
    *   **Variable Thresholds / Body Bias:** This is a more generalized approach where the threshold voltage can be adjusted to many levels, not just two. This is achieved by **changing the substrate bias voltage** of a transistor. Reducing `Vth` makes transistors faster but increases leakage rapidly. This flexibility allows adjusting transistor speed and leakage during runtime via programming, but it requires a **triple-well fabrication process**, which increases cost.
    *   **Reduce Temperature Operation:** This is another general method mentioned for reducing leakage.

3.  **Power Gating Using Sleep Transistors:**
    *   The idea is to introduce **auxiliary power supply voltages (e.g., `V_DDV`, `V_SSV` at lower levels)** in addition to the main `V_DD` and `V_SS`.
    *   **"Sleep transistors"** (activated by a sleep signal `SL`) are placed between the main power rails and the circuit block.
    *   When a circuit is not needed, it can be put into a **"sleep mode" or "power down mode"** by activating these transistors, which either **completely turns off or significantly reduces the voltage** to the circuit.
    *   For storage elements like flip-flops, instead of completely switching off `V_DD` (which would lose data), the voltage is just reduced to retain data, and it can be "jacked up" again when needed.
    *   This technique can be implemented at the chip level by embedding power switches in standard cells.
    *   When used properly, this method has been demonstrated to **reduce power by as much as a thousand times**, making it a very effective design philosophy for power control, though it requires detailed circuit analysis.

The video concludes by stating that further techniques for power reduction will be discussed in subsequent lectures.

---

<p align="center">
  <a href="https://youtu.be/_A7fUR2Itsc">
    <img src="https://img.youtube.com/vi/_A7fUR2Itsc/0.jpg" alt="Watch the video" width="500"/>
  </a>
</p>

This video focuses on **techniques to reduce power consumption at the gate level**. The speaker emphasizes that these methods can be "quite easily integrated as part of the synthesis tools". A very important design principle is to **"incorporate power aware design philosophy from the very beginning,"** not waiting until the end of the design process.

When recalling power consumption in a CMOS network, the speaker refines the average power dissipation expression: **`alpha * C * V_DD^2 * F`**.
*   **Load Capacitance (C)**: This includes not only the load capacitance from driving other gates but also "the parasitic capacitance of the interconnects" and "the drain capacitance of the local transistors".
*   **Internal Capacitance (C_internal)**: It's important to consider that "all the other internal nodes of this gate" also have internal capacitance that charges and discharges, not just the output node. This makes the power calculation "more accurate".
*   **Activity Factor (alpha)**: This represents the "signal activity" or "transition" at a node.

At the gate level, the primary focus is on **dynamic power consumption**, which occurs "whenever the gate output is switching state zero to one one to zero". To estimate signal activity, an objective function is introduced: **`sum(C_i * p_i * (1-p_i))`**, which you "want to minimize" to reduce dynamic power.
*   **`p_i`** denotes the "probability that the logic value of a node is at logic one".
*   **`1 - p_i`** is the probability that the node is at logic zero.
*   The product **`p_i * (1 - p_i)`** represents the "probability that there is a transition" at that node, also known as the activity factor `alpha_i`.
*   For "totally random data," where `p_i` is 0.5, `alpha_i` will be 0.25, but in "real circuits data will not be totally random," so `alpha_i` will typically be much less.

The video then delves into several techniques:

1.  **Technology Mapping Approach**:
    *   **Technology mapping** is the process where, given a design specification, a netlist of cells is generated using "standard cells" from a predefined library.
    *   The objective here is to **"minimize the total switching activity"** during this mapping process.
    *   The speaker illustrates with a 4-input AND gate where the inputs (a, b, c, d) have different signal probabilities (e.g., a=0.2, b=0.2, c=0.5, d=0.5).
    *   By calculating the `p_i` and `alpha_i` values for internal nodes in different technology mappings (e.g., a 3-level AND vs. a 2-level AND decomposition), it's shown that one mapping can result in a "much larger" total signal activity (e.g., 0.23 vs. 0.0679), making it "a very bad technology mapping" from a power dissipation standpoint.
    *   This introduces an "additional parameter during technology mapping" to consider power alongside delay, creating a "tradeoff that you will have to decide".

2.  **Phase Assignment**:
    *   This technique involves **moving gates across functional blocks to reduce the activity**.
    *   The speaker provides an example of a circuit computing `a_bar * b`, where `a` is a high-activity node. By modifying the AND gate into a NOR gate and changing `b` to `b_bar`, the output function remains the same, but the number of "high activity" nodes can be reduced from two to one, thereby reducing power consumption.
    *   Another example involves a multiplexer where an inverter on a high-activity input `a` is moved to input `b` and also an inverter is added to the output. Functionally, the multiplexer remains the same, but the number of "high activity nodes" is reduced.
    *   This technique uses **"Boolean algebra rules to change the functional wave"** and "restructuring gates" to reduce `alpha_i` values.

3.  **Pin Swapping**:
    *   This is an "interesting thing" where the **"impact of the switching activity of a gate input with respect to power dissipation depends on the relative position of the input"** in a multi-input gate.
    *   Although gates like NAND are functionally commutative (e.g., `abcd` vs. `dcba`), "in CMOS this does not always happen" regarding power dissipation.
    *   The design philosophy is to "put the **higher activity nodes to the inputs whose impact on power dissipation is the least**".
    *   Using a 4-input NAND gate example, where `d` has maximum activity and `a` has lowest activity, the observation is that placing the "least active input... at the top" of the transistor stack (e.g., `a b c d` where `a` is at the top) is better. This is because even if lower transistors switch heavily, the "charging discharging impact will be the least" if the topmost transistor (controlled by the least active input) is "mostly non conducting".

The video concludes by stating that "more techniques at the level of gates" for power control will be discussed in the next lecture.

---

<p align="center">
  <a href="https://youtu.be/tS536xrS3vE">
    <img src="https://img.youtube.com/vi/tS536xrS3vE/0.jpg" alt="Watch the video" width="500"/>
  </a>
</p>

In this lecture, the speaker continues the discussion on **techniques for low power at the level of gates or functional blocks (RTL level)**.

The first method discussed concerns **glitch**.
*   **Glitches, also known as hazards**, are defined as "unwanted signal transitions because of the delays in the gates" or "spurious transitions that means transitions which are functionally not expected to happen in the output due to unbalanced path delays".
*   If a circuit has glitches, it means the "output node is changing unnecessarily a few times," which leads to "charging and discharging of the capacitor is taking place unnecessarily" and thus "extra power consumption".
*   The speaker notes that **dynamic CMOS logic does not produce any glitches** because of its "pre charge and compute state" operation, meaning "this kind of glitches will not happen there" and "they will have zero glitching power".
*   An example of a two-level NAND circuit demonstrates a **static hazard**, where an output that is "supposed to remain at one" temporarily goes down and then up due to "delay in balance," leading to "one discharging and one one charging".
*   The "rule of thumb" is that a "chain kind of a structure and many gates in cascade" is "not a good structure with respect to glitches" and is "bound to be several such glitches here". In contrast, if you implement the same circuit as a **"balanced tree structure this will be having fewer glitches"**.

Next, the concept of **pre-computation** is introduced.
*   The motivation is that the "total time you take to generate the output can be unequal" depending on the functional block.
*   The core idea is that "if some of the common cases can be detected early by using some special circuits then the output of that circuit can disable the main functional block from participating in the calculation". This helps to disable "unnecessary transitions".
*   This involves adding "pre computation logic to disable or un select some of the paths which can help in reducing power dissipation".
*   A typical use case involves looking for "specific input patterns," and "if some patterns are found we can disable loading of these registers so that new values will not be loaded thereby transitions in the combinational logic will be avoided". This is "like a clock gating" that avoids "dynamic power consumption".
*   The speaker illustrates this with a **comparator example**:
    *   For two N-bit numbers, `A` and `B`, the **most significant bits (MSBs)** are compared first using an exclusive NOR (XNOR) gate.
    *   "If the two most significant bits are different then there is no need to look at the other bits". In this case, the one-bit comparator's result is sent directly to the output "without involving this n minus one bit comparator".
    *   Crucially, "when this bits are not equal we are not loading any new value into these two resistors... thereby we are stopping any kind of transitions from happening in this part of the combinational circuit so we can reduce the power consumption quite significantly".

The video then revisits **clock gating**.
*   This technique "selectively stop the clock from reaching some of the flip flops or registers so that unnecessary transitions are avoided".
*   While it can pose issues for testability and race conditions, "from low power design point of view this is quite effective".
*   The justification is that "in a typical chip the clock network is the one major source of power dissipation because every node of the clock network makes two transitions in every clock period in every cycle," meaning "every node has the highest values of alpha activity". Therefore, "targeting the clock network is very justified".
*   An example of a shift register where a flip-flop's output feeds the next flip-flop's clock input is shown to be problematic for "testability considerations designs for testing". The solution involves adding "additional multiplexers" and feeding the clock to all inputs, controlled by an "extra additional control pin," enabling or disabling the clock as needed.

A **low power flip-flop** structure is then detailed.
*   Normally, a flip-flop activates on every clock edge, leading to "some power consumption inside".
*   The low-power version makes an observation: "if my applied d value and my current output value are same... then even if i store it the output value will not change".
*   Therefore, "if they are same i am disabling the clock because i need not apply the clock because the next state will also be the same".
*   This is implemented by feeding the D input and Q output to an **XOR gate**. When `D` and `Q` are the same (00 or 11), the XOR output is 0, which "disables the clock" via an AND gate.
*   "So here the clock will be coming to the flip flop only for those instances where the applied d value and the output q stored value are different". The "overhead is two additional gates one x o r and one and gate are required".

The **input gating technique** is discussed next.
*   This technique is "quite simple and commonly used" and involves disabling "some of the inputs that means we prevent them from changing".
*   An example is given with multiple functional blocks (adder, subtractor, multiplier) whose outputs are fed to a multiplexer.
*   The problem is that even if only one block (e.g., adder) is selected, the "f two and f three are also participating in the calculation" because "a and b are changing," leading to "unnecessarily signal changes going on inside" them and consuming power.
*   The solution: "if i know from select that i am going to select f one so why dont disable the inputs of f two and f three in some way so that signal transitions here and here does not occur".
*   This can be done using "buffers or tri state buffers" at the inputs. When a block is not needed, its inputs are disabled, meaning "no signal transitions occurring here which means no transitions will be occurring inside this". This prevents "useless transition" and "additional power" consumption.

Finally, a **reduced power shift register** is explained.
*   A normal shift register is a "chain of d flip flops connected in cascade".
*   The low-power principle involves "splitting the shift register into two parts".
*   This means separating the "odd number cells in one shift register and the even number cells in the other shift register".
*   Inputs are applied alternately to the two shift registers (e.g., D1 to one, D2 to the other).
*   Crucially, these two shift registers are clocked "at half the clock frequency" (f/2) compared to the original frequency (f).
*   A final multiplexer combines the outputs alternately from the two shift registers to "generate the output at the frequency of f".
*   This technique exploits the principle that **"power dissipation is directly proportional to the frequency"**. It allows keeping the "speed of the shift register at this same level but to reduce the power consumption quite significantly".

The speaker concludes by noting that "these kind of adhoc techniques you can imply in various blocks of your design" to reduce power consumption.

---



