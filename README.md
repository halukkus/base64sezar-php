# base64sezar-php
Php Veri Şifreleme Metodu
<p>Belirli bir anahtar ile veriyi alfanumerik karakterler ile şifrelemek ve aynı anahtar ile şifrelemeyi çözmek mümkündür. Bu yolla açık haldeki id ler, get metodu ile gönderilen veriler şifrelenebilir. Veritabanında tutulan kritik öneme sahip datalar şifrelenebilir. </p>

```php
include('base64sezar.php');
$id = 1695; // örnek id
$anahtar = 'AvrupaYazilim'; // örnek şifrelemede bu anahtar kullanılacak
```

## Sade şifreleme:
```php
$gizliid = base64sezar($id,$anahtar,'e',1); // anahtar ile şifrele
$acikid = base64sezar($gizliid,$anahtar,'d',1); // anahtar ile çöz
echo $gizliid.'<br>';
echo $acikid.'<br>';
```
örnek çıktı: 
```
D7RvEU
1695
```

## Genişletilmiş şifreleme:
```php
$gizliid = base64sezar(rand(99,999).'/'.$id.'/'.rand(99,999),$anahtar,'e',1); // random sayılar (tercihen) ekleyerek şifreliyoruz.
$acikid = base64sezar('/',kriptola($gizliid,$anahtar,'d',1))[1]; // anahtar ile çözüp, / ile parçalayıp id yi alıyoruz.
echo $gizliid.'<br>';
echo $acikid.'<br>';
```
örnek çıktı: 
```
Ft5sCrgiOMqZGuDg
1695
```
Fonksiyon Yapısı: 
```
base64sezar($s,$x,$t,$v);

	 s : şifrelenecek değer
	 x : anahtar
	 t : e => şifrele
	     d => çöz
	 v : 1 => büyük-küçük harf + rakamlar
	     2 => sadece büyük harf + rakamlar
```
