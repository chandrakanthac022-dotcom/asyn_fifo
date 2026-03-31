# Asynchronous FIFO Design (Verilog RTL)


## 📌 Project Description

This project implements a complete **Asynchronous FIFO (First-In First-Out)** using Verilog RTL.

The FIFO enables safe data transfer between **two different clock domains** using **Clock Domain Crossing (CDC)** techniques.


## 🚀 Features

* Dual clock FIFO (different read and write clocks)
* Gray code based pointer design
* 2-Flip-Flop Synchronizer for CDC
* Full and Empty flag generation
* Dual-port memory implementation
* Complete testbench with simulation


## 🧠 Key Concepts Used

* Asynchronous FIFO architecture
* Clock Domain Crossing (CDC)
* Gray Code Conversion
* Binary to Gray Pointer Logic
* Full and Empty Flag Logic
* Dual-Port RAM



## 📁 Project Structure

async_fifo_design/
│
├── rtl/
│   ├── write_pointer.v
│   ├── read_pointer.v
│   ├── sync.v
│   ├── fifo_memory.v
│   ├── async_fifo.v
│
├── tb/
│   └── async_fifo_tb.v
│
├── docs/
│   ├── block_diagram.png
│   ├── waveform.png
│
├── README.md
└── .gitignore



## ⚙️ Module Description

### 1️⃣ Write Pointer (wr_clk domain)

* Generates write address
* Binary → Gray conversion
* Generates FULL flag

verilog
wr_bin_next  = wr_bin + (wr_en & ~full);
wr_gray_next = (wr_bin_next >> 1) ^ wr_bin_next;
```



### 2️⃣ Read Pointer (rd_clk domain)

* Generates read address
* Binary → Gray conversion
* Generates EMPTY flag

```verilog
rd_bin_next  = rd_bin + (rd_en & ~empty);
rd_gray_next = (rd_bin_next >> 1) ^ rd_bin_next;
```



### 3️⃣ Synchronizer (CDC)

* 2-Flip-Flop Synchronizer
* Prevents metastability

```verilog
FF1 → FF2 → synchronized output
```


### 4️⃣ FIFO Memory

* Dual-port RAM
* Independent read/write clocks

```verilog
mem[wr_addr] <= data_in;
data_out <= mem[rd_addr];


### 5️⃣ FULL Flag Logic

FIFO is FULL when:

```verilog
wr_gray_next == {~rd_gray_sync[MSB:MSB-1], rd_gray_sync[MSB-2:0]}
```



### 6️⃣ EMPTY Flag Logic

FIFO is EMPTY when:

```verilog
rd_gray_next == wr_gray_sync
```


### 7️⃣ Top FIFO Module

Integrates:

* Write pointer
* Read pointer
* Synchronizers
* Memory
* Full / Empty logic



## 🔄 Data Flow

Write Side (wr_clk)
      ↓
Write Pointer → FIFO Memory → Read Pointer
      ↑                           ↓
     FULL                      EMPTY
      ↑                           ↓
Read Sync                 Write Sync
```


## ▶️ Simulation

Testbench verifies:

* Write operation
* Read operation
* FULL condition
* EMPTY condition
* Different clock domains

Simulation tools used:

* EDA Playground
* Icarus Verilog


## 📊 Waveform

(Add your waveform screenshot here)



## 🛠 Tools Used

* Verilog HDL
* EDA Playground
* Icarus Verilog
* Yosys (for synthesis)


## 🎯 Applications

* UART communication
* Network buffers
* CPU pipelines
* Video/data streaming systems
* Clock domain crossing systems


## 💡 Important Concepts

### Why Gray Code?

Gray code changes only **one bit at a time**, reducing errors during clock domain crossing.



### Why Synchronizer?

Prevents **metastability** when signals pass between different clock domains.


### FIFO Types

* Synchronous FIFO → same clock
* Asynchronous FIFO → different clocks

