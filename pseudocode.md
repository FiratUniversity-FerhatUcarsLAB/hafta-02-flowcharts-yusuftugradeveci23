BAŞLA

// Kullanıcıdan PIN'i oku
HAK = 3
DO
    YAZ "Lütfen PIN'inizi girin:"
    OKU PIN_GIRILEN
    EĞER PIN_GIRILEN = PIN_DOĞRU İSE
        YAZ "PIN doğrulandı."
        BREAK
    DEĞİLSE
        HAK = HAK - 1
        YAZ "Hatalı PIN. Kalan hak: ", HAK
        EĞER HAK = 0 İSE
            YAZ "Hesap bloke oldu."
            BITIR
        FİN
    FİN
LOOP

// İşlem menüsü
TEKRAR = "EVET"
WHILE TEKRAR = "EVET"
    YAZ "Mevcut bakiyeniz: ", BAKİYE

    // Para çekme miktarını sor
    YAZ "Çekmek istediğiniz tutarı girin (20 TL katları):"
    OKU TUTAR

    // Tutar kontrolü
    EĞER TUTAR MOD 20 ≠ 0 İSE
        YAZ "Tutar 20 TL katları olmalıdır."
        DEVAM
    FİN

    // Günlük limit kontrolü
    EĞER (TUTAR + GUNLUK_TOPLAM) > GUNLUK_LIMIT İSE
        YAZ "Günlük limit aşıldı."
        DEVAM
    FİN

    // Bakiye kontrolü
    EĞER TUTAR > BAKİYE İSE
        YAZ "Yetersiz bakiye."
        DEVAM
    FİN

    // İşlemi gerçekleştirme
    BAKİYE = BAKİYE - TUTAR
    GUNLUK_TOPLAM = GUNLUK_TOPLAM + TUTAR
    YAZ "İşlem başarılı. Kalan bakiyeniz: ", BAKİYE

    // Tekrar işlem yapmak ister mi
    YAZ "Başka işlem yapmak istiyor musunuz? (EVET/HAYIR):"
    OKU TEKRAR
WEND

YAZ "Teşekkürler, iyi günler!"
BITIR
