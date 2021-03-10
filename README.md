### Yapıkredi API kullanım notları

İlk aşamada sitede bazı yazılarla karşılaşmama rağmen developer dostu örnekli döküman bulamadığım için deneyerek ve uzun uğraşlar sonucu çözüm sağladım.<br>
Ayrıca bazı bankalarda yine aynı url yapısı ve oauth sistemini hemen hemen -birebir- kullandığını gördüm. <br>
Basitçe anlatmaya çalıştım, umarım faydası olur.<br>

- https://apiportal.yapikredi.com.tr üzerinde hesap açıp register olun
- https://apiportal.yapikredi.com.tr/documentation üzerindeki gibi ihtiyacınız olan işlevlerde (örneğin current rates - döviz kuru eklemek gibi) application oluşturup oauth2 üretin
- Application onay aldıktan sonra applicationu düzenleyerek api kısmındaki key ve secreti alın

#### oauth2 ile token alma
- HTTP post request yapılmalı
- URL https://api.yapikredi.com.tr/auth/oauth/v2/token olmalı
- Headera "Accept: application/json" ve "Content-Type: application/x-www-form-urlencoded" eklenmeli
- Post edilecek data "scope=oob&grant_type=client_credentials&client_id=AlınanKEYKodu&client_secret=AlınanSECRETKodu" şeklinde olmalı
- Gelen cevap json olacak access_token içeriğini kullanacak ve bu içeriği expires_in zamanındaki saniye kadar saklamanız ve yeni isteklerde aynı tokeni kullanmanız gerekmekte.
<br>
- Örnek Curl kodu : 

~~~~
curl -X POST -k -H 'Accept: application/json' -H 'Content-Type: application/x-www-form-urlencoded' -i 'https://api.yapikredi.com.tr/auth/oauth/v2/token' --data 'scope=oob&grant_type=client_credentials&client_id=**************&client_secret=************'
~~~~

- Cevap : 

~~~~
{
  "access_token":"ACCESSTOKENBURADA",
  "token_type":"Bearer",
  "expires_in":3600,
  "scope":"oob"
}
~~~~

- access_token valueyi not edelim.

#### döviz kuru bilgisi çekmek
- HTTP get request yapılmalı (standart çağırı)
- URL https://api.yapikredi.com.tr/api/investmentrates/v1/currencyRates olmalı (bu link döviz kuru içindir)
- Headera oauth2 ile aldığınız access_token key içeriğini "Authorization:Bearer ACCESSTOKENBURAYA" ACCESSTOKENBURAYA yazısın yerine ekleyin.
<br>

- Örnek Curl kodu :

~~~~
curl --include --header "Authorization:Bearer **********" --request GET 'https://api.yapikredi.com.tr/apnvestmentrates/v1/currencyRates'
~~~~

- Cevap : 

~~~~
{"response": {"exchangeRateList": [
    {
        "averageR........
....
...
..
...
....

    }
]}}
~~~~

Faydalı olması dileğiyle .. <Murat B.>
