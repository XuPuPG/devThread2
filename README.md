C++20, CMake 3.16+

Написать программу, где будет основной поток (Main Thread), поток brocker'a (Brocker Thread), поток worker'a (Worker Thread).
- Main Thread будет симулировать работу основного потока приложения, потому он будет состоять из цикла while (true), телом которого будет sleep на 5 секунд и вызов метода create_task(<lambda>), где <lambda> - лямбда выражение, которое будет выполнятся в worker потоке. На данный момент, в лябмде может быть, что угодно, я буду туда вставлять свои методы, для теста твоего решения.
- Brocker Thread будет принимать задачи (task) в виде объектов лямбда-функций и сохранять их у себя в очереди (std::queue). Каждый раз, как Worker Thread заканчивает или простаивает (т.е. не выполняет никаких задач), Brocker Thread передаёт Worker Thread'у task из очереди, если таковой есть.
- Worker Thread просто выполняет текущий, данный ему brocker'ом, таск.
- create_task возвращает объект класса Task, который является оболочкой для нашей лямбды.
- У объекта Task можно запросить результат работы лямбды и если работа ещё не выполнена, то результат ожидается тем потоком, в котором был запрощен результат.
- Стараться использовать shared_ptr, а не обычные указатели (*).
  
Пример работы
-------------------------
```cpp
  
...<работа программы>...
  
shared_ptr<Task> hard_task = create_task([](){
  ...<тяжелые вычисления>...
  return <результат вычислений>;
});
  
...<работа программы>...
  
std::cout << hard_task->get() << std::endl; //вызов результата вычислений где-то дальше в этом же потоке.
  
...<работа программы>...
```

Дополнительная информация
-------------------------
Нужно написать пару тестов, на своё усмотрение, используя gtest.
Проект должен билдиться на Ubuntu20.04, с этим и поможет CMake.

Задача похожа на работу Celery, но в более упрощенном виде.

Нужно коммитить решение, если есть какие-то изменения или решения что-то переделать, то нужно закоммитить это.
Решение пушится в виде pull request'a на репозиторий: https://github.com/frstrtr/devThread2
  
Схема работы Celery
-------------------------
![Celery схема](https://github.com/frstrtr/devThread2/blob/main/django-celery.png)

Суть задачи
-------------------------
Задача ориентированна на понимание многопоточной разработки на C++ и мультитред разработки в целом, обучение работы с CMake, освежение в памяти C++, работа с шаблонами, работа с указателями и умными указателями, работа с gtest и мультиплатформенная сборка.
