
```python
def exif(img):

    exif_data = {} 
    try: 
        i = Image.open(img) 
        tags = i._getexif() 
        for tag, value in tags.items(): 
            decoded = TAGS.get(tag, tag) 
            exif_data[decoded] = value 
    except: 
        pass 
    return exif_data 
```

Example:

![](../../assets/Code_Snippets/General/Error%20Handling.png)