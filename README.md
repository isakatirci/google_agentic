# Sıfırdan ajana

Bu depo, Google ve Kaggle’ın 5 günlük ajan kursunu ve onun yanında kullanılan skill dosyalarını tutar.

Skill, ajana verilen kısa bir iş tarifidir. Siz bir komut yazarsınız. Ajan o tarifi açar ve adımları uygular.

Kurs İngilizcedir. Bu sayfa Türkçedir. Terimleri İngilizce bırakır. Kodda da o kelimeler geçer.

İlk kez deniyorsanız buradan okuyun. Her cümle tek bir iş söyler. Acele etmeyin. Bir günü bitirmeden sonrakine geçmeyin.

## Bu depoda ne var

Üç tür dosya vardır.

Kurs, `5-day-ai-agents` klasöründedir. Her günün not defteri vardır. Not defteri, hücre hücre çalışan örnektir. Her günün ders notu da vardır. Ders notu, PDF’den Markdown’a çevrilmiştir. PDF de durur. Okumak için Markdown daha kolaydır.

Sözlük, kökteki `GLOSSARY.md` dosyasıdır. Herkes aynı kelimeyi aynı anlamda kullanır. Skill dosyaları bu sözlüğe uyar.

Skill dosyaları, `.cursor/skills` klasöründedir. Bunlar kursu bitirmek için değildir. Kendi projenizde ajan kurmak içindir.

`repomix-output.md` diye bir dosya görürseniz onu commit etmeyin. O dosya, bütün deponun tek metin halinde kopyasıdır. İnceleme içindir. Kaynak değildir.

## Bilgisayarınızda ne olmalı

Python 3.10 veya daha yenisi gerekir. Bu makinede 3.14 da çalışır.

Bir terminal açın. Proje klasörüne gidin.

```powershell
cd c:\Users\isa\google_agentic
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install jupyterlab google-adk
```

`Activate.ps1` hata verirse PowerShell’de betik çalıştırmaya izin yoktur. O pencere için şunu bir kez çalıştırın. Sonra etkinleştirmeyi tekrar deneyin.

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

Sanal ortam açıksa satırın başında `(.venv)` görürsünüz. Paketler bu ortama iner. Bilgisayarınızın genel Python’una karışmaz.

## Anahtar

Ajan, Gemini modelini çağırır. Bunun için bir anahtar gerekir.

Anahtarı Google AI Studio’dan alın: https://aistudio.google.com/app/api-keys

Anahtarı koda yazmayın. Dosyaya yapıştırıp GitHub’a göndermeyin. Ortam değişkeni olarak verin.

PowerShell’de, o pencere kapanana kadar geçerli olan yol budur.

```powershell
$env:GOOGLE_API_KEY = "buraya-anahtar"
```

Kalıcı tutmak isterseniz bir `.env` dosyası kullanın. `.env` zaten yok sayılır. Yine de `git status` çıktısında görünmediğinden emin olun.

Kaggle’da çalışıyorsanız anahtarın adı `GOOGLE_API_KEY` olan bir sırdır. Not defterleri bunu `UserSecretsClient` ile okur.

Anahtar olmadan kurulum biter. İlk gerçek cevap anahtarla gelir.

## Bir sohbet ile bir ajan aynı şey değildir

Bir sohbet kutusu tek turda metin döner. Siz sorarsınız. Model yazar. İş biter.

Bir ajan hedefe kadar gider. Düşünür. Bir araç çağırır. Aracın sonucunu okur. Gerekirse tekrar çağırır. Sonra durur.

Örnek düşünün. “Yarın İstanbul’da yağmur var mı?” sorusu tek başına bir ajan değildir. Model bunu kendi hafızasından uydurabilir.

Aynı soru bir ajan olur, şu üç parça varsa.

Model, ne yapacağına karar verir. Bu beyindir. Canlı hava durumunu kendisi çekmez.

Araç, hava durumunu bir servisten getirir. Bu eldir. Araç ya bilgi getirir ya da bir iş yapar. İkisini birden karma karışık yapmaz.

Orkestrasyon, sıradaki adımı seçer. Düşün. Çağır. Sonucu gör. Dur. Bu sinir sistemidir.

Bu üçü yoksa elinizde yalnızca bir prompt vardır. Prompt, modele yazdığınız talimattır.

## Kelimeler

Bu kelimeler kursun her yerinde geçer. Ezberlemeyin. Bir ajan yazarken geri dönün.

**Agent.** Hedefe varana kadar plan yapan, iş yapan ve sonucu kullanan sistemdir.

