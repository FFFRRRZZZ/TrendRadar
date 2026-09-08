# Codex 工作摘要

更新时间：2026-09-08

## 当前目标

为当前 Codex 和网页 ChatGPT 建立 GitHub 共享工作摘要，支持读取对方进展后分别工作。

## 已完成

- 阅读用户粘贴的完整架构说明，区分共享摘要与自动开发流水线。
- 检查 GitHub 连接，成功列出 FFFRRRZZZ/TrendRadar；返回写权限，该仓库公开。
- 准备共享目标、双方摘要、交接规则及网页启动指令。
- 用户已指定 FFFRRRZZZ/TrendRadar，首次共享文件随本次提交存放在 shared-work/。

## 共享测试结果

- 已从 master 分支的 CHATGPT.md 实际读到 BRIDGE-TEST-001。
- 已核对提交 1080ab4b370cfd9a42381496a8a19f1d1b3bbad8，其修改包含该标记。
- 网页端报告已读取 CODEX.md；用户提供网页端写入提交，Codex 已独立核对写入结果。
- 本轮“网页端写入 → GitHub → Codex 读回”交接通过。
- Codex 回执标记：BRIDGE-TEST-001-ACK。
- 这是按需共享交接，未配置自动后台同步或自动唤醒。

## 下一步

1. 网页端可读取本文件并核对 BRIDGE-TEST-001-ACK，验证回执方向。
2. 后续双方工作前读取共享文件，结束后更新各自摘要。
3. CHATGPT.md 中旧的“未验证”状态由网页端在下一次更新时修正，保留测试记录。

## 当前未开展的工作

未调用付费 Codex Action、配置定时任务或自动合并，也未对 TrendRadar 项目代码进行修改。

## 019f3b58-e6c6-7fd2-a5ff-b2017eec6d6e｜查明400ns中断原因

- 来源任务：Codex“查明400ns中断原因”（任务 ID：`019f3b58-e6c6-7fd2-a5ff-b2017eec6d6e`）。
- 快照时间：2026-09-08 15:16（UTC+08:00）；源任务当时为空闲。本节是时间点摘要，不代表之后未发生的运行结果。
- 用户目标与约束：查明 NAMD/WTM-eABF 在 400 ns 运行、续跑和收敛中的中断原因；保持同一前缀的坐标、速度、盒子和 Colvars 状态配套；ALC0315 膜相完全沿用 SM102 13 Å alloff 限制；新输出使用短名称且不覆盖旧包。

### 用户已确认的决定

- ALC0315 膜相两态均使用 `alchDecouple off`；脂质 `dz_membrane` 墙为 `-25～25 Å`、`k=10`；Cl–N 三维距离墙为下限 `13 Å`、`k=10`、`forceNoPBC no`；质子态的脂质与共炼金 Cl 一起标记。
- 两个 PMF 是续跑后的累计结果；SM102 的 400 ns 到 500 ns 端点由 `15.7972` 变为 `12.6707 kcal/mol`，目前不能称为已收敛。
- 配置、脚本和新输出目录应使用短前缀；旧检查点路径只读不写。

### 当前状态

- 已完成：MC3 一次中断定位为周期盒形状失控，约第 `10,535,000` 步从 `272,137` 突增至 `338,262 Å³`，随后报 `Periodic cell has become too small for original patch grid`。使用 `stepsPerCycle 1`、`pairlistsPerCycle 1`、`fullElectFrequency 1`、`margin 4` 的稳定化续跑实际完成 `20,000,000` 步（40 ns），无 FATAL、约束失败或 NaN；另一次重启因 VDW 约 `1.264×10^9 kcal/mol`、861 个速度超限及 Colvars 步数不配套而启动失败。
- 已完成：SM102/SM102H 续跑的长输出前缀在第 5000 步写 DCD 时触发 `buffer overflow detected`；已改用短输出名。多个带步数 PMF 来自 `keepFreeEnergyFiles on`；已提供关闭该选项并保留 ABF history 的修正版。曾发现状态文件在内联 Colvars 定义前读取，已改为所有 `cv config` 后执行 `cv load`，运行前仍要核对状态连续性。
- 已完成：ALC0315 膜相包。中性对象为 `A31/LU/50`（N=`23648`，脂质原子 `23643–23791`），质子态为 `A3H/LU/63`（N=`25597`，脂质原子 `25592–25741`），共炼金 Cl=`18306`；初始 Cl–N 距离约 `67.03/67.61 Å`。配置输出为短名 `n.conf/h.conf`、`out_n/out_h`，目标各 400 ns。
- 已完成：ALC0315 溶剂包按 `kc2_sol_namd` 参照生成。中性体系 9072 原子、LIG 149 且净电荷 0；质子化体系 9038 原子、LIG 150 且净电荷 +1，并由额外 Cl 保持体系中性；配置为 NPT 300 K、WTM-eABF、`2×10^8` 步×2 fs、`alchDecouple on`，从各自 `step5_10.restart` 启动。

### 验证、阻碍与建议

- 已通过：ALC0315 膜相的原子范围、电荷、FEP 标记、13 Å/脂质墙参数、初始距离和文件一致性；溶剂包的 PDB/PSF 原子顺序、FEP 选择、配置引用和打包校验；MC3 的 40 ns 稳定化运行及日志诊断。
- 未完成：ALC0315 膜相和溶剂包在目标服务器上的 NAMD/CUDA 启动测试；源任务本机 WSL 返回 `E_ACCESSDENIED`，不能把运行时验证写成通过。SM102 PMF 仍缺少严格收敛证据。
- AI 建议：继续至少 50 ns，并比较连续三个 10 ns 区段的端点变化，参考阈值为 `0.2–0.3 kcal/mol`；服务器长跑前先做 preflight/短时启动，确认同一前缀的 restart/Colvars 步数连续；MC3 恢复参数时逐步修改，并保持 `fullElectFrequency` 整除 `stepsPerCycle`。
- 定位信息：源任务工作目录不是 Git 仓库，没有关联分支、commit、PR 或 Issue；相关产物是本地配置、脚本和压缩包，未上传到 TrendRadar 项目代码。
