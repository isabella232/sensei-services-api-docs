# Sensei Wins Auto-Crop Service

This service exposes an http-based API for automatic computation of image crops, based on the AutoCrop2 library.


## API

`POST /api/autocrop`

Crop suggestions for a given image can be generated by sending `multipart/form-data` POST requests to `/api/autocrop` where:

   * a binary body part named `image` provides the raw image data
   * a text body part named `numSuggestions` specifies the number of crop suggestions to return (optional, default: 4)
   * a text body part named `perAspectRatio`: `true` if `numSuggestions` should be returned per aspect ratio, `false` if `numSuggestions` refers to the overall number of suggestions (optional, default: true)
   * a text body part named `aspectRatios`: a comma separated list of aspect ratios of crop rectangles (optional, default: 64/27, 16/9, 3/2, 4/3, 3/4, 2/3)
   * a text body part named `cropRectScaleRatios` a comma separated list of crop rectangle scale ratios (optional, default: 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0)
   * a text body part named `useFaceDetect`: `true` if faces should be detected and used them for crop selections (optional, default: true)
   * a text body part named `sortType` specifies the type of sort (one of `all`, `composition`, `saliency`, `cutThrough`, `saliencySparse`, `centerness`) (optional, default: all)

On successful completion the server returns a JSON object with an array of generated crop suggestions (ordered by overall score).

Example:

```
    POST http://localhost:8888/api/autocrop HTTP/1.1
    Content-Type: multipart/form-data; boundary=---------------------------41184676334

    -----------------------------41184676334
    Content-Disposition: form-data; name="image"; filename="test.jpg"
    Content-Type: image/jpeg

    (Binary data not shown)
    -----------------------------41184676334
    Content-Disposition: form-data; name="numSuggestions"

    4
    -----------------------------41184676334
    Content-Disposition: form-data; name="perAspectRatio"

    true
    -----------------------------41184676334
    Content-Disposition: form-data; name="aspectRatios"

    64/27, 16/9, 3/2, 4/3, 3/4, 2/3
    -----------------------------41184676334
    Content-Disposition: form-data; name="cropRectScaleRatios"

    0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0
    -----------------------------41184676334
    Content-Disposition: form-data; name="useFaceDetect"

    true
    -----------------------------41184676334
    Content-Disposition: form-data; name="sortType"

    all
    -----------------------------41184676334--


    HTTP/1.1 200 OK
    Content-Type: application/json

    {
      "cropSuggestions": [
        {
          "xMin": 12,
          "xMax": 933,
          "yMin": 51,
          "yMax": 665,
          "goodCandidate": true,
          "score": {
            "isComputed": true,
            "composition": 0.5272046327590942,
            "saliency": 0.999984622001648,
            "cutThrough": 0.5878231525421143,
            "saliencySparse": 1,
            "centerness": 0.03821255639195442,
            "objPenalty": 1.181644320487976,
            "overallScore": 128.6769389919098
          }
        },
        {
          "xMin": 80,
          "xMax": 990,
          "yMin": 0,
          "yMax": 682,
          "goodCandidate": true,
          "score": {
            "isComputed": true,
            "composition": 0.48188623785972595,
            "saliency": 1,
            "cutThrough": 0.6612163782119751,
            "saliencySparse": 1,
            "centerness": 0.332835853099823,
            "objPenalty": 1.1816644668579102,
            "overallScore": 128.66142010479786
          }
        },
        {
          "xMin": 93,
          "xMax": 912,
          "yMin": 25,
          "yMax": 639,
          "goodCandidate": true,
          "score": {
            "isComputed": true,
            "composition": 0.5162208676338196,
            "saliency": 0.9880324602127075,
            "cutThrough": 0.5803415179252625,
            "saliencySparse": 1,
            "centerness": 0.05272615700960159,
            "objPenalty": 1.181664228439331,
            "overallScore": 128.6557898310332
          }
        },
        {
          "xMin": 0,
          "xMax": 1023,
          "yMin": 8,
          "yMax": 583,
          "goodCandidate": true,
          "score": {
            "isComputed": true,
            "composition": 0.5152865052223206,
            "saliency": 0.9313555955886841,
            "cutThrough": 0.6267793774604797,
            "saliencySparse": 1,
            "centerness": 0.5885596871376038,
            "objPenalty": 1.1816564798355103,
            "overallScore": 128.64725550679543
          }
        }
      ],
      "rectsOfInterest": [
        {
          "xMin": 368,
          "xMax": 437,
          "yMin": 99,
          "yMax": 168,
          "isFace": true,
          "enabled": true,
          "rectOfInterestType": "kPreserve"
        },
        {
          "xMin": 675,
          "xMax": 740,
          "yMin": 103,
          "yMax": 168,
          "isFace": true,
          "enabled": true,
          "rectOfInterestType": "kPreserve"
        },
        {
          "xMin": 571,
          "xMax": 678,
          "yMin": 228,
          "yMax": 335,
          "isFace": true,
          "enabled": true,
          "rectOfInterestType": "kPreserve"
        }
      ],
    }
```  