## `Array` Konstruktoru

`Array` konstruktoru parametrləri necə istifadə etdiyi naməlum olduğu üçün
yeni sıra yaradılarkən hər zaman sıra sabitlərindən - `[]` notasiyasından 
istifadə olunması tövsiyyə olunur. 

    [1, 2, 3]; // Nəticə: [1, 2, 3]
    new Array(1, 2, 3); // Nəticə: [1, 2, 3]

    [3]; // Nəticə: [3]
    new Array(3); // Nəticə: []
    new Array('3') // Nəticə: ['3']


`Array` konstruktorunda yalnız bir arqument verildildə və bu arqumentin `Number`
olduğu halda konstruktor `length` özəlliyi arqumentin dəyərinə bərabər olan yeni bir
*boş* sıra qaytaracaq. Qeyd olunmalıdır ki, bu halda **yalnız** `length` özəlliyi 
təyin olunacaq. Aktual indekslər naməlum olacaqdır.

    var arr = new Array(3);
    arr[1]; // undefined
    1 in arr; // false, indeks təyin olunmayıb

Sıranın uzunluğunu əvvəlcədən təyin edə bilmək yalnız bir neçə hallarda
məqsədəuyğundur, dövr ehtiyac duymadan bir sözün təkrarlanması kimi.

    new Array(count + 1).join(stringToRepeat);

### Xülasə

Sıra konstruktoru üçün sabitlərin istifadəsi tövsiyyə olunur. Onlar qısa, anlaşıqlı olur və kodun
oxunurluluğunu artırır.

