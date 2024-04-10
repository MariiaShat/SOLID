SOLID - это процесс, который помогает с инкапсуляцией отдельных сущностей и с прозрачной организацией архитектуры, так они могут получиться при использовании принципа SOLID. Это нам говорит о том, что при использовании сразу нескольких принципов, как единого целого, намного лучше, чем использовать каждый принцип по отдельности.
1.	[S]ingle responsibility - принцип единой ответственности
2.	[O]pen-closed - принцип открытостей и закрытостей
3.	[L]iskov substitution - принцип подстановки
4.	[I]nterface segregation - принцип разделение интерфейсов
5.	[D]ependency inversion - принцип инферсии зависимости.
У каждого есть область и границы приминения, для примера разберем каждый из принципов для того чтобы определить их зоны:
1.	Принцип единой ответственности, звучит как у модуля должна быть одна причина для изменения или класс, должен отвечать только за что-то одно. Например:
class Auto { constructor (model: string) { } getCarModel() { } saveCustomer0rder (o: Auto) { } setCarModel() { } getCustomer0rder(id: string) { } removeCustomer0rder ( id: string) { } updateCarSet(set: object) { } }
(Чтобы не хранить в одном классе куча методов, которые постоянно будут изменяться от любых факторов, будем использовать первый принцип, так как эти изменения нарушают принцип единой ответственности, потому что теперь у классов есть полно причин чтобы изменяться)
class Auto { constructor (model: string) { } getCarModel() { } setCarModel( ) { } } class CustomerAuto { saveCustomer0rder (o: Auto) { } getCustomer0rder( id: string) { } removeCustomer0rder( id: string) { } } class AutoDB { updateCarSet( set: object) { } }
(Применяя первый принцип, у нас получается разбить все на отдельные сущности, где каждый отвечает за свою определенную задачу)
2.	Принцип открытостей и закрытостей звучит ,как модуль должен быть открыт для рассширения, но закрыт для изменения. Основная идея этого принципа разработка устойчивого к изменениям приложения.
(Рассмотрим пример, у нас есть магазин автомобилей, где мы продаем Теслу и Ауди, а то есть массив из двух интенсов)
class Auto { constructor (public model: string) { } getCarModel ( ) { } } const shop: Array = [ new Auto( 'Tesla'), new Auto( 'Audi'), ];
(Теперь для этих моделей, нам нужно сформировать цену, в этом нам поможет функция getPrice, она примет существующий массив,пробежится по нему, найдет нужную модель и на основании его выдаст нам правильную цену)
const getPrice = (auto: Array): string | void => { for (let i = 0; i < auto. length; i++) { switch (auto [i].model) { case 'Tesla': return '80 000$' case 'Audi': return '50 000$'; default: return 'No Auto Price' } }
} getPrice (shop) ;
(Вроде все хорошо, но проблемы придут, если к нам на склад завезут новую модель, например BMW и нам придется и для новой модели рассщитывать новую цену. И если мы просто рассширили массив новым значением, то функцию getPrice пришлось изменить, тк нужной логики она не сдержала)
const shop: Array Auto> = [ new Auto( 'Tesla'), new Auto( 'Audi'), new Auto( 'BMW'), ];
const getPrice = (auto: Array): string | void => { for (let i = 0; i < auto. length; it+) { switch (auto [i].model) { case 'Tesla': return '80 000$'; case 'Audi': return '50 000$'; case 'BMW': return '70 000$'; default: return 'No Auto Price'; } } }
(Теперь, применяя второй принцип, у нас появилась Абстракция, в которой описывается метод возврата цены. Дальше отталкиваясь от родительского класса абстракции, создаем дочерние классы, реализовав в каждом подклассе метод getPrice, который возвращает цену.)
abstract class CarPrice { abstract getPrice(): string; }
class Tesla extends CarPrice { getPrice() { return '80 000$'; } } class Audi extends CarPrice { getPrice() { return '50 000$'; } } class Bmw extends CarPrice { getPrice() { return '70 000$'; } }
const shop: Array = [ new Tesla(), new Audi(), new Bmw(), ]; const getPrice = (auto: Array<CarPrices): string | void => { for (let i = 0; i < auto. length; i++) { auto[i].getPrice() ; } } getPrice(shop);
(Так же изменяем функцию getPrice, чтобы при переборе массива она просто вызывала метод getPrice у каждого элемента. Таким образом мы убираем все последующие изменения внутри нее, а если добавляется новая модель, то мы просто [рассширяем] массив существующих авто и [не изменяем] реализацию выдачи цены)
3.	Принцип подстановки звучит, как необходимо чтобы классы могли служить заменой для своих супер классов. Основная цель, это проектировать логику таким образом, чтобы класс и наследники могли использоваться вместо родителей.
(Приведем пример. Предположим у нас есть класс Rectangle(Треугольник), который принимает высоту и ширину, а так же имеет три метода установка высоты, установка ширины и расчет площади)
class Rectangle { constructor(public width: number, public height: number) {] setwidth(width: number) { this.width = width; } setHeight(height: number) { this.height = height; } areadf ( ): number { return this.width * this .height; } }
(Однако предположим, что нам понадобился класс Square(Квадрат). Мы можем видеть, что класс Square наследуется от класса Rectangle и определяет его методы и на данный момент вроде бы все логично, но уже на этом этапе мы нарушаем принцип подстановки )
class Square extends Rectangle { width: number = 0; height: number = 0; constructor (size: number) { super(size, size); } setwidth(width: number) { this .width = width; this. height = width; } setHeight(height: number) { this.width = height; this.height = height; } }
(Мы можем определить интенс квадрата и даже поменять его стороны, все будет работать корректно, нужные величины выдаются)
const mySquare = new Square(20); mySquare. setwidth(30); mySquare, setWidth(40);
( Но если вдруг Rectangle мы будем использовать в качестве интерфейса, а работать будем корректно с классом Square, то будут проблемы. Из за того, что мы используем класс квадрата, вместо того чтобы каждое из полей устанавливать отдельно, мы изменяем сразу оба параметра )
const changeShapeSize = (figure: Rectangle): void => { figure.setWidth(10); figure. setHeight(20); } const changeShapeSize = (figure: Rectangle): void => { figure.setwidth(10); figure. setHeight(20); }
(Применяя третий принцип, мы создаем общий интерфейс для всех классов и вместо наследования одного класса от другого, используем интерфейс, таким образом данный принцип помогает выявлять проблемные абстракции и скрытые связи между сущностями)
interface Figure { setwidth(width: number): void; setHeight (width: number): void; areadf(): void; } class Rectangle implements Figure { setWidth(width: number) { } setHeight (height: number) { } areadf ( ){ } } class Square implements Figure { setwidth(width: number) { } setHeight(height: number) { } areadf ( ) { } }
4.	Принцип разделения интерфейсов звучит, как сущности не должны зависить от интерфейсов, которые не используют.
(В качестве примера, предположим, что интерфейс AutoSet, который содержит методы комплектаций всех наших автомобилей, у нас это три простых метода)
interface AutoSet { getTeslaSet(): any; getAudiSet(): any; getBmwSet(): any; }
(Однако, если мы захотим создать класс наследуясь нашего интерфейса, то возникнет проблема. Если мы наследуемся от интерфейса, то все методы реализованные в нем должны быть обязательно описаны в классе потомке, в результате чего каждый класс получит минимум по два ненужных ему метода. )
class Tesla implements AutoSet { getTeslaSet(): any { }; getAudiSet() : any { }; getBmwSet(): any { }; } class Audi implements AutoSet { getTeslaset(): any { }; getAudiSet(): any { }; getBmwSet(): any { }; } class Bmw implements AutoSet { getTeslaSet(): any { }; getAudiSet(): any { }; getBmwSet(): any { }; }
(Применяя четвертый принцип, решение, оказывается, простым. Как понятно из названия принципа, интерфейсы нужно разделить, таким образом у нас есть три основных интерфейса, которые не зависят друг от друга и которые содержат описание будующим имплементациям. После чего от каждого интерфейса мы создаем класс наследник, который на этот раз содержит в себе только нужную ему функциональность)
interface TeslaSet { getTeslaSet(): any; } interface AudiSet { getAudiSet (): any; } interface BmwSet { getBmwSet(): any; } class Tesla implements TeslaSet { getTeslaSet(): any { }; } class Audi implements AudiSet { getAudiSet(): any { }; } class Bmw implements BmwSet { getBmwSet(): any { }; }
5.	Принцип инферсии зависимости. Он содержит два основных утверждения: 1. Модули высших уровней не должны зависить от модулей низших уровней. Оба должны зависеть от абстракций. 2. Абстракции не должны зависеть от деталей, детали должны зависеть от абстракций.
(Приведем пример, у нас есть низкоуровневый класс xmlHttpRequestService, в котором реализована логика запроса и есть высокоуровневый класс Http, в конструктор которого мы передаем низкоуровневый модуль, после чего дергаем его методы. Таким образом мы нарушаем принцип инферсии зависимости)
class xmlHttpRequestService { }
class xmIHttpService extends xmIHttpRequestService { request(url: string, type: string) { } }
class Http { constructor (private xmlHttpService: xmlHttpService) { }
get(url: string, options: any) { this.xmlHttpService.request(url, 'GET'); } post(url: string) { this.xmlHttpService.request (url, 'POST'); } }
(Чтобы решить эту проблему, для начала нам нужно создать интерфейс и этот интерфейс содержит реализацию метода request)
interface Connection { request (url: string, options: any): any; }
(После чего в наш высокоуровневый класс мы передаем именно интерфейс и вот, теперь наш класс не зависет от низкоуровневого модуля, а оба зависят от абстракции в виде интерфейса Connection)
class Http { constructor (private httpConnection: Connection) { }
get (url: string, options: any) { this.httpConnection.request(url, 'GET'); } post(url: string) { this.httpConnection.request(url, 'POST'); } }
(И последнее, что нам осталось сделать это переписать класс xmIHttpService чтобы он реализовал интерфейс Connection)
class mlHttpRequestService { open( ) { } send()) { } } interface Connection { request(url: string, options: any): any;
class xmlHttpService implements Connection { xhr = new xmlHttpRequestService() ;
request (url: string, type: string) { this.xhr.open(); this.xhr.send(); } }

