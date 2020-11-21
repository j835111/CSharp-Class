# 協程(Coroutine)

## Coroutine 與 Thread
在系統層面上，coroutine 通常和 fiber 是一樣的意思。Fiber 是一種特別的 user thread，如同一般的 thread 那樣具有自己的 call stack 與 program counter，但具備了以下的特性：

- Thread 不需自己定義中斷點（也就是呼叫 yield 的地方），而是讓 OS 或 thread library 來決定是否進行 context switch，通常由執行時間來判斷。然而 fiber 需要自行定義中斷點，context switch 只會發生在明確呼叫 yield 的地方。
- 因為 context switch 是由使用者自行控制，因此 fiber 通常 不需要 mutex 之類的東西來避免 race condition。
- Kernel thread 會進入 OS 的排程中，在多核心的 CPU 上可能會使用不同的核心同時執行許多 kernel thread。但 fiber 屬於特別種類的 user thread，它無法利用多核心進行平行處理。