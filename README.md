Sunucu aws üzerinde bulunmaktadır. Aws içerisinde Ubuntu 24.04 kullanılmıştır.
Url bilgisi: 52.19.220.25.
Let's Encrypt kullanılmamasının sebebi; Let's Encrypt bir alan adı gerektirir. Bu durumda, gerçek bir alan adı almalı ve DNS kayıtlarını yaptırmamız gereklidir.
----------------------------------------------------------------------
http çıktısı doğru gelirken, https çıktısında şu hata alınmaktadır.
curl -I https://52.19.220.25
curl: (60) SSL certificate problem: self-signed certificate
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.

Çözüm yolu şu şekildedir:
Sertifikayı curl ile Güvenilir Hale Getirme
Eğer self-signed sertifikayı curl ile geçerli olarak tanıtmak isterseniz, sertifikayı curl'ın sertifika deposuna ekleyebilirsiniz.

1.Adım: Sertifikayı İndiriyoruz:
Sertifikayı bir dosyaya indirin (örneğin nginx.crt):
sudo cp /etc/nginx/ssl/nginx.crt /home/ubuntu/nginx.crt

2. Adım: Sertifikayı curl ile Kullanma:
curl komutunu sertifika dosyasını belirterek kullanabilirsiniz.
curl --cacert /home/ubuntu/nginx.crt https://52.19.220.25

3. Adım:  Sertifikayı Sistem Sertifika Deposu ile Güvenilir Hale Getirme
Sertifikayı Sistem Sertifika Deposuna Ekleme:
sudo cp /etc/nginx/ssl/nginx.crt /usr/local/share/ca-certificates/nginx.crt
sudo update-ca-certificates
4. Adoım : Sonra curl Komutunu Tekrar Çalıştırıyoruz.
Sertifikayı ekledikten sonra, curl komutunu doğrulama ile çalıştırabilirsiniz:
curl https://52.19.220.25
----------------------------------------------------------------------
Çıktılar: 
curl -I http://52.19.220.25
HTTP/1.1 200 
Server: nginx/1.24.0 (Ubuntu)
Date: Sun, 28 Jul 2024 20:45:17 GMT
Content-Type: text/html;charset=UTF-8
Content-Length: 3868
Connection: keep-alive
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
X-Frame-Options: DENY
Content-Language: en-US
--------------------------------------------------------------------------
curl -I https://52.19.220.25
HTTP/1.1 200 
Server: nginx/1.24.0 (Ubuntu)
Date: Sun, 28 Jul 2024 20:45:24 GMT
Content-Type: text/html;charset=UTF-8
Content-Length: 3868
Connection: keep-alive
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
Strict-Transport-Security: max-age=31536000 ; includeSubDomains
X-Frame-Options: DENY
Content-Language: en-US
---------------------------------------------------------------------------
