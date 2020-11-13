# IOTA_DATA_SET

### kütüphaneleri yüklüyoruz
from bs4 import BeautifulSoup
import requests
import csv

### Veri çekme için gerekli parametreler
headers_param = {"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.193 Safari/537.36"}
#Verileri çekeceğim web sayfası
Link="https://www.coinlore.com/coin/iota/historical-data/-7200/1605301199#hist-prices"

### Verileri ayırırken kullanıcağımız diziler
Date_IOTA=['Date']
Open_IOTA=['Open']
High_IOTA=['High']
Low_IOTA=['Low']
Close_IOTA=['Close']
Volume_=['Volume']
Volume_IOTA=['Volume IOTA']
Market_Cap_IOTA=['Market Cap']
IOTA_DATA=[]

### Dosya işlemleri
filename = "IOTA_DATA.csv"
csvFile = open(filename, "w")

###lineterminator== Dosyaya yazarken alt satıra kaçmasını engelliyor 
csvwrite = csv.writer(csvFile,lineterminator='\n')

###Veriyi parselleme
IOTA_page=requests.get(f"{Link}",headers=headers_param)
soup = BeautifulSoup(IOTA_page.content,"html.parser")

###Veriyi ayıklama
ozellikler= list(soup.find_all("td",attrs={"class":"no-wrap"}))
print(ozellikler)

###Verideki $ işaretlerini temizliyoruz
for i in range(0,len(ozellikler)):
  ozellikler[i]=ozellikler[i].text.replace("$","")

###Elimizdeki veriyi parçalıyoruz
for i in range(0,9863,8):
   Date_IOTA.append(ozellikler[0+i])
   Open_IOTA.append(ozellikler[1+i])
   High_IOTA.append(ozellikler[2+i])
   Low_IOTA.append(ozellikler[3+i])
   Close_IOTA.append(ozellikler[4+i])
   Volume_.append(ozellikler[5+i])
   Volume_IOTA.append(ozellikler[6+i])
   Market_Cap_IOTA.append(ozellikler[7+i])
   
###Parçaladığımız veriyi birleştiriyoruz
for d,o,h,l,c,v,vi,m in zip(Date_IOTA,Open_IOTA,High_IOTA,Low_IOTA,Close_IOTA,Volume_,Volume_IOTA,Market_Cap_IOTA):
    IOTA_DATA.append([d,o,h,l,c,v,vi,m])

###Elimnizdeki datayıCSV dosyasına yazıyoruz
csvwrite.writerows(IOTA_DATA)
