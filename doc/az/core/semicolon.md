## Avtomatik nöqtəvergül yerləşdirməsi

Baxmayaraq ki, JavaScript C üslubu sintaksisə sahibdir, o nöqtəvergül
istifadəsini məcbur **etmir**, ona görə onları ötürmək mümkündür.

JavaScript nöqtəvergülsüz dil deyil. Əslində, o qaynaq kodunu anlamaq
üçün nöqtəvergüllərə ehtiyac duyur. Ona görə də, JavaScript təhliledicisi 
çatışmayan nöqtəvergül səbəbilə yaranan təhlil xətası ilə üzləşdiyi zaman 
**avtomatik olaraq** nöqtəvergülləri yerləşdirir.

    var foo = function() {
    } // təhlil xətası, nöqtəvergül gözlənilirdi
    test()

Yerləşdirmə baş verir, və təhliledici yenidən cəhd edir.

    var foo = function() {
    }; // xəta yoxdur, təhliledici davam edir
    test()

Avtomatik nöqtəvergül yerləşdirməsi dilin **ən böyük** boşluqlarından biri
hesab olunur, çünki kodun davranışını dəyişə *bilər*.

### Necə işləyir

Aşağıdakı kodda nöqtəvergül istifadə olunmayıb, ona görə onları yerləşdirmək
artıq təhliledicinin verəcəyi qərardır.

    (function(window, undefined) {
        function test(options) {
            log('testing!')

            (options.list || []).forEach(function(i) {

            })

            options.value.test(
                'long string to pass here',
                'and another long string to pass'
            )

            return
            {
                foo: function() {}
            }
        }
        window.test = test

    })(window)

    (function(window) {
        window.someLibrary = {}

    })(window)

Aşağıda təhliledicinin "təxminetmə" oyununun nəticəsidir.

    (function(window, undefined) {
        function test(options) {

            // yerləşdirilmədi, sətirlər birləşdirildi
            log('testing!')(options.list || []).forEach(function(i) {

            }); // <- yerləşdirildi

            options.value.test(
                'long string to pass here',
                'and another long string to pass'
            ); // <- yerləşdirildi

            return; // <- yerləşdirildi, return ifadəsini qırır
            { // block olaraq qəbul edildi

                // etiket (label) və ifadə elanı
                foo: function() {} 
            }; // <- yerləşdirildi
        }
        window.test = test; // <- yerləşdirildi

    // sətirlər yenidən birləşdirildi
    })(window)(function(window) {
        window.someLibrary = {}; // <- yerləşdirildi

    })(window); //<- yerləşdirildi

> **Qeyd:** Javascript təhliledicisinin özündən sonra yeni sətir gələn return 
> ifadələrini "düzgün" ələ ala bilmir. Bu tam olaraq avtomatik nöqtəvergül 
> yerləşdirilməsinin xətası olmasa da yenə də istənməyən bir yan-təsirdir.

Təhliledici yuxarıdakı kodun davranışını ciddi şəkildə dəyişdi. Müəyyən hallarda bu
**yanlış** nəticələr gətirir.

### Öndəgələn mötərizə

Mötərizənin öndə gələməsi halında təhliledici nöqtəvergül **yerləşdirməyəcək**.

    log('testing!')
    (options.list || []).forEach(function(i) {})

Bu kod bir sətrə çevriləcək.

    log('testing!')(options.list || []).forEach(function(i) {})

Ehtimallar **çox** yüksəkdir ki, `log` funksiya **qaytarmayacaq**; ona görə də,
yuxarıdakı `undefined is not a function` ifadə edən `TypeError`a gətirib çıxaraq.

### Xülasə

Nöqtəvergülləri **heç vaxt** ötürməmək şiddətlə tövsiyyə olunur. Həmçinin fiqurlu
mötərizələrin onların aid olduğu ifadələr ilə bir sətirdə saxlamaq və heç vaxt
bir-sətirli `if` / `else` ifadələrində onları ötürməmək tövsiyyə olunur. Bu ölçülər
təkcə kodununuz düzgün olmasını təmin etməyəcək, həm də JavaScript təhliledicisinin
kodun davranışını dəyişməsinin qarşısını alacaq.

