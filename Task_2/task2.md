

# TASK 2 — File Handling & Getting started with Pillow

> Before touching pixels, you need to be comfortable opening, inspecting, converting, and saving images as files. 

---

### Task 2.1 — Installation 

- Install Pillow 

         pip install Pillow
- verify if it works
        To verify it worked, open a Python shell and type:
```python
from PIL import Image
print(Image.__version__)
```
- write a Script to open a txt file write multiple lines to it and read from it and print to screen.

- Explaination how images are stored 

    https://youtu.be/06OHflWNCOE

- Write a script that opens any image of your choice and prints the following:
  - File format (JPEG, PNG, etc.)
  - Image mode (RGB, RGBA, L, etc.)
  - Image size (width × height in pixels)

```python
    image.open(filepath) # opens an image file and returns an Image object
    image.format # the file format as a string e.g. `"JPEG"`, `"PNG"`
    image.mode # the color mode as a string e.g. `"RGB"`, `"RGBA"`, `"L"`
    image.size # a tuple `(width, height)` in pixels
```

**Expected terminal output:**
```
Format : JPEG
Mode   : RGB
Size   : 1920 x 1080
```

**Explore**
- What does "mode" mean for an image? Why might two images of the same scene have different modes?

- What happens if you try to open a file that doesn't exist? and how to handle the error . 
---

### Task 2.2 — Format Conversion & Saving

**What to do:**
*  Write a script that takes a JPEG image and saves it as PNG, and vice versa.

    - `Image.open(filepath)` — to open the source image
    - `image.save(output_path)` — saves the image; Pillow detects format from the file extension
    - `image.convert("RGB")` — converts the image mode before saving (you'll need this — read why below)

- Check the file sizes of the originals vs the converted files.
```python
        import os

        file_path = "example.jpg" # Replace with your file path
        size_bytes = os.path.getsize(file_path)
        print(f"The size of the file is: {size_bytes} bytes")
```        
- (optional) Convert to grayscale
    - `image.convert("L")` — converts any image to grayscale. `"L"` = luminance (brightness only)


**Expected:** Script `TASK 2/convert.py` + converted images saved to folder.

**Explore:**
- Why is a PNG of the same image larger or smaller than its JPEG?
- What information is lost when you convert to grayscale?
- What does `image.convert('RGB')` do and when would you need it?

---

### Task 2.3 — Metadata & Image Properties (struggle time Task)


**Background:** JPEG images taken on phones or cameras store extra information called **EXIF data** — things like GPS coordinates, camera model, date/time, and exposure settings. This is embedded inside the image file itself, invisible to the naked eye.
 
**Functions you'll need:**
- `image._getexif()` — returns a dictionary of raw EXIF tag IDs and values (JPEG only)
- `ExifTags.TAGS` — a dictionary that maps tag ID numbers to human-readable names
 
**Pseudocode:**
```
from PIL import Image, ExifTags
 
open a JPEG photo as img 
 
call img._getexif() and store result in exif_data
 
if exif_data is None:
    print "No EXIF data found"
else:
    loop over each (tag_id, value) in exif_data.items():
        look up the tag name using ExifTags.TAGS.get(tag_id, "Unknown")
        print tag_name and value
```
> 💡 `_getexif()` returns raw tag IDs (numbers like `271`, `272`, `306`). They're meaningless on their own. `ExifTags.TAGS` is a dictionary that maps `271 → "Make"`, `272 → "Model"`, `306 → "DateTime"` etc. Use `.get(tag_id, "Unknown")` so you don't crash on unrecognised tags.
 
> 💡 Some values will be long and unreadable bytes. That's fine — just print what you can. Focus on finding GPS coordinates, device model, and date/time if they exist.

> 💡 By default, Pillow **strips EXIF data** when you resave. This is actually useful for steganography tools — but also raises an interesting question: if you need to *preserve* EXIF, how would you do it? Look up the `exif` parameter in `image.save()`.
 
**Explore**
- What is EXIF data and where is it stored?
- Why does it matter from a privacy perspective?
- Does Pillow preserve or strip EXIF on resave by default?
- Why might stripping EXIF matter (or not matter) for a steganography tool?
 
**Expected:** Script `TASK 2/metadata.py`


---

### ✅ Week 1 Checkpoint

Before moving on, you should be able to answer these without looking anything up:
- [ ] How do you open an image with Pillow?
- [ ] What are the most common image modes and what do they represent?
- [ ] How do you save an image in a different format?
- [ ] How do you resize an image?
- [ ] (optional) How do you safely handle file I/O errors?

## 📚 Reference Links

- [Pillow Documentation](https://pillow.readthedocs.io/en/stable/)
- [Image module of Pillow](https://pillow.readthedocs.io/en/stable/reference/Image.html)
- [Python Bitwise Operators](https://wiki.python.org/moin/BitwiseOperators)
- [ASCII Table](https://www.asciitable.com/)
- [LSB Steganography — Wikipedia](https://en.wikipedia.org/wiki/Steganography#Digital_steganography)
- [NumPy for Images](https://numpy.org/doc/stable/user/quickstart.html)

---