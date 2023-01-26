Этот репозиторий содержит неоффициальную пользовательскую документацию для некоторых роботов компании Wheeltec.

# [WHEELTEC R350A PLUS](https://supereyes.ru/catalog/roboty_nvidia_jetson_nano/robot_avtomobil_wheeltec_r350a_plus/)

![[Pasted image 20230126093658.png]]

## Документация к оригинальному ПО

1. управление клавиатурой
// сначала нужно включить узел инициализации (только если вы хотите включить управление клавиатурой отдельно)
roslaunch turn_on_wheeltec_robot turn_on_wheeltec_robot.launch
//открыть узел управления клавиатурой
roslaunch wheeltec_robot_rc keyboard_teleop.launch

2. линейное патрулирование (радиолокационное обход препятствий)
roslaunch simple_follower line_follower.launch 

3. следовать за радаром.
roslaunch simple_follower laser_follower.launch 

4. Визуальное слежение.
roslaunch simple_follower visual_follower.launch 

5. 2D сборка, 2D навигация.
roslaunch turn_on_wheeltec_robot mapping.launch 
roslaunch turn_on_wheeltec_robot navigation.launch 
Сохранение карты одним щелчком мыши (WHEELTEC.pgm, WHEELTEC.yaml)
roslaunch turn_on_wheeltec_robot map_saver.launch 

6.3D построение карт, 3D навигация.
roslaunch turn_on_wheeltec_robot 3d_mapping.launch 
roslaunch turn_on_wheeltec_robot 3d_navigation.launch 

7. чисто визуальная навигация
roslaunch turn_on_wheeltec_robot pure3d_mapping.launch
roslaunch turn_on_wheeltec_robot pure3d_navigation.launch

8. голосовое управление
// включить нижний, навигационный, радарный узлы сканирования
roslaunch xf_mic_asr_offline base.launch
//включить узел инициализации массива микрофонов
roslaunch xf_mic_asr_offline mic_init.launch

9. kcf следовать
roslaunch kcf_track kcf_tracker.launch

10.Автономное построение карт Подробнее см. в учебнике по функции автономного построения карт
// Запустите файл rrt_slam.launch.
roslaunch turn_on_wheeltec_robot rrt_slam.launch
//Откройте rviz и нажмите на "добавить" → "по теме" слева внизу, чтобы добавить плагин конфигурации.
clicked_point показывает диапазон и начальную точку случайного числа
detected_points Обнаруженные граничные точки
Границы Граничные точки, полученные фильтром, те же данные, что и выше
центроиды Действительные граничные точки после фильтрации
global_detector_shapes глобальное дерево
локальный_детектор_фигуры локальное дерево
// Используйте инструмент публикации точек rviz, чтобы задать 4 граничные точки для дерева роста по часовой или против часовой стрелки, а также начальную точку для дерева роста (как можно ближе к начальной точке робота), после чего робот будет исследовать карту на основе дерева роста.

11. WHEELTEC APP передача карт, построение карт и навигация
//Приложение WHEELTEC APP можно использовать, подключив мобильный телефон к wifi соединению автомобиля и открыв приложение. APP может управлять движением тележки, сохранять карты и просматривать экран камеры, подробнее см. руководство по функциям WHEELTEC APP
// Для передачи карты необходимо вручную открыть узел RGB-камеры: roslaunch usb_cam usb_cam-test.launch
Сторона APP может просматривать запись с камеры в режиме реального времени
//Карта, вам нужно открыть узел карты вручную: roslaunch turn_on_wheeltec_robot mapping.launch 
На стороне APP можно просматривать эффект отображения и сохранять его, а также управлять движением каретки.
// Навигация, необходимо открыть узел навигации вручную: roslaunch turn_on_wheeltec_robot navigation.launch 
Сторона APP может управлять движением каретки

12. формирование мультиробота
//Подготовка перед использованием многокамерного формирования.
// Ведущий и ведомый подключены к одной сети и правильно настроили многокамерную связь  
//Мастер-кары заранее создают 2D карту и сохраняют ее
//Ведущий автомобиль размещается в начальной точке карты, а ведомые автомобили размещаются вблизи позиции инициализации (позиция формирования ведомых по умолчанию)  
//Удаленный вход в разные тележки и синхронизация времени. 
sudo date -s "2022-04-01 15:15:00" 
// Первый шаг: откройте 2D навигацию в основной части корзины 
roslaunch turn_on_wheeltec_robot navigation.launch 
//Шаг 2: Запустите программу формирования отдельно на ведомой стороне 
roslaunch wheeltec_multi wheeltec_slave.launch
//Шаг 3: Откройте узел управления клавиатурой на стороне мастера или используйте джойстик и т.д. для управления движением мастера 
roslaunch wheeltec_multi keyboard_teleop.launch 
//Шаг 4 (не обязательно): запустите rviz для наблюдения за движением тележки 
rviz

13. WEB-браузер для отображения камеры
Хост: roslaunch usb_cam usb_cam-test.launch
          rosrun web_video_server web_video_server
