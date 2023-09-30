# Serialization

## EXAMPLE: Sending data (including a binary file) using multipart/form-data:

POST /api/multipart HTTP/1.1
... (Headers)
...
content-type: multipart/form-data; boundary=----WebKitFormBoundaryiZLewAfltKpc8KJI
... (More Headers)
...

------WebKitFormBoundaryiZLewAfltKpc8KJI
Content-Disposition: form-data; name="pieName"

Pecan
------WebKitFormBoundaryiZLewAfltKpc8KJI
Content-Disposition: form-data; name="pieIngredients"

pecans,sugar,more sugar,even more sugar,crust
------WebKitFormBoundaryiZLewAfltKpc8KJI
Content-Disposition: form-data; name="favoritePie"

true
------WebKitFormBoundaryiZLewAfltKpc8KJI
Content-Disposition: form-data; name="inventory"

10
------WebKitFormBoundaryiZLewAfltKpc8KJI
Content-Disposition: form-data; name="image"; filename="low_res_shrek.png"
Content-Type: image/png

PNG


IHDR2!}øgAMA±üa cHRMz&úèu0ê`:pºQ<PeXIfMM*i&  2 !@Vlç2iTXtXML:com.adobe.xmp<x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0">
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
      <rdf:Description rdf:about=""
            xmlns:exif="http://ns.adobe.com/exif/1.0/"
            xmlns:tiff="http://ns.adobe.com/tiff/1.0/">
         <exif:PixelYDimension>120</exif:PixelYDimension>
         <exif:ColorSpace>1</exif:ColorSpace>
         <exif:PixelXDimension>120</exif:PixelXDimension>
         <tiff:Orientation>1</tiff:Orientation>
      </rdf:Description>
   </rdf:RDF>
</x:xmpmeta>
æ!]>IDATX	c¬RþÏ ñÌï¿#8P33X¤¯HC¼oÚ]1ª2'¸,;£\Ó%p©¡ÅõÈ`/Æ¦<"$ÅÕî<âSÚ_b1±}Ã	´uòb3²~Æ	ü|â$°ÇO¥0Ô~g@äÑ¤<,0#Öaá@qÎá[Àóc¢â?¥Lò7+fkÅl1°b6ÀÆûê&àM·ÀrÈÄ7.8wØÄÈ¨Gàq:HÃ&FXÞþaD	ÓSÓùQø0Î0&Ö4P³aëÃ(4;Óo>£ka!0üüCüý_n1.DÁ2lbdÔ#ñ<À£12Àa=cU""ë¥ÿÿÃPpÃìX¹Zc*þð³tÀÆÙaãäÃl¶Ô^þôD¡AÌNØ×»FJp
ÎhH@qËßÿl(ÿ°gÀ]7ß¡ªò6]AmÞüûóCH³!¥ÉU£f?ç#fÁ²mù/¸þÑ¤AÂApga¹ûðÜW ¶æ-H
³ñùæ0H9C4'`¼_`LúË_´(;*f©UW(aÚa&ß¿HIEND®B`
------WebKitFormBoundaryiZLewAfltKpc8KJI--


## EXAMPLE: Sending data (including a binary file) using application/json:

POST /api/multipart HTTP/1.1
... (Headers)
...
content-type: application/json
... (More Headers)
...

{"pieName":"Pecan","pieIngredients":["pecans","sugar","more sugar","even more sugar","crust"],"favoritePie":true,"inventory":10,"image":{}}


