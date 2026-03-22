Herkese merhaba arkadaşlar,
Bu bölümde anonimlik konusunda dikkat edilmesi gereken temel noktaları ele alacağız.

---

## Proxychains ve Tor Kullanımı

Beyler/Hanımlar, bu kurulum sayesinde trafiğimizi Tor ağının içinden geçireceğiz. Yani hedef sistem bizi değil, dünyanın herhangi bir yerindeki bir Tor çıkış düğümünü görecek.
Eğerki tor nedir bilmiyorsanız yada proxy nedir bilmiyorsanız google üzerinden hızlı bir şekilde araştırmanızı tavsite ederim.

ilk önce tor ve proxychains4'i indirelim, eğer debian temelli bir dağıtım kullanıyorsanız bu komut yeterli olacaktır

```bash
sudo apt update && sudo apt install tor proxychains4 -y
```

Proxychains'in çalışması için arkada Tor'un'da aktif olması lazım. Servisi başlatalım:

```bash
sudo systemctl start tor
# Eğer her açılışta kendi başlasın diyorwsanız bu komutuda giriniz
sudo systemctl enable tor
```

proxychains4'i yapılandırmamız lazım, çünkü trafiği istediğimiz gibi yönlendirmemiz için

ilk önce yapılandırma dosyasını açın.
```bash
sudo nano /etc/proxychains4.conf
```

**Dosya içinde şunları bulun ve düzeltin:**
*   `dynamic_chain` satırının başındaki `#` işaretini **kaldırın**. (Bir proxy patlarsa zincir kopmasın, devam etsin.)
*   `strict_chain` satırının başına `#` **koyun**.
*   `proxy_dns` kısmındaki `#` işaretini **kaldırın**. (DNS sızıntısı olmasın, operatörümüz nereye gittiğimizi görmesin.)
*   En alta inin, **[ProxyList]** altında şu satırın olduğundan emin olun:
    `socks5  127.0.0.1 9050`



Artık herhangi bir aracın önüne `proxychains4` eklediğiniz an veriler tor üzerinden geçecektir.

*   **IP değişti mi?**
    `proxychains4 curl https://ipinfo.io/ip`
*   **Gizli Nmap Taraması:**
    `proxychains4 nmap -sT -Pn <hedef-ip>`
    *(Not: Tor sadece TCP sever, o yüzden `-sT` kullanmayı unutmayın!)*

Firefox veya diğer tarayıcılarınızın manuel proxy ayarları kısmında toru yazdığınızda tarayıcınızda tor'a girecektir.

---
