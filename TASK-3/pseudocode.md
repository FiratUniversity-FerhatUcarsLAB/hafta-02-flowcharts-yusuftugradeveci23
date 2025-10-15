Fonksiyon AnaMenu()
    Dongu
        Secim = MenuGoster("1 - Randevu Al", "2 - Tahlil Sonucu Gör")

        Eger Secim = 1 ise
            HastaKimlik = KimlikDogrula(TCNo)
            Eger HastaKimlik = Dogru ise
                PoliklinikSecimi = PoliklinikListesiGoster()
                DoktorSecimi = DoktorListesiGoster(PoliklinikSecimi)
                UygunSaatler = UygunSaatleriGoster(DoktorSecimi)
                SecilenSaat = KullaniciSecimi(UygunSaatler)
                RandevuOnayi = RandevuOnayla(SecilenSaat)
                Eger RandevuOnayi = Basarili ise
                    SMSSend(HastaKimlik, SecilenSaat)
                Aksi halde
                    Mesaj("Randevu onayı başarısız")
            Aksi halde
                HataMesaji("Kimlik doğrulama başarısız")

        Eger Secim = 2 ise
            HastaKimlik = KimlikDogrula(TCNo)
            Eger HastaKimlik = Dogru ise
                Eger TahlilVarMi(HastaKimlik) = True ise
                    Eger SonucHazirMi(HastaKimlik) = True ise
                        SonucGoruntule(HastaKimlik)
                        PDFIndir(HastaKimlik)
                    Aksi halde
                        Mesaj("Sonuç henüz hazır değil")
                Aksi halde
                    Mesaj("Tahlil bulunamadı")
            Aksi halde
                HataMesaji("Kimlik doğrulama başarısız")

        BitirMi = KullaniciSor("Başka işlem yapmak ister misiniz? (E/H)")
        Eger BitirMi = "H" ise
            DonguyuBitir
