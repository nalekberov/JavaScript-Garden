## Nə üçün `eval` istifadə etməməli

`eval` funksiyası JavaScript kodunu lokal skopda icra edir.

    var number = 1;
    function test() {
        var number = 2;
        eval('number = 3');
        return number;
    }
    test(); // 3
    number; // 1

Amma, `eval` lokal skopda o zaman icra olunur ki, o birbaşa çağırılır *və*
çağırılan funksiyanın adı `eval`dır.

    var number = 1;
    function test() {
        var number = 2;
        var copyOfEval = eval;
        copyOfEval('number = 3');
        return number;
    }
    test(); // 2
    number; // 3

`eval`ın istifadəsindən qaçınılmalıdır. Onun istifadəsinin 99.9%-ni **onsuz**
da həyata keçirmək olar.
    
### `eval` Qılıqda

[Taymaut funksiyaları](#other.timeouts) `setTimeout` və `setInterval` hər ikisi
birinci arqumenti string olaraq götürə bilərlər. Bu halda `eval` birbaşa
çağırılmadığından, Bu string **həmişə** global skopda icra olunur.

### Təhklüksəzilik məsələləri

`eval` həm də təhlükəsizlik problemidir, çünki o, ona ötürülən **istənilən** kodu
icra edir. O **heç vaxt** mənbəyi naməlum və ya güvənilməyən yerdən olan kod üçün
istifadə olunmalı deyil.

### Xülasə

`eval` heç vaxt istifadə olunmalı deyil. Onu istifadə edən istənilən kod parçasının
istifadə məqsədi, performansı və təhlükəsizliyi sual edilməlidir. Əgər hər hansı bir
şey işləməsi üçün `eval` tələb edirsə , birincisi o istifadə edilməli **deyil**. `eval`
istifadəsinə ehtiyaca duymayan *daha yaxşı dizayn* tətbiq edilməlidir.

