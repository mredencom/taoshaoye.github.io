---
layout: post
title: PHP设计模式
category: php
tags: [php]
---

* content
{:toc}

设计模式是反复出现问题的解决方案; **如何解决某些问题的指导方针**。它们不是可以插入应用程序并等待神奇发生的类，包或库。相反，这些是如何在某些情况下解决某些问题的指导原则。

![](https://cloud.githubusercontent.com/assets/11269635/23065273/1b7e5938-f515-11e6-8dd3-d0d58de6bb9a.png) 

* * *

设计模式超简单的解释！（本项目从 [design-patterns-for-humans](https://github.com/kamranahmedse/design-patterns-for-humans) fork）

* * *

## 介绍

> 设计模式是反复出现问题的解决方案; 如何解决某些问题的指导方针

维基百科将它们描述为

> 在软件工程中，软件设计模式是软件设计中给定上下文中常见问题的通用可重用解决方案。它不是可以直接转换为源代码或机器代码的完成设计。它是如何解决可在许多不同情况下使用的问题的描述或模板。

### ⚠️注意

* 设计模式不是解决所有问题的灵丹妙药。
* 不要试图强迫他们; 如果这样做的话，应该发生坏事。
* 请记住，设计模式是问题的解决方案，而不是解决问题的解决方案；所以不要过分思考。
* 如果以正确的方式在正确的地方使用，他们可以证明是救世主; 否则他们可能会导致代码混乱。

> 另请注意，下面的代码示例是PHP-7，但是这不应该阻止你因为概念是相同的。

### 设计模式的类型

* [创建型](#creational-design-patterns)
* [结构型](#structural-design-patterns)
* [行为型](#behavioral-design-patterns)

## 创建型设计模式

简单来说

> 创建模式专注于如何实例化对象或相关对象组。

维基百科说

> 在软件工程中，创建设计模式是处理对象创建机制的设计模式，试图以适合于该情况的方式创建对象。对象创建的基本形式可能导致设计问题或增加设计的复杂性。创建设计模式通过某种方式控制此对象创建来解决此问题。

* [简单工厂模式（Simple Factory）](#-simple-factory)
* [工厂方法模式（Factory Method）](#-factory-method)
* [抽象工厂模式（Abstract Factory）](#-abstract-factory)
* [构建器模式](#-builder)
* [原型模式（Prototype）](#-prototype)
* [单例模式（Singleton）](#-singleton)

### 🏠简单工厂模式（Simple Factory）

现实世界的例子

> 考虑一下，你正在建房子，你需要门。你可以穿上你的木匠衣服，带上一些木头，胶水，钉子和建造门所需的所有工具，然后开始在你的房子里建造它，或者你可以简单地打电话给工厂并把内置的门送到你这里不需要了解关于制门的任何信息或处理制作它所带来的混乱。

简单来说

> 简单工厂只是为客户端生成一个实例，而不会向客户端公开任何实例化逻辑

维基百科说

> 在面向对象编程（OOP）中，工厂是用于创建其他对象的对象 - 正式工厂是一种函数或方法，它从一些方法调用返回变化的原型或类的对象，这被假定为“新”。

**程序化示例**

首先，我们有一个门界面和实现

```php
interface Door
{
    public function getWidth(): float;
    public function getHeight(): float;
}

class WoodenDoor implements Door
{
    protected $width;
    protected $height;

    public function __construct(float $width, float $height)
    {
        $this->width = $width;
        $this->height = $height;
    }

    public function getWidth(): float
    {
        return $this->width;
    }

    public function getHeight(): float
    {
        return $this->height;
    }
}
```

然后，我们有我们的门工厂，门，并返回它

```php
class DoorFactory
{
    public static function makeDoor($width, $height): Door
    {
        return new WoodenDoor($width, $height);
    }
}
```

然后它可以用作

```php
// Make me a door of 100x200
$door = DoorFactory::makeDoor(100, 200);

echo 'Width: ' . $door->getWidth();
echo 'Height: ' . $door->getHeight();

// Make me a door of 50x100
$door2 = DoorFactory::makeDoor(50, 100);
```

**什么时候用？**

当创建一个对象不仅仅是一些分配而且涉及一些逻辑时，将它放在专用工厂中而不是在任何地方重复相同的代码是有意义的。

### 🏭工厂方法模式（Factory Method）

现实世界的例子

> 考虑招聘经理的情况。一个人不可能对每个职位进行面试。根据职位空缺，她必须决定并将面试步骤委托给不同的人。

简单来说

> 它提供了一种将实例化逻辑委托给子类的方法。

维基百科说

> 在基于类的编程中，工厂方法模式是一种创建模式，它使用工厂方法来处理创建对象的问题，而无需指定将要创建的对象的确切类。这是通过调用工厂方法来创建对象来完成的 - 在接口中指定并由子类实现，或者在基类中实现并可选地由派生类覆盖 - 而不是通过调用构造函数。

**程序化示例**

以上面的招聘经理为例。首先，我们有一个访谈者界面和一些实现

```php
interface Interviewer
{
    public function askQuestions();
}

class Developer implements Interviewer
{
    public function askQuestions()
    {
        echo 'Asking about design patterns!';
    }
}

class CommunityExecutive implements Interviewer
{
    public function askQuestions()
    {
        echo 'Asking about community building';
    }
}
```

现在让我们创造我们的 `HiringManager`

```php
abstract class HiringManager
{

    // Factory method
    abstract protected function makeInterviewer(): Interviewer;

    public function takeInterview()
    {
        $interviewer = $this->makeInterviewer();
        $interviewer->askQuestions();
    }
}

```

现在任何孩子都可以延长并提供所需的面试官

```php
class DevelopmentManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new Developer();
    }
}

class MarketingManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new CommunityExecutive();
    }
}
```

然后它可以用作

```php
$devManager = new DevelopmentManager();
$devManager->takeInterview(); // Output: Asking about design patterns

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // Output: Asking about community building.
```

**什么时候用？**

在类中有一些通用处理但在运行时动态决定所需的子类时很有用。换句话说，当客户端不知道它可能需要什么样的子类时。

### 🔨抽象工厂模式（Abstract Factory）

现实世界的例子

> 从Simple Factory扩展我们的门例子。根据您的需求，您可以从木门店，铁门的铁门或相关商店的PVC门获得木门。另外，你可能需要一个有不同种类特色的家伙来安装门，例如木门木匠，铁门焊机等。你可以看到门之间存在依赖关系，木门需要木匠，铁门需要焊工等

简单来说

> 工厂工厂; 将个人但相关/依赖工厂分组在一起而不指定其具体类别的工厂。

维基百科说

> 抽象工厂模式提供了一种封装一组具有共同主题但没有指定其具体类的单个工厂的方法

**程序化示例**

翻译上面的门例子。首先，我们有我们的`Door`界面和一些实现

```php
interface Door
{
    public function getDescription();
}

class WoodenDoor implements Door
{
    public function getDescription()
    {
        echo 'I am a wooden door';
    }
}

class IronDoor implements Door
{
    public function getDescription()
    {
        echo 'I am an iron door';
    }
}
```

然后我们为每种门类型都配备了一些装配专家

```php
interface DoorFittingExpert
{
    public function getDescription();
}

class Welder implements DoorFittingExpert
{
    public function getDescription()
    {
        echo 'I can only fit iron doors';
    }
}

class Carpenter implements DoorFittingExpert
{
    public function getDescription()
    {
        echo 'I can only fit wooden doors';
    }
}
```

现在我们有抽象工厂，让我们制作相关对象的家庭，即木门工厂将创建一个木门和木门配件专家和铁门工厂将创建一个铁门和铁门配件专家

```php
interface DoorFactory
{
    public function makeDoor(): Door;
    public function makeFittingExpert(): DoorFittingExpert;
}

// Wooden factory to return carpenter and wooden door
class WoodenDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new WoodenDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Carpenter();
    }
}

// Iron door factory to get iron door and the relevant fitting expert
class IronDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new IronDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Welder();
    }
}
```

然后它可以用作

```php
$woodenFactory = new WoodenDoorFactory();

$door = $woodenFactory->makeDoor();
$expert = $woodenFactory->makeFittingExpert();

$door->getDescription();  // Output: I am a wooden door
$expert->getDescription(); // Output: I can only fit wooden doors

// Same for Iron Factory
$ironFactory = new IronDoorFactory();

$door = $ironFactory->makeDoor();
$expert = $ironFactory->makeFittingExpert();

$door->getDescription();  // Output: I am an iron door
$expert->getDescription(); // Output: I can only fit iron doors
```

正如你所看到的木门工厂的封装`carpenter`和`wooden door`还铁门厂已封装的`iron door`和`welder`。因此，它帮助我们确保对于每个创建的门，我们没有得到错误的拟合专家。

**什么时候用？**

当存在相互关联的依赖关系时，涉及非简单的创建逻辑

### 👷构建器模式

现实世界的例子

> 想象一下，你在Hardee's，你订购了一个特定的交易，让我们说，“Big Hardee”，他们毫无_疑问地_把它交给你了; 这是简单工厂的例子。但有些情况下，创建逻辑可能涉及更多步骤。例如，你想要一个定制的地铁交易，你有多种选择如何制作你的汉堡，例如你想要什么面包？你想要什么类型的酱汁？你想要什么奶酪？在这种情况下，建筑商模式得以拯救。

简单来说

> 允许您创建不同风格的对象，同时避免构造函数污染。当有几种风格的物体时很有用。或者在创建对象时涉及很多步骤。

维基百科说

> 构建器模式是对象创建软件设计模式，其目的是找到伸缩构造器反模式的解决方案。

话虽如此，让我补充说一下伸缩构造函数反模式是什么。在某一点或另一点，我们都看到了如下构造函数：

```php
public function __construct($size, $cheese = true, $pepperoni = true, $tomato = false, $lettuce = true)
{
}
```

如你看到的; 构造函数参数的数量很快就会失控，并且可能难以理解参数的排列。此外，如果您希望将来添加更多选项，此参数列表可能会继续增长。这被称为伸缩构造器反模式。

**程序化示例**

理智的替代方案是使用构建器模式。首先，我们要制作汉堡

```php
class Burger
{
    protected $size;

    protected $cheese = false;
    protected $pepperoni = false;
    protected $lettuce = false;
    protected $tomato = false;

    public function __construct(BurgerBuilder $builder)
    {
        $this->size = $builder->size;
        $this->cheese = $builder->cheese;
        $this->pepperoni = $builder->pepperoni;
        $this->lettuce = $builder->lettuce;
        $this->tomato = $builder->tomato;
    }
}
```

然后我们有了建设者

```php
class BurgerBuilder
{
    public $size;

    public $cheese = false;
    public $pepperoni = false;
    public $lettuce = false;
    public $tomato = false;

    public function __construct(int $size)
    {
        $this->size = $size;
    }

    public function addPepperoni()
    {
        $this->pepperoni = true;
        return $this;
    }

    public function addLettuce()
    {
        $this->lettuce = true;
        return $this;
    }

    public function addCheese()
    {
        $this->cheese = true;
        return $this;
    }

    public function addTomato()
    {
        $this->tomato = true;
        return $this;
    }

    public function build(): Burger
    {
        return new Burger($this);
    }
}
```

然后它可以用作：

```php
$burger = (new BurgerBuilder(14))
                    ->addPepperoni()
                    ->addLettuce()
                    ->addTomato()
                    ->build();
```

**什么时候用？**

当可能存在几种类型的对象并避免构造函数伸缩时。与工厂模式的主要区别在于：当创建是一步过程时，将使用工厂模式，而当创建是多步骤过程时，将使用构建器模式。

### 🐑原型模式（Prototype）

现实世界的例子

> 记得多莉？被克隆的羊！让我们不详细介绍，但关键点在于它完全是关于克隆的

简单来说

> 通过克隆基于现有对象创建对象。

维基百科说

> 原型模式是软件开发中的创新设计模式。当要创建的对象类型由原型实例确定时使用它，该实例被克隆以生成新对象。

简而言之，它允许您创建现有对象的副本并根据需要进行修改，而不是从头开始创建对象并进行设置。

**程序化示例**

在PHP中，它可以很容易地使用 `clone`

```php
class Sheep
{
    protected $name;
    protected $category;

    public function __construct(string $name, string $category = 'Mountain Sheep')
    {
        $this->name = $name;
        $this->category = $category;
    }

    public function setName(string $name)
    {
        $this->name = $name;
    }

    public function getName()
    {
        return $this->name;
    }

    public function setCategory(string $category)
    {
        $this->category = $category;
    }

    public function getCategory()
    {
        return $this->category;
    }
}
```

然后它可以像下面一样克隆

```php
$original = new Sheep('Jolly');
echo $original->getName(); // Jolly
echo $original->getCategory(); // Mountain Sheep

// Clone and modify what is required
$cloned = clone $original;
$cloned->setName('Dolly');
echo $cloned->getName(); // Dolly
echo $cloned->getCategory(); // Mountain sheep
```

您也可以使用魔术方法`__clone`来修改克隆行为。

**什么时候用？**

当需要一个与现有对象类似的对象时，或者与克隆相比，创建的成本会很高。

### 💍单例模式（Singleton）

现实世界的例子

> 一次只能有一个国家的总统。无论何时打电话，都必须将同一位总统付诸行动。这里的总统是单身人士。

简单来说

> 确保只创建特定类的一个对象。

维基百科说

> 在软件工程中，单例模式是一种软件设计模式，它将类的实例化限制为一个对象。当需要一个对象来协调整个系统的操作时，这非常有用。

单例模式实际上被认为是反模式，应该避免过度使用它。它不一定是坏的，可能有一些有效的用例，但应谨慎使用，因为它在您的应用程序中引入了一个全局状态，并且在一个地方更改它可能会影响其他区域，并且它可能变得非常难以调试。关于它们的另一个坏处是它使你的代码紧密耦合加上嘲弄单例可能很困难。

**程序化示例**

要创建单例，请将构造函数设为私有，禁用克隆，禁用扩展并创建静态变量以容纳实例

```php
final class President
{
    private static $instance;

    private function __construct()
    {
        // Hide the constructor
    }

    public static function getInstance(): President
    {
        if (!self::$instance) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    private function __clone()
    {
        // Disable cloning
    }

    private function __wakeup()
    {
        // Disable unserialize
    }
}
```

然后才能使用

```php
$president1 = President::getInstance();
$president2 = President::getInstance();

var_dump($president1 === $president2); // true
```

# [](#structural-design-patterns)结构型设计模式

简单来说

> 结构模式主要涉及对象组成，或者换句话说，实体如何相互使用。或者另一种解释是，它们有助于回答“如何构建软件组件？”

维基百科说

> 在软件工程中，结构设计模式是通过识别实现实体之间关系的简单方法来简化设计的设计模式。

* [适配器模式（Adapter）](#-adapter)
* [桥梁模式（Bridge）](#-bridge)
* [组合模式（Composite）](#-composite)
* [装饰模式（Decorator）](#-decorator)
* [门面模式（Facade）](#-facade)
* [享元模式（Flyweight）](#-flyweight)
* [代理模式（Proxy）](#-proxy)

### 🔌适配器模式（Adapter）

现实世界的例子

> 请注意，您的存储卡中有一些照片，需要将它们传输到计算机上。为了传输它们，您需要某种与您的计算机端口兼容的适配器，以便您可以将存储卡连接到您的计算机。在这种情况下，读卡器是适配器。另一个例子是着名的电源适配器; 三脚插头不能连接到双管插座，需要使用电源适配器，使其与双叉插座兼容。另一个例子是翻译人员将一个人所说的话翻译成另一个人

简单来说

> 适配器模式允许您在适配器中包装其他不兼容的对象，以使其与另一个类兼容。

维基百科说

> 在软件工程中，适配器模式是一种软件设计模式，它允许将现有类的接口用作另一个接口。它通常用于使现有类与其他类一起工作而无需修改其源代码。

**程序化示例**

考虑一个有猎人的游戏，他猎杀狮子。

首先，我们有一个`Lion`所有类型的狮子必须实现的接口

```php
interface Lion
{
    public function roar();
}

class AfricanLion implements Lion
{
    public function roar()
    {
    }
}

class AsianLion implements Lion
{
    public function roar()
    {
    }
}
```

猎人期望任何`Lion`接口的实现都可以进行搜索。

```php
class Hunter
{
    public function hunt(Lion $lion)
    {
        $lion->roar();
    }
}
```

现在让我们说我们必须`WildDog`在我们的游戏中添加一个，以便猎人也可以追捕它。但我们不能直接这样做，因为狗有不同的界面。为了使它与我们的猎人兼容，我们将不得不创建一个兼容的适配器

```php
// This needs to be added to the game
class WildDog
{
    public function bark()
    {
    }
}

// Adapter around wild dog to make it compatible with our game
class WildDogAdapter implements Lion
{
    protected $dog;

    public function __construct(WildDog $dog)
    {
        $this->dog = $dog;
    }

    public function roar()
    {
        $this->dog->bark();
    }
}
```

而现在`WildDog`可以在我们的游戏中使用`WildDogAdapter`。

```php
$wildDog = new WildDog();
$wildDogAdapter = new WildDogAdapter($wildDog);

$hunter = new Hunter();
$hunter->hunt($wildDogAdapter);
```

### 🚡桥梁模式（Bridge）

现实世界的例子

> 考虑您有一个包含不同页面的网站，您应该允许用户更改主题。你会怎么做？为每个主题创建每个页面的多个副本，或者您只是创建单独的主题并根据用户的首选项加载它们？桥模式允许你做第二个ie

![](Docs/images/bridge.png)

用简单的话说

> 桥模式是关于优先于继承的组合。实现细节从层次结构推送到具有单独层次结构的另一个对象。

维基百科说

> 桥接模式是软件工程中使用的设计模式，旨在“将抽象与其实现分离，以便两者可以独立变化”

**程序化示例**

从上面翻译我们的WebPage示例。这里我们有`WebPage`层次结构

```php
interface WebPage
{
    public function __construct(Theme $theme);
    public function getContent();
}

class About implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "About page in " . $this->theme->getColor();
    }
}

class Careers implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "Careers page in " . $this->theme->getColor();
    }
}
```

和单独的主题层次结构

```php

interface Theme
{
    public function getColor();
}

class DarkTheme implements Theme
{
    public function getColor()
    {
        return 'Dark Black';
    }
}
class LightTheme implements Theme
{
    public function getColor()
    {
        return 'Off white';
    }
}
class AquaTheme implements Theme
{
    public function getColor()
    {
        return 'Light blue';
    }
}
```

而且这两个层次结构

```php
$darkTheme = new DarkTheme();

$about = new About($darkTheme);
$careers = new Careers($darkTheme);

echo $about->getContent(); // "About page in Dark Black";
echo $careers->getContent(); // "Careers page in Dark Black";
```

### 🌿组合模式（Composite）

现实世界的例子

> 每个组织都由员工组成。每个员工都有相同的功能，即有工资，有一些责任，可能会或可能不会向某人报告，可能会或可能不会有一些下属等。

简单来说

> 复合模式允许客户以统一的方式处理单个对象。

维基百科说

> 在软件工程中，复合模式是分区设计模式。复合模式描述了一组对象的处理方式与对象的单个实例相同。复合的意图是将对象“组合”成树结构以表示部分整体层次结构。通过实现复合模式，客户可以统一处理单个对象和组合。

**程序化示例**

以上面的员工为例。这里我们有不同的员工类型

```php
interface Employee
{
    public function __construct(string $name, float $salary);
    public function getName(): string;
    public function setSalary(float $salary);
    public function getSalary(): float;
    public function getRoles(): array;
}

class Developer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;
    
    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}

class Designer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;

    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}
```

然后我们有一个由几种不同类型的员工组成的组织

```php
class Organization
{
    protected $employees;

    public function addEmployee(Employee $employee)
    {
        $this->employees[] = $employee;
    }

    public function getNetSalaries(): float
    {
        $netSalary = 0;

        foreach ($this->employees as $employee) {
            $netSalary += $employee->getSalary();
        }

        return $netSalary;
    }
}
```

然后它可以用作

```php
// Prepare the employees
$john = new Developer('John Doe', 12000);
$jane = new Designer('Jane Doe', 15000);

// Add them to organization
$organization = new Organization();
$organization->addEmployee($john);
$organization->addEmployee($jane);

echo "Net salaries: " . $organization->getNetSalaries(); // Net Salaries: 27000
```

### ☕装饰模式（Decorator）

现实世界的例子

> 想象一下，您经营一家提供多种服务的汽车服务店。现在你如何计算收费账单？您选择一项服务并动态地向其添加所提供服务的价格，直到您获得最终成本。这里的每种服务都是装饰者。

简单来说

> Decorator模式允许您通过将对象包装在装饰器类的对象中来动态更改对象在运行时的行为。

维基百科说

> 在面向对象的编程中，装饰器模式是一种设计模式，它允许将行为静态或动态地添加到单个对象，而不会影响同一类中其他对象的行为。装饰器模式通常用于遵守单一责任原则，因为它允许在具有独特关注区域的类之间划分功能。

**程序化示例**

让我们以咖啡为例。首先，我们有一个简单的咖啡实现咖啡界面


```php
interface Coffee
{
    public function getCost();
    public function getDescription();
}

class SimpleCoffee implements Coffee
{
    public function getCost()
    {
        return 10;
    }

    public function getDescription()
    {
        return 'Simple coffee';
    }
}
```

我们希望使代码可扩展，以允许选项在需要时修改它。让我们做一些附加组件（装饰器）

```php
class MilkCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 2;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', milk';
    }
}

class WhipCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 5;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', whip';
    }
}

class VanillaCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 3;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', vanilla';
    }
}
```

让我们现在喝杯咖啡

```php
$someCoffee = new SimpleCoffee();
echo $someCoffee->getCost(); // 10
echo $someCoffee->getDescription(); // Simple Coffee

$someCoffee = new MilkCoffee($someCoffee);
echo $someCoffee->getCost(); // 12
echo $someCoffee->getDescription(); // Simple Coffee, milk

$someCoffee = new WhipCoffee($someCoffee);
echo $someCoffee->getCost(); // 17
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip

$someCoffee = new VanillaCoffee($someCoffee);
echo $someCoffee->getCost(); // 20
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip, vanilla
```

### 📦门面模式（Facade）

现实世界的例子

> 你怎么打开电脑？“按下电源按钮”你说！这就是你所相信的，因为你正在使用计算机在外部提供的简单界面，在内部它必须做很多事情来实现它。这个复杂子系统的简单接口是一个外观。

简单来说

> Facade模式为复杂的子系统提供了简化的界面。

维基百科说

> 外观是一个对象，它为更大的代码体提供了简化的接口，例如类库。

**程序化示例**

从上面看我们的计算机示例。这里我们有电脑课

```php
class Computer
{
    public function getElectricShock()
    {
        echo "Ouch!";
    }

    public function makeSound()
    {
        echo "Beep beep!";
    }

    public function showLoadingScreen()
    {
        echo "Loading..";
    }

    public function bam()
    {
        echo "Ready to be used!";
    }

    public function closeEverything()
    {
        echo "Bup bup bup buzzzz!";
    }

    public function sooth()
    {
        echo "Zzzzz";
    }

    public function pullCurrent()
    {
        echo "Haaah!";
    }
}
```

在这里，我们有门面

```php
class ComputerFacade
{
    protected $computer;

    public function __construct(Computer $computer)
    {
        $this->computer = $computer;
    }

    public function turnOn()
    {
        $this->computer->getElectricShock();
        $this->computer->makeSound();
        $this->computer->showLoadingScreen();
        $this->computer->bam();
    }

    public function turnOff()
    {
        $this->computer->closeEverything();
        $this->computer->pullCurrent();
        $this->computer->sooth();
    }
}
```

现在使用立面

```php
$computer = new ComputerFacade(new Computer());
$computer->turnOn(); // Ouch! Beep beep! Loading.. Ready to be used!
$computer->turnOff(); // Bup bup buzzz! Haah! Zzzzz
```

### 🍃享元模式（Flyweight）

现实世界的例子

> 你有没有从一些摊位买到新鲜的茶？他们经常制作你需要的不止一个杯子，并为其他任何客户保存其余的，以节省资源，例如天然气等.Flyweight模式就是那个即共享。

简单来说

> 它用于通过尽可能多地与类似对象共享来最小化内存使用或计算开销。

维基百科说

> 在计算机编程中，flyweight是一种软件设计模式。flyweight是一个通过与其他类似对象共享尽可能多的数据来最小化内存使用的对象; 当简单的重复表示将使用不可接受的内存量时，它是一种大量使用对象的方法。

**程序化的例子**

从上面翻译我们的茶例子。首先，我们有茶类和茶具

```php
// Anything that will be cached is flyweight.
// Types of tea here will be flyweights.
class KarakTea
{
}

// Acts as a factory and saves the tea
class TeaMaker
{
    protected $availableTea = [];

    public function make($preference)
    {
        if (empty($this->availableTea[$preference])) {
            $this->availableTea[$preference] = new KarakTea();
        }

        return $this->availableTea[$preference];
    }
}
```

然后我们有`TeaShop`接受订单并为他们服务

```php
class TeaShop
{
    protected $orders;
    protected $teaMaker;

    public function __construct(TeaMaker $teaMaker)
    {
        $this->teaMaker = $teaMaker;
    }

    public function takeOrder(string $teaType, int $table)
    {
        $this->orders[$table] = $this->teaMaker->make($teaType);
    }

    public function serve()
    {
        foreach ($this->orders as $table => $tea) {
            echo "Serving tea to table# " . $table;
        }
    }
}
```

它可以如下使用

```php
$teaMaker = new TeaMaker();
$shop = new TeaShop($teaMaker);

$shop->takeOrder('less sugar', 1);
$shop->takeOrder('more milk', 2);
$shop->takeOrder('without sugar', 5);

$shop->serve();
// Serving tea to table# 1
// Serving tea to table# 2
// Serving tea to table# 5
```

### 🎱代理模式（Proxy）
现实世界的例子

> 你有没有用过门禁卡进门？打开该门有多种选择，即可以使用门禁卡或按下绕过安检的按钮打开。门的主要功能是打开，但在它上面添加了一个代理来添加一些功能。让我用下面的代码示例更好地解释它。

简单来说

> 使用代理模式，类表示另一个类的功能。

维基百科说

> 代理以其最一般的形式，是一个充当其他东西的接口的类。代理是一个包装器或代理对象，客户端正在调用它来访问幕后的真实服务对象。使用代理可以简单地转发到真实对象，或者可以提供额外的逻辑。在代理中，可以提供额外的功能，例如当对真实对象的操作是资源密集时的高速缓存，或者在调用对象的操作之前检查先决条件。

**程序化示例**

从上面看我们的安全门示例。首先我们有门界面和门的实现

```php
interface Door
{
    public function open();
    public function close();
}

class LabDoor implements Door
{
    public function open()
    {
        echo "Opening lab door";
    }

    public function close()
    {
        echo "Closing the lab door";
    }
}
```

然后我们有一个代理来保护我们想要的任何门

```php
class SecuredDoor
{
    protected $door;

    public function __construct(Door $door)
    {
        $this->door = $door;
    }

    public function open($password)
    {
        if ($this->authenticate($password)) {
            $this->door->open();
        } else {
            echo "Big no! It ain't possible.";
        }
    }

    public function authenticate($password)
    {
        return $password === '$ecr@t';
    }

    public function close()
    {
        $this->door->close();
    }
}
```

以下是它的使用方法

```php
$door = new SecuredDoor(new LabDoor());
$door->open('invalid'); // Big no! It ain't possible.

$door->open('$ecr@t'); // Opening lab door
$door->close(); // Closing lab door
```

另一个例子是某种数据映射器实现。例如，我最近使用这种模式为MongoDB制作了一个ODM（对象数据映射器），我在使用魔术方法的同时围绕mongo类编写了一个代理`__call()`。所有方法调用被代理到原始蒙戈类和结果检索到的返回，因为它是但在的情况下，`find`或`findOne`数据被映射到所需的类对象和对象返回代替`Cursor`。

## [](#behavioral-design-patterns)行为型设计模式

简单来说

> 它关注对象之间的职责分配。它们与结构模式的不同之处在于它们不仅指定了结构，还概述了它们之间的消息传递/通信模式。或者换句话说，他们协助回答“如何在软件组件中运行行为？”

维基百科说

> 在软件工程中，行为设计模式是识别对象之间的共同通信模式并实现这些模式的设计模式。通过这样做，这些模式增加了执行该通信的灵活性。

* [责任链模式（Chain Of Responsibilities）](#-chain-of-responsibility)
* [命令行模式（Command）](#-command)
* [迭代器模式（Iterator）](#-iterator)
* [中介者模式（Mediator）](#-mediator)
* [备忘录模式（Memento）](#-memento)
* [观察者模式（Observer）](#-observer)
* [访问者模式（Visitor）](#-visitor)
* [策略模式（Strategy）](#-strategy)
* [状态模式（State）](#-state)
* [模板方法模式（Template Method）](#-template-method)

### 🔗责任链模式（Chain Of Responsibilities）

现实世界的例子

> 例如，你有三种付款方式（`A`，`B`和`C`）安装在您的帐户; 每个都有不同的数量。`A`有100美元，`B`具有300美元和`C`具有1000美元，以及支付偏好被选择作为`A`再`B`然后`C`。你试着购买价值210美元的东西。使用责任链，首先`A`会检查帐户是否可以进行购买，如果是，则进行购买并且链条将被破坏。如果没有，请求将继续进行帐户`B`检查金额，如果是链将被破坏，否则请求将继续转发，直到找到合适的处理程序。在这里`A`，`B`和`C` 是链条的链接，整个现象是责任链。

简单来说

> 它有助于构建一系列对象。请求从一端进入并继续从一个对象到另一个对象，直到找到合适的处理程序。

维基百科说

> 在面向对象的设计中，责任链模式是一种由命令对象源和一系列处理对象组成的设计模式。每个处理对象都包含定义它可以处理的命令对象类型的逻辑; 其余的传递给链中的下一个处理对象。

**程序化示例**

翻译上面的帐户示例。首先，我们有一个基本帐户，其中包含将帐户链接在一起的逻辑和一些帐户

```php
abstract class Account
{
    protected $successor;
    protected $balance;

    public function setNext(Account $account)
    {
        $this->successor = $account;
    }

    public function pay(float $amountToPay)
    {
        if ($this->canPay($amountToPay)) {
            echo sprintf('Paid %s using %s' . PHP_EOL, $amountToPay, get_called_class());
        } elseif ($this->successor) {
            echo sprintf('Cannot pay using %s. Proceeding ..' . PHP_EOL, get_called_class());
            $this->successor->pay($amountToPay);
        } else {
            throw new Exception('None of the accounts have enough balance');
        }
    }

    public function canPay($amount): bool
    {
        return $this->balance >= $amount;
    }
}

class Bank extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Paypal extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Bitcoin extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}
```

现在让我们使用上面定义的链接准备链（即Bank，Paypal，Bitcoin）

```php
// Let's prepare a chain like below
//      $bank->$paypal->$bitcoin
//
// First priority bank
//      If bank can't pay then paypal
//      If paypal can't pay then bit coin

$bank = new Bank(100);          // Bank with balance 100
$paypal = new Paypal(200);      // Paypal with balance 200
$bitcoin = new Bitcoin(300);    // Bitcoin with balance 300

$bank->setNext($paypal);
$paypal->setNext($bitcoin);

// Let's try to pay using the first priority i.e. bank
$bank->pay(259);

// Output will be
// ==============
// Cannot pay using bank. Proceeding ..
// Cannot pay using paypal. Proceeding ..:
// Paid 259 using Bitcoin!
```

### 👮命令行模式（Command）

现实世界的例子

> 一个通用的例子是你在餐厅点餐。您（即`Client`）要求服务员（即`Invoker`）携带一些食物（即`Command`），服务员只是将请求转发给主厨（即`Receiver`），该主厨知道什么以及如何烹饪。另一个例子是你（即）使用遥控器（）`Client`打开（即`Command`）电视（即）。`Receiver``Invoker`

简单来说

> 允许您将操作封装在对象中。这种模式背后的关键思想是提供将客户端与接收器分离的方法。

维基百科说

> 在面向对象的编程中，命令模式是行为设计​​模式，其中对象用于封装执行动作或稍后触发事件所需的所有信息。此信息包括方法名称，拥有该方法的对象以及方法参数的值。

**程序化示例**

首先，我们有接收器，它可以执行每个可以执行的操作

```php
// Receiver
class Bulb
{
    public function turnOn()
    {
        echo "Bulb has been lit";
    }

    public function turnOff()
    {
        echo "Darkness!";
    }
}
```

然后我们有一个接口，每个命令将实现，然后我们有一组命令

```php
interface Command
{
    public function execute();
    public function undo();
    public function redo();
}

// Command
class TurnOn implements Command
{
    protected $bulb;

    public function __construct(Bulb $bulb)
    {
        $this->bulb = $bulb;
    }

    public function execute()
    {
        $this->bulb->turnOn();
    }

    public function undo()
    {
        $this->bulb->turnOff();
    }

    public function redo()
    {
        $this->execute();
    }
}

class TurnOff implements Command
{
    protected $bulb;

    public function __construct(Bulb $bulb)
    {
        $this->bulb = $bulb;
    }

    public function execute()
    {
        $this->bulb->turnOff();
    }

    public function undo()
    {
        $this->bulb->turnOn();
    }

    public function redo()
    {
        $this->execute();
    }
}
```

然后我们`Invoker`与客户端进行交互以处理任何命令

```php
// Invoker
class RemoteControl
{
    public function submit(Command $command)
    {
        $command->execute();
    }
}
```

最后，让我们看看我们如何在客户端使用它

```php
$bulb = new Bulb();

$turnOn = new TurnOn($bulb);
$turnOff = new TurnOff($bulb);

$remote = new RemoteControl();
$remote->submit($turnOn); // Bulb has been lit!
$remote->submit($turnOff); // Darkness!
```

命令模式还可用于实现基于事务的系统。在执行命令时，一直保持命令历史记录的位置。如果成功执行了最后一个命令，那么所有好处都只是遍历历史记录并继续执行`undo`所有已执行的命令。

### ➿迭代器模式（Iterator）

现实世界的例子

> 旧的无线电设备将是迭代器的一个很好的例子，用户可以从某个频道开始，然后使用下一个或上一个按钮来浏览相应的频道。或者以MP3播放器或电视机为例，您可以按下下一个和上一个按钮来浏览连续的频道，换句话说，它们都提供了一个界面来迭代各自的频道，歌曲或电台。

简单来说

> 它提供了一种访问对象元素而不暴露底层表示的方法。

维基百科说

> 在面向对象的编程中，迭代器模式是一种设计模式，其中迭代器用于遍历容器并访问容器的元素。迭代器模式将算法与容器分离; 在某些情况下，算法必然是特定于容器的，因此不能解耦。

**程序化的例子**

在PHP中，使用SPL（标准PHP库）很容易实现。从上面翻译我们的广播电台示例。首先，我们有`RadioStation`

```php
class RadioStation
{
    protected $frequency;

    public function __construct(float $frequency)
    {
        $this->frequency = $frequency;
    }

    public function getFrequency(): float
    {
        return $this->frequency;
    }
}
```

然后我们有了迭代器

```php
use Countable;
use Iterator;

class StationList implements Countable, Iterator
{
    /** @var RadioStation[] $stations */
    protected $stations = [];

    /** @var int $counter */
    protected $counter;

    public function addStation(RadioStation $station)
    {
        $this->stations[] = $station;
    }

    public function removeStation(RadioStation $toRemove)
    {
        $toRemoveFrequency = $toRemove->getFrequency();
        $this->stations = array_filter($this->stations, function (RadioStation $station) use ($toRemoveFrequency) {
            return $station->getFrequency() !== $toRemoveFrequency;
        });
    }

    public function count(): int
    {
        return count($this->stations);
    }

    public function current(): RadioStation
    {
        return $this->stations[$this->counter];
    }

    public function key()
    {
        return $this->counter;
    }

    public function next()
    {
        $this->counter++;
    }

    public function rewind()
    {
        $this->counter = 0;
    }

    public function valid(): bool
    {
        return isset($this->stations[$this->counter]);
    }
}
```

然后它可以用作

```php
$stationList = new StationList();

$stationList->addStation(new RadioStation(89));
$stationList->addStation(new RadioStation(101));
$stationList->addStation(new RadioStation(102));
$stationList->addStation(new RadioStation(103.2));

foreach($stationList as $station) {
    echo $station->getFrequency() . PHP_EOL;
}

$stationList->removeStation(new RadioStation(89)); // Will remove station 89
```

### 👽中介者模式（Mediator）

现实世界的例子

> 一个典型的例子就是当你在手机上与某人交谈时，有一个网络提供商坐在你和他们之间，你的对话通过它而不是直接发送。在这种情况下，网络提供商是中介。

简单来说

> Mediator模式添加第三方对象（称为mediator）来控制两个对象（称为同事）之间的交互。它有助于减少彼此通信的类之间的耦合。因为现在他们不需要了解彼此的实施。

维基百科说

> 在软件工程中，中介模式定义了一个对象，该对象封装了一组对象的交互方式。由于它可以改变程序的运行行为，因此这种模式被认为是一种行为模式。

**程序化示例**

这是聊天室（即中介）与用户（即同事）相互发送消息的最简单示例。

首先，我们有调解员即聊天室


```php
interface ChatRoomMediator 
{
    public function showMessage(User $user, string $message);
}

// Mediator
class ChatRoom implements ChatRoomMediator
{
    public function showMessage(User $user, string $message)
    {
        $time = date('M d, y H:i');
        $sender = $user->getName();

        echo $time . '[' . $sender . ']:' . $message;
    }
}
```

然后我们有我们的用户，即同事

```php
class User {
    protected $name;
    protected $chatMediator;

    public function __construct(string $name, ChatRoomMediator $chatMediator) {
        $this->name = $name;
        $this->chatMediator = $chatMediator;
    }

    public function getName() {
        return $this->name;
    }

    public function send($message) {
        $this->chatMediator->showMessage($this, $message);
    }
}
```

和用法

```php
$mediator = new ChatRoom();

$john = new User('John Doe', $mediator);
$jane = new User('Jane Doe', $mediator);

$john->send('Hi there!');
$jane->send('Hey!');

// Output will be
// Feb 14, 10:58 [John]: Hi there!
// Feb 14, 10:58 [Jane]: Hey!
```

### 💾备忘录模式（Memento）

现实世界的例子

> 以计算器（即发起者）为例，无论何时执行某些计算，最后的计算都会保存在内存中（即纪念品），以便您可以回到它并使用某些操作按钮（即看管人）恢复它。

简单来说

> Memento模式是关于以稍后可以以平滑方式恢复的方式捕获和存储对象的当前状态。

维基百科说

> memento模式是一种软件设计模式，它提供将对象恢复到其先前状态的能力（通过回滚撤消）。

当您需要提供某种撤消功能时通常很有用。

**程序化示例**

让我们举一个文本编辑器的例子，它不时地保存状态，你可以根据需要恢复。

首先，我们有memento对象，可以保存编辑器状态

```php
class EditorMemento
{
    protected $content;

    public function __construct(string $content)
    {
        $this->content = $content;
    }

    public function getContent()
    {
        return $this->content;
    }
}
```

然后我们有我们的编辑器即即将使用memento对象的创作者

```php
class Editor
{
    protected $content = '';

    public function type(string $words)
    {
        $this->content = $this->content . ' ' . $words;
    }

    public function getContent()
    {
        return $this->content;
    }

    public function save()
    {
        return new EditorMemento($this->content);
    }

    public function restore(EditorMemento $memento)
    {
        $this->content = $memento->getContent();
    }
}
```

然后它可以用作

```php
$editor = new Editor();

// Type some stuff
$editor->type('This is the first sentence.');
$editor->type('This is second.');

// Save the state to restore to : This is the first sentence. This is second.
$saved = $editor->save();

// Type some more
$editor->type('And this is third.');

// Output: Content before Saving
echo $editor->getContent(); // This is the first sentence. This is second. And this is third.

// Restoring to last saved state
$editor->restore($saved);

$editor->getContent(); // This is the first sentence. This is second.
```

### 😎观察者模式（Observer）

现实世界的例子

> 一个很好的例子是求职者，他们订阅了一些职位发布网站，只要有匹配的工作机会，他们就会得到通知。

简单来说

> 定义对象之间的依赖关系，以便每当对象更改其状态时，都会通知其所有依赖项。

维基百科说

> 观察者模式是一种软件设计模式，其中一个称为主体的对象维护其依赖者列表，称为观察者，并通常通过调用其中一种方法自动通知它们任何状态变化。

**程序化的例子**

从上面翻译我们的例子。首先，我们有求职者需要通知职位发布

```php
class JobPost
{
    protected $title;

    public function __construct(string $title)
    {
        $this->title = $title;
    }

    public function getTitle()
    {
        return $this->title;
    }
}

class JobSeeker implements Observer
{
    protected $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function onJobPosted(JobPost $job)
    {
        // Do something with the job posting
        echo 'Hi ' . $this->name . '! New job posted: '. $job->getTitle();
    }
}
```

然后我们会找到求职者会订阅的招聘信息

```php
class EmploymentAgency implements Observable
{
    protected $observers = [];

    protected function notify(JobPost $jobPosting)
    {
        foreach ($this->observers as $observer) {
            $observer->onJobPosted($jobPosting);
        }
    }

    public function attach(Observer $observer)
    {
        $this->observers[] = $observer;
    }

    public function addJob(JobPost $jobPosting)
    {
        $this->notify($jobPosting);
    }
}
```

然后它可以用作

```php
// Create subscribers
$johnDoe = new JobSeeker('John Doe');
$janeDoe = new JobSeeker('Jane Doe');

// Create publisher and attach subscribers
$jobPostings = new EmploymentAgency();
$jobPostings->attach($johnDoe);
$jobPostings->attach($janeDoe);

// Add a new job and see if subscribers get notified
$jobPostings->addJob(new JobPost('Software Engineer'));

// Output
// Hi John Doe! New job posted: Software Engineer
// Hi Jane Doe! New job posted: Software Engineer
```

### 🏃访问者模式（Visitor）

现实世界的例子

> 考虑去迪拜的人。他们只需要一种方式（即签证）进入迪拜。抵达后，他们可以自己来迪拜的任何地方，而无需征求许可或做一些腿部工作，以便访问这里的任何地方; 让他们知道一个地方，他们可以访问它。访客模式可以让您做到这一点，它可以帮助您添加访问的地方，以便他们可以尽可能多地访问，而无需做任何腿部工作。

简单来说

> 访客模式允许您向对象添加更多操作，而无需修改它们。

维基百科说

> 在面向对象的编程和软件工程中，访问者设计模式是一种将算法与其运行的对象结构分离的方法。这种分离的实际结果是能够在不修改这些结构的情况下向现有对象结构添加新操作。这是遵循开放/封闭原则的一种方式。

**程序化的例子**

让我们举一个动物园模拟的例子，我们有几种不同的动物，我们必须让它们成为声音。让我们用访客模式翻译这个

```php
// Visitee
interface Animal
{
    public function accept(AnimalOperation $operation);
}

// Visitor
interface AnimalOperation
{
    public function visitMonkey(Monkey $monkey);
    public function visitLion(Lion $lion);
    public function visitDolphin(Dolphin $dolphin);
}
```

然后我们有动物实施

```php
class Monkey implements Animal
{
    public function shout()
    {
        echo 'Ooh oo aa aa!';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitMonkey($this);
    }
}

class Lion implements Animal
{
    public function roar()
    {
        echo 'Roaaar!';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitLion($this);
    }
}

class Dolphin implements Animal
{
    public function speak()
    {
        echo 'Tuut tuttu tuutt!';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitDolphin($this);
    }
}
```

让我们实现我们的访客

```php
class Speak implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        $monkey->shout();
    }

    public function visitLion(Lion $lion)
    {
        $lion->roar();
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        $dolphin->speak();
    }
}
```

然后它可以用作

```php
$monkey = new Monkey();
$lion = new Lion();
$dolphin = new Dolphin();

$speak = new Speak();

$monkey->accept($speak);    // Ooh oo aa aa!    
$lion->accept($speak);      // Roaaar!
$dolphin->accept($speak);   // Tuut tutt tuutt!
```

我们可以通过为动物建立一个继承层次结构来做到这一点，但是每当我们不得不为动物添加新动作时我们就必须修改动物。但现在我们不必改变它们。例如，假设我们被要求向动物添加跳跃行为，我们可以通过创建新的访问者来添加它，即

```php
class Jump implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        echo 'Jumped 20 feet high! on to the tree!';
    }

    public function visitLion(Lion $lion)
    {
        echo 'Jumped 7 feet! Back on the ground!';
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        echo 'Walked on water a little and disappeared';
    }
}
```

并用于使用

```php
$jump = new Jump();

$monkey->accept($speak);   // Ooh oo aa aa!
$monkey->accept($jump);    // Jumped 20 feet high! on to the tree!

$lion->accept($speak);     // Roaaar!
$lion->accept($jump);      // Jumped 7 feet! Back on the ground!

$dolphin->accept($speak);  // Tuut tutt tuutt!
$dolphin->accept($jump);   // Walked on water a little and disappeared
```

### 💡策略模式（Strategy）

现实世界的例子

> 考虑排序的例子，我们实现了冒泡排序，但数据开始增长，冒泡排序开始变得非常缓慢。为了解决这个问题，我们实现了快速排序。但是现在虽然快速排序算法对大型数据集的效果更好，但对于较小的数据集来说速度非常慢。为了解决这个问题，我们实施了一个策略，对于小型数据集，将使用冒泡排序并进行更大规模的快速排序。

简单来说

> 策略模式允许您根据情况切换算法或策略。

维基百科说

> 在计算机编程中，策略模式（也称为策略模式）是一种行为软件设计模式，可以在运行时选择算法的行为。

**程序化的例子**

从上面翻译我们的例子。首先，我们有战略界面和不同的战略实施

```php
interface SortStrategy
{
    public function sort(array $dataset): array;
}

class BubbleSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "Sorting using bubble sort";

        // Do sorting
        return $dataset;
    }
}

class QuickSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "Sorting using quick sort";

        // Do sorting
        return $dataset;
    }
}
```

然后我们的客户将使用任何策略

```php
class Sorter
{
    protected $sorter;

    public function __construct(SortStrategy $sorter)
    {
        $this->sorter = $sorter;
    }

    public function sort(array $dataset): array
    {
        return $this->sorter->sort($dataset);
    }
}
```

它可以用作

And it can be used as
```php
$dataset = [1, 5, 4, 3, 2, 8];

$sorter = new Sorter(new BubbleSortStrategy());
$sorter->sort($dataset); // Output : Sorting using bubble sort

$sorter = new Sorter(new QuickSortStrategy());
$sorter->sort($dataset); // Output : Sorting using quick sort
```

### 💢状态模式（State）

现实世界的例子

> 想象一下，你正在使用一些绘图应用程序，你选择绘制画笔。现在画笔根据所选颜色改变其行为，即如果你选择了红色，它会画成红色，如果是蓝色则会是蓝色等。

简单来说

> 它允许您在状态更改时更改类的行为。

维基百科说

> 状态模式是一种行为软件设计模式，它以面向对象的方式实现状态机。使用状态模式，通过将每个单独的状态实现为状态模式接口的派生类来实现状态机，并通过调用由模式的超类定义的方法来实现状态转换。状态模式可以解释为一种策略模式，它能够通过调用模式接口中定义的方法来切换当前策略。

**程序化的例子**

让我们以文本编辑器为例，它允许您更改键入的文本的状态，即如果您选择了粗体，则开始以粗体显示，如果是斜体，则以斜体显示等。

首先，我们有状态接口和一些状态实现

```php
interface WritingState
{
    public function write(string $words);
}

class UpperCase implements WritingState
{
    public function write(string $words)
    {
        echo strtoupper($words);
    }
}

class LowerCase implements WritingState
{
    public function write(string $words)
    {
        echo strtolower($words);
    }
}

class DefaultText implements WritingState
{
    public function write(string $words)
    {
        echo $words;
    }
}
```

然后我们有编辑

```php
class TextEditor
{
    protected $state;

    public function __construct(WritingState $state)
    {
        $this->state = $state;
    }

    public function setState(WritingState $state)
    {
        $this->state = $state;
    }

    public function type(string $words)
    {
        $this->state->write($words);
    }
}
```

然后它可以用作

```php
$editor = new TextEditor(new DefaultText());

$editor->type('First line');

$editor->setState(new UpperCase());

$editor->type('Second line');
$editor->type('Third line');

$editor->setState(new LowerCase());

$editor->type('Fourth line');
$editor->type('Fifth line');

// Output:
// First line
// SECOND LINE
// THIRD LINE
// fourth line
// fifth line
```

### 📒<模板方法模式（Template Method）

现实世界的例子

> 假设我们正在建造一些房屋。构建的步骤可能看起来像
> 
> *   准备房子的基地
> *   建造墙壁
> *   添加屋顶
> *   添加其他楼层

> 这些步骤的顺序永远不会改变，即在建造墙壁等之前不能建造屋顶，但是每个步骤都可以修改，例如墙壁可以由木头或聚酯或石头制成。

简单来说

> 模板方法定义了如何执行某个算法的框架，但是将这些步骤的实现推迟到子类。

维基百科说

> 在软件工程中，模板方法模式是一种行为设计模式，它定义了操作中算法的程序框架，将一些步骤推迟到子类。它允许重新定义算法的某些步骤而不改变算法的结构。

**程序化示例**

想象一下，我们有一个构建工具，可以帮助我们测试，lint，构建，生成构建报告（即代码覆盖率报告，linting报告等），并在测试服务器上部署我们的应用程序。

首先，我们有基类，它指定构建算法的骨架

```php
abstract class Builder
{

    // Template method
    final public function build()
    {
        $this->test();
        $this->lint();
        $this->assemble();
        $this->deploy();
    }

    abstract public function test();
    abstract public function lint();
    abstract public function assemble();
    abstract public function deploy();
}
```

然后我们可以实现我们的实现

```php
class AndroidBuilder extends Builder
{
    public function test()
    {
        echo 'Running android tests';
    }

    public function lint()
    {
        echo 'Linting the android code';
    }

    public function assemble()
    {
        echo 'Assembling the android build';
    }

    public function deploy()
    {
        echo 'Deploying android build to server';
    }
}

class IosBuilder extends Builder
{
    public function test()
    {
        echo 'Running ios tests';
    }

    public function lint()
    {
        echo 'Linting the ios code';
    }

    public function assemble()
    {
        echo 'Assembling the ios build';
    }

    public function deploy()
    {
        echo 'Deploying ios build to server';
    }
}
```

然后它可以用作

```php
$androidBuilder = new AndroidBuilder();
$androidBuilder->build();

// Output:
// Running android tests
// Linting the android code
// Assembling the android build
// Deploying android build to server

$iosBuilder = new IosBuilder();
$iosBuilder->build();

// Output:
// Running ios tests
// Linting the ios code
// Assembling the ios build
// Deploying ios build to server
```

### 🚦总结一下

那就是把它包起来。我将继续改进这一点，因此您可能希望观看/加注此存储库以重新访问。此外，我计划对架构模式进行相同的编写，请继续关注它。

### License

[![](https://camo.githubusercontent.com/45b46deab81a0adb3164212be341f1dd65111cf3/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4c6963656e73652d43432532304259253230342e302d6c69676874677265792e737667)](https://creativecommons.org/licenses/by/4.0/)

### [转载](https://github.com/guanguans/design-patterns-for-humans-cn)

* 转载为了记录.

