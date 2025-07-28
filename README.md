import numpy as np
import matplotlib.pyplot as plt
import matplotlib.colors as mcolors

# Paso 1: definir color base en hexadecimal y convertir a RGB
base_hex = "#66ccff"  # azul claro
base_rgb = np.array(mcolors.to_rgb(base_hex))

# Paso 2: generar matriz de valores aleatorios entre 0 y 1 (10x10)
data = np.random.rand(10, 10)

# Paso 3: función para oscurecer el color base según valor [0,1]
def darken_color(value, base_color):
    """
    value: float entre 0 (sin cambio) y 1 (oscuro)
    base_color: array RGB (valores entre 0 y 1)
    """
    factor = 1 - value  # a mayor valor, más oscuro (factor entre 1 y 0)
    darkened = base_color * factor
    darkened = np.clip(darkened, 0, 1)  # asegurar rango válido
    return mcolors.to_hex(darkened)

# Paso 4: aplicar función a cada elemento de la matriz para obtener colores hexadecimales
color_matrix = np.vectorize(lambda v: darken_color(v, base_rgb))(data)

# Paso 5: convertir matriz de colores hexadecimales a matriz RGB para matplotlib
rgb_matrix = np.array([[mcolors.to_rgb(c) for c in row] for row in color_matrix])

# Paso 6: mostrar la imagen con imshow sin usar cmap
plt.figure(figsize=(6, 6))
plt.imshow(rgb_matrix)
plt.axis('off')
plt.title(f"Mapa de color personalizado oscureciendo {base_hex}")

# Bonus: crear barra de color personalizada (degradado del color base a negro)
gradient = np.linspace(0, 1, 256)
gradient_colors = [mcolors.to_rgb(darken_color(v, base_rgb)) for v in gradient]
gradient_rgb = np.array([gradient_colors])

plt.figure(figsize=(8, 1))
plt.imshow(gradient_rgb, aspect='auto')
plt.axis('off')
plt.title("Barra de color personalizada (de color base a oscuro)")

plt.tight_layout()
plt.show()
