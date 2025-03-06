# Compresor-Matrix
The Compresor Matrix (CM), is a compression technique that in itself all the text information to be compressed, converts it into a matrix table, and then inserts the values ​​of the matrix tables into pixels of an image on a color scale, each intensity corresponds to an acci value, now to decompress it checks pixel by pixel and decompresses the acci characters, this algorithm is optimized to not save spaces to optimize space.

This method is very efficient compared to other compression methods, because this method uses a combination of techniques to achieve excellent compression performance.

## Code to compress text with CM (Compresor Matrix)
```
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image

# Function to convert text to a matrix of letters
def text_to_matrix(text, width):
    text = text.replace(" ", "")  # Remove spaces
    n = len(text)
    height = (n // width) + (1 if n % width != 0 else 0)  # Calculate height based on width
    matrix = np.full((height, width), '', dtype='str')
    
    # Fill the matrix with letters
    for i, char in enumerate(text):
        row = i // width
        col = i % width
        matrix[row, col] = char
    return matrix

# Function to assign a color based on the ASCII value of the letter
def letter_to_color(letter):
    ascii_value = ord(letter)
    return (ascii_value % 256, (ascii_value // 256) % 256, 0)  # Colors in RGB

# Function to create the compressed image and save it
def generate_compressed_image(text, width=20, filename="compressed_image.png"):
    matrix = text_to_matrix(text, width)
    
    # Generate an image with the colors of the letters
    height, width_matrix = matrix.shape
    image_data = np.zeros((height, width_matrix, 3), dtype=int)
    
    for i in range(height):
        for j in range(width_matrix):
            if matrix[i, j]:  # If there's a letter in the position
                image_data[i, j] = letter_to_color(matrix[i, j])
    
    # Create the image and save it
    img = Image.fromarray(image_data.astype('uint8'))
    img.save(filename)
    print(f"Compressed image saved as {filename}")

# Example of text to compress
text = "This is an example of what you can compress"

# Generate the compressed image and save it
generate_compressed_image(text, width=20, filename="compressed_image.png")

```

## Code to decompress the text that is in the image with CM (Compresor Matrix)
```
import numpy as np
from PIL import Image

# Function to convert RGB color to letter
def color_to_letter(r, g, b):
    ascii_value = r + g * 256  # To simplify, we combine the RGB values into a single value
    return chr(ascii_value)  # Convert the value to an ASCII character

# Function to decompress the image and retrieve the text
def decompress_image(image_path, width=20):
    img = Image.open(image_path)
    img_data = np.array(img)
    
    # Calculate the dimensions of the matrix
    height, width_matrix, _ = img_data.shape
    matrix = np.full((height, width_matrix), '', dtype='str')
    
    # Convert each pixel to the corresponding letter
    for i in range(height):
        for j in range(width_matrix):
            r, g, b = img_data[i, j]
            letter = color_to_letter(r, g, b)
            matrix[i, j] = letter
    
    # Retrieve the text
    text = ''.join(matrix.flatten())  # Convert the matrix to a string
    return text.strip()  # Return the text without extra spaces

# Decompress the previously generated image
text_recovered = decompress_image("compressed_image.png", width=20)
print(f"Recovered text: {text_recovered}")


```
## How efficient is this compression algorithm?
To find out this, we will compare how much space a .txt with 1 million characters takes up, a compressed file with 1 million characters, and an image with all the text compressed using my method.

### Aca el codigo para esta comparacion para comprimir 1 millon de caracteres aleatorios mediante CM (Compresor Matrix)
```
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
import random
import string

# Function to convert a text to a matrix of letters
def text_to_matrix(text, width):
    text = text.replace(" ", "")  # Remove spaces
    n = len(text)
    height = (n // width) + (1 if n % width != 0 else 0)  # Calculate height based on width
    matrix = np.full((height, width), '', dtype='str')
    
    # Fill the matrix with letters
    for i, char in enumerate(text):
        row = i // width
        col = i % width
        matrix[row, col] = char
    return matrix

# Function to assign a color based on the ASCII value of the letter
def letter_to_color(letter):
    ascii_value = ord(letter)
    return (ascii_value % 256, (ascii_value // 256) % 256, 0)  # RGB colors

# Function to create the compressed image and save it
def generate_compressed_image(text, width=20, filename="compressed_image.png"):
    matrix = text_to_matrix(text, width)
    
    # Generate an image with the letter colors
    height, width_matrix = matrix.shape
    image_data = np.zeros((height, width_matrix, 3), dtype=int)
    
    for i in range(height):
        for j in range(width_matrix):
            if matrix[i, j]:  # If there is a letter at the position
                image_data[i, j] = letter_to_color(matrix[i, j])
    
    # Create the image and save it
    img = Image.fromarray(image_data.astype('uint8'))
    img.save(filename)
    print(f"Compressed image saved as {filename}")

# Generate one million random letters
random_text = ''.join(random.choices(string.ascii_lowercase + string.ascii_uppercase + string.digits, k=1000000))

# Generate the compressed image and save it
generate_compressed_image(random_text, width=100, filename="compressed_image.png")

```
