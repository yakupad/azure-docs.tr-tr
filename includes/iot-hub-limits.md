Aşağıdaki tabloda farklı hizmet katmanları (S1, S2, S3, F1) ile ilişkili sınırlar listelenmektedir. Bir katmandaki her bir *birimin* maliyeti hakkında bilgi için bkz. [IoT Hub Fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/).

| Kaynak | S1 Standart | S2 Standart | S3 Standart | F1 Ücretsiz |
| --- | --- | --- | --- | --- |
| İleti/gün |400,000 |6,000,000 |300,000,000 |8,000 |
| En fazla birim |200 |200 |10 |1 |

> [!NOTE]
> S1 veya S2 hub’ı ile 200’den fazla veya S3 hub’ı ile 10’dan fazla birim kullanacağınızı düşünüyorsanız lütfen Microsoft desteğine başvurun.
> 
> 

Aşağıdaki tabloda IOT Hub kaynakları için geçerli olan sınırlar listelenmektedir:

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına en fazla ücretli IoT hub’ı sayısı |50 |
| Azure aboneliği başına en fazla ücretsiz IoT hub’ı sayısı |1 |
| Bir cihaz kimliği karakter sayısı | 128 |
| En fazla cihaz kimliği sayısı<br/> (tek bir çağrıda döndürülen) |1000 |
| Cihazdan buluta iletiler için IoT Hub en fazla ileti bekletme süresi |7 gün |
| Cihazdan bulut iletinin en büyük boyutu |256 KB |
| Cihazdan buluta toplu işin en büyük boyutu |256 KB |
| Cihazdan buluta toplu işte en fazla ileti sayısı |500 |
| Buluttan cihaza iletinin en büyük boyutu |64 KB |
| Buluttan cihaza iletiler için en büyük TTL |2 gün |
| Buluttan cihaza iletiler için en fazla teslim <br/> sayısı |100 |
| Geri bildirim iletileri için en fazla teslim sayısı <br/> (buluttan cihaza iletiye yanıt olarak) |100 |
| Buluttan cihaza iletiye yanıt olarak <br/> geri bildirim iletileri için en fazla TTL |2 gün |
| En büyük cihaz ikizi boyutu <br/> (etiketler, rapor edilen özellikler ve istenen özellikler) | 8 KB |
| Cihaz ikizi dize değerinin en büyük boyutu | 4 KB |
| Cihaz ikizindeki en büyük nesne derinliği | 5 |
| Doğrudan yöntem yükünün en büyük boyutu | 128 KB |
| En büyük iş geçmişi bekletme süresi | 30 gün |
| En fazla eşzamanlı iş sayısı | 10 (S3 için), 5 (S2 için), 1 (S1 için) |
| En fazla ek uç nokta sayısı | 10 (S1, S2, S3 için) |
| En fazla ileti yönlendirme kuralı sayısı | 100 (S1, S2, S3 için) |


> [!NOTE]
> 50'den fazla Ücretli IOT hub'ı bir Azure aboneliğine ihtiyacınız varsa, Microsoft desteğine başvurun.


> [!NOTE]
> Şu anda en fazla cihazları tek bir IOT hub'ına bağlanabilir 500.000 sayısıdır. Bu sınırı artırmak istiyorsanız, ilgili kişi [Microsoft Support](https://azure.microsoft.com/support/options/).

Aşağıdaki kotalar aşıldığında IoT Hub hizmeti istekleri kısıtlar:

| Kısıtlama | Hub başına değer |
| --- | --- |
| Kimlik kayıt defteri işlemleri <br/> (oluşturma, alma, listeleme, güncelleştirme, silme) <br/> tek tek veya toplu içeri/dışarı aktarma |(S3 için) 83.33/sec/Unit (5000/dk/birim) <br/> (100/dk/birim) 1.67/sec/Unit (için S1 ve S2 için). |
| Cihaz bağlantıları |6000/sn/birim (S3 için), 120/sn/birim (S2 için), 12/sn/birim (S1 için). <br/>En az 100/sn. |
| Cihazdan buluta gönderim |6000/sn/birim (S3 için), 120/sn/birim (S2 için), 12/sn/birim (S1 için). <br/>En az 100/sn. |
| Buluttan cihaza gönderim | (S1 ve S2 için için) 83.33/sec/Unit (5000/dk/birim) (S3 için), 1.67/sec/unit (100/dk/birim). |
| Buluttan cihaza alım |833.33/sec/Unit (50000/dk/birim) (S3 için), 16.67/sec/unit (1000/dk/birim) (S1 ve S2 için). |
| Dosya karşıya yükleme işlemleri |83.33 dosya karşıya yükleme bildirimi/sn/birim (5000/dk/birim) (S3 için), 1,67 dosya karşıya yükleme bildirimler/sn/birim (100/dk/birim) (S1 ve S2 için). <br/> Bir Azure Depolama hesabı için tek seferde 10000 SAS URI’si çıkarılabilir.<br/> Tek seferde 10 SAS URI’si/cihaz çıkarılabilir. |
| Doğrudan yöntemler | 24MB/sn/birim (S3 için), 480KB/sn/birim (S2 için), 160KB/sn/birim (S1 için)<br/> 8 ölçüt boyutu azaltma KB temel. |
| Cihaz ikizi okumaları | 50/sn/birim (S3 için), En fazla 10/sn veya 1/sn/birim (S2 için), 10/sn (S1 için) |
| Cihaz ikizi güncelleştirmeleri | 50/sn/birim (S3 için), En fazla 10/sn veya 1/sn/birim (S2 için), 10/sn (S1 için) |
| İş işlemleri <br/> (oluşturma, güncelleştirme, listeleme, silme) | 83.33/sec/Unit (5000/dk/birim) (S3 için), 1.67/sec/unit (100/dk/birim) (S2 için), (S1 için) 1.67/sec/unit (100/dk/birim) |
| İşler işlemleri için cihaz başına aktarım hızı | 50/sn/birim (S3 için), En fazla 10/sn veya 1/sn/birim (S2 için), 10/sn (S1 için) |
