import requests
from bs4 import BeautifulSoup
from datetime import datetime, timedelta
import pytz
from flask import Flask, render_template

app = Flask(__name__)

# İstanbul'un saat dilimi bilgisi
timezone = pytz.timezone('Europe/Istanbul')

# Web sayfasından veriyi çekme
url = 'https://www.sabah.com.tr/istanbul-namaz-vakitleri'
r = requests.get(url)
soup = BeautifulSoup(r.content, 'html.parser')

# Akşam namazı vaktini bulma
vakitler_div = soup.find('div', class_='vakitler boxShadowSet')
if vakitler_div is not None:
    vakitler = vakitler_div.find_all('span')
    aksam = vakitler[4].text

    # Tarih ve saat hesaplama
    now = datetime.now(timezone)
    aksam_saat = datetime.strptime(aksam, "%H:%M")
    aksam_tarihi = datetime.now(timezone).date() if aksam_saat.time() > now.time() else datetime.now(timezone).date() + timedelta(days=1)
    aksam_zamani = timezone.localize(datetime.combine(aksam_tarihi, aksam_saat.time()))
    kalan_sure = aksam_zamani - now

    if kalan_sure.total_seconds() > 0:
        kalan_dakika = int((kalan_sure.total_seconds() // 60) % 60)
        kalan_saat = int(kalan_sure.total_seconds() // 3600)
        print(f"İftara kalan süre: {kalan_saat} saat {kalan_dakika} dakika\n")
        print(f"İstanbul'da önümüz akşam namazı vakti: {aksam}\n")
    else:
        print("Bugünkü iftar zamanı geçti. Yarınki iftar için bekleyin.\n")
else:
    print("Hata: Vakitler bulunamadı.")

@app.route('/')
def iftar_suresi():
    # Tarih ve saat hesaplama
    now = datetime.now(timezone)
    aksam_saat = datetime.strptime(aksam, "%H:%M")
    aksam_tarihi = datetime.now(timezone).date() if aksam_saat.time() > now.time() else datetime.now(timezone).date() + timedelta(days=1)
    aksam_zamani = timezone.localize(datetime.combine(aksam_tarihi, aksam_saat.time()))
    kalan_sure = aksam_zamani - now
    
    if kalan_sure.total_seconds() > 0:
        kalan_dakika = int((kalan_sure.total_seconds() // 60) % 60)
        kalan_saat = int(kalan_sure.total_seconds() // 3600)
        return render_template('iftar.html', kalan_saat=kalan_saat, kalan_dakika=kalan_dakika)
    else:
        return render_template('iftar.html', kalan_saat="0", kalan_dakika="0")

if __name__ == '__main__':
    app.run()
