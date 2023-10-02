# Serialization

## EXAMPLE: Sending data (including a binary file) using multipart/form-data:

```
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
```

## EXAMPLE: Sending data (including a binary file) using application/json:

```
POST /api/multipart HTTP/1.1
... (Headers)
...
content-type: application/json
... (More Headers)
...

{"pieName":"Pecan","pieIngredients":["pecans","sugar","more sugar","even more sugar","crust"],"favoritePie":true,"inventory":10,"image":{}}
```

# Gotchas:

1. There is currently no way to MANUALLY set the multipart/form-data boundary which is necessary for the serializing and deserializing of the payload data. As a direct result of this, we MUST NOT set the 'Content-Type' header when using multipart/form-data and the FormData class that is defined by Javascript. By omitting this header, we are forcing the browser to examine the payload and set these values for us. Example below.
```
const requestData = new FormData();
requestData.append('file', file); // file from File API

const response = await fetch('/backend/upload', {
    method: 'POST',
    headers: {
        'Accept': 'application/json'
    },
    body: requestData
});
```

2. The JSON format does not understand how to encode binary data. It observes 3 basic data types: Number, String, and Boolean. These types are generally inferred on the receiving end by the pressence or lack of surrounding quotes. "boolean":true will be parsed as a boolean, while "boolean":"true" would be parsed as a string. We can make a similar example for numbers. In addition to these basic types, JSON is capable of sending reference data types, such as Objects and Arrays, by first encoding them as strings (shown above). Ultimately, a JSON payload is sent as a string and parsed into it's constituent parts on the receiving side. In the event that we NEED to send a binary file using JSON formatting, we must first convert the binary file content into a base64 encoded string. This not only increases the size of the payload data, as the encoding generally increases the size of the file content by 25%, but also adds the overhead of encoding and decoding the file content as a part of our serialization procedure. Example below.

```
  const sendJSON = async (e) => {
    e.preventDefault();

    const convertBase64 = (file) => {
      return new Promise((resolve, reject) => {
        const fileReader = new FileReader();
        fileReader.readAsDataURL(file);

        fileReader.onload = () => {
          resolve(fileReader.result);
        };

        fileReader.onerror = (error) => {
          reject(error);
        };
      });
    };

    let encodedBinary = await convertBase64(file);

    const toSerialize = {
      pieName: "Pecan",
      pieIngredients: ["pecans", "sugar", "more sugar", "even more sugar", "crust"],
      favoritePie: true,
      inventory: 10,
      image: encodedBinary,
    }

    fetch('/api/multipart', {
      method: "POST",
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(toSerialize)
    });
  }

// Our JSON Payload: {"pieName":"Pecan","pieIngredients":["pecans","sugar","more sugar","even more sugar","crust"],"favoritePie":true,"inventory":10,"image":"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAhCAYAAACbffiEAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAUGVYSWZNTQAqAAAACAACARIAAwAAAAEAAQAAh2kABAAAAAEAAAAmAAAAAAADoAEAAwAAAAEAAQAAoAIABAAAAAEAAAAyoAMABAAAAAEAAAAhAAAAAEBWbOcAAAIyaVRYdFhNTDpjb20uYWRvYmUueG1wAAAAAAA8eDp4bXBtZXRhIHhtbG5zOng9ImFkb2JlOm5zOm1ldGEvIiB4OnhtcHRrPSJYTVAgQ29yZSA2LjAuMCI+CiAgIDxyZGY6UkRGIHhtbG5zOnJkZj0iaHR0cDovL3d3dy53My5vcmcvMTk5OS8wMi8yMi1yZGYtc3ludGF4LW5zIyI+CiAgICAgIDxyZGY6RGVzY3JpcHRpb24gcmRmOmFib3V0PSIiCiAgICAgICAgICAgIHhtbG5zOmV4aWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20vZXhpZi8xLjAvIgogICAgICAgICAgICB4bWxuczp0aWZmPSJodHRwOi8vbnMuYWRvYmUuY29tL3RpZmYvMS4wLyI+CiAgICAgICAgIDxleGlmOlBpeGVsWURpbWVuc2lvbj4xMjA8L2V4aWY6UGl4ZWxZRGltZW5zaW9uPgogICAgICAgICA8ZXhpZjpDb2xvclNwYWNlPjE8L2V4aWY6Q29sb3JTcGFjZT4KICAgICAgICAgPGV4aWY6UGl4ZWxYRGltZW5zaW9uPjEyMDwvZXhpZjpQaXhlbFhEaW1lbnNpb24+CiAgICAgICAgIDx0aWZmOk9yaWVudGF0aW9uPjE8L3RpZmY6T3JpZW50YXRpb24+CiAgICAgIDwvcmRmOkRlc2NyaXB0aW9uPgogICA8L3JkZjpSREY+CjwveDp4bXBtZXRhPgrmIV0+AAACDElEQVRYCWOsjFL+z4AEGBl/IPEQzJfvvyM4UBYzMxOGWKSvCIYYSICDiRlDvG/aXQwxkICqMieGuJQsO4aYo4sAXAzTJXCpocUY9chgiy/GpgQFlDwiJMWK1Y3uHjwY4lPaX2KIMbF9wxADCbR18mKIM7J+xhADCfx8g+IksJrHT6Uw1H5nQOSb0aSFETwDLDAaIwMcARjWD5sYYeETQK1xF85/geFbkMDzY6IY4pI/EKUGTPI3K2aJA5JrjcVsMbBiNhbAxhz7huomkOCOTbfAcsjENwYuOHfYxMioR+BxOkgYwyZGWN7+YUQJ01PT+VH4MM6Kg4IwJpzWNFCAs2GMreuPw5goNDvTbxQ+iKNrYYghBhIw/PwAQ/z9X24MMS4ORMEybGJk1CMY8TzAAqMxMsARgGE9Y1WaIiLrA6X//8NQAxZwlMPsWLlaYyr+8AGzdAMZwMaD2WHj5MNstoDUXv70BkShAAFBzE7Y138Iu0aTFkpwDQLOaIwMgkhAcQLL3/9sKAIM/7BnwF0336GqA/I2XUFt3oAU/PvzGkMdSICdHbOPIaXJh1WtoxFmP+cPI2bBsm35L7j+0aQFD4pBwhiNkUESEXBnDJsYYbn78APcVyCGthLmGC1InI0Ns4nx+QvmMAgfHwtIOQYIDkM0J2CSvJxfYEwU+stftJIUKDt/KmapVVeCKGEB2pthgibfv0gAAAAASUVORK5CYII="}
```
3. Please note that there is NO direct correlation between flask's wtforms and multipart/form-data. Although when you use multipart/form-data, you will continue to find the respective values in 'form.data['key_name']', just as if you had used a JSON formatted payload, the 'form.data' itself NEVER implies multipart/form-data in and of itself! 

# Recommendations:

In consideration of the above, it is generally recommended to send binary data (image, audio, video files) using multipart/form-data serialization as it is simpler to implement and is less resource intensive. When doing so, DO NOT set the 'Content-Type' Header on your request!!
