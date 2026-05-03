# Linux System Programming & Embedded Software Foundation 三個月計畫

## 0. 計畫定位

這是一個三個月的 side project，目標不是快速精通 embedded Linux / firmware，而是建立長期累積的能力。

我希望透過這個計畫培養：

- Linux user-space system programming 能力
- C programming 與 debugging 能力
- 對 Linux process、`/proc`、CPU / memory metrics 的理解
- 系統觀察、問題分析、debugging 的能力
- 能夠持續 output 技術理解與學習紀錄
- 未來往 embedded Linux / firmware / platform software 發展的基礎

最終成果是一個 GitHub project：

```text
linux-system-profiler

這個 project 會是一個用 C 寫的 Linux user-space system monitor / profiler。

1. 時程總覽

計畫期間：

2026/5/4–2026/8/9

其中：

2026/5/22–2026/6/2：出國 conference，完全不安排任務
階段	日期	主題	Checkpoint
Phase 0	5/4–5/10	啟動與環境建立	5/10
Phase 1	5/11–5/21	C / Makefile / Linux 開發基礎	5/21
Break	5/22–6/2	出國 conference，完全休息	無
Phase 2	6/3–6/14	重啟 + /proc / process 基礎	6/14
Phase 3	6/15–6/28	process monitor + file parsing	6/28
Phase 4	6/29–7/12	gdb / memory / signal handling	7/12
Phase 5	7/13–7/26	CPU / memory usage 計算	7/26
Phase 6	7/27–8/9	logging / pthread / portfolio 整理	8/9
2. 開發環境

不在 Mac 上跑 VM。

建議使用：

Mac:
- VS Code
- browser
- terminal
- 不跑 VM
- 不做高負載測試

GitHub Codespaces 或 Linux server:
- 寫 C
- 編譯
- gdb debug
- 跑 Linux user-space project

GitHub:
- 管理 repo
- 寫 README
- 每兩週 tag milestone
3. 安全原則

這個計畫原則上不會傷害 Mac，因為主要運算不在 Mac 本機上執行。

但在 Codespaces 或 shared Linux server 上要避免：

fork bomb
無限產生 process/thread
無限寫 log 檔
長時間 CPU 滿載
在 shared server 上跑壓力測試
嘗試繞過 sudo 權限
嘗試取得未授權 root 權限

絕對不要執行：

:(){ :|:& };:

這是 fork bomb，可能讓 Linux 環境失控，若在 shared server 上會影響其他使用者。

每次實驗前可以設定基本限制：

ulimit -u 100
ulimit -v 1048576
4. AI / Codex 使用規範

AI 可以使用，但不能當主要寫手。

原則
1. 每個新功能，我會先自己寫出 minimal version。
2. AI 可以用來解釋概念、給 debugging hint、review code、潤飾文件。
3. 不直接貼上 AI 產生的大段 code。
4. 如果使用 AI 產生的 code，必須能解釋每一行，並用自己的風格重寫。
5. 每個 checkpoint 都記錄 AI 幫了什麼，以及我如何確認自己真的理解。
建議比例
Phase 0–3:
自己寫 80%，AI 20%

Phase 4–6:
自己寫 60–70%，AI 30–40%
Phase 0：啟動與環境建立
日期
2026/5/4–2026/5/10
目標

完成基本開發環境與 project skeleton。

任務
 建立 GitHub repo：linux-system-profiler
 開好 GitHub Codespaces 或 Linux server 開發環境
 確認 gcc、make、gdb 可用
 建立基本 C 專案骨架
 寫 README 初稿
 完成第一個可執行程式
初始 repo 結構
linux-system-profiler/
├── README.md
├── Makefile
├── src/
│   └── main.c
├── include/
│   └── sysprof.h
├── docs/
│   └── checkpoint-00-setup.md
└── labs/
    └── README.md
第一個程式目標
make
./sysprof

輸出：

Linux System Profiler
Version: 0.1.0
Checkpoint 0：2026/5/10
應完成
 GitHub repo 已建立
 Makefile 可以編譯
 ./sysprof 可以執行
 README 初稿完成
 docs/checkpoint-00-setup.md 完成
Phase 1：C / Makefile / Linux 開發基礎
日期
2026/5/11–2026/5/21
目標

建立 C project 的基本肌肉。

任務
 熟悉 Makefile
 熟悉 C pointer / struct / malloc / file I/O
 熟悉 gcc warning
 初步使用 gdb
 建立 coding style
labs 練習
labs/
├── 01-pointer-basics.c
├── 02-struct-demo.c
├── 03-malloc-array.c
├── 04-file-read-demo.c
└── 05-gdb-segfault-demo.c
Makefile 目標

支援：

make
make run
make clean
make debug

建議 flags：

-Wall -Wextra -Werror -g
Checkpoint 1：2026/5/21
應完成
 Makefile 完成
 labs 至少 3 個小練習完成
 gdb 可以跑起來
 docs/checkpoint-01-c-foundation.md 完成
檢視問題
我能不能自己寫出一個基本 Makefile？
我知道 -Wall -Wextra -Werror -g 是什麼嗎？
我遇到 segmentation fault 時，能不能用 gdb 找到出錯行？
malloc/free 的風險是什麼？
我目前對 C 最不熟的是哪一塊？
Break：出國 conference
日期
2026/5/22–2026/6/2
原則

這段完全不安排任務。

這是計畫的一部分，不是中斷。

如果有零碎時間，最多只做：

 看 README
 記一兩句靈感
 想一下回來後要做什麼

但不列入要求。

Phase 2：重啟 + /proc / process 基礎
日期
2026/6/3–2026/6/14
目標

理解 Linux process 與 /proc。

要理解的觀念
process 是什麼
PID 是什麼
/proc 是什麼
Linux 為什麼把系統資訊暴露成 pseudo filesystem
如何用 C 讀取 /proc/[pid]/status
任務
 實作 ./sysprof --pid <PID>
 讀取 /proc/[pid]/status
 parse 基本欄位
 處理 PID 不存在的錯誤
目標輸出
PID: 1234
Name: bash
State: S
VmRSS: 12345 kB
Threads: 1
建議檔案
src/
├── main.c
├── process.c
└── proc_reader.c

include/
├── sysprof.h
├── process.h
└── proc_reader.h
Checkpoint 2：2026/6/14
應完成
 ./sysprof --pid <PID> 可以使用
 可以讀 /proc/[pid]/status
 能處理 PID 不存在的錯誤
 docs/checkpoint-02-proc-process.md 完成
檢視問題
PID 是什麼？
process state 的 R / S / Z 大概代表什麼？
/proc 為什麼不是普通資料夾？
如果 PID 不存在，我的程式會怎麼處理？
我目前的 parser 有哪些限制？
Phase 3：process monitor + file parsing
日期
2026/6/15–2026/6/28
目標

把 process monitor 做得更完整，並練習 parsing、錯誤處理、程式結構。

任務
 完善 ./sysprof --pid <PID>
 支援 ./sysprof --self
 設計 process_info_t
 加入基本錯誤處理
 更新 README usage
 整理 process parsing 文件
目標輸出
Process Information
-------------------
PID: 1234
Name: bash
State: S (sleeping)
PPid: 1000
VmSize: 123456 kB
VmRSS: 23456 kB
Threads: 1
建議資料結構
typedef struct {
    int pid;
    char name[256];
    char state[64];
    int ppid;
    long vm_size_kb;
    long vm_rss_kb;
    int threads;
} process_info_t;
技術重點
fopen
fgets
fclose
string parsing
struct design
error handling
function decomposition
Checkpoint 3：2026/6/28
應完成
 process monitor v1
 process_info_t struct
 基本錯誤處理
 README 更新 usage
 docs/checkpoint-03-process-monitor.md 完成
檢視問題
我怎麼設計 process_info_t？
parsing /proc/[pid]/status 時遇到什麼困難？
如果欄位不存在，我的程式會怎麼做？
我有沒有把 main.c 寫得太肥？
我可以如何重構？
Phase 4：gdb / memory / signal handling
日期
2026/6/29–2026/7/12
目標

建立系統軟體開發需要的 debugging 能力。

任務
 練習 gdb breakpoint
 練習 gdb backtrace
 練習 segmentation fault debugging
 理解 memory leak 基礎
 加入 signal handling
 支援 Ctrl+C graceful shutdown
 寫 docs/debugging-notes.md
功能目標
./sysprof --pid 1234 --interval 1

每秒更新一次。

按 Ctrl+C 時：

Caught SIGINT, exiting...

然後乾淨結束。

安全提醒

不要寫沒有 sleep 的無限迴圈：

while (1) {
    read_proc();
}

這會讓 CPU 滿載。雖然不會傷害 Mac，但會浪費 Codespaces / server 資源，在 shared server 上也可能影響其他人。

請加上：

sleep(interval);

或支援：

./sysprof --pid 1234 --interval 1 --duration 30
建議新增檔案
src/
├── signal_handler.c
└── cli.c

include/
├── signal_handler.h
└── cli.h
Checkpoint 4：2026/7/12
應完成
 支援 --interval
 支援 Ctrl+C graceful shutdown
 docs/debugging-notes.md 完成
 docs/checkpoint-04-debugging-signal.md 完成
檢視問題
segmentation fault 時我怎麼查？
Ctrl+C 其實送出什麼 signal？
為什麼 signal handler 不適合做太複雜的事情？
while loop 為什麼要加 sleep 或 duration？
我這階段最難 debug 的 bug 是什麼？
Phase 5：CPU / memory usage 計算
日期
2026/7/13–2026/7/26
目標

開始做真正的 system monitor。

任務
 讀 /proc/stat
 計算 CPU usage
 讀 /proc/meminfo
 計算 memory usage
 讀 /proc/loadavg
 完成 system monitor v1
 寫 docs/checkpoint-05-system-monitor.md
功能目標
./sysprof

輸出：

System Information
------------------
CPU Usage: 13.2%
Memory Usage: 61.7%
Load Average: 0.42 0.39 0.35

支援：

./sysprof --watch --interval 1
CPU usage 計算概念

從 /proc/stat 讀 cumulative CPU time counters。

total = user + nice + system + idle + iowait + irq + softirq + steal
idle_all = idle + iowait
cpu_usage = 1 - idle_delta / total_delta

重點：

CPU usage 是一段時間內的比例，所以要讀兩次 counter，計算差值。
Memory usage 計算概念

從 /proc/meminfo 讀：

MemTotal
MemAvailable

計算：

memory_usage = 1 - MemAvailable / MemTotal
建議新增檔案
src/
├── cpu.c
├── memory.c
└── loadavg.c

include/
├── cpu.h
├── memory.h
└── loadavg.h
Checkpoint 5：2026/7/26
應完成
 system monitor v1
 CPU usage 計算
 memory usage 計算
 load average 顯示
 docs/checkpoint-05-system-monitor.md 完成
檢視問題
為什麼 CPU usage 要讀兩次 /proc/stat？
idle 和 iowait 有什麼差別？
MemFree 和 MemAvailable 有什麼差別？
load average 是 CPU usage 嗎？
我可以怎麼驗證我的數字合理？
Phase 6：logging / pthread / portfolio 整理
日期
2026/7/27–2026/8/9
目標

把 project 整理成可展示成果。

任務
 加入 --duration
 加入 CSV logging
 可選：加入 pthread sampling
 整理 README
 寫 docs/design.md
 寫 docs/learning-reflection.md
 整理履歷 bullet
 tag v1.0
功能目標
./sysprof --watch --interval 1 --duration 60 --log output.csv

輸出 CSV：

timestamp,cpu_usage,memory_usage,load1,load5,load15
2026-07-27T20:00:00,13.2,61.7,0.42,0.39,0.35
pthread 可選設計
main thread:
- 處理 CLI
- 顯示結果

sampler thread:
- 定期讀 /proc
- 寫入 log

注意：

pthread 是加分，不是必要。
如果進度較慢，優先完成 logging 與文件整理。
安全提醒

logging 不可以無限制寫檔。

請一定要有：

--duration

或預設最多跑 60 秒。

最終 repo 結構
linux-system-profiler/
├── README.md
├── Makefile
├── include/
│   ├── sysprof.h
│   ├── process.h
│   ├── cpu.h
│   ├── memory.h
│   ├── loadavg.h
│   ├── logger.h
│   └── signal_handler.h
├── src/
│   ├── main.c
│   ├── process.c
│   ├── cpu.c
│   ├── memory.c
│   ├── loadavg.c
│   ├── logger.c
│   └── signal_handler.c
├── docs/
│   ├── design.md
│   ├── linux-procfs.md
│   ├── debugging-notes.md
│   ├── learning-reflection.md
│   ├── checkpoint-00-setup.md
│   ├── checkpoint-01-c-foundation.md
│   ├── checkpoint-02-proc-process.md
│   ├── checkpoint-03-process-monitor.md
│   ├── checkpoint-04-debugging-signal.md
│   └── checkpoint-05-system-monitor.md
├── examples/
│   └── sample_output.csv
└── labs/
README 最終內容

README 應包含：

Project overview
Motivation
Features
Build instructions
Usage examples
Architecture
What I learned
Limitations
Future work
最終履歷 bullet
Linux System Profiler in C
- Built a Linux user-space system profiler that reads CPU, memory, load average, and process metrics from the proc filesystem.
- Implemented periodic sampling, signal handling, CSV logging, and structured error handling in C.
- Practiced system-level debugging with gdb and documented Linux procfs, process states, and CPU utilization calculation.
- Designed the project as a foundation for embedded Linux and power/performance monitoring workflows.
Checkpoint 6：2026/8/9
應完成
 sysprof v1.0
 README 完整版
 docs/design.md
 docs/learning-reflection.md
 examples/sample_output.csv
 最終履歷描述
 Git tag v1.0
檢視問題
這三個月我最明顯進步的是什麼？
我現在能不能解釋 /proc 是什麼？
我能不能解釋 CPU usage 怎麼算？
我遇到最難的 bug 是什麼？我怎麼解？
這個 project 如果繼續做，下一步會是什麼？
每週建議投入時間

理想：

每週 4–6 小時

可以拆成：

平日 2 次，每次 60–90 分鐘
週末 1 次，每次 2–3 小時

最低限度：

每週 2–3 小時

但至少要做到：

每週一次 commit
每兩週一次 checkpoint
每次學習固定流程
1. 看 README / checkpoint，確認目前目標
2. 自己先寫 20–40 分鐘
3. 編譯，處理 warning/error
4. 用 gdb 或 print debug
5. 卡住再問 AI
6. 修完後 commit
7. 寫 3–5 行 note：今天學到什麼
每兩週 Checkpoint 模板
# Checkpoint XX: Title

## Period

YYYY/MM/DD–YYYY/MM/DD

## Goal

這一階段原本想完成什麼？

## Completed

- 

## Technical concepts I learned

1. 
2. 
3. 

## Bugs / problems I encountered

- 

## How I debugged / solved them

- 

## AI usage

AI helped me with:

- 

I verified my understanding by:

- 

## Remaining questions

- 

## Next milestone

- 
三個月後的成功標準

到 2026/8/9 時，只要達成以下成果，就算成功：

 有一個乾淨的 GitHub repo
 程式可以 make / run
 可以讀 process 資訊
 可以顯示 CPU / memory / load average
 可以 interval monitoring
 可以 Ctrl+C 安全退出
 可以輸出 CSV log
 README 寫得清楚
 docs 裡有 checkpoint 與學習筆記
 我能用自己的話講解這個 project
第二期可能方向

如果第一期順利完成，第二期可以進入：

Embedded Linux & Power/Thermal Awareness

可能內容：

/sys
cpufreq
thermal zone
CPU governor
runtime power management
Raspberry Pi / Jetson profiling
eBPF / perf 初步
kernel module introduction