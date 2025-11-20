# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:

      #include <stdio.h>
      
      typedef struct { long long x, y; } Point;
      
      long long modInv(long long a, long long m) {
        long long m0 = m, x0 = 0, x1 = 1, q, t;
        while (a > 1) {
          q = a / m; t = m; m = a % m; a = t;
          t = x0; x0 = x1 - q * x0; x1 = t;
        }
        return x1 < 0 ? x1 + m0 : x1;
      }
      
      Point add(Point P, Point Q, long long a, long long p) {
        Point R; long long λ;
        if (P.x == Q.x && P.y == Q.y)
          λ = (3 * P.x * P.x + a) * modInv(2 * P.y, p) % p;
        else
          λ = (Q.y - P.y) * modInv(Q.x - P.x, p) % p;
        R.x = (λ * λ - P.x - Q.x + p) % p;
        R.y = (λ * (P.x - R.x) - P.y + p) % p;
        return R;
      }
      
      Point mul(Point P, long long k, long long a, long long p) {
        Point R = P; k--;
        while (k--) R = add(R, P, a, p);
        return R;
      }
      
      int main() {
        long long p, a, b, privA, privB;
