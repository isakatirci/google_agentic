# Google ve Kaggle'dan 5 Günlük Yapay Zeka Aracısı (AI Agent) Geliştirme Kursu

Bu depo, Google ve Kaggle iş birliğiyle hazırlanan beş günlük yapay zeka aracısı (`AI Agent`) geliştirme kursunun materyallerini ve Cursor platformu için tasarlanmış ilgili beceri (`skill`) dosyalarını bir araya getirmektedir. Amacımız, özellikle **junior geliştiricilere**, `AI Agent`'ları sıfırdan tasarlama, inşa etme, değerlendirme ve dağıtma süreçlerinde adım adım rehberlik etmek, modern `AI Agent` geliştirme pratiklerini ve Google'ın Ajan Geliştirme Kiti (`ADK`) gibi araçları tanıtarak onları sektördeki güncel `trend`lere hazırlamaktır.

## Depo İçeriği ve Yapısı

Bu depo, öğrenme ve uygulama deneyiminizi zenginleştirecek çeşitli kaynaklar içerir:

*   **Kurs Materyalleri (`5-day-ai-agents/`):**
    *   Bu klasör, kursun temelini oluşturan beş gün için düzenlenmiş not defterlerini (`Jupyter Notebooks`), detaylı ders notlarını ve `PDF` kaynaklarını barındırır. Her gün için ayrı klasörler (`Day1`, `Day2` vb.) altında düzenlenmiştir ve her günün materyalleri belirli bir konuya odaklanır. Örneğin:
        *   **Day1:** `Agent` temelleri ve mimarileri.
        *   **Day2:** `Tool`'lar (Araçlar) ve `Model Context Protocol` (`MCP`).
        *   **Day3:** `Context Engineering` (Bağlam Mühendisliği), `Session`'lar (Oturumlar) ve `Memory` (Hafıza).
        *   **Day4:** Kalite, `Observability` (Gözlemlenebilirlik) ve `Evaluation` (Değerlendirme).
        *   **Day5:** `Agent-to-Agent Communication` (`A2A`) (Ajanlar Arası İletişim) ve `Deployment` (Dağıtım).
*   **Ortak Terimler Sözlüğü (`GLOSSARY.md`):**
    *   Yapay zeka aracıları dünyasında sıkça karşılaşılan ve bu kursta kullanılan anahtar `terim`lerin (`Agent`, `Model`, `Tool`, `Orchestration`, `Trajectory`, `Session`, `Event`, `State`, `Memory`, `Context Engineering`, `Compaction`, `AgentTool`, `Workflow agent`, `MCP`, `A2A`, `Agent card`, `Agent Ops`, `Eval set`, `Golden set`, `HITL` vb.) ayrıntılı, anlaşılır açıklamalarını içerir. Bu sözlük, kavramsal birliği sağlamak ve `terim`lerin doğru anlaşılmasını garanti etmek açısından kritik öneme sahiptir.
*   **Cursor Becerileri (`.cursor/skills/`):**
    *   Bu dizin, Cursor platformunda yapay zeka aracısı geliştirme sürecini kolaylaştırmak için tasarlanmış özel beceri dosyalarını içerir. Bu beceriler, geliştiricilerin belirli görevleri (örneğin, `agent` mimarisi seçimi, `agent` tanımlama, `context engineering`, `tool` tasarımı, `A2A` veya `MCP` üzerinden `agent` bağlantısı kurma, `session` yönetimi, `memory` kalıcılığı, `agent`'ları `evaluation` ve `observability`, ve `agent`'ları `deployment`) adım adım gerçekleştirmesine olanak tanıyan iş akışları sunar. Her bir beceri, ilgili `AI Agent` geliştirme adımına odaklanır ve pratik uygulamayı destekler.
*   **Repomix Çıktısı (`repomix-output.md`):**
    *   Bu dosya, deponun genel bir özeti ve Cursor becerileri ile Google `ADK` rehberinin birleştirilmiş halini temsil eden **yalnızca inceleme amaçlı bir belgedir.** Depo hakkındaki üst düzey bilgileri, Cursor becerilerinin sağladığı iş akışlarını ve Google `ADK`'nın kurulum ve çalıştırma pratiklerini özetler. **Bu dosya asla `git commit` işlemine dahil edilmemelidir**, çünkü bu dosya, depo içeriğinin otomatik olarak oluşturulmuş bir özetidir ve gerçek proje dosyalarınızı veya konfigürasyonunuzu temsil etmez.

