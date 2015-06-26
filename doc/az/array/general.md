## Sıra iterasıyası və özəllikləri

Sıraların JavaScript-də obyekt olmaqlarına baxmayaraq, [`for in`](#object.forinloop)
istifadəsi üçün hər hansı bir səbəb yoxdur. Əslində sıralarda `for in` istifadəsinə
 **qarşı** yaxşı səbəb var.

> **Qeyd:** JavaScript sıraları *associative* **deyildirlər**. JavaScript-də sadəcə 
> [objects](#object.general) ilə açar-dəyər əlaqəsini yaratmaq olar. Və *associative*
> sıralar element sırasını **qoruyurlar** amma, obyektlər **qorumurlar**.

`for in` dövrü prototip zincirində olan bütün özəllikləri gəzdiyi üçün və bunu 
əngəlləməyin tək yolu [`hasOwnProperty`](#object.hasownproperty) istifadə etmək
olduğu üçün `for in` dövrü adi bir `for` dövründən **iyirmi dəfəyə** qədər gecdir.

### İterasiya

Sıralarda iterasiya edərkən ən yaxşı performansı əldə etmənin ən yaxşı yolu
klassik `for` dövründən istifadə etməkdir.

    var list = [1, 2, 3, 4, 5, ...... 100000000];
    for(var i = 0, l = list.length; i < l; i++) {
        console.log(list[i]);
    }

Yuxarıdakı nümunədə bir optimizasiya daha var, o da sıranın uzunluğunun 
`l = list.length` yolu ilə saxlanmasıdır.

Baxmayaraq ki, `length` özəlliyi sıranın özündə tanımlanmış olur, dövrünün hər
iterasiyasında onu oxumağın bir maliyyəsi vardır. Və bu halda yeni JavaScript 
mühərriklərinin bunu optimizasıya **edə bilərlərsə** də, kodun yeni mühərriklərdə
işləyəcəyinə zəmanət verilə bilməz.

In fact, leaving out the caching may result in the loop being only **half as
fast** as with the cached length.

### `length` Özəlliyi

`length` özəlliyini *getter* ilə sadəcə sırada olan elementlərin sayını qaytara
bildiyimiz halda, *setter* ilə isə sıranı **qısaltmaq** olar.

    var arr = [1, 2, 3, 4, 5, 6];
    arr.length = 3;
    arr; // [1, 2, 3]

    arr.length = 6;
    arr.push(4);
    arr; // [1, 2, 3, undefined, undefined, undefined, 4]

Daha az uzunluq verdikdə sıranı qısaldır, çoxaltdıqda isə seyrək sıra yaradırıq. 

### Xülasə

Ən yaxşı performanse üçün hər zaman `for` dövründən istifadə etmək və `length`
özəlliyini keşləmək tövsiyyə olunur. Sırada `for in` istifadəsi pis yazılmış
kodun göstəricisidir ki, bu da gələcəkdə baqlara və pis performansa səbəb ola 
bilər.

