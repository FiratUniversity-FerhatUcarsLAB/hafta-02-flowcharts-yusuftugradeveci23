// Akıllı Ev Güvenlik Sistemi - Alarm & Bildirim Süreçleri
// Sürekli çalışan ana döngü (7/24)

BAŞLA
InitializeSystem()
WHILE SistemAktifMi() DO          // Ana DÖNGÜ - sürekli çalışır
    timestamp = Now()

    // 1) Tüm sensörleri oku
    hareketVerileri = ReadMotionSensors()           // hareket sensörleri
    kapipencereVerileri = ReadDoorWindowSensors()  // kapı/pencere sensörleri
    kameraDurumu = ReadCameraStatus()               // kamera bağlantı / durum
    extraSensörler = ReadOtherSensors()            // isteğe bağlı (cam kırılma, duman, vs.)

    // 2) Tehdit tespiti ve değerlendirme
    threatScore = EvaluateThreat(hareketVerileri, kapipencereVerileri, kameraDurumu, extraSensörler)

    // 3) Yanlış alarm kontrolü (ör. ev sahibi evde mi?)
    if IsOwnerHome() THEN
        falseAlarmRisk = High
    else
        falseAlarmRisk = Low
    end if

    // 4) Alarm seviyesi belirle (1-düşük,2-orta,3-yüksek)
    alarmLevel = DetermineAlarmLevel(threatScore, falseAlarmRisk)

    // 5) Alarmlar / aksiyonlar
    if alarmLevel == 0 THEN
        // Normal: hiçbir işlem (günlüğe kaydet)
        LogEvent("No threat", timestamp)
    else
        ActivateLocalAlarm(alarmLevel)        // siren / ışık vs.
        if ShouldActivateCamera(alarmLevel, kameraDurumu) THEN
            ActivateCameraRecording()
        end if

        // 6) Bildirim gönderme (SMS + App + Email)
        SendNotifications(alarmLevel, threatScore, timestamp)

        // 7) Alarm devam / sıfırlama kontrolü
        WHILE AlarmNotReset() DO
            // Sürekli alarm halinde yeniden değerlendirme (ör. her 10s)
            Sleep(AlarmCheckInterval)
            // Opsiyonel: yeni sensör okumaları ile seviyesi güncelle
            threatScore = EvaluateThreat(ReadMotionSensors(), ReadDoorWindowSensors(), ReadCameraStatus(), ReadOtherSensors())
            newAlarmLevel = DetermineAlarmLevel(threatScore, falseAlarmRisk)
            if newAlarmLevel > alarmLevel THEN
                alarmLevel = newAlarmLevel
                AdjustAlarmIntensity(alarmLevel)
                SendNotifications(alarmLevel, threatScore, Now())
            end if
            // Eğer ev sahibi gelmişse veya manuel sıfırlanmışsa döngüden çık
            if IsOwnerHome() THEN
                // Yanlış alarm ihtimali yüksek; tercihen bekle veya uyarı gönder
                LogEvent("Owner arrived - possible false alarm", Now())
            end if
        END WHILE

        // Alarm sıfırlandıysa kapat
        DeactivateLocalAlarm()
        StopCameraRecordingIfActive()
        LogEvent("Alarm reset", Now())
    end if

    // 8) Döngü bekleme / sonraki iterasyon (sürekli)
    Sleep(MainLoopInterval)
END WHILE
BİTİR

// --- Yardımcı Fonksiyonlar (özet) ---
FUNCTION InitializeSystem()
    // Ağ, sensör ve bildirim servislerini başlat / doğrula
END FUNCTION

FUNCTION SistemAktifMi() RETURNS BOOL
    // Sistem açık/kapalı durumunu kontrol et (örn. kullanıcı paneli)
END FUNCTION

FUNCTION EvaluateThreat(motion, doorwin, cam, extra) RETURNS INTEGER
    // Sensör verilerini scoring ile birleştir
    // Ör: hareket + açık kapı = yüksek puan; tek hareket düşük puan
    // Döner: threatScore (0 = yok, 1..10 arası)
END FUNCTION

FUNCTION DetermineAlarmLevel(score, falseAlarmRisk) RETURNS INT
    // Score ve falseAlarmRisk'e göre 0..3 arası seviye döndür
    // Ör: score < 3 -> 0, 3-5 -> 1, 6-7 -> 2, >=8 -> 3; falseAlarmRisk artırır/azaltır
END FUNCTION

FUNCTION SendNotifications(level, score, timestamp)
    // SMS gönder
    // Push notification uygulamaya gönder
    // E-posta gönder
    // Bildirim içeriğinde: seviye, kısa açıklama, zaman damgası, kamera linki (varsa)
    // Log yaz
END FUNCTION

FUNCTION AlarmNotReset() RETURNS BOOL
    // Kullanıcıdan manuel sıfırlama gelmedi mi? (Panel/APP/Şifre)
    // Veya otomatik sıfırlama kuralları (ör. doğrulama gelene kadar devam et)
END FUNCTION
