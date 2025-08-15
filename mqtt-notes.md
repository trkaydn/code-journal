# IoT (Internet of Things) Notları

*10 Ağustos 2025 - Tarık Aydın*

## MQTT (Message Queuing Telemetry Transport)

- Client’ler arasında mesaj aktarımı sağlayan hafif bir protokoldür.  
- Publisher ve Subscriber arasında iletişim sağlar.  
- Gönderilen mesaj **Topic** olarak adlandırılır.  
- **MQTT Broker**, mesajı ileten aracıdır.  
- Publisher ile Subscriber birbirini tanımaz.  
- İletişim Topic üzerinden gerçekleşir.  
- Client’ler **unique ID** sahibidir.  
- IoT sektöründe sıkça kullanılır.  
- Client’ler, Broker SSL sertifikasını doğrulaması ve trafiği şifrelemesi için **TLS (Transport Layer Security)** kurulmalıdır.  
- Broker, client’ler üzerindeki SSL sertifikasını da doğrulayabilir. Hem client hem broker tarafında sertifika doğrulaması yapılırsa buna **mutual authentication** denir.  
- MQTT broker üzerine authentication server kurulabilir. Cihazlar username-password ile bağlanabilir, ancak SSL sertifikası kadar güvenli değildir.  
- Kural tabanlı yetkilendirme yapılabilir (**IoT Policy**).
