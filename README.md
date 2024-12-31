> [!TIP]
> English README available as README.en.md on the root directory.

# Uyarı
Çeşitli yasaklar bu listeye dahil edilmemiştir, alan adları çeşitli kaynaklardan alınmış ve toplanmıştır; aşağıdaki yöntemleri kontrol edebilirsiniz.

- USOM yasakları çok geniş, belirsiz ve çoğu zaman eksik olduğu için dahil edilmemiştir
- Bazı yasaklar İSS'ler tarafından uygulanır ancak BTK ve ESB API'leri tarafından sorgulanamaz. (Metodoloji kısmına bkz.)

# Katılım Sağlamak 
Katkıda bulunulması gereken ana dosya `yasakli_siteler.csv`dir, lütfen değişikliklerinizi dosyanın sonuna ekleyin veya alfabetik olarak ekleyin. Emin değilseniz lütfen bir issue oluşturun.

# Sorgulama
CSV dosyasını sorgulayabilen bir [basit sayfa](https://kulwsmm2saihuybu.vercel.app/) yapmak için v0 kullandım

# Metodoloji ve Strateji
DNS çözümlemesi, bireysel bir İSS'sinin varsayılan DNS sunucuları (bu durumda Superonline'ın) kullanılarak yapılır, eğer site engellenmişse, IP, ESB'ye ait olan `2a01:358:4014:a00::3` ve `195.175.254.2` olarak çözümlenir.

Ne yazık ki bu yalnızca alan adı yasal olarak yasaklanmışsa işe yarar, alan adı meşru olmayan bir şekilde yasaklanmışsa (örn. mahkeme kararı olmadan, USOM tarafından vb.) iki (şimdiye kadar keşfedilen) sonuç olabilir.

## USOM tarafından yasaklandı
USOM yasakları zaten herkese açık olarak yayınlanıyor, ancak bu püf noktasını bilmek sanırım iyi olur, Host header'ı USOM tarafından yasaklanmış bir alan adına ayarlanmış olarak “ESB AIDIYET” sunucusuna bir istek gönderildiğinde yanıt header'ları aşağıdaki gibidir;
```
HTTP/1.1 307 Temporary Redirect
Via: 1.0 middlebox
Location: http://88.255.216.16/landpage?op=2&ms=http://malicious.domain/
Connection: close
```
Yönlendirme konumunun ESB'ye ait olmadığını, farklı bir partiye ait olduğunu görebilirsiniz

## Muhtelif Partilerin Yasakları
Bu alan adlarını kimin yasakladığını ve kaç alan adına bu şekilde "shadowban" uygulandığını tam olarak anlayamadım, bulabildiğim bir örnek OnlyFans idi.
```
HTTP/1.1 307 Temporary Redirect
Via: 1.0 middlebox
Location: http://aidiyet.esb.org.tr/landpage?ms=http%3A%2F%2Fonlyfans.com%2F
Connection: close
```
Yönlendirme konumunun ESB'nin kendi sunucusu olduğunu görebilirsiniz

## Meşru Yasaklar
Bu curl yöntemini yasal olarak yasaklanmış bir alan adı üzerinde kullanırsanız, yasaklanmamış alan adları kullandığınızda da görülen belirsiz bir yanıt header'ı dönecektir.
```
HTTP/1.1 400
Content-Type: text/plain;charset=UTF-8
Content-Length: 0
Date: Tue, 31 Dec 2024 18:13:31 GMT
Connection: close
```