**Model.** Karar veren çekirdektir. Bu depodaki örnekler `gemini-2.5-flash-lite` kullanır. Model tek başına dosya silmez ve arama yapmaz.

**Tool.** Modelin çağırabildiği fonksiyondur. Ücret bakmak bir araçtır. Sipariş vermek başka bir araçtır. Her aracın bir görevi vardır.

**Orchestration.** Bir sonraki adımı seçen döngüdür. Model serbest seçebilir. Ya da sıra önceden sabittir.

**Trajectory.** Cevaba giden bütün yoldur. Düşünceler, araç çağrıları ve dönen sonuçlar bu yoldadır. Son cümle yolun yalnızca son adımıdır. Notu yalnızca son cümleye vermeyin.

**Session.** Bir kullanıcının bir ajanla süren tek konuşmasıdır. Başka kullanıcıyla paylaşılmaz. Aynı kullanıcı yeni bir oturum açarsa bu, yeni bir konuşmadır.

**Event.** Oturumdaki tek satırdır. Sizin cümleniz bir satırdır. Modelin cevabı bir satırdır. Araç çağrısı bir satırdır. Aracın dönüşü bir satırdır.

**State.** O oturumun karalama defteridir. Anahtar ve değer tutar. Örnek: `user:name` = `Ayşe`. Konuşma bitince bu defterin ömrü biter. Bu, bellek değildir.

**Memory.** Oturum kapandıktan sonra duran olgudur. Yarınki oturum onu geri getirebilir. “Ayşe çayı şekersiz içer” bir bellektir. Bunu belleğe yazmazsanız yarın ajan hatırlamaz.

**Context engineering.** Bu turda modele giden paketi kurmaktır. Pakette talimat, geçmiş, state, bellek, araç sonuçları ve belgeler vardır. Hepsini yığmak iyi paket değildir. Her parçanın orada bir nedeni olmalıdır.

**Compaction.** Eski turları kısa bir özete çevirmektir. Pencere dolmasın diye yapılır. Geçmiş silinmez. Modele giden metin kısalır.

**AgentTool.** Bir uzman ajanı, araç gibi sarmaktır. Çağıran ajan kontrolde kalır. Uzmanın sonucu ona geri döner. Konuşmayı uzmana teslim etmez.

**Workflow agent.** Sırayı prompt ile seçmeyen yöneticidir. Üçü vardır. `SequentialAgent` işi sabit sırada yapar. `ParallelAgent` bağımsız işleri birlikte yapar. `LoopAgent` bir işi, onay veya üst sınıra kadar tekrarlar.

**MCP.** Model Context Protocol. Ajanın, dış bir sunucudaki araçları çağırması için ortak dildir. Her sistem için ayrı istemci yazmazsınız.

**A2A.** Agent2Agent. Bir ajanın, başka bir serviste çalışan ajana iş vermesidir. MCP bir fonksiyon çağırır. A2A bir ajana iş verir. İkisini karıştırmayın.

**Agent card.** A2A ajanının kimlik kartıdır. Adres `/.well-known/agent-card.json` olur. Kartta ad, açıklama ve beceriler vardır.

**Agent Ops.** Ajanı yazılım gibi işletmektir. Ölçersiniz. İzi okursunuz. Not verirsiniz. Kötü not varsa kullanıcıya çıkmaz.

**Eval set.** Kayıtlı denemelerdir. Her denemede kullanıcı cümlesi, beklenen cevap ve beklenen araç çağrıları vardır.

**Golden set.** Yayın öncesi geçmesi zorunlu olan seçilmiş deneme kümesidir. Üretimde bozulan bir örnek bu kümeye eklenir.

**HITL.** Human in the loop. İnsan döngüdedir. Geri alınması zor bir işte kişi onaylar. Ya da otomatik hakemin kullanacağı ölçütü kişi yazar.

## Beş gün

Her gün iki not defteri ve bir ders notu içerir. Not defterini yukarıdan aşağı, hücre hücre çalıştırın. “Hepsini çalıştır” demeyin. Kota dolabilir. Hata olursa hangi hücrede olduğunu görürsünüz.

Kaggle’da not defteri hazır gelir. ADK orada yüklüdür. Kendi bilgisayarınızda yukarıdaki kurulumu yapın.

### Gün 1. Ajan nedir

Dosyalar `5-day-ai-agents/Day1` içindedir.

`Introduction to Agents.md` ders notudur. Ajanın parçalarını anlatır. Seviyeler buradadır.

Seviye 0 yalnızca modeldir. Tek turda metin yazar.

Seviye 1 araç kullanır. Bir seferde bir iş yapar.

Seviye 2 çok adımlı plan kurar.

