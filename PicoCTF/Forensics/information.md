### Challenge: information

The author of this challenge provides an image. We have to read the image's metadata to find our flag. We can do it with 2 ways.
  - First way: Use an online tool like the [Extract Image Metadata Tool](https://brandfolder.com/workbench/extract-metadata).
  - Second way: Open the directory where you saved your image on any Terminal and type `$ sudo apt install imagemagick`. This will install a tool called ImageMagick. After the installation you have to type `identify -verbose image.png`.

Both ways print the same metadata. Our flag is stored in the license of the image but its also encoded to base64. After decoding it with `echo (license)|base64 --decode` we get the flag.
