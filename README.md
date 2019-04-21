# Tehnici și Mecanisme de Proiectare Software

Лабораторная №1

*Подготовила - Черней Ирина, группа TI-164*

*Целью данной лабораторной работы было имплементировать 5 порождающих шабловнов*

Были выбраны 5 шаблонов: 
1. Одиночка (Singleton)
Это порождающий паттерн проектирования, который гарантирует, что у класса есть только один экземпляр, и предоставляет к нему глобальную точку доступа.

Структура шаблона одиночка
![Структура шаблона одиночка](https://refactoring.guru/images/patterns/diagrams/singleton/structure-ru-2x.png )

Чтоб реализовать этот шаблон был создан класс [Map](https://github.com/cerneiirina/TMPS/blob/master/patternLab/Map/Map.cs)
```
public class Map
{
    private static Map MapObject;
    ***
    public static Size Size;
    private Map(Point p,int height, int width)
    {
        Size = new Size(width, height);
        PointStart = p;
        GameField = new Rectangle(PointStart, Size);
        Graphics.FillRectangle(new SolidBrush(Color.OldLace), GameField);
    }
    ***
    public static Map GetInstance(Point p, int height, int width, Graphics gs)
    {
        if (MapObject == null)
        {
            Graphics = gs;
            MapObject = new Map(p, height, width);
            return MapObject;
        }
        else
            return MapObject;
    }
    ***
}
```
Этот шаблон был использован так как нам была необходима только одна инициализация карты в приложении

2. Фабричный метод (Factory Method)

Это порождающий паттерн проектирования, который определяет общий интерфейс для создания объектов в суперклассе, позволяя подклассам изменять тип создаваемых объектов.

Паттерн Фабричный метод предлагает создавать объекты не напрямую, используя оператор new, а через вызов особого фабричного метода. Объекты всё равно будут создаваться при помощи  new, но делать это будет фабричный метод.

![Image of Factory Method](https://refactoring.guru/images/patterns/cards/factory-method-mini-2x.png)

Объявление [абстрактного класса](https://github.com/cerneiirina/TMPS/blob/master/patternLab/Factory/AbstractObject.cs) в 
```
public abstract class AbstractObject : IObjectBuilder
{
  ***
}
```
Чтобы эта система заработала, все возвращаемые объекты должны иметь общий интерфейс. 

Для объявления [класса, отвечающего за "круги"](https://github.com/cerneiirina/TMPS/blob/master/patternLab/Factory/Circle/CircleBuilder.cs) в программе:
```
public class CircleBuilder : AbstractObject, IObjectBuilder
{
  ***
}
```
Объявление [класса, который отвечает за "квадраты"](https://github.com/cerneiirina/TMPS/blob/master/patternLab/Factory/Rectangle/RectangleBuilder.cs)

```
public class RectangleBuilder: AbstractObject, IObjectBuilder
{
  ***
}
```

3. Абстрактная фабрика (Abstract Factory)

Это порождающий паттерн проектирования, который позволяет создавать семейства связанных объектов, не привязываясь к конкретным классам создаваемых объектов.

![Image of Abstract Factory](https://refactoring.guru/images/patterns/cards/abstract-factory-mini-2x.png)

Итрерфейс был объявлен в следующем [файле](https://github.com/cerneiirina/TMPS/blob/master/patternLab/Factory/IUnitFactory.cs)
```
public interface IUnitFactory
{
    string Name { get; set; }
    IObjectBuilder ObjectBuilder { get; }       

}
```

Классы наследники созданны в следующих файлов  [file1](https://github.com/cerneiirina/TMPS/blob/master/patternLab/Factory/CircleFactory.cs)  [file2](https://github.com/cerneiirina/TMPS/blob/master/patternLab/Factory/RectangleFactory.cs)

```
public class RectangleFactory : IUnitFactory
{
  ***
}
public class CircleFactory : IUnitFactory
{
  ***
}
```


4. Строитель (Builder)

Это порождающий паттерн проектирования, который позволяет создавать сложные объекты пошагово. Строитель даёт возможность использовать один и тот же код строительства для получения разных представлений объектов.

![Image of Builder](https://refactoring.guru/images/patterns/diagrams/builder/solution1-2x.png)

Строитель позволяет создавать сложные объекты пошагово. Промежуточный результат всегда остаётся защищён.

Объявление [Object builder interface](https://github.com/cerneiirina/TMPS/blob/master/patternLab/Factory/IObjectBuilder.cs)

```
public interface IObjectBuilder
{       
Graphics formGraphics { get; set; }
IObjectBuilder BuildObject();
UserColor ObjectColor { get; set; }
void ChangeColor();

}
```
Реализацию этого интерфейса вы можете проверить в Factory Method.
Классы CircleBuilder и RectengleBuilder реализует его.

5. Проторип (Prototype)

Это порождающий паттерн проектирования, который позволяет копировать объекты, не вдаваясь в подробности их реализации. Ниже приведен пример структуры шаблона прототип

![Image of Prototype](https://refactoring.guru/images/patterns/diagrams/prototype/structure-2x.png)

Прототип был использован как для [кругов](https://github.com/cerneiirina/TMPS/blob/master/patternLab/Factory/Circle/CircleBuilder.cs)
, так и для [квадратов](https://github.com/cerneiirina/TMPS/blob/master/patternLab/Factory/Rectangle/RectangleBuilder.cs)

Приведённые ниже методы возвращают новый экземпляр объектов.

```
public static CircleBuilder CopyCircle(CircleBuilder tempCircle)
{
    CircleBuilder temp = new CircleBuilder(tempCircle.formGraphics, tempCircle.ThisObject, tempCircle.ObjectColor,
        tempCircle.timer);

    return temp;
}

public static RectangleBuilder CopyRectangle(RectangleBuilder tempRectangle)
{
    RectangleBuilder temp = new RectangleBuilder(tempCircle.formGraphics, tempCircle.ThisObject, tempCircle.ObjectColor,
        tempCircle.timer);

    return temp;
}
```

Использовали мы этот паттерн для копирования кругов и квадратов 


Результат работы программы:
![Image of program](https://github.com/cerneiirina/TMPS/blob/master/%D0%91%D0%B5%D0%B7%D1%8B%D0%BC%D1%8F%D0%BD%D0%BD%D1%8B%D0%B9.jpg)

Вывод :
 В ходе данной лабораторной работы мы изучили и реализовали порождающие паттерны, они нам упрощают и структурируют код.
