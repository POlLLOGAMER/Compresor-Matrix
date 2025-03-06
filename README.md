# Compresor-Matrix
The Compresor Matrix (CM), is a compression technique that in itself all the text information to be compressed, converts it into a matrix table, and then inserts the values ​​of the matrix tables into pixels of an image on a color scale, each intensity corresponds to an acci value, now to decompress it checks pixel by pixel and decompresses the acci characters, this algorithm is optimized to not save spaces to optimize space.

This method is very efficient compared to other compression methods, because this method uses a combination of techniques to achieve excellent compression performance.

## Code to compress text
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

## Code to decompress the text that is in the image
```
import numpy as np
from PIL import Image

# Función para convertir color RGB a letra
def color_to_letter(r, g, b):
    ascii_value = r + g * 256  # Para simplificar, combinamos los valores RGB en un solo valor
    return chr(ascii_value)  # Convertimos el valor a un caracter ASCII

# Función para descomprimir la imagen y recuperar el texto
def decompress_image(image_path, width=20):
    img = Image.open(image_path)
    img_data = np.array(img)
    
    # Calcular las dimensiones de la matriz
    height, width_matrix, _ = img_data.shape
    matrix = np.full((height, width_matrix), '', dtype='str')
    
    # Convertimos cada pixel a la letra correspondiente
    for i in range(height):
        for j in range(width_matrix):
            r, g, b = img_data[i, j]
            letter = color_to_letter(r, g, b)
            matrix[i, j] = letter
    
    # Recuperamos el texto
    text = ''.join(matrix.flatten())  # Convertimos la matriz a un string
    return text.strip()  # Retornamos el texto sin espacios extra

# Descomprimir la imagen generada previamente
text_recovered = decompress_image("compressed_image.png", width=20)
print(f"Texto recuperado: {text_recovered}")

```
