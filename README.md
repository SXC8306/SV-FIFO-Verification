# SV-FIFO-Verification

## 📌 项目简介
基于 SystemVerilog 搭建的同步 FIFO 验证环境。项目采用约束随机测试（CRV）与功能覆盖率驱动的验证方法，结合 SVA (SystemVerilog Assertions) 进行边界时序监控，成功挖掘并定位了设计中的边界 Bug。

## 🛠️ 技术栈
*   **验证语言**：SystemVerilog (OOP, 约束随机, SVA)
*   **仿真工具**：VCS / QuestaSim
*   **波形调试**：Verdi

## 🏗️ 验证环境架构
1.  **Transaction (事务)**：定义读写激励的数据包。
2.  **Generator (生成器)**：产生带约束的随机读写指令。
3.  **Driver (驱动器)**：将事务转化为底层引脚信号，驱动 DUT (Design Under Test)。
4.  **Monitor (监视器)**：采样 DUT 的实际输入输出信号。
5.  **Scoreboard (记分板)**：比对预期数据与 DUT 实际输出数据，判断功能正确性。

## 🔥 核心亮点与 Bug 分析
*   **SVA 断言监控**：针对 FIFO 的边界情况编写了时序断言（如检查 `empty` 和 `full` 标志位是否在正确周期拉高/拉低）。
*   **致命 Bug 定位**：通过约束随机激励与 SVA 结合，成功抓到了设计代码中的 **“空满标志延迟一拍”** 边界 Bug。
    *   *现象*：当 FIFO 处于将满/将空临界状态，读写使能同时有效时，标志位状态与预期不符。
    *   *排查*：通过 Verdi 抓取波形，比对断言失败的时间点，定位到设计中的组合逻辑延迟导致标志位更新滞后。
*   **覆盖率收敛**：通过收集功能覆盖率（Covergroup），调整随机约束权重，最终使功能覆盖率达到 **100%**。

## 📂 目录结构
*   `/rtl`: 包含 FIFO 的 RTL 设计代码
*   `/tb`: 包含 SystemVerilog 验证平台代码
*   `/sim`: 包含运行脚本（Makefile）或仿真工程文件