Seviye 3 birkaç uzman ajanı bir araya getirir.

Seviye 4 kendi ajanlarını değiştiren araştırma hedefidir. Ürün işi için burada durmayın. Kurs bunu ürün şablonu saymaz.

`day-1a-from-prompt-to-action.ipynb` ilk ajanı kurar. Arama aracı ekler. Bir tur çalıştırır.

`day-1b-agent-architectures-eb9049.ipynb` tek ajan ile takımı ayırır. Sabit sıra, paralel iş ve döngü buradadır.

Bu günün sonunda şunu söyleyebilmelisiniz. Bu sistem ajan mı, yoksa tek prompt mu. Kaçıncı seviyede. Hangi model. Hangi araçlar. Sırayı kim seçiyor.

### Gün 2. Araç ve MCP

Dosyalar `5-day-ai-agents/Day2` içindedir.

Ders notu `Agent Tools & Interoperability with Model Context Protocol (MCP).md` dosyasıdır.

`day-2a-agent-tools.ipynb` fonksiyon aracı, uzman ajanı araç yapma ve kod çalıştırma örnekleridir.

`day-2b-agent-tools-best-practices-2af7d1.ipynb` iyi araç yazmayı anlatır. Ad, şema, yan etki ve hata metni burada önemlidir.

İyi bir araç şunları yapar.

Görevi tek cümleyle söyler. Argümanların tipi yazılır. Dönüş ya başarıdır ya da hatadır. Hata metni, modele sonraki adımı söyler. Geri alınması zor işte kişiye sorar. Bu, HITL’dir.

MCP ile dış sunucuya bağlanırsınız. `tool_filter` izin listesidir. Sunucunun bütün araçlarını açmayın. Araç metni de modelin okuduğu metindir. Kötü bir metin, modeli yanlış işe itebilir.

### Gün 3. Oturum, bağlam ve bellek

Dosyalar `5-day-ai-agents/Day3` içindedir.

Ders notu `Context Engineering_ Sessions & Memory.md` dosyasıdır.

`day-3a-agent-sessions (1).ipynb` oturumu tutar. Dosya adındaki `(1)` bilerek durur. Yeniden adlandırmayın. Kurs böyle gelmiştir.

`day-3b-agent-memory.ipynb` oturum bitince kalan olguyu yazar ve sonra arar.

Üç raf vardır. Karıştırmayın.

State, bu konuşmanın karalama defteridir.

Session, bu konuşmanın bütün satırlarıdır.

Memory, yarın da lazım olan olgudur.

Uzun konuşma pencereyi doldurur. Compaction eski satırları özete çevirir. State ile bellek bunun yerine geçmez.

`load_memory` modeli aramaya bırakır. Ucuzdur. Model aramayı unutabilir.

`preload_memory` her turdan önce arar. Olgu oradadır. Token her tur harcanır.

Başlangıçta `load_memory` yeter. Unutulan bir olgu pahalıya patlıyorsa `preload_memory` kullanın.

### Gün 4. İz ve not

Dosyalar `5-day-ai-agents/Day4` içindedir.

Ders notu `Agent Quality.md` dosyasıdır.

`day-4a-agent-observability.ipynb` log, iz ve ADK web arayüzünü gösterir.

`day-4b-agent-evaluation.ipynb` deneme kümesi ve `adk eval` komutunu gösterir.

Ajan yanlış cevap verince prompt’u hemen değiştirmeyin. Önce yolu okuyun. Yol, trajectory’dir.

Log, bir anda ne olduğunu söyler.

Trace, adımların sırasını söyler. Hangi araç, hangi argüman, ne döndü.

Metric, gecikme ve hata oranı gibi sayılardır.

Bir deneme kümesinde üç şey durur. Kullanıcının cümlesi. Beklenen son cevap. Beklenen araç çağrıları.

`tool_trajectory_avg_score` araç adı ve argüman uyumuna bakar.

`response_match_score` metin benzerliğine bakar. Doğruluğun kanıtı değildir. Aynı doğru cevap başka kelimelerle yazılabilir.

Golden set, kullanıcı görmeden önce geçmesi gereken kümedir. Üretimde kırılan bir örnek bu kümeye eklenir. Bir sonraki sürüm aynı yerden kırılmasın diye.

### Gün 5. Başkasının ajanı ve yayın

Dosyalar `5-day-ai-agents/Day5` içindedir.

Ders notu `Prototype to Production.md` dosyasıdır.

`day-5a-agent2agent-communication.ipynb` A2A ile uzak ajanı çağırır.

`day-5b-agent-deployment.ipynb` prototipi servise taşır.

