Forensic - Beulah
Description : Bersatu kita teguh, bercerai kita corrupted.
Kita diberikan sebuah file pcapng, mari kita buka dengan wireshark.
Kita dapat mengekstrak file yang di download melalui http, dengan melakukan File > Export Objects > HTTP
<br><br>
<p align="center">
<img src="https://github.com/laBigby/cscctf-foren/blob/master/101.JPG">
<br><br>
Terdapat content-type application/force-download dari www.yourfilelink.com, mari kita save file yang bernama direct.php tersebut.
<br><br>
<p align="center">
<img src="https://github.com/laBigby/cscctf-foren/blob/master/102.JPG">
<br><br>
Setelah dilihat ternyata file tersebut merupakan zip, mari di unzip
<br><br>
<p align="center">
<img src="https://github.com/laBigby/cscctf-foren/blob/master/103.JPG">
<br><br>
Didapatkan 7 file yang berisi base64, yang harus disatukan menjadi 1 file utuh dengan urutan yang benar pula untuk mendapatkan flag.
Penulis menyediakan pula hash agar saat proses permutasi file, bisa dibedakan mana file utuh yang benar dan mana file sampah yang corrupt sehingga tidak terlalu mengotori directory peserta
<br><br>
Hash yang diberikan 53453310bf3752492eb41cc0f4a948ad
<br><br>
Mari kita selesaikan dengan scripting
<br><br>
from itertools import permutations<br>
import os<br>
import hashlib<br>
x = ["file1","file2","file3","file4","file5","file6","file7"]<br>
bruh= list(permutations(x, 7))<br>
j=1<br>
for i in bruh:<br>
	os.system("cat "+i[0]+" > base"+str(j)+"\n")<br>
	os.system("cat "+i[1]+" >> base"+str(j)+"\n")<br>
	os.system("cat "+i[2]+" >> base"+str(j)+"\n")<br>
	os.system("cat "+i[3]+" >> base"+str(j)+"\n")<br>
	os.system("cat "+i[4]+" >> base"+str(j)+"\n")<br>
	os.system("cat "+i[5]+" >> base"+str(j)+"\n")<br>
	os.system("cat "+i[6]+" >> base"+str(j)+"\n")<br>
	os.system("base64 -d base"+str(j)+" > new"+str(j))<br>
	os.system("rm base"+str(j))<br>
	hasher = hashlib.md5()<br>
	with open('new'+str(j), 'rb') as afile:<br>
	    buf = afile.read()<br>
	    hasher.update(buf)<br>
	md5=hasher.hexdigest()<br>
	if(md5!="53453310bf3752492eb41cc0f4a948ad"):<br>
		os.system("rm new"+str(j))<br>
	else:<br>
		break<br>
	j+=1<br>
<br><br>
Kita mendapatkan flag !
<br><br>
<p align="center">
<img src="https://github.com/laBigby/cscctf-foren/blob/master/104.jpg">
<br><br>
Flag : CCC{its_pcap_m491c_h3h3}