Веб-просмотр хоста: http://localhost:8080/ (тот, кто отправляет точку доступа, является хостом)
Просмотр веб-страницы клиента: http://192.168.0.100:8080 (клиент - это тот, кто подключается к точке доступа)
Примечание】Мы рекомендуем использовать Google Chrome, после тестирования браузера 360 Extreme, браузер IE не может открыть изображение

14.TensorFlow и ROS комбинированное использование (Jetson серии необходимо ввести виртуальную среду для запуска, см. руководство по функциям, Raspberry Pi 2GB не может работать)
//TensorFlow распознавание объектов
roslaunch ros_tensorflow ros_tensorflow_classify.launch
//Обнаружение объектов TensorFlow
roslaunch ros_tensorflow ros_tensorflow_detect.launch
Запустите rqt_image_view и выберите тему result_ripe для просмотра
//TensorFlow распознавание рукописных цифр
roslaunch ros_tensorflow ros_tensorflow_mnist.launch

15. распознавание меток AR
roslaunch turn_on_wheeltec_robot ar_label.launch
Создайте QR-код со стороной 5 и содержимым 0
rosrun ar_track_alvar createMarker -s 5 0
Следование этикетке AR
roslaunch simple_follower ar_follower.launch
   
16.2.4G беспроводная ручка для управления со стороны ROS
// сначала нужно включить узел инициализации
roslaunch turn_on_wheeltec_robot turn_on_wheeltec_robot.launch
//открыть узел управления беспроводной ручкой
roslaunch wheeltec_joy joy_control.launch

17. Глубокое обучение (только для мастера серии Jetson)
//включить узел глубокого обучения
roslaunch darknet_ros darknet_ros.launch
//включить узел действия распознавания жестов
roslaunch wheeltec_yolo_action gesture.launch
//открыть узел движения песка
roslaunch wheeltec_yolo_action dp_drive.launch

18. Симуляция навигации при сборке Gazebo (выполняется на стороне виртуальной машины)
//2D строительство беседки
roslaunch wheeltec_gazebo_function mapping.launch
// управление клавиатурой
roslaunch wheeltec_gazebo_function keyboard_teleop.launch
//Сохранить карту
Сохранение карты одним щелчком мыши (WHEELTEC.pgm, WHEELTEC.yaml)
roslaunch wheeltec_gazebo_function map_saver.launch
//2D навигация по беседке
roslaunch wheeltec_gazebo_function navigation.launch

19. Функции ROS Qt (работающие на стороне виртуальной машины)
//запустите файл sh, чтобы настроить вход в ssh без пароля и включить roscore на стороне cart
bash initssh.sh
//После завершения работы файла sh запустите файл запуска, чтобы запустить функцию qt
roslaunch qt_ros_test qt_ros_test.launch

20. Функция преобразования текста в звук TTS
roslaunch tts tts_make.launch
Генерируйте звук по пути /tts_make/audio

21. Слежение за скелетом человека и управление позой (только для мастера серии Jetson)
// контроль осанки
roslaunch bodyreader bodyinteraction.launch
// скелет тела
roslaunch bodyreader bodyfollow.launch
//Сочетайте управление стойкой и следованием - запуск по умолчанию - режим управления стойкой, скрещенные руки перед грудью - режим переключения.
roslaunch bodyreader final.launch

------------------------------------------
Другие распространенные команды

Рекурсивно изменяет время модификации файлов в текущей (терминальной) папке.
найти /* -exec touch {} \;

Для запуска в рабочем пространстве и установки всех зависимостей пакета ROS (rosdep настроен в образе)
rosdep install --from-paths src --ignore-src -r -y

Чтобы изменить системное время.
sudo date -s "2022-06-15 09:00:00"

Укажите пакет функций для компиляции.
catkin_make -DCATKIN_WHITELIST_PACKAGES="имя пакета функций"
Чтобы раскомпилировать указанный пакет.
catkin_make -DCATKIN_WHITELIST_PACKAGES=""

Для установки pip используйте источник douban (интернет будет намного быстрее).
pip install -i https://pypi.doubanio.com/simple/ имя пакета python

вход по ssh.
ssh -Y wheeltec@192.168.0.100

nfs mount :
sudo mount -t nfs 192.168.0.100:/home/wheeltec/wheeltec_robot /mnt
nfs unmount:
sudo umount -t nfs 192.168.0.100:/home/wheeltec/wheeltec_robot /mnt

Чтобы предоставить исполняемые разрешения всем файлам в папке.
sudo chmod -R 777 Папки

Чтобы открыть путь к карте.
cd /home/wheeltec/wheeltec_robot/src/turn_on_wheeltec_robot/map
Чтобы сохранить карту вручную.
rosrun map_server map_saver -f 20220615

Чтобы включить камеру и просмотреть тему изображения с помощью инструмента rqt.
roslaunch turn_on_wheeltec_robot wheeltec_camera.launch
rqt_image_view

Просмотр узлов по отношению к темам
rqt_graph

Создайте дерево TF в формате pdf
rosrun tf view_frames
Вид деревьев TF
rosrun rqt_tf_tree rqt_tf_tree

vnc для настройки разрешения
xrandr --fb 1024x768

Переведено с помощью www.DeepL.com/Translator (бесплатная версия)