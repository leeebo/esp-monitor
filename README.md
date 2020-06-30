
## 任务监视

### 使用方法

1. 将 `components` 目录拷贝到用户工程目录
2. 将 `example/taskmonitor/sdkconfig.defaults` 文件拷贝到用户工程目录
3. 包含 `taskmonitor.h` 头文件
4. 初始化任务监视 `taskMonitorInit();`
5. 启动任务监视`taskMonitorStart();`，默认 `1000 ms` 输出一次任务信息

输出示例：

```
I (20327) TASK_MONITOR: Task dump

I (20337) TASK_MONITOR: Load    Stack left      Name    PRI

I (20337) TASK_MONITOR: 4.19    1240    Tmr Svc         1

I (20347) TASK_MONITOR: 95.80   1252    IDLE0   0

I (20347) TASK_MONITOR: 0.01    2480    main    1

I (20357) TASK_MONITOR: 0.00    3816    esp_timer       22

I (20357) TASK_MONITOR: Free heap: 245428
```

* Load：CPU 占用率
* Stack Left: 剩余堆栈大小
* Name：task 名
* PRI：task 优先级

### 其他可以监视的 task 参数

```
/**
 *  Used with the uxTaskGetSystemState() function to return the state of each task in the system.
*/
typedef struct xTASK_STATUS
{
	TaskHandle_t xHandle;			/*!< The handle of the task to which the rest of the information in the structure relates. */
	const char *pcTaskName;			/*!< A pointer to the task's name.  This value will be invalid if the task was deleted since the structure was populated! */ /*lint !e971 Unqualified char types are allowed for strings and single characters only. */
	UBaseType_t xTaskNumber;		/*!< A number unique to the task. */
	eTaskState eCurrentState;		/*!< The state in which the task existed when the structure was populated. */
	UBaseType_t uxCurrentPriority;	/*!< The priority at which the task was running (may be inherited) when the structure was populated. */
	UBaseType_t uxBasePriority;		/*!< The priority to which the task will return if the task's current priority has been inherited to avoid unbounded priority inversion when obtaining a mutex.  Only valid if configUSE_MUTEXES is defined as 1 in FreeRTOSConfig.h. */
	uint32_t ulRunTimeCounter;		/*!< The total run time allocated to the task so far, as defined by the run time stats clock.  See http://www.freertos.org/rtos-run-time-stats.html.  Only valid when configGENERATE_RUN_TIME_STATS is defined as 1 in FreeRTOSConfig.h. */
	StackType_t *pxStackBase;		/*!< Points to the lowest address of the task's stack area. */
	uint32_t usStackHighWaterMark;	/*!< The minimum amount of stack space that has remained for the task since the task was created.  The closer this value is to zero the closer the task has come to overflowing its stack. */
#if configTASKLIST_INCLUDE_COREID
	BaseType_t xCoreID;				/*!< Core this task is pinned to (0, 1, or -1 for tskNO_AFFINITY). This field is present if CONFIG_FREERTOS_VTASKLIST_INCLUDE_COREID is set. */
#endif
} TaskStatus_t;
```