## Konstruktorlar

Javascript-də konstruktorlar da başqa dillərdən fərqlidir. İstənilən önündə `new`
ilə gələn funksiya çağırışı konstruktor olaraq çıxış edir.

Konstruktorların - çağırılan funksiyakların - içində `this`in dəyəri yeni yaranmış
obyektə müraciət edir. Bu **yeni** obyektin [prototipi](#object.prototype) konstruktor
olaraq çağırılan funksiyanın `prototype`inə təyin olunur.

Əgər çağırılan funksiyanın aşkar `return` ifadəsi yoxdursa, onda o o qeyri-aşkar
şəkildə `this`in - yeni obyektin - dəyərini qaytarır.

    function Person(name) {
        this.name = name;
    }

    Person.prototype.logName = function() {
        console.log(this.name);
    };

    var sean = new Person();

Yuxarıdakı `Person`u konstruktor olaraq çağırır və yeni yaranmış obyektin
`prototype`ini `Person.prototype` olaraq təyin edir.

Aşkar `return` ifadəsi halında, funksiya bu ifadə tərəfindən göstərilən
dəyəri qaytarır, amma bir şərt ilə ki, qaytarılan dəyər **yalnız** bir 
`Object`dir.

    function Car() {
        return 'ford';
    }
    new Car(); // yeni obyekt, 'ford' deyil

    function Person() {
        this.someValue = 2;

        return {
            name: 'Charles'
        };
    }
    new Person(); // qaytarılab obyekt ({name:'Charles'}), someValue olmamaq şərtilə

`new` açarsözü ötürüldüyü zaman, funksiya yeni bir obyekt **qaytarmayacaq**.

    function Pirate() {
        this.hasEyePatch = true; // gets set on the global object!
    }
    var somePirate = Pirate(); // somePirate is undefined

Yuxarıdakı nümunə bəzi hallarda işlədiyi halda, JavaScript-də 
[`this`](#function.this)in işləmə qaydalarına əsasən, o
*global object*i `this` olaraq istifadə edəcək.

### Factorilər

`new` açarsözünü ötürməkdən ötrü, konstruktor funksiyası aşkar şəkildə bir dəyər
qaytarmalıdır.

    function Robot() {
        var color = 'gray';
        return {
            getColor: function() {
                return color;
            }
        }
    }

    new Robot();
    Robot();

`Robot`a olan hər iki çağırış eyni şeyi  - [Closure](#function.closures) 
kimi çıxış edən `getColor` xüsusiyyəti olan yeni yaranmış obyekti - qaytarır.

Onu da qeyd etmək lazımdır ki, `new Robot()` çağırışı qaytarılan obyektin
prototipinə təsir **etmir**. Prototip yeni yaradılmış obyektə təyin olunduğu
halda, `Robot` heç vaxt bu yeni obyekti qaytarmır.

Yuxarıdakı misalda, `new` açarsözünün istifadə etməklə etməmək arasında
funksional fərq yoxdur.

### Factorilər ilə yeni obketlərin yaradılması

`new`nun istifadəsini çox vaxt istifadə **etməmək** tövsiyyə olunur, çünki
onun istifadəsini unutmaq baqlara gətirib çıxara bilər.

Yeni obyekti yaratmaqdan ötrü, daha yaxşısı factory-lərdən istifadə olunmalıdır,
və bu factory-lərin içində yeni obyekt düzəldilməlidir.

    function CarFactory() {
        var car = {};
        car.owner = 'nobody';

        var milesPerGallon = 2;

        car.setOwner = function(newOwner) {
            this.owner = newOwner;
        }

        car.getMPG = function() {
            return milesPerGallon;
        }

        return car;
    }

Yuxarıdakı əskik `new` açarsözünə qarşı möhkəm olsa, və mütləq şəkildə
[private variables](#function.closures) istifadəsini daha rahat etsə də,
bir sıra mənfi xüsusiyyətlərlə gəlir.

 1. O daha çox yaddaş istifa edir, çünki yeni yaradılmış obyektlər metodları 
    prototipdə **paylaşmır**.
 2. Varisliyi etmək üçün factory bir obyektin bütün metodlarını kopyalamalı və
    onları yeni obyektin prototipdində yerləşdirməlidir.
 3. Əskik `new` açarsözünə görə prototip zincirini atmaq dilin ruhuna ziddir.

### Xülasə

`new` açarsözünü ötürmək baqlara gətirib çıxardığı halda, o prototiplərin 
istifadəsini tamamilə atmaq üçün əlbəttə səbəb **deyil**. Nəticədə verilmiş
tətbiqin ehtiyacları üçün hansı həllin daha uyğun gəldiyidir. Obyekt
yaradılması üçün spesifik üslubunun seçilməsi və onun **düzgün** istifadəsi
isə xüsusilı önəm daşıyır.

