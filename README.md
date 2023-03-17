# **Разбор JVM**
##
Просмотрите код ниже и опишите  каждую строку с точки зрения происходящего в JVM
________________________

public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}

Вначале наши классы JvmComprehension, Object, Integer, System
будут подгружены Classloader он делится на 3 вида:
##
#### 
* Loading
  * Bootstrap Classloader - загружает основные модули Java SE и JDK
  * Platform ClassLoader - загружает выбранные (на основе безопасности / разрешений) модули Java SE и JDK.
  * Application ClassLoader - используется для загрузки классов приложения из classpath

Класс JvmComprehension будет найден на нижнем уровне Application ClassLoader,
все остальные будут найдены на самом верхнем уровне Bootstrap Classloader
Загружены классы будут в порядке обращения к ним, чтобы не забивать память

#### 
* Linking
  * проверка кода на валидацию.
  * подготовка примитивов (int i)
  * разрешение символьных ссылок (не произойдет, библиотеки стандартные).

#### 
* Initialization
  * выполняются инициализаторы static и инициализаторы полей static

public static void main(String[] args)

printAll(o, i, ii)

System.out.println("finished")
________________________

1. В момент вызова метода main() в стеке выделяется блок памяти (frame), отведенный для его нужд.
2. Объявляется переменная i типа int и помещается в стек.
3. ClassLoader загружает класс Object. Создается объект класса Object в куче (heap), а ссылка на него помещается в стек.
4. ClassLoader загружает класс Integer. Ссылочная переменная ii типа Integer будет создана в стеке, но само значение переменной будет храниться в куче.
5. В момент вызова метода printAll() в стеке выделяется блок памяти (frame), отведенный для его нужд.
мчмчс6. Ссылочная переменная uselessVar типа Integer будет создана в стеке, но само значение будет храниться в куче.
7. Объект типа String создается в куче (heap) и передается методу println(). После того, как метод println() отработал, работа метода printAll() завершается, и его фрейм удаляется из стека.
8. Объект типа String создается в куче (heap) и передается методу println(). После того, как метод println() отработал, работа метода main() завершается, и его фрейм удаляется из стека.
