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

![Epipolar Line Image](https://upload.wikimedia.org/wikipedia/commons/thumb/1/14/Epipolar_geometry.svg/640px-Epipolar_geometry.svg.png?1589134919459)

Bu durumda `epipolar line` elde etmiş oluruz. `p` verilirse `p' epipolar line`, `p'` verilirse `p epipolar line` elde ederiz.

`Epipolar line` bildiğimizde `epipolar line` üzerinde aramamızı yapabiliriz.

Tüm bunlar için `t` ve `R` nin verilmesi lazım!

Bunu bir adım ileriye taşıyalım `t` ve `R` bize verilmese de biz bunu `t` ve `R` olmadan yapmaya çalışalım.

## Cartesian Product
![](https://render.githubusercontent.com/render/math?math=axb%3D%0A%5Cbegin%7Bbmatrix%7D%0Ai%20%26%20j%20%26%20k%5C%5C%0Aa_1%20%26%20a_2%20%26%20a_3%5C%5C%0Ab_1%20%26%20b_2%20%26%20b_3%0A%5Cend%7Bbmatrix%7D)

![](https://render.githubusercontent.com/render/math?math=%5Cbegin%7Bequation%7D%0Aaxb%3D%0Ai%0A%5Cbegin%7Bbmatrix%7D%0Aa_2%20%26%20a_3%5C%5C%0Ab_2%20%26%20b_3%0A%5Cend%7Bbmatrix%7D%0A%2B%0Aj%0A%5Cbegin%7Bbmatrix%7D%0Aa_1%20%26%20a_3%5C%5C%0Ab_1%20%26%20b_3%0A%5Cend%7Bbmatrix%7D%0A%2B%0Ak%0A%5Cbegin%7Bbmatrix%7D%0Aa_1%20%26%20a_2%5C%5C%0Ab_1%20%26%20b_2%0A%5Cend%7Bbmatrix%7D)

Kartezyen product bu şekilde ancak biz böyle yapmak yerinde matrix şeklinde gösterebiliriz.

![](https://render.githubusercontent.com/render/math?math=axb%3D%5Ba%5D_x%20b=%0A%5Cbegin%7Bbmatrix%7D%0A0%20%26%20-a_3%20%26%20a_2%5C%5C%0Aa_3%20%26%200%20%26%20-a_1%5C%5C%0A-a_2%20%26%20a_1%20%26%200%0A%5Cend%7Bbmatrix%7D%0A%5Cbegin%7Bbmatrix%7D%0Ab_1%5C%5C%0Ab_2%5C%5C%0Ab_3%0A%5Cend%7Bbmatrix%7D)

![](https://render.githubusercontent.com/render/math?math=%5Ba%5D_x%3D%0A%5Cbegin%7Bbmatrix%7D%0A0%20%26%20-a_3%20%26%20a_2%5C%5C%0Aa_3%20%26%200%20%26%20-a_1%5C%5C%0A-a_2%20%26%20a_1%20%26%200%0A%5Cend%7Bbmatrix%7D)






![](https://render.githubusercontent.com/render/math?math=)