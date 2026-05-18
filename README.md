# Experimental verification of Linear-Block-Code
# Aim
Write a simple python program to Generate Matrix, Codeword, Hamming weight, Syndrome matrix and find the error on received codeword using Linear block code. 
# Tools required
 Google coLab 
# Theory
Linear Block Code is an error control coding technique used to detect and correct errors during data transmission. In this method, message bits are converted into codewords by adding parity bits using the Generator Matrix G. Generator Matrix: G=[P∣Ik] The codeword is generated as: c=mG The Parity Check Matrix H is used to detect errors. H=[Ir∣PT] Syndrome calculation is performed using: S=rHT

If the syndrome is zero, no error is present. Otherwise, the error position is identified and corrected. The waveform of the corrected codeword is plotted using Matplotlib to represent the digital signal visually.
# Program
```
import numpy as np

# Input message bits and parity bits
r = int(input("Enter the Message bits : "))
c = int(input("Enter the Parity bits : "))

# Input parity matrix P
print("\nEnter the Parity Matrix values row-wise")

P = []

for i in range(r):
    while True:
        row = list(map(int, input(f"Enter the row values : {i+1} : ").split()))

        # Check correct number of elements
        if len(row) != c:
            print(f"Please enter exactly {c} values")
        else:
            P.append(row)
            break

P = np.array(P)

# Generator matrix G = [P | I]
I = np.eye(r, dtype=int)
G = np.hstack((P, I))

# All possible message bits
M = np.array([
    [int(x) for x in format(i, f'0{r}b')]
    for i in range(2**r)
])

# Generate codewords
C = (M @ G) % 2

# Hamming weights
weights = np.sum(C, axis=1)

# Minimum Hamming distance
dmin = np.min(weights[1:])

# Parity check matrix H = [I | P^T]
H = np.hstack((np.eye(c, dtype=int), P.T))

# Transpose of H
Ht = H.T

# Display Generator Matrix
print("\nThe Generator Matrix is:")
for row in G:
    print(*row)

# Display codewords
print("\nMessage Bits\tCodeword\tHamming Weight")

for m, cw, w in zip(M, C, weights):
    print(
        " ".join(map(str, m)),
        "\t",
        " ".join(map(str, cw)),
        "\t",
        w
    )

# Display minimum distance
print("\nMinimum Hamming distance :", dmin)

# Display Parity Check Matrix
print("\nParity Check Matrix")

for row in H:
    print(*row)

# Display transpose
print("\nParity Check Matrix Transpose")

for row in Ht:
    print(*row)

# Input received codeword
rc = np.array(
    list(map(int, input("\nEnter the error codeword : ").split()))
)

# Check codeword length
if len(rc) != r + c:
    print(f"Error: Codeword must contain {r+c} bits")
else:

    # Syndrome calculation
    s = (rc @ Ht) % 2

    print("\nSyndrome of given received codeword is :", *s)

    # Error detection
    error = np.zeros_like(rc)

    for i in range(len(Ht)):
        if np.array_equal(s, Ht[i]):
            error[i] = 1

    print("The error position is :", *error)

    # Error correction
    corrected = (rc + error) % 2

    print("The correct codeword is :", *corrected)

```
# Output 
<img width="3840" height="2160" alt="image" src="https://github.com/user-attachments/assets/cdd2b4dc-7b14-4bb8-8b1d-a78da4446f61" />



# Results
Using linear block codes, errors in transmitted data can be efficiently detected and corrected, improving the reliability of communication systems without significantly increasing the data size.
