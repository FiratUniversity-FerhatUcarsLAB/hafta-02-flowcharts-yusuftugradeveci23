digraph ATM {
    rankdir=TB; // Yukarıdan aşağıya
    node [fontname="Arial"];

    // Başla / Bitir
    BASLA [shape=oval, label="BAŞLA"];
    BITIR [shape=oval, label="BİTİR"];

    // PIN kontrol
    PIN_GIR [shape=parallelogram, label="PIN Gir"];
    PIN_DOGRU_MU [shape=diamond, label="PIN doğru mu?"];
    HAK_BITTI [shape=diamond, label="Hak bitti mi?"];

    // İşlem döngüsü
    BAKIYE_GOSTER [shape=box, label="Bakiyeyi göster"];
    TUTAR_GIR [shape=parallelogram, label="Çekmek istediğiniz tutarı gir (20 TL katları)"];
    TUTAR_KONTROL [shape=diamond, label="Tutar 20 TL katı mı?"];
    GUNLUK_LIMIT_KONTROL [shape=diamond, label="Günlük limit aşıldı mı?"];
    BAKIYE_KONTROL [shape=diamond, label="Bakiye yeterli mi?"];
    ISLEM_YAP [shape=box, label="Bakiye güncelle, işlem başarılı"];
    TEKRAR_ISLEM [shape=diamond, label="Başka işlem yapmak ister misiniz?"];

    // Mesajlar
    MESAJ_HATA [shape=box, label="Hata! Tekrar deneyin"];
    MESAJ_HAK [shape=box, label="Hesap bloke oldu"];
    MESAJ_TES_ [shape=box, label="Teşekkürler, iyi günler!"];

    // Bağlantılar
    BASLA -> PIN_GIR;
    PIN_GIR -> PIN_DOGRU_MU;
    PIN_DOGRU_MU -> BAKIYE_GOSTER [label="Evet"];
    PIN_DOGRU_MU -> HAK_BITTI [label="Hayır"];
    HAK_BITTI -> PIN_GIR [label="Hayır"];
    HAK_BITTI -> MESAJ_HAK [label="Evet"];
    MESAJ_HAK -> BITIR;

    // İşlem döngüsü
    BAKIYE_GOSTER -> TUTAR_GIR;
    TUTAR_GIR -> TUTAR_KONTROL;
    TUTAR_KONTROL -> MESAJ_HATA [label="Hayır"];
    MESAJ_HATA -> TUTAR_GIR;
    TUTAR_KONTROL -> GUNLUK_LIMIT_KONTROL [label="Evet"];
    GUNLUK_LIMIT_KONTROL -> MESAJ_HATA [label="Evet"];
    GUNLUK_LIMIT_KONTROL -> BAKIYE_KONTROL [label="Hayır"];
    BAKIYE_KONTROL -> MESAJ_HATA [label="Hayır"];
    BAKIYE_KONTROL -> ISLEM_YAP [label="Evet"];
    ISLEM_YAP -> TEKRAR_ISLEM;
    TEKRAR_ISLEM -> TUTAR_GIR [label="Evet"];
    TEKRAR_ISLEM -> MESAJ_TES_ [label="Hayır"];
    MESAJ_TES_ -> BITIR;
}

