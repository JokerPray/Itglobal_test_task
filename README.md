# Itglobal Тестовые задания 
## First task
### Описание
Разработан скрипт на Python для обработки видеофайла с целью обнаружения и выделения движущихся объектов. С помощью библиотеки OpenCV производится:

#### 1)Вычитание фона
#### 2)Обработка маски
#### 3)Поиск и отрисовка контуров
#### 4)Обведение движущихся объектов прямоугольниками в видео

```
  import cv2
  import numpy as np
  
     #Путь к видеофайлу
  cap = cv2.VideoCapture('C:/Users/Romanum33/Testing/1111.mp4')
  backSub = cv2.createBackgroundSubtractorMOG2()
  
  if not cap.isOpened():
      print("Error opening video file")
  else:
      while True:        
          ret, frame = cap.read()
  
          # Если кадр успешно прочитан
          if ret:            
              fg_mask = backSub.apply(frame)
  
              # Пороговая бинаризация
              retval, mask_thresh = cv2.threshold(fg_mask, 180, 255, cv2.THRESH_BINARY)
  
              # Морфологическая обработка
              kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (3, 3))
              mask_eroded = cv2.morphologyEx(mask_thresh, cv2.MORPH_OPEN, kernel)
  
              # Поиск контуров
              contours, hierarchy = cv2.findContours(mask_eroded, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
  
              # Фильтрация по площади
              min_contour_area = 500
              large_contours = [cnt for cnt in contours if cv2.contourArea(cnt) > min_contour_area]
  
              # Копия кадра для отрисовки контуров
              frame_out = frame.copy()
  
              # Отрисовка прямоугольников вокруг больших контуров
              for cnt in large_contours:
                  x, y, w, h = cv2.boundingRect(cnt)
                  cv2.rectangle(frame_out, (x, y), (x + w, y + h), (0, 0, 200), 3)
  
              # Показ итогового кадра
              cv2.imshow('Frame_final', frame_out)
  
              # Выход из цикла, если нажата клавиша 'q'
              if cv2.waitKey(30) & 0xFF == ord('q'):
                  break
          else:
              break  # Выход из цикла, если кадры закончились     
  
  cap.release()
  cv2.destroyAllWindows()
 ```
## Second task
### Описание
Функция max_tens вычисляет максимальное количество десяток, которые можно составить из заданного количества чисел 2, 3 и 4.
```
def max_tens(count_of_twos, count_of_threes, count_of_fours):
    total_tens = 0
    c2 = count_of_twos
    c3 = count_of_threes
    c4 = count_of_fours

    # 1. Комбинация: 2 + 4 + 4
    num1 = min(c2, c4 // 2)
    total_tens += num1
    c2 -= num1
    c4 -= num1 * 2

    # 2. Комбинация: 3 + 3 + 4
    num2 = min(c3 // 2, c4)
    total_tens += num2
    c3 -= num2 * 2
    c4 -= num2

    # 3. Комбинация: 2 + 2 + 3 + 3
    num3 = min(c2 // 2, c3 // 2)
    total_tens += num3
    c2 -= num3 * 2
    c3 -= num3 * 2

    # 4. Комбинация: 2 + 2 + 2 + 4
    num4 = min(c2 // 3, c4)
    total_tens += num4
    c2 -= num4 * 3
    c4 -= num4

    # 5. Комбинация: 2 + 2 + 2 + 2 + 2
    num5 = c2 // 5
    total_tens += num5
    c2 -= num5 * 5

    return total_tens

```
## Third task
### Описание 
Функция knapsack решает классическую задачу о рюкзаке с использованием динамического программирования. Цель — максимизировать суммарную ценность предметов, помещенных в рюкзак, без превышения его максимального веса. Каждый предмет может быть выбран не более одного раза.
```
def knapsack(weights, values, maxWeight):
    n = len(values)
    dp = [[0] * (maxWeight + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        for w in range(maxWeight + 1):
            if weights[i - 1] <= w:                
                dp[i][w] = max(dp[i - 1][w],
                               dp[i - 1][w - weights[i - 1]] + values[i - 1])
            else:                
                dp[i][w] = dp[i - 1][w]

    return dp[n][maxWeight]

```
