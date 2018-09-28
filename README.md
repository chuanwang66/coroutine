It's an asymmetric coroutine library (like lua).

You can use coroutine_open to open a schedule first, and then create coroutine in that schedule. 

You should call coroutine_resume in the thread that you call coroutine_open, and you can't call it in a coroutine in the same schedule.

Coroutines in the same schedule share the stack , so you can create many coroutines without worry about memory.

But switching context will copy the stack the coroutine used.

Read source for detail.

Chinese blog : http://blog.codingnow.com/2012/07/c_coroutine.html

### 这个库用到uthread，只支持平台：
	- 跨平台“蝇量级”携程库Protothreads：https://coolshell.cn/articles/10975.html
### 协程：
	即“用户线程”，好处在于用户级别（非内核级别）地在“主线程”和“用户线程”之间切换，比系统线程效率高。
        如果“用户线程/协程”中要做I/O操作，这样做：
	- 协程中：先coroutine_yield()切回主线程context；
	- 协程中：执行I/O操作，此时不会阻塞主线程；
	- 协程中：执行完I/O操作后，coroutine_resume()返回继续执行协程；	（？待验证）
	- 协程中：协程最终执行完，自动切回主线程。
