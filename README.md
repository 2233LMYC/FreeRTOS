# RTOS

#### 介绍
手动移植FreeRTOS，测试完成

#### 软件
Clion CubeMX 

FreeRTOS源码：https://github.com/FreeRTOS/FreeRTOS-Kernel

此工程内包含F1MDK版本与Clion版本，F4MDK版本，自行选择复制到自己的项目里即可

#### 方法

1.  包含头文件：
         set(USER_INC FreeRTOS_M3_gcc/inc FreeRTOS_M3_gcc/portable/ARM_CM3)
         set(USER_SRC "FreeRTOS_M3_gcc/portable/ARM_CM3/*.*" "FreeRTOS_M3_gcc/portable/MemMang/*.*" "FreeRTOS_M3_gcc/src/*.*")

2.  引用头文件：
        #include "FreeRTOS.h"
        #include "task.h"
3.  
          stm32f1xx_it.c中添加以下代码：
          #include "FreeRTOS.h"
          #include "task.h"
          extern void xPortSysTickHandler(void);
    
          删除 SVC_Handler() 和 PendSV_Handler()两个函数
    
          SysTick_Handler中添加：
          if(xTaskGetSchedulerState()!=taskSCHEDULER_NOT_STARTED)
          {
              xPortSysTickHandler();
          }

4.  任务：
        //任务1
        void vTask1(void *pvParameters)
        {
          for(;;)
          {
            HAL_GPIO_TogglePin(GPIOC,GPIO_PIN_13);
            vTaskDelay(100);
        
          }
        }
        //任务2
        void vTask2(void *pvParameters)
        {
          for(;;)
          {
            HAL_GPIO_TogglePin(GPIOA,GPIO_PIN_6);
            vTaskDelay(1000);
          }
        }
        
5.  创建任务：
            xTaskCreate(vTask1,"LED",128,NULL,2,NULL);
            xTaskCreate(vTask2,"JDQ1",128,NULL,2,NULL);
  
6.  开启调度器：
              vTaskStartScheduler();
        