```
include('kripto.php');
$id = 1695; // örnek id
$anahtar = 'AvrupaYazilim'; // örnek şifrelemede bu anahtar kullanılacak
```
Sade şifreleme:
```php
$gizliid = kriptola($id,$anahtar,'e',1); // anahtar ile şifrele
$acikid = kriptola($gizliid,$anahtar,'d',1); // anahtar ile çöz
echo $gizliid.'<br>';
echo $acikid.'<br>';
```
örnek çıktı:

D7RvEU
1695

Genişletilmiş şifreleme:
```php
$gizliid = kriptola(rand(99,999).'/'.$id.'/'.rand(99,999),$anahtar,'e',1); // random sayılar (tercihen) ekleyerek şifreliyoruz.
$acikid = explode('/',kriptola($gizliid,$anahtar,'d',1))[1]; // anahtar ile çözüp, / ile parçalayıp id yi alıyoruz.
echo $gizliid.'<br>';
echo $acikid.'<br>';
```
örnek çıktı:

Ft5sCrgiOMqZGuDg
1695