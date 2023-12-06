# Lab 5_1: Керований "Вогник"
Author: Hashchuk Bohdan

### Налаштування lcd.ioc
Для початку нам потрібно було вибрати мікроконтроллер з яким ми будемо працювати. В моєму випадку це STM32F411E-DISCO . Після цього, ми маємо у вкладці Project Manager/Code Gerenator відмітити наступні місця:
![image](https://github.com/ucu-cs/stm32-led-hashchuk_bohdan/assets/145265923/8491e209-e3ab-4684-9676-7afa281b0af8)

Потім нажимаємо Ctr+S і генеруємо код.
Тепер приступаємо до налаштування пінів, так як ми сконфігурували цілий мікрокнроллер, то наша IDE сама сконфігурувала всі основні пристрої які там є, в тому числі наші діоди які знаходяться на PD12-PD15. 
Тепер нам потрібно сконфігурувати user кнопку яка буде налаштовувати переривання.
![image](https://github.com/ucu-cs/stm32-led-hashchuk_bohdan/assets/145265923/c6fe7dc1-1851-43fb-a22d-18d829a1fb75)
![image](https://github.com/ucu-cs/stm32-led-hashchuk_bohdan/assets/145265923/6551289d-fde2-433b-bb81-835a83c0d884)
Впринципі все, з налаштуванням lcd.ioc ми завершили.
Далі я реалізовував власне алгоритм посилаючись на WritePin для передачі сигналу на діод,
```
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin) {
	if(GPIO_Pin == UButton_Pin) {
		static uint32_t last_change_tick;
		if(HAL_GetTick() - last_change_tick < 50) {
			return;
		}
		last_change_tick = HAL_GetTick();
		if(button_is_pressed) {
			button_is_pressed = 0;
			++pressed;
		} else {
			button_is_pressed = 1;
		}
	}
}
```
В цій функції ми маємо GPIO_Pin , це індикатор що кнопку було нажато
Далі є перевірка для запобіганні поворному випадковому нажаттю кнопки, або ж простіше щоб заглушити шуми які можуть помилково викликати функцію.
Далі йде HAL_GetTick() це функція що повертає час виклику останнього зміни стану.

### Important
Для заливання проекту на мікроконтроллер нічого додатково робити не потрібно. Просто в main.c залити код.

### Results
В результаті роботи, на мікроконтроллері почали мигати діоди з затримкою у 200 мілісекунд . При нажатті кнопки спрацьовує interrupt, і діоди починають працювати в іншому напрямку.
# Additional tasks
