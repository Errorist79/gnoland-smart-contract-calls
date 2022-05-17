
* validator (local) - addr: g1l2hd0mw0j0ufk6yd5r0rjj8g2txuqx6ew322yy pub: gpub1pgfj7ard9eg82cjtv4u4xetrwqer2dntxyfzxz3pqwtlvr03fsup7ucq43y7zrnf7jgfayf073x8kems5u9hlae6emd4wwlkd4p, path: <nil>

**IMPORTANT** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

unfair hello match hope cattle tragic cable surround path pet happy hobby desert early elbow deliver mansion clock top roast fame sauce make slam

# Gnoland'ı indirelim ve kuralım
git clone https://github.com/gnolang/gno.git
cd ./gno
make

# Cüzdan oluşturalım 
./build/gnokey add validator

./build/gnokey query auth/accounts/g1l2hd0mw0j0ufk6yd5r0rjj8g2txuqx6ew322yy --remote gno.land:36657

# Musluktan biraz token alın
https://gno.land/faucet 

# Gnoland akıllı sözleşme çağrısı/isteği ile bir pano oluşturalım
"PANO_İSMİ" ile belirttiğimiz kısmı istediğiniz bir isim ile değiştirin (büyük harf ve boşluk kullanmayın)
sonrasında "PANO_İSMİ" gördüğünüz her yerde şimdi gireceğiniz ismi kullanmayı unutmayın!

./build/gnokey maketx call validator --pkgpath "gno.land/r/boards" --func CreateBoard --args "PANO_İSMİ" --gas-fee 1gnot --gas-wanted 3000000 > createboard.unsigned.txt

# account number ve sequence bilgilerini öğrenin
"CÜZDAN_ADRESİNİZ" yazan her kısma cüzdan adresinizi yazmayı unutmayın!  
"account number" değişmez, ancak "sequence" gönderdiğiniz her başarılı işlem sonrası değişir, bu yüzden bunu gerekli yerlerde sık sık sorgulamamız gerekiyor.

./build/gnokey query auth/accounts/CÜZDAN_ADRESİNİZ --remote gno.land:36657

# HESAP_NUMARASI ve SEQUENCE_NUMARASI yazan kısmı üstte ki komutun çıktısıyla değiştirin. (Komutun çıktısında sequence ve account_number bilgilerini görmelisiniz)
./build/gnokey sign validator --txpath createboard.unsigned.txt --chainid "testchain" --number HESAP_NUMARASI --sequence SEQUENCE_NUMARASI > createboard.signed.txt
./build/gnokey broadcast createboard.signed.txt --remote gno.land:36657

# Oluşturduğumuz panonun id'sini öğrenelim
./build/gnokey query "vm/qeval" --data "gno.land/r/boards
GetBoardIDFromName(\"PANO_İSMİ\")" --remote gno.land:36657

Çıktı şuna benzeyecektir;
height: 0
data: (149 gno.land/r/boards.BoardID)
(true bool)
Benim durumumda, id 149'dur. siz de bu numara farklı olacak. 

# Oluşturduğumuz pano'da, bir gönderi yayınlayalım
"PANO_İD" yazan her yere, az önce sorgulayıp bulduğumuz İD numarasını yazın. 

./build/gnokey maketx call validator --pkgpath "gno.land/r/boards" --func CreatePost --args PANO_İD --args "BURAYA PAYLAŞMAK İSTEDİĞİNİZ BİR BAŞLIK YAZIN" --args#file "./examples/gno.land/r/boards/README.md" --gas-fee 1gnot --gas-wanted 3000000 > createpost.unsigned.txt

# account number ve sequence bilgilerini öğrenin

./build/gnokey query auth/accounts/CÜZDAN_ADRESİNİZ --remote gno.land:36657
./build/gnokey sign validator --txpath createpost.unsigned.txt --chainid "testchain" --number HESAP_NUMARASI --sequence SEQUENCE_NUMARASI > createpost.signed.txt
./build/gnokey broadcast createpost.signed.txt --remote gno.land:36657

# Oluşturduğumuz gönderiye yorum yapalım
## pano id'yi değiştirmeyi unutma! "Örnek yorum" yazan kısmı istediğiniz bir cümle ile değiştirebilirsiniz.
./build/gnokey maketx call validator --pkgpath "gno.land/r/boards" --func "CreateReply" --args "PANO_İD" --args "1" --args "1" --args "ÖRNEK_YORUM" --gas-fee 1gnot --gas-wanted 3000000 > createcomment.unsigned.txt

## account number ve sequence bilgilerini öğrenin
./build/gnokey query auth/accounts/CÜZDAN_ADRESİNİZ --remote gno.land:36657

./build/gnokey sign validator --txpath createcomment.unsigned.txt --chainid "testchain" --number HESAP_NUMARASI --sequence SEQUENCE_NUMARASI > createcomment.signed.txt
./build/gnokey broadcast createcomment.signed.txt --remote gno.land:36657

Yorumu bu şekilde sorgulayabilirsiniz.; 

./build/gnokey query "vm/qeval" --data "gno.land/r/boards
GetBoardIDFromName(\"PANO_İSMİ\")" --remote gno.land:36657


# Bir paket yayınlayın
./build/gnokey maketx addpkg validator --pkgpath "gno.land/p/avl" --pkgdir "examples/gno.land/p/dom" --deposit 100gnot --gas-fee 1gnot --gas-wanted 2000000 > addpkg.avl.unsigned.txt

## account number ve sequence bilgilerini öğrenin
./build/gnokey query "auth/accounts/CÜZDAN_ADRESİNİZ" --remote gno.land:36657
./build/gnokey sign validator --txpath addpkg.avl.unsigned.txt --chainid "testchain" --number HESAP_NUMARASI --sequence SEQUENCE_NUMARASI > addpkg.avl.signed.txt
# Bu adımda hata alabilirsiniz. "package already exist" tarzında bir hata mesajı çıkıyorsa, bu linkte ki https://github.com/gnolang/gno/tree/master/examples/gno.land/p diğer paketleri deneyin. --pkgdir "examples/gno.land/p/dom" "dom" yazan kısmı burada ki diğer paketler ile değiştirip deneyin.

./build/gnokey broadcast addpkg.avl.signed.txt --remote gno.land:36657


# Bir realm paketi yayınlayın

./build/gnokey maketx addpkg validator --pkgpath "gno.land/r/boards" --pkgdir "examples/gno.land/r/boards" --deposit 100gnot --gas-fee 1gnot --gas-wanted 300000000 > addpkg.boards.unsigned.txt
./build/gnokey sign validator --txpath addpkg.boards.unsigned.txt --chainid "testchain" --number HESAP_NUMARASI --sequence SEQUENCE_NUMARASI > addpkg.boards.signed.txt

# Bu adımda da aynı şekilde hata aldım, ancak timeout hatası. çözümünü bilmiyorum ne yazık ki. Sorunun kaynağından emin değilim.
./build/gnokey broadcast addpkg.boards.signed.txt --remote gno.land:36657




