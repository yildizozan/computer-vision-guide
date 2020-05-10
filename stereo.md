Farklı açıdan çekilmiş iki fotoğraf lazım.

Aynı eksen etrafında döndürülmüş olması da gerekiyor.

Farklı noktalardan çekilmiş olması lazım. Bu farklı pozisyonlar insanın iki gözüne denk geliyor.

### Disparity
Farklı yerden çekilmiş iki fotoğrafta aynı noktaların farklı yerlerde olmasına `disparity` deniliyor.

Her bir noktanın diğer görüntüdeki dengini bulsak ve aralarındaki dengini çıkarsak noktayla arasındaki bulalım. Her pixel için elimize bir tane sayı geçecek. O sayıdan oluşan görüntü oluşturursam o zaman `disparity` resmini elde ederim. `Disparity`'de pixel ne kadar parlaksa o kadar yakındır.

`Disparity` görselinde sıfır olan yerler, diğer resimde orası görünmüyor. Yani `occlusion` var.

`Stereo`'da `occlusion` olacaktır. `Occlusion` ne kadar fazlaysa iş o kadar zordur.

Aynı noktadan çekilen fotoğraflarda `occlusion` hiç olmaz ancak o zamanda derinlik elde edilmez. Bu sebeple `occlusion`'u çözmemiz lazım.

![Basic Perspective Projection](assets\stereo-basic-perspective-projection.png)

1, 2, 3, 4 noktaları görüntüde aynı yere düşerç Teorik olarak sadece bir görüntüye bakarak bir noktanın derinliğini bulmak mümkün değil.

Gerçek dünyada p noktasının üzerinde düşecek sonsuz tane nokta var! Bu noktaların gerçek konumunu bulmak için başka noktadan bakmamız lazım.

# İşte Stereo!

![Why Stereo Vision](assets/stereo-why-stereo-vision.png)

Üçgenleme yöntemi ile çözüyoruz. Haritacılar bu işle çok uğraşıyorlar.

### Parallax:
İki gözün farklı görmesi ve `occlusion`'lar oluşturması.

### Vanishing Point:
Paralel giden paralellerin perspektifte birleştiği nokta.

## Julesz Random-Dot Experiment
Cisimleri algılarken:
- Vanishing point
- Perspektif
- Cisimlerin büyüklüğü küçüklüğü
önemli ama iki görüntü arasında birbirine denk gelen noktaların denkleştirilmeside insan için önemli.

## STEREOGRAM

![A Simple Stereo System](assets\stereo-why-stereo-vision-1.png)

İki kamera arasında `Tx` kadar fark var.

![A Simple Stereo System](assets\stereo-why-stereo-vision-1.png)

`Stereo`'da `Tx`'e `baseline` diyoruz.

