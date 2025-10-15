BAŞLA

GİRİŞ: ogrenciNo, sifre
EĞER ogrenciNo ve sifre doğru ise
    DERS_LISTESI ← sistemdeki tüm dersler
    TOPLAM_KREDI ← 0

    DÖNGÜ: Her ders için DERS_LISTESI üzerinde
        DERS_BILGISI_GOSTER()

        EĞER kullanıcı dersi seçmek isterse
            EĞER kontenjan dolu değilse
                EĞER onkosul dersi tamamlanmışsa
                    EĞER zaman çakışması yoksa
                        EĞER (TOPLAM_KREDI + DERS_KREDISI) ≤ 35 ise
                            DERSI_EKLE()
                            TOPLAM_KREDI ← TOPLAM_KREDI + DERS_KREDISI
                        DEĞİLSE
                            YAZ "Kredi limiti aşıldı!"
                        SON
                    DEĞİLSE
                        YAZ "Zaman çakışması var!"
                    SON
                DEĞİLSE
                    YAZ "Önkoşul dersi alınmamış!"
                SON
            DEĞİLSE
                YAZ "Kontenjan dolu!"
            SON
        SON
    SON DÖNGÜ

    DÖNGÜ: Kullanıcı ders çıkarmak isterse
        DERSI_SIL()
        TOPLAM_KREDI ← TOPLAM_KREDI - DERS_KREDISI
    SON DÖNGÜ

    EĞER GPA < 2.5 ise
        YAZ "Danışman onayı gerekiyor."
    DEĞİLSE
        YAZ "Kayıt başarıyla tamamlandı."
    SON
DEĞİLSE
    YAZ "Giriş bilgileri hatalı!"
SON

BİTİR
