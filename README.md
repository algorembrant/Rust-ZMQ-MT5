## Rust + ZMQ + MT5's MQL5: Exploiting Micro-Second BID/ASK Live Trading Data

![Rust](https://img.shields.io/badge/Rust-000000?style=for-the-badge&logo=rust&logoColor=white)
![MetaTrader 5](https://img.shields.io/badge/MetaTrader_5-0075FF?style=for-the-badge&logo=metatraderfive&logoColor=white)
![ZeroMQ](https://img.shields.io/badge/ZeroMQ-DF0000?style=for-the-badge&logo=zeromq&logoColor=white)
![MQL5](https://img.shields.io/badge/MQL5-F5A623?style=for-the-badge&logo=metatraderfive&logoColor=white)

## 🏆 Trivia
> **The very First**: Exploiting micro-second BID/ASK live trading data from **MetaTrader 5 (MT5)** using the specific combination of **Rust + ZeroMQ (ZMQ) + MetaQuotes Language (MQL5)**..

I was trying to make a trading orderflows platform, and I came up with an idea: why not use MT5 as datafeed for it? Unfortunately, there are no existing ideal projects I could use as a benchmark, hence I created my own. <br><br>
I've  worked on and made ~100+ Python trading projects, and I always use MT5 as datafeed, but I want faster; that is why I came up with using Rust as a solution, and ZMQ binding for MT5 is a common practice. In worse case sscenario, there are no _Rust + ZMQ + MT5_ combination projects existed in the world, which is pretty nuts. I didn't know it only existed in my head, Lmao. So I created it with the help of Google Antigravity.

This simple orderflow tool will serve as a benchmark for future projects.

---

## 🌐 Software Demonstration

https://github.com/user-attachments/assets/71c51bb2-324b-4c97-ab0d-807a2c0d0d7c



## 🏆 A survey for AI leading platforms

Extensive research across GitHub, Hugging Face, and consultation with leading AI models confirmed that no prior public projects utilizing this exact high-performance stack existed before this implementation.

**AI Survey Results**
> "no public projects combination of Rust, ZeroMQ (ZMQ), and MetaTrader 5 (MT5)."

**Proof**


<table align="center">
  <tr>
    <td align="center">
      <video 
        src="https://github.com/user-attachments/assets/18f9865c-39b8-40cd-a969-733c435621db" 
        controls 
        muted
        loop
      ></video>
    </td>
    <td align="center">
      <video 
        src="https://github.com/user-attachments/assets/ccff1713-6c14-46cc-94b0-a1831f4a8b0d" 
        controls 
        muted
        loop
      ></video>
    </td>
  </tr>
</table>


**Additional Validation**:
<table cellspacing="0" cellpadding="0">
  <tr>
    <td><img src="survey/ans1.png" alt=""></td>
    <td><img src="survey/ans2.png" alt=""></td>
    <td><img src="survey/ans3.png" alt=""></td>
  </tr>
  <tr>
    <td><img src="survey/ans4.png" alt=""></td>
    <td><img src="survey/ans5.png" alt=""></td>
    <td><img src="survey/ans6.png" alt=""></td>
  </tr>
</table>


I used AI-leading platforms (Grok, Gemini, Claude, ChatGPT, MSCopilot, Perplexity) to validate my claim for _"**The very First**: Exploiting micro-second BID/ASK live trading data from **MetaTrader 5 (MT5)** using the specific combination of **Rust + ZeroMQ (ZMQ) + MetaQuotes Language (MQL5).**"_

---

## 📂 Project Structure
These are the only necessary files that you need to have.

```graphql
Rust-ZMQ-MT5
├── MQL5                        # Relocate this (individual files) into MT5's literal path directory, located outside this repository
│   ├── Experts
│   │   └── ZmqPublisher.mq5    # The "Server": Publishes Tick Data via TCP
│   ├── Include
│   │   └── Zmq
│   │       └── Zmq.mqh         # ZMQ Wrapper Library for MQL5 (64-bit compatible)
│   └── Libraries
│       ├── libzmq.dll          # 64-bit ZeroMQ DLL (Required)
│       └── libsodium.dll       # Dependency for libzmq (Required)
├── mt5-chart                   # The "Client": Rust Desktop App
│   ├── src
│   │   └── main.rs             # Main Application Logic (Subscribes & Plots)
│   └── Cargo.toml              # Project Dependencies
└── README.md                   

```

---

## 🚀 Functionality

The system consists of two main components acting in a Publisher-Subscriber model data flow:

### 1. The Publisher (MetaTrader 5 / MQL5)
*   **Role**: The Data Source.
*   **Implementation**: An Expert Advisor (`ZmqPublisher.mq5`) utilizing a custom ZMQ wrapper (`Zmq.mqh`).
*   **Protocol**: Binds to a TCP HOST on port `5555`.
*   **Operation**: On every incoming tick (`OnTick` data event), it:
    *   Extracts Symbol, Bid price, Ask price, and Server Time.
    *   Serializes this data into a JSON string.
    *   Publishes it instantly to the ZMQ socket.

### 2. The Subscriber (Rust)
*   **Role**: The Data Consumer & Visualization.
*   **Implementation**: A high-performance GUI application built with `eframe` (egui).
*   **Protocol**: Connects to the TCP socket on `localhost:5555`.
*   **Operation**:
    *   **Asynchronous Data Ingestion**: A background `tokio` task listens for incoming ZMQ messages.
    *   **Deserialization**: Parses the raw JSON bytes into Rust `TickData` structs.
    *   **Real-time Plotting**: Updates an `egui_plot` chart at 60fps (or higher) to visualize the Bid/Ask spread in real-time.

---

## 🔧 Installation & Usage

### Prerequisites
*   **MetaTrader 5** Client [*link*](https://www.metatrader5.com/) and it must already be connected to a Broker (use whatever broker you like)
*   **Rust** (via `rustup`) [*link*](https://rust-lang.org/tools/install/) and configure its file path [*Link*](https://www.youtube.com/watch?v=2PmPWWTmfiU) on your computer.
*   **Visual Studio Build Tools (C++)** (Required for Rust Linker) [*link*](https://visualstudio.microsoft.com/visual-cpp-build-tools/) select the C++ Build tools (in my case I've downloaded 6gb)

### Step 1: MT5 Setup
1.  Copy `MQL5/Experts/ZmqPublisher.mq5` to your MT5 Data Folder's `MQL5/Experts/`. *(relocate outside this repository, into your MT5 literal path directory)*<br>
2.  Copy `MQL5/Include/Zmq/Zmq.mqh` to `MQL5/Include/Zmq/`. *(do the same for this)*<br>
3.  **Crucial**: Download `libzmq.dll` and `libsodium.dll` (64-bit versions) and place them in `MQL5/Libraries`.<br>
4.  Open the MT5 platform, look at the top of the screen. Go to **Tools -> Options -> Expert Advisors** and check **"Allow DLL imports"**.<br>
5.  Open the MetaEditor5. Go to **Navigator (Ctrl+d) -> Experts -> and find ZmqPublisher.mq5**.  Select and compile 'ZmqPublisher.mq5' once.<br>
6.  Go back to the MT5 platform. Drag and drop `ZmqPublisher` onto any chart.<br>
7.  Check. Go to **Toolbox (ctrl+t) -> Journal** section, check if it says *Success Message*: `ZmqPublisher bound to tcp://0.0.0.0:5555`

### Step 2: Rust Client Setup
1.  Open a terminal (for instance: Windows PowerShell, not a code editor like VS Code) in the `mt5-chart` folder. Or simply copy the file path
    ```bash
    cd mt5-chart

    # In my case, I will bash
    cd C:\Users\User\Desktop\VSCode\Rust-ZMQ-MT5\mt5-chart
    ```
3.  Then run the application using another bash:
    ```bash
    cargo run --release
    ```

---

## ⚠️ Common Errors & Troubleshooting

The development of this bridge involved solving several critical environment-specific issues. Here is a definitive guide to fixing them:

Make sure to download Rust [*Link*](https://rust-lang.org/tools/install/) and configure its file path [*Link*](https://www.youtube.com/watch?v=2PmPWWTmfiU) on your computer.

### 1. MQL5: "Access Violation" / Crash on Load
*   **Symptom**: MT5 crashes or the EA removes itself immediately upon loading.
*   **Cause**: **64-bit Pointer Truncation**. Original MQL5 code often uses `int` (4 bytes) for handles. On 64-bit MT5, pointers are 8 bytes (`long`).
*   **Fix**: Ensure `Zmq.mqh` uses `long` for all socket and context handles. The provided code in this repo has this fix applied.

### 2. MQL5: "Error 126" (Cannot Load Library)
*   **Symptom**: `Failed to load 'libzmq.dll'`.
*   **Cause**: Missing dependencies. `libzmq.dll` depends on `libsodium.dll` and the **Visual C++ Redistributable**.
*   **Fix**: Ensure `libsodium.dll` [*link*](https://www.dll-files.com/libzmq.dll.html) is in the same `MQL5/Libraries` folder and that you have the VC++ [*link*](https://visualstudio.microsoft.com/visual-cpp-build-tools/) Redistributable installed.

### 3. MQL5: "Error 22" (Invalid Argument) during Bind
*   **Cause 1**: **Binding to Wildcard**. Defaulting to `tcp://*:5555` can fail on some Windows network stacks.
    *   **Fix**: Change bind address to `tcp://0.0.0.0:5555`.
*   **Cause 2**: **String Encoding**. MQL5 strings are Unicode (`wchar_t`), but ZeroMQ expects standard ANSI/UTF-8 C-strings.
    *   **Fix**: The `Zmq.mqh` wrapper must perform `StringToCharArray` conversion before passing the address to `zmq_bind`.

### 4. Rust: "Linker 'link.exe' not found."
*   **Symptom**: `cargo run` fails during compilation.
*   **Cause**: Missing **Microsoft Visual C++ Build Tools**. Rust does not include a linker by default on Windows.
*   **Fix**: Install "Desktop development with C++" workload via the Visual Studio Installer.

### 5. Rust: "OS Error 32" (File used by another process)
*   **Symptom**: `Error: failed to remove ... target/release/deps/...`
*   **Cause**: File locking by VSCode's Rust Analyzer, Terminal, or Antivirus.
*   **Fix**:
    1.  Close VSCode.<br>
    2.  Open an external PowerShell terminal.<br>
    3.  Run `cargo clean`.<br>
    4.  Run `cargo run --release`.<br>

---

## 📜 Citation

If you use this project in your research or application, please cite it as follows:

```bibtex
@software{RustZmqMt5,
  author = {Algorembrant},
  title = {Rust + ZMQ + MT5's MQL5: Exploiting Micro-Second BID/ASK Live Trading Data},
  year = {2026},
  url = {https://github.com/algorembrant/Rust-ZMQ-MT5},
  note = {First known implementation for exploiting micro-second live BID/ASK trading data using Rust + ZMQ + MT5's MQL5}
}
```
