import psycopg2

# Bilgiler
db_bilgileri = {
    "host": "localhost",
    "database": "Kitap_Sistemi",
    "user": "postgres",
    "password": "1234", 
    "port": "5432"
}

# 1. VERİ EKLEME 
def kitap_ekle():
    ad = input("Kitap Adı: ")
    yazar = input("Yazarı: ")
    sayfa = input("Sayfa Sayısı: ") 
    
    try:
        baglanti = psycopg2.connect(**db_bilgileri)
        imlec = baglanti.cursor()
        sorgu = "INSERT INTO kitaplar (kitap_adi, yazar, sayfa_sayisi) VALUES (%s, %s, %s)"
        imlec.execute(sorgu, (ad, yazar, int(sayfa)))
        baglanti.commit()
        print("Kayıt başarıyla eklendi!")
    except Exception as hata:
        print("Bir hata oluştu:", hata)
    finally:
        baglanti.close()

# 2. VERİ LİSTELEME
def kitap_listele():
    try:
        baglanti = psycopg2.connect(**db_bilgileri)
        imlec = baglanti.cursor()
        imlec.execute("SELECT * FROM kitaplar")
        tum_kitaplar = imlec.fetchall()
        
        print(" MEVCUT KİTAPLAR ")
        for kitap in tum_kitaplar:
            print("ID:", kitap[0], "| Ad:", kitap[1], "| Yazar:", kitap[2], "| Sayfa:", kitap[3])
    except Exception as hata:
        print("Listeleme hatası:", hata)
    finally:
        baglanti.close()

# 3. VERİ GÜNCELLEME
def kitap_guncelle():
    kitap_listele() 
    secilen_id = input("\nGüncellenecek kitabın ID numarasını yaz: ")
    yeni_isim = input("Yeni Kitap Adı: ")
    
    try:
        baglanti = psycopg2.connect(**db_bilgileri)
        imlec = baglanti.cursor()
        sorgu = "UPDATE kitaplar SET kitap_adi = %s WHERE id = %s"
        imlec.execute(sorgu, (yeni_isim, secilen_id))
        baglanti.commit()
        print("Kayıt başarıyla güncellendi!")
    except Exception as hata:
        print("Güncelleme hatası:", hata)
    finally:
        baglanti.close()

# 4. VERİ SİLME 
def kitap_sil():
    kitap_listele()
    silinecek_id = input("\nSilmek istediğin kitabın ID numarasını yaz: ")
    
    try:
        baglanti = psycopg2.connect(**db_bilgileri)
        imlec = baglanti.cursor()
        imlec.execute("DELETE FROM kitaplar WHERE id = %s", (silinecek_id,))
        baglanti.commit()
        print("Kayıt başarıyla silindi!")
    except Exception as hata:
        print("Silme hatası:", hata)
    finally:
        baglanti.close()

# MENÜ
while True:
    print("KİTAP KAYIT PANELİ ")
    print("1- Kitap Ekle")
    print("2- Kitapları Gör")
    print("3- Kitap İsmi Güncelle")
    print("4- Kitap Sil")
    print("5- Çıkış")
    
    secim = input("Ne yapmak istersin?: ")
    
    if secim == "1":
        kitap_ekle()
    elif secim == "2":
        kitap_listele()
    elif secim == "3":
        kitap_guncelle()
    elif secim == "4":
        kitap_sil()
    elif secim == "5":
        print("Güle güle!")
        break
    else:
        print("Lütfen 1 ile 5 arasında bir sayı seç!")