## Geliştirme Ortamı Kurulumu

Bu projeyi yerel ortamınızda sorunsuz bir şekilde çalıştırmak ve kurs materyallerinden tam verim almak için aşağıdaki gereksinimleri karşılamanız gerekmektedir:

*   **Python:** **Python 3.10 veya üzeri bir sürüm** kurulu olmalıdır. Daha eski sürümler `uyumluluk` sorunlarına yol açabilir.
*   **Sanal Ortam (`Virtual Environment`):** Proje `bağımlılık`larını (`dependencies`) izole etmek ve sisteminizdeki diğer `Python` projeleriyle çakışmaları önlemek için bir **sanal ortam (`venv` veya `conda` gibi) kullanmanız şiddetle tavsiye edilir.**
    ```bash
    python -m venv venv # Bir sanal ortam oluşturma
    source venv/bin/activate # Sanal ortamı etkinleştirme (Linux/macOS)
    # veya venv\Scripts\activate (Windows)
    ```
*   **JupyterLab:** Kurs not defterlerini etkileşimli olarak çalıştırmak ve deney yapmak için **JupyterLab kurulumu** gereklidir.
    ```bash
    pip install jupyterlab
    jupyter lab # JupyterLab'i başlatma
    ```
*   **Google ADK (`google-adk`):** Google'ın Ajan Geliştirme Kiti'ni (`Agent Development Kit`) yüklemeniz gerekmektedir. Bu kit, `agent`'larınızı oluşturmak ve yönetmek için gerekli temel `tool`'ları (araçları) sağlar.
    ```bash
    pip install google-adk
    ```

## Google Gemini ile Entegrasyon ve API Anahtarı Yönetimi

`Gemini model`ini kullanarak `agent`'larınızı geliştirmek ve çalıştırmak için bir `GOOGLE_API_KEY`'e ihtiyacınız olacaktır.

*   **API Anahtarı Edinme:** Anahtarınızı Google `AI Studio` platformundan kolayca edinebilirsiniz.
*   **Ortam Değişkeni Olarak Ayarlama:** **`GOOGLE_API_KEY`'i asla doğrudan `kod`unuzun içine yazmayın veya `GitHub` gibi herkese açık bir `repo`'da paylaşmayın!** Bunun yerine, anahtarınızı bir `ortam değişkeni` (`environment variable`) olarak ayarlamalısınız. Bu, `güvenlik` (`security`) açısından kritik öneme sahiptir.
    ```bash
    export GOOGLE_API_KEY="buraya-anahtar" # Linux/macOS
    # veya set GOOGLE_API_KEY="buraya-anahtar" (Windows CMD)
    # veya $env:GOOGLE_API_KEY="buraya-anahtar" (Windows PowerShell)
    ```
*   **Kaggle Not Defterleri İçin:** Kaggle ortamında çalışırken, not defterleri `API anahtar`ınızı `UserSecretsClient` aracı aracılığıyla güvenli bir şekilde okuyacak şekilde yapılandırılmıştır.

## Google ADK Rehberi ve En İyi Uygulamalar

Google `ADK` rehberi, `agent`'larınızı geliştirirken izlemeniz gereken önemli adımları ve en iyi `pratik`leri (`best practices`) belirtir:

*   **`google-adk` Kullanımı:** `Agent`'larınızın temel yapılarını ve etkileşimlerini yönetmek için bu `kit`i nasıl kullanacağınızı öğrenin.
*   **`Agent` Tanımlama:** Her `agent`'ın bir `ad`a, kullanılacak bir `model`e, belirli `talimatlar`a (`instructions`) ve görevlerini yerine getirecek `tool`'lara sahip olması gerektiğini unutmayın.
*   **`API Call`'ları (API Çağrıları):** `API call`'ları arasında uygun `zamanlama`yı (`spacing`) koruyarak `hız limiti` (`rate limit`) aşımlarına takılmayı önleyin.
*   **Hata Yönetimi:** `Hız limiti` aşımları ve `sunucu` hataları gibi durumlar için `yeniden deneme yapılandırmaları` (`retry configurations`) kullanın.
*   **`Agent`'ı Çalıştırma:** `InMemoryRunner` ile hızlı `prototipleme` ve `run_debug` modunu kullanarak ayrıntılı `hata ayıklama` (`debugging`) süreçlerini öğrenin.
*   **`Session Management` (Oturum Yönetimi):** `Prototip` aşamasındaki `session management` ile `üretim ortamı`ndaki (`production environment`) kalıcı ve güvenilir `session management` arasındaki farkları anlayın.
*   **`Security Warning` (Güvenlik Uyarısı):** Kimlik doğrulama bilgisi içeren `URL`'leri ve diğer `gizli verileri` (`secrets`) her zaman `koruma` altına almanız gerektiğini unutmayın.

