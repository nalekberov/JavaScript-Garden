## `arguments` Obyekti

JavaScript-də hər bir funksiyanın əhatə dairəsi xüsusi `arguments` dəyişəninə 
müraciət edə bilər. Bu dəyişən funksiyaya ötürülmüş bütün arqumentlərin 
siyahısını özündə saxlayır.

> **Qeyd:** `arguments` funksiyanın əhatə dairəsində `var` vasitəsilə təyin 
> olunduğu və ya formal parametrin adı olduğu halda, `arguments` obyekti 
> yaradılmayacaq.

`arguments` obyekti `Array` **deyil**. Sıranın bəzi semantikasına - daha dəqiq
`length` xüsusiyyətinə - sahib olduğu halda, o bunu `Array.prototype`dan 
gətirmir və faktiki olaraq `Object`dir.

Bu səbəbdən `arguments` üzərində `push`, `pop` və ya `slice` kimi standart
sıra metodlarından  istifadə etmək mümkün **deyil**. Sadə `for` dövrü
yaxşı işlədiyi halda, onun üzərində standart `Array` metodlarından istifadə 
etmək üçün onu real `Array`a çevirmək zəruridir.

### Array-a çevirmək

Aşağıdakı kod `arguments` obyektinin bütün elementlərini daşıyan `Array`
qaytaracaq.

    Array.prototype.slice.call(arguments);

Bu çevirmə çox **yavaş** olduğuna görə, onu kodun performans yönündən həssas
olduğu hissələrində istifadə etmək **tövsiyyə olunmur**.

### Arqumentləri Ötürmək

Göstərilən nümunə bir funksiyadan digər funksiyaya arqument ötürülməsinin
tövsiyyə edilən yoludur.

    function foo() {
        bar.apply(null, arguments);
    }
    function bar(a, b, c) {
        // do stuff here
    }

Başqa bir hiylə isə `call` və `apply`dan birlikdə metodları - öz arqumentləri
ilə birlikdə, həmçinin `this`in dəyərini istifadə edən funksiyaları - ancaq 
arqumentlərini istifadə edən normal funksiyalara çevirməkdir.

    function Person(first, last) {
      this.first = first;
      this.last = last;
    }

    Person.prototype.fullname = function(joiner, options) {
      options = options || { order: "western" };
      var first = options.order === "western" ? this.first : this.last;
      var last =  options.order === "western" ? this.last  : this.first;
      return first + (joiner || " ") + last;
    };

    // "fullname"in bağlı olmayan versiyasını yaratmaq, 'first' və 'last'
    // xüsusiyyətləri birinci arqumentləri olaraq ötürülən istənilən obyektdə
    // yararlıdır. Bu örtük, fullname sayının dəyişdiyi və arqumentlərin sırasının
    // dəyişdiyi halda dəyişdirilməyə ehtiyac duymayacaq.
    Person.fullname = function() {
      // Nəticə: Person.prototype.fullname.call(this, joiner, ..., argN);
      return Function.call.apply(Person.prototype.fullname, arguments);
    };

    var grace = new Person("Grace", "Hopper");

    // 'Grace Hopper'
    grace.fullname();

    // 'Turing, Alan'
    Person.fullname({ first: "Alan", last: "Turing" }, ", ", { order: "eastern" });


### Formal Parametrlər və Arqumentlər İndeksləri

`arguments` obyekti *getter* və *setter* funksiyalarını həm xüsusiyyətləri,
həm də funksiyanın formal parametrləri üçün yaradır.

Nəticədə, formal parametrin dəyərini dəyişmək `arguments` obyektində ona uyğun
gələn xüsusiyyətin də dəyişilməsinə gətirib çıxarır, və ya əksinə.

    function foo(a, b, c) {
        arguments[0] = 2;
        a; // 2

        b = 4;
        arguments[1]; // 4

        var d = c;
        d = 9;
        c; // 3
    }
    foo(1, 2, 3);

### Performans Mifləri və Həqiqətləri

`arguments` obyekti yalnız, funksiyanın içində o adda nəsə elan olunduğu və ya
funksiyanın formal parametlərindən biri olduğu zaman yaradılmır. O istifadə
olunur və ya olunmur fərq etməz.

*getter* və *setter*lər hər ikisi **həmişə** yaradılır; ona görə də, onu istifadə
etməyin performansa demək olar ki, heç bir təsiri olmur, xüsusilə `arguments` 
obyektinin xüsusiyyətlərinə çox sadə bir müraciət olan real dünya kodlarında.

> **ES5 Qeydi:** Bu *getter* və *setter*lər sərt rejimdə yaradılmır.

Lakin, bir hal var ki, modern JavaScript mühərriklərində performansı ciddi
şəkildə aşağı salacaq. Bu hal `arguments.callee`ın istifadəsidir.

    function foo() {
        arguments.callee; // bu funksiya obyekti ilə nə isə et
        arguments.callee.caller; // və çağıran funksiya obyekti
    }

    function bigLoop() {
        for(var i = 0; i < 100000; i++) {
            foo(); // Normal inlayn edilə bilərdi...
        }
    }

Yuxarıdakı kodda `foo` [inlayninqə][1] tabe ola bilməz, çünki onun həm özü,
həm də çağıranı barədə bilməyə ehtiyacı vardır. Bu təkcə inlayninqdən əldə
oluna biləcək mümkün performans qazancının qarşısını almır, o həm də
enkaspsulyasiyanı qırır, çünki funksiya indi spesifik çağırılma kontekstindən
asılı ola bilər.

`arguments.callee` və ya onun hər hansı bir xüsusisyyətini istifadə etmək
**şiddətlə tövsiyyə edilmir**.

> **ES5 Qeydi:** Sərt rejimdə istifadədən yığışdırıldığı üçün `arguments.callee` 
> bir `TypeError` atacaq.

[1]: http://en.wikipedia.org/wiki/Inlining


