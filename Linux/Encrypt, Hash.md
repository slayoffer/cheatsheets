## Encode and Decode a string in base64

We pass a text using the pipeline operator to the base64 command to encode it and add the “- - decode” flag to decode it to the original value. It’s as simple as that.

```bash
echo password1 | base64
echo xxx | base64 --decode
```