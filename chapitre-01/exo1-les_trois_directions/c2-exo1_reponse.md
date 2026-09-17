# Exercice VR/XR - Convention OpenXR

## Code Python

import sys

def Avant():
    return (0.0, 0.0, -1.0) # -Z

def Haut():
    return (0.0, 1.0, 0.0) # +Y

def Droite():
    return (1.0, 0.0, 0.0) # +X

def dot(a, b):
    return a[0]*b[0] + a[1]*b[1] + a[2]*b[2]

data = sys.stdin.read().strip().split() #lit les données
x, y, z = map(float, data[:3]) #calcul les valeurs de coordonnees
point = (x, y, z)

print(f"{dot(point, Avant()):.4f}")
print(f"{dot(point, Haut()):.4f}")
print(f"{dot(point, Droite()):.4f}")