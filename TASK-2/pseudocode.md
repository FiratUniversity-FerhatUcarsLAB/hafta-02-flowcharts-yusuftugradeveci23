ALGORİTMA: E-TİCARET SİTESİ SEPET YÖNETİMİ VE ÖDEME SİSTEMİ

BAŞLA

    // 1. KULLANICI GİRİŞİ
    KULLANICI_GİRİŞ_YAP()
    EĞER giriş_başarılı DEĞİLSE
        MESAJ_GÖSTER("Giriş başarısız! Lütfen tekrar deneyin.")
        SİSTEMİ_DURDUR()
    SON

    // 2. ÜRÜN SEÇİMİ VE SEPETE EKLEME
    SEPET ← BOŞ
    TEKRARLA
        ÜRÜN_GÖSTER_KATEGORİLER()
        SEÇİLEN_ÜRÜN ← KULLANICI_SEÇİMİ()

        // 3. STOK KONTROLÜ
        EĞER STOK( SEÇİLEN_ÜRÜN ) > 0 İSE
            ADET ← KULLANICI_GİRDİSİ("Kaç adet eklemek istiyorsunuz?")
            EĞER ADET ≤ STOK( SEÇİLEN_ÜRÜN ) İSE
                SEPETE_EKLE(SEÇİLEN_ÜRÜN, ADET)
                MESAJ_GÖSTER("Ürün sepete eklendi.")
            DEĞİLSE
                MESAJ_GÖSTER("Yetersiz stok! Maksimum: " + STOK(SEÇİLEN_ÜRÜN))
            SON
        DEĞİLSE
            MESAJ_GÖSTER("Bu ürün stokta yok.")
        SON

        DEVAM_ET ← KULLANICI_GİRDİSİ("Başka ürün eklemek ister misiniz? (E/H)")
    SÜRECEK DEVAM_ET = "E" OLDUĞU SÜRECE

    // 4. SEPET ÖZETİ
    SEPETİ_GÖSTER()
    TOPLAM_TUTAR ← SEPET_TOPLAMI()

    // 5. İNDİRİM KODU UYGULAMA
    KOD ← KULLANICI_GİRDİSİ("İndirim kodunuz varsa girin (yoksa boş bırakın):")
    EĞER KOD_DOĞRU(KOD) İSE
        İNDİRİM_ORANI ← KODDAN_ORAN_AL(KOD)
        İNDİRİM_TUTARI ← TOPLAM_TUTAR * İNDİRİM_ORANI
        TOPLAM_TUTAR ← TOPLAM_TUTAR - İNDİRİM_TUTARI
        MESAJ_GÖSTER("İndirim uygulandı. Yeni toplam: " + TOPLAM_TUTAR)
    DEĞİLSE
        MESAJ_GÖSTER("Geçersiz indirim kodu.")
    SON

    // 6. KARGO HESAPLAMA
    KARGO_TÜRÜ ← KULLANICI_GİRDİSİ("Kargo türü seçin: Standart / Hızlı")
    EĞER KARGO_TÜRÜ = "Hızlı" İSE
        KARGO_ÜCRETİ ← 50
    DEĞİLSE
        KARGO_ÜCRETİ ← 25
    SON
    TOPLAM_TUTAR ← TOPLAM_TUTAR + KARGO_ÜCRETİ
    MESAJ_GÖSTER("Kargo ücreti eklendi. Yeni toplam: " + TOPLAM_TUTAR)

    // 7. ÖDEME AŞAMASI
    ÖDEME_YÖNTEMİ ← KULLANICI_GİRDİSİ("Ödeme yöntemi seçin: KrediKartı / Havale / KapıdaÖdeme")

    SEÇİM:
        DURUM ÖDEME_YÖNTEMİ = "KrediKartı":
            KART_BİLGİLERİ ← KULLANICI_GİR()
            EĞER KART_ONAY(KART_BİLGİLERİ) İSE
                MESAJ_GÖSTER("Ödeme başarılı!")
            DEĞİLSE
                MESAJ_GÖSTER("Ödeme reddedildi.")
                SİSTEMİ_DURDUR()
            SON

        DURUM ÖDEME_YÖNTEMİ = "Havale":
            MESAJ_GÖSTER("Lütfen verilen IBAN'a ödeme yapın.")
        
        DURUM ÖDEME_YÖNTEMİ = "KapıdaÖdeme":
            MESAJ_GÖSTER("Kapıda ödeme seçildi.")

    // 8. SİPARİŞ ONAYI
    SİPARİŞ_OLUŞTUR(SEPET, TOPLAM_TUTAR)
    MESAJ_GÖSTER("Siparişiniz başarıyla oluşturuldu!")
    MESAJ_GÖSTER("Sipariş toplamı: " + TOPLAM_TUTAR)

BİTİR
