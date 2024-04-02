# c4ptur3-th3-fl4g
## 1) Translation & Shifting
### 1. c4n y0u c4p7u23 7h3 f149?
#### answer : can you capture the flag?

---

### 2. 01101100 01100101 01110100 01110011 00100000 01110100 01110010 01111001 00100000 01110011 01101111 01101101 01100101 00100000 01100010 01101001 01101110 01100001 01110010 01111001 00100000 01101111 01110101 01110100 00100001
#### answer : lets try some binary out!

---

### 3. MJQXGZJTGIQGS4ZAON2XAZLSEBRW63LNN5XCA2LOEBBVIRRHOM======
#### answer : base32 is super common in CTF's

---

### 4. RWFjaCBCYXNlNjQgZGlnaXQgcmVwcmVzZW50cyBleGFjdGx5IDYgYml0cyBvZiBkYXRhLg==
#### answer : Each Base64 digit represents exactly 6 bits of data.

---

### 5. 68 65 78 61 64 65 63 69 6d 61 6c 20 6f 72 20 62 61 73 65 31 36 3f
#### answer : hexadecimal or base16?

--- 

### 6. Ebgngr zr 13 cynprf!
#### answer : Rotate me 13 places!

---

### 7. *@F DA:? >6 C:89E C@F?5 323J C:89E C@F?5 Wcf E:>6DX
#### answer : You spin me right round baby right round (47 times)

---

### 8. - . .-.. . -.-. --- -- -- ..- -. .. -.-. .- - .. --- -.<br>. -. -.-. --- -.. .. -. --.
#### answer : TELECOMMUNICATIONENCODING

---

### 9. 85 110 112 97 99 107 32 116 104 105 115 32 66 67 68
#### answer : Unpack this BCD

---

### 10. LS0tLS0gLi0tLS0gLi0tLS0gLS0tLS0gLS0tLS0gLi0tLS0gLi0tLS0gLS0tLS0KLS0tLS0gLi0tLS0gLi0tLS0gLS0tLS0gLS0tLS0gLi0tLS0gLS0tLS0gLi0tLS0KLS0tLS0gLS0tLS0gLi0tLS0gLS0t<br>--snip---
#### answer : Let's make this a bit trickier...

---

webtools used : https://gchq.github.io/CyberChef/ 

---

## 2) Spectrograms
### 1. Download the file
#### answer : super secret message

---

webtools used : https://www.dcode.fr/spectral-analysis

---

## 3) Steganography
### 1. Decode the image to reveal the answer.
#### answer : SpaghettiSteg

---

webtools used : https://futureboy.us/stegano/decinput.html

---

## 4) Security through obscurity
### 1. Download and get 'inside' the file. What is the first filename & extension?
#### answer : hackerchat.png

---

### 2. Get inside the archive and inspect the file carefully. Find the hidden text.
#### answer : "AHH_YOU_FOUND_ME!"

---

````shell
┌──(grimlock㉿GRIMLOCK)-[~/Downloads]
└─$ strings meme_1559010886025.jpg             
JFIF
"Exif
$3br
%&'()*456789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
        #3R
&'()*56789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
~tQ@
~tQ@
~tQ@
~tQ@
I#mP*
FWr7`

--snip--

{@84
2$Es
i2Mc
IEND
"AHH_YOU_FOUND_ME!" 
hackerchat.png
````

