# STM32LED_PWM practice
---
step1: SYS下的debug 調成 Serial Wire -> 能用st_link
step2: RCC下的HSE 調成 Crystal Resonator ->使用外部晶源，穩定性高、精度高
step3: Clock 設置成 72 MHz
step4: TIM3 設定 Internal Clock ， Channel 1 設定成 PWM 
    (這裡的內部時鐘指的是從APB管線來的，外部 HSE 是「水庫」，RCC是「水廠 + 輸水管系統」
    Timer 是「家裡的水龍頭」，雖然水的來源是外部的水庫，但你開水龍頭時，是用「家裡水管」來供水。 所以對你來說，水是從內部來的，雖然實際來源是水庫。)  
step5: Prescaler(分頻器)設定成72-1(這樣實際會是72)，這樣每秒便會計數72MHz/72=1MHz
step6: Counter Period設定成100-1(這樣實際會是100)，這代表PWM每次都會從1數到100，又每秒會數1MH次，所以實際上每秒會數一萬次1~100
step7: 生成程式碼後，在user code 2 內 輸入 HAL_TIM_PWM_Start(&htim3, TIM_CHANNEL_1);這樣timer3的channel1就會被開啟
step8: while迴圈中開始寫呼吸燈，使用 __HAL_TIM_SET_COMPARE(&htim3, TIM_CHANNEL_1, i)，用for迴圈，比i大就會關掉，讓i漸大，這樣PWM輸出的就會逐漸增加
step9: A6就是PWM的輸出腳
step10: 本程式同時還寫了讓STM32上本身就有的LED燈亮/不亮的簡單GPIO操作