## Yapay Zeka Aracısı Kavramlarına Derinlemesine Bakış

Bu rehber, basit "tek turda metin üreten sohbet" uygulamaları ile "hedefe ulaşmak için `tool`'ları stratejik olarak çağıran ve sonuçları `değerlendiren`" gelişmiş yapay zeka `agent`'ları arasındaki temel farkı vurgular. Bir `AI Agent`'ın temel bileşenleri şunlardır:

*   **Karar Veren `Model`:** Aracının "beynini" oluşturan, durumu analiz eden, hedefleri belirleyen ve eylemler planlayan temel yapıdır. Genellikle `büyük dil modelleri` (`LLM`'ler) kullanılır.
*   **Araçlar (`Tools`):** Aracının belirli görevleri yerine getirmek için kullandığı dış işlevler veya `API`'lerdir (örneğin, `web search` yapma, `hesaplama` yapma, `veritabanı`na erişme, bir `e-posta` gönderme). Bu `tool`'lar, `agent`'ın dış dünya ile etkileşim kurmasını sağlar.
*   **Orkestrasyon (`Orchestration`):** Aracının adımlarını, `tool` çağrılarını ve hedefe ulaşma stratejisini yöneten süreçtir. Bu, `agent`'ın hangi `tool`'u ne zaman kullanacağına karar vermesini, sonuçları yorumlamasını ve bir sonraki adıma geçmesini içerir.

`GLOSSARY.md` dosyasında ayrıntılarıyla bulacağınız ek kavramlar:

*   **Trajectory (`Yörünge`):** Bir aracının başlangıçtan hedefe ulaşana kadar gerçekleştirdiği tüm eylemlerin, gözlemlerin ve `state` (`durum`) değişikliklerinin kronolojik kaydıdır.
*   **Session (`Oturum`):** Bir kullanıcı ile `agent` arasındaki belirli bir etkileşim dizisidir. Her `session`'ın kendine özgü bir `bağlam`ı (`context`) ve `yaşam döngüsü` (`lifecycle`) vardır ve `agent`'ın geçmiş etkileşimleri hatırlamasını sağlar.
*   **Event (`Olay`):** Bir `session` içinde meydana gelen önemli bir eylem veya `state` (`durum`) değişikliğidir (örneğin, bir kullanıcının mesaj göndermesi, bir `agent`'ın belirli bir `tool`'u çağırması, bir `agent`'ın bir görevi tamamlaması).
*   **State (`Durum`):** Bir `agent`'ın belirli bir anda sahip olduğu tüm bilgiler ve özelliklerdir. Bu, `memory` (`hafıza`) içeriği, mevcut hedef, kullanılan `tool`'lar ve alınan kararlar gibi öğeleri içerebilir.
*   **Memory (`Hafıza`):** Aracının geçmiş etkileşimlerden ve öğrendiklerinden bilgi depolama ve geri çağırma yeteneğidir. Kısa vadeli (`current conversation context`) ve uzun vadeli (`knowledge base`) `memory` türleri bulunur.
*   **Context Engineering (`Bağlam Mühendisliği`):** Modele gönderilen `istem`in (`prompt`) ve ilgili bilgilerin, `model`in en iyi `performans`ı göstermesi ve doğru kararları alması için dikkatlice seçilmesi, organize edilmesi ve `optimize` edilmesi sürecidir.
*   **Compaction (`Sıkıştırma`):** `Memory`'deki veya `bağlam`daki (`context`) gereksiz, tekrarlayan veya daha az önemli bilgilerin azaltılarak daha verimli hale getirilmesidir. Bu, `model`in `bağlam penceresi`ni (`context window`) aşmadan önemli bilgileri korumasına yardımcı olur.
*   **AgentTool:** Gelişmiş bir kavram olup, bir yapay zeka aracısının, daha karmaşık bir görevi yerine getirmek için başka bir uzman `agent`'ı bir `tool` (araç) olarak kullanmasını ifade eder.
*   **İş Akışı `Agent` Türleri:**
    *   `SequentialAgent` (`Sıralı Ajan`): Görevleri önceden belirlenmiş, sabit bir sırayla yürüten `agent`'lar. Her adım, bir önceki adımın sonucuna bağlıdır.
    *   **`ParallelAgent` (`Paralel Ajan`):** Birden fazla görevi veya alt görevi eş zamanlı olarak yürüten `agent`'lar. Bu, görevlerin birbirinden bağımsız olduğu durumlarda `performans`ı artırır.
    *   **`LoopAgent` (`Döngüsel Ajan`):** Belirli bir koşul karşılanana kadar görevleri veya adımları tekrar eden `agent`'lar. Genellikle bir hedefe ulaşana veya belirli bir `state` (`durum`) oluşana kadar kullanılır.
*   **Model Bağlam Protokolü (`MCP`):** Farklı `servis`ler, `model`ler ve `tool`'lar arasında standartlaştırılmış bir iletişim kurarak, `agent` sistemlerinin farklı bileşenlerle daha kolay `entegre` edilmesini ve birlikte çalışmasını sağlayan bir `protokol`dür.
*   **Ajanlar Arası İletişim (`A2A`):** Bir yapay zeka aracısının, daha karmaşık görevleri veya uzmanlık gerektiren durumları başka bir `agent`'a `devret`mesini (`delegate`) veya onunla iş birliği yapmasını ifade eder. Bu, `modüler` ve uzmanlaşmış `agent` sistemleri oluşturmaya olanak tanır.
*   **Agent Card (`Ajan Kartı`):** Bir `agent`'ın yeteneklerini, sınırlamalarını, amaçlarını, mimarisini, kullanılan `model`leri, `tool`'ları ve sorumlu kullanım yönergelerini özetleyen kapsamlı bir bilgi kartıdır. `Şeffaflık` (`transparency`) ve yönetilebilirlik için önemlidir.
*   **Agent Ops (`Ajan Operasyonları`):** `Agent`'ların `üretim ortamı`nda (`production environment`) izlenmesi (`monitoring`), yönetilmesi (`management`), sürdürülmesi (`maintenance`) ve `performans`larının `optimize` edilmesi ile ilgili `süreç`ler ve `tool`'lardır.
*   **Eval Set (`Değerlendirme Seti`):** Bir `agent`'ın `performans`ını nesnel olarak ölçmek ve doğrulamak için kullanılan, gerçek dünya `senaryo`larını yansıtan özel olarak hazırlanmış `veri setleri`dir.
*   **Golden Set (`Altın Set`):** Bilinen doğru veya ideal cevapları ve davranışları içeren, yüksek `kalite`li bir `değerlendirme veri seti`dir. Bir `agent`'ın ne kadar iyi `performans` gösterdiğini belirlemek için `referans` olarak kullanılır.
*   **HITL (`Human-in-the-Loop` - İnsan Destekli Döngü):** Yapay zeka sistemlerinin `karar alma süreçleri`ne (`decision-making processes`) veya `iş akışları`na (`workflows`) insan müdahalesinin dahil edilmesidir. Bu, özellikle `hassas` (`sensitive`), `kritik` (`critical`) veya `belirsiz` (`ambiguous`) görevlerde `güvenlik` (`safety`), `etik uygunluk` (`ethical compliance`) ve `doğruluk` (`accuracy`) sağlamak için önemlidir.

## Beş Günlük Kursun Detaylı İçeriği

Kursun her günü, ilgili konuları derinlemesine ele alan iki adet `Jupyter not defteri` ve bir adet detaylı `ders notu` (`Markdown` veya `PDF` formatında) içermektedir. Bu yapı, hem `teorik bilgi`yi hem de `pratik uygulama`yı bir arada sunarak öğrenme deneyiminizi en üst düzeye çıkarmayı hedefler.

**Önemli Not: Not defterlerini çalıştırırken lütfen aşağıdaki noktalara dikkat edin:**

*   Not defterlerinin hücrelerini **yukarıdan aşağıya doğru sırayla tek tek çalıştırmanız** şiddetle tavsiye edilir.
*   **Tüm hücreleri aynı anda çalıştırmaktan kaçının.** Bu durum, `API kota limitleri`ne (`API rate limits`) takılma riskini artırabilir ve olası `hata`ların (`error`) veya `beklenmedik davranış`ların (`unexpected behavior`) kaynağını tespit etmeyi oldukça zorlaştırabilir. Her adımı anlayarak ve sonuçları gözlemleyerek ilerlemek, `öğrenme süreci`niz için çok daha faydalı olacaktır.

### Gün 1: Ajanlara Giriş ve Temel Mimariler

Bu gün, yapay zeka `agent`'larının ne olduğunu, nasıl çalıştığını ve farklı yetenek seviyelerini kapsar. İlk `agent`'ınızı kurarak `pratik deneyim` (`hands-on experience`) kazanacaksınız.

*   **Ajan Seviyeleri (`Capabilities`):**
    *   **`Seviye 0 (Yalnızca Model)`:** Temel `dil modeli` (`language model`) yeteneklerini kullanan `agent`'lar. Sadece `metin` üretirler ve dış dünya ile etkileşim kurmazlar. Örneğin, basit bir `sohbet botu` (`chatbot`).
    *   **`Seviye 1 (Araç Kullanımı)`:** Dış `tool`'ları (örneğin, `web search` yapma, `hesap makinesi`, `takvim API`'si) kullanarak belirli görevleri yerine getirebilen `agent`'lar.
    *   **`Seviye 2 (Çok Adımlı Planlama)`:** Karmaşık görevleri mantıksal alt adımlara ayırarak `planlama` (`planning`) yapabilen ve bu adımları sırayla yürütebilen `agent`'lar.
    *   **`Seviye 3 (Uzman Ajan Takımları)`:** Birden fazla uzmandan oluşan `agent`'ların (örneğin, bir `planlayıcı agent`, bir `araştırma agent`'ı, bir `kod yazma agent`'ı) bir araya gelerek işbirliği yaparak daha büyük hedeflere ulaşabildiği sistemler.
    *   **`Seviye 4 (Öz-Değişim Yeteneği)`:** `Agent`'ların kendi davranışlarını, kurallarını veya hatta `kod`larını dinamik olarak değiştirebildiği ileri seviye `agent`'lar. **Bu seviye, `ürün şablonları` (`product templates`) için genellikle önerilmez** çünkü `öngörülemezlik` (`unpredictability`), `güvenlik açıkları` (`security vulnerabilities`) ve `kontrol zorlukları` (`control challenges`) gibi yüksek `risk`ler barındırabilir.
*   **Uygulamalı Çalışmalar:**
    *   Günün not defterleri, ilk yapay zeka aracınızı kurmayı ve basit bir `arama tool`'u entegre etmeyi adım adım öğretir.
    *   Tek bir `agent` ile birden fazla `agent`'tan oluşan `takım yapıları` (`team structures`) arasındaki farkları, avantajlarını ve dezavantajlarını karşılaştırır.
    *   Sabit sıralı (`SequentialAgent`), paralel çalışma (`ParallelAgent`) ve döngüsel (`LoopAgent`) mimariler gibi farklı `agent iş akışı` (`agent workflow`) tasarımlarını uygulamalı olarak inceler.

### Gün 2: Ajan Araçları ve MCP (`Model Context Protocol`) ile Birlikte Çalışabilirlik

Bu gün, `agent`'ların dış sistemlerle nasıl etkileşim kurduğunu ve daha karmaşık `tool`'ların nasıl tasarlanacağını derinlemesine ele alır.

*   **Ajan Araçlarının Derinlemesine İncelenmesi:** `Agent`'ların görevlerini yerine getirmek için kullandığı `tool`'ların (örneğin, bir `veritabanı sorgulama tool`'u, bir `resim oluşturma tool`'u) tasarımı, uygulanması ve bu `tool`'ların verimli ve güvenli bir şekilde nasıl geliştirileceği konusunda en iyi `pratik`ler (`best practices`) sunulur.
*   **Model Bağlam Protokolü (`MCP`):** Farklı `servis`ler, `model`ler ve `tool`'lar arasında standartlaştırılmış bir iletişim kurarak, `agent` sistemlerinin farklı bileşenlerle daha kolay `entegre` edilmesini ve birlikte çalışmasını sağlayan bir `protokol`dür. `MCP`'nin nasıl çalıştığı ve `agent`'larınızda nasıl kullanacağınız açıklanır.

### Gün 3: Bağlam Mühendisliği (`Context Engineering`), Oturumlar (`Sessions`) ve Hafıza Yönetimi (`Memory Management`)

Bu gün, `agent`'ların etkileşim `bağlam`ını (`context`) nasıl anladığını, kullanıcılarla olan `session`'ları nasıl yönettiğini ve bilgiyi nasıl depolayarak geçmişi hatırladığını inceler.

*   **Bağlam Mühendisliği Teknikleri:** `Agent`'ın `karar alma süreci`ni (`decision-making process`) ve `yanıt`larını (`responses`) etkileyen bilgilerin (geçmiş `konuşmalar` (`conversations`), `kullanıcı tercihleri` (`user preferences`), güncel `olay`lar (`events`) vb.) nasıl toplanacağı, işleneceği ve `model`e nasıl sunulacağı konusunda detaylı `strateji`ler.
*   **Oturum Yönetimi (`Session Management`):** `Agent`'ın bir `kullanıcı`nın (`user`) etkileşimini birden fazla `mesaj` (`message`) veya `eylem` (`action`) arasında `zaman içinde` (`over time`) nasıl takip ettiği ve sürdürdüğü. Kesintisiz ve tutarlı bir `kullanıcı deneyimi` (`user experience`) için `session management`'ın önemi.
*   **Hafıza Mimarileri (`Memory Architectures`):** `Agent`'ın kısa vadeli (`anlık etkileşimler` (`instantaneous interactions`) için) ve uzun vadeli (`öğrenilen bilgiler` (`learned knowledge`) ve `kalıcı veriler` (`persistent data`) için) `memory` sistemlerinin nasıl tasarlandığı, uygulandığı ve bu `memory`'lerin nasıl `sorgulanıp` (`queried`) güncellendiği.

### Gün 4: Ajan Kalitesi: Gözlemlenebilirlik (`Observability`) ve Değerlendirme (`Evaluation`)

Bu gün, `üretim ortamı`nda (`production environment`) çalışan `agent`'ların `performans`ını ve `güvenilirlik`ini (`reliability`) sağlamak için gerekli olan `gözlem` (`observation`) ve `değerlendirme` (`evaluation`) yöntemlerini ele alır.

*   **Ajan Gözlemlenebilirliği (`Agent Observability`):** `Agent`'ın `çalışma zamanı`ndaki (`runtime`) davranışlarını, aldığı kararları, kullandığı `tool`'ları, `performans`ını ve olası `hata`larını (`errors`) izleme (`monitoring`), `loglama` (`logging`) ve `analiz etme` (`analyzing`) yolları. Bu, `sorun giderme` (`troubleshooting`) ve `optimizasyon` (`optimization`) için `kritik öneme` sahiptir.
*   **Ajan Değerlendirme Metodolojileri (`Agent Evaluation Methodologies`):** Bir `agent`'ın `doğruluk` (`accuracy`), `verimlilik` (`efficiency`), `güvenilirlik` (`reliability`), `güvenlik` (`safety`) ve istenen `çıktı`ları (`outputs`) üretme yeteneğini `objektif` olarak ölçmek için kullanılan çeşitli `metrik`ler (`metrics`), `çerçeve`ler (`frameworks`) ve `test yaklaşımları` (`testing approaches`) (örneğin, `Eval Set`, `Golden Set` kullanımı).

### Gün 5: Prototiplemeden Üretime: Ajanlar Arası İletişim (`A2A`) ve Dağıtım (`Deployment`)

Bu gün, geliştirilen `agent`'ların gerçek dünya `uygulama`larına (`applications`) nasıl `entegre` edildiğini, birbirleriyle nasıl etkileşim kurduğunu ve `üretim ortamları`na nasıl güvenli ve `ölçeklenebilir` (`scalable`) bir şekilde `dağıtıldığı`nı (`deployed`) kapsar.

*   **Ajanlar Arası İletişim (`A2A`):** Karmaşık görevleri çözmek veya birden fazla `uzmanlık alanı`nı (`domain expertise`) birleştirmek için birden fazla `agent`'ın `koordineli` (`coordinated`) bir şekilde nasıl iletişim kurduğu ve `işbirliği` (`collaboration`) yaptığı.
*   **Ajan Dağıtımı (`Agent Deployment`):** Geliştirilen `agent`'ların `üretim ortamları`nda güvenli, `ölçeklenebilir` ve yönetilebilir bir şekilde (örneğin, `bulut platformları`na (`cloud platforms`) veya `API servisleri` (`API services`) olarak) nasıl `dağıtılacağı` (`deployed`). `Rollout stratejileri`, `izleme` (`monitoring`) ve `sürüm kontrolü` (`version control`) gibi konular ele alınır.

Bu kapsamlı kurs, `junior` geliştiricilere yapay zeka `agent`'ları dünyasında sağlam bir `temel` (`foundation`) ve `pratik beceriler` (`practical skills`) kazandırarak, modern `AI uygulama`ları (`AI applications`) geliştirmeleri için gerekli `bilgi birikimi`ni (`knowledge`) sunmayı hedeflemektedir.
