1. Single responsibility (Принцип единой отвественности).
У модуля должна только одна причина для изменения. P.s: В идеале при возникновении действий должен затрагиватся только один класс.

Пример: 
У нас есть первоначальный класс.

class Auto{
    constructor(model: string){}
    getCarModel(){}
    saveCustomerOrder(o: Auto){}
}

Но у нас появились новые сущности через месяц. Вот что будет с ним:

class Auto{
    constructor(model: string){}
    getCarModel(){}
    saveCustomerOrder(o: Auto){}
    setCarModel(){}
    getCutomerOrder(id: string){}
    removeCustomerOrder(id: string){}
    updateCarSet(set: object){}
}

Он превратился всякий мусор. Чтобы такое избежать мы разбиваем на отдельные сущности.

Пример:

class Auto{
    constructor(model: string){}
    getCarModel(){}
    setCarModel(){}
}

class CustomerAuto{
    saveCustomerOrder(o: Auto){}
    getCutomerOrder(id: string){}
    removeCustomerOrder(id: string){}
}

class AutoDB{
    updateCarSet(set: object){}
}

Теперь у нас есть 3 отдельные функции.

2. Open-Closed (Принцип открытости и закрытости.)
Модуль должен быть открыт для расширения, но закрыт для изменения.

Пример:

class Auto{
    constuctor(public model: string){}
    getCarModel(){}
}

const shop: Array<Auto> = [
    new Auto("Tesla"),
    new Auto("Audi"),
];

const getPrice = (auto: Array<Auto>): string | void => {
    for (let i = 0; i < auto.length; i++){
        switch (auto[i].model){
            case 'Tesla': return '80.000$';
            case 'Audi': return '50.000$';
            default: return 'No Auto Price';
        }
    }
}

getPrice(shop);

Все выглядит замечательно, но если добавить новую модель машины, то придется менять и "const getPrice", именно это уже не будет "Принцип открытости и закрытости."

Поэтому мы делаем следующее:

abstract class CarPrice{
    abstact getPrice(): string;
}

class Tesla extends CarPrice{
    getPrice(){
        return '80.000$';
    }
}

class Audi extends CarPrice{
    getPrice(){
        return '50.000$';
    }
}

class Bmw extends CarPrice{
    getPrice(){
        return '70.000$';
    }
}

После этого мы меняем "const getPrice"

const shop: Array<Auto> = [
    new Auto("Tesla"),
    new Auto("Audi"),
    new Auto("Bmw")
];

const getPrice = (auto: Array<Auto>): string | void => {
    for (let i = 0; i < auto.length; i++){
        auto[i].getPrice();
    }
}

getPrice(shop);

3. Liskov Substitution (Принцип подстановки Барбары Лисков.)

Пример:

class Rectangle {
    constructor(public width: number, public heigth: number)

    setWidth(width: number){
        this.width = width;
    }

    setHeigth(heigth: number){
        this.heigth = heigth;
    }

    areaOf(): number {
        return this.width * this.heigth
    }
}

class Square extends Rectangle{
    width: number = 0;
    heigth: number = 0;

    constructor(size: number){
        super(size, size)
    }

    setWidth(width: number){
        this.width = width;
        this.heigth = heigth;
    }

    setHeigth(heigth: number){
        this.heigth = heigth;
        this.width = width;
    }
}

const mySquare = new Square(20);
mySquare.setWidth(30);
mySquare.setWidth(40);

--------------------------------

interface Figure{
    setWidth(width: number): void;
    setHeigth(heigth: number): void;
    areaOf(): void;
}

class Rectangle implements Figure{
    setWidth(width: number){}
    setHeigth(heigth: number){}
    areaOf(){}
}

class Square implements Figure{
    setWidth(width: number){}
    setHeigth(heigth: number){}
    areaOf(){}
}

4. Interface Segregation.

interface AutoSet{
    getTeslaSet(): any;
    getAudiSet(): any;
    getBmwSet(): any;
}

class Tesla implements AutoSet{
    getTeslaSet(): any;
    getAudiSet(): any;
    getBmwSet(): any;
}

class Audi implements AutoSet{
    getTeslaSet(): any;
    getAudiSet(): any;
    getBmwSet(): any;
}

class Bmw implements AutoSet{
    getTeslaSet(): any;
    getAudiSet(): any;
    getBmwSet(): any;
}

interface TeslaSet{
    getTeslaSet(): any;
}

interface AudiSet{
    getAudiSet(): any;
}

interface BmwSet{
    getBmwSet(): any;
}

class Tesla implements TeslaSet{
    getTeslaSet(): any;
}

class Tesla implements TeslaSet{
    getAudiSet(): any;
}

class Tesla implements TeslaSet{
    getBmwSet(): any;
}