![X ekseni](https://render.githubusercontent.com/render/math?math=x_l%3Df%28x%2Fz%29)

![Y ekseni](https://render.githubusercontent.com/render/math?math=y_l%3Df%28y%2Fz%29)

Diyelim sol kamera için `x1`, `y1` pozisyonuna düşüyor. Peki sağ kamera için?

Sağ kamera için

![Y ekseni](https://render.githubusercontent.com/render/math?math=y_r%3Df%28y%2Fz%29)

`y` ekseninde aynı kalır. `Baseline` nedeniyle değişen `x` olacak.

![X ekseni](https://render.githubusercontent.com/render/math?math=x_r%3Df%28X%20-%20T_x%29%2Fz)

Bu şekilde kamerayı sağa kaydırmak yerine gerçek dünyadaki noktayı sola kaydırmış olduk. Eğer böyle yaparsak iki kamera birbirinin tıpatıp aynısı oluyor sadee birisi gerçek dünya ile konuşurken `-Tx` kadar çıkartmak gerekli.

Biz en başından beri `disparity` peşindeyiz.

![Stereo Disparity](assets/stereo-disparity.png)

Burada birbirine denk gelen noktada `y`'ler aynı `x`'ler farklı;

### Sol kamera

![Stereo Disparity](assets/stereo-disparity-left-camera.png)

### Sağ kamera

![Stereo Disparity](assets/stereo-disparity-right-camera.png)

### Stereo Disparity

![Stereo Disparity](assets/stereo-disparity-formula.png)

![Stereo Disparity](assets/stereo-disparity-short-formula.png)

![Stereo Disparity](assets/stereo-disparity-explain.png)

> `Tx = baseline` iki kamera arasındaki uzaklık.

Bu eşitlik bir noktanın derinliği hakkında oluşuyor.

`d`'yi sıfıra götürelim sağ ve sol görüntü üzerinde nokta aynı yerde demektir. `d` sıfır olduğu zaman sonsuza gidiyor.

Bu seferde `d`'yi büyütelim. `d`'yi ne kadar büyütürsek `Z` o kadar küçülüyor. Kameranın dibine kadar geliyor.

`Tx` küçültelim; `Tx` sıfır olduğu zaman `d` ne oluyor?

`d` her zaman için sıfır olacak. `Tx` sıfır olduğu durumda derinlikten bahsedemeyiz. Derinlik için `Tx (baseline)` olması lazım, yoksa derinlik bulunmaz.

Mesela bazı hayvanlarda `central projection` aynı yerde, yani iki gözde aynı yerde. Aynı yerde olunca derinliği bulamayız.

Eğer elde ettiğimiz görüntüleri birleştirip bir panoroma oluşturacaksak o zaman `Tx=0` olması lazım. Derinlikle alakalı yok. Derinlik olursa dikiş yapamıyoruz. Tam üst üste oturmuyor.

`Yr` ve `Yl` arasındaki farkın sıfır çıkmasının sebebi nedir? \
Sebebi kamerayı sadece x ekseninde hareket ettirdik.

Kamerayı y ekseninde de hareket ettirirsek orada da fark elde ederiz.

Bu farklılaşma sürekli olarak bir doğru üzerinde oluyor.

`P` noktasına denk gelebilecek tüm noktalar, sağ görüntüde bir doğru üzerinde olmalı.

Yani `x`, `y`'e değişebilir ancak `disparity` sadece bir doğru üzerinde oluyor. Bir doğru üzerindeki farklılaşma. Dolayısıyla `stereo`'da arama problemi tek boyutlu bir problem `P` noktasını sağ görüntü üzerinde ararken tüm görüntü üzerinde aramıyoruz, bir doğru üzerinde arıyoruz. Buna `epipolar constraint` deniliyor.

> Derinlik ve `stereo disparity` ters orantılıdır.

İnsanda `Tx=63mm`, `Tx` değeri diğer değerlerle uyumlu ise sorun yok. 

`f`'yi ihmal edelim.

Örneğin:

d1=63/10 (6 pixel)

d2=63/60 (1 pixel)

d3=63/10,000

d4=63/15,000

`d3` ve `d4` yaklaşık 0 pixel!

`Z` ile `Tx` birbirine benzeye sayılarsa stereo işe yarıyor. `Z` **çok büyük** sayılarsa işe yaramıyor.

Bu durumda insan 1.2 metre sonra çokta derinlik alamıyor. Peki insan derinlik algısını nasıl alıyor?
`Occlusion`'lardan alıyor, cisimlerin büyüklüklerinden alıyoruz, `vanishing point`'lerden alıyoruz.


# EPIPOLAR GEOMETRY

![Epipolar Geometry](assets/stereo-epipolar-geometry.png)

`P` noktasının yerini değiştirdiğimizde

`ep` ve `e'p'` doğruları yer değişecek ancak `e` ve `e'` değişmeyecek, neden?

Cevap: `Baseline` olduğu için sadece kameralar yer değiştirince değişir.

`O` ve `O'` kameralarımızın merkezleri
`e` ve `e'` epipoldür.
`l` ve `l'` ise epipolar lines.

![Transation](https://render.githubusercontent.com/render/math?math=%5Cvec%7BOp%7D.%5B%5Cvec%7BOO%27%7Dx%5Cvec%7BO%27p%27%7D%5D%3D0)

`Transation` iki kamera arasındaki uzaklık.

İki kamera arasında `T` var. Birde `R` yani `rotation` var. `Rotation`'u da işin içine koymamız gerekiyor.

![Vector Op](https://render.githubusercontent.com/render/math?math=%5Cvec%7BO'p'%7D) vektörünü ![Vector Op](https://render.githubusercontent.com/render/math?math=%5Cvec%7BOp%7D) koordinat sistemi cinsinden açıklamak için elimdeki `p'`'nü rotate etmek gerekir.

![p.[tx (Rp')]](https://render.githubusercontent.com/render/math?math=p.%5Btx%28Rp%27%29%5D)

Burada rotation matrisi 3x3 matrisdir.

> Hatırlatmak isterim ki 12 bilinmeyeni olan `projection matrisi` idi.

Rotation marrisi içinde 9 sayı var ancak içerisinde 3 bilinmeyen var. x, y, ve z etrafında dönme.

`Rp'` ifadesinde `R` 3x3 rotation matris ve `p` ise 3'lük vektör.

#### Soru:
3x3 matris ve 3'lük vectör çarpımı ne olur?
#### Cevap:
Yukarıdaki ifadeye bakacak olursak `Rp'` 3'lük vectör gelir. `t` ile `Rp'` kartezyen çarpımından 3'lük vectör gelir. Son olarak `p` 3'lük vectördür ve önceki ifade ile çarpımından dan gelen 3'lük vectör ile **dot product** yapılırsa sonuç *skaler* olur.

![p.[tx (Rp')]](https://render.githubusercontent.com/render/math?math=p.%5Btx%28Rp%27%29%5D=0)

Yukarıdaki ifadenin 0 olması gerekiyor.

Eğer birisi bize iki görüntü verip `t` ve `R` değerlerini de biliyorsak sonuç 0 olmalı.

> `p`, `t`, `R` değerlerini biliyoruz `p'` bulabilir miyiz?

Bunu matematiksel olarak düşündüğümüzde `p`, `t`, `R` verilmişse `p'` bulabilmem lazım. Yapacağımız tek şey onları çarpıp tersini alıp karşı tarafa geçirmek.

Ancak öyle bir şey olamaz `corresponding` problemini hiç resme bakmadan çözmüş gibi bir şey olduk ancak resme bakmamız lazım.

Aslında matematiksel olarakta söylediğimiz çok doğru değil çünkü `p`, `t`, `R` verildiğinde ve `p'`'nü bulduğumuzda elimizde cevap değil `p'`'nün bulunduğu doğru denklemi geçiyor. Doğrudan `p'` bulamıyoruz.

![Epipolar Line Image](https://upload.wikimedia.org/wikipedia/commons/thumb/1/14/Epipolar_geometry.svg/640px-Epipolar_geometry.svg.png)

Bu durumda `epipolar line` elde etmiş oluruz. `p` verilirse `p' epipolar line`, `p'` verilirse `p epipolar line` elde ederiz.

`Epipolar line` bildiğimizde `epipolar line` üzerinde aramamızı yapabiliriz.

Tüm bunlar için `t` ve `R` nin verilmesi lazım!

Bunu bir adım ileriye taşıyalım `t` ve `R` bize verilmese de biz bunu `t` ve `R` olmadan yapmaya çalışalım.

## Cartesian Product
![](https://render.githubusercontent.com/render/math?math=axb%3D%0A%5Cbegin%7Bbmatrix%7D%0Ai%20%26%20j%20%26%20k%5C%5C%0Aa_1%20%26%20a_2%20%26%20a_3%5C%5C%0Ab_1%20%26%20b_2%20%26%20b_3%0A%5Cend%7Bbmatrix%7D)

![](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bequation%7D%0Aaxb%3D%0Ai%0A%5Cbegin%7Bbmatrix%7D%0Aa_2%20%26%20a_3%5C%5C%0Ab_2%20%26%20b_3%0A%5Cend%7Bbmatrix%7D%0A%2B%0Aj%0A%5Cbegin%7Bbmatrix%7D%0Aa_1%20%26%20a_3%5C%5C%0Ab_1%20%26%20b_3%0A%5Cend%7Bbmatrix%7D%0A%2B%0Ak%0A%5Cbegin%7Bbmatrix%7D%0Aa_1%20%26%20a_2%5C%5C%0Ab_1%20%26%20b_2%0A%5Cend%7Bbmatrix%7D)

Kartezyen product bu şekilde ancak biz böyle yapmak yerinde matrix şeklinde gösterebiliriz.

![](https://render.githubusercontent.com/render/math?math=axb%3D%5Ba%5D_x%20b%3D%0A%5Cbegin%7Bbmatrix%7D%0A0%20%26%20-a_3%20%26%20a_2%5C%5C%0Aa_3%20%26%200%20%26%20-a_1%5C%5C%0A-a_2%20%26%20a_1%20%26%200%0A%5Cend%7Bbmatrix%7D%0A%5Cbegin%7Bbmatrix%7D%0Ab_1%5C%5C%0Ab_2%5C%5C%0Ab_3%0A%5Cend%7Bbmatrix%7D)

![](https://render.githubusercontent.com/render/math?math=%5Ba%5D_x%3D%0A%5Cbegin%7Bbmatrix%7D%0A0%20%26%20-a_3%20%26%20a_2%5C%5C%0Aa_3%20%26%200%20%26%20-a_1%5C%5C%0A-a_2%20%26%20a_1%20%26%200%0A%5Cend%7Bbmatrix%7D)

İşte `skew-symmetrix` matrix. Bu işlemin sonucu bize vektör verecek.

![Transation](https://render.githubusercontent.com/render/math?math=%5Cvec%7BOp%7D.%5B%5Cvec%7BOO%27%7Dx%5Cvec%7BO%27p%27%7D%5D%3D0)

![p.[tx (Rp')]](https://render.githubusercontent.com/render/math?math=p.%5Btx%28Rp%27%29%5D=0)

Aslında yukarıdaki ifadelerin ikiside aynı.

![](https://render.githubusercontent.com/render/math?math=p%5ET%20%5Cepsilon%20p%27%3D0)

Burada `epsilon` şeklinde gösterilen `essential matrix`.

![](https://render.githubusercontent.com/render/math?math=%5Cepsilon%3D%5Bt_x%5DR)

> Skew-Symmetric matris nasıl yazılır?

![Skew-Symmetric Matrix Transform](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bbmatrix%7D%0Aa_1%5C%5C%0Aa_2%5C%5C%0Aa_3%0A%5Cend%7Bbmatrix%7D%0A%3D%0A%5Cbegin%7Bbmatrix%7D%0A0%20%26%20-a_3%20%26%20a_2%5C%5C%0Aa_3%20%26%200%20%26%20-a_1%5C%5C%0A-a_2%20%26%20a_1%20%26%200%0A%5Cend%7Bbmatrix%7D)


Öncelikle `epsilon` değerindeki `t`'yi `skew-symmetric` haline getirmemiz gerekiyor. Sonra `t`, 3x3 matris halini alır. `R`, 3x3 matris idi zaten. Bu durumda `epsilon` 3x3 matris olur.

![](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bbmatrix%7D%0A.%20%26%20.%20%26%20.%0A%5Cend%7Bbmatrix%7D%0A%5Cbegin%7Bbmatrix%7D%0A.%20%26%20.%20%26%20.%5C%5C%0A.%20%26%20.%20%26%20.%5C%5C%0A.%20%26%20.%20%26%20.%0A%5Cend%7Bbmatrix%7D%0A%5Cbegin%7Bbmatrix%7D%0A.%5C%5C%0A.%5C%5C%0A.%0A%5Cend%7Bbmatrix%7D)

Bu durumda yukarıdaki duruma getirdik matrisleri. Bunların hepsini çarparsak sonuç bir tane skaler olacak. Bunu da sırayla yapacak olursak;

![](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bbmatrix%7D%0A.%20%26%20.%20%26%20.%5C%5C%0A.%20%26%20.%20%26%20.%5C%5C%0A.%20%26%20.%20%26%20.%0A%5Cend%7Bbmatrix%7D%0A%5Cbegin%7Bbmatrix%7D%0A.%5C%5C%0A.%5C%5C%0A.%0A%5Cend%7Bbmatrix%7D)

işleminin sonucu `3x1` vektör. Sonra

![](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bbmatrix%7D%0A.%20%26%20.%20%26%20.%0A%5Cend%7Bbmatrix%7D%0A%5Cbegin%7Bbmatrix%7D%0A.%5C%5C%0A.%5C%5C%0A.%0A%5Cend%7Bbmatrix%7D)

işleminin sonucu ise `skaler` olacaktır.

## Essential Matrix

![](https://render.githubusercontent.com/render/math?math=p%5ET%20%5Cepsilon%20p%27%3D0)

İçerisinde 9 tane sayı var ve o sayılar `R` ve `t` çarpımından oluşuyor. Bunu kamera kalibrasyonunda kullanabiliriz ama `epsilon` matrisi nasıl buluruz?

> Elimizde 9 tane denklem var `p` ve `p'` nü nasıl buluruz?

> Cevap basit onları bilebilecek görüntüler çekebiliriz. Peki bunu nasıl yaparız. Örneğin bomboş resim ortasında bir tane siyah nokta var. İki resimde de siyah noktanın resmin ortasına geldiğini biliyoruz bu sebeple `p` ve `p'` buluruz.

İki resim çektik bu bize `p` ve `p'` bilinir ancek `epsilon` değerinin hala bilmiyoruz. İlk çekilen resimler bize sadece bir tane eşitlik verdi. Ortasında nokta olan o sahneyi sağa sola götürerek istediğimiz kadar denklem elde ederiz. Her çektiğimiz resim bize `epsilon` içindeki sayılar hakkında bilgi veriyor. O bilgileri yeteri kadar toplayıp alt alta koyduğumuz zaman o 9 tane noktayı bulabiliriz.

İki tane kamerayı ayrı ayrı kalibrasyonunu yapmadan doğrudan `essential matrisi` buldum. `eEssential matrisi` bulduktan sonra `epipolar geometriyi` çözmüş olduk.

Buda demek oluyorki yeni bir sahnede çektim fotoğrafı iki tane görüntü var elimde, bir tane noktayı seçtim, seçtiğim noktaya denk gelen `epipolar line` nerede ise formülde yerlerine koyduk aramamızı o doğru üzerinde yapabiliriz.

`Essential matrisi` bir sayı ile çarpalım yine `essential matris`tir çünkü bulduğumuz `epsilon` matrisindeki 9 sayı, `unique` 9 tane sayı değil. benim sistemimin başka `essential matris`leride var ve onların hepsi birbirine denktir. Aralarındaki farklılık bir scaler olabilir, neden?

![](https://render.githubusercontent.com/render/math?math=p%5ET%20%5Cepsilon%20p%27%3D0)

her iki tarafı `alpha` ile çarparsak değişmez, bir `skaler` ile bağlıdır.

`R` içeriside 3 tane sayı var `t` içerisinde de 3 tane sayı var ama `t` için önemli olan vektörün yönü, nerede bağladığı nerede bittiği önemli değil. Vektörün yönünüde iki tane sayı ile ifade edebilirim. Bu durumda `essential matris`de `3+2=5` tane sayı var.

![Ep'](https://render.githubusercontent.com/render/math?math=%5Cepsilon%20p%27) bir doğru formülü `p'` noktasının sol resim üzeride gelebileceğin yerlerin doğrusu. Tersi yani ![](https://render.githubusercontent.com/render/math?math=p%5ET%5Cepsilon)'de doğru benzer şekilde `p'` üzerinde gelebileceği yerlerin doğrusu.

![Transation](https://render.githubusercontent.com/render/math?math=%5Cvec%7BOp%7D.%5B%5Cvec%7BOO%27%7Dx%5Cvec%7BO%27p%27%7D%5D%3D0)

Buradaki meseleyi tekrar edecek olursak. Aynı düzlem üzerindeki iki vektörü `cross product` yaparsak o düzleme dik başka bir vektör elde ederiz.

![](https://render.githubusercontent.com/render/math?math=%5Cvec%7BV%7D%7C%5Cvec%7BY%7Dx%5Cvec%7BZ%7D%7C) burada 
![](https://render.githubusercontent.com/render/math?math=%7C%5Cvec%7BY%7Dx%5Cvec%7BZ%7D%7C) 
ifadesinden 
![](https://render.githubusercontent.com/render/math?math=%5Cvec%7BA%7D) 
elde edilir. 
![](https://render.githubusercontent.com/render/math?math=%5Cvec%7BX%7D.%5Cvec%7BA%7D) 
ise 
![](https://render.githubusercontent.com/render/math?math=%5Cvec%7BX%7D) 
vektörünü 
![](https://render.githubusercontent.com/render/math?math=%5Cvec%7BX%7D), 
![](https://render.githubusercontent.com/render/math?math=%5Cvec%7BY%7D) 
ve 
![](https://render.githubusercontent.com/render/math?math=%5Cvec%7BZ%7D) 
vektörüne dik. İfade
![](https://render.githubusercontent.com/render/math?math=%5Cvec%7BX%7D.%5Cvec%7BA%7D)
bu hale geldi
![](https://render.githubusercontent.com/render/math?math=%5Cvec%7BA%7D) 
vektörü
![](https://render.githubusercontent.com/render/math?math=%5Cvec%7BX%7D)
vektörüne dik olduğunu söylemiştim `dot product` yaptığımızda 0 olmalı. İşin matematiğini basitçe ele aldıktan sora yapmaya çalıştığımız `p` verildiğinde `p'` üzerinde yer aldığı doğru parçalarını bulabilelim.

![](https://render.githubusercontent.com/render/math?math=p%3DK%5Chat%7Bp%7D)

![](https://render.githubusercontent.com/render/math?math=p%3DK%5E%7B-1%7D%5Chat%7Bp%7D)

![](https://render.githubusercontent.com/render/math?math=p%5E%7BT%7D%5Cepsilon%20p%27%3D0)

![](https://render.githubusercontent.com/render/math?math=%28K%5E%7B-1%7Dp%29%5E%7BT%7D%5Cepsilon%20K%27p%27%3D0)

> Bu noktada ![](https://render.githubusercontent.com/render/math?math=%28K%5E%7B-1%7Dp%29%5E%7BT%7D) nedir?


> ![](https://render.githubusercontent.com/render/math?math=%28AB%29%5ET%3DB%5ET%20A%5ET)
olduğundan

![](https://render.githubusercontent.com/render/math?math=K%5ET%20p%5ET%20%5Cepsilon%20K%27p%27%20%3D%200)

buradan ...

![](https://render.githubusercontent.com/render/math?math=F%20%3D%20K%5E%7B-T%7D%20%5Cepsilon%20K%27%5E%7B-1%7D)

`K` sol kamera için `intristic` parametreler matrisi. `K'` sağ kamera için `intristic` parametreler matrisi.

Buradaki `F` fundamental matris.

yani ![](https://render.githubusercontent.com/render/math?math=p%5ET%20F%20p%27%20%3D%200) olmalı.

`Essential` matris gerçek dünyadaki koordinatlarda çalışıyor ancak `fundamental` matrix image koordinatlarında çalışıyor ve bizim buna erişimimiz var.

- Fundamental matrix **3x3** boyutlu bir matrix.
- Essential matrisin rankı **2**.
- Essential matrisin içerisinde **5** tane sayı var. 
- Fundamental matrisin içerisinde **7** tane sayı var. 

Buradaki `fundamental` matrisin rankı **3** olsaydı bunu çözebilirdik ancak rankı **2**.

> *Hatırlatma: `Fx` çözdüğümüzde `epipolar lines` gelecek.*

> *Hatırlatma: Bir doğru üzerinde noktaları aldığımızda `F` hesaplayamayız.*

## WEAK CALIBRATION
![](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bbmatrix%7D%0Au%20%26%20v%20%26%201%0A%5Cend%7Bbmatrix%7D%0A%5Cbegin%7Bbmatrix%7D%0AF_%7B11%7D%20%26%20F_%7B12%7D%20%26%20F_%7B13%7D%5C%5C%0AF_%7B12%7D%20%26%20F_%7B22%7D%20%26%20F_%7B23%7D%5C%5C%0AF_%7B13%7D%20%26%20F_%7B32%7D%20%26%20F_%7B33%7D%0A%5Cend%7Bbmatrix%7D%0A%5Cbegin%7Bbmatrix%7D%0Au%27%5C%5C%0Av%27%5C%5C%0A1%0A%5Cend%7Bbmatrix%7D%0A%3D0)

Eğer sol görüntünün, sağ görüntüye denk geldiğini biliyorsam ve elimde bunun gibi alt alta **9** tane eşitlik varsa burada `F` matrisini buluruz.

`F` matrix rank **2** olduğu için **9** tane sayıya gerek yok. **8** tane sayıylada bulabiliriz çünkü rank **3** değil.

![](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bbmatrix%7D%0Au%20%26%20v%20%26%201%0A%5Cend%7Bbmatrix%7D%0A%5Cbegin%7Bbmatrix%7D%0AF_%7B11%7D%20%26%20F_%7B12%7D%20%26%20F_%7B13%7D%5C%5C%0AF_%7B12%7D%20%26%20F_%7B22%7D%20%26%20F_%7B23%7D%5C%5C%0AF_%7B13%7D%20%26%20F_%7B32%7D%20%26%20F_%7B33%7D%0A%5Cend%7Bbmatrix%7D%0A%5Cbegin%7Bbmatrix%7D%0Au%27%5C%5C%0Av%27%5C%5C%0A1%0A%5Cend%7Bbmatrix%7D%0A%3D0)

![](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bbmatrix%7D%0Auu%27%20%26%20uv%27%2C%20u%2C%20vu%27%2C%20vv%27%2C%20v%2C%20u%2C%20v%2C%201%0A%5Cend%7Bbmatrix%7D%0A%5Cbegin%7Bbmatrix%7D%0AF_%7B11%7D%5C%5C%0AF_%7B12%7D%5C%5C%0AF_%7B13%7D%5C%5C%0AF_%7B21%7D%5C%5C%0AF_%7B22%7D%5C%5C%0AF_%7B23%7D%5C%5C%0AF_%7B31%7D%5C%5C%0AF_%7B32%7D%5C%5C%0AF_%7B33%7D%5C%5C%0A%5Cend%7Bbmatrix%7D%0A%3D0)

biz bunun gibi alt alta 9 tane matrix getirirsek sonuç:

![](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bbmatrix%7D%0Au_1u%27_1%20%26%20u_1v%27_1%2C%20u_1%2C%20v_1u%27_1%2C%20v_1v%27_1%2C%20v_1%2C%20u_1%2C%20v_1%5C%5C%0Au_2u%27_2%20%26%20u_2v%27_2%2C%20u_2%2C%20v_2u%27_2%2C%20v_2v%27_2%2C%20v_2%2C%20u_2%2C%20v_2%5C%5C%0Au_3u%27_3%20%26%20u_3v%27_3%2C%20u_3%2C%20v_3u%27_3%2C%20v_3v%27_3%2C%20v_3%2C%20u_3%2C%20v_3%5C%5C%0Au_4u%27_4%20%26%20u_4v%27_4%2C%20u_4%2C%20v_4u%27_4%2C%20v_4v%27_4%2C%20v_4%2C%20u_4%2C%20v_4%5C%5C%0Au_5u%27_5%20%26%20u_5v%27_5%2C%20u_5%2C%20v_5u%27_5%2C%20v_5v%27_5%2C%20v_5%2C%20u_5%2C%20v_5%5C%5C%0Au_6u%27_6%20%26%20u_6v%27_6%2C%20u_6%2C%20v_6u%27_6%2C%20v_6v%27_6%2C%20v_6%2C%20u_6%2C%20v_6%5C%5C%0Au_7u%27_7%20%26%20u_7v%27_7%2C%20u_7%2C%20v_7u%27_7%2C%20v_7v%27_7%2C%20v_7%2C%20u_7%2C%20v_7%5C%5C%0Au_8u%27_8%20%26%20u_8v%27_8%2C%20u_8%2C%20v_8u%27_8%2C%20v_8v%27_8%2C%20v_8%2C%20u_8%2C%20v_8%0A%5Cend%7Bbmatrix%7D%0A%5Cbegin%7Bbmatrix%7D%0AF_%7B11%7D%5C%5C%0AF_%7B12%7D%5C%5C%0AF_%7B13%7D%5C%5C%0AF_%7B21%7D%5C%5C%0AF_%7B22%7D%5C%5C%0AF_%7B23%7D%5C%5C%0AF_%7B31%7D%5C%5C%0AF_%7B32%7D%5C%5C%0AF_%7B33%7D%5C%5C%0A%5Cend%7Bbmatrix%7D%0A%3D-%0A%5Cbegin%7Bbmatrix%7D%0A1%5C%5C%0A1%5C%5C%0A1%5C%5C%0A1%5C%5C%0A1%5C%5C%0A1%5C%5C%0A1%5C%5C%0A1%5C%5C%0A%5Cend%7Bbmatrix%7D)

Yukarı 9 tane sayı olmasına rağmen 8 tane denklemle çözebiliriz çünkü `epsilon` matrix gibi 
`F` matriside `up-to-scale` tanımlı yani **9** tane `unique` sayı değil, **8** tane tanımlı.

Biz tüm sayıları **9**'a bölersek **9.** sayı **1** olur ancak yinede `fundamental` matrix olur. Yukarıdaki **8** denklemi oluşturmak için 8 tane `corresponse` bilmek gerekiyor.

> Dikkat edilece nokta eğer `corresponse`'lar aynı doğru üzerinde ise `fundamental` matrisler doğru çıkmıyor çünkü değişik derinliklerden ve değişik düzlemlerden farklı noktalar vermek lazım ki yeteri kadar elimizde bilgi olsun.

## Rectification

Öyle iki tane `sanal kamera`m olsaydı ki bunların görüntü düzlemleri birbirine paralel olsun ve bu `sanal kamera`lar arasında `epipolar line`lar oluşturmaya çalıştığım zaman o doğrular birbirine parallel gelip aynı satıra denk gelsin.

`Central projection` ile `sanal kamera`yı kestiği yere noktayı koyuyoruz. Bunu teker teker uyguluyoruz. Bu `sanal kamera`ların ürettiği görüntüler kendisinden `rectified`.

`Rectification` yaptığımızda `corresponse` bulmak çok kolaylaşıyor. Doğru çizmekle, `F` ile uğraşmak zorunda kalmıyoruz.

Sanal iki görüntünüm varmış gibi düşünüyoruz. Tek yaptığımız şey bu iki görüntünün hesaplanması. Hesaplandıktan sonra `corresponse` problemi (çünkü zor olan o) çözmek daha kolaylaşıyor.

## A Simple Stereo System

![]()

- `Xl` ve `Xr` arasındaki fark `disparity`.
- `Ol` ve `Or` aras `T` yani `baseline`.
- `Central projection` ile görüntü düzlemi arasındaki uzaklık `odak uzaklığı`.

Burada amacımız `Z`'yi bulmak `Z`'yi bulmak için üçgenlerin benzerliğini kullanarak çözeriz.

![](https://render.githubusercontent.com/render/math?math=%5Cdfrac%7BT%7D%7BT-%28x_l-x_r%29%7D%0A%3D%0A%5Cdfrac%7BZ%7D%7Bz-f%7D)

![](https://render.githubusercontent.com/render/math?math=Z%3Df%0A%5Cdfrac%7BT%7D%7Bx_l-x_r%7D)

Eğer `disparity` büyük olursa `Z` küçük olur. Bunu şöyle örneklendirebiliriz. İşaret parmağımızı 4-5 cm gözlerimizin önüne getiririz. Sağ gözümüzü kapatıp sol gözümüzle bakarız sonrada tam tersi şekilde yaparız ve parmağımızın çok yer değiştirdiğini göreceğiz işte bu durum `disparity`'nin fazla olmasıdır. Benzer şekilde aynı şeyi uzaktaki cisimlere uyguladığımızda ise cisimler az yer değiştirir.

![](https://render.githubusercontent.com/render/math?math=Z%3Df%5Cdfrac%7BT%7D%7Bd%7D)

Burada `T` ve `f` sabit. `T`'yi değiştirmeyi deneyelim. `T`'nin değişmesi için kameraların yaklaştırılıp uzaklaştırmam lazım.

`T`'yi **0** yapmak iki kamerayı *tek kamera* yapmak demek. Aynı kamerayla iki farklı zamanda fotoğraf çekmek demek. Peki bu mümkün mü?

Buradaki mesele `baseline`! `Baseline` yok olduğu zaman formül **işlevsiz**.

Açı düşük bir açı ise gözler paralelleşiyorç Göz çok uzağa *focus* oluyor. Göz yakına *focus* olmak için açının büyümesi gerek, açı büyüyünce gözler kendilerini iyi gererek yakına *focus* oluyor. VR gözlüklerinde açık çok büyük ancak objeyi çok yakında görüyoruz bu durum insanın kafasını karıştırıyor ve mide bulantısı gibi durumlar ortaya çıkıyor.

### Baseline yakın mı olmalı uzak mı?

Birbirine yaklaştırdığımıza `hassasiyet` artar.

![](https://render.githubusercontent.com/render/math?math=Z%3Df%5Cdfrac%7BT%7D%7Bd%7D)

bu durumda `T` küçülür. `T` küçük olduğu zaman `d`'deki ufak yanılma `Z`'yi çok kötü bir şekilde etkiliyor. `T` yeteri kadar büyükse `d`'deki bir iki pixellik yanılma bizi o kadar etkilemez. Dolayısıyla kameraları olabildiği kadar uzak yapmalıyız.

### Özetle:
> `Baseline` yakınsa, `FOV` artar.\
Sorun *hesaplama problemi* ve *`T`'ye çok bağımlı* olmam.

> `Baseline` uzaksa, `FOV` artar.\
Sorun ise *az yerin görülmesi*

Güzel çözümlerden bir tanesi `baseline` uzaklaştır ancak kameraları birbirine doğru çevirmek. Aynı insan gözü gibi. Ancak buradaki problem kameraları birbirinden ne kadar uzaklaştırırsam **görüntülerin birbirlerinden görünüşü** o kadar **farklı** oluyor.

1. ``Corresponse` bulmak çok zor oluyor. Örneğin bir küpe baktığımızı düşünelim. Kameralar çok uzak olduğunda her iki kamera kübün **farklı yanlarını** görebilir bu şekilde `correnponse` kurmak **imkansız**.
2. `Işıklandırma` **farkından** dolayı görüntüler **çok çok farklı** olmaya başlıyor

![](https://render.githubusercontent.com/render/math?math=)

![](https://render.githubusercontent.com/render/math?math=)

![](https://render.githubusercontent.com/render/math?math=)

![](https://render.githubusercontent.com/render/math?math=)

![](https://render.githubusercontent.com/render/math?math=)