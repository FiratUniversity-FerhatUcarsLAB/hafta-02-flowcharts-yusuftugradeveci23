BAŞLA
----------------------------------------
ANA MENÜ:
    1 - Randevu Al
    2 - Tahlil Sonucu Gör
SEÇİM <- kullanıcıdan seçim al

----------------------------------------
MODÜL 1: RANDEVU AL
EĞER SEÇİM = 1 İSE:
    KimlikDoğrula()
    EĞER KimlikDoğru İSE:
        PoliklinikSeç()
        DoktorListesiGoster()
        UygunSaatleriGoster()
        RandevuSec()
        RandevuOnayla()
        SMSGonder()
    DEĞİLSE:
        HataMesajiGoster()
        TekrarDeneme()

----------------------------------------
MODÜL 2: TAHLİL SONUÇLARI
EĞER SEÇİM = 2 İSE:
    KimlikDoğrula()
    EĞER KimlikDoğru İSE:
        EĞER TahlilVar Mı? İSE:
            EĞER SonucHazır Mı? İSE:
                SonucuGoruntule()
                PDFIndirSeceneği()
            DEĞİLSE:
                BeklemeMesajiGoster()
        DEĞİLSE:
            TahlilYokMesajiGoster()
    DEĞİLSE:
        HataMesajiGoster()
        TekrarDeneme()

----------------------------------------
BAŞKA İŞLEM YAPMAK İSTER MİSİN? (döngü)
EĞER Evet İSE:
    Ana Menüye dön
DEĞİLSE:
    BITIR
