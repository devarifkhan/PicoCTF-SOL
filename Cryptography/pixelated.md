# Cryptography
## Pixelated - 100 Points
## Question
> I have these 2 images, can you make a flag out of them? scrambled1.png scrambled2.png

## Solution
> We receive two PNG files of 256x256, with what looks like random pixel values.
Well, there's only so much we can do to combine them to something that will produce a flag. In this case, we had to add the different pixel values:
```python
from PIL import Image
import numpy as np
import os
file_names = ["scrambled1.png", "scrambled2.png"]
img_data = [np.asarray(Image.open(f'{name}')) for name in file_names]
data = img_data[0].copy() + img_data[1].copy()
new_image = Image.fromarray(data)
new_image.save("out.png", "PNG")
```

> Flag is: picoCTF{d72ea4af}