Aynı programın içindeki uzman için `AgentTool` veya `sub_agents` yeter.

Başka bir sürecin ajanı için A2A kullanılır. Karşı taraf bir kart yayınlar. Siz `RemoteA2aAgent` ile o karta bağlanırsınız.

Yayın kapısı vardır. Yeni sürüm, golden set’i geçmeden kullanıcıya çıkmaz. Prompt değişikliği de sürümdür. Araç şemasındaki değişiklik de sürümdür.

Küçük bir trafik payına çıkın. İzleri okuyun. Bozulursa önceki sürüme dönün. Yeni bozulmayı deneme kümesine ekleyin.

Bu depodaki örnekler önce yerelde çalışır. Vertex AI Agent Engine, Cloud Run ve GKE üretim seçenekleridir. Fatura açılmasın diye deneme dağıtımını silin. `min_instances` değerini 0 yapmak, kullanılmayan kopyayı kapatır.

## Skill dosyaları nasıl çalışır

`.cursor/skills` altındaki her klasör bir tariftir. Tarifin adı `SKILL.md` dosyasındadır.

İki tür vardır.

Birincisi sizin yazdığınız komuttur. Sohbette `/design-agent` gibi yazarsınız. Bu dosya işi yapmaz. Asıl tarife işaret eder ve durur. Başında `disable-model-invocation: true` yazar. Ajan bu dosyayı kendiliğinden açmaz. Siz çağırırsınız.

İkincisi asıl tariftir. Adımlar oradadır. Her adımın sonunda `Done when` vardır. Bu, işin bittiğini söyleyen kontrol cümlesidir. Ajan, görev o tarife uyunca bu dosyayı kendisi açabilir.

Uzun tablolar `reference.md` içindedir. Ana tarif kısa kalsın diye oraya konmuştur.

Komutların listesi `.cursor/skills/google-agent-skills/SKILL.md` dosyasındadır. Takıldığınızda oraya bakın.

### Kurma

| Ne yapmak üzeresiniz | Ne yazarsınız | Asıl tarif |
| --- | --- | --- |
| Ajanın ne olduğuna karar vermek | `/design-agent` | `defining-an-agent` |
| Takımın şeklini seçmek | `/choose-agent-workflow` | `choosing-agent-architecture` |
| Kodu yazıp bir tur çalıştırmak | `/scaffold-adk-agent` | `building-with-adk` |
| Araç eklemek | `/add-agent-tools` | `designing-agent-tools` |
| MCP sunucusuna bağlanmak | `/connect-mcp` | `using-mcp` |

### Geliştirme

| Ne yapmak üzeresiniz | Ne yazarsınız | Asıl tarif |
| --- | --- | --- |
| Modele bu turda ne gideceğine karar vermek | `/engineer-agent-context` | `engineering-agent-context` |
| Konuşmayı turlar arasında tutmak | `/add-agent-session` | `managing-agent-sessions` |
| Oturum bitince olguyu saklamak | `/add-agent-memory` | `persisting-agent-memory` |
| Bir hatanın yolunu okumak | `/trace-agent` | `observing-agents` |
| Yolu notlamak | `/eval-agent` | `evaluating-agents` |

### Yayın

| Ne yapmak üzeresiniz | Ne yazarsınız | Asıl tarif |
| --- | --- | --- |
| Başka servisteki ajanı çağırmak | `/connect-a2a` | `connecting-agents` |
| Kapıdan geçirip dağıtmak | `/ship-agent` | `deploying-agents` |

Bu komutlar, proje `.cursor/skills` klasörünü görüyorsa çalışır. Başka bir projede kullanmak için bu klasörü o projenin içine kopyalayın. Aynı yollar kalsın. Sonra o projeyi Cursor’da açın.

## Sıfırdan ilk çalışan ajana

Bu yol, Gün 1’in küçültülmüş halidir. Her adımı bitirin. Sonrakine ondan sonra geçin.

1. Sanal ortamı açın. `google-adk` yüklü olsun. `GOOGLE_API_KEY` tanımlı olsun.

2. `GLOSSARY.md` dosyasını bir kez okuyun. Anlamadığınız kelimeyi işaretleyin. Kod yazarken oraya dönün.

3. Cursor sohbetinde `/design-agent` yazın. Ajanın seviyesini, modelini, araçlarını ve orkestrasyonunu söyleyin. Seviye 1 ile başlayın. Tek araç yeter.

4. İş tek bir ajanın işiyse `/choose-agent-workflow` yazmayın. İki ayrı talimat veya iki ayrı araç seti varsa yazın. Sabit sıra gerekiyorsa `SequentialAgent` seçilir. “Önce şunu yap, sonra bunu” cümlesi sıra garantisi değildir.

