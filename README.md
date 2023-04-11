# Симуляция формирования структуры при кристаллизации материала

Кристаллизацией называется процесс образования кристаллов при
изменении агрегатного состояния металлов (сплавов) из жидкого в твердое
— это первичная кристаллизация, в течение которой формируется
кристаллическая решетка.

![Схема процесса кристаллизации из жидкого расплава](http://dl4.joxi.net/drive/2023/04/12/0054/1190/3572902/02/12cb26d03a.jpg)

На рисунке изображены последовательные этапы зарождения
первичных центров кристаллизации в условиях переохлаждения жидкого
сплава и дальнейшего роста зародышей кристаллов за счет процессов
диффузии атомов жидкой фазы, наслаивающихся на уже имеющуюся
твердую фазу. Рост граней кристаллов идет послойно до момента их
соприкосновения, в результате чего нарушается правильная форма
кристаллов. По окончании процесса кристаллизации образуется структура
5
сплава в виде зерен — кристаллов с неправильной геометрической формой,
называемых кристаллитами.

## Пример кода на ***JavaScript***:

***
    // Создаем массив для хранения кругов
    let circles = [];

    function setup() {
    createCanvas(400, 400);
    }

    function draw() {
    background(220);
  
      // Отображаем все круги из массива
    for(let i = 0; i < circles.length; i++) {
    // Увеличиваем диаметр круга на каждом кадре
    circles[i].diameter += 1;
    
    // Проверяем, не пересекается ли круг с другими кругами
    for(let j = 0; j < circles.length; j++) {
      if(i != j) {
        let distance = dist(circles[i].x, circles[i].y, circles[j].x, circles[j].y);
        let radiusSum = (circles[i].diameter + circles[j].diameter) / 2;
        if(distance - 2 < radiusSum) {
          // Если круги пересекаются, останавливаем их рост
          circles[i].diameter -= 1;
        }
      }
    }
    
    // Проверяем, не выходит ли круг за границы холста
    if(circles[i].x - circles[i].diameter/2 < 0 || circles[i].x + circles[i].diameter/2 > width ||
      circles[i].y - circles[i].diameter/2 < 0 || circles[i].y + circles[i].diameter/2 > height) {
      // Если круг выходит за границы, останавливаем его рост
      circles[i].diameter -= 1;
    }
    
    // Отображаем круг
    circles[i].display();
    }
    }

    function mousePressed() {
    // Создаем новый круг при щелчке мыши, если он не пересекается
    // с другими кругами и не выходит за границы холста
    let validCircle = true;
    let newCircle = new Circle(mouseX, mouseY);
    for(let i = 0; i < circles.length; i++) {
    let distance = dist(circles[i].x, circles[i].y, newCircle.x, newCircle.y);
    let radiusSum = (circles[i].diameter + newCircle.diameter) / 2;
    if(distance - 2 < radiusSum) {
      validCircle = false;
      break;
    }
    }
    if(newCircle.x - newCircle.diameter/2 < 0 || newCircle.x + newCircle.diameter/2 > width ||
    newCircle.y - newCircle.diameter/2 < 0 || newCircle.y + newCircle.diameter/2 > height) {
    validCircle = false;
    }
  
    if(validCircle) {
    circles.push(newCircle);
    }
    }

    class Circle {
    constructor(x, y) {
    this.x = x;
    this.y = y;
    this.diameter = 10;
    }
  
    display() {
    ellipseMode(CENTER);
    noFill();
    stroke(0);
    ellipse(this.x, this.y, this.diameter, this.diameter);
    }
    }
***

## **Результат:**

![Анимация](https://user-images.githubusercontent.com/114745140/231302399-d2736436-17f7-4cc7-8235-54a69e58e7d9.gif)
