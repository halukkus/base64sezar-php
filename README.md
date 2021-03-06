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
$acikid = explode('/',base64sezar($gizliid,$anahtar,'d',1))[1]; // anahtar ile çözüp, / ile parçalayıp id yi alıyoruz.
echo $gizliid.'<br>';
echo $acikid.'<br>';
```
örnek çıktı: 
```
Ft5sCrgiOMqZGuDg
1695
```
`base64sezar.php`
```php
function base64sezar($s,$x,$t,$v){ 
	$s_t='';
	if ($v=='1'){
	$pwf='abcdefghijklmnoprstuvyzxwq0123456789ABCDEFGHIJKLMNOPRSTUVYZXWQ';
	} elseif ($v=='2') {
	$pwf='ABCDEFGHIJKLMNOPRSTUVYZXWQ0123456789';
	}
	if ($t=='e'){
		$s=base64_encode($s);
		$s=str_replace('=','',$s);
	}
	$x=base64_encode($x);
	$x=str_replace('=','',$x);
	if (strlen($s)>0 && strlen($x)>0 ){
		$x2=0;
		$pwf_s=strlen($pwf)-1;
		$s_s=strlen($s)-1;
		$x_s=strlen($x)-1;
		$x1=0;
		while($x1 <= $s_s) {
			$ikarakter=substr($s,$x1,1);
			$ikarakter_p=strpos($pwf,$ikarakter);
			$xkarakter=substr($x,$x2,1);
			$xkarakter_p=strpos($pwf,$xkarakter);
			if ($x2<$x_s){
				$x2++;
			} else {
				$x2=0;	
			}
			if ($t=='e'){
				$ikarakter_p=$ikarakter_p+$xkarakter_p;
				if ($ikarakter_p>=$pwf_s){
					$pwf_kalan=$ikarakter_p-$pwf_s;
					$ikarakter_p=$pwf_kalan;
				} else {
					$ikarakter_p=$ikarakter_p+1;
				}
			} elseif ($t=='d'){
				$ikarakter_p=intval($ikarakter_p)-intval($xkarakter_p);
				if ($ikarakter_p<0){
					$pwf_kalan=intval($pwf_s)+intval($ikarakter_p);
					$ikarakter_p=$pwf_kalan;
				} else {
					$ikarakter_p=$ikarakter_p-1;
				}
			}
			$s_t=$s_t.''.substr($pwf,$ikarakter_p,1);
		$x1++;
		}
	}
	if ($t=='d'){
		$s_t=base64_decode($s_t);
	}
return $s_t;	
}
```

Fonksiyon Yapısı: 
```
base64sezar($s,$x,$t,$v);

	 s : şifrelenecek değer
	 x : anahtar
	 t : e => şifrele (encode)
	     d => çöz (decode)
	 v : 1 => büyük-küçük harf + rakamlar
	     2 => sadece büyük harf + rakamlar
```
