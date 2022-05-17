# Gnoland'ı indirelim ve yükleyelim

```bash
git clone https://github.com/gnolang/gno.git
cd ./gno
make
```
## Cüzdan oluşturalım 

```bash
./build/gnokey add validator
```
## Musluktan biraz token alın

https://gno.land/faucet 

## Gnoland akıllı sözleşme çağrısı/isteği ile bir pano oluşturalım

`PANO_İSMİ` ile belirttiğimiz kısmı istediğiniz bir isim ile değiştirin (büyük harf ve boşluk kullanmayın)
sonrasında "PANO_İSMİ" gördüğünüz her yerde şimdi gireceğiniz ismi kullanmayı unutmayın!
```bash
./build/gnokey maketx call validator --pkgpath `gno.land/r/boards` --func CreateBoard --args `PANO_İSMİ` --gas-fee 1gnot --gas-wanted 3000000 > createboard.unsigned.txt
```

#### account number ve sequence bilgilerini öğrenin

`CÜZDAN_ADRESİNİZ` yazan her kısma cüzdan adresinizi yazmayı unutmayın!  
`account number` değişmez, ancak `sequence` gönderdiğiniz her başarılı işlem sonrası değişir, bu yüzden bunu gerekli yerlerde sık sık sorgulamamız gerekiyor.

```bash
./build/gnokey query auth/accounts/CÜZDAN_ADRESİNİZ --remote gno.land:36657
```

#### `HESAP_NUMARASI` ve `SEQUENCE_NUMARASI` yazan kısmı üstte ki komutun çıktısıyla değiştirin. (Komutun çıktısında sequence ve account_number bilgilerini görmelisiniz)

```bash
./build/gnokey sign validator --txpath createboard.unsigned.txt --chainid "testchain" --number HESAP_NUMARASI --sequence SEQUENCE_NUMARASI > createboard.signed.txt
./build/gnokey broadcast createboard.signed.txt --remote gno.land:36657
```

## Oluşturduğumuz panonun id'sini öğrenelim
```bash
./build/gnokey query "vm/qeval" --data "gno.land/r/boards
GetBoardIDFromName(\"PANO_İSMİ\")" --remote gno.land:36657
```

Çıktı şuna benzeyecektir;
```bash
height: 0
data: (149 gno.land/r/boards.BoardID)
(true bool)
```
Benim durumumda, id 149'dur. siz de bu numara farklı olacak. 

## Oluşturduğumuz pano'da, bir gönderi yayınlayalım
`PANO_İD` yazan her yere, az önce sorgulayıp bulduğumuz İD numarasını yazın. 

```bash
./build/gnokey maketx call validator --pkgpath "gno.land/r/boards" --func CreatePost --args PANO_İD --args "BURAYA PAYLAŞMAK İSTEDİĞİNİZ BİR BAŞLIK YAZIN" --args#file "./examples/gno.land/r/boards/README.md" --gas-fee 1gnot --gas-wanted 3000000 > createpost.unsigned.txt
```

### account number ve sequence bilgilerini öğrenin

```bash
./build/gnokey query auth/accounts/CÜZDAN_ADRESİNİZ --remote gno.land:36657
./build/gnokey sign validator --txpath createpost.unsigned.txt --chainid "testchain" --number HESAP_NUMARASI --sequence SEQUENCE_NUMARASI > createpost.signed.txt
./build/gnokey broadcast createpost.signed.txt --remote gno.land:36657
```

## Oluşturduğumuz gönderiye yorum yapalım

#### pano id'yi değiştirmeyi unutma! `Örnek yorum` yazan kısmı istediğiniz bir cümle ile değiştirebilirsiniz.

```basg
./build/gnokey maketx call validator --pkgpath "gno.land/r/boards" --func "CreateReply" --args "PANO_İD" --args "1" --args "1" --args "ÖRNEK_YORUM" --gas-fee 1gnot --gas-wanted 3000000 > createcomment.unsigned.txt
```

## account number ve sequence bilgilerini öğrenin
```bash
./build/gnokey query auth/accounts/CÜZDAN_ADRESİNİZ --remote gno.land:36657

./build/gnokey sign validator --txpath createcomment.unsigned.txt --chainid "testchain" --number HESAP_NUMARASI --sequence SEQUENCE_NUMARASI > createcomment.signed.txt
./build/gnokey broadcast createcomment.signed.txt --remote gno.land:36657
```

Yorumu bu şekilde sorgulayabilirsiniz.; 

```bash
./build/gnokey query "vm/qeval" --data "gno.land/r/boards
GetBoardIDFromName(\"PANO_İSMİ\")" --remote gno.land:36657
```

# Bir paket yayınlayın

```bash
./build/gnokey maketx addpkg validator --pkgpath "gno.land/p/avl" --pkgdir "examples/gno.land/p/dom" --deposit 100gnot --gas-fee 1gnot --gas-wanted 2000000 > addpkg.avl.unsigned.txt
```

## account number ve sequence bilgilerini öğrenin

```bash
./build/gnokey query "auth/accounts/CÜZDAN_ADRESİNİZ" --remote gno.land:36657
./build/gnokey sign validator --txpath addpkg.avl.unsigned.txt --chainid "testchain" --number HESAP_NUMARASI --sequence SEQUENCE_NUMARASI > addpkg.avl.signed.txt
```
Bu adımda hata alabilirsiniz. "package already exist" tarzında bir hata mesajı çıkıyorsa, [bu linkte ki](https://github.com/gnolang/gno/tree/master/examples/gno.land/p) diğer paketleri deneyin. `--pkgdir "examples/gno.land/p/dom` >> `dom` yazan kısmı burada ki diğer paketler ile değiştirip deneyin. (Bu adımda ki ilk komuttan bahsediyoruz)

```bash
./build/gnokey broadcast addpkg.avl.signed.txt --remote gno.land:36657
```

# Bir realm paketi yayınlayın

```bash
./build/gnokey maketx addpkg validator --pkgpath "gno.land/r/boards" --pkgdir "examples/gno.land/r/boards" --deposit 100gnot --gas-fee 1gnot --gas-wanted 300000000 > addpkg.boards.unsigned.txt
./build/gnokey sign validator --txpath addpkg.boards.unsigned.txt --chainid "testchain" --number HESAP_NUMARASI --sequence SEQUENCE_NUMARASI > addpkg.boards.signed.txt
```
# Bu adımda da aynı şekilde hata aldım, ancak timeout hatası. çözümünü bilmiyorum ne yazık ki. Sorunun kaynağından emin değilim.

```
./build/gnokey broadcast addpkg.boards.signed.txt --remote gno.land:36657
```
