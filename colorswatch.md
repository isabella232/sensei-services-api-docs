# Sensei Wins AutoSwatch Service

Service exposing an http-based API for automatic computation of image swatches, based on the native [AutoSwatch](https://git.corp.adobe.com/jianmzha/AutoSwatch/tree/master/AutoSwatchLib) library.


## API

`POST /api/autoswatch`

Swatch suggestions for a given image can be generated by sending `multipart/form-data` POST requests to `/api/autoswatch` where:

   * a binary body part named `image` provides the raw image data
   * a text body part named `numSwatches` specifies the number of swatch suggestions to return (optional, default: 5)
   * a text body part named `swatchSize`: length in pixels of one side of the swatch square, if 0 it will be automatically determined (optional, default: 0)

On successful completion the server returns a JSON array of generated swatch suggestions (ordered by score).

Example:

```
    POST http://localhost:8888/api/autoswatch HTTP/1.1
    Content-Type: multipart/form-data; boundary=---------------------------41184676334

    -----------------------------41184676334
    Content-Disposition: form-data; name="image"; filename="test.jpg"
    Content-Type: image/jpeg

    (Binary data not shown)
    -----------------------------41184676334
    Content-Disposition: form-data; name="numSwatches"

    4
    -----------------------------41184676334
    Content-Disposition: form-data; name="swatchSize"

    0
    -----------------------------41184676334--


    HTTP/1.1 200 OK
    Content-Type: application/json

    [
      { 
        "xMin": 237,
        "xMax": 277,
        "yMin": 237,
        "yMax": 277,
        "score": 1545.68798828125 
      },
      { 
        "xMin": 396,
        "xMax": 435,
        "yMin": 375,
        "yMax": 415,
        "score": 356.76751708984375 
      },
      { 
        "xMin": 752,
        "xMax": 792,
        "yMin": 335,
        "yMax": 375,
        "score": 76.80172729492188 
      },
      { 
        "xMin": 375,
        "xMax": 415,
        "yMin": 198,
        "yMax": 237,
        "score": 21.88166046142578 
      }
    ]
```