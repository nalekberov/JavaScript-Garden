## `delete` Operatoru

Qısa olaraq, global dəyişənləri, funksiyaları və bəzi digər `DontDelete` atributuna
sahib olan JavaScript nəsnələrini silmək *mümkünsüzdür*

### Global kod və Funksiya kodu

Dəyişən və ya funksiya, global və ya [funksiya skopunda](#function.scopes) 
tanıdıldığı zaman, o ya Activation object ya da Global objectin xüsusiyyəti olur.
Bu cür xüsusiyyətlərin müəyyən atributları olur, onlardan biri də `DontDelete`
atributudur. Global və ya funksiya kodunun dəyişən və funksiyaları hər zaman
,özəllikləri `DontDelete` ilə yaradır, ona görə də silinə bilməzlər.

    // global dəyişən:
    var a = 1; // DontDelete təyin olunub
    delete a; // false
    a; // 1

    // normal funksiya:
    function f() {} // DontDelete təyin olunub
    delete f; // false
    typeof f; // "function"

    // yenidən təyin etmək kömək etmir:
    f = 1;
    delete f; // false
    f; // 1

### Aşkar xüsusiyyələr

Aşkar şəkildə təyin olunmuş özəlliklər normalda silinə bilir.

    // Aşkar təyin olunmuş özəllik:
    var obj = {x: 1};
    obj.y = 2;
    delete obj.x; // true
    delete obj.y; // true
    obj.x; // undefined
    obj.y; // undefined

Yuxarıdakı misalda `obj.x` və `obj.y` silinə bilərlər, çünki `DontDelete` atributları
yoxdur. Buna görə aşağıdakı misal da işləyir.

    // bu yaxşı işləyir, IE xaric:
    var GLOBAL_OBJECT = this;
    GLOBAL_OBJECT.a = 1;
    a === GLOBAL_OBJECT.a; // true - sadəcə bir global dəyişən
    delete GLOBAL_OBJECT.a; // true
    GLOBAL_OBJECT.a; // undefined

Burda biz `a` dəyişənini silmək üçün hiylədən istifadə edirik. [`this`](#function.this)
burada Global object-ə müraciət edir və biz aşkar şəkildə `a` dəyişənini onun xüsusiyyəti
kimi elan edirik, hansı ki, bu da bizə onu silməyə imkan yaradır.

IE (ən azından 6-8) müəyyən buglara sahibdir, ona görə yuxarıdakı kod işləmir.


### Funksiya arqumentləri və hazır gələnlər

Funksiyaların normal arqumentləri [`arguments` objects](#function.arguments)
və hazır gələn xüsusiyyətlər də həmçinin `DontDelete` atributu ilə gəlir.

    // funksiya arqumentləri və xüsusiyyətlər:
    (function (x) {
    
      delete arguments; // false
      typeof arguments; // "object"
      
      delete x; // false
      x; // 1
      
      function f(){}
      delete f.length; // false
      typeof f.length; // "number"
      
    })(1);

### Host obyektlər

`delete` operatorunun davranışı host olunmuş obyektlər üçün gözlənilməzdir.
Spesifikasiyaya görə, host obyektlər istənilən cür davranışı tətbiq edə bilərlər.


### Xülasə

`delete` operatoru bəzən gözlənilməyən davranışa sahib olur və ancaq normal obyektlərdə
aşkar şəkildə təyin olunmuş xüsusiyyətlərin silinməsi üçün sağlam şəkildə istifadə
oluna bilər.
