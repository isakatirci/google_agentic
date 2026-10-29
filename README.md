# Google & Kaggle — 5 Günlük Agent Geliştirme Kursu

> Bu repo, Google ve Kaggle iş birliğiyle hazırlanan beş günlük agent geliştirme kursunun materyallerini barındırır. Bunların yanı sıra kurs kavramlarını **kendi projenizde** adım adım uygulamanızı sağlayan **skill** dosyaları da içerir.
>
> Hedef kitle: sıfırdan başlayan junior developer. Kavramı öğrenmek için kurs notlarını okuyun; kendi ajanınızı kurmak için skill'leri kullanın.

---

## İçindekiler

1. [Bu Repo Ne İşe Yarar?](#bu-repo-ne-işe-yarar)
2. [Depo Yapısı](#depo-yapısı)
3. [Kurulum](#kurulum)
4. [Skill Nedir ve Nasıl Kullanılır?](#skill-nedir-ve-nasıl-kullanılır)
5. [Skill Komutları — Sıfırdan Yayına Tam Yol Haritası](#skill-komutları--sıfırdan-yayına-tam-yol-haritası)
6. [İlk Gün İçin Hızlı Başlangıç](#ilk-gün-için-hızlı-başlangıç)
7. [Kurs İçeriği — Gün Gün](#kurs-içeriği--gün-gün)
8. [Temel Kavramlar Sözlüğü](#temel-kavramlar-sözlüğü)
9. [API Key Güvenliği](#api-key-güvenliği)

---

## Bu Repo Ne İşe Yarar?

"AI agent" derken kastettiğimiz şey, bir hedefe ulaşmak için **plan yapan, tool çağıran ve sonuçları değerlendiren** bir sistemdir. Tek turda metin üreten bir chatbot agent değildir.

Bu repo size iki şey sunar:

| Ne | Nerede |
|---|---|
| Kursun teorik altyapısı (notebook, ders notu, PDF) | `5-day-ai-agents/` |
| O teoriyi kendi projenizde uygulatan adım adım skill'ler | `.cursor/skills/` |

Skill'ler dersi bitirmek için değildir. **Kendi ajanınızı kurarken kullanılır.**

---

## Depo Yapısı

```
google_agentic/
│
├── 5-day-ai-agents/          # Kurs materyalleri
│   ├── Day1/                 # Agent temelleri ve mimariler
│   ├── Day2/                 # Tools ve MCP
│   ├── Day3/                 # Context Engineering, Session, Memory
│   ├── Day4/                 # Observability ve Evaluation
│   └── Day5/                 # A2A ve Deployment
│
├── .cursor/skills/           # Skill dosyaları (asıl rehber bunlar)
│   ├── google-agent-skills/  # Skill komutları index'i
│   ├── design-agent/         # /design-agent komutu
│   ├── defining-an-agent/    # Komutun açtığı asıl skill
│   ├── choose-agent-workflow/
│   ├── scaffold-adk-agent/
│   ├── add-agent-tools/
│   ├── connect-mcp/
│   ├── engineer-agent-context/
│   ├── add-agent-session/
│   ├── add-agent-memory/
│   ├── trace-agent/
│   ├── eval-agent/
│   ├── connect-a2a/
│   └── ship-agent/
│
├── GLOSSARY.md               # Terim sözlüğü — kodu yazarken yanınızda olsun
└── README.md                 # Bu dosya
```

> **Not:** `repomix-output.md` dosyası deponun tek metin haline getirilmiş kopyasıdır; yalnızca harici inceleme içindir. Git commit'e eklemeyin.

---

## Kurulum

### 1. Gereksinimler

- **Python 3.10+** — Daha eski sürümler uyumluluk sorunu çıkarır.
- **pip** — Python paket yöneticisi.

### 2. Sanal ortam oluşturun

Projenizin bağımlılıklarını sistemden izole etmek için her zaman virtual environment kullanın.

```bash
# Oluştur
python -m venv venv

# Etkinleştir (macOS / Linux)
source venv/bin/activate

# Etkinleştir (Windows PowerShell)
.\venv\Scripts\Activate.ps1
```

### 3. Gerekli paketleri yükleyin

```bash
pip install google-adk jupyterlab
```

### 4. API key'i ayarlayın

```bash
# macOS / Linux
export GOOGLE_API_KEY="buraya-anahtarinizi-yazin"

# Windows PowerShell
$env:GOOGLE_API_KEY = "buraya-anahtarinizi-yazin"
```

> **⚠️ ÖNEMLİ:** API key'i asla kod dosyasına yazmayın, asla GitHub'a push etmeyin. Her zaman ortam değişkeni (`environment variable`) olarak saklayın.

API key'inizi [Google AI Studio](https://aistudio.google.com) üzerinden edinebilirsiniz.

### 5. JupyterLab'i başlatın

```bash
jupyter lab
```

---

## Skill Nedir ve Nasıl Kullanılır?

Skill, bir AI agent'ın (Cursor, Antigravity vb.) belirli bir görevi nasıl yapacağını tarif eden yapılandırılmış bir talimat dosyasıdır. Bu projedeki skill'ler `.cursor/skills/` altındadır.

### İki tür dosya vardır

**1. Komut dosyası** — siz yazarsınız, agent açar ve asıl skill'e yönlendirir.

```
Örnekler: /design-agent, /scaffold-adk-agent, /ship-agent
```

Bu dosyaların başında `disable-model-invocation: true` yazar. Agent bu dosyayı kendiliğinden açmaz; siz tetiklersiniz.

**2. Asıl skill dosyası** — adımların ve `Done when` kontrol cümlelerinin bulunduğu yer.

```
Örnekler: defining-an-agent/SKILL.md, building-with-adk/SKILL.md
```

Her adımın sonunda `Done when` yazar. Bu cümle doğru olmadan o adım bitmemiş sayılır. Bir sonraki adıma atlamayın.

### Kural: Bir turda tek iş

Bir günde bütün komutları çalıştırmaya çalışmayın. Her komut bir davranıştır. `Done when` cümlesi sağlanınca durun. Bir sonraki iş için başka bir tur açın.

### Komutların tam listesi

`.cursor/skills/google-agent-skills/SKILL.md` dosyasını açın. Hangi komutu ne zaman yazacağınızı unutursanız oraya bakın.

---

## Skill Komutları — Sıfırdan Yayına Tam Yol Haritası

Aşağıdaki sıra, sıfırdan başlayıp çalışan bir agent'ı production'a çıkarmak için izlenecek yoldur. Her komut için şunlar yazılmıştır: **ne zaman yazarsınız**, **ne olur**, **ne zaman biter**.

---

### 🔵 OLUŞTURMA (Create)

---

#### 1. `/design-agent`

**Ne zaman yazarsınız:** Agent'ın ne olduğuna henüz karar vermediyseniz. Kod yazmadan önce gelir.

**Ne olur:** `defining-an-agent` skill'ini açar ve durur. O skill şu soruları sorar:
- Bu sistem gerçekten agent mı, yoksa tek turda metin dönen bir prompt mu?
- Seviyesi kaç? (1–4)
- Hangi model kullanılacak?
- Araç listesi ne?
- Sırayı kim seçiyor — model mi, deterministik bir workflow mu?
- Agent Ops ve güvenlik satırlarından hangisi bu agent'a değiyor?

**Ne zaman biter:** Aşağıdakiler yazılmış olmalıdır:
- [ ] Seviye
- [ ] Model
- [ ] Araç listesi
- [ ] Orkestrasyon seçimi
- [ ] Her güvenlik ve Agent Ops kararı ("Sonra bakarız" karar sayılmaz)

> **💡 İpucu:** Seviye 1 ile başlayın. Tek araç yeter. Seviye 4 (agent kendi kodunu değiştirir) ilk projeniz için uygun değildir.

---

#### 2. `/choose-agent-workflow`

**Ne zaman yazarsınız:** İki ayrı talimat veya iki ayrı araç seti varsa. Tek agent yetiyorsa bu komutu atlayın.

**Ne olur:** `choosing-agent-architecture` skill'ini açar. Hangi workflow şeklinin seçileceğini belirler:

| Durum | Seçim |
|---|---|
| Tek iş, tek araç seti | Tek agent yeter |
| Model hangi uzmanı çağıracağına kendisi karar verecek | Koordinatör + `AgentTool` |
| Sıra sabitse | `SequentialAgent` |
| İşler birbirine bağlı değilse | `ParallelAgent` + toplayıcı agent |
| Taslak → eleştiri → yeniden yazma döngüsü | `LoopAgent` (`max_iterations` ile) |

**Ne zaman biter:**
- [ ] Şeklin adı yazılmış
- [ ] Agent'lar ve `output_key` değerleri listelenmiş
- [ ] "Daha basit bir şekil seçilseydi ne bozulurdu?" sorusu yanıtlanmış

---

#### 3. `/scaffold-adk-agent`

**Ne zaman yazarsınız:** Tasarım bittikten sonra. Bu komut kodu kurar ve bir tur çalıştırır.

**Ne olur:** `building-with-adk` skill'ini açar. Skill şunları yapar:
- `google-adk` paketini yükler
- API key'i `GOOGLE_API_KEY` ortam değişkeninden okur (koda yazmaz)
- Gemini modeli için 429 hatasına karşı retry ayarı yapar
- Agent'a ad, talimat ve araç listesi ekler
- `InMemoryRunner` ile `run_debug` çağırır (geçici oturum — kalıcı session sonraki komutta)
- `adk web` ile tarayıcıda açar

**Ne zaman biter:**
- [ ] Ad, model, talimat, araç listesi, retry ayarı var
- [ ] `run_debug` veya `adk web` ile cevap dönmüş

> **İpucu:** Cevap geldiyse araca bakın. Araç çağrılmadıysa kontrol edilecek iki yer: (1) araç `tools` listesinde mi? (2) talimat ne zaman çağrılacağını söylüyor mu?

---

#### 4. `/add-agent-tools`

**Ne zaman yazarsınız:** Kendi fonksiyonunuzu ekleyecekseniz. İlk turdaki hazır search tool yetiyorsa erteleyin.

**Ne olur:** `designing-agent-tools` skill'ini açar. Araç türünü seçmenizi sağlar:

| Araç Türü | Ne Zaman |
|---|---|
| Function tool | Kendi Python fonksiyonunuz |
| `AgentTool` | Bir uzman agent'ı tool gibi çağırmak |
| MCP tool | Dış bir sunucunun tool'u |
| OpenAPI tool | Bir OpenAPI belgesinden otomatik üretim |
| Long-running tool + HITL | Yavaş işler veya insan onayı gereken adımlar |

Her araç için:
- Tip annotation yazılır
- Docstring ne zaman çağrılacağını açıklar
- Dönüş ya `status: success` ya `status: error` olur; hata metni model'e sonraki adımı söyler

**Ne zaman biter:**
- [ ] Her aracın görevi, şeması, kısa sonucu ve hata metni var

---

#### 5. `/connect-mcp`

**Ne zaman yazarsınız:** Tool, sizin fonksiyonunuz değil de başka bir programın tool'uysa.

**Ne olur:** `using-mcp` skill'ini açar. `McpToolset` ile sunucuya bağlar:
- Yerel sunucu → `stdio` taşıması
- Uzak sunucu → HTTP taşıması

> **⚠️ Güvenlik:** `tool_filter` kullanarak izin listesi oluşturun. Sunucunun tüm tool'larını açmayın. Kötü yazılmış bir tool açıklaması modeli yanlış yönlendirebilir — buna **tool shadowing** denir.

**Ne zaman biter:**
- [ ] Sunucu, taşıma yolu, izin listesi yazılmış
- [ ] `McpToolset` üzerinden bir çağrı başarılı dönmüş

---

### 🟡 GELİŞTİRME (Develop)

---

#### 6. `/engineer-agent-context`

**Ne zaman yazarsınız:** Agent çalışıyor ama context window şişiyor veya modele gereksiz metin gidiyorsa.

**Ne olur:** `engineering-agent-context` skill'ini açar. Modele gönderilen paketi bilinçli olarak seçmenizi sağlar:

Paket şu bloklardan oluşur:
- Talimat (instruction)
- Oturum geçmişi veya özeti
- State değerleri
- O tur için getirilen memory
- Tool şemaları ve son tool sonuçları
- Getirilen belgeler (retrieved documents)

Her blok ya pakettedir ya da neden dışarıda bırakıldığı yazılmıştır.

**Uzun konuşmalar için:** `compaction` eski turları özete çevirir. Geçmiş silinmez; modele giden metin kısalır.

**Ne zaman biter:**
- [ ] Paket listesi yazılmış
- [ ] Büyüyen konuşma için özet veya cache seçilmiş
- [ ] Hiçbir blok "eklemesi kolay diye" pakette değil

---

#### 7. `/add-agent-session`

**Ne zaman yazarsınız:** Konuşma ikinci turda sıfırlanıyorsa.

**Ne olur:** `managing-agent-sessions` skill'ini açar. `SessionService` seçtirir:

| Servis | Ne zaman |
|---|---|
| `InMemorySessionService` | Prototip — süreç kapanınca silinir |
| `DatabaseSessionService` | Kendi veritabanınızda kalıcı session |
| Agent Engine session | Google Cloud üzerinde yönetilen production session |

**State prefix kuralları:**

| Prefix | Kapsam |
|---|---|
| _(yok)_ | Yalnızca bu session |
| `temp:` | Yalnızca bu çağrı |
| `user:` | Aynı kullanıcının gelecek session'ları |
| `app:` | Tüm uygulama — başka kullanıcının verisini buraya koymayın |

**Ne zaman biter:**
- [ ] Servis adı yazılmış
- [ ] Her session uygulama + kullanıcı + session ID ile ayrılmış
- [ ] Her state anahtarının prefix'i ne kadar yaşayacağını belirtiyor

---

#### 8. `/add-agent-memory`

**Ne zaman yazarsınız:** Session bittikten sonra da hatırlanması gereken bir olgu varsa.

**Ne olur:** `persisting-agent-memory` skill'ini açar.

`memory_service` tek başına yetmez. `add_session_to_memory` ile oturumu belleğe eklemek gerekir. Agent'ın arama yapabilmesi için ayrıca bir araç da tanımlanır.

**İki yükleme stratejisi:**

| Strateji | Nasıl çalışır | Ne zaman |
|---|---|---|
| `load_memory` | Model aramayı kendisi kararlaştırır | Model aramayı unutabilir; ucuz |
| `preload_memory` | Her turdan önce otomatik arar | Token harcar; olgunun hep orada olmasını garantiler |

**İki memory servisi:**

| Servis | Nasıl çalışır |
|---|---|
| `InMemoryMemoryService` | Ham satırlarda kelime arar; süreç kapanınca gider |
| `VertexAiMemoryBankService` | Olguları sadeleştirir; süreç dışında durur |

**Ne zaman biter:**
- [ ] Bir olgu birinci session'da söylenmiş, ikinci session'da yalnızca memory sayesinde cevaplanmış
- [ ] Başka kullanıcı o olguyu göremiyor

---

#### 9. `/trace-agent`

**Ne zaman yazarsınız:** Cevap yanlışsa veya tool hiç çağrılmadıysa. Prompt'u büyütmeden önce yolu okuyun.

**Ne olur:** `observing-agents` skill'ini açar.

| Araç | Ne gösterir |
|---|---|
| Log | Bir anda ne olduğu |
| Trace | Adımların kronolojik sırası |
| Metric | Gecikme, hata oranı gibi sayılar |

Skill şunları yapar:
- Log seviyesini DEBUG'a çeker
- ADK web arayüzünün Events sekmesine bakar
- Model isteğini, tool listesini, çağrıyı ve sonucu bulur
- Kırılan adımı adlandırır

En sık karşılaşılan hata: araç `str` bekler ama talimat liste gönderir. Tip düzeltilir.

Uzun süreli çalışmalar için `LoggingPlugin` kullanılır (single callback değil — plugin tüm Runner'daki agent'ları, model ve tool çağrılarını görür).

**Ne zaman biter:**
- [ ] Kırılan adımın adı var
- [ ] Kanıt: bir log satırı veya bir trace olayı
- [ ] Production Runner'da `LoggingPlugin` veya eşdeğeri var

---

#### 10. `/eval-agent`

**Ne zaman yazarsınız:** Agent'a güvenmeden production'a çıkmadan önce.

**Ne olur:** `evaluating-agents` skill'ini açar.

Bir eval set'te üç şey durur:
1. Kullanıcının cümlesi
2. Beklenen son cevap
3. Beklenen tool çağrıları

**İki temel metrik:**

| Metrik | Ne ölçer |
|---|---|
| `tool_trajectory_avg_score` | Tool adı ve argüman uyumu |
| `response_match_score` | Metin benzerliği |

> `response_match_score` doğruluğun kanıtı değildir. Aynı doğru cevap farklı kelimelerle yazılabilir.

**Golden set:** Kullanıcı görmeden önce geçmesi gereken seçilmiş set. Production'da kırılan örnek bu sete eklenir.

Komut `adk eval` çalıştırır.

**Ne zaman biter:**
- [ ] En az bir eval var
- [ ] Beklenen tool'lar ve beklenen cevap yazılmış
- [ ] Eşikler sayısal
- [ ] Komut çalışmış veya doldurulmuş halde kişiye verilmiş

---

### 🟢 YAYINA ALMA (Ship)

---

#### 11. `/connect-a2a`

**Ne zaman yazarsınız:** Çağıracağınız agent başka bir servisteyse.

> Aynı programın içindeki uzman için bu komut gerekmez — onun için `AgentTool` veya `sub_agents` yeter.
> MCP bir **fonksiyon** çağırır. A2A bir **agent'a iş** verir. Bunları karıştırmayın.

**Ne olur:** `connecting-agents` skill'ini açar.
- Karşı taraf `to_a2a` ile kart yayınlar → adres `/.well-known/agent-card.json`
- Siz `RemoteA2aAgent` ile o karta bağlanırsınız
- Kendi session tablonuzu karşı tarafa vermezsiniz — bir görev verirsiniz, bir sonuç alırsınız

**Ne zaman biter:**
- [ ] Uzak agent'ın kart adresi var
- [ ] İstemci `RemoteA2aAgent` veya eşdeğeri
- [ ] Özel olay günlüğü, görevin kendisi değil

---

#### 12. `/ship-agent`

**Ne zaman yazarsınız:** Agent yerelde çalışıyor ve eval set'i geçiyorsa. Geçmeden yayın olmaz.

> Prompt değişikliği de bir sürümdür. Tool şemasındaki değişiklik de bir sürümdür.

**Ne olur:** `deploying-agents` skill'ini açar.

Paket bir klasördür. `agent.py` içinde `root_agent` bulunur. Bağımlılıklar sabit olmalıdır.

`GOOGLE_GENAI_USE_VERTEXAI` değişkenini bilerek seçin:
- `1` → Vertex AI
- `0` → AI Studio

**Üç deployment hedefi:**

| Platform | Ne zaman |
|---|---|
| Vertex AI Agent Engine | Yönetilen agent runtime — tavsiye edilen |
| Cloud Run | Küçük servis |
| GKE | Çok agent'lı küme, fazla kontrol |

> Deneme deployment'larını silin. `min_instances=0` kullanılmayan kopyayı kapatır. Silme işlemi için "listedeki ilk" değil, az önce kurduğunuz kaynağın adını kullanın.

**Ne zaman biter:**
- [ ] Klasör tam
- [ ] Deployment hedefi yazılmış
- [ ] Bu sürümün eval sonucu var
- [ ] Deneme deployment sıfıra iner veya silinir

---

## İlk Gün İçin Hızlı Başlangıç

Sıfırdan çalışan bir agent'a ulaşmak için bu sırayı izleyin:

```
1. Python sanal ortamını açın
   → google-adk yüklü olsun
   → GOOGLE_API_KEY tanımlı olsun (koda yazmayın)

2. GLOSSARY.md dosyasını bir kez okuyun
   → Kod yazarken oraya dönün

3. /design-agent yazın
   → Seviye 1 seçin, tek araç seçin

4. İkinci agent gerekmiyorsa /choose-agent-workflow yazmayın

5. /scaffold-adk-agent yazın
   → Bir tur cevap gelene kadar bu komutta kalın

6. Cevabı okuyun — araç çağrıldı mı kontrol edin

7. Ancak bundan sonra /add-agent-tools yazın
```

**Memory, session ve deployment bu ilk yolun dışındadır. Her biri ayrı bir turdur.**

---

## Kurs İçeriği — Gün Gün

Her gün için: iki Jupyter notebook + bir ders notu (Markdown ve PDF).

> **⚠️ Notebook çalıştırma kuralı:** Hücreleri yukarıdan aşağıya, tek tek çalıştırın. Tüm hücreleri aynı anda çalıştırmayın — API rate limit'e takılırsınız ve hata kaynağını bulmak zorlaşır.

---

### Gün 1 — Agent'a Giriş ve Temel Mimariler

**Dosyalar:** `day-1a-from-prompt-to-action.ipynb`, `day-1b-agent-architectures.ipynb`, `Introduction to Agents.md`

Agent nedir, nasıl çalışır, kaç seviyesi vardır?

| Seviye | Açıklama |
|---|---|
| 0 | Yalnızca model — metin üretir, dış dünyaya dokunmaz |
| 1 | Tool kullanır (web search, hesap makinesi, takvim API) |
| 2 | Çok adımlı planlama yapar |
| 3 | Uzman agent takımları ile işbirliği yapar |
| 4 | Kendi kodunu değiştirebilir — **ilk projeniz için seçmeyin** |

Günün konuları:
- İlk agent'ı kurma ve basit search tool entegrasyonu
- Tek agent vs. çok agent takım yapıları
- `SequentialAgent`, `ParallelAgent`, `LoopAgent` mimarilerini karşılaştırma

---

### Gün 2 — Agent Tool'ları ve MCP

**Dosyalar:** `day-2a-agent-tools.ipynb`, `day-2b-agent-tools-best-practices.ipynb`, `Agent Tools & Interoperability with MCP.md`

Agent'ların dış sistemlerle nasıl etkileşim kurduğunu öğrenin.

- Tool tasarımı, tip annotation, docstring, hata yönetimi
- **Model Context Protocol (MCP):** Farklı servisler ve tool'lar arasında standart iletişim protokolü
- MCP sunucusuna `McpToolset` ile bağlanma
- Tool shadowing riski ve `tool_filter` ile önlenmesi

---

### Gün 3 — Context Engineering, Session ve Memory

**Dosyalar:** `day-3a-agent-sessions.ipynb`, `day-3b-agent-memory.ipynb`, `Context Engineering: Sessions & Memory.md`

Agent'ın bağlamı nasıl yönettiğini öğrenin.

- **Context Engineering:** Modele o turda gönderilecek paketin seçilmesi
- **Session:** Bir kullanıcının bir agent ile tek konuşması; event ve state barındırır
- **State prefix'leri:** `temp:` → `user:` → `app:` kapsam hiyerarşisi
- **Memory:** Session bittikten sonra kalan olgular; `load_memory` vs. `preload_memory`
- **Compaction:** Context window dolmadan özet tutma

---

### Gün 4 — Agent Kalitesi: Observability ve Evaluation

**Dosyalar:** `day-4a-agent-observability.ipynb`, `day-4b-agent-evaluation.ipynb`, `Agent Quality.md`

Production'da çalışan agent'ları nasıl izlersiniz ve değerlendirirsiniz?

- **Observability:** Log, trace, metric — runtime davranışını izleme ve analiz etme
- **Evaluation:** Eval set oluşturma, `tool_trajectory_avg_score`, `response_match_score`
- **Golden set:** Her production değişikliğinin geçmesi gereken seçilmiş test seti
- **Agent Ops:** Değişiklikleri ölçme, trace etme ve kapı koyma disiplini

---

### Gün 5 — Prototipten Production'a: A2A ve Deployment

**Dosyalar:** `day-5a-agent2agent-communication.ipynb`, `day-5b-agent-deployment.ipynb`, `Prototype to Production.md`

Geliştirilen agent'ları gerçek dünyaya nasıl çıkarırsınız?

- **A2A (Agent2Agent):** Farklı servislerdeki agent'lar arasında görev devretme standardı
- **Agent card:** `/.well-known/agent-card.json` adresinde yayınlanan JSON kimlik kartı
- **Deployment hedefleri:** Vertex AI Agent Engine, Cloud Run, GKE
- Rollout stratejileri, monitoring, version control

---

## Temel Kavramlar Sözlüğü

Bu terimler `GLOSSARY.md` dosyasından alınmıştır. Kod yazarken ve prompt hazırlarken bu kelimeleri kullanın.

| Terim | Açıklama |
|---|---|
| **Agent** | Planlayan, tool çağıran ve sonuçları değerlendirerek hedefe ulaşan sistem. Tek turda metin üreten prompt değil. |
| **Model** | Agent'ın beyni. Canlı veri çekmez, dünyayı değiştirmez — sadece akıl yürütür. |
| **Tool** | Modelin çağırdığı fonksiyon. Agent'ın elleri. Bilgi getirir veya eylem gerçekleştirir. |
| **Orchestration** | Sonraki düşüncenin, çağrının veya durmanın kararını veren döngü. Sinir sistemi. |
| **Trajectory** | Cevaba ulaşmak için izlenen tüm düşünce, tool çağrısı ve gözlemlerin kronolojik kaydı. |
| **Session** | Bir kullanıcı ile bir agent arasındaki tek sürekli konuşma. Başka kullanıcıyla paylaşılmaz. |
| **Event** | Session içindeki tek kayıt: kullanıcı girişi, model çıktısı, tool çağrısı veya tool sonucu. |
| **State** | Session'ın kısa süreli not defteri. Tool'ların ve sub-agent'ların okuyabildiği key-value çiftleri. Memory değildir. |
| **Memory** | Session bittikten sonra saklanan, sonraki session'da getirilen olgular. |
| **Context Engineering** | Modele o turda gönderilecek paketin bilinçli olarak seçilmesi: talimat, geçmiş, state, memory, tool sonuçları. |
| **Compaction** | Eski event'ların daha kısa özetle değiştirilmesi — context window'u aşmadan önemli bilgiyi korur. |
| **AgentTool** | Uzman bir agent'ın tool gibi sarılması. Çağıran agent kontrolde kalır, sonuç ona döner. |
| **Workflow agent** | Deterministik yönlendirici: `SequentialAgent`, `ParallelAgent`, `LoopAgent`. Sırayı prompt değil kod belirler. |
| **MCP** | Model Context Protocol. Agent'ın dış sunucudaki tool'ları ve kaynakları çağırmak için kullandığı standart. |
| **A2A** | Agent2Agent. Bir agent'ın kendi servisi olan başka bir agent'a görev devretmek için kullandığı standart. |
| **Agent card** | A2A agent'ının `/.well-known/agent-card.json` adresinde yayınladığı JSON kimlik kartı. |
| **Agent Ops** | Agent değişikliklerini ölçme, trace etme, değerlendirme ve kapı koyma disiplini. |
| **Eval set** | Beklenen son cevap ve beklenen tool çağrılarıyla kayıt altına alınmış konuşmalar seti. |
| **Golden set** | Bir değişikliğin kullanıcıya ulaşmadan geçmesi gereken seçilmiş eval set'i. Production hataları buraya eklenir. |
| **HITL** | Human in the Loop. Bir kişinin eylemi onaylaması veya otomatik bir yargıcın rubric'ini yazması. |

---

## API Key Güvenliği

Bu kursta Google Gemini API kullanılır. API key güvenliği zorunludur:

✅ **Yapın:**
- Key'i `GOOGLE_API_KEY` ortam değişkeni olarak saklayın
- `.env` dosyası kullanıyorsanız `.gitignore`'a ekleyin
- Kaggle notebook'larında `UserSecretsClient` kullanın

❌ **Yapmayın:**
- Key'i Python dosyasına yazmayın (`api_key = "AIza..."`)
- Key'i GitHub'a push etmeyin
- `adk web` adresindeki token'ı paylaşmayın

```python
# ✅ Doğru
import os
api_key = os.environ.get("GOOGLE_API_KEY")

# ❌ Yanlış
api_key = "AIzaSyXXXXXXXXXXXXXXXXXXXXXXX"
```

---

*Bu repo, Apache 2.0 lisansı altında Google LLC tarafından hazırlanan Kaggle 5-day Agents notebook'larından uyarlanmıştır.*
