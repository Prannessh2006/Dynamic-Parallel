NexusParallel: High-Performance Distributed Compute Engine

NexusParallel is a hardware-aware, multi-process distributed compute engine built in C++ and Qt6. It is specifically architected to harness the full power of the 12 logical processors on the AMD Ryzen 5 5600H, transforming a standard laptop into a high-throughput parallel processing cluster.

Developed during an intensive 20-day technical sprint, this project bridges the gap between high-level application development and low-level Windows Kernel internals.

🛠️ Key Technical Features
1. Hardware-Aware Process Orchestration
CPU Affinity Pinning: Utilizes the Windows API (SetProcessAffinityMask) to lock individual worker processes to specific logical cores.

Reduced Cache Contention: By preventing the OS from migrating threads, the system minimizes L1/L2 cache misses and eliminates context-switching overhead.

Suspended Bootstrapping: Workers are launched in a CREATE_SUSPENDED state to allow the Master process to configure hardware limits before execution begins.

2. Low-Level IPC & Memory Management
Shared Memory Backbone: Engineered a zero-latency data exchange layer using Windows File Mapping (CreateFileMappingA, MapViewOfFile).

Kernel Handle Safety: Implements strict RAII principles to ensure all Windows Kernel handles and memory maps are released upon system teardown, preventing "zombie" processes.

3. Lock-Free Synchronization & Work-Stealing
Atomic Telemetry: Uses std::atomic variables to track real-time progress across 12 independent processes without the performance "Mutex Wall".

Dynamic Load Balancing: Features a custom work-stealing algorithm that allows idle cores to "steal" tasks, ensuring 100% utilization across the Ryzen 5 5600H.

📊 Real-Time Telemetry Dashboard
The Master Application features a custom-built Qt6 dashboard providing deep visibility into the cluster's health:

Throughput Velocity: A live QtCharts engine plotting the real-time "Delta Ops" of the entire cluster.

Core-Level Heatmap: Progress bars for all 12 cores displaying real-time PID, Progress %, and Steal Counts.

🚀 Getting Started
Prerequisites
OS: Windows 10/11 (Uses Windows-specific Kernel APIs).

Hardware: Optimized for 12-core architectures (e.g., AMD Ryzen 5 5600H).

Framework: Qt 6.x (with QtCharts module).

Compiler: MSVC or MinGW (supporting C++17 or higher).

Installation
Clone the repository:

Bash
git clone https://github.com/Prannessh/NexusParallel.git
Open the project in Qt Creator.

Run CMake to link the Qt6::Charts and Qt6::Widgets libraries.

Build and Run.
