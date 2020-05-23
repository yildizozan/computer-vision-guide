# Visual Motion

Visual motion'da sahne veya kamera haketli olabili hatta ikiside hareketli olabilir.

Görüntü üzerindeki hareketlerden yola çıkarak:
- Bu bilgileri kullanarak 
- Cisimleri hızları hakkında
- Bir sahnede neyin değişip değişmediği hakkında
etc..

### Structure from motion
Hareket bilgisinden yola çıkarak sahnenin 3D yapısı hakkında bir şeyler söyleyebilir miyiz ve sahne ve kamera hakkında bir şeyler söyleyebilir miyiz onunda çıkarımlar yapacağız.

### Recognition by motion
Bu hareket sistemini bir şeyi şeyi tanımada çok kullanırız. Örneğin uzaktan gelen birini yüzünden tanıyabiliriz.

MPEG v7 Bir hareketin bilgisi sıkıştırma konusunda bilgi verirmiş.

<Random Dot Example>
<h5>asd</h5>

## Image Sequence

Genel durumlarımız

Bu görüntüler alınırken:
- Kamera yerinden oynamış olabilir
- Kamera yerinden oynamış olabilir (rotation)
- Transation olabilir

> Tüm bunlar olurkan ışık değişmediğini varsayıyoruz.

## Visual Motion

Sadece hareket 
Örneğin bir robotumuz var ve duvara gidiyor. Duvarla roboy arasında `d` kadar mesafe varsa ve hızımızı biliyorsak yani duvara 100 metre var ve hızımız 20 m/s bu durumda 100/20=5 saniye sonra çarparız. Eğer cisimin ne kadar uzakta oludğunu bilmiyorsak hızımızı da bilmiyorsak ne yapacağız? İşte bunu çözmek için duvara bir yuvarlak çizdiğimizi düşünelim, robot duvara yaklaştıkça yuvarlak büyüyecektir. Biz bu yuvarlağın büyümesinden ne zaman çarpacağımızı bulabiliriz.

![](assets\motion-collision.png)

L büyüklüğünde bir obje çarpacağa yere V hızında yaklaşıyor. D(t) fonksiyonu `t` anında objenin çarpacağa yere olan uzaklığını biliyor. `D(t)=D0-vt`  formülü `0` eşit olduğunda çarpacak demektir buradan `T=D0/V` zamanında robot çarpacak. `T` değerini sadece bize verilen görüntülerden çıkarmak istiyoruz. 

![](assets\motion-collision-deltoid.png)

Yukarıdaki formülde `l(t)` ile `L`'yi birbiriyle ilişkilendirdik ancak burada hız hakkında bir şey yok. Buraya hızıda dahil etmemiz gerekiyor. Çünkü bu formül `t` anındaki `l(t)`'nin boyu hakkında bilgi veriyor ayrıca `f` focal length, `L` objenin boyu, `D(t)` cismin kameraya olan uzaklığı var. Eksik olan hız ve cismin görüntü üzerindeki büyüme küçülme hızı yok.

Eğer her iki tarafın `t`'ye göre türevini alırsak; `f` ve `L` sabitlerimiz olur.

![](https://render.githubusercontent.com/render/math?math=l%28t%29%3Df%5Cfrac%7BL%7D%7BD%28t%29%7D)

![](https://render.githubusercontent.com/render/math?math=l%28t%29%3D%5Cfrac%7B%5Cpartial%28l%28t%29%29%7D%7B%5Cpartial%20t%7D%3DfL%20%5Cfrac%7B%5Cpartial%20%28D%5E%7B-1%7D%28t%29%29%7D%7B%5Cpartial%20t%7D)

![](https://render.githubusercontent.com/render/math?math=l%27%28t%29%3DfL%5Cfrac%7B-1%7D%7BD%5E2%28t%29%7D%5Cfrac%7B%5Cpartial%20%28D%28t%29%29%7D%7B%5Cpartial%20t%7D)

Bu durumda

![](https://render.githubusercontent.com/render/math?math=v%3D%5Cfrac%7BtL%7D%7BD%5E2%28t%29%7DV)

Yukarıdaki `V` cismin gerçek hızı, `v` ise cismin görüntüdeki büyüme hızı. 

![](https://render.githubusercontent.com/render/math?math=D%28t%29%3DD_0-vt)

![](https://render.githubusercontent.com/render/math?math=%5Cfrac%7B%5Cpartial%28D%28t%29%29%7D%7B%5Cpartial%20t%7D%3D-v)

![](https://render.githubusercontent.com/render/math?math=l%27%28t%29%3DfL%5Cfrac%7Bv%7D%7BD%5E2%28t%29%7D)

![](https://render.githubusercontent.com/render/math?math=l%28t%29%3D%5Cfrac%7BfL%7D%7BD%28t%29%7D)

![](https://render.githubusercontent.com/render/math?math=%5Cfrac%7Bl%28t%29%7D%7Bl%27%28t%29%7D%3D%5Cfrac%7BfL%7D%7BD%28t%29%7D%5Cfrac%7BD%5E2%28t%29%7D%7BfL%5Cupsilon%7D%3D%5Cfrac%7BD%28t%29%7D%7B%5Cupsilon%7D%3D%5Ctau)

so, time to collison is:

![](https://render.githubusercontent.com/render/math?math=%5Ctau%3D%5Cfrac%7Bl%28t%29%7D%7Bl%27%28t%29%7D)

Bunu yaparken `L`, `D0` veya `v` bilmemize gerek yok.

## Stereo ile Motion Kıyaslama

Stereo'da iki görüntüyle sınırlı değil çok daha fazla olabilir ancak minimum iki görüntü üzerinde tüm stereo işlemlerini açıklayabiliriz (örneğin: epipolar geometri, fundamental matrix vb.) Stereo'da `baseline` kavramı çok önemli.

> Baseline: iki kamera arasındaki uzaklık.

 Motion'da `N` tane frameden bahsederiz. `Baseline`lar çok küçük olur yani kameranın ufacık bir hareketi. Hatta motionda `baseline`lar bazen `0` olur çünkü kamera kendi etrafında dönmüştür.

 Stereoda görüntüler alınırken aynı anda alınıyorlar mesela 20 tane kamera olsa da bir `t` anında hepsi aynı anda görüntü alır. Motion o şekilde değil bir kamera sahnenin içinde harekete ediyor hatta cisimlerde hareket edebilir.

 ### Problems
 #### Correspondence Problemi
 
Motionda, stereoda olduğu gibi `correspondence problemi`  var. Stereoda bunu çözmek için `block` tabanlı yöntemler vardı ve bunlar dense bilgiler üretiyorlardı onun çalışmazsa `feature matching` tabanlı yöntemleri uyguluyorduk.

Arka arkaya gelen iki tane framein birbirleriyle nasıl denkleştikleri.

#### Reconstruction Problem

Stereoda iki görüntü arasındaki denklikleri biliyorsak .?. nasıl yaparız? Nesnelerin 3D konumlarını gerçek dünyada nasıl buluruz?

Stereo için 3D parametlerini `R` ve `T` bulunması `E` ve `F`nin bulunması şeklinde düşünebiliriz.

Aynı problemler motionda da var. Sahnedeki cisimlerin 3D konumları ve kamera nasıl hareket etti yani kameranın rotasyonu ve transitionu nasıldı?

#### Segmentation

Kamera hareket ederken cisimlerde sahne içerisinde hareket edebilir. Buda demektir ki sahnenin içinde kaç çeşit hareket var.


![](https://render.githubusercontent.com/render/math?math=)
 
![](https://render.githubusercontent.com/render/math?math=)