5. `/scaffold-adk-agent` yazın. Bitti sayılması için şunlar gerekir. Ajanın adı vardır. Modeli vardır. Talimatı vardır. Araç listesi vardır. 429 hatalarına karşı yeniden deneme ayarı vardır. `run_debug` veya `adk web` ile bir tur cevap dönmüştür.

6. Cevap geldiyse durup okuyun. Araç çağrıldı mı. Çağrılmadıysa iki yere bakın. Araç `tools` listesinde mi. Talimat, ne zaman çağrılacağını söylüyor mu.

7. Ancak ondan sonra `/add-agent-tools` ile kendi fonksiyonunuzu ekleyin. Dönüş sözlüğü `status` taşısın. Hata durumunda `error_message` bir sonraki adımı söylesin.

İlk turda yayın, bellek ve A2A yoktur. Onlar sonraki günlerin işidir. İlk tur “merhaba” deyip arama yapabilen küçük bir ajandır. Bu, çalışan bir temeldir.

## Bozulunca ne yaparsınız

Prompt’u hemen büyütmeyin. `/trace-agent` yazın.

Log seviyesini DEBUG yapın. ADK web arayüzünde Events sekmesine bakın. Model isteğini, araç listesini, çağrıyı ve sonucu bulun. Kırılan adımı adlandırın.

Sık kırılma şudur. Araç `str` bekler. Talimat liste gönderir. İz, yanlış çağrıyı gösterir. Tipi düzeltin.

`adk web` açıkken aynı porta ikinci sunucu başlamaz. Önce eskisini kapatın.

Kaggle’da `adk web` bir vekil adres verir. O adreste jeton vardır. Adresi kimseyle paylaşmayın.

## Kendi projenize taşıma

Yeni bir klasör açın. Sanal ortam kurun. Anahtarı ortam değişkenine koyun.

Bu depodaki `.cursor/skills` klasörünü yeni klasöre kopyalayın. `GLOSSARY.md` dosyasını da kopyalayın. Skill’ler o sözlüğe güvenir.

Cursor’da yeni klasörü açın. `/design-agent` ile başlayın. Sonra `/scaffold-adk-agent`.

Kurs not defterlerini yeni projeye taşımayın. Onlar ders örneğidir. Sizin ajanınız kendi `agent.py` dosyasında durur.

Bir davranışı bitirin. Sonra ikinci davranışı ekleyin. Aynı günde bellek, değerlendirme ve yayın eklemeyin. Her biri ayrı bir turdur. Skill’ler de böyle bölünmüştür.

## Sık takıldığınız yerler

429 hatası, çok sık istek attığınızı söyler. Örneklerdeki yeniden deneme ayarını kullanın. Hücreleri art arda, beklemeden çalıştırmayın.

Boş araç listesi, modelin elinin olmadığını söyler. `tools=[...]` satırını kontrol edin.

Ajan yarını hatırlamıyorsa bellek yazılmamıştır. `memory_service` tek başına yetmez. Oturumu belleğe eklemek ve bir arama aracı vermek de gerekir.

İki kullanıcı aynı olguyu görüyorsa oturum veya bellek karışmıştır. Her oturum uygulama, kullanıcı ve oturum kimliği ile ayrılır.

“Önce A, sonra B” bazen atlanır. Sıra şartsa workflow agent kullanın.

Onaylı sipariş ikinci çalıştırmada sıfırdan başlıyorsa aynı çağrı sürdürülmemiştir. `designing-agent-tools` içindeki devam bloğunu izleyin. Yeni bir koşu, eski onayı getirmez.

## Ne gönderilmez

GitHub’a şunları koymayın.

`.env` ve içindeki anahtar.

`repomix-output.md`.

Sanal ortam klasörü `.venv`.

Kaggle vekil adresi ve jeton.

Başkasının oturum verisi.

Commit etmeden önce `git status` çalıştırın. Anahtar görünen bir dosya listede varsa onu çıkarın.

## Lisans

Not defterleri Apache 2.0 ile gelir. Telif 2025 Google LLC’dir. Türev işte bu başlığı saklayın.

Skill metinleri bu not defterlerinden uyarlanmış çalışma kurallarıdır. Dersin yerine geçmez. Ders notlarının uzun metnini başka yere olduğu gibi kopyalamayın. Kısa kod örneği ve iş tarifi yeter.

Kursun kendi açıklaması `5-day-ai-agents/Readme.md` dosyasındadır. O dosya İngilizcedir. Bu sayfa, ilk kez deneyen biri için Türkçe yürüyüş yoludur.
