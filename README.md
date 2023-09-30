# Serialization

## EXAMPLE: Sending data (including a binary file) using multipart/form-data:

POST /api/multipart HTTP/1.1<br>
... (Headers)<br>
...<br>
content-type: multipart/form-data; boundary=----WebKitFormBoundaryiZLewAfltKpc8KJI<br>
... (More Headers)<br>
...<br>

------WebKitFormBoundaryiZLewAfltKpc8KJI<br>
Content-Disposition: form-data; name="pieName"<br>

Pecan<br>
------WebKitFormBoundaryiZLewAfltKpc8KJI<br>
Content-Disposition: form-data; name="pieIngredients"<br>

pecans,sugar,more sugar,even more sugar,crust<br>
------WebKitFormBoundaryiZLewAfltKpc8KJI<br>
Content-Disposition: form-data; name="favoritePie"<br>

true<br>
------WebKitFormBoundaryiZLewAfltKpc8KJI<br>
Content-Disposition: form-data; name="inventory"<br>

10<br>
------WebKitFormBoundaryiZLewAfltKpc8KJI<br>
Content-Disposition: form-data; name="image"; filename="low_res_shrek.png"<br>
Content-Type: image/png<br>

PNG<br>


IHDR2!}øgAMA±üa cHRMz&úèu0ê`:pºQ<PeXIfMM*i&  2 !@Vlç2iTXtXML:com.adobe.xmp<x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0"><br>
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"><br>
      <rdf:Description rdf:about=""<br>
            xmlns:exif="http://ns.adobe.com/exif/1.0/" <br>
            xmlns:tiff="http://ns.adobe.com/tiff/1.0/"> <br>
         <exif:PixelYDimension>120</exif:PixelYDimension> <br>
         <exif:ColorSpace>1</exif:ColorSpace><br>
         <exif:PixelXDimension>120</exif:PixelXDimension> <br>
         <tiff:Orientation>1</tiff:Orientation> <br>
      </rdf:Description> <br>
   </rdf:RDF><br>
</x:xmpmeta><br>
æ!]>IDATX	c¬RþÏ ñÌï¿#8P33X¤¯HC¼oÚ]1ª2'¸,;£\Ó%p©¡ÅõÈ`/Æ¦<"$ÅÕî<âSÚ_b1±}Ã	´uòb3²~Æ	ü|â$°ÇO¥0Ô~g@äÑ¤<,0#Öaá@qÎá[Àóc¢â?¥Lò7+fkÅl1°b6ÀÆûê&àM·ÀrÈÄ7.8wØÄÈ¨Gàq:HÃ&FXÞþaD<br>	ÓSÓùQø0Î0&Ö4P³aëÃ(4;Óo>£ka!0üüCüý_n1.DÁ2lbdÔ#ñ<À£12Àa=cU""ë¥ÿÿÃPpÃìX¹Zc*þð³tÀÆÙaãäÃl¶Ô^þôD¡AÌNØ×»FJp<br>
ÎhH@qËßÿl(ÿ°gÀ]7ß¡ªò6]AmÞüûóCH³!¥ÉU£f?ç#fÁ²mù/¸þÑ¤AÂApga¹ûðÜW ¶æ-H<br>
³ñùæ0H9C4'`¼_`LúË_´(;*f©UW(aÚa&ß¿HIEND®B`<br>
------WebKitFormBoundaryiZLewAfltKpc8KJI--<br>


## EXAMPLE: Sending data (including a binary file) using application/json:

POST /api/multipart HTTP/1.1<br>
... (Headers)<br>
...<br>
content-type: application/json<br>
... (More Headers)<br>
...<br>

{"pieName":"Pecan","pieIngredients":["pecans","sugar","more sugar","even more sugar","crust"],"favoritePie":true,"inventory":10,"image":{}}<